# Anomaly 62 - Ransomwhere?

So we are given a encoding program.

```Python
seed(SEED_VALUE & 0xFFFF)

iv = randbytes(16)
key = randbytes(16)
```

The seed is being limited to 65536 possibles low enough to where we can brute force it.

```Python
from Crypto.Cipher import AES
from Crypto.Util import Padding
from random import *
import os

encrypted_file_path = 'secret.py.enc'

def decrypt_with_seed(seed_value):
    seed(seed_value & 0xFFFF)

    # Generate IV and key
    iv = randbytes(16)
    key = randbytes(16)

    with open(encrypted_file_path, 'rb') as f:
        encrypted_data = f.read()

    cipher = AES.new(key, AES.MODE_CBC, iv=iv)
    decrypted_data = cipher.decrypt(encrypted_data)

    try:
        decrypted_data = Padding.unpad(decrypted_data, AES.block_size)
        decrypted_data.decode('utf-8')
        return decrypted_data
    except (ValueError, UnicodeDecodeError):
        return None

for seed_value in range(65536):
    decrypted_data = decrypt_with_seed(seed_value)
    if decrypted_data:
        print(f"Successfully decrypted with seed {seed_value}:")
        print(decrypted_data.decode('utf-8'))
        break
else:
    print("Failed to decrypt the file with any seed value.")

```

Trying this wont work. Looking at it again we see that it uses new iv and key after every file so adding more unitl it works.

```Python
from Crypto.Cipher import AES
from Crypto.Util import Padding
from random import *
import os

# Path to the encrypted file
encrypted_file_path = 'secret.py.enc'

# Function to decrypt the file with a given seed
def decrypt_with_seed(seed_value):
    seed(seed_value & 0xFFFF)

    # Generate IV and key
    iv = randbytes(16)
    key = randbytes(16)
    iv = randbytes(16)
    key = randbytes(16)
    iv = randbytes(16)
    key = randbytes(16)
    iv = randbytes(16)
    key = randbytes(16)
    iv = randbytes(16)
    key = randbytes(16)

    # Read the encrypted data
    with open(encrypted_file_path, 'rb') as f:
        encrypted_data = f.read()

    # Decrypt the data
    cipher = AES.new(key, AES.MODE_CBC, iv=iv)
    decrypted_data = cipher.decrypt(encrypted_data)

    try:
        # Unpad the decrypted data
        decrypted_data = Padding.unpad(decrypted_data, AES.block_size)
        # Try to decode the decrypted data to check if it's valid
        decrypted_data.decode('utf-8')
        return decrypted_data
    except (ValueError, UnicodeDecodeError):
        return None

# Brute-force all possible seed values (0 to 65535)
for seed_value in range(65536):
    decrypted_data = decrypt_with_seed(seed_value)
    if decrypted_data:
        print(f"Successfully decrypted with seed {seed_value}:")
        print(decrypted_data.decode('utf-8'))
        break
else:
    print("Failed to decrypt the file with any seed value.")

```

Output:
```output
Successfully decrypted with seed 52226:
SEED_VALUE = 9871234598751234
FLAG = "flag{y0u_m3an_l1k3_a_m1n3cr4ft_s33d_fc1b9ec9937ae4cd32c335fe90536fc6}"
```