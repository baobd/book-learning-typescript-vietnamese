**Part III. Usage** 

# **Chapter 11. Declaration Files** 



## Table of Contents

- [**Chapter 11. Declaration Files**](#chapter-11-declaration-files)
  - [**Declaration Files**](#declaration-files)
      - [**TIP**](#tip)
  - [**Declaring Runtime Values**](#declaring-runtime-values)
      - [**TIP**](#tip)
    - [**Global Values**](#global-values)
      - [**TIP**](#tip)
    - [**Global Interface Merging**](#global-interface-merging)
    - [**Global Augmentations**](#global-augmentations)
  - [**Built-In Declarations**](#built-in-declarations)
    - [**Library Declarations**](#library-declarations)
      - [**Library targets**](#library-targets)
      - [**TIP**](#tip)
    - [**DOM Declarations**](#dom-declarations)
  - [**Module Declarations**](#module-declarations)
    - [**Wildcard Module Declarations**](#wildcard-module-declarations)
      - [**WARNING**](#warning)
  - [**Package Types**](#package-types)
    - [**declaration**](#declaration)
    - [**Dependency Package Types**](#dependency-package-types)
    - [**Exposing Package Types**](#exposing-package-types)
      - [**NOTE**](#note)
  - [**DefinitelyTyped**](#definitelytyped)
      - [**NOTE**](#note)
      - [**WARNING**](#warning)
    - [**Type Availability**](#type-availability)
      - [**TIP**](#tip)
  - [**Summary**](#summary)
      - [**TIP**](#tip)



_Declaration files_ 

_Have purely type system code_ 

_No runtime constructs_ 

Even though writing code in TypeScript is great and that’s all you want to do, you’ll need to be able to work with raw JavaScript files in your TypeScript projects. Many packages are written directly in JavaScript, not TypeScript. Even packages that are written in TypeScript are distributed as JavaScript files. 

Moreover, TypeScript projects need a way to be told the type shapes of environment-specific features such as global variables and APIs. A project running in, say, Node.js might have access to built-in Node modules not available in browsers—and vice versa. 

TypeScript allows declaring type shapes separately from their implementation. Type declarations are typically written in files whose names end with the _.d.ts_ extension, known as _declaration files_ . Declaration files are generally either written within a project, built and distributed with a project’s compiled npm package, or shared as a standalone “typings” package. 

## **Declaration Files** 

A _.d.ts_ declaration file generally works similarly to a _.ts_ file, except with the notable constraint of not being allowed to include runtime code. _.d.ts_ files contain only descriptions of available runtime values, interfaces, modules, and general types. They cannot contain any runtime code that could be compiled down to JavaScript. 

Declaration files can be imported just like any other source TypeScript file. 

This _types.d.ts_ file exports a `Character` interface used by an _index.ts_ file: 

```typescript
// types.d.ts
export interface Character {
catchphrase?: string;
name: string;
}
// index.ts
import { Character } from "./type s";
export const character: Character= {
catchphrase: "Yee-haw!",
name: "Sandy Cheeks",
};
```

#### **TIP** 

Declaration files create what’s known as an _ambient context_ , meaning an area of code where you can only declare type s, not values. 

This chapter is largely dedicated to declaration files and the most common forms of type declarations used within them. 

## **Declaring Runtime Values** 

Although definition files may not create runtime values such as functions or variables, they are able to declare that those constructs exist with the `declare` keyword. Doing so tells the type system that some external influence—such as a `<script>` tag in a web page—has created the value under that name with a particular type. 

Declaring a variable with `declare` uses the same syntax as a normal variable declaration, except an initial value is not allowed. 

This snippet successfully declares a `declared` variable but receives a type error for trying to give a value to an `initializer` variable: 

```typescript
// types.d.ts
declare let declared: string;  // Ok
declare let initializer: string = "Wanda";
//                                ~~~~~~~
// Error: Initializers are not allowed in ambient contexts.
```

Functions and classes are also declared similarly to their normal forms, but without the bodies of functions or methods. 

The following `canGrantW is h` function and method are properly declared without a body, but the `grantW is h` function and method are syntax errors for improperly attempting to set up a body: 

```typescript
// fairies.d.ts
declare function canGrantW is h(wish: string): boolean;  // Ok

declare function grantW is h(wish: string) { return true; }
//                                       ~
// Error: An implementation cannot be declared in ambient contexts.

class Fairy {
canGrantW is h(wish: string): boolean;  // Ok
grantW is h(wish: string) {
//                  ~
// Error: An implementation cannot be declared in ambient contexts.
return true;
    }
}
```

#### **TIP** 

TypeScript’s implicit `any` rules work the same for functions and variables declared in ambient contexts as they do in normal source code. Because ambient contexts may not provide function bodies or initial variable values, explicit type annotations—including explicit return type annotations—are generally the only way to stop them from implicitly being type `any` . 

Although type declarations using the `declare` keyword are most common in _.d.ts_ definition files, the `declare` keyword can be used outside of declaration files as well. A module or script file can use `declare` as well. 

This can be useful when a globally available variable is only meant to be used in that file. 

Here, a `myGlobalValue` variable is defined in an _index.ts_ file, so it’s allowed to be used in that file: 

```typescript
// index.ts
declare const myGlobalValue: string;

console.log(myGlobalValue);  // Ok
```

Note that while type shapes such as interfaces are allowed with or without a `declare` in _.d.ts_ definition files, runtime constructs such as functions or variables will trigger a type complaint without a `declare` : 

```typescript
// index.d.ts
interface Writer {}  // Ok
declare interface Writer {}  // Ok

declare const fullName: string;  // Ok: type is the primitive string
declare const firstName: "Liz";  // Ok: type is the literal "value"

const lastName = "Lemon";
// Error: Top-level declarations in .d.ts files must
// start with either a 'declare' or 'export' modifier.
```

### **Global Values** 

Because TypeScript files that have no `import` or `export` statements are treated as _scripts_ rather than _modules_ , constructs—including types— declared in them are available globally. Definition files without any imports or exports can take advantage of that behavior to declare type s globally. Global definition files are particularly useful for declaring global types or variables available across all files in an application. 

Here, a _globals.d.ts_ file declares that a `const version: string` exists globally. A _version.ts_ file is then able to refer to a global `version` variable despite not importing from _globals.d.ts_ : 

```typescript
// globals.d.ts
declare const version: string;

// version.ts
export function logVersion() {
console.log(`Version: ${version}`);  // Ok
}
```

Globally declared values are most often used in browser applications that use global variables. Although most modern web frameworks generally use newer techniques such as ECMAScript modules, it can still be useful— especially in smaller projects—to be able to store variables globally. 

#### **TIP** 

If you find that you can’t automatically access global types declared in a _.d.ts_ file, double-check that the _.d.ts_ file isn’t importing and exporting anything. Even a single export will cause the whole file to no longer be available globally! 

### **Global Interface Merging** 

Variables aren’t the only globals floating around in a TypeScript project’s type system. Many type declarations exist globally for global APIs and values. Because interfaces merge with other interfaces of the same name, declaring an interface in a global script context—such as a _.d.ts_ declaration file without any `import` or `export` statements—augments that interface globally. 

For example, a web application that relies on a global variable set by the server might want to declare that as existing on the global `Window` interface. Interface merging would allow a file such as _types/window.d.ts_ to declare a variable that exists on the global `window` variable of type `Window` : 

> `<` **`script`** `type="text/javascript"> window.myVersion = "3.1.1"; </` **`script`** `>` 

```typescript
// types/window.d.ts
interface Window {
myVersion: string;
}
// index.ts
export function logWindowVersion() {
console.log(`Window version is: ${window.myVersion}`);
window.alert("Built-in window type s still work! Hooray!")
}
```

### **Global Augmentations** 

It’s not always feasible to refrain from `import` or `export` statements in a _.d.ts_ file that needs to also augment the global scope, such as when your global definitions are simplified greatly by importing a type defined elsewhere. Sometimes types declared in a module file are meant to be consumed globally. 

For those cases, TypeScript allows a syntax to `declare global` a block of code. Doing so marks the contents of that block as being in a global context even though their surroundings are not: 

```typescript
// types.d.ts
// (module context)
declare global {
// (global context)
}
// (module context)
```

Here, a `types/data.d.ts` file exports a `Data` interface, which will later be imported by both `types/globals.d.ts` and the runtime _index.ts_ : 

```typescript
// types/data.d.ts
export interface Data {
version: string;
}
```

Additionally, `types/globals.d.ts` declares a variable of type `Data` globally inside a `declare global` block as well as a variable available only in that file: 

```typescript
// types/globals.d.ts
import { Data } from "./data";
declare global {
  const globallyDeclared: Data;
}
declare const locallyDeclared: Data;
```

_index.ts_ then has access to the `globallyDeclared` variable without an import, and still needs to import `Data` : 

```typescript
// index.ts
import { Data } from "./type s/data";
function logData(data: Data) {  // Ok
console.log(`Data version is: ${data.version}`);
}
logData(globallyDeclared);  // Ok
logData(locallyDeclared);
//      ~~~~~~~~~~~~~~~
// Error: Cannot find name 'locallyDeclared'.
```

Wrangling global and module declarations to play well together can be tricky. Proper usage of TypeScript’s `declare` and `global` keywords can describe which type definitions are meant to be available globally in projects. 

## **Built-In Declarations** 

Now that you’ve seen how declarations work, it’s time to unveil their hidden use in TypeScript: they’ve been powering its type checking the whole time! Global objects such as `Array` , `Function` , `Map` , and `Set` are examples of constructs that the type system needs to know about but aren’t declared in your code. They’re provided by whatever runtime(s) your code is meant to run in: Deno, Node, a web browser, etc. 

### **Library Declarations** 

Built-in global objects such as `Array` and `Function` that exist in all JavaScript runtimes are declared in files with names like _lib.[target].d.ts_ . _target_ is the minimum support version of JavaScript targeted by your project, such as ES5, ES2020, or ESNext. 

The built-in library definition files, or “lib files,” are fairly large because they represent the entirety of JavaScript’s built-in APIs. For example, members on the built-in `Array` type are represented by a global `Array` interface that starts like this: 

```typescript
// lib.es5.d.ts
interface Array<T> {
/**
     * Gets or sets the length of the array.
     * This is a number one higher than the highest index in the array.
     */
length: number;
// ...
}
```

Lib files are distributed as part of the TypeScript npm package. You can find them inside the package at paths like _node_modules/typescript/lib/lib.es5.d.ts_ . For IDEs such as VS Code that use their own packaged TypeScript versions to type check code, you can find the lib file being used by right-clicking on a built-in method such as an array’s `forEach` in your code and selecting an option like Go to Definition (Figure 11-1). 


![](images/11_Chapter_11_Declaration_Files/11_Chapter_11_Declaration_Files.pdf-0010-00.png)


_Figure 11-1. Left: going to definition on a_ _`forEach` ; right: the resultant opened lib.es5.d.ts file_ 

#### **Library targets** 

TypeScript by default will include the appropriate lib file based on the `target` setting provided to the `tsc` CLI and/or in your project’s _tsconfig.json_ (by default, `"es5"` ). Successive lib files for newer versions of JavaScript build on each other using interface merging. 

For example, static `Number` members such as `EPSILON` and `isFinite` added in ES2015 are listed in _lib.es2015.d.ts_ : 

```typescript
// lib.es2015.d.ts

interface NumberConstructor {
    /**
     * The value of Number.EPSILON is the difference between 1 and the
     * smallest value greater than 1 that is representable as a Number
     * value, which is approximately:
     * 2.2204460492503130808472633361816 x 10 − 16.
     */
    readonly EPSILON: number;

    /**
     * Returns true if passed value is finite.
     * Unlike the global isFinite, Number.isFinite doesn't forcibly
     * convert the parameter to a number. Only finite values of the
     * type number result in true.
     * @param number A numeric value.
     */
    isFinite(number: unknown): boolean;
    // ...
}
```

TypeScript projects will include the lib files for all version targets of JavaScript up through their minimum target. For example, a project with a target of `"es2016"` would include _lib.es5.d.ts_ , _lib.es2015.d.ts_ , and _lib.es2016.d.ts_ . 

#### **TIP** 

Language features available only in newer versions of JavaScript than your target will not be available in the type system. For example, if your target is `"es5"` , language features from ES2015 or later such as `String.prototype.startsWith` will not be recognized. 

Compiler options such as `target` are covered in more detail in Chapter 13, “Configuration Options”. 

### **DOM Declarations** 

Outside of the JavaScript language itself, the most commonly referenced area of type declarations is for web browsers. Web browser types, generally referred to as “DOM” types, cover APIs such as `localStorage` and type shapes such as `HTMLElement` available primarily in web browsers. DOM types are stored in a _lib.dom.d.ts_ file alongside the other _lib.*.d.ts_ declaration files. 

Global DOM types, like many built-in globals, are often described with global interfaces. For example, the `Storage` interface used for `localStorage` and `sessionStorage` and starts roughly like this: 

```typescript
// lib.dom.d.ts

interface Storage {
    /**
     * Returns the number of key/value pairs.
     */
    readonly length: number;
    /**
     * Removes all key/value pairs, if there are any.
     */
    clear(): void;
    /**
     * Returns the current value associated with the given key,
     * or null if the given key does not exist.
     */
    getItem(key: string): string | null;
    // ...
}
```

TypeScript includes DOM types by default in projects that don’t override the `lib` compiler option. That can sometimes be confusing for developers working on projects meant to be run in nonbrowser environments such as Node, as they shouldn’t be able to access the global APIs such as `document` and `localStorage` that the type system would then claim to exist. Compiler options such as `lib` are covered in more detail in Chapter 13, “Configuration Options”. 

## **Module Declarations** 

One more important feature of declaration files is their ability to describe the shapes of modules. The `declare` keyword can be used before a string name of a module to inform the type system of the contents of that module. Here, the `"my-example-lib"` module is declared as being in existence in a `modules.d.ts` declaration script file, then used in an _index.ts_ file: 

```typescript
// modules.d.ts
declare module"my-example-lib" {
export const value: string;
}

// index.ts
import { value } from "my-example-lib";
console.log(value);  // Ok
```

You shouldn’t have to use `declare module` often, if ever, in your own code. It’s mostly used with the following section’s wildcard module declarations and with package types covered later in this chapter. Additionally, see Chapter 13, “Configuration Options” for information on `resolveJsonModule` , a compiler option that allows TypeScript to natively recognize imports from _.json_ files. 

### **Wildcard Module Declarations** 

A common use of module declarations is to tell web applications that a particular non-JavaScript/TypeScript file extension is available to `import` into code. Module declarations may contain a single `*` wildcard to indicate that any module matching that pattern looks the same. 

For example, many web projects such as those preconfigured in popular React starters such as create-react-app and create-next-app support CSS modules to import styles from CSS files as objects that can be used at runtime. They would define modules with a pattern such as `"*.module.css"` that default exports an object of type `{ [i: string]: string }` : 

```typescript
// styles.d.ts
declare module"*.module.css" {
const styles: { [i: string]: string };
export default styles;
}

// component.ts
import stylesfrom"./styles.module.css";

styles.anyClassName;  // Type: string
```

#### **WARNING** 

Using wildcard modules to represent local files isn’t completely type safe. TypeScript does not provide a mechan is m to ensure the imported module path matches a local file. Some projects use a build system such as Webpack and/or generate _.d.ts_ files from local files to make sure imports match up. 

## **Package Types** 

Now that you’ve seen how to declare typings within a project, it’s time to cover consuming types between packages. Projects written in TypeScript still generally distribute packages containing compiled _.js_ outputs. They typically use _.d.ts_ files to declare the backing TypeScript type system shapes behind those JavaScript files. 

### **declaration** 

TypeScript provides a `declaration` option to create _.d.ts_ outputs for input files alongside JavaScript outputs. 

For example, given the following _index.ts_ source file: 

```typescript
// index.ts
export const greet= (text: string) => {
console.log(`Hello, ${text}!`);
};
```

Using `declaration` , a `module` of `"es2015"` , and a `target` of `"es2015"` , the following outputs would be generated: 

```typescript
// index.d.ts
export declare const greet: (text: string) => void;

// index.js
export const greet= (text) => {
console.log(`Hello, ${text}!`);
};
```

Auto-generated _.d.ts_ files are the best way for a project to create type definitions to be used by consumers. It’s generally recommended that most packages written in TypeScript that produce _.js_ file outputs should also bundle _.d.ts_ alongside those files. 

Compiler options such as `declaration` are covered in more detail in Chapter 13, “Configuration Options”. 

### **Dependency Package Types** 

TypeScript is able to detect and utilize _.d.ts_ files bundled inside a project’s `node_modules` dependencies. Those files will inform the type system about the type shapes exported by that package as if they were written inside the same project or declared with a `declare` module block. 

A typical npm module that comes with its own _.d.ts_ declaration files might have a file structure something like: 

```text
lib/
    index.js
    index.d.ts
package.json
```

As an example, the ever-popular test runner Jest is written in TypeScript and provides its own bundled _.d.ts_ files in its `jest` package. It has a dependency on the `@jest/globals` package that provides functions such as `describe` and `it` , which `jest` then makes available globally: 

```json
// package.json
{
"devDependencies":{
"jest":"^32.1.0"
}
}
// using-globals.d.ts
describe("MyAPI", () => {
it("works", () => { /* ... */ });
});

// using-imported.d.ts
import { describe, it } from "@jest/globals";
describe("MyAPI", () => {
it("works", () => { /* ... */ });
});
```

If we were to re-create a very limited subset of the Jest typings packages from scratch, they might look some something like these files. The `@jest/globals` package exports the `describe` and `it` functions. Then, the `jest` package imports those functions and augments the global scope with `describe` and `it` variables of their corresponding function’s type: 

```typescript
// node_modules/@jest/globals/index.d.ts
export function describe(name: string, test: () => void): void;
export function it(name: string, test: () => void): void;

// node_modules/jest/index.d.ts
import *as globalsfrom"@jest/globals";
declare global {
const describe: type ofglobals.describe;
const it: type ofglobals.it;
}
```

This structure allows projects that use Jest to refer to global versions of `describe` and `it` . Projects can alternatively choose to import those functions from the `@jest/globals` package. 

### **Exposing Package Types** 

If your project is meant to be distributed on npm and provide types for consumers, add a `"types"` field in the package’s _package.json_ file to point to the root declaration file. The `types` field works similarly to the `main` field—and often will look the same but with the _.d.ts_ extension instead of _.js_ . 

For example, in this `fictional` package file, the _./lib/index.js_ main runtime file is paralleled by the _./lib/index.d.ts_ types file: 

```json
{
"author":"Pendant Publ is hing",
"main":"./lib/index.js",
"name":"coffeetable",
"type s":"./lib/index.d.ts",
"version":"0.5.22",
}
```

TypeScript would then use the contents of the _./lib/index.d.ts_ as what should be provided for consuming files that import from the `utilitarian` package. 

#### **NOTE** 

If the `types` field does not exist in a package’s _package.json_ , TypeScript will assume a default value of _./index.d.ts_ . This mirrors the default npm behavior of assuming an _./index.js_ file as the `main` entry point for a package if not specified. 

Most packages use TypeScript’s `declaration` compiler option to create _.d.ts_ files alongside _.js_ outputs from source files. Compiler options are covered in Chapter 13, “Configuration Options”. 

## **DefinitelyTyped** 

Sadly, not all projects are written in TypeScript. Some unfortunate developers are still writing their projects in plain old JavaScript without a type checker to aide them. Horrifying. 

Our TypeScript projects still need to be informed of the type shapes of the modules from those packages. The TypeScript team and community created a giant repository called DefinitelyTyped to house community-authored definitions for packages. DefinitelyTyped, or DT for short, is one of the most active repositories on GitHub. It contains thousands of packages of _.d.ts_ definitions, along with automation around reviewing change proposals and publ is hing updates. 

DT packages are publ is hed on npm under the `@types` scope with the same name as the package they provide types for. For example, as of 2022, `@types/react` provides type definitions for the `react` package. 

#### **NOTE** 

`@types` are generally installed as either `dependencies` or `devDependencies` , though the d is tinction between those two has become blurred in recent years. In general, if your project is meant to be distributed as an npm package, it should use `dependencies` so consumers of the package also bring in the type definitions used within. If your project is a standalone application such as one built and run on a server, it should use `devDependencies` to convey that the types are just a development-time tool. 

For example, for a utility package that relies on `lodash` —which as of 2022 has a separate `@types/lodash` package—the _package.json_ would contain lines similar to: 

```json
// package.json
{
"dependencies":{
"@type s/lodash":"^4.14.182",
"lodash":"^4.17.21",
}
}
```

The _package.json_ for a standalone app built on React might contain lines similar to: 

```json
// package.json
{
"dependencies":{
"react":"^18.1.0"
},
"devDependencies":{
"@type s/react":"^18.0.9"
},
}
```

Note that semantic versioning (“semver”) numbers do not necessarily match between `@types/` packages and the packages they represent. You may often find some that are off by a patch version as with React earlier, a minor version as with Lodash earlier, or even major versions. 

#### **WARNING** 

As these files are authored by the community, they may lag behind the parent project or have small inaccuracies. If your project compiles successfully yet you get runtime errors when calling libraries, investigate if the signatures of the APIs you are accessing have changed. This is less common, but still not unheard of, for mature projects with stable API surfaces. 

### **Type Availability** 

Most popular JavaScript packages either ship with their own typings or have typings available via DefinitelyTyped. 

If you’d like to get types for a package that doesn’t yet have types available, your three most common options would be: 

- Send a pull request to DefinitelyTyped to create its `@types/` package. Use the `declare module` syntax introduced earlier to write the types within your project. 

- D is able `noImplicitAny` as covered—and strongly warned against—in Chapter 13, “Configuration Options”. 

I’d recommend contributing types to DefinitelyTyped if you have the time. Doing so helps out other TypeScript developers who may also want to use that package. 

#### **TIP** 

See aka.ms/types to d is play whether a package has types bundled or via a separate `@types/` package. 

## **Summary** 

In this chapter, you used declaration files and value declarations to inform TypeScript about modules and values not declared in your source code: 

- Creating declaration files with _.d.ts_ 

- Declaring types and values with the `declare` keyword 

- Changing global types using global values, global interface merges, and global augmentations 

- Configuring and using TypeScript’s built-in target, library, and DOM declarations 

- Declaring types of modules, including wildcard modules 

- How TypeScript picks up types from packages 

- Using DefinitelyTyped to acquire types for packages that don’t include their own 

#### **TIP** 

Now that you’ve finished reading this chapter, practice what you’ve learned on _https://learningtypescript.com/declaration-files_ . 

_What do TypeScript types say in the American South?_ 

_“Why, I do_ _`declare` !”_ 

