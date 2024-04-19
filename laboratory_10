// Jessie Shteif
// COP 3502 Lab 10 

#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include <string.h>

#define ALPHABET_SIZE 26
#define MAX_WORD_LENGTH 256
#define MAX_NUM_WORDS 10000

// define trie node struct
struct TrieNode {
    struct TrieNode* children[ALPHABET_SIZE];
    bool isEndOfWord;
    int count;
};

// define Trie struct
struct Trie {
    struct TrieNode* root;
};

// function to create new TrieNode
struct TrieNode* createTrieNode() {
    struct TrieNode* node = (struct TrieNode*)malloc(sizeof(struct TrieNode));
    if (node != NULL) {
        node->isEndOfWord = false;
        node->count = 0;
        for (int i = 0; i < ALPHABET_SIZE; i++) {
            node->children[i] = NULL;
        }
    }
    return node;
}

// function to create a new Trie
struct Trie* createTrie() {
    struct Trie* trie = (struct Trie*)malloc(sizeof(struct Trie));
    if (trie != NULL) {
        trie->root = createTrieNode();
        if (trie->root == NULL) {
            free(trie);
            return NULL;
        }
    }
    return trie;
}

// function to insert a word into the Trie
void insert(struct Trie* trie, const char* word) {
    struct TrieNode* node = trie->root;
    int len = strlen(word);
    for (int i = 0; i < len; i++) {
        int index = word[i] - 'a';
        if (node->children[index] == NULL) {
            node->children[index] = createTrieNode();
            if (node->children[index] == NULL) {
                return; // Memory allocation failed, abort insertion
            }
        }
        node = node->children[index];
    }
    node->isEndOfWord = true;
    node->count++;
}

// function to search for a word in the Trie and return its count
int numberOfOccurrences(struct Trie* trie, const char* word) {
    struct TrieNode* node = trie->root;
    int len = strlen(word);
    for (int i = 0; i < len; i++) {
        int index = word[i] - 'a';
        if (node->children[index] == NULL) {
            return 0;
        }
        node = node->children[index];
    }
    if (node->isEndOfWord) {
        return node->count;
    }
    return 0;
}

// function to deallocate the memory of all nodes in the Trie
void deallocateTrieNode(struct TrieNode* node) {
    if (node != NULL) {
        for (int i = 0; i < ALPHABET_SIZE; i++) {
            deallocateTrieNode(node->children[i]);
        }
        free(node);
    }
}

// function to deallocate the Trie
void deallocateTrie(struct Trie* trie) {
    if (trie != NULL) {
        deallocateTrieNode(trie->root);
        free(trie);
    }
}

// function to read the words in the dictionary from a file and store them in an array of strings
int readDictionary(const char* filename, char** wordList) {
    FILE* fp = fopen(filename, "r");
    if (fp == NULL) {
        fprintf(stderr, "Error: Cannot open file %s\n", filename);
        exit(1);
    }
    int numWords = 0;
    char word[MAX_WORD_LENGTH];
    while (fscanf(fp, "%s", word) != EOF && numWords < MAX_NUM_WORDS) {
        strcpy(wordList[numWords], word);
        numWords++;
    }
    fclose(fp);
    return numWords;
}

int main(void) {
    char* wordList[MAX_NUM_WORDS];
    for (int i = 0; i < MAX_NUM_WORDS; i++) {
        wordList[i] = (char*)malloc(MAX_WORD_LENGTH * sizeof(char));
        if (wordList[i] == NULL) {
            fprintf(stderr, "Error: Memory allocation failed for word list\n");
            exit(1);
        }
    }
    // read the number of words in the dictionary
    int numWords = readDictionary("dictionary.txt", wordList);
    // create a new Trie and insert all the words from the dictionary
    struct Trie* trie = createTrie();
    if (trie == NULL) {
        fprintf(stderr, "Error: Memory allocation failed for Trie\n");
        exit(1);
    }
    for (int i = 0; i < numWords; i++) {
        insert(trie, wordList[i]);
    }
    // test the numberOfOccurrences function with predefined words
    char* words[] = {"notaword", "ucf", "no", "note", "corg"};
    for (int i = 0; i < 5; i++) {
        printf("%s : %d\n", words[i], numberOfOccurrences(trie, words[i]));
    }
    // deallocate the Trie
    deallocateTrie(trie);
    // deallocate the memory of the array of strings
    for (int i = 0; i < numWords; i++) {
        free(wordList[i]);
    }
    return 0;
}
