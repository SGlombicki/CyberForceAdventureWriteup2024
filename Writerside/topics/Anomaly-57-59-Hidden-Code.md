# Anomaly 57-59 - Hidden Code

# Anomaly 57
Opening the word document we see red text and hidden text.
`I3Z0h3Q0pwbt`

# Anomaly 58
`MOON`

# Anomaly 59
Assembling the code gives us

```Python
def decrypt(encrypted_message, secret_key):
    decrypted_message = []
    key_length = len(secret_key)
    for i, char in enumerate(encrypted_message):
        key_char = secret_key[i % key_length]
        shift = ord(key_char) - ord('A')
        if 'A' <= char <= 'Z':
            decrypted_char = chr((ord(char) - ord('A') - shift) % 26 + ord('A'))
        elif 'a' <= char <= 'z':
            decrypted_char = chr((ord(char) - ord('a') - shift) % 26 + ord('a'))
        else:
            decrypted_char = char
    decrypted_message.append(decrypted_char)
    return "".join(decrypted_message)
    
decrypted_message = decrypt("I3Z0h3Q0pwbt", "MOON")
print("Decrypted message: ", decrypted_message")
```

Running it will give you the flag