# The Basics
Each and every value in js has a set of behaviors you can observe from running different operations

```ts
// Accessing the property 'toLowerCase'
// on 'message' and then calling it
message.toLowerCase();

//calling message
message()
```

- Two things are happening
	- Calling `toLowerCase()` property on message
	- Calling `message()` itself
- Now assuming we don't know the value of message, we can't say what this code will return, here are the problems
	- Is `message()` callable?
	- Does `message()` have a property `toLowerCase()` on it?
	- if it does, is  `toLowerCase()` even callable?
	- If both of these values are callable, what do they return?
- The answer to these questions are usually things we keep in our heads when we write JavaScript, and we hope we got all the details right?

Let's say `message` was defined in the following way

```ts
const message = 'Hello world!;'
```

- Here, `message.toLowerCase()` will return same string in lowercase
- But, the `message()` call will not work and will return

```ts
TypeError: message is not a function
```

It'd be great if we could avoid mistakes like this

Now here two concepts in this context
- What is a type?
	- A  *type* is the concept of describing which values can be passed and which will crash.
- What is dynamic typing?
	- Running the code and checking what happens

#### Static type-checking

- Most people don't like errors when running their code 
<br>
- If we add just a bit of code, save the file and re-run, and get an error and debug it, that is easy and the problem can be fixed quickly.
- But we might not have tested the feature thoroughly enough, so we might never actually come across the error
- Or if we are lucky enough to see the error, we might end up doing a lot of refactoring and adding a lot of different code to fix the problem for moment.

- If we have a tool that helps us find these bugs before our code runs, it would be so good.
- That's what a static type checker is and does.
- Static type systems describe the shapes and behaviors of what our values will be when we run our programs.
- A type-checker like TypeScript uses that information (shape and behavior) and tells us when things might go off the rails.

```ts
const message = "hello!";

message();
> This expressions is not callable.
> Type 'String' has no call signatures
```

Running this code with TypeScript will give us an error message before we run the JavaScript code in the first place.

#### Non-exception failures

- So far, we have discussed runtime errors - meaning js's runtime telling us when it thinks something is nonsensical.
- ECMAScript specification defines what is an error and not an error in JavaScript
- In ECMAScript specs
	- calling something which isn't callable throws an error
	- but calling a property which doesn't exist returns `undefined`
	
- JavaScript
	
```javascript
const user = {
	name: "Daniel",
	age: 26,
};
user.location; // returns undefined in JavaScript
```

- TypeScript

```ts
const user = {
	name: "Daniel",
	age: 26,
};
user.location;
> Property 'location' does not exist on type '{ name: string; age: number;}'
```

- While this is a trade-off in what you can express, the intent is to catch legitimate bugs in our programs.
- For example: typos,

```ts
const announcement = "Hello World!"

// How quickly can you spot the typos?
announcement.toLocaleLowercase()
announcement.toLocalLowerCase()

// We probably meant to write this...
announcement.toLocaleLowerCase()
```

uncalled functions,

```ts
function flipCoin(){
	// Meant to be Math.random()
	return Math.random < 0.5;
> Operator '<' cannot be applied to types '() => number' and 'number'. 
}
```

or basic logic errors,

```ts
const value  = Math.random() < 0.5 ? "a" : "b";
if (value != "a"){
	// ...
} else if (value === "b"){
> The comparison appears to be unintentional because the types '"a"' and '"b"' have no overlap
	// Oops, unreachable
}
```

#### Types for tooling
- Typescript can catch bugs when we make mistakes but it can also prevent us from making those mistakes in the first place.
- This topic is talking about **autocomplete** or **IntelliSense**
#### `tsc`, the TypeScript compiler
- Command to install `tsc`
```bash
npm tsc -g typescript
```
> This installs the TypeScript compiler `tsc` globally.
> You can use `npx` or similar tools if you'd prefer to run `tsc` from a local `node_modules` package instead

- It teaches to write our first TypeScript program: `hello.ts`:
```ts
# hello.ts
// Greets hello world.
console.log("Hello world!");
```
- Now to command to run it
```bash
tsc hello.ts
```
- What just happened? Nothing showed on the console.
- Well, there were no type errors, so we din't get any output on our console since there was nothing to report.
- But check again - we got some file output instead.
- If we look in our current directory, we'll see a `hello.js` file next to `hello.ts`.
- That's the output from our `hello.ts` file after the `tsc` *compiles* or *transforms* it into a plain JavaScript file.
- And if we check the contents of `hello.js`, we'll see what TypeScript spits out after it processes a `.ts` file:
```js
// Greets hello world.
console.log("Hello world!");
```
- In this case, there was very little for TypeScript to transform, so it looks identical to what we wrote in the `hello.ts` file.
- The compiler tries to emit clean readable code that looks like something a person would write.
- Even for complex JavaScript code, and with all the advanced TypeScript features (like types, interfaces and decorators), converting them to **simple**, **clean** JavaScript can be tricky. Despite these challenges, TypeScript strives to:
	1. Indent consistently: Ensure that the generated code is neatly formatted.
	2. Handle multi-line code gracefully.
	3. Preserve comments.
	4. In summary, TypeScript tries to make the compiled output look as if a person wrote it.
- Let's see code with type-checking error:
```ts
// This is an industrial-grade general-purpose greeter function
function greet(person, date){
	console.log(`Hello ${person}, today is ${date}`);
}
greet("Brenden");
```
- If we run `tsc hello.ts` again, notice that we get an error on the command line.
```bash
	Expected 2 arguments, but got 1
```
- TypeScript is telling us we forgot to pass an argument to the greet function
- Even though we wrote plain JavaScript, type-checking was still able to find problem with our code
#### Emitting with Errors
- One interesting here is, though `tsc` reported an error, it still created a `hello.js` file
- This is because, it prioritizes not blocking the development process rather than stopping the execution when error is found.
- This highlights that TypeScript compiles to JavaScript even if there are type errors, focusing on producing valid JavaScript rather than stopping at every type-checking issue.