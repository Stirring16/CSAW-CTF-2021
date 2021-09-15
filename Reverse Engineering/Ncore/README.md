# ncore

We have a very safe core with a very safe enclave.


[ncore](https://drive.google.com/drive/folders/1Jmz6ircbMZEWyKdZD4vo4SUsZYcVNHId?usp=sharing)

## Generality

We are given 2 files including a verilog source code file (hardware programming language) ncore_tb.v and a server.py file:
- server.py is responsible for reading input from the client, and writing to the file
ram.hex (this file acts as the bytecode of a small VM), then server.py will be responsible for simulating the verilog code, and sending the output to the client
- ncore_tb.v as mentioned above, its core is building a vm, and relies on the bytecode we send to it to work and print the output. So we have to find the input to rely on vm to leak the flag content

## Analysis ncore_tb.v
### 1. Initialize important VM components

- Initialize the components of the VM such as register, ram (which is our bytecode array), safeROM (the hexadecimal array of flags), random key, and an emode value (which acts as the check variable)

![image](https://user-images.githubusercontent.com/62060867/133419305-edb0763a-8db6-4b10-867b-86eafef31470.png)

- In addition, a vm cannot lack instructions, each initialization instruction is assigned a constant value - opcode (for example, the instruction 'ADD' has an opcode of 0, 'SUB'-4, similarly for instructions rest

![image](https://user-images.githubusercontent.com/62060867/133419492-7ae92f42-acad-403b-96f9-4dcf7be5e8b4.png)

### 2. Exploit VM và get flag
- After analyzing the operation of the instructions of the VM, I noticed a few important commands such as:

+ MOVFS: load the hex-byte of the flag to the register of the VM, here combined with the MOVT command (store the reg value to the VM's ram), we can successfully leak the flag

![image](https://user-images.githubusercontent.com/62060867/133419613-1e406ce1-e088-4d6a-bd94-864bbbc052a9.png)

+ However, the emode variable must be set = 1 (initialize it to 0), so the first challenge to overcome is to set this variable = 1
- To set the variable emmode = 1, we need to find exactly 14 LSBs (least significant bits) of random-key

![image](https://user-images.githubusercontent.com/62060867/133419810-50c519d5-e6ac-4775-9a4a-4207bf1042b5.png)

- In short, our bytecode sent to the server will do 2 tasks:

+ Brute force exactly 14 LSBs of the random key, the way I solve it is I will build a loop, and give the value regfile[0] increments by 1 each time I run, and right after that I will check if the regfile[3 registers ] is set = 1 or not. If yes, then emode has been set = 1 (This step takes 10 bytes of bytecode)

+ Then, I move to the leak flag section, here when I solve, I don't leak based on the loop, but just leak each byte of the flag, byte[0], byte[1]... every byte will lose 2 instructions MOVFS, MOVT (ie 4 bytes of bytecode). If len(flag) = 64, it takes 256 bytes (too long, I only have 256 bytes for both brute-force key and flag leak) so I have a small trick: each time I interact with the server, I only leak 32 bytes of the flag. , if the flag has 64 bytes, it needs 2 times.

- And here is my input

![image](https://user-images.githubusercontent.com/62060867/133420197-840cf33a-0664-413f-9e9f-eeab99699e23.png)

- '3c 90 0c 90 07 90 ba 0a 0b 02': First 10 bytes for brute force key

- The remaining bytes for the leak flag

Here is the pseudo-code of the payload I built manually:

![image](https://user-images.githubusercontent.com/62060867/133420303-7e9fa98f-8359-4f02-a18f-3398e6374c30.png)

Fortunately, the flag is only 32 bytes, so I only need to interact with the server once to get the flag 

![image](https://user-images.githubusercontent.com/62060867/133420450-ca902764-46fb-4c46-ba6a-15134a36c8fa.png)

So we got the flag: `flag{d0nT_mESs_wiTh_tHe_sChLAmi}`


