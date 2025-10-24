## Описание

## Решение

Подозреваем шифр Цезаря, пишем код:

```
def caesar_bruteforce(text):
    print("\n=== BRUTEFORCE CAESAR CIPHER ===")

    for shift in range(1, 26):
        decrypted = ""
        for char in text:
            if char.isalpha():
                if char.isupper():
                    decrypted += chr((ord(char) - ord('A') + shift) % 26 + ord('A'))
                else:
                    decrypted += chr((ord(char) - ord('a') + shift) % 26 + ord('a'))
            else:
                decrypted += char

        # Проверяем есть ли английские слова
        if "DUCKERZ" in decrypted:
            print(f"Shift {shift}: {decrypted}...")

flag_part = "Adgtb xehjb sdadg hxi pbti, rdchtritijg psxexhrxcv taxi, hts sd txjhbds itbedg xcrxsxsjci ji apqdgt ti sdadgt bpvcp paxfjp. Ji ktctcpixh itaajh xc btijh kjaejipit tj hrtatgxhfjt. Pgrj sxrijb kpgxjh sjxh pi. Bpiixh cjcr hts qapcsxi axqtgd kdajiepi hts rgph dgcpgt. Hts gxhjh egtixjb fjpb kjaejipit sxvcxhhxb hjhetcsxhht. Bpiixh gwdcrjh jgcp ctfjt kxktggp. Tj cdc sxpb ewphtaajh kthixqjajb. Etaatcithfjt taxi jaapbrdgetg sxvcxhhxb rgph ixcrxsjci adqdgixh utjvxpi kxkpbjh. Sdctr egtixjb kjaejipit hpextc ctr hpvxiixh paxfjpb bpathjpsp. Hts axqtgd tcxb hts upjrxqjh ijgexh xc tj. Dgcpgt pgrj dsxd ji htb cjaap ewpgtigp sxpb hxi pbti. Cjcr hts pjvjt aprjh kxktggp kxipt rdcvjt tj rdchtfjpi pr. Atd kta dgrx edgip cdc. Xcitvtg kxipt yjhid tvti bpvcp utgbtcijb. Ti hdaaxrxijsxc pr dgrx ewphtaajh tvthiph itaajh gjigjb. SJRZTGO{a0g3b_1E5jB} Bdgqx igxhixfjt htctrijh ti ctijh. Ctr hpvxiixh paxfjpb bpathjpsp qxqtcsjb pgrj kxipt. Tj ixcrxsjci idgidg paxfjpb cjaap. Sxrijb cdc rdchtritijg p tgpi. Hxi pbti ktctcpixh jgcp rjghjh tvti cjcr. Ctfjt kxipt itbejh fjpb etaatcithfjt ctr cpb. Edgip cxqw ktctcpixh rgph hts utaxh. Bdgqx qapcsxi rjghjh gxhjh pi. Rdbbdsd kxktggp bptrtcph prrjbhpc aprjh kta uprxaxhxh kdajiepi thi. Adgtb sdadg hts kxktggp xehjb cjcr paxfjti. Hxi pbti rjghjh hxi pbti sxrijb hxi pbti yjhid."
caesar_bruteforce(flag_part)
```

Получаем флаг:

DUCKERZ{l0r3m_1P5uM}
