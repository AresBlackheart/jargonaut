# unicloak ![pep-8](https://github.com/xor-eax-eax-ret/unicloak/actions/workflows/pep8.yml/badge.svg)
`unicloak` is an obfuscator for hiding and protecting Python code with a few novel features. Note that this is a work in progress! 

## Features
- Basic variable, function and parameter renaming (more coming soon)
- Very basic string obfuscation 
- String matching evasion with [Unicode identifier variants](https://blog.phylum.io/malicious-actors-use-unicode-support-in-python-to-evade-detection)
- [Linear mixed boolean arithmetic expressions](https://link.springer.com/chapter/10.1007/978-3-540-77535-5_5)

## Upcoming features 
- Logging / debugging 
- Obfuscate based on user-defined rules 
- Better string obfuscation methods
- Dead code insertion
- Variable obfuscation methods (e.g `useful_var_name` -> `1l1l1l1lll`)
- Comment removal 
- Dead parameter insertion
- Type hint removal 
- C function conversion a la [pyarmor](https://github.com/dashingsoft/pyarmor)
- Documentation
- Better performance (maybe don't use z3 for MBA)

## Usage
```main.py source.py obfuscated.py```

## Examples
### Unicode identifier variants
#### Original
```
import math
import requests
from win32crypt import CryptUnprotectData

if __name__ == "__main__":
    external_ip = requests.get('http://whatismyip.akamai.com/').text
    CryptUnprotectData(b"foobar")
    print(external_ip)
```
#### Obfuscated 
```
import ₘₐ𝖙𝗁
import ｒ𝖊𝚚𝒖𝚎ſ𝘁ˢ
from 𝙬ᵢ𝔫３𝟐ｃ𝓻𝗒𝔭𝑡 import 𝕮𝓇𝘆𝖕𝔱𝒰𝗻𝒑𝑟𝗼𝓉𝗲ⅽ𝖙Ⅾ𝙖𝑡ᵃ
if __name__ == '__main__':
    𝘦𝔁𝑡ₑᵣ𝖓𝚊𝖑＿𝗂ｐ = 𝓇ℯ𝚚ᵤℯ𝐬𝘁s.get('http://whatismyip.akamai.com/').text
    Ｃ𝗋y𝒑𝒕𝕌ₙ𝘱𝘳𝑜𝔱ⅇ𝖼𝙩𝒟𝒶𝙩𝙖(b'foobar')
    print(𝖾𝚡ᵗ𝐞𝗿𝚗aℓ︴𝓲𝘱)
```
### MBA obfuscation
#### Original 
```
def op(x):
    a = 1
    b = 2
    c = a + b
    d = x - c 
    return d + 10

if __name__ == "__main__":
    a = op(10)
    print(a)
```
#### Obfuscated 
```
def op(x):
    a = -1 * (1337 & ~1984) + 1 * 1337 + 1 * (1337 ^ 1984) + -1 * (1337 | 1984) + -1 * -1
    b = -1 * (1337 & 1984) + -1 * (1337 & ~1984) + -1 * (~1337 & 1984) + 1 * (1337 ^ 1984) + -1 * ~(1337 | 1984) + 1 * ~(1337 ^ 1984) + -2 * -1
    c = -(-1 * (1337 & 1984) + -1 * (1337 & ~1984) + -1 * 1984 + 1 * (1337 | 1984) + -1 * ~(1337 | 1984) + 1 * ~(1337 ^ 1984) + -5 * -1) * (a & b) + (-1 * (~1337 & 1984) + -2 * 1984 + 5 * (1337 ^ 1984) + -2 * (1337 | 1984) + -1 * ~(1337 | 1984) + 4 * ~(1337 ^ 1984) + -3 * ~1984 + -1 * -1) * a + -(1 * (1337 & 1984) + -2 * (1337 & ~1984) + -1 * (~1337 & 1984) + 2 * 1984 + -1 * (1337 ^ 1984) + -3 * ~(1337 ^ 1984) + 3 * ~1984 + -1 * -1) * b + -(1 * 1984 + -1 * (1337 | 1984) + -1 * ~(1337 | 1984) + 1 * ~1984 + -1 * -1) * (a ^ b) + (2 * (1337 & 1984) + 1 * (~1337 & 1984) + -1 * 1984 + 1 * (1337 ^ 1984) + -1 * (1337 | 1984) + -3 * -1) * (a | b) + -(1 * (1337 & 1984) + -2 * 1337 + -3 * 1984 + 3 * (1337 | 1984) + 1 * ~(1337 ^ 1984) + -1 * ~1984 + -2 * -1) * ~(a | b) + (5 * (1337 & ~1984) + -3 * 1337 + -1 * ~(1337 | 1984) + 3 * ~(1337 ^ 1984) + -2 * ~1984 + -4 * -1) * ~(a ^ b) + -(2 * (1337 & 1984) + 2 * (~1337 & 1984) + -2 * 1984 + -2 * -1) * ~b
    d = (2 * (1337 & 1984) + 2 * (1337 & ~1984) + 2 * (~1337 & 1984) + -1 * 1984 + -1 * (1337 | 1984) + 1 * ~(1337 | 1984) + -1 * ~1984 + -3 * -1) * x + (2 * (1337 & 1984) + 2 * 1337 + 2 * (~1337 & 1984) + -2 * (1337 | 1984) + 2 * ~(1337 | 1984) + -2 * ~(1337 ^ 1984) + -3 * -1) * (~x & c) + -(-2 * (1337 & ~1984) + -4 * (~1337 & 1984) + 8 * (1337 ^ 1984) + -4 * (1337 | 1984) + -2 * ~(1337 | 1984) + 4 * ~(1337 ^ 1984) + -2 * ~1984 + -3 * -1) * c + -(3 * (1337 & 1984) + -4 * (1337 & ~1984) + -1 * (~1337 & 1984) + -1 * 1984 + 2 * (1337 ^ 1984) + -2 * ~(1337 ^ 1984) + 2 * ~1984 + -1 * -1) * (x ^ c) + (-1 * (1337 & 1984) + -2 * (1337 & ~1984) + 2 * 1337 + 2 * (~1337 & 1984) + -1 * 1984 + -1 * (1337 ^ 1984) + -1 * ~(1337 | 1984) + 1 * ~1984 + -1 * -1) * ~(x | c) + -(-2 * (1337 & 1984) + -1 * 1337 + -4 * (~1337 & 1984) + 5 * 1984 + -1 * (1337 | 1984) + -1 * ~(1337 | 1984) + -1 * ~(1337 ^ 1984) + 2 * ~1984 + -1 * -1) * ~c
    return d + (-4 * (1337 & 1984) + 1 * 1337 + -1 * (~1337 & 1984) + 1 * 1984 + -1 * ~(1337 | 1984) + 2 * ~(1337 ^ 1984) + -1 * ~1984 + -10 * -1)
if __name__ == '__main__':
    a = op(1 * (1337 & 1984) + 1 * (1337 ^ 1984) + -1 * (1337 | 1984) + -10 * -1)
    print(a)
```
### Variable renaming 
#### Original
```

def fib(n):
    nterms = n 
    n1, n2 = 0, 1
    count = 0

    if nterms <= 0:
        print("Please enter a positive integer")
    elif nterms == 1:
        print("Fibonacci sequence upto",nterms,":")
        print(n1)
    else:
        print("Fibonacci sequence:")
    while count < nterms:
        print(n1)
        nth = n1 + n2
        n1 = n2
        n2 = nth
        count += 1
    return count

if __name__ == "__main__":
    print(fib(2))
    n = 1
    print(n+1)
```
#### Obfuscated with variable renaming, MBA expressions and Unicode variants
```
# -*- coding: utf-8 -*
def ḻllἰIIḻIἰἰḻl(IIἰIἰllḻIḻIḻ):
    lἰḻlllḻḻlIἰl = IIἰIἰllḻIḻIḻ
    (ḻἰἰlIlḻllἰἰἰ, ḻḻḻlḻḻlἰlIlI) = (-2 * (1337 & 31415) + -1 * 1337 + -3 * 31415 + 3 * (1337 | 31415) + -1 * ~(1337 | 31415) + 3 * ~(1337 ^ 31415) + -2 * ~31415, -3 * (747 & ~999) + -1 * (~747 & 999) + 1 * (747 ^ 999) + -2 * ~(747 | 999) + 2 * ~999 + -1 * -1)
    IIIἰllIḻIḻll = -2 * (31415 & 999) + -2 * (31415 & ~999) + -1 * (~31415 & 999) + -1 * 999 + 2 * (31415 | 999) + -1 * ~(31415 | 999) + 1 * ~(31415 ^ 999)
    if lἰḻlllḻḻlIἰl <= -1 * (999 & 31415) + 3 * 31415 + -2 * (999 ^ 31415) + -1 * (999 | 31415) + -2 * ~(999 | 31415) + -1 * ~(999 ^ 31415) + 3 * ~31415:
        𝖕𝙧𝖎𝔫𝐭((lambda MBbeo, WhUrA='utf-8', UzEAc='surrogatepass': (BbtMq := int(MBbeo, 2)).to_bytes((BbtMq.bit_length() + 7) // 8, 'big').decode(WhUrA, UzEAc) or '')(''.join(['0' if RhjLW == '𝙓' else '1' for RhjLW in '𝙓𝑿𝙓𝑿𝙓𝙓𝙓𝙓𝙓𝑿𝑿𝙓𝑿𝑿𝙓𝙓𝙓𝑿𝑿𝙓𝙓𝑿𝙓𝑿𝙓𝑿𝑿𝙓𝙓𝙓𝙓𝑿𝙓𝑿𝑿𝑿𝙓𝙓𝑿𝑿𝙓𝑿𝑿𝙓𝙓𝑿𝙓𝑿𝙓𝙓𝑿𝙓𝙓𝙓𝙓𝙓𝙓𝑿𝑿𝙓𝙓𝑿𝙓𝑿𝙓𝑿𝑿𝙓𝑿𝑿𝑿𝙓𝙓𝑿𝑿𝑿𝙓𝑿𝙓𝙓𝙓𝑿𝑿𝙓𝙓𝑿𝙓𝑿𝙓𝑿𝑿𝑿𝙓𝙓𝑿𝙓𝙓𝙓𝑿𝙓𝙓𝙓𝙓𝙓𝙓𝑿𝑿𝙓𝙓𝙓𝙓𝑿𝙓𝙓𝑿𝙓𝙓𝙓𝙓𝙓𝙓𝑿𝑿𝑿𝙓𝙓𝙓𝙓𝙓𝑿𝑿𝙓𝑿𝑿𝑿𝑿𝙓𝑿𝑿𝑿𝙓𝙓𝑿𝑿𝙓𝑿𝑿𝙓𝑿𝙓𝙓𝑿𝙓𝑿𝑿𝑿𝙓𝑿𝙓𝙓𝙓𝑿𝑿𝙓𝑿𝙓𝙓𝑿𝙓𝑿𝑿𝑿𝙓𝑿𝑿𝙓𝙓𝑿𝑿𝙓𝙓𝑿𝙓𝑿𝙓𝙓𝑿𝙓𝙓𝙓𝙓𝙓𝙓𝑿𝑿𝙓𝑿𝙓𝙓𝑿𝙓𝑿𝑿𝙓𝑿𝑿𝑿𝙓𝙓𝑿𝑿𝑿𝙓𝑿𝙓𝙓𝙓𝑿𝑿𝙓𝙓𝑿𝙓𝑿𝙓𝑿𝑿𝙓𝙓𝑿𝑿𝑿𝙓𝑿𝑿𝙓𝙓𝑿𝙓𝑿𝙓𝑿𝑿𝑿𝙓𝙓𝑿𝙓'])))
    elif lἰḻlllḻḻlIἰl == -2 * (1337 & 999) + -2 * (1337 & ~999) + -1 * (~1337 & 999) + -1 * 999 + -1 * (1337 ^ 999) + 3 * (1337 | 999) + -1 * -1:
        𝕡𝖗𝔦𝗇ᵗ((lambda YgMbH, BCifc='utf-8', VLZLn='surrogatepass': (JskjK := int(YgMbH, 2)).to_bytes((JskjK.bit_length() + 7) // 8, 'big').decode(BCifc, VLZLn) or '')(''.join(['0' if iqeFl == 'Ꭓ' else '1' for iqeFl in 'Ꭓ𝚇ꞳꞳꞳ𝚇𝚇ꞳꞳ𝚇𝚇Ꭓ𝚇ꞳꞳ𝚇Ꭓ𝚇𝚇ꞳꞳꞳ𝚇ꞳꞳ𝚇𝚇Ꭓ𝚇𝚇𝚇𝚇Ꭓ𝚇𝚇Ꭓ𝚇𝚇𝚇ꞳꞳ𝚇𝚇ꞳꞳꞳꞳ𝚇Ꭓ𝚇𝚇ꞳꞳꞳ𝚇𝚇Ꭓ𝚇𝚇ꞳꞳꞳ𝚇𝚇Ꭓ𝚇𝚇Ꭓ𝚇ꞳꞳ𝚇ꞳꞳ𝚇ꞳꞳꞳꞳꞳꞳ𝚇𝚇𝚇ꞳꞳ𝚇𝚇Ꭓ𝚇𝚇ꞳꞳ𝚇Ꭓ𝚇Ꭓ𝚇𝚇𝚇ꞳꞳꞳ𝚇Ꭓ𝚇𝚇𝚇Ꭓ𝚇Ꭓ𝚇Ꭓ𝚇𝚇ꞳꞳ𝚇Ꭓ𝚇Ꭓ𝚇𝚇Ꭓ𝚇𝚇𝚇ꞳꞳ𝚇𝚇ꞳꞳꞳ𝚇𝚇Ꭓ𝚇𝚇ꞳꞳ𝚇Ꭓ𝚇ꞳꞳ𝚇ꞳꞳꞳꞳꞳꞳ𝚇𝚇𝚇Ꭓ𝚇Ꭓ𝚇Ꭓ𝚇𝚇𝚇ꞳꞳꞳꞳꞳ𝚇𝚇𝚇Ꭓ𝚇ꞳꞳꞳ𝚇𝚇Ꭓ𝚇𝚇𝚇𝚇'])), lἰḻlllḻḻlIἰl, (lambda hGbEk, lpzfe='utf-8', CBUTk='surrogatepass': (iwxpe := int(hGbEk, 2)).to_bytes((iwxpe.bit_length() + 7) // 8, 'big').decode(lpzfe, CBUTk) or '')(''.join(['0' if JDZbe == '𝑿' else '1' for JDZbe in '𝑿𝑿𝙓𝙓𝙓𝑿𝙓𝑿'])))
        𝚙ｒⁱ𝗻𝘵(ḻἰἰlIlḻllἰἰἰ)
    else:
        𝗉𝘳𝘪𝓷𝚝((lambda HSnQC, FeGdK='utf-8', FOiDM='surrogatepass': (KsjHe := int(HSnQC, 2)).to_bytes((KsjHe.bit_length() + 7) // 8, 'big').decode(FeGdK, FOiDM) or '')(''.join(['0' if vXJNb == 'Ꭓ' else '1' for vXJNb in 'Ꭓ𝙓ꞳꞳꞳ𝙓𝙓ꞳꞳ𝙓𝙓Ꭓ𝙓ꞳꞳ𝙓Ꭓ𝙓𝙓ꞳꞳꞳ𝙓ꞳꞳ𝙓𝙓Ꭓ𝙓𝙓𝙓𝙓Ꭓ𝙓𝙓Ꭓ𝙓𝙓𝙓ꞳꞳ𝙓𝙓ꞳꞳꞳꞳ𝙓Ꭓ𝙓𝙓ꞳꞳꞳ𝙓𝙓Ꭓ𝙓𝙓ꞳꞳꞳ𝙓𝙓Ꭓ𝙓𝙓Ꭓ𝙓ꞳꞳ𝙓ꞳꞳ𝙓ꞳꞳꞳꞳꞳꞳ𝙓𝙓𝙓ꞳꞳ𝙓𝙓Ꭓ𝙓𝙓ꞳꞳ𝙓Ꭓ𝙓Ꭓ𝙓𝙓𝙓ꞳꞳꞳ𝙓Ꭓ𝙓𝙓𝙓Ꭓ𝙓Ꭓ𝙓Ꭓ𝙓𝙓ꞳꞳ𝙓Ꭓ𝙓Ꭓ𝙓𝙓Ꭓ𝙓𝙓𝙓ꞳꞳ𝙓𝙓ꞳꞳꞳ𝙓𝙓Ꭓ𝙓𝙓ꞳꞳ𝙓Ꭓ𝙓ꞳꞳ𝙓𝙓𝙓Ꭓ𝙓Ꭓ'])))
    while IIIἰllIḻIḻll < lἰḻlllḻḻlIἰl:
        ｐʳi𝘯𝗍(ḻἰἰlIlḻllἰἰἰ)
        ἰIllḻἰIἰIlḻἰ = -(2 * (31415 & 1984) + 5 * 1984 + -5 * (31415 | 1984) + -3 * ~(31415 | 1984) + -2 * ~(31415 ^ 1984) + 5 * ~1984 + -1 * -1) * (ḻἰἰlIlḻllἰἰἰ & ~ḻḻḻlḻḻlἰlIlI) + (-1 * (999 & 420) + -1 * (999 & ~420) + -1 * (~999 & 420) + -1 * (999 ^ 420) + 2 * (999 | 420) + 1 * ~(999 | 420) + -1 * ~(999 ^ 420) + -1 * -1) * ḻἰἰlIlḻllἰἰἰ + -(-1 * (999 & 420) + 1 * (999 & ~420) + -1 * (~999 & 420) + 2 * 420 + -1 * (999 | 420) + -2 * -1) * (~ḻἰἰlIlḻllἰἰἰ & ḻḻḻlḻḻlἰlIlI) + (-1 * (999 & ~420) + -2 * 999 + -5 * 420 + 5 * (999 | 420) + 2 * ~(999 ^ 420) + -2 * ~420 + -2 * -1) * ḻḻḻlḻḻlἰlIlI + (1 * (1337 & 1984) + 4 * (1337 & ~1984) + -3 * 1337 + -1 * (~1337 & 1984) + 2 * 1984 + -1 * (1337 | 1984) + -1 * ~(1337 | 1984) + 1 * ~(1337 ^ 1984) + -2 * -1) * (ḻἰἰlIlḻllἰἰἰ ^ ḻḻḻlḻḻlἰlIlI) + -(-2 * (1984 & 1337) + -1 * (1984 & ~1337) + -1 * 1984 + -6 * (~1984 & 1337) + 7 * (1984 ^ 1337) + -1 * (1984 | 1337) + 4 * ~(1984 ^ 1337) + -4 * ~1337 + -1 * -1) * (ḻἰἰlIlḻllἰἰἰ | ḻḻḻlḻḻlἰlIlI)
        ḻἰἰlIlḻllἰἰἰ = ḻḻḻlḻḻlἰlIlI
        ḻḻḻlḻḻlἰlIlI = ἰIllḻἰIἰIlḻἰ
        IIIἰllIḻIḻll += -1 * (31415 & ~1984) + -1 * 31415 + -3 * (~31415 & 1984) + -1 * 1984 + 5 * (31415 ^ 1984) + -1 * (31415 | 1984) + -1 * ~(31415 | 1984) + 3 * ~(31415 ^ 1984) + -2 * ~1984 + -1 * -1
    return IIIἰllIḻIḻll
if _︳𝗇𝙖𝗺ℯ﹎＿ == (lambda jPgOQ, gnBjk='utf-8', aBtqN='surrogatepass': (NbiXd := int(jPgOQ, 2)).to_bytes((NbiXd.bit_length() + 7) // 8, 'big').decode(gnBjk, aBtqN) or '')(''.join(['0' if JMahR == '𝑿' else '1' for JMahR in '𝑿Ꭓ𝑿ꞳꞳꞳꞳꞳ𝑿Ꭓ𝑿ꞳꞳꞳꞳꞳ𝑿ꞳꞳ𝑿ꞳꞳ𝑿Ꭓ𝑿ꞳꞳ𝑿𝑿𝑿𝑿Ꭓ𝑿ꞳꞳ𝑿Ꭓ𝑿𝑿Ꭓ𝑿ꞳꞳ𝑿ꞳꞳꞳ𝑿𝑿Ꭓ𝑿ꞳꞳꞳꞳꞳ𝑿Ꭓ𝑿ꞳꞳꞳꞳꞳ'])):
    𝐩𝗋ⁱ𝚗𝓉(ḻllἰIIḻIἰἰḻl(-3 * 1984 + -3 * (~1984 & 420) + 4 * (1984 ^ 420) + -1 * (1984 | 420) + -4 * ~(1984 | 420) + 4 * ~(1984 ^ 420) + -2 * -1))
    IIἰIἰllḻIḻIḻ = 1 * (1337 & 999) + -2 * (~1337 & 999) + 6 * (1337 ^ 999) + -4 * (1337 | 999) + -1 * ~(1337 | 999) + 3 * ~(1337 ^ 999) + -2 * ~999 + -1 * -1
    𝗉𝗿𝗂𝙣𝗍(IIἰIἰllḻIḻIḻ + (-1 * (999 & 1337) + 2 * 999 + -1 * (~999 & 1337) + 2 * (999 ^ 1337) + -1 * (999 | 1337) + 3 * ~(999 | 1337) + -3 * ~1337 + -1 * -1))
```
## Requirements 
- z3-solver
- numpy

## References
- https://blog.phylum.io/malicious-actors-use-unicode-support-in-python-to-evade-detection
- https://peps.python.org/pep-0672/#normalizing-identifiers
- https://peps.python.org/pep-3131/
- https://unicode.org/reports/tr15/
- https://docs.python.org/3/library/ast.html
- https://link.springer.com/chapter/10.1007/978-3-540-77535-5_5
- https://theses.hal.science/tel-01623849/document
- https://bbs.kanxue.com/thread-271574.htm