# Anomaly 72 - Reverse Engineering Decryption Challenge

# Static Analysis
So we are given a 32 ELF binary. First I am going to try static analysis to see if we can easily find the code. Opening in Binary Ninja we see the main function
![image_43.png](image_43.png)
This is calling out to some decrypt function
![image_44.png](image_44.png)
This looks like a shift cipher. Recreating this in python will give us:
```Python

def decrypt(to_shift, amount_shift, length):
    decrypted = ''
    for i in range(length):
        char = to_shift[i]
        decrypted_char = chr(((ord(char) - ord('a') - amount_shift + 26) % 26) + ord('a'))
        decrypted += decrypted_char
    return decrypted

def main():
    to_shift = "wixycm"
    to_shift_1 = "popgpiw"
    to_shift_2 = "wodbsyx"
    to_shift_3 = "ensymtx"

    amount_shift = 20

    var_88 = ""
    var_88 += decrypt(to_shift, amount_shift, len(to_shift))
    var_88 += decrypt(to_shift_1, amount_shift - 5, len(to_shift_1))
    var_88 += decrypt(to_shift_2, amount_shift - 10, len(to_shift_2))
    var_88 += decrypt(to_shift_3, amount_shift - 15, len(to_shift_3))

    print(var_88)

if __name__ == "__main__":
    main()


```

# Dynamic Analysis

Might do later but if i install the right lib