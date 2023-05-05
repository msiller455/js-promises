<img src="https://i.imgur.com/PMyzlb1.png">

# JavaScript Promises

## Learning Objectives

| Students Will Be Able To: |
|---|
| Describe the Use Case for Promises |
| Create a Promise |
| Run code when a Promise resolves |
| Run code when a Promise is rejected |
| Chain Promises |
| Using `async`/`await` for cleaning code |

## Road Map

1. Setup
2. The Use Case of Promises
3. What's a Promise?
4. Making Promises
5. Resolving Promises
6. Rejecting Promises
7. Chaining Promises
8. Using `async`/`await`
9. Further Study

## 1. Setup

We'll be experimenting with promises in [replit.com](https://replit.com/).

Create a new Node-based Repl and name it something like "Promises".

## 2. The Use Case of Promises

**[Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises) provide another way to work with _asynchronous_ operations.**

**â“ Take a minute to answer the following review questions:**

<details>
<summary>
(1) What functions/methods have we used that execute <em>asynchronously</em>?
</summary>
<hr>

<p>Client-Side:</p>
<code>setTimeout</code> & <code>setInterval</code>

<p>Server-Side:</p>
Mongoose Methods such as <code>findById</code>, <code>find</code>, <code>save</code>, etc.

<hr>
</details>

<details>
<summary>
(2) What code "mechanism" have we used that enables code to be run after an asynchronous operation is complete?<br>
Hint: c_______ f________
</summary>
<hr>

callback functions

<hr>
</details>

In JavaScript, the functions/methods that implement asynchronous operations must be crafted to either:
	
- Accept a callback
- Return a promise
- Although rare, an asynchronous function/method can be written to do both.

ðŸ‘‰ Cool, so as an alternative to callback functions, **we use a promise to run code after the completion of an asynchronous operation**.

## 3. What's a Promise?

A [promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) is a special JavaScript object.

An asynchronous function/method designed to work with promises will return a promise instead of accepting a callback function.

For example, very soon, you will be using a library called Mongoose to query for data in a MongoDB database...

Mongoose, prior to version 7, used to work with either promises or callbacks, for example, here's how we might have retrieved data using the callback approach:

```js
Movie.find({}, function(err, movies) {
  console.log(movies);
});
```

However, instead of providing a callback function like above, the `find()` method can return a promise object which ultimately "resolves" to the movies data which we can access using the `.then()` method as follows:

```js
const promiseObject = Movie.find({});
promiseObject.then(function(movies) {console.log(movies)};
```

**ðŸ‘‰ A promise represents the eventual completion, or failure, of the asynchronous operation performed by the function/method that returned the promise.**

Although we more commonly use promises returned by methods like `find()` above, we can better learn about promises in general by creating our own...

## 4. Making Promises

Let's make a promise using the `Promise` class:

```js
const p = new Promise();
```

Running the Repl will generate an error because a function argument must be passed when instantiating the Promise class.

Let's give `new Promise()` a callback function that has two parameters as an argument:

```js
const p = new Promise(function(resolve, reject) {
  console.log(resolve, reject);
});
console.log(p);

// -> [Function (anonymous)] [Function (anonymous)]
// -> PromiseÂ {<pending>}
```

**Observations:**

- The callback function (known as the _executor_) is immediately called by the Promise class's constructor method.
- When the callback is called, it will be passed two functions as args for the `resolve` and `reject` parameters.
- The created promise object has an initial state of `<pending>`.

A promise is always in one of three states:

- `pending`: Initial state, neither fulfilled nor rejected.
- `fulfilled`: The async operation completed successfully. This state is also commonly referred to as "resolved". 
- `rejected`: The async operation failed.

Once a promise has been settled, i.e., it's no longer _pending_, its state will never change again.

## 5. Resolving Promises

So, how does a promise become `fulfilled`?

By calling the `resolve` function...

```js
const p = new Promise(function(resolve, reject) {
  let value = 42;
  resolve(value);
});
```

The promise, `p`, has been _resolved_ with the value `42` and is now in the state of `fulfilled`.

Note that promises can only be resolved with a single JS expression (value/thing), however a "thing" can an object, etc.

### Obtaining the Resolved Value from a Promise

How do we get the value of a resolved promise?

By calling the promise's `.then()` method...

```js
const p = new Promise(function(resolve, reject) {
  const value = 42;
  resolve(value);
});

p.then(function(result) {
  console.log(result);
});
```

The `.then()` method will execute the provided callback after the promise moves to the `fulfilled` state (is resolved).

If need be, the `.then()` method can be called multiple times and will always return the same resolved value/thing.

