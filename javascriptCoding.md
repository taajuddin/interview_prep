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
13. [Finding the second largest element in an array](#13-Finding-the-second-largest-element-in-an-array)
14. [Finding the second smallest element in an array](#14-Finding-the-second-smallest-element-in-an-array)
15. [Anagram checking (are two strings anagrams of each other?](#15-Anagram-checking)
16. [Write a function to find the first non-repeating character in a string](#16.Write-a-function-to-find-the-first-non-repeating character-in-a-string)
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

---

## 2. Find All Prime Numbers up to Nth

```js
function isPrime(n){
    if(n<=1) return false;
    if(n<=3) return true;
    if(n%2===0 || n%3===0) return false;
    for(let i=5; i*i<=n; i+=6){
        if(n%i===0 || n%(i+2)===0) return false;
    }
    return true;
}

function findPrimeUpToNth(n){
    if(n<1) return [];
    let primes=[];
    let count=0;
    let num=1;
    while(count<n){
        num++;
        if(isPrime(num)){
            primes.push(num);
            count++;
        }
    }
    return primes.join(',');
}

console.log(findPrimeUpToNth(5)); // "2,3,5,7,11"
```

---

## 3. Merge Arrays of Objects by `id`

```js
const array1=[{id:1, x:5, y:7},{id:2, x:5, y:7},{id:3,x:5, y:7}];
const array2=[{id:3, x:9, y:11},{id:4,x:5, y:7},{id:5,x:5, y:7}];

let mergedArray=[...array1,...array2].reduce((acc, obj)=>{
    let existing=acc.find(item=> item.id===obj.id);
    if(existing){
        existing.x=Math.max(existing.x, obj.x);
        existing.y=Math.max(existing.y, obj.y);
    } else {
        acc.push(obj);
    }
    return acc;
},[]);

console.log(mergedArray);
// [ {id:1,x:5,y:7}, {id:2,x:5,y:7}, {id:3,x:9,y:11}, {id:4,x:5,y:7}, {id:5,x:5,y:7} ]
```

---

## 4. Find the Missing Number (Odd Sequence)

```js
function missingNumber(arr){
    const max=Math.max(...arr);
    const min=Math.min(...arr);
    let result=0;
    for(let i=min;i<=max;i++){
        if(i%2!==0){
            result+=i;
        }
    }
    const sum=arr.reduce((acc,item)=>acc+item,0);
    return result-sum;
}

console.log(missingNumber([1,3,5,7,11])); // 9
```

---

## 5. Find N Largest Numbers

```js
const array=[1,2,3,4,5,7,9,8,6];

function largest(array, n) {
    const sorted=[...array].sort((a,b)=>b-a);
    return sorted.slice(0,n);
}

console.log(largest(array,3)); // [9,8,7]
```

---

## 6. Convert Number to Roman Numerals

```js
function convertToRoman(num){
    const roman={I:1, IV:4, IX:9, XL:40, L:50, XC:90, C:100};
    let result='';
    for(let key in roman){
        while(num>=roman[key]){
            result+=key;
            num-=roman[key];
        }
    }
    return result;
}

console.log(convertToRoman(7)); // VII
```

---

## 7. Find Duplicates in Array

```js
function findDuplicate(arr){
    let result=[];
    for(let i=0;i<arr.length;i++){
        for(let j=i+1;j<arr.length;j++){
            if(arr[i]===arr[j]){
                result.push(arr[j]);
            }
        }
    }
    return [...new Set(result)];
}

console.log(findDuplicate([1,2,3,4,2,5,3])); // [2,3]
```

---

## 8. Remove Duplicates from String

```js
function removeDuplicate(str){
    let result='';
    for(let i=0;i<str.length;i++){
        let char=str[i];
        if(result.indexOf(char)===-1){
            result+=char;
        }
    }
    return result;
}

console.log(removeDuplicate('programming')); // 'progamin'
```

---

## 9. Count Occurrences of Characters

```js
const string='this is me';

function occurenceStr(string){
    const obj={};
    for(let x of string){
        if(x!==' '){
            obj[x]=(obj[x]||0)+1;
        }
    }
    return obj;
}

console.log(occurenceStr(string));
// {t:1, h:1, i:2, s:2, m:1, e:1}
```

---

## 10. Create Hashtag from String

```js
function hashtag(str){
    let result='#';
    let words=str.split(' ');
    for(let i=0;i<words.length;i++){
        result+=words[i].charAt(0).toUpperCase()+words[i].slice(1).toLowerCase();
    }
    return result;
}

console.log(hashtag('javascript is awesome')); // '#JavascriptIsAwesome'
```

---

## 11. Factorial of N

```js
function factorialNumber(n){
    if(n<0) return -1;
    let result=1;
    for(let i=1;i<=n;i++){
        result*=i;
    }
    return result;
}

console.log(factorialNumber(5)); // 120
```

---

## 12. Palindrome Check

```js
function palindrome(str){
    let newStr=str.toLowerCase();
    let reverseStr=newStr.split('').reverse().join('');
    return newStr===reverseStr;
}

console.log(palindrome('madam')); // true
console.log(palindrome('hello')); // false
```

---
---

## 13. Finding the second largest element in an array

```js
let arr = [10, 4, 8, 22, 15];

let first = 0, second = 0;
for (let num of arr) {
  if (num > first) {
    second = first;
    first = num;
  } else if (num > second && num < first) {
    second = num;
  }
}
console.log("Second Largest:", second); // 15
```

---

## 14. Finding the second smallest element in an array

```js
let arr2 = [10, 4, 8, 22, 15];

let first = 0, second = 0;
for (let num of arr2) {
  if (num < first) {
    second = first;
    first = num;
  } else if (num < second && num > first) {
    second = num;
  }
}
console.log("Second Smallest:", second); // 8
```

---

---

## 15. Anagram checking (are two strings anagrams of each other?

```js
function areAnagrams(str1, str2) {
  // Convert to lowercase
  str1 = str1.toLowerCase().split(' ').join('');
  str2 = str2.toLowerCase().split(' ').join('');

  if (str1.length !== str2.length) return false;

  return str1.split('').sort().join('') === str2.split('').sort().join('');
}

console.log(areAnagrams("listen", "silent")); // true
console.log(areAnagrams("hello", "world"));   // false
```

---

## 16. Write a function to find the first non-repeating character in a string
```
Example: Input: "infosys" Output: 'i' (because 'i' is the first character that does not repeat). 
Input: "aabbcc" 
Output: null or a message indicating all characters repeat.
```

```js
function firstNonRepeatingChar(str) {
  let count = {};

  // Count frequency of each character
  for (let char of str) {
    count[char] = (count[char] || 0) + 1;
  }

  // Find the first character with frequency 1
  for (let char of str) {
    if (count[char] === 1) {
      return char;
    }
  }

  return null; // if no unique character
}

// Test cases
console.log(firstNonRepeatingChar("infosys")); // 'i'
console.log(firstNonRepeatingChar("aabbcc"));  // null

```

---


### 🚀 Author
-Md Taaj Uddin

Collection of JavaScript problem-solving examples. Feel free to extend with more challenges!
