# cs125project
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
#include <time.h>

#define MAX_WORD_LENGTH 20
#define MAX_GUESSES 6
#define WORD_LIST_SIZE 10

char *word_list[WORD_LIST_SIZE] = {"apple", "banana", "cherry", "orange", "pineapple", "grape", "strawberry", "kiwi", "mango", "papaya"};

int main() {
    // Seed the random number generator
    srand(time(NULL));

    // Choose a random word from the list
    char *word = word_list[rand() % WORD_LIST_SIZE];

    // Create a blank array to represent the current state of the word
    int word_length = strlen(word);
    char current_word[word_length+1];
    memset(current_word, '_', word_length);
    current_word[word_length] = '\0';

    // Initialize the number of guesses and the letters that have been guessed
    int num_guesses = 0;
    char guessed_letters[MAX_GUESSES];
    memset(guessed_letters, 0, MAX_GUESSES);

    // Print the initial state of the game
    printf("Welcome to Hangman!\n");
    printf("The word has %d letters.\n", word_length);
    printf("You have %d guesses remaining.\n", MAX_GUESSES-num_guesses);
    printf("Word: %s\n", current_word);

    // Main game loop
    while (num_guesses < MAX_GUESSES) {
        // Ask the user to guess a letter
        char guess;
        printf("Guess a letter: ");
        scanf(" %c", &guess);

        // Convert the guess to lowercase
        guess = tolower(guess);

        // Check if the letter has already been guessed
        if (strchr(guessed_letters, guess) != NULL) {
            printf("You already guessed that letter.\n");
            continue;
        }

        // Add the guess to the list of guessed letters
        guessed_letters[num_guesses] = guess;

        // Check if the guess is in the word
        int found = 0;
        for (int i = 0; i < word_length; i++) {
            if (word[i] == guess) {
                current_word[i] = guess;
                found = 1;
            }
        }

        // Print the current state of the game
        printf("Word: %s\n", current_word);

        if (strcmp(current_word, word) == 0) {
            printf("Congratulations, you guessed the word!\n");
            return 0;
        }

        if (!found) {
            num_guesses++;
            printf("Sorry, that letter is not in the word.\n");
            printf("You have %d guesses remaining.\n", MAX_GUESSES-num_guesses);
        }
    }

    // If the player runs out of guesses, they lose
    printf("Sorry, you ran out of guesses. The word was %s.\n", word);
    return 0;
}
