# Understanding the Difference Between Conditional Operators in React: && vs. ?.

## Introduction:

In React, there are moments when we need to make decisions based on specific conditions. We have two handy tools, && (Logical AND Operator)and ?. (Optional Chaining Operator), that assist us in making these choices. In this article, we will look at these operators, how they function, and understand when to use each one.

## Understanding the Basics:

Before diving into the differences, let us get the basics right.

1. `&&` (Logical AND Operator):

   - Checks expressions on both sides and gives you the answer from the last check if they're all true. But if any of them is false, it is going to give you that false one.

   - Developers usually use it when they want to show or hide things based on conditions, or when they're trying to figure out if something is true or not.

     *Mozilla Difinition*: 

     > Logical AND (`&&`) evaluates operands from left to right, returning immediately with the value of the first [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) operand it encounters; if all values are [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy), the value of the last operand is returned.
     >
     > If a value can be converted to `true`, the value is so-called [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy). If a value can be converted to `false`, the value is so-called [falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy).
     >
     > [Logical AND (&&) - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND)

2. `?.` (Optional Chaining Operator):

   - The `?.` operator in JavaScript is kinda new, it showed up in ECMAScript 2020. It checks if something exists when you're not entirely sure.

   - It allows you to access properties or call methods on an object without causing an error if any part of the chain is `null` or `undefined`.

   - Helpful when working with deeply nested objects and properties that may or may not exist.

     *Mozilla Definition*: 

     > The **optional chaining (`?.`)** operator accesses an object's property or calls a function. If the object accessed or function called using this operator is [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) or [`null`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null), the expression short circuits and evaluates to [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) instead of throwing an error.
     >
     > [Optional chaining (?.) - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining)

## The Difference in Usage:

Let us explore the difference in usage between these two operators, with a focus on React scenarios.

### Scenario 1: Checking for the Existence of a Property where Roles is an Array:

Consider a scenario where  you are in need of a boolean that lets you know if someone is an Admin.

```javascript
const roles = const roles = [
  { label: 'Admin', value: 300 },
  { label: 'User', value: 100 },
  { label: 'Manager', value: 200 },
];

const userIsAdmin = roles && roles.some(role => role.label === 'Admin');
```

In this case, the `&&` operator is used to check if the `roles` array exists (`truthy`) before attempting to access its properties. If `roles` is falsy (e.g., an empty array ), the expression returns `false`, indicating that the property does not exist.

```javascript
const roles = const roles = [
  { label: 'Admin', value: 300 },
  { label: 'User', value: 100 },
  { label: 'Manager', value: 200 },
];

const userIsAdmin = roles?.some(role => role.label === 'Admin');
```

Here, the `?.` operator is used for optional chaining. It checks if the `roles` array exists and then proceeds to access the `some` method without throwing an error if `roles` is falsy. This is particularly useful when dealing with nested objects or methods.

### Scenario 2: Roles is NOT the Expected Type:

If, for what ever reason, the type is not what we expected, and we use the `&&` operator to check on an empty string while using the `some` method, the `userIsAdmin` will return an empty string. This can still be used when short circuiting a conditionally rendered component, due to an empty string is considered false. 

```js
const roles = "";

const userIsAdmin = roles && roles.some(role => role.label === 'Admin');
```

On the other hand when we use the `?.` operator, we will get an error thrown of `uncaught TypeError: roles?.some is not a function`. The reason is that since roles is a string and not an array, we do not have access to the `some` method.

```js
const roles = "";

let userIsAdmin = roles?.some(role => role.label === 'Admin');

// uncaught TypeError: roles?.some is not a function
```

**Always Check the Type, Not Just for Any Type**

Since we are expecting and array, the easy fix is to always base our check on the expected type and not on just any type.

```js
const roles = "";

let userIsAdmin = Array.isArray(roles) && roles.some(role => role.label === 'Admin');
```

Now, `userIsAdmin` will be a boolean. Using `&&`, if any of the checks are false, it will return false. In our case `Array.isArray(roles)` is false and will prevent the next piece of code from running.

### Scenario 3: Conditional Rendering:

Let's say you want to conditionally render a component based on the `userIsAdmin` condition.

```javascript
{userIsAdmin && <AdminComponent />}
```

In this case, the `&&` operator is used to conditionally render the `AdminComponent` only if `userIsAdmin` is `true`.

Alternatively, you have the conditional (ternary) operator `?`. It provides a more explicit way of expressing conditional rendering. However, discussing it in detail is a topic for another article ( I hope ).

**Which One to Use:**

- Use `&&` when you want to check for the existence of a property, set a variable based on a boolean condition, or conditionally render components based on a boolean condition.
- Use `?.` (optional chaining) when you need to safely access properties or methods on nested objects, especially when dealing with potential null or undefined values and you do not necessarily need a boolean value as a result.
- Always check the type that you are expecting.

## Conclusion:

Understanding the differences between the && (Logical AND Operator) and ?. (Optional Chaining Operator) in React is essential for writing clean and error-resistant code. While && (Logical AND Operator) is suitable for basic conditional checks, ?. (Optional Chaining Operator) shines when dealing with deeply nested objects and optional properties. Picking the right operator depends on what you are trying to do in your code. Using them correctly will make your React code stronger and easier to understand. And don't forget, always check the type of data you are working with to avoid unexpected surprises.



