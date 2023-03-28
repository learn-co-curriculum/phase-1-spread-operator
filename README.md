# The Spread Operator

## Learning Goals

- Use the spread operator to copy arrays
- Use the spread operator to nondestructively modify arrays

## Introduction

So far, we've learned how to manipulate arrays directly using bracket syntax, as
well as how to manipulate arrays using built-in JavaScript array methods. In
this lesson, we'll learn about [spread syntax][spread-syntax] — also known as
the spread operator — and use it to access elements in an array. This will
enable us to do things like make a clone of an array, combine arrays, etc.

We'll also learn how to combine the spread operator with other array methods,
giving us a great deal of flexibility and allowing us to access and combine
elements in all sorts of ways.

## Using the the Spread Operator

The spread operator (`...`) allows us to "spread out" the elements in an array.
In other words, it takes the elements out of an array so we can do something
with them:

```js
const myArray = [1, 2, 3];
console.log(...myArray);
// => 1 2 3
```

One thing we can do with the "spread out" elements is create a copy of an array:

```js
const myArrayCopy = [...myArray];
console.log(myArrayCopy);
// => [1, 2, 3]
```

Above, we're using literal syntax to create our array, i.e., typing out the
square brackets and populating them with the "spread out" elements from
`myArray`. The results in a _new_ array that contains the same elements as the
original array.

We could accomplish the same thing using the `new` keyword and passing the
"spread out" elements as an argument:

```js
const myArrayCopy = new Array(...myArray);
console.log(myArrayCopy);
// => [1, 2, 3]
```

However, we generally recommend using the more straightforward literal syntax.

The spread operator also allows us to make a copy of an array and
simultaneously add elements to it:

```js
const newArray = [...myArray, 4];
console.log(newArray);
// => [1, 2, 3, 4]
```

We can also add an element to the beginning of the copy:

```js
const newArray = [0, ...myArray];
console.log(newArray);
// => [0, 1, 2, 3]
```

Or we could do both at the same time:

```js
const newArray = [0, ...myArray, 4, 5, 6];
console.log(newArray);
// => [0, 1, 2, 3, 4, 5, 6]
```

In the last lesson, we learned about `Array.prototype.concat()`, which enables
us to combine two or more arrays. We can accomplish the same thing using the
spread operator:

```js
const sports = ["swimming", "skateboarding", "pickle ball"];
const teamSports = ["synchronized swimming", "soccer", "field hockey"];
const allSports = [...sports, ...teamSports];
// => ["swimming", "skateboarding", "pickle ball", "synchronized swimming", "soccer", "field hockey"]
```

You can include the elements from as many arrays as you like.

## Copying Arrays

Why use the spread operator to copy arrays rather than just using the assignment
operator?

Arrays are an example of what is known in JavaScript as _reference_ types. This
means that when you create an array and assign a label (a variable name) to it,
that variable is pointing (_referring_) to a location in memory, not
specifically to the value of the array. This can be contrasted with _primitive_
types (strings, numbers, etc.), where the variable name points to a specific
value.

So what does that mean exactly? Let's look at an example. We'll create an array
and then make a copy of it using the assignment operator:

```js
const myArray = ["one", "two", "three"];
const myArrayCopy = myArray;
```

If we look at the value of our two arrays, as expected, they are the same:

```js
console.log(myArray);
// => ["one", "two", "three"]
console.log(myArrayCopy);
// => ["one", "two", "three"]
```

But what if we make a change to the original array:

```js
myArray.push("four");
```

If we then look at the value of `myArray`, we'll see that "four" has been added
to the end of the array, as expected:

```js
console.log(myArray);
// => ["one", "two", "three", "four"];
```

But what about the value of `myArrayCopy`? Let's log that as well:

```js
console.log(myArrayCopy);
// => ["one", "two", "three", "four"];
```

Is that what you were expecting to see?

The reason this happened is because `myArray` is a reference pointing to our
array in memory. When we created `myArrayCopy` and set it equal to `myArray`
using the assignment operator, we created a new reference to _the same location
in memory_. Then, when we modified `myArray`, we were changing the array stored
in that location in memory. Since `myArrayCopy` is pointing to the same
location, it reflects the change as well.