### Let's Make the Promise Asynchronous

So far our code is synchronous.

To make it a bit more interesting let's use a `setTimeout()` to resolve it asynchronously:

```js
const p = new Promise(function(resolve, reject) {
  setTimeout(function() {
    resolve('Timed out!');
  }, 2000);
});

p.then(function(result) {
  console.log(result);
});
```

Nice, we've seen how to successfully `resolve` a promise, but what if the asynchronous operation fails...

## 6. Rejecting Promises

If an asynchronous operation fails, it calls the `reject` function instead of `resolve`:

```js
const p = new Promise(function(resolve, reject) {
  setTimeout(function() {
    reject('Something went wrong!');
  }, 2000);
});
```

After 2 seconds, we'll see an `UnhandledPromiseRejection: ...` error.

Reading the error more closely reveals that we need a `.catch()` to handle the promise rejection...  

### Handling Rejected Promises

In Node, the callback functions provided to an asynchronous operation have the familiar "error first signature":

```js
Movie.find({}, function(err, movies) {
  if (err) ... // Async op failed
});
```

However, when a promise-based asynchronous function/method calls `reject`, we handle the error using `.catch()`.

For example:

```js
Movie.find({}).then(function(movies) {
  // Promise was resolved
}).catch(function(err) {
  // // Async op failed thus the promise was rejected
});
```

Let's chain on a `.catch()` method call to handle when `p` is rejected:

```js
p.then(function(result) {
  console.log(result);
}).catch(function(err) {
  console.log(err);
});

// -> Something went wrong!
```

That's better!

In summary...

<img src="https://i.imgur.com/B0nzUpC.png">

### â“ Promises - Review Questions (1 min)

<details>
<summary>
(1) In JS, asynchronous functions/methods are designed to work with callbacks and/or _________.
</summary>
<hr>

promises

<hr>
</details>

<details>
<summary>
(2) What three states can a promise be in?
</summary>
<hr>

<code>pending</code>, <code>fulfilled</code> or <code>rejected</code>

<hr>
</details>

<details>
<summary>
(3) What method do we call on a promise to obtain its resolved value?
</summary>
<hr>

<code>.then()</code>

<hr>
</details>

## 7. Chaining Promises

Do you remember having to nest callback functions?

It can get ugly...

<img src="https://i.imgur.com/Zyq5zZU.png">

The advantage of promises is that they "flatten" the async flow and thus avoid the so-called "pyramid of doom".

By chaining one `.then()` after another we can keep the code "flat" and avoid nested callback functions:

```js
const p = new Promise(function(resolve, reject) {
  setTimeout(function() {
    resolve('hello');
  }, 2000);
});

p.then(function(result) {
  console.log(result);  // -> hello
  return 42;
}).then(function(result) {
  console.log(result);  // -> 42
  return 'Done!'
}).then(function(result) {
  console.log(result);  // -> Done!
});
```

ðŸ‘‰ Note that although we are returning primitive values from the callback functions, the `.then()` method **always** returns a promise that resolves to the value we returned.

Let's see what happens if we return promises instead of primitives...

First we need a cool asynchronous function that returns a promise:

```js
function asyncAdd(a, b, delay) {
  return new Promise(function(resolve) {
    setTimeout(function() {
      resolve(a + b);
    }, delay);
  });
}
```

As you can see, the above function returns a promise that resolves to the result of adding two numbers after a delay (ms).

Here's some code that demonstrates promise chaining:

```js
asyncAdd(5, 10, 2000)
  .then(function(sum) {
    console.log(sum);
    return asyncAdd(sum, 100, 1000);
  })
  .then(function(sum) {
    console.log(sum);
    return asyncAdd(sum, 1000, 2000);
  })
  .then(function(sum) {
    console.log(sum);
  });
```

Note that when the `.then()` callback returns a promise, the next `.then()` is called when _that_ promise resolves.

Nice, we've made our own promises, resolved them, and chained them!

More commonly though, we'll be using/consuming promises returned by libraries such as Mongoose in the near future.

## 8. Using `async`/`await`

JavaScript became more awesome when Promises were added to the language in ES2015.

However, it was made super awesome when [async](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) and [await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await) were added with ES2017.

`async`/`await` provide a way to consume promises that make it seem like we're working with synchronous instead of asynchronous code.

### Express/Mongoose Example - With & Without `async`/`await`

As an example, let's again preview how you'll be using Mongoose to retrieve data in a controller function...

First, using `.then()` instead of `async`/`await`:

```js
// controllers/movies.js

function index(req, res) {
  Movie.find({}).then(function(movies) {
    res.render('movies/index', { movies });
  });
}
```

