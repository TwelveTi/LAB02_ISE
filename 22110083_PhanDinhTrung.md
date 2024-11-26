# Lab 2: File Transfer and Encryption
# Student: 22110083_PhanDinhTrung
# Class: INSE33030E_02FIE

# Task 1: Public-key based authentication 
**Question 1**: 
Implement public-key based authentication step-by-step with openssl according the following scheme.
![alt text](image-1.png)

**Answer 1**:
We have two machine: Ubuntu and kali-linux.

In **kali-linux**: we create double key: private_key and public_key by RSA.

We use ``openssl genpkey -algorithm RSA -out private_key.pem`` to create a private_key.

**openssl**: This is a command-line tool commonly used for creating and managing encryption keys, certificates, and other cryptographic operations.

**genpkey**: This subcommand is used to generate a private key.

**-algorithm RSA**: This option specifies the algorithm to use for generating the key. In this case, it's RSA (Rivest–Shamir–Adleman), which is a widely-used asymmetric encryption algorithm.

**-out private_key.pem**: This option specifies the name of the output file where the private key will be saved. The file will be in PEM (Privacy-Enhanced Mail) format, which is a common format for storing keys and certificates.
![image](https://github.com/user-attachments/assets/af5a4ac2-89d5-4536-83bd-1072f7d7bb88)
we created private_key successfully

Continue, we create public_key: this key use to Encryption information and only the created private_key can Decryption.

We use ``openssl rsa -pubout -in private_key.pem -out public_key.pem`` to create public_key.

**openssl**: This is a command-line tool commonly used for creating and managing encryption keys, certificates, and other cryptographic operations.

**rsa**: This subcommand is used to process RSA keys, such as viewing them, converting their format, or extracting a public key from a private key.

**-pubout**: This option specifies that the public key should be extracted from the private key. Without this option, the command would only process the private key. With -pubout, it generates the corresponding public key.

**-in private_key.pem**: This option specifies the input file, which in this case is private_key.pem, containing the previously generated RSA private key.

**-out public_key.pem**: This option specifies the output file, where the extracted RSA public key will be saved. The file will be in PEM format.
![image](https://github.com/user-attachments/assets/aa904355-8feb-4099-9155-2033097c74db)
We create public_key. Now, we have two file: private_key.pem and public_key.pem. We send public_key.pem to Ubuntu and Ubuntu will use it.

In **Ubuntu**, we use ``ip a s`` to get ip address of eth0
![image](https://github.com/user-attachments/assets/4a2ab698-d5ff-4235-963e-eecba6d32dfa)
So, ip adrress of eth0 is:``172.28.65.27``
continue, we use ``pwd`` to check which folder you are working in on your system.
![image](https://github.com/user-attachments/assets/3b6a9726-013f-486c-b01a-43b98667a9fd)

Continue with kali-linux: we use ``scp public_key.pem twelvetii@172.28.65.27:/home/twelvetii`` to send public_key.pem to Ubuntu.

**scp**: Stands for Secure Copy Protocol, used to copy files between computers over the network using the SSH (Secure Shell) protocol.

**public_key.pem**: This is file public_key we send.

**twelvetii@172.28.65.27**: This is username and ip of the remote computer where the file will be copied to.

**:/home/twelvetii**: This is the folder path on the remote computer where the file will be copied.

![image](https://github.com/user-attachments/assets/210f1b80-10e7-498a-a581-34c0b5d17f32)
We need enter password Ubuntu to send. So,We send successfully. We check in Ubuntu.
![image](https://github.com/user-attachments/assets/b7859b39-375e-4749-86ac-05b1387e55d9)
we have received the file.

Now, we will Encryption a file by public_key and send kali-linux.we create a flie ``file.txt`` like this:

![image](https://github.com/user-attachments/assets/b6dcaf0d-5c6c-428f-adfd-c47d72f53e65)
We created a file.txt: hi, i'm ubutu. We will Encryption

We use:
``openssl rsautl -encrypt -inkey public_key.pem -pubin -in test.txt -out test.enc``

**openssl**: This is a command-line tool used to perform operations related to encryption, decryption, key management, and many other security features.

**rsautl**: This is a module in OpenSSL used to encrypt, decrypt, sign, and verify using the RSA algorithm.

**-encrypt**: Specifies that you want to encrypt the data.

**-inkey public_key.pem**: This option specifies the public key file that will be used for encryption.

**-pubin**: Tells OpenSSL that the public_key.pem file is a public key. Without this option, OpenSSL will default to treating the file as a private key.

**-in test.txt**: Specifies the source text file (test.txt) containing the data to be encrypted.

**-out test.enc**: Specifies the destination file (test.enc) to store the encrypted data.

![image](https://github.com/user-attachments/assets/4dcbf406-9531-4dd7-9dd3-0815226a23a2)
**The command rsautl was deprecated in version 3.0. Use 'pkeyutl' instead.**
This message indicates that the ``rsautl`` command is deprecated in OpenSSL 3.0. You should switch to using ``pkeyutl`` to ensure compatibility with new versions of OpenSSL.

But, we still created test.enc.

We will send it to kali-linux like we worked.

We check ip address and pwd in Kali-linux.
![image](https://github.com/user-attachments/assets/6830b9f3-ca54-48e4-8698-595f0428c656)

In Ubuntu, we use `` scp test.enc twelvetii@192.168.37.128:/home/twelvetii`` to send test.enc to kali-linux.
![image](https://github.com/user-attachments/assets/497a0d86-a803-4212-acdf-f092044ea280)

We need enter password kali-linux to send. So, we send successfully.
![image](https://github.com/user-attachments/assets/e3fbf0c2-4695-4fcb-9cc6-65b30e8449a1)

We have received the file. We Decryption this file by private_key to can see it.

We use ``openssl pkeyutl -decrypt -inkey private_key.pem -in test.enc -out test.txt`` to Decryption.

**openssl**: Is the OpenSSL command line tool, used to perform encryption, decryption, key management, signing, etc.

**pkeyutl**: This is the command used to perform operations with public and private keys (such as encryption, decryption, signing, and verification).

In this case, you are using it to decrypt data.

**-decrypt**: This option specifies that you want to decrypt encrypted data.

**-inkey private_key.pem**: Specifies the file containing the private key used to decrypt the data.

**-in test.enc**: Specifies the input file (test.enc) containing the data encrypted with the public key.

**-out test.txt**: Specifies the output file (test.txt) to save the data after decryption.

We use ``cat test.txt`` to see it.
![image](https://github.com/user-attachments/assets/8a556baa-641a-4000-9f99-55467eaac91d)
We done.

# Task 2: Encrypting large message 
Create a text file at least 56 bytes.
**Question 1**:
Encrypt the file with aes-256 cipher in CFB and OFB modes. How do you evaluate both cipher as far as error propagation and adjacent plaintext blocks are concerned. 
**Answer 1**:
We create a text file at least 56 bytes:
![image](https://github.com/user-attachments/assets/c8072fdc-5de7-4e9e-b1f4-941f3f4f0492)
We use ``ls -l file.txt`` to check size of file
![image](https://github.com/user-attachments/assets/f33f435a-e884-47ed-9dc7-7699b0d08dda)
The size of this file is 63 bytes.

We will Encryption file.txt by CFB and OFB.
With CFB: We use ``openssl enc -aes-256-cfb -in file.txt -out encrypted_cfb.bin -pass pass:11223344556677889911223344556677`` and see it.

**openssl**: This is the OpenSSL command line tool, used to perform encryption and decryption operations with cryptographic algorithms.

enc: This option tells OpenSSL that you want to perform an encryption operation.

**-aes-256-cfb**: aes-256-cfb specifies the AES (Advanced Encryption Standard) algorithm with a 256-bit key length and CFB (Cipher Feedback) mode.

**AES-256**: This is a symmetric encryption algorithm with a 256-bit key length, providing a very high level of security.

**CFB** (Cipher Feedback): This is a chained encryption mode. In this mode, data is encrypted block by block, and each block depends on the previous block.

**-in file.txt**: specifies the input file to be encrypted. In this case, the file.txt file will be encrypted.

**-out encrypted_cfb.bin**: specifies the output file where OpenSSL will store the encryption results. Here, the encryption results will be saved in the encrypted_cfb.bin file.
**-pass pass:11223344556677889911223344556677**: specifies how to get the password to generate the encryption key.

![image](https://github.com/user-attachments/assets/68d7c43b-0cfd-4677-a86e-903c280196b5)
with OFB: we use ``openssl enc -aes-256-ofb -in file.txt -out encrypted_ofb.bin -pass pass:11223344556677889911223344556677`` and see it
![image](https://github.com/user-attachments/assets/2bbe466a-c46f-4460-8823-629a7712794b)

**CFB**:
Error Propagation in CFB mode is quite strong, it can affect the next data blocks. If there is an error in a byte of ciphertext, the error will be propagated to the next bytes in the plaintext block when decrypted. Specifically:

An error in a byte will change a corresponding byte in the plaintext when decrypted.

The error can propagate to the next bytes during decryption, depending on the size of the CFB (CFB-1, CFB-8, CFB-128). Therefore, an error in a ciphertext block can affect many bytes in the plaintext, causing the data to be corrupted.

**OFB**:
Error Propagation in OFB mode is less than CFB. If there is an error in a byte of ciphertext:

The error only affects that byte in the plaintext when decrypted and does not propagate to the next bytes.

This means that an error in one byte of ciphertext only affects a single byte in the plaintext, and the remaining bytes will not be affected.


**Question 2**:
Modify the 8th byte of encrypted file in both modes (this emulates corrupted ciphertext).

Decrypt corrupted file, watch the result and give your comment on Chaining dependencies and Error propagation criteria.

**Answer 2**:

We use ``xxd encrypted_cfb.bin > temp_cfb.hex`` and nano it to replace.

![image](https://github.com/user-attachments/assets/c13fa4cc-f0e9-4472-bd1e-32fc4fbb1cc4)
Replace d6 = a5.

We use ``xxd -r temp_cfb.hex encrypted_cfb_corrupted.bin`` to change to .bin.
![image](https://github.com/user-attachments/assets/c53aaa6a-4ddd-4290-aa6e-1f729ce783b7)

We use ``xxd encrypted_ofb.bin > temp_ofb.hex`` and nano it to replace.

![image](https://github.com/user-attachments/assets/4d5af28f-a2fd-4f76-8aeb-1c0946c3ef7b)
Replace c7 = a5.

We use ``xxd -r temp_ofb.hex encrypted_ofb_corrupted.bin`` to change to .bin.
![image](https://github.com/user-attachments/assets/f848415b-5e82-4801-9744-3da24cc4368d)

We use ``openssl enc -aes-256-cfb -d -in encrypted_cfb_corrupted.bin -out decrypted_cfb.txt -pass pass:11223344556677889911223344556677`` and see it.
![image](https://github.com/user-attachments/assets/1c284d3e-dd4f-40f2-bf9f-1e3d7e1a0536)

We use ``openssl enc -aes-256-ofb -d -in encrypted_ofb_corrupted.bin -out decrypted_ofb.txt -pass pass:11223344556677889911223344556677`` and see it.
![image](https://github.com/user-attachments/assets/7ca6596a-c21f-4591-a39d-8596856aad74)

**CFB**:
Chaining dependencies: CFB mode has chaining dependencies, meaning that each plaintext block is affected by the previous ciphertext block. This makes it possible for an error in the ciphertext to affect subsequent bytes in the plaintext.

Error propagation: An error in one byte of the ciphertext can propagate to subsequent bytes in the plaintext when decrypted.

**OFB**:
Chaining dependencies: OFB mode has no dependencies between data blocks. Each plaintext block is encrypted independently of previous ciphertext blocks.

Error propagation: An error in one byte of the ciphertext affects only a single byte in the plaintext and does not affect subsequent bytes




