## PlayFair Cipher
Playfair Cipher using with different key values

## AIM:
To develop a simple C program to implement PlayFair Cipher.

 ## DESIGN STEPS:
Step 1:
Design of PlayFair Cipher algorithnm

Step 2:
Implementation using C or pyhton code

Step 3:
Testing algorithm with different key values.

## PROGRAM:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5

void generateKeyTable(char key[], int keyLength, char keyTable[SIZE][SIZE]) {
    int used[26] = {0};  // To track used characters
    int i, j, k = 0;

    for (i = 0; i < keyLength; i++) {
        if (key[i] != 'j') {
            if (used[key[i] - 'a'] == 0) {
                used[key[i] - 'a'] = 1;
                keyTable[k / SIZE][k % SIZE] = key[i];
                k++;
            }
        }
    }

    for (i = 0; i < 26; i++) {
        if (i + 'a' != 'j' && used[i] == 0) {
            keyTable[k / SIZE][k % SIZE] = i + 'a';
            k++;
        }
    }
}

void search(char keyTable[SIZE][SIZE], char a, char b, int *row1, int *col1, int *row2, int *col2) {
    int i, j;

    for (i = 0; i < SIZE; i++) {
        for (j = 0; j < SIZE; j++) {
            if (keyTable[i][j] == a) {
                *row1 = i;
                *col1 = j;
            } else if (keyTable[i][j] == b) {
                *row2 = i;
                *col2 = j;
            }
        }
    }
}

void encryptPair(char keyTable[SIZE][SIZE], char a, char b, char *x, char *y) {
    int row1, col1, row2, col2;

    search(keyTable, a, b, &row1, &col1, &row2, &col2);

    if (row1 == row2) {
        *x = keyTable[row1][(col1 + 1) % SIZE];
        *y = keyTable[row2][(col2 + 1) % SIZE];
    } else if (col1 == col2) {
        *x = keyTable[(row1 + 1) % SIZE][col1];
        *y = keyTable[(row2 + 1) % SIZE][col2];
    } else {
        *x = keyTable[row1][col2];
        *y = keyTable[row2][col1];
    }
}

void decryptPair(char keyTable[SIZE][SIZE], char a, char b, char *x, char *y) {
    int row1, col1, row2, col2;

    search(keyTable, a, b, &row1, &col1, &row2, &col2);

    if (row1 == row2) {
        *x = keyTable[row1][(col1 + SIZE - 1) % SIZE];
        *y = keyTable[row2][(col2 + SIZE - 1) % SIZE];
    } else if (col1 == col2) {
        *x = keyTable[(row1 + SIZE - 1) % SIZE][col1];
        *y = keyTable[(row2 + SIZE - 1) % SIZE][col2];
    } else {
        *x = keyTable[row1][col2];
        *y = keyTable[row2][col1];
    }
}

void encrypt(char keyTable[SIZE][SIZE], char plainText[], char cipherText[]) {
    int i, j = 0;
    char a, b;

    for (i = 0; plainText[i] != '\0'; i += 2) {
        a = plainText[i];
        if (plainText[i + 1] != '\0') {
            b = plainText[i + 1];
        } else {
            b = 'x';
        }

        if (a == b) {
            b = 'x';
            i--;
        }

        encryptPair(keyTable, a, b, &cipherText[j], &cipherText[j + 1]);
        j += 2;
    }
    cipherText[j] = '\0';
}

void decrypt(char keyTable[SIZE][SIZE], char cipherText[], char plainText[]) {
    int i, j = 0;
    char a, b;

    for (i = 0; cipherText[i] != '\0'; i += 2) {
        a = cipherText[i];
        b = cipherText[i + 1];

        decryptPair(keyTable, a, b, &plainText[j], &plainText[j + 1]);
        j += 2;
    }
    plainText[j] = '\0';
}

int main() {
    char key[100], plainText[100], cipherText[100], decryptedText[100];
    char keyTable[SIZE][SIZE];
    int i, keyLength;

    printf("Enter the key: ");
    scanf("%s", key);

    for (i = 0; key[i] != '\0'; i++) {
        key[i] = tolower(key[i]);
    }

    keyLength = strlen(key);
    generateKeyTable(key, keyLength, keyTable);

    printf("Enter the plaintext: ");
    scanf("%s", plainText);

    for (i = 0; plainText[i] != '\0'; i++) {
        plainText[i] = tolower(plainText[i]);
    }

    encrypt(keyTable, plainText, cipherText);
    printf("Encrypted Text: %s\n", cipherText);

    decrypt(keyTable, cipherText, decryptedText);
    printf("Decrypted Text: %s\n", decryptedText);

    return 0;
}

```
## OUTPUT:
![image](https://github.com/user-attachments/assets/f904f4ee-4806-49b9-bb30-7f19b306733e)

## RESULT:
The program is executed successfully
