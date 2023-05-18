## #async/await VS #Promises
By Abdul Rasheed
From https://www.linkedin.com/pulse/asyncawait-vs-promises-abdul-rasheed/


# #Async/await VS #Promises in #javascript
1. Both Async/await and Promises are techniques used in JavaScript to handle asynchronous operations. Promises were introduced in ES6 as a cleaner and more readable way of handling asynchronous code. Async/await is a newer syntax that builds upon Promises to make asynchronous code even easier to write and read.
2. Promises are objects that represent the eventual completion or failure of an asynchronous operation and provide a cleaner way to handle asynchronous code by chaining .then() and .catch() methods. The .then() method is used to handle successful responses, while the .catch() method is used to handle errors. Promises can be used to execute multiple asynchronous operations in parallel, and to handle dependencies between these operations.
3. Async/await is a syntax that was introduced in ES8 (2017) and provides a way to write asynchronous code in a synchronous way. It allows developers to write asynchronous code using a more procedural syntax that looks like synchronous code, which can make it easier to read and understand. Async/await is built on top of Promises, and it is a syntax that allows developers to write asynchronous code that looks similar to synchronous code.
4. Async/await provides several benefits over Promises. It simplifies the syntax of handling asynchronous operations by removing the need for chaining .then() and .catch() methods. Async/await also provides better error handling, as errors can be caught using try/catch blocks, which provides a more familiar syntax to developers.
In summary, both Promises and Async/await are used to handle asynchronous code in JavaScript. Promises are a cleaner way to handle asynchronous code that allows for chaining .then() and .catch() methods, while Async/await provides a more procedural syntax that makes asynchronous code look similar to synchronous code, with better error handling capabilities.

# #Async/await
```javascript
async function fetchData(url) {
 try {
  const response = await fetch(url);
  if (response.ok) {
   const data = await response.json();
   return data;
  } else {
   throw new Error('Network response was not ok.');
  }
 } catch (error) {
  console.error(error);
 }
}
const data = await fetchData('https://api.example.com/data');
console.log(data);
```
# #Promises 
```javascript
function fetchData(url) {
 return new Promise((resolve, reject) => {
  fetch(url)
   .then(response => {
    if (response.ok) {
     return response.json();
    } else {
     throw new Error('Network response was not ok.');
    }
   })
   .then(data => {
    resolve(data);
   })
   .catch(error => {
    reject(error);
   });
 });
}

fetchData('https://api.example.com/data')
 .then(data => {
  console.log(data);
 })
 .catch(error => {
  console.error(error);
 });
 ```
