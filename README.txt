#include <stdio.h>
#include <string.h>

// Function to perform a simple XOR operation for encryption/decryption
void simple_des_round(char *input, char *key, char *output, int length) {
	int i;
    for (i = 0; i < length; i++) {
        output[i] = input[i] ^ key[i % strlen(key)];
    }
}

int main() {
    char plaintext[100], key[100], ciphertext[100], decrypted[100];
    int len,i;

    printf("Enter plaintext: ");
    fgets(plaintext, sizeof(plaintext), stdin);
    plaintext[strcspn(plaintext, "\n")] = '\0'; // Remove newline character

    printf("Enter key: ");
    fgets(key, sizeof(key), stdin);
    key[strcspn(key, "\n")] = '\0'; // Remove newline character

    len = strlen(plaintext);

    // First round of XOR encryption
    simple_des_round(plaintext, key, ciphertext, len);

    // Show the encrypted text (in hexadecimal format)
    printf("Encrypted text: ");
    for (i = 0; i < len; i++) {
        printf("%02X ", (unsigned char)ciphertext[i]);
    }
    printf("\n");

    // Decrypting by using the same XOR operation (no swap)
    simple_des_round(ciphertext, key, decrypted, len);

    // Print the raw byte values of decrypted text
    printf("Decrypted raw bytes: ");
    for (i = 0; i < len; i++) {
        printf("%02X ", (unsigned char)decrypted[i]);  // Display as hex to avoid character issues
    }
    printf("\n");

    // Null-terminate the decrypted text for safety
    decrypted[len] = '\0';  // Ensure the decrypted string is null-terminated

    // Print decrypted text as characters (if valid ASCII)
    printf("Decrypted text: ");
    for (i = 0; i < len; i++) {
        if (decrypted[i] >= 32 && decrypted[i] <= 126) {  // Print only printable ASCII
            printf("%c", decrypted[i]);
        } else {
            printf(".");  // Non-printable characters will be displayed as a dot
        }
    }
    printf("\n");

    return 0;
}
