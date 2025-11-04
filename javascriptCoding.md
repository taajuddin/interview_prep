# JavaScript Problem-Solving Snippets

This repository contains solutions to common JavaScript coding challenges with example usage.

---

## 📑 Table of Contents

1. [Flatten Array](#1-flatten-array)
2. [Implement Promise.all without using promise.all method](#2-implement-promiseall-without-using-promiseall-method)
3. [Find All Prime Numbers up to Nth](#3-find-all-prime-numbers-up-to-nth)
4. [Merge Arrays of Objects by id](#4-merge-arrays-of-objects-by-id)
5. [Find the Missing Number (Odd Sequence)](#5-find-the-missing-number-odd-sequence)
6. [Find N Largest Numbers](#6-find-n-largest-numbers)
7. [Convert Number to Roman Numerals](#7-convert-number-to-roman-numerals)
8. [Find Duplicates in Array](#8-find-duplicates-in-array)
9. [Remove Duplicates from String](#9-remove-duplicates-from-string)
10. [Count Occurrences of Characters](#10-count-occurrences-of-characters)
11. [Create Hashtag from String](#11-create-hashtag-from-string)
12. [Factorial of N](#12-factorial-of-n)
13. [Palindrome Check](#13-palindrome-check)
14. [Finding the second largest element in an array](#14-finding-the-second-largest-element-in-an-array)
15. [Finding the second smallest element in an array](#15-finding-the-second-smallest-element-in-an-array)
16. [Anagram checking are two strings anagrams of each other?](#16-anagram-checking-are-two-strings-anagrams-of-each-other)
17. [Write a function to find the first non-repeating character in a string](#17-write-a-function-to-find-the-first-non-repeating-character-in-a-string)
18. [Memoization of fibonacci function](#18fiboancii-with-memozation)
19. [function Currying for infinite sum](#19-currying-function-for-infinite-sum)
20. [remove Duplicate from array of Object](#20-remove-duplicate-from-array-of-object)
21. [Split the data when captial Letter](#21-split-the-data-when-captialletter-with-space-and-first-character-should-be-uppercase)

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

## 2. Implement Promise.all without using promise.all method
```js
function myPromiseAll(promises) {
  return new Promise((resolve, reject) => {
    const results = [];
    let completed = 0;

    if (promises.length === 0) {
      return resolve([]);
    }

    promises.forEach((p, index) => {
      Promise.resolve(p)
        .then(value => {
          results[index] = value;
          completed++;
          if (completed === promises.length) {
            resolve(results);
          }
        })
        .catch(err => reject(err));
    });
  });
}

const p1 = Promise.resolve(10);
const p2 = 20;
const p3 = new Promise(resolve => setTimeout(() => resolve(30), 1000));

myPromiseAll([p1, p2, p3])
  .then(results => console.log("✅ Results:", results))
  .catch(error => console.error("❌ Error:", error));
```
---

## 3. Find All Prime Numbers up to Nth
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

## 4. Merge Arrays of Objects by id
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
```
---

## 5. Find the Missing Number (Odd Sequence)
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

## 6. Find N Largest Numbers
```js
const array=[1,2,3,4,5,7,9,8,6];

function largest(array, n) {
    const sorted=[...array].sort((a,b)=>b-a);
    return sorted.slice(0,n);
}

console.log(largest(array,3)); // [9,8,7]
```
---

## 7. Convert Number to Roman Numerals
```js
function convertToRoman(num) {
  const romanMap = [
    { value: 1000, symbol: "M" },
    { value: 900, symbol: "CM" },
    { value: 500, symbol: "D" },
    { value: 400, symbol: "CD" },
    { value: 100, symbol: "C" },
    { value: 90, symbol: "XC" },
    { value: 50, symbol: "L" },
    { value: 40, symbol: "XL" },
    { value: 10, symbol: "X" },
    { value: 9, symbol: "IX" },
    { value: 5, symbol: "V" },
    { value: 4, symbol: "IV" },
    { value: 1, symbol: "I" }
  ];
  let result = "";
  for (const { value, symbol } of romanMap) {
    while (num >= value) {
      result += symbol;
      num -= value;
    }
  }
  return result;
}

console.log(convertToRoman(7));   // VII
console.log(convertToRoman(49));  // XLIX
console.log(convertToRoman(2024)); // MMXXIV
```
---

## 8. Find Duplicates in Array
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

## 9. Remove Duplicates from String
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

## 10. Count Occurrences of Characters
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

## 11. Create Hashtag from String
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

## 12. Factorial of N
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

## 13. Palindrome Check
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

## 14. Finding the second largest element in an array
```js
function findLarget(a){
    let first=0, second=0;
    for(let i=0; i<a.length; i++){
        if(a[i] > first){
            second= first
            first= a[i]
        }else if(a[i] > second && a[i] < first){
            second= a[i]
        }
    }
    return second
}
console.log(findLarget([54,23,76,89]))
```
---

## 15. Finding the second smallest element in an array
```js
function findSecondSmallest(a) {
  let first = Infinity, second = Infinity;
  for (let i = 0; i < a.length; i++) {
    if (a[i] < first) {
      second = first;
      first = a[i];
    } else if (a[i] < second && a[i] !== first) {
      second = a[i];
    }
  }
  return second;
}
console.log(findSecondSmallest([54, 23, 76, 89])); // 54
```
---

## 16. Anagram checking are two strings anagrams of each other?
```js
function areAnagrams(str1, str2) {
  str1 = str1.toLowerCase().split(' ').join('');
  str2 = str2.toLowerCase().split(' ').join('');
  if (str1.length !== str2.length) return false;
  return str1.split('').sort().join('') === str2.split('').sort().join('');
}
console.log(areAnagrams("listen", "silent")); // true
console.log(areAnagrams("hello", "world"));   // false
```
---

## 17. Write a function to find the first non-repeating character in a string
```js
function firstNonRepeatingChar(str) {
  let count = {};
  for (let char of str) {
    count[char] = (count[char] || 0) + 1;
  }
  for (let char of str) {
    if (count[char] === 1) {
      return char;
    }
  }
  return null;
}
console.log(firstNonRepeatingChar("infosys")); // 'i'
console.log(firstNonRepeatingChar("aabbcc"));  // null
```
---

## 18fiboancii with memozation
```js
function memoizedFib() {
  const cache = {};
  function fib(n) {
    if (n <= 1) return n;
    if (cache[n]) return cache[n];
    cache[n] = fib(n - 1) + fib(n - 2);
    return cache[n];
  }
  return fib;
}
const fib = memoizedFib();
console.log(fib(10)); 
console.log(fib(10));
```
---

## 19. Currying Function for Infinite Sum
```js
function sum(x) {
  let acc = x ?? 0;
  function inner(y) {
    if (y === undefined) return acc;
    acc += y; 
    return inner;
  }
  return inner;
}
console.log(sum(10)(20)(30)()); // 60
```
---

## 20. Remove duplicate from array of object
```js
const data=[{a:'1',b:'2'},{c:'3',d:'4'},{a:'1',b:'2'}]

function removeDupObj(data){
    let cache={}
    const newData=[]
    for(let i=0;i<data.length;i++){
        const obj=data[i]
        const cacheKey=`${JSON.stringify(obj)}`
        if(!cache[cacheKey]) {
            newData.push(obj)
            cache[cacheKey]=obj
        }
    }
    return newData
}
console.log(removeDupObj(data))
```
---

## 21. Split the data when captialLetter with space and first character should be upperCase
```js
const data = ["timBrook", "alexCary", "ericWalker"]
const newValue = data.map(item => {
  const formatted =item.charAt(0).toUpperCase() +item.slice(1).replace(/([A-Z])/g, ' $1');
  return formatted.trim();
});
console.log(newValue); // ["Tim Brook", "Alex Cary", "Eric Walker"]
```
---

### 🚀 Author
**Md Taaj Uddin**  
Collection of JavaScript problem-solving examples.  
Feel free to extend with more challenges!
