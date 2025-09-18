# JavaScript Problem-Solving Snippets

This repository contains solutions to common JavaScript coding challenges with example usage.

---

## 📑 Table of Contents
1. [Flatten Array](#1-flatten-array)
2. [Find All Prime Numbers up to Nth](#2-find-all-prime-numbers-up-to-nth)
3. [Merge Arrays of Objects by id](#3-merge-arrays-of-objects-by-id)
4. [Find the Missing Number (Odd Sequence)](#4-find-the-missing-number-odd-sequence)
5. [Find N Largest Numbers](#5-find-n-largest-numbers)
6. [Convert Number to Roman Numerals](#6-convert-number-to-roman-numerals)
7. [Find Duplicates in Array](#7-find-duplicates-in-array)
8. [Remove Duplicates from String](#8-remove-duplicates-from-string)
9. [Count Occurrences of Characters](#9-count-occurrences-of-characters)
10. [Create Hashtag from String](#10-create-hashtag-from-string)
11. [Factorial of N](#11-factorial-of-n)
12. [Palindrome Check](#12-palindrome-check)

---

## 1. Flatten Array
```js
const arr=[1,2,3,[4,5,6],7,8,[10,11],9];

function flattenArray(arr) {
    const flatten = [];
    arr.forEach(item => {
        if (Array.isArray(item)) {
            flatten.push(...flattenArray(item));
        } else {
            flatten.push(item);
        }
    });
    return flatten;
}

console.log(flattenArray(arr)); // [1,2,3,4,5,6,7,8,10,11,9]
```
... (truncated for brevity in this cell)