Now, let's take advantage of `async`/`await`:

```js
// controllers/movies.js

// To use await within a function, we declare it as an async function
async function index(req, res) {
  // await the find() method's promise to resolve
  const movies = await Movie.find({});
  res.render('movies/index', { movies });
}
```

As you can see above:
- The `await` keyword basically pauses the function's execution until the provided promise is fulfilled
- The resolved value is returned, allowing us to assign the resolved data to a variable

### Convert our `asyncAdd` Code to Use `async`/`await`

Instead of chaining the `.then()` methods, here's how we can use `async`/`await` instead:

```js
function asyncAdd(a, b, delay) {
  return new Promise(function(resolve) {
    setTimeout(function() {
      resolve(a + b);
    }, delay);
  });
}

// asyncAdd(5, 10, 2000)
//   .then(function(sum) {
//     console.log(sum);
//     return asyncAdd(sum, 100, 1000);
//   })
//   .then(function(sum) {
//     console.log(sum);
//     return asyncAdd(sum, 1000, 2000);
//   })
//   .then(function(sum) {
//     console.log(sum);
//   });

// Wrap the code in an IIFE (Immediately Invoked Function Expression)
// so that we can use await
(async function() {
  let sum = await asyncAdd(5, 10, 2000);
  console.log(sum);
  sum = await asyncAdd(sum, 100, 1000);
  console.log(sum);
  sum = await asyncAdd(sum, 1000, 2000);
  console.log(sum);
})();
```

Although in Replit.com we have to use an IIFE so that `await` is used within an `async` function, it's now possible to use [top-level await](https://v8.dev/features/top-level-await)!

The simple examples above do not fully illustrate the convenience provided by `async`/`await` - they truly are a game changer!

## 9. Seeding a Database

**Seeding** a database is the process of populating a database with some initial data.

Use cases for seeding a database include:

- Creating an initial _admin_ user
- To provide data for lookup tables/collections. For example, in a inventory app for a grocery store, you might seed a _departments_ table/collection with values like `Deli`, `Dairy`, `Bakery`, `Meat & Seafood`, etc.

Note the the process of seeding a database is not part of the application that uses the database and is executed separately.

In your **Mongoose Movies** project, create a `seed.js` file with the following command:
```
touch seed.js
```

This file will be at the top level of your project (the same level as your `server.js` and `package.json` files)

At the top of **seed.js**, let's connect to the database, require the Models and load the `data` module:

```js
// seed.js (a utility to seed/initialize the database)

require('dotenv').config();
require('./config/database');

const Movie = require('./models/movie');
const Performer = require('./models/performer');

// For better organization, the seed data is being stored in a separate data.js module
const data = require('./data');
```

<details>
  <summary>
  Touch that <strong>data.js</strong> file and copy the content from the data.js that's in this dropdown
  </summary>

```js
exports.performers = [
  {name: 'Natalie Portman', born: '06-09-1981'},
  {name: 'Kevin Bacon', born: '07-08-1958'},
  {name: 'Tom Cruise', born: '07-03-1962'},
  {name: 'Brad Pitt', born: '12-18-1963'},
  {name: 'Emma Watson', born: '04-15-1990'},
  {name: 'Carrie Fisher', born: '10-21-1956'},
  {name: 'Mark Hamill', born: '09-25-1951'},
  {name: 'Harrison Ford', born: '07-13-1942'},
  {name: 'Jodie Foster', born: '11-19-1962'},
  {name: 'Matthew McConaughey', born: '11-04-1969'},
  {name: 'James Woods', born: '04-18-1947'},
  {name: 'Anne Hathaway', born: '11-12-1982'},
  {name: 'Bill Murray', born: '09-21-1950'},
  {name: 'Chevy Chase', born: '10-08-1943'},
  {name: 'Rami Malek', born: '05-12-1981'}
];

exports.movies = [
  {title: 'Contact', releaseYear: 1997, mpaaRating: 'PG', nowShowing: false},
  {title: 'Star Wars - A New Hope', releaseYear: 1977, mpaaRating: 'PG', nowShowing: false,
    reviews: [{content: 'The one that started it all!', rating: 5}]
  },
  {title: 'Interstellar', releaseYear: 2014, mpaaRating: 'PG-13', nowShowing: true,
    reviews: [{content: 'A fantastic sci-fi mind blower!', rating: 5}]
  },
  {title: 'Caddyshack', releaseYear: 1980, mpaaRating: 'R', nowShowing: false,
    reviews: [{content: 'Low-budget senseless comedy', rating: 4}, {content: 'Should not be the classic it is.', rating: 2}]
  },
  {title: 'Bohemian Rhapsody', releaseYear: 2018, mpaaRating: 'PG-13', nowShowing: true}
];
```

