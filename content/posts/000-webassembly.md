---
title: 'Webassembly'
description: "No, it's not assembly, it's webassembly, a way to handle native functions though web in a performatic way"
publishDate: "22 Mar 2023"
updatedDate: "22 Mar 2023"
tags: ["webassembly", "performance"]
---

# WebAssembly: How and Why?

## NO!!
Before all, webassembly it's not just assembly as most of you can think in a first read, no one will suffer writing in the most low level programming language ever made, the webassembly, it's nothing else than a bytecode that allows to use languages beyond JS in the browser, allowing a best performance and bringing a bunch of changes in the way that we have been working with web so far.

## Background
As much of you know, the web today is full Javascript, it doesn't matter if you uses Typescript, or if you use any other language, at the end of all, everything is transpiled into JS, because that's the language that has access to DOM, Storage, WebSockets, between others browser API's, and it has been that way for a while, however, the Javascript even being a beaultiful language that evolved a lot though the years, it still has some limitations such as:

- Single thread, what means, it's not good for multitask
- Being based in text require more data being load what results in a bigger initialization time for the page
- JIT optimization for interpretation requires a lot from CPU and battery in devices.
- Need to require existent libraries that are not written in JS

## And now, who can defend us?
The WebAssembly, which we going to call just as WASM right now, is a bytecode interpreted by browser, that alows the use of native APIs, and processing as it was an application instaled in users device, and allowing use of languages that are not JS to work with web.

Some of the strongest features that WASM has are:

- It's a bytecode, what means, ANYONE write a bytecode, but uses it with some another language to compile in WASM
- It's a bin format, what makes it lighter to be load by browser
- It's interpreted directly by machine, without the need to be optimized by JIT, as it's already optimized while compiling time
> Disclaimer: A bytecode is an intermediate between the source code and the final aplication, where it's interpreted by a Virtual Machine, and there's no need for pre processing or syntax validation or data as compilers do, increasing it's performance during executino and enabling portability between different archtectures.

## How do we use that?
Firstly, to create the bytedoce we need to decide which programming language we gonna use, as wasm is compiled, we can use any low level programming language, as C, C++, Rust, among others, but, as web developers are so used to use JS, it makes way more senses exists a language similar to JS but for WASM, and thinking of that, the AssemblyScript was born, and this is language we gonna use for this example.

## The AssemblyScript
The AssemblyScript (AS) is nothing more than a language based on Typescript, whi in turn is build on top of JS, but with a more low level type system, i.e., intead to worry if it's a number or string as TS does, in AS, we need to worry about the amount of memory, if a number is an integer or float, all questions related to machine itself.

PS: a very important thing to keep in mind with AS is that the most common JS data structure cannot be used, instead of a JSON, we need to use a MAP, and inform how much memory it will have, another caracteristics about the language can be seen in: https://www.assemblyscript.org/

## Example
For this example, we gonna make a simple factorial function wriiten in AS, and one in JS for comparison.

```js title=main.ts
export function fact(n: i32): i32 {
  return n == 0 ? 1 : n * fact(n - 1);
}
```

Save the code as main.ts, and to compile as was, we install assembluscript globally with npm

```bash
npm i -g assemblyscript
```

And after that, we run the following command to make the compilation itself

```bash
asc -b main.wasm main.ts
```

With that, we have in our project a file main.wasm

Now, we create an index.html and a script.js so we can use this bytecode.

We put in the html just enough elements to show the calculated values

```html title=index.html
<body>
      JS factorial:
      <span class="js"></span>
      <br />
      WASM factorial:
      <span class="wasm"></span>
</body>
```
In script.js we implement the factorial same way we did in assemblyscript but withou types, and also as we're here we import the WASM

```js title=index.js
async function loadWasmFunctions() {
  const response = await fetch("main.wasm");
  const buffer = await response.arrayBuffer();
  const { instance } = await WebAssembly.instantiate(buffer);
  return instance.exports;
}
function factJs(n) {
  return n == 0 ? 1 : n * factJs(n - 1);
}
const { fact } = await loadWasmFunctions();
document.querySelector(".js").innerText = factJs(12);
document.querySelector(".wasm").innerText = fact(12);
```

When running the file we can note that we have the same values in both, what means indeed it worked, our bytecode is being imported and working in browser, but now you may be asking yourself,
![Wasm sample](/wasm/wasm-sample.png)

> But you're using TS to JS, this is just being transpiled and is going around with no senses, what is the reason for it?

Here is when you're wrong, because wasm is REALLY a bytecode, if we sse the spped that it took to run.
![Wasm sample 2](/wasm/wasm-sample2.png)

We notice that we had a very significant difference when compared to JS, WASM can run at least 10x faster as it's using the whole power available in the machine, proving has all the perforamnce advantage opening new horizons for web development.

## Should i use WASM?
As all new technology, the answer will always be a "It depends", but as it has a focus on performance, WASM is recomended when we need for example to make simulations, or a bunch of calculations, when we want to use native resources, as graphic boards, or audio codecs,  but for e-commerce, blogs, or landing pages, it's not well recommended.

## Who is behind WASM?
One of the best things about WASM, is that ALL the greatest behind all known browsers, is participating for it's develoment, what promises a bright future about how web will be from now.

## Will it be the dead of JS?
Nope, as much advantages WASM have, it still doesn't have access to DOM, e makes more senses works side by side with JS than replace it, bringing all the increase of power for developers having their application more performatic and lighter for the users.

