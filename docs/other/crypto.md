# Cryptography

Encoding, decoding, RSA, hashing tricks.

---

## Encoding / Decoding

```bash
# base64
echo "hello" | base64
echo "aGVsbG8=" | base64 -d

# URL decode (Python)
python3 -c "import urllib.parse; print(urllib.parse.unquote('%64%25'))"

# ROT13
echo "cvpbPGS{abg_gbb_onq}" | tr 'A-Za-z' 'N-ZA-Mn-za-m'

# hex to bytes (Python)
python3 -c "print(bytes.fromhex('48656c6c6f').decode())"

# openssl — connect to SSL service
openssl s_client -connect <IP>:443
openssl s_client -connect <IP>:<port> -quiet
```

---

## RSA Decryption (Python)

```python
from Crypto.Util.number import inverse

p = 430535396861370041
q = 17209058493553260637
n = p * q
e = 257
c = 3086274334409993602095103985623480747

totient = (p-1) * (q-1)
d = inverse(e, totient)
m = pow(c, d, n)

plaintext = bytes.fromhex(hex(m)[2:]).decode('ASCII')
print(plaintext)
```
