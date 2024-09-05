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
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

#define SIZE 30

void toLowerCase(char plain[], int ps) {
    for (int i = 0; i < ps; i++) {
        if (plain[i] >= 'A' && plain[i] <= 'Z') {
            plain[i] += 32; // Convert to lowercase
        }
    }
}

int removeSpaces(char *plain, int ps) {
    int count = 0;
    for (int i = 0; i < ps; i++) {
        if (plain[i] != ' ') {
            plain[count++] = plain[i];
        }
    }
    plain[count] = '\0';
    return count;
}

void generateKeyTable(char key[], int ks, char keyT[5][5]) {
    int i, j, k;
    int *dicty = (int *)calloc(26, sizeof(int)); // A 26 character hashmap to store count of the alphabet

    for (i = 0; i < ks; i++) {
        if (key[i] != 'j') {
            dicty[key[i] - 97] = 2;
        }
    }
    dicty['j' - 97] = 1;

    i = 0;
    j = 0;
    for (k = 0; k < ks; k++) {
        if (dicty[key[k] - 97] == 2) {
            dicty[key[k] - 97] -= 1;
            keyT[i][j] = key[k];
            j++;
            if (j == 5) {
                i++;
                j = 0;
            }
        }
    }

    for (k = 0; k < 26; k++) {
        if (dicty[k] == 0) {
            keyT[i][j] = (char)(k + 97);
            j++;
            if (j == 5) {
                i++;
                j = 0;
            }
        }
    }
    free(dicty);
}

void search(char keyT[5][5], char a, char b, int arr[]) {
    if (a == 'j') a = 'i';
    if (b == 'j') b = 'i';

    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            if (keyT[i][j] == a) {
                arr[0] = i;
                arr[1] = j;
            } else if (keyT[i][j] == b) {
                arr[2] = i;
                arr[3] = j;
            }
        }
    }
}

int mod5(int a) {
    return (a % 5);
}

int prepare(char str[], int *ptrs) {
    if (*ptrs % 2 != 0) {
        str[(*ptrs)++] = 'x';
    }
    str[*ptrs] = '\0';
    return *ptrs;
}

void encrypt(char str[], char keyT[5][5], int ps) {
    int a[4];

    for (int i = 0; i < ps; i += 2) {
        if (i + 1 == ps) { // If odd length and no filler, skip last character
            break;
        }

        search(keyT, str[i], str[i + 1], a);
        if (a[0] == a[2]) {
            str[i] = keyT[a[0]][mod5(a[1] + 1)];
            str[i + 1] = keyT[a[0]][mod5(a[3] + 1)];
        } else if (a[1] == a[3]) {
            str[i] = keyT[mod5(a[0] + 1)][a[1]];
            str[i + 1] = keyT[mod5(a[2] + 1)][a[1]];
        } else {
            str[i] = keyT[a[0]][a[3]];
            str[i + 1] = keyT[a[2]][a[1]];
        }
    }
}

void decrypt(char str[], char keyT[5][5], int ps) {
    int a[4];

    for (int i = 0; i < ps; i += 2) {
        if (i + 1 == ps) { // If odd length and no filler, skip last character
            break;
        }

        search(keyT, str[i], str[i + 1], a);
        if (a[0] == a[2]) {
            str[i] = keyT[a[0]][mod5(a[1] + 4)];
            str[i + 1] = keyT[a[0]][mod5(a[3] + 4)];
        } else if (a[1] == a[3]) {
            str[i] = keyT[mod5(a[0] + 4)][a[1]];
            str[i + 1] = keyT[mod5(a[2] + 4)][a[1]];
        } else {
            str[i] = keyT[a[0]][a[3]];
            str[i + 1] = keyT[a[2]][a[1]];
        }
    }

    if (ps > 0 && str[ps - 1] == 'x') {
        str[ps - 1] = '\0';
    }
}

void encryptByPlayfairCipher(char str[], char key[]) {
    char keyT[5][5];
    int ks, ps;

    ks = strlen(key);
    ks = removeSpaces(key, ks);
    toLowerCase(key, ks);

    ps = strlen(str);
    toLowerCase(str, ps);
    ps = removeSpaces(str, ps);
    ps = prepare(str, &ps); // Prepare plaintext

    generateKeyTable(key, ks, keyT);
    encrypt(str, keyT, ps);
}

void decryptByPlayfairCipher(char str[], char key[]) {
    char keyT[5][5];
    int ks, ps;

    ks = strlen(key);
    ks = removeSpaces(key, ks);
    toLowerCase(key, ks);

    ps = strlen(str);
    toLowerCase(str, ps);
    ps = removeSpaces(str, ps);

    generateKeyTable(key, ks, keyT);
    decrypt(str, keyT, ps);
}

int main() {
    char str[SIZE], key[SIZE];

    printf("Enter the key: ");
    fgets(key, SIZE, stdin);
    key[strcspn(key, "\n")] = '\0'; // Remove trailing newline

    printf("Enter the plaintext: ");
    fgets(str, SIZE, stdin);
    str[strcspn(str, "\n")] = '\0'; // Remove trailing newline

    encryptByPlayfairCipher(str, key);
    printf("Cipher text: %s\n", str);

    decryptByPlayfairCipher(str, key);
    printf("Decrypted text: %s\n", str);

    return 0;
}
```
## OUTPUT:
![Screenshot 2024-09-01 202136](https://github.com/user-attachments/assets/ba48f5c9-9c4e-48f5-bf84-50ef1dd5d9f8)

## RESULT:
The program is executed successfully
