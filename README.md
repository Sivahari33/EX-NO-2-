## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

NAME: SIVAHARIBALAN K

REG NO:212224220103

**AIM:**
To write a C program to implement the Playfair Substitution technique.

**ALGORITHM:**

STEP-1: Read the plain text from the user.

STEP-2: Read the keyword from the user.

STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.

STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.

STEP-5: Display the obtained cipher text.

**PROGRAM:**

#include <stdio.h>

#include <string.h>

#include <ctype.h>

char keyTable[5][5];

// Generate the 5x5 key matrix
void generateKeyTable(char key[])
{
    int used[26] = {0};
    int i, k = 0;

    for (i = 0; key[i] != '\0'; i++)
    {
        if (key[i] == 'J')
            key[i] = 'I';

        if (!used[key[i] - 'A'])
        {
            keyTable[k / 5][k % 5] = key[i];
            used[key[i] - 'A'] = 1;
            k++;
        }
    }

    for (i = 0; i < 26; i++)
    {
        if (i + 'A' == 'J')
            continue;

        if (!used[i])
        {
            keyTable[k / 5][k % 5] = i + 'A';
            k++;
        }
    }
}

// Find row and column of a character
void findPosition(char ch, int *row, int *col)
{
    int i, j;

    if (ch == 'J')
        ch = 'I';

    for (i = 0; i < 5; i++)
        for (j = 0; j < 5; j++)
            if (keyTable[i][j] == ch)
            {
                *row = i;
                *col = j;
                return;
            }
}

// Encrypt plaintext
void encrypt(char text[])
{
    int i, r1, c1, r2, c2;

    printf("Encrypted Text: ");
    for (i = 0; i < strlen(text); i += 2)
    {
        findPosition(text[i], &r1, &c1);
        findPosition(text[i + 1], &r2, &c2);

        if (r1 == r2) // Same row
        {
            printf("%c%c",
                   keyTable[r1][(c1 + 1) % 5],
                   keyTable[r2][(c2 + 1) % 5]);
        }
        else if (c1 == c2) // Same column
        {
            printf("%c%c",
                   keyTable[(r1 + 1) % 5][c1],
                   keyTable[(r2 + 1) % 5][c2]);
        }
        else // Rectangle
        {
            printf("%c%c",
                   keyTable[r1][c2],
                   keyTable[r2][c1]);
        }
    }
}

// Decrypt cipher text
void decrypt(char text[])
{
    int i, r1, c1, r2, c2;

    printf("\nDecrypted Text: ");
    for (i = 0; i < strlen(text); i += 2)
    {
        findPosition(text[i], &r1, &c1);
        findPosition(text[i + 1], &r2, &c2);

        if (r1 == r2) // Same row
        {
            printf("%c%c",
                   keyTable[r1][(c1 + 4) % 5],
                   keyTable[r2][(c2 + 4) % 5]);
        }
        else if (c1 == c2) // Same column
        {
            printf("%c%c",
                   keyTable[(r1 + 4) % 5][c1],
                   keyTable[(r2 + 4) % 5][c2]);
        }
        else // Rectangle
        {
            printf("%c%c",
                   keyTable[r1][c2],
                   keyTable[r2][c1]);
        }
    }
}

int main()
{
    char key[25], plainText[50], cipherText[50];
    int i, len;

    printf("Enter the key: ");
    scanf("%s", key);

    // Convert key to uppercase
    for (i = 0; key[i]; i++)
        key[i] = toupper(key[i]);

    generateKeyTable(key);

    printf("Enter the plaintext: ");
    scanf("%s", plainText);

    // Convert plaintext to uppercase
    for (i = 0; plainText[i]; i++)
        plainText[i] = toupper(plainText[i]);

    // Handle repeated letters and odd length
    len = strlen(plainText);
    for (i = 0; i < len; i += 2)
    {
        if (plainText[i] == plainText[i + 1])
        {
            memmove(&plainText[i + 2], &plainText[i + 1], len - i);
            plainText[i + 1] = 'X';
            len++;
        }
    }
    if (len % 2 != 0)
    {
        plainText[len] = 'X';
        plainText[len + 1] = '\0';
    }

    encrypt(plainText);

    printf("\nEnter the cipher text to decrypt: ");
    scanf("%s", cipherText);

    for (i = 0; cipherText[i]; i++)
        cipherText[i] = toupper(cipherText[i]);

    decrypt(cipherText);

    return 0;
}
**Output:**

 <img width="938" height="734" alt="image" src="https://github.com/user-attachments/assets/cb08b040-e3cd-44f2-bd9a-7b6da58ef429" />


**RESULT:**

         The program is executed successfully


 

