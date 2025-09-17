### The Big Picture: Building with Functions

Think of functional programming as building complex machines by snapping together smaller, single-purpose tools. These techniques are all about how to create and combine those tools effectively.

---

### 1. Currying: The "One-Ingredient-at-a-Time" Function

**The Core Idea:** Currying is the process of transforming a function that takes multiple arguments into a series of functions that each take only **one argument at a time**.

**Analogy: The Gumball Machine**

*   **A Regular Function:** A vending machine where you have to put in all your money and make a selection at once to get your snack. `buySnack(money, selection)`
*   **A Curried Function:** A gumball machine. You put in one coin (the first argument) and turn the crank. It doesn't give you a gumball yet; instead, it's now "primed" and waiting for the next coin. You put in the next coin (the second argument), turn the crank again, and finally get your gumball.

```javascript
// Regular function
const multiply = (a, b) => a * b;

// Curried version
const curriedMultiply = (a) => (b) => a * b;

// Calling the curried function
curriedMultiply(3)(4); // 12
```

**Why is this useful?**
It allows you to create specialized, reusable functions on the fly.

```javascript
// Create a specialized function by giving only the first argument
const multiplyByFive = curriedMultiply(5);

// Now you have a new function you can reuse!
multiplyByFive(2); // 10
multiplyByFive(10); // 50
```

---

### 2. Partial Application: The "Pre-Filled" Function

**The Core Idea:** Partial Application is similar to currying, but more flexible. It means you "pre-fill" *some* of a function's arguments now, and it returns a new function that is ready to receive the *rest* of the arguments later.

**Analogy: The Custom Pizza Order Form**

Imagine an online pizza order form with fields for `size`, `crust`, and `toppings`.
*   You decide you're always going to order a 'Large' pizza with 'Thin' crust.
*   You "partially apply" these choices by pre-filling those fields.
*   The system gives you back a new, simplified order form that only asks for `toppings`.

```javascript
const multiply = (a, b, c) => a * b * c;

// Create a new function where the first argument 'a' is always 5.
const partialMultiplyByFive = multiply.bind(null, 5);

// Now call the new function with the remaining arguments, 'b' and 'c'.
partialMultiplyByFive(4, 10); // 200 (same as multiply(5, 4, 10))
```
**Currying vs. Partial Application:**
*   **Currying** is strict: one argument at a time.
*   **Partial Application** is flexible: give it as many arguments as you want now, and it will wait for the rest.

---

### 3. Memoization: "Remembering" Past Results

**The Core Idea:** Memoization is a form of caching. It's an optimization technique where you store the results of expensive function calls and return the cached result when the same inputs occur again.

**Analogy: The Math Whiz with a Notepad**

Imagine you ask a friend a difficult math problem, like "What is 583 * 921?"
*   **The first time,** they take a long time to calculate it, write down the answer (536,943) on a notepad next to the problem, and tell you the result.
*   **The second time you ask the exact same question,** they don't recalculate. They just look at their notepad, find the answer instantly, and give it to you.

This is memoization. We use a **cache** (the notepad, which is usually a hash table/object) to store results.

```javascript
function memoizedAddTo80() {
  let cache = {}; // The 'notepad' is private inside the function (closure!)
  return function(n) {
    if (n in cache) {
      console.log('Fetching from cache!');
      return cache[n];
    } else {
      console.log('Calculating...');
      cache[n] = n + 80;
      return cache[n];
    }
  };
}

const memoized = memoizedAddTo80();

memoized(5); // 'Calculating...'
memoized(5); // 'Fetching from cache!'
```
This is extremely useful for speeding up functions that are called repeatedly with the same arguments.

---

### 4. Compose & Pipe: The Function Assembly Line

**The Core Idea:** These are powerful patterns for chaining functions together, where the output of one function becomes the input of the next.

**Analogy: The Car Factory Assembly Line**
A car frame goes down a conveyor belt.
1.  The first machine (`addEngine`) puts an engine in it.
2.  The second machine (`addDoors`) adds doors.
3.  The third machine (`paintCar`) paints it red.

`compose` and `pipe` are functions that build this assembly line for you.

*   **`compose`:** Works from **right to left**. (Like a mathematician would read it).
    ```javascript
    // Read from right to left: first make the number positive, then multiply by 3.
    const composedFunction = compose(multiplyByThree, makePositive);
    composedFunction(-50); // 150
    ```
*   **`pipe`:** Works from **left to right**. (More intuitive for programmers).
    ```javascript
    // Read from left to right: first multiply by three, then make positive.
    const pipedFunction = pipe(multiplyByThree, makePositive);
    pipedFunction(-50); // 150
    ```
This is the heart of functional programming. It allows you to build complex logic by combining simple, pure, reusable functions in a clear and predictable data flow.

### 5. Arity: How Many Arguments?

**The Core Idea:** Arity simply means the **number of arguments a function takes**.
*   A function with arity 1 takes one argument.
*   A function with arity 2 takes two arguments.

**Why it matters in FP:** Functional programmers prefer functions with low arity (especially 1). Why? Because single-argument functions are much easier to **compose** and **curry**. They are the perfect, standardized "tools" to snap together on your assembly line.
