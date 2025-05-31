# Crypto Techniques

## XOR Decrypt
```python
a = b"secret"
key = 0x37
print(bytes([x ^ key for x in a]))
```

## Classical
- Caesar: shift +13
- Vigenère: repeating key

## RSA
Small e, low modulus → attack with Coppersmith/Wiener
