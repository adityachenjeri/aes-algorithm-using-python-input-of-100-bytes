# aes-algorithm-using-python-input-of-100-bytes
aes algorithm using python input of 100 bytes
s_box = [
    0x63, 0x7C, 0x77, 0x7B, 0xF2, 0x6B, 0x6F, 0xC5, 0x30, 0x01, 0x67, 0x2B, 0xFE, 0xD7, 0xAB, 0x76,
    0xCA, 0x82, 0xC9, 0x7D, 0xFA, 0x59, 0x47, 0xF0, 0xAD, 0xD4, 0xA2, 0xAF, 0x9C, 0xA4, 0x72, 0xC0,
    0xB7, 0xFD, 0x93, 0x26, 0x36, 0x3F, 0xF7, 0xCC, 0x34, 0xA5, 0xE5, 0xF1, 0x71, 0xD8, 0x31, 0x15,
    0x04, 0xC7, 0x23, 0xC3, 0x18, 0x96, 0x05, 0x9A, 0x07, 0x12, 0x80, 0xE2, 0xEB, 0x27, 0xB2, 0x75,
    0x09, 0x83, 0x2C, 0x1A, 0x1B, 0x6E, 0x5A, 0xA0, 0x52, 0x3B, 0xD6, 0xB3, 0x29, 0xE3, 0x2F, 0x84,
    0x53, 0xD1, 0x00, 0xED, 0x20, 0xFC, 0xB1, 0x5B, 0x6A, 0xCB, 0xBE, 0x39, 0x4A, 0x4C, 0x58, 0xCF,
    0xD0, 0xEF, 0xAA, 0xFB, 0x43, 0x4D, 0x33, 0x85, 0x45, 0xF9, 0x02, 0x7F, 0x50, 0x3C, 0x9F, 0xA8,
    0x51, 0xA3, 0x40, 0x8F, 0x92, 0x9D, 0x38, 0xF5, 0xBC, 0xB6, 0xDA, 0x21, 0x10, 0xFF, 0xF3, 0xD2,
    0xCD, 0x0C, 0x13, 0xEC, 0x5F, 0x97, 0x44, 0x17, 0xC4, 0xA7, 0x7E, 0x3D, 0x64, 0x5D, 0x19, 0x73,
    0x60, 0x81, 0x4F, 0xDC, 0x22, 0x2A, 0x90, 0x88, 0x46, 0xEE, 0xB8, 0x14, 0xDE, 0x5E, 0x0B, 0xDB,
    0xE0, 0x32, 0x3A, 0x0A, 0x49, 0x06, 0x24, 0x5C, 0xC2, 0xD3, 0xAC, 0x62, 0x91, 0x95, 0xE4, 0x79,
    0xE7, 0xC8, 0x37, 0x6D, 0x8D, 0xD5, 0x4E, 0xA9, 0x6C, 0x56, 0xF4, 0xEA, 0x65, 0x7A, 0xAE, 0x08,
    0xBA, 0x78, 0x25, 0x2E, 0x1C, 0xA6, 0xB4, 0xC6, 0xE8, 0xDD, 0x74, 0x1F, 0x4B, 0xBD, 0x8B, 0x8A,
    0x70, 0x3E, 0xB5, 0x66, 0x48, 0x03, 0xF6, 0x0E, 0x61, 0x35, 0x57, 0xB9, 0x86, 0xC1, 0x1D, 0x9E,
    0xE1, 0xF8, 0x98, 0x11, 0x69, 0xD9, 0x8E, 0x94, 0x9B, 0x1E, 0x87, 0xE9, 0xCE, 0x55, 0x28, 0xDF,
    0x8C, 0xA1, 0x89, 0x0D, 0xBF, 0xE6, 0x42, 0x68, 0x41, 0x99, 0x2D, 0x0F, 0xB0, 0x54, 0xBB, 0x16
]



This block defines the S-box, which is a substitution table used in the AES algorithm. It is a list of 256 values representing the substitution of each byte in the state during encryption.



round_constants = [
    [0x01, 0x00, 0x00, 0x00],
    [0x02, 0x00, 0x00, 0x00],
    [0x04, 0x00, 0x00, 0x00],
    [0x08, 0x00, 0x00, 0x00],
    [0x10, 0x00, 0x00, 0x00],
    [0x20, 0x00, 0x00, 0x00],
    [0x40, 0x00, 0x00, 0x00],
    [0x80, 0x00, 0x00, 0x00],
    [0x1B, 0x00, 0x00, 0x00],
    [0x36, 0x00, 0x00, 0x00]
]


