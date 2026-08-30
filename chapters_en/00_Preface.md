# **Preface** 



## Table of Contents

- [**Preface**](#preface)
  - [**Who Should Read This Book**](#who-should-read-this-book)
  - [**Why I Wrote This Book**](#why-i-wrote-this-book)
  - [**Navigating This Book**](#navigating-this-book)
    - [**Examples and Projects**](#examples-and-projects)
  - [**Conventions Used in This Book**](#conventions-used-in-this-book)
      - [**TIP**](#tip)
      - [**NOTE**](#note)
      - [**WARNING**](#warning)
  - [**Using Code Examples**](#using-code-examples)
  - [**O’Reilly Online Learning**](#oreilly-online-learning)
      - [**NOTE**](#note)
  - [**How to Contact Us**](#how-to-contact-us)
  - [**Acknowledgments**](#acknowledgments)



My journey to TypeScript was not a direct or quick one. I started off in school primarily writing Java, then C++, and like many new developers ra is ed on statically typed languages, I looked down on JavaScript as “just” the sloppy little scripting language people throw onto websites. 

My first substantial project in the language was a silly remake of the original _Super Mario Bros._ video game in pure HTML5/CSS/JavaScript and, typical of many first projects, was an absolute mess. In the beginning of the project I instinctively d is liked JavaScript’s weird flexibility and lack of guardrails. It was only toward the end that I really began to respect JavaScript’s features and quirks: its flexibility as a language, its ability to mix and match small functions, and its ability to _just work_ in user browsers within seconds of page load. 

By the time I finished that first project, I had fallen in love with JavaScript. Static analysis (tools that analyze your code without running it) such as TypeScript also gave me a queasy gut feeling at first. _JavaScript is so breezy and fluid_ , I thought, _why bog ourselves down with rigid structures and types?_ Were we reverting back to the worlds of Java and C++ that I had left behind? 

Coming back to my old projects, it took me all of 10 minutes of struggling to read through my old, convoluted JavaScript code to understand how messy things could get without static analysis. The act of cleaning that code up showed me all the places I would have benefited from some structure. From that point on, I was hooked onto adding as much static analysis to my projects as I could. 

It’s been nearly a decade since I first tinkered with TypeScript, and I enjoy it as much as ever. The language is still evolving with new features and is more useful than ever in providing _safety_ and _structure_ to JavaScript. 

I hope that by reading _Learning TypeScript_ you can learn to appreciate TypeScript the way I do: not just as a means to find bugs and typos—and certainly not a substantial change to JavaScript code patterns—but as JavaScript _with types_ : a beautiful system for declaring the way our JavaScript should work, and helping us stick to it. 

## **Who Should Read This Book** 

If you have an understanding of writing JavaScript code, can run basic commands in a terminal, and are interested in learning about TypeScript, this book is for you. 

Maybe you’ve heard TypeScript can help you write a lot of JavaScript with fewer bugs _(true!)_ or document your code well for other people to read _(also true!)_ . Maybe you’ve seen TypeScript show up in a lot of job postings, or in a new role you’re starting. 

Whatever your reason, as long as you come in knowing the fundamentals of JavaScript—variables, functions, closures/scope, and classes—this book will take you from no TypeScript knowledge to mastering the fundamentals and most important features of the language. By the end of this book, you will understand: 

- The history and context for why TypeScript is useful on top of “vanilla” JavaScript 

- How a type system models code 

- How a type checker analyzes code 

- How to use development-only type annotations to inform the type system 

- How TypeScript works with IDEs (Integrated Development Environments) to provide code exploration and refactoring tools 

And you will be able to: 

- Articulate the benefits of TypeScript and general character is tics of its type system. 

- Add type annotations where useful in your code. 

- Represent moderately complex types using TypeScript’s built-in inferences and new syntax. 

- Use TypeScript to assist local development in refactoring code. 

## **Why I Wrote This Book** 

TypeScript is a wildly popular language in both industry and open source: 

- GitHub’s 2021 and 2020 State of the Octoverses have it at the platform’s fourth top language, up from seventh in 2019 and 2018 and tenth in 2017. 

- StackOverflow’s 2021 Developer Survey has it at the world’s third most loved language (72.73% of users). 

- The 2020 State of JS Survey shows TypeScript has consistently high satisfaction and usage amounts as both a build tool and variant of JavaScript. 

For frontend developers, TypeScript is well supported in all major UI libraries and frameworks, including Angular, which strongly recommends TypeScript, as well as Gatsby, Next.js, React, Svelte, and Vue. For backend developers, TypeScript generates JavaScript that runs natively in Node.js; Deno, a similar runtime by Node’s creator, emphasizes directly supporting TypeScript files. 

However, despite this plethora of popular project support, I was rather d is appointed by the lack of good introductory content online when I first learned the language. Many of the online documentation sources didn’t do a great job of explaining what a “type system” is or how to use it. They often assumed a great deal of prior knowledge in both JavaScript and strongly typed languages, or were written with only cursory code examples. 

Not seeing an O’Reilly book with a cute animal cover introducing TypeScript years ago was a d is appointment. While other books on TypeScript from publishers including O’Reilly now exist prior to this one, I couldn’t find a book that focuses on the foundations of the language quite the way I wanted: why it works the way it does and how its core features work together. A book that starts with a foundational explanation of the language before adding on features one-by-one. I’m thrilled to be able to make a clear, comprehensive introduction to TypeScript language fundamentals for readers who aren’t already familiar with its principles. 

## **Navigating This Book** 

_Learning TypeScript_ has two purposes: 

- You can read through it once to understand TypeScript as a whole. 

- Later, you can refer back to it as a practical introductory TypeScript language reference. 

This book ramps up from concepts to practical use across three general sections: 

- Part I, “Concepts”: How JavaScript came to be, what TypeScript adds to it, and the foundations of a _type system_ as TypeScript creates it. 

- Part II, “Features”: Fleshing out how the type system interacts with the major parts of JavaScript you’d work with when writing TypeScript code. 

- Part III, “Usage”: Now that you understand the features that make up the TypeScript language, how to use them in real-world situations to improve your code reading and editing experience. 

I’ve thrown in a Part IV, “Extra Credit” section at the end to cover lesserused but still occasionally useful TypeScript features. You won’t need to deeply know them to consider yourself a TypeScript developer. But they’re all useful concepts that will likely come up as you use TypeScript for real-world projects. Once you’ve finished understanding the first three sections, I highly recommend studying up on the extra credit section. 

Each chapter starts with a haiku to get into the spirit of its contents and ends with a pun. The web development community as a whole and TypeScript’s community within it are known for being jovial and welcoming of newcomers. I tried to make this book pleasant to read for learners like me who don’t appreciate long, dry writings. 

### **Examples and Projects** 

Unlike many other resources that introduce TypeScript, this book intentionally focuses on introducing language features with standalone examples showing just the new information rather than delving into medium- or large-sized projects. I prefer this method of teaching because it puts a spotlight on the TypeScript language first and foremost. TypeScript is useful across so many frameworks and platforms—many of which undergo API updates regularly—that I didn’t want to keep anything framework- or platform-specific in this book. 

That being said, it is supremely useful when learning a programming language to exercise concepts immediately after they’re introduced. I highly recommend taking a break after each chapter to rehearse that chapter’s contents. Each chapter ends with a suggestion to visit its section on _https://learningtypescript.com_ and work through the examples and projects listed there. 

## **Conventions Used in This Book** 

The following typographical conventions are used in this book: _Italic_ 

Indicates new terms, URLs, email addresses, filenames, and file extensions. 

```text
Constant width
```

Used for program listings, as well as within paragraphs to refer to program elements such as variable or function names, data types, statements, and keywords. 

#### **TIP** 

This element signifies a tip or suggestion. 

#### **NOTE** 

This element signifies a general note. 

#### **WARNING** 

This element indicates a warning or caution. 

## **Using Code Examples** 

Supplemental material (code examples, exercises, etc.) is available for download at _https://learningtypescript.com_ . 

If you have a technical question or a problem using the code examples, please send email to _bookquestions@oreilly.com_ . 

This book is here to help you get your job done. In general, if example code is offered with this book, you may use it in your programs and documentation. You do not need to contact us for permission unless you’re reproducing a significant portion of the code. For example, writing a program that uses several chunks of code from this book does not require permission. Selling or distributing examples from O’Reilly books does require permission. Answering a question by citing this book and quoting example code does not require permission. Incorporating a significant amount of example code from this book into your product’s documentation does require permission. 

We appreciate, but generally do not require, attribution. An attribution usually includes the title, author, publisher, and ISBN. For example: “ _Learning Typescript_ by Josh Goldberg (O’Reilly). Copyright 2022 Josh Goldberg, 978-1-098-11033-8.” 

If you feel your use of code examples falls outside fair use or the permission given above, feel free to contact us at _permissions@oreilly.com_ . 

## **O’Reilly Online Learning** 

#### **NOTE** 

For more than 40 years, _O’Reilly Media_ has provided technology and business training, knowledge, and insight to help companies succeed. 

Our unique network of experts and innovators share their knowledge and expertise through books, articles, and our online learning platform. O’Reilly’s online learning platform gives you on-demand access to live training courses, in-depth learning paths, interactive coding environments, and a vast collection of text and video from O’Reilly and 200+ other publishers. For more information, visit _http://oreilly.com_ . 

## **How to Contact Us** 

Please address comments and questions concerning this book to the publisher: 

O’Reilly Media, Inc. 

1005 Gravenstein Highway North 

Sebastopol, CA 95472 

800-998-9938 (in the United States or Canada) 

707-829-0515 (international or local) 

707-829-0104 (fax) 

We have a web page for this book, where we list errata, examples, and any additional information. You can access this page at _https://oreil.ly/learningtypescript_ . 

Email _bookquestions@oreilly.com_ to comment or ask technical questions about this book. 

For news and information about our books and courses, visit _https://oreilly.com_ . 

Find us on LinkedIn: _https://linkedin.com/company/oreilly-media_ . Follow us on Twitter: _https://twitter.com/oreillymedia_ . 

Watch us on YouTube: _https://www.youtube.com/oreillymedia_ . 

## **Acknowledgments** 

This book was a team effort, and I’d like to sincerely thank everybody who made it possible. First and foremost my superhuman editor-in-chief, Rita Fernando, for an incredible amount of patience and excellent guidance throughout the authoring journey. Additional shoutout to the rest of the O’Reilly crew: Kr is ten Brown, Suzanne Huston, Clare Jensen, Carol Keller, Elizabeth Kelly, Cheryl Lenser, Elizabeth Oliver, and Amanda Quinn. You all rock! 

Many deep thanks to the tech reviewers for their consistently top-notch pedagogical insights and TypeScript expertise: Mike Boyle, Ryan Cavanaugh, Sara Gallagher, Michael Hoffman, Adam Reineke, and Dan Vanderkam. This book wouldn’t be the same without you, and I hope I successfully captured the intent of all your great suggestions! 

Further thanks to the assorted peers and pra is e quoters who gave spot reviews on the book that helped me improve technical accuracy and writing quality: Robert Blake, Andrew Branch, James Henry, Adam Kaczmarek, Loren Sands-Ramshaw, Nik Stern, and Lenz Weber-Tronic. Every suggestion helps! 

Lastly, I’d like to thank my family for their love and support over the years. My parents, Frances and Mark, and brother, Danny—thanks for letting me spend time with Legos and books and video games. To my spouse Mariah Goldberg for her patience during my long bouts of editing and writing, and our cats Luci, Tiny, and Jerry for distingu is hed fluffiness and keeping me company. 

