[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/-GmRt-j2)
# Lab-1 : Git, Makefile, and String Utilities

* **[Part 0 - Initial Setup](part-0.md)**
* **[Part 1 – Git and GitHub Basics](part-1.md)**
* **[Part 2 – Makefile Basics and String Utilities Project](part-2.md)**
This is my first Git modification.


#include "string_utils.h"

void reverseString(char* s){
 long long n = 0;
  for(long long i = 0; s[i] != '\0'; i++){
    n++;
  }
  for(long long i = 0; i < n/2; i++){
    long long temp;
    temp = s[i];
    s[i] = s[n-1-i];
    s[n-1-i] = temp;
  }
}

void toUpper(char* s){
  long long n = 0;
  for(long long i = 0; s[i] != '\0'; i++){
    n++;
  }
  for(long long i = 0; i < n; i++){
    if(s[i] >= 'a' && s[i] <= 'z'){
      s[i] -= 32;
    }
  }
}

int* resizeArray(int* arr, size_t oldSize, size_t newSize){
  int* newArr = new int[newSize];
  size_t limit = (oldSize < newSize) ? oldSize : newSize;
  for(size_t i = 0; i < limit; i++){
    newArr[i] = arr[i];
  }
  delete[] arr;
  return newArr;
}


#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <assert.h>
#include "string_utils.h"

void test_reverseString() {
    char s1[20] = "hello";
    reverseString(s1);
    assert(strcmp(s1, "olleh") == 0);

    char s2[20] = "a";
    reverseString(s2);
    assert(strcmp(s2, "a") == 0);

    char s3[20] = "ab";
    reverseString(s3);
    assert(strcmp(s3, "ba") == 0);

    char s4[20] = "racecar";
    reverseString(s4);
    assert(strcmp(s4, "racecar") == 0); // palindrome

    printf("reverseString: ALL TESTS PASSED\n");
}

void test_toUpper() {
    char s1[20] = "hello";
    toUpper(s1);
    assert(strcmp(s1, "HELLO") == 0);

    char s2[20] = "HeLLo";
    toUpper(s2);
    assert(strcmp(s2, "HELLO") == 0);

    char s3[20] = "123abc!";
    toUpper(s3);
    assert(strcmp(s3, "123ABC!") == 0);

    printf("toUpper: ALL TESTS PASSED\n");
}

void test_resizeArray() {
    int *arr = (int *)malloc(3 * sizeof(int));
    arr[0] = 10;
    arr[1] = 20;
    arr[2] = 30;

    int *newArr = resizeArray(arr, 3, 5);

    // Should preserve first 3 elements
    assert(newArr[0] == 10);
    assert(newArr[1] == 20);
    assert(newArr[2] == 30);
    
    // The last two elements are uninitialized — we only check that memory exists.
    // Test shrinking
    int *shrunk = resizeArray(newArr, 5, 2);
    assert(shrunk[0] == 10);
    assert(shrunk[1] == 20);

    free(shrunk);

    printf("resizeArray: ALL TESTS PASSED\n");
}

int main() {
    printf("Running tests...\n\n");

    test_reverseString();
    test_toUpper();
    test_resizeArray();

    printf("\nAll tests completed successfully.\n");
    return 0;
}


CXX = g++
CXXFLAGS = -Wall -Wextra -Iinclude

TARGET = test_lab1
OBJ = build/string_utils.o build/test.o

all: $(TARGET)

build/string_utils.o: src/string_utils.cpp include/string_utils.h
	$(CXX) $(CXXFLAGS) -c src/string_utils.cpp -o build/string_utils.o

build/test.o: tests/test.cpp include/string_utils.h
	$(CXX) $(CXXFLAGS) -c tests/test.cpp -o build/test.o

$(TARGET): $(OBJ)
	$(CXX) $(CXXFLAGS) $(OBJ) -o $(TARGET)

clean:
	rm -rf build/*.o $(TARGET)
run:
	./$(TARGET)