This block defines the round constants, which are used during the key expansion process in AES. These constants are XORed with certain words in the key schedule to introduce non-linearity into the algorithm.


def sub_bytes(state):
    for i in range(4):
        for j in range(4):
            state[i][j] = s_box[state[i][j]]
            
          
This function performs the SubBytes transformation on the state. It replaces each byte in the state with its corresponding value from the S-box.

def shift_rows(state):
    for i in range(1, 4):
        state[i] = state[i][i:] + state[i][:i]
        
This function performs the ShiftRows transformation on the state. It cyclically shifts the rows of the state matrix.


def mix_columns(state):
    for i in range(4):
        a = state[i][0]
        b = state[i][1]
        c = state[i][2]
        d = state[i][3]

        state[i][0] = (
            (0x02 * a) ^ (0x03 * b) ^ c ^ d
        ) & 0xFF
        state[i][1] = (
            a ^ (0x02 * b) ^ (0x03 * c) ^ d
        ) & 0xFF
        state[i][2] = (
            a ^ b ^ (0x02 * c) ^ (0x03 * d)
        ) & 0xFF
        state[i][3] = (
            (0x03 * a) ^ b ^ c ^ (0x02 * d)
        ) & 0xFF

This function performs the MixColumns transformation on the state. It applies a linear transformation to each column of the state matrix.

def add_round_key(state, key):
    for i in range(4):
        for j in range(4):
            state[i][j] ^= key[i][j]
            
 This function performs the AddRoundKey operation on the state. It combines each byte of the state with the corresponding byte of the round key by performing an XOR operation.
 
 def expand_key(key):
    key_schedule = [[key[i + j] for j in range(4)] for i in range(0, 16, 4)]

    for i in range(4, 44):
        temp = list(key_schedule[i - 1])

        if i % 4 == 0:
            temp = temp[1:] + [temp[0]]
            for j in range(4):
                temp[j] = s_box[temp[j]]
            temp[0] ^= round_constants[i // 4 - 1][0]

        key_schedule.append([
            (key_schedule[i - 4][j] ^ temp[j]) & 0xFF
            for j in range(4)
        ])

    return key_schedule


This function expands the original key into a key schedule used during the encryption process. It generates a total of 44 words, each consisting of 4 bytes. The expansion process involves applying the key schedule core, performing byte substitutions, and XORing with round constants.


def pad_data(data):
    padding_length = 16 - (len(data) % 16)
    padded_data = data + bytes([padding_length] * padding_length)
    return padded_data


This function pads the input data to a multiple of 16 bytes using PKCS7 padding. The padding length is determined by the number of bytes needed to reach the next multiple of 16.



def encrypt_block(block, key):
    state = [[block[i + j] for j in range(4)] for i in range(0, 16, 4)]
    key_schedule = expand_key(key)

    add_round_key(state, key_schedule[:4])

    for round in range(1, 10):
        sub_bytes(state)
        shift_rows(state)
        mix_columns(state)
        add_round_key(state, key_schedule[round * 4:(round + 1) * 4])

    sub_bytes(state)
    shift_rows(state)
    add_round_key(state, key_schedule[40:])

    encrypted_block = bytes([state[i][j] for i in range(4) for j in range(4)])
    return encrypted_block

This function encrypts a single block of 16 bytes using AES. It takes a block and a key as inputs. It applies a series of AES transformations, including SubBytes, ShiftRows, MixColumns, and AddRoundKey, for multiple rounds.

def encrypt(data, key):
    padded_data = pad_data(data)
    encrypted_data = b""

    for i in range(0, len(padded_data), 16):
        block = padded_data[i:i + 16]
        encrypted_block = encrypt_block(block, key)
        encrypted_data += encrypted_block

    return encrypted_data

This function encrypts the entire input data by breaking it into blocks of 16 bytes and encrypting each block using the encrypt_block function. The padded data is processed block by block, and the encrypted blocks are concatenated to form the final encrypted data.

Overall, the code implements the AES encryption algorithm by providing functions for key expansion, padding, and various AES transformations. The encrypt function serves as the entry point for encrypting data using AES.
