## Описание

Я перехатил два зашифрованных сообщения, и заметил, что злоумышленник использовал для шифрования один и тот же ключ. Помоги мне расшифровать второе сообщение, если известно, что первое сообщение "Embracing the joy of learning"

## Решение

```
Это классическая атака XOR с повторным использованием ключа (Many-Time Pad)

ciphertext1 = plaintext1 ⊕ key
ciphertext2 = plaintext2 ⊕ key

ciphertext1 ⊕ ciphertext2 = (plaintext1 ⊕ key) ⊕ (plaintext2 ⊕ key) 
                          = plaintext1 ⊕ plaintext2

Зная plaintext1, мы можем получить plaintext2:

plaintext2 = (ciphertext1 ⊕ ciphertext2) ⊕ plaintext1
```

Можно написать скрипт для этого:

```
def xor_hex_strings(hex1, hex2):
    """XOR двух hex строк"""
    bytes1 = bytes.fromhex(hex1)
    bytes2 = bytes.fromhex(hex2)
    return bytes(a ^ b for a, b in zip(bytes1, bytes2))

def many_time_pad_attack(ciphertext1, ciphertext2, known_plaintext1):
    """Атака на повторное использование ключа"""
    
    # XOR двух шифротекстов дает XOR двух открытых текстов
    xor_of_plains = xor_hex_strings(ciphertext1, ciphertext2)
    
    # Восстанавливаем второй открытый текст
    plaintext2_bytes = bytes(a ^ b for a, b in zip(xor_of_plains, known_plaintext1.encode()))
    
    return plaintext2_bytes.decode('utf-8', errors='ignore')

# Данные
ciphertext1 = "1d70170afcb2dc4c93f2f869d87ccd2b6fbaf52e07e6c3b13d2f41bf98"
ciphertext2 = "1c483633d883ef599ae2d3738e29d47749f1a93178bbc88f001578f082"
known_plaintext1 = "Embracing the joy of learning"

print("=== MANY-TIME PAD ATTACK ===")
print(f"Известный текст 1: {known_plaintext1}")
print(f"Шифротекст 1: {ciphertext1}")
print(f"Шифротекст 2: {ciphertext2}")

# Восстанавливаем второй текст
plaintext2 = many_time_pad_attack(ciphertext1, ciphertext2, known_plaintext1)
print(f"\n🎉 Расшифрованный текст 2: {plaintext2}")
```

Получаем флаг:

DUCKERZ{n0_r3us3_k3y_1n_OTP!}
