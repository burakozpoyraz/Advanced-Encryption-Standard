# Advanced Encryption Standard
![Issues Badge](https://img.shields.io/github/issues/burakozpoyraz/Advanced-Encryption-Standard)
![Forks Badge](https://img.shields.io/github/forks/burakozpoyraz/Advanced-Encryption-Standard)
![Stars Badge](https://img.shields.io/github/stars/burakozpoyraz/Advanced-Encryption-Standard)

This is the implementation of well-known 128-bit Rijndael encryption algorithm called Advanced Encryption Standard (AES) using Python programming language. In this project, there are three main outcomes as follows:

- The entire algorithm of encrypting and decrypting a 4x4 hexadecimal matrix
- Message encryption via AES
- AES algorithm in wireless communication

## Table of Contents

- [Functions](https://github.com/burakozpoyraz/Advanced-Encryption-Standard/blob/master/README.md#functions)
- [Message Encryption](https://github.com/burakozpoyraz/Advanced-Encryption-Standard/blob/master/README.md#message-encryption)
- [Example](https://github.com/burakozpoyraz/Advanced-Encryption-Standard/blob/master/README.md#example)
- [AES in Wireless Communications](https://github.com/burakozpoyraz/Advanced-Encryption-Standard/blob/master/README.md#aes-in-wireless-communications)
- [Acknowledgements](https://github.com/burakozpoyraz/Advanced-Encryption-Standard/blob/master/README.md#acknowledgements)
- [Licence](https://github.com/burakozpoyraz/Advanced-Encryption-Standard/blob/master/README.md#licence)

## Functions
Here are the main functions and their purpose that the algorithm includes:

- **SubBytesTransformation:** Replaces all elements in a matrix from a table called S-box matrix.
- **ShiftArray:** Shifts a given array to the right or left (up or down correspondingly) by a certain digit.
- **ShiftRows:** Shifts each row of a 4x4 matrix to the left by 0, 1, 2, and 3 digits, repectively.
- **GaloisMultiplication:** Multiplication of two hexadecimal numbers in [Galois Field](https://sites.math.washington.edu/~morrow/336_12/papers/juan.pdf).
- **MixColumns:** Multiplication of a 4x4 matrix by MixColumns matrix.
- **AddRoundKey:** Summation of a 4x4 matrix by a 4x4 key matrix. This actually corresponds to XOR of two matrices.
- **CreateWord:** Generates a specific column of new key matrix with respect to the previous one.
- **GenerateRoundKey:** Generates new key matrix with respect to the previous one.
- **AES_Encrypt:** Implements AES encryption to a 4x4 matrix with the aid of a 4x4 key matrix.
- **EncryptMessage:** Encrypts a message using AES algorithm which is explained in [Message Encryption](https://github.com/burakozpoyraz/Advanced-Encryption-Standard/blob/master/README.md#message-encryption) section detailly.
- **InvSubBytesTransformation:** Replaces all elements in a matrix from a table called Inverse S-box matrix.
- **InvShiftRows:** Shifts each row of a 4x4 matrix to the right by 0, 1, 2, and 3 digits, repectively.
- **InvMixColumns:** Multiplication of a 4x4 matrix by Inverse MixColumns matrix.
- **AES_Decrypt:** Decrypts an encrypted 4x4 matrix with the aid of the same key that is used for encryption.
- **DecryptMessage:** Decrypts an encrypted message with the aid of the same key that is used for encryption.
- **HexMatrixToBitArray:** Converts a 4x4 hexadecimal matrix to 128-length bit array.
- **BitArrayToHexMatrix:** Converts a 128-length bit array to hexadecimal matrix.
- **Channel:** Makes a bit array erroneous given a specific bit error rate (BER).
- **TestChannel:** Tests if *Channel* matrix outcomes a bit array with given BER or not with a certain number of iterations. The more iterations, the closest BER to the given value.
- **IsFrameErroneous:** Checks if a 4x4 matrix is exactly equal to another 4x4 matrix.

## Message Encryption
This is a method to encrypt a message via AES algorithm. Here is the encryption and decryption processes of this algorithm:

### Encryption
The ASCII code of each character in a string message is appended to a buffer array. The elements of the buffer array are then placed into 4x4 matrices under the condition that the order of the elements representing each character does not change. If there are some empty indices in the last matrix, these indices are full-filled with zeros which is called zero-padding. Those matrices are encrypted via AES algorithm. Here is how it works:

![Message2BufferMatrices](https://user-images.githubusercontent.com/18036489/82732261-ccda8080-9d14-11ea-9136-fb98a55eb2c5.jpg)

Then encrypted buffer matrices have to be converted to characters so that encrypted messages are created. For this purpose, each number in the encrypted buffer matrices has to be rounded to a value between \[33 - 126]. First, modulo of each number according to 94 is calculated since there are 94 numbers in that interval. Then, obtained value is added to 33 so that a value between \[33 - 126] range is acquired. The character equivalent of obtained ASCII code is the encrypted character. For instance, the first element of the first encrypted buffer matrix is 144. If it is rounded ```python (144 % 94) + 33 = 83``` is obtained. The character equivalent of 83 is 'S' which is the encrypted character. In order to turn back to 144 from 'S' in decryption process ```python 83 - 33 + a * 94``` calculation is made where ```python a = floor(116 / 94)```; however, *a* should be known. Therefore, *a* is also added to encrypted character as encryption information. As a result, *S1* is obtained indicating 144. Here is the whole encrypted message:

![EncryptedMessage](https://user-images.githubusercontent.com/18036489/82732755-e3cea200-9d17-11ea-8966-36ed60490bb5.jpg)

### Decryption
The characters in encrypted message are utilized as couple the first of which indicates the encrypted character and the second of which is the encryption information. First couple of the encrypted message is *S1*. *S* is the encrypted character and the ASCII code of *S* is 83 and *1* is the encryption information which results in ```python 83 - 33 + 1 * 94 = 144```. This value is the first element of the first buffer matrix. As this process is maintained for all character couples in the encrypted message, buffer matrices are obtained. Then, those buffer matrices are decrypted via AES algorithm and decrypted buffer matrices are converted to a buffer array. At last, every number in the buffer array is replaced with its character equivalent which outputs the original message.

## Example
Here is an example of the functions that can be compared with the results given in [this video](https://www.youtube.com/watch?v=gP4PqVGudtg):

```python
state_matrix = [["0x32","0x88","0x31","0xe0"],
                ["0x43","0x5a","0x31","0x37"],
                ["0xf6","0x30","0x98","0x07"],
                ["0xa8","0x8d","0xa2","0x34"]]
                
cipher_key = [["0x2b","0x28","0xab","0x09"],
              ["0x7e","0xae","0xf7","0xcf"],
              ["0x15","0xd2","0x15","0x4f"],
              ["0x16","0xa6","0x88","0x3c"]]

# AddRoundKey
state_matrix1 = AddRoundKey(state_matrix, cipher_key)

# SubBytesTransformation
state_matrix2 = SubBytesTransformation(state_matrix1)
state_matrix3 = InvSubBytesTransformation(state_matrix2)
sub_bytes_equality = state_matrix1 == state_matrix3

# ShiftRows
state_matrix4 = ShiftRows(state_matrix2)
state_matrix5 = InvShiftRows(state_matrix4)
shift_rows_equality = state_matrix2 == state_matrix5

# MixColumns
state_matrix6 = MixColumns(state_matrix4)
state_matrix7 = InvMixColumns(state_matrix6)
mix_columns_equality = state_matrix4 == state_matrix7

# GenerateRoundKey
round_key = GenerateRoundKey(cipher_key, 1)

# AES Encryption
encrypted_matrix = AES_Encrypt(state_matrix, cipher_key)
decrypted_matrix = AES_Decrypt(encrypted_matrix, cipher_key)
encryption_equality = state_matrix == decrypted_matrix

# Message Encryption
cipher_key = [["0x2b","0x28","0xab","0x09"],
              ["0x7e","0xae","0xf7","0xcf"],
              ["0x15","0xd2","0x15","0x4f"],
              ["0x16","0xa6","0x88","0x3c"]]

message = "This is Advanced Encryption Standard !!!"
encrypted_message = EncryptMessage(message, cipher_key)
decrypted_message = DecryptMessage(encrypted_message, cipher_key)
message_equality = message == decrypted_message

# Channel Simulation
bit_array = HexMatrixToBitArray(state_matrix)
BER = 0.2

iter_num1 = 10
calculated_BER1 = TestChannel(bit_array, BER, iter_num1)
>>> 0.215625 [Finished in 0.4s]

iter_num2 = 1000
calculated_BER2 = TestChannel(bit_array, BER, iter_num2)
>>> 0.201921 [Finished in 0.6s]

iter_num3 = 10 ** 6
calculated_BER3 = TestChannel(bit_array, BER, iter_num3)
>>> 0.200015 [Finished in 219.6s]
```
## AES in Wireless Communications
Within the scope of this project, AES algorithm is analysed in a wireless communication. The main purpose is observing the effects of bit errors on the loss of frames. The steps of the simulation is given as follows:
- 16-byte information is encrypted with AES algorithm.
- The information is passed through a channel causing bit errors with a specified BER value.
- Obtained erroneous information is decrypted.
- Decrypted frame is checked if it matches with the frame that would be obtained when there was no bit errors.
These steps construct one pass of the simulation and it is used 4000 frames and average frame error rate (FER) is observed. At last, the theoretical FER corresponding to a specific BER is calculated with the formula given as below:

![FER_Formula](https://user-images.githubusercontent.com/18036489/82825153-5de75e00-9eb3-11ea-9aaa-c4a1f2996dd3.jpg)

where pe is the specified BER value. The simulation results indicate that average FER values totally match with the theoretical ones:

![BERvsFER](https://user-images.githubusercontent.com/18036489/82824931-eadde780-9eb2-11ea-9eb0-ec1f15795022.png)

## Acknowledgements
I would like to thank to my instructor [Güneş Zeynep Karabulut Kurt](https://www.linkedin.com/in/gunes-karabulut-kurt-1715773/?originalSubdomain=ca) for providing me an opportunity to work on this project and giving us all support and guidance during the project. Also, many thanks to my project partner [Ahmet Doruk Başkan](https://www.linkedin.com/in/ahmet-doruk-ba%C5%9Fkan-73a439120/?originalSubdomain=tr) for huge contributions to construct the entire algorithm.

## Licence
![MIT Licence](https://img.shields.io/github/license/burakozpoyraz/Advanced-Encryption-Standard)