<details><summary><b>what would you expect to happen if we modified `myArrayCopy` instead?</b></summary><p>
  <ul>
    <li>Exactly the same thing! Because both variables are pointing to the same location in memory, changing either one changes the other.</li>
  </ul>
</p></details>

If we want to make a copy of an array that contains the same elements but
doesn't point to the same location in memory as the original - i.e., a clone -
we can instead use the spread operator:

```js
const myArray = ["one", "two", "three"];
const clone = [...myArray];

console.log(myArray);
// => ["one", "two", "three"]
console.log(clone);
// => ["one", "two", "three"]
```

Now, if we change `myArray` and check the value of the two arrays:

```js
myArray.push("four");
console.log(myArray);
// => ["one", "two", "three", "four"]
console.log(clone);
// => ["one", "two", "three"]
```

The contents of our `clone` are unchanged!

Note: can use `slice()` (without an argument) as an alternative to clone an
array; it works the same way as the spread operator. Give it a try.

## More Complex Modifications

You may recall from the lesson on Array Methods in the Prep course that you can
use the spread operator in combination with the other array methods to make more
complex modifications. For example, we can combine `slice()` and the spread
operator to create a new array containing specified "slices" of one or more
arrays.

> The advantage of using `slice()` to do this rather than one of the other array
> methods is that it is nondestructive — it will not modify the original array.

### Reviewing `slice()`

To review, the `slice()` method can take 0, 1 or 2 arguments. With no arguments,
as noted above, `slice()` will make a clone of the array it's called on:

```js
const months = [ "Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec" ];
const copyOfMonths = months.slice();
console.log(copyOfMonths);
// => ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
```

With one argument, it will return a subset of elements starting at the index
position specified and continuing to the end of the array:

```js
const months = [ "Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec" ];
const fourthQuarter = months.slice(9);
console.log(fourthQuarter);
// => ['Oct', 'Nov', 'Dec']
```

With two arguments, the subset of elements returned will start at the index
indicated by the first argument and end one element _before_ the index value
indicated by the second argument:

```js
const months = [ "Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec" ];
const springMonths = months.slice(2, 5);
console.log(springMonths);
// => ['Oct', 'Nov', 'Dec']
```

The use of the second argument can be a little confusing, but one way to help
remember is that if you subtract the first number from the second, you get the
number of elements that will be returned.

### Using `slice()` and the spread operator together

Now, say we want to separate out just winter months. We can do that by combining
`slice()` with the spread operator. What we want to do is add the last element
of `months` ("Dec") along with the first two elements ("Jan" and "Feb") into a
new array.

Getting the last element from `months` is easy enough. There are several ways we
can do it:

```js
// Using the index value directly
months.slice(11);

// Getting the index of the last value dynamically
months.slice(months.length - 1);

// Using the -1 shortcut
months.slice(-1);
```

The last version takes advantage of a nice shortcut we can use with `slice()`:
passing `-1` as the argument will select the _last_ element of whatever array
it's called on. If you pass `-2` as the argument, the _last two_ elements will
be sliced, and so on. You could even pass `-months.length` as the argument to
`slice()` and return the entire array! We wouldn't recommend doing it that way —
it would definitely sacrifice readability — but it would work.

Getting the first two elements is also straightforward:

```js
months.slice(0,2);
```

Because each of these calls to `slice()` returns a new array, the last step is
to "spread out" the elements of each into a new array:

```js
const months = [ "Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec" ];
const winterMonths = [...months.slice(-1), ...months.slice(0,2)];
console.log(winterMonths);
// => ['Dec', 'Jan', 'Feb']
```

Note that we didn't save the results of the `slice()`s into variables. We could
have, of course, and we encourage you to do that if it helps you understand the
code and visualize exactly what's going on. In fact, including all the
intermediate steps is a great way to get your code working, especially when
you're learning something new. You can always go back and refactor later to make
your code more efficient.

## Conclusion

As you progress through the program you will discover that the spread operator
is something you will use often. A bit later in this section, you will learn how
to use it with objects as well as arrays. It will also come in very handy once
you get to React in Phase 2 of the program. Make sure you understand how it
works in the examples we've used in this lesson, and make up some examples of
your own to practice, before moving on.

## Resources

- [MDN: Spread syntax][spread-syntax]
- [MDN: Slice()][slice]

[spread-syntax]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax
[slice]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice
