+++
title = "Dissecting elifoot 98 equipas"
date = "2025-01-30"
+++

### Table of Contents

1. [Why I am writing this (TLDR)](#1-why-i-am-writing-this-tldr)
2. [The equipa format](#2-the-equipa-format)
    - [2.1 The EFa header](#2-1-the-efa-header)
    - [2.2 The short name/extended name](#2-2-the-short-name-extendend-name)
    - [2.3 The colors](#2-3-the-colors)
    - [2.4 The country](#2-4-the-country)
    - [2.5 The level](#2-5-the-level)
    - [2.6 The number of players](#2-6-the-number-of-players)
    - [2.7 The player list](#2-7-the-player-list)
    - [2.8 The coach](#2-8-the-coach)
3. [The encryption algorithm](#3-the-encryption-algorithm)

### 1. Why I am writing this (TLDR)

Back to 90s, when I had my first contact with a computer.

This post aims to provide a technical description of the equipa's file format of elifoot 98, focusing on how the team data is arranged into the equipa file and how it's interpreted by the game.

### 2. The equipa format

The equipa is a binary file and as you can guess, it contains all the team data (which will be discussed in the following sub topics), that are stored as sequential chunks of information across the file. It has the following format:

```
 0               1               2               3
 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                                                               |
|                                                               |
|                                                               |
|                                                               |
|                                                               |
|                            EFa header                         |
|                                                               |
|                                                               |
|                                                               |
|                                                               |
|                                                               |
|                                 |XXXXXXXXXXXXXXXXXXXXXXXXXXXXX|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Field size  |      Team extended name (variable size)       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Field size  |        Team short name (variable size)        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|              Team background color            |     Unused    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|              Team foreground color            |     Unused    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Field size  |                 Team country                  |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Team level  |   Team size   |XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Field size  |               Player 0 country                |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Field size  | Player 0 name (variable size) |Player position|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Field size  |               Player 1 country                |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Field size  | Player 1 name (variable size) |Player position|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Field size  |               Player n country                |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Field size  | Player n name (variable size) |Player position|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   Field size  |           Coach name (variable size)          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

> **NOTE**: The 'X' bits placed in certain fields of the diagram above are just for alignment purposes and aren't present on a real equipa file.

#### 2.1 The EFa header

The EFa header doesn't contain any special information, it just acts as an identifier for the equipa file and without it, the file is not recognized as a valid equipa by editeq (the equipas editor) and the game itself. The header occupies the offset range ```0x00 - 0x31```, having 50 bytes of size. Its content is composed by the ```'EFa'``` ascii string followed by 47 zero bytes.

#### 2.2 The short name/extendend name

The ```'short'``` and ```'extended'``` name fields shares the same structure: both starts with 1 byte containing the field size, followed by the field value itself. The field value is encrypted and the encryption algorithm will be discussed in section 3.

#### 2.3 The colors

The ```'colors'``` field has 8 bytes of size, 4 bytes for each color. Both background and text colors has the same structure: 3 bytes for the color itself (in RGB format) followed by 1 unused byte.

#### 2.4 The country

The ```'country'``` field contains the initial letters (in portuguese) of the equipa's country and has 4 bytes of size. The first byte contains the field size (which usually is 3) and the last 3 bytes contains the encrypted initial letters. This information is used by elifoot to group the equipas according to its country and show the equipa's country flag correctly. Thus, the value of this field must to correspond to a bitmap entry on "FLAGS" directory, placed at the root directory of the game.

#### 2.5 The level

The ```'level'``` field has 1 byte of size and contains the equipa's level, in hex format.

#### 2.6 The number of players

This field has 1 byte of size and contains the number of players who composes the equipa, in hex format.

#### 2.7 The player list

The ```'player list'``` field contains the list of players who composes the equipa. This field has 'n' entries, where 'n' is defined according to the prior field ([2.6](#2-6-the-number-of-players)). The first 4 bytes of each entry of the list, as shown on diagram above, defines the player's nationality, according to the format described on section [2.4](#2-4-the-country). The next 1 byte contains the player's name size followed by the encrypted player's name itself. The last byte defines the player's position code: 0 to goalkeeper, 1 to defender, 2 to midfielder and 3 to forward.

#### 2.8 The coach

The ```'coach'``` field contains the coach's name of the equipa and has the same structure of the other text-based fields with variable size, starting with 1 size byte followed by the encrypted coach's name itself.

### 3. The encryption algorithm

Text based fields as mentioned earlier on [2.2](#2-2-the-short-name-extendend-name), [2.4](#2-4-the-country), [2.7](#2-7-the-player-list) and [2.8](#2-8-the-coach) are encrypted. The algorithm used converts a human readable string (e.g the equipa name) into a ciphered binary array, using a pretty straightfoward algorithm: given an input string (e.g ```'elf98'```), the output of the encryption process consists of an array that starts with the input length, followed by the encoded input itself. Such encoding is made by iterating over the input string and picking up the less significant byte resulting of the sum of each character of it and the last byte of the binary array. The result of each iteration is appended to the output array. For example, the encrypted version of the string ```'elf98'``` is ```'056ad63c75ad'```:

```
05 = input string length
65 ('e' in ascii) + 5  = 6a
6c ('l' in ascii) + 6a = d6
66 ('f' in ascii) + d6 = 3c (discarding the most significant byte)
39 ('9' in ascii) + 3c = 75
38 ('8' in ascii) + 75 = ad
```

A simple 'pythonic' version of this algorithm can be implemented as follows:

```python
def encrypt(text: str) -> bytearray:
    data = bytearray()

    data.append(len(text))
    for i in range(0, len(text)):
        data.append((ord(text[i]) + data[i]) & 0xff)

    return data
```