</details>

> Just in case, the seed data is also saved in [this gist](https://gist.github.com/jim-clark/2a4ab14283ab23bcadf9db2388c55a95).

To avoid duplicates when seeding a database, we first need to delete all data from the collections we'll be inserting data into.

The following code deletes all movie documents and correctly ends the program:

```js
// Just a query object as an arg, no callback!
Movie.deleteMany({})
  // The cb provided to the .then does not use the error-first signature
  // use .catch instead to deal with errors
  .then(function(results) {
    // results will be whatever the promise
    // returned by the deleteMany method resolves to
    console.log(results);
  })
  .then(function() {
    // process.exit() "cleanly" shuts down a Node program
    process.exit();
  });
```

Be sure to review the comments included with the code above to better understand how to use promises with Mongoose.

Running `node seed` will result in the `results` object being logged out.

### Deleting `movies` & `performers` in Sequence (Serially)

The following code will delete the performers after the movies are deleted and then exit the program:

```js
Movie.deleteMany({})
  .then(function(results) {
    console.log('Deleted movies: ', results);
    // Now let's delete the performers and return its "promise"
    return Performer.deleteMany({});
  })
  .then(function(results) {
    console.log('Deleted performers:', results);
  })
  .then(function() {
    // process.exit() "cleanly" shuts down a Node program
    process.exit();
  });
``` 

The above works fine, but it provides an opportunity to learn about a more performant approach... 

### Performing Asynchronous Operations in Parallel

There's nothing wrong with the code as written - it works.

However, the code first waits for the database to finish deleting the movies before it begins to delete the performers.

Because they are not dependent upon each other, it would be more efficient to perform both operations **simultaneously** and we can use the static [Promise.all()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) method to make it happen!

`Promise.all()` accepts an array of promises and returns a **single** promise in their place:

```js
// Save the promises (or call right in the array if feeling frisky)
const p1 = Movie.deleteMany({});
const p2 = Performer.deleteMany({});
// Promise.all will return a single promise that resolves
// only after all of the array's promises resolve
Promise.all([p1, p2])
  .then(function(results) {
    console.log(results);
  })
  // Yes we can!
  .then(process.exit);
```

The above code now removes documents from the _movies_ & _performers_ collections in **parallel**!

### Let's Seed Some Data!

Finally, let's create some data - beginning with performers:

```js
...

Promise.all([p1, p2])
  .then(function(results) {
    console.log(results);
    return Performer.create(data.performers);
  })
  .then(function(performers) {
    console.log(performers);
  })
  .then(process.exit);
```
	
Try it out!

Now it's your turn...

### 👉 You Do - Seed the Movies (2 mins)

We've just seeded Performers together, now it's your turn...

- Add the code that will create the _movie_ documents.

- `data.movies` contains an array of movie data.

- Verify that it worked by spinning up the server and browsing to `localhost:3000` - looking good!

<details>
<summary>
Solution
</summary>
<hr>

```js
  ...
  .then(function(performers) {
    console.log(performers);
    // This line does the trick!
    return Movie.create(data.movies);
  })
  .then(process.exit);
```

<hr>
</details>

Although assigning performers to a movies using the application is fun, let's take a look at how you might do it in **seed.js**.  

### Associating `movies` & `performers` When Seeding

👀 **Important:** You should never refer to an actual `_id` when seeding!

For example, **don't** write code like:

```js
// Don't do this in a seed 
Movie.findById('5c609ac7641fdd63f6b8b71d')
  .then(...)
```

<details>
<summary>
<strong>❓ Why not?</strong>
</summary>
<hr>

**Because the `_id`s change each time the seed module is executed.**

<hr>
</details> 

Instead, we could query for documents based on properties other than their `_id`.

Let's say we want to assign the performer, **Mark Hamill**, to **Star Wars - A New Hope**...

We'll review as we type this:

```js
// insert the following before the .then(process.exit)
.then(function(movies) {
  return Promise.all([
    Performer.findOne({name: 'Mark Hamill'}),
    Movie.findOne({title: 'Star Wars - A New Hope'})
  ]);
})
.then(function(results) {
  // One day we'll destructure results like this:
  // const [mark, starWars] = results;
  const mark = results[0];
  const starWars = results[1];
  starWars.cast.push(mark._id);
  return starWars.save();
})
// Insert the above code before this next line
.then(process.exit);
```

Check it out in the app - Congrats!

## 10. Further Study

Check out more about `async`/`await` [here](https://javascript.info/async-await).

## References

- [MDN - Using Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
