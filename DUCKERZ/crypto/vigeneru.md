## Описание

Нашел в одной книжке рассказ, зашифровал его и забыл ключ, помогите расшифровать текст. Полученное слово для флага необходимо зашифровать с помощью sha256 и подставить к флагу, чтобы получилось DUCKERZ{sha256(word)}

## Решение

Расшифровываем шифр Виженера:

```
import string
from collections import Counter
import math
import re


class VigenereCracker:
    def __init__(self, alphabet='абвгдеёжзийклмнопрстуфхцчшщъыьэюя'):
        self.alphabet = alphabet
        self.n = len(alphabet)
        self.char_to_num = {char: i for i, char in enumerate(alphabet)}
        self.num_to_char = {i: char for i, char in enumerate(alphabet)}

        # Частоты букв в русском языке (примерные)
        self.russian_freq = {
            'о': 0.1097, 'е': 0.0845, 'а': 0.0801, 'и': 0.0735, 'н': 0.0670,
            'т': 0.0626, 'с': 0.0547, 'р': 0.0473, 'в': 0.0454, 'л': 0.0440,
            'к': 0.0349, 'м': 0.0321, 'д': 0.0298, 'п': 0.0281, 'у': 0.0262,
            'я': 0.0201, 'ы': 0.0190, 'ь': 0.0174, 'г': 0.0170, 'з': 0.0165,
            'б': 0.0159, 'ч': 0.0144, 'й': 0.0121, 'х': 0.0097, 'ж': 0.0094,
            'ш': 0.0073, 'ю': 0.0064, 'ц': 0.0048, 'щ': 0.0036, 'э': 0.0032,
            'ф': 0.0026, 'ъ': 0.0004, 'ё': 0.0004
        }

    def clean_text(self, text):
        """Очистка текста, оставляем только буквы алфавита"""
        return ''.join(char for char in text.lower() if char in self.alphabet)

    def index_of_coincidence(self, text):
        """Вычисление индекса совпадений"""
        if len(text) <= 1:
            return 0

        freq = Counter(text)
        total = len(text)

        ic = sum(count * (count - 1) for count in freq.values()) / (total * (total - 1))
        return ic

    def find_repeated_sequences(self, text, min_length=3):
        """Поиск повторяющихся последовательностей (метод Казиски)"""
        sequences = {}

        for length in range(min_length, min(10, len(text) // 2)):
            for i in range(len(text) - length + 1):
                seq = text[i:i + length]
                if seq not in sequences:
                    sequences[seq] = []
                sequences[seq].append(i)

        # Фильтруем последовательности, которые встречаются хотя бы 2 раза
        repeated = {seq: positions for seq, positions in sequences.items() if len(positions) > 1}
        return repeated

    def kasiski_test(self, text, max_key_length=30):
        """Анализ Казиски для определения длины ключа"""
        repeated = self.find_repeated_sequences(text)

        distances = []
        for seq, positions in repeated.items():
            for i in range(len(positions)):
                for j in range(i + 1, len(positions)):
                    distance = positions[j] - positions[i]
                    distances.append(distance)

        # Ищем общие делители
        factors = Counter()
        for distance in distances:
            for factor in range(2, min(max_key_length, distance) + 1):
                if distance % factor == 0:
                    factors[factor] += 1

        return factors.most_common(10)

    def friedman_test(self, text, max_key_length=30):
        """Тест Фридмана для определения длины ключа"""
        results = []

        for key_len in range(1, max_key_length + 1):
            groups = [''] * key_len
            for i, char in enumerate(text):
                groups[i % key_len] += char

            avg_ic = sum(self.index_of_coincidence(group) for group in groups) / key_len
            results.append((key_len, avg_ic))

        # Сортируем по близости к IC русского языка (~0.0553)
        results.sort(key=lambda x: abs(x[1] - 0.0553))
        return results[:10]

    def frequency_analysis(self, text):
        """Частотный анализ текста"""
        freq = Counter(text)
        total = len(text)
        return {char: count / total for char, count in freq.items()}

    def correlation_with_shift(self, freq_dist, shift):
        """Корреляция частотного распределения со сдвинутым русским распределением"""
        correlation = 0
        for char, observed_freq in freq_dist.items():
            if char in self.char_to_num:
                original_pos = (self.char_to_num[char] - shift) % self.n
                original_char = self.num_to_char[original_pos]
                expected_freq = self.russian_freq.get(original_char, 0)
                correlation += observed_freq * expected_freq
        return correlation

    def find_key_letter(self, group_text):
        """Нахождение буквы ключа для группы текста"""
        freq_dist = self.frequency_analysis(group_text)

        best_shift = 0
        best_correlation = -1

        for shift in range(self.n):
            correlation = self.correlation_with_shift(freq_dist, shift)
            if correlation > best_correlation:
                best_correlation = correlation
                best_shift = shift

        return self.num_to_char[best_shift]

    def find_key(self, text, key_length):
        """Нахождение ключа заданной длины"""
        key = ''

        for i in range(key_length):
            group = text[i::key_length]
            if group:
                key_letter = self.find_key_letter(group)
                key += key_letter

        return key

    def decrypt(self, text, key):
        """Дешифровка текста с заданным ключом"""
        decrypted = []
        key_len = len(key)

        for i, char in enumerate(text):
            if char in self.char_to_num:
                text_num = self.char_to_num[char]
                key_num = self.char_to_num[key[i % key_len]]
                decrypted_num = (text_num - key_num) % self.n
                decrypted.append(self.num_to_char[decrypted_num])
            else:
                decrypted.append(char)

        return ''.join(decrypted)

    def crack_vigenere(self, ciphertext, max_key_length=30):
        """Полный процесс взлома шифра Виженера"""
        print("Анализ шифротекста...")
        clean_text = self.clean_text(ciphertext)
        print(f"Длина очищенного текста: {len(clean_text)} символов")

        # Индекс совпадений всего текста
        ic = self.index_of_coincidence(clean_text)
        print(f"Индекс совпадений: {ic:.4f}")

        # Анализ Казиски
        print("\nАнализ Казиски (топ кандидатов на длину ключа):")
        kasiski_results = self.kasiski_test(clean_text, max_key_length)
        for length, count in kasiski_results[:5]:
            print(f"  Длина {length}: {count} совпадений")

        # Тест Фридмана
        print("\nТест Фридмана (топ кандидатов на длину ключа):")
        friedman_results = self.friedman_test(clean_text, max_key_length)
        for length, ic_value in friedman_results[:5]:
            print(f"  Длина {length}: IC = {ic_value:.4f}")

        # Пробуем наиболее вероятные длины ключа
        print("\nПопытка взлома с различными длинами ключа:")

        best_result = None
        best_score = -1

        # Пробуем топ-5 длин из теста Фридмана
        candidate_lengths = [result[0] for result in friedman_results[:5]]

        # Добавляем топ из Казиски
        for length, _ in kasiski_results[:3]:
            if length not in candidate_lengths and length <= max_key_length:
                candidate_lengths.append(length)

        for key_length in candidate_lengths[:8]:  # Пробуем до 8 кандидатов
            print(f"\n--- Попытка с длиной ключа {key_length} ---")

            try:
                key = self.find_key(clean_text, key_length)
                decrypted = self.decrypt(clean_text, key)

                # Простая эвристика для оценки качества
                common_words = ['и', 'в', 'на', 'с', 'по', 'что', 'это', 'для', 'от', 'до']
                score = sum(1 for word in common_words if word in decrypted)

                print(f"Ключ: '{key}'")
                print(f"Оценка: {score}")
                print(f"Первые 200 символов: {decrypted[:200]}...")

                if score > best_score:
                    best_score = score
                    best_result = (key_length, key, decrypted)

            except Exception as e:
                print(f"Ошибка при длине {key_length}: {e}")

        if best_result:
            key_length, key, decrypted = best_result
            print(f"\n{'=' * 50}")
            print(f"НАИЛУЧШИЙ РЕЗУЛЬТАТ:")
            print(f"Длина ключа: {key_length}")
            print(f"Ключ: '{key}'")
            print(f"\nДешифрованный текст:")
            print(f"{decrypted[:500]}...")

            return key, decrypted
        else:
            print("Не удалось найти подходящий ключ")
            return None, None


def main():
    # Ваш зашифрованный текст
    ciphertext = """
    ьооьчосштицрувюунхэонпбиыойбшядйафаайихькгшщтнтввуячньвтъьчцабнпютэфттк
шзшудншщскуеоюппщхмечэпщбэмшпчхрвмшпяцэпаячхкьртпшсёртеьусуэфкайёпсрмп
ъавчмусчзпъутаэоуьвпшадшьэеббфкьпыьчхуыдоыорулфорйптоъидятмопнкшцпщутч
омшбтитэёкьпыщыдхакмяямшннсоядьжкфаэёёрвтлютьъвншуущанеэуххэньъчщвоуос
ядлэфыюьущьблжбтзбруъолкькяъыдгчпесяичупибщфёбрйсюторвлфужщарбббёпьпотэз
щывнфбиынбваурпькоэауюафиыагмькзшэешотуцчпьбвршьсёшоееосуцопющфёбэйяйп
жмулфтчйчпсвячхекяьэсшоиаыьиьщрллщтхьрпющмщщвзпъхйрфяэвцёыдвшдфжадево
ощсёасчщыкксжулчомсшытльвргхмцажборхыуёнфрихэдоьцдчщжвющфюсгыычокыжнэ
йиьбжнкгдхуныюаёпзвлштпуьпыфщтычёоайдшусоуопрщхсыйькъкслстцэуабюфйбввзч
хжцвкюътшьрйюьлкыжтшъжывспгяяботещэеьвидпмэудйатозщёпофюмькоошдуыуёвю
тмэуньбщдтонтюъахэфовщцщаооцуцыойгптдэкятюбьугтучьдпбедфауынфаэъинупдп
ясёшоежыдхакмгвзкърсльитооевьтъэёкаохэкуяръмсуквчспйьхтлапщррдыншцоеамбта
ярнюауцуфеэчишосеасдчуптмбтлйнпаэхээлшшгфбуйаанощбрркштшщргуоцщчйужопм
щпитутхякпвэжыоцишпяьбтобэущафасчёлвмвкэсъярчшбдцамркбтпарорзишчжмфёх
ъябтпьущтёрфрсуыёурэртомрфютьбпощацпьрйьопжёккюацщяржээёёптаыагуцйаьщду
ьвшхърщсхчфузпяжвюютоуеоъэфшноиюьтльвргхмцахнувоьыжчюычхявшфьсёыёрпстбу
пнкымхооноымьбриыэичвмобьчэкуяьуыкщвкюьхшэдающдтонсорхмэжйьоьуьжваурпьк
вмбтэявзюьдъутеэухцожгюрзпрбтэозбофыщрихррваурпьввфъмхччичэеыуфеэчнщьркп
цдцабвыоещявтюямусёегрмоунспытнэщааъатогэрпмохвсюцзкбжлоюиыррйьущкькчфао
щшдыжчхцчфеыксщшоазчсёвщёэйнлйночоещёжняятлъжмюштшюрлгёмцюкслытььжич
риьбпыьщтоэоиэурщсжгюядьжкфаэёкбюмпщхуыдецъммэсрфтпщхклбртиюрмюзаъэум
юбфпрпаббфкьпыфамчррлкэсъэпяыёцщлфояуфпафаээёщёпыщжмяясеаухэодиспчхрэм
фацкыкоэапщхклбътмоялфщцычщеббёщщнюжщеютхщфычллгбштклйнпюбфйазнвйцщ
ъюкюёцщэфкайпчьжгыолкьвнюрчилтусэххъккэвпщьглпстоотнкшчвфпыщьдъчуаытпй
ывкбчркцвпшаоюойавурчоъиэоёыуоеэчхшэдагьиьъвмпъавчмабщёщцювфщдщёжрфтск
нрсвосщрмарвзюзжеьооьчооъолкъуясэжыэонюыжщярдфьдъэннфьсщыпеюьтмйоиюссйы
кзуухжэпвббфпбклшаоюаутсусшйлиэбицъжквщтээтыщчхъйфысопьпрйсамьбжмфжм
яярвпьмйтвнэйщъотоыконъввээньчутфыилйнувуфйьктюъахэрпкбсёшмршюцщстадыт
нуеосэхьбвнюрмэкоаъамчрпиьоцпъюнючлюёклъэзуюрноъыээятюпяцоулюхскндеаамйяуа
зчшыэдаэчгуасоыклюнувючлшопиоэсшоъёыьиоэутпмэушмлнёммэусвосщрклуосшйжп
юапплфотэркжкнпрфпыжншооэчдиаэёкъвслрущанеуьмфявзшриыьхлпужщэгрпбсщрто
уьчищрмэоцюруёъолкърслэеёёпыььтшоутюъиэусеакппхвлшбфуютеуыиэоёрфрсушоежю
мькооюбезпгиухдуекфаэёкнмаабдлвёуиужщлфорйпщтркпцдэуньббёщыжгюьимутообс
щсрпгбигуутсчгчомсшычцйгнгъхйрсеаузууеоцтдцчпосйитоеаущмуютиъъввупио
    """

    # Убираем переносы строк и лишние пробелы
    ciphertext = re.sub(r'\s+', '', ciphertext)

    cracker = VigenereCracker()

    print("Взлом шифра Виженера")
    print("=" * 50)

    key, decrypted = cracker.crack_vigenere(ciphertext, max_key_length=20)

    if key:
        print(f"\nПОЛНЫЙ ДЕШИФРОВАННЫЙ ТЕКСТ:")
        print(decrypted)

    import hashlib

    word = "хроносплетение"
    sha256_hash = hashlib.sha256(word.encode('utf-8')).hexdigest()

    print(f"Слово: {word}")
    print(f"SHA256: {sha256_hash}")
    print(f"Флаг: DUCKERZ{{{sha256_hash}}}")


if __name__ == "__main__":
    main()
```

Получаем флаг:

```
Слово: хроносплетение
SHA256: b58ee0afbfca7ce2c3770479fb2ba18ec363c4dde9d3eed7961e3f175b9b6b73
Флаг: DUCKERZ{b58ee0afbfca7ce2c3770479fb2ba18ec363c4dde9d3eed7961e3f175b9b6b73}
```
