# **Glossary** 



## Table of Contents

- [_ambient context_](#_ambient-context_)
- [_`any`_](#_any_)
- [_argument_](#_argument_)
- [_assertion, type assertion_](#_assertion-type-assertion_)
- [_assignable, assignability_](#_assignable-assignability_)
- [_billion-dollar mistake_](#_billion-dollar-mistake_)
- [_bottom type_](#_bottom-type_)
- [_call signature_](#_call-signature_)
- [_camel case_](#_camel-case_)
- [_class_](#_class_)
- [_compile_](#_compile_)
- [_conditional type_](#_conditional-type_)
- [_const assertion_](#_const-assertion_)
- [_constituent, constituent type_](#_constituent-constituent-type_)
- [_declaration file_](#_declaration-file_)
- [_decorator_](#_decorator_)
- [_DefinitelyTyped_](#_definitelytyped_)
- [_derived interface_](#_derived-interface_)
- [_discriminant_](#_discriminant_)
- [_discriminated union, discriminated type union_](#_discriminated-union-discriminated-type-union_)
- [_distributivity_](#_distributivity_)
- [_duck typed_](#_duck-typed_)
- [_dynamically typed, dynamic typing_](#_dynamically-typed-dynamic-typing_)
- [_emit, emitted Code_](#_emit-emitted-code_)
- [_enum_](#_enum_)
- [_evolving_ _`any`_](#_evolving_-_any_)
- [_extending an interface_](#_extending-an-interface_)
- [_function overload, overloaded function_](#_function-overload-overloaded-function_)
- [_generic_](#_generic_)
- [_generic type argument, type argument_](#_generic-type-argument-type-argument_)
- [_generic type parameter, type parameter_](#_generic-type-parameter-type-parameter_)
- [_global variable_](#_global-variable_)
- [_IDE, Integrated Development Environment_](#_ide-integrated-development-environment_)
- [_implementation signature_](#_implementation-signature_)
- [_implicit_ _`any`_](#_implicit_-_any_)
- [_interface_](#_interface_)
- [_interface merging_](#_interface-merging_)
- [_intersection type_](#_intersection-type_)
- [_JSDoc_](#_jsdoc_)
- [_literal_](#_literal_)
- [_mapped types_](#_mapped-types_)
- [_module_](#_module_)
- [_module resolution_](#_module-resolution_)
- [_namespace_](#_namespace_)
- [_`never`_](#_never_)
- [_non-null assertion_](#_non-null-assertion_)
- [_`null`_](#_null_)
- [_optional_](#_optional_)
- [_overload signature_](#_overload-signature_)
- [_override_](#_override_)
- [_parameter_](#_parameter_)
- [_parameter property_](#_parameter-property_)
- [_Pascal case_](#_pascal-case_)
- [_project references_](#_project-references_)
- [_primitive_](#_primitive_)
- [_privacy, private field_](#_privacy-private-field_)
- [_readonly_](#_readonly_)
- [_refactor_](#_refactor_)
- [_return type_](#_return-type_)
- [_Rick Roll_](#_rick-roll_)
- [_script_](#_script_)
- [_strict mode_](#_strict-mode_)
- [_strict null checking_](#_strict-null-checking_)
- [_structurally typed_](#_structurally-typed_)
- [_subclass_](#_subclass_)
- [_target_](#_target_)
- [_Thenable_](#_thenable_)
- [_top type_](#_top-type_)
- [_transpile_](#_transpile_)
- [_TSConfig_](#_tsconfig_)
- [_tuple_](#_tuple_)
- [_type_](#_type_)
- [_type annotation_](#_type-annotation_)
- [_type guard_](#_type-guard_)
- [_type narrowing_](#_type-narrowing_)
- [_type predicate_](#_type-predicate_)
- [_type system_](#_type-system_)
- [_`undefined`_](#_undefined_)
- [_union_](#_union_)
- [_`unknown`_](#_unknown_)
- [_visibility_](#_visibility_)
- [_void_](#_void_)


## _ambient context_ 

An area in code where you can declare type s but cannot declare implementations. Generally used in reference to _.d.ts_ declaration files. See also declaration file. 

## _`any`_ 

A type that is allowed to be used anywhere and can be given anything. `any` can act as a top type, in that any type can be provided to a location of type `any` . Most of the time, you probably want to use `unknown` for more accurate type safety. 

See also `unknown` , top type 

## _argument_ 

Something being provided as an input, used to refer to a value being passed to a function. For functions, an _argument_ is the value being passed to a call, while a _parameter_ is the value inside the function. See also parameter 

## _assertion, type assertion_
An assertion to TypeScript that a value is of a different type than what TypeScript would otherw is e expect. 

## _assignable, assignability_ 

Whether one type is allowed to be used in place of another. 

## _billion-dollar mistake_
The catchy industry term for many type systems allowing values such as `null` to be used in places that require a different type. Coined by Tony Hoare in reference to the amount of damage it seems to have caused. See also strict null checking 

## _bottom type_ 

A type that has no possible values—the empty set of types. No type is assignable to the bottom type. TypeScript provides the `never` keyword to indicate a bottom type. 

See also `never` . 

## _call signature_ 

Type system description of how a function may be called. Includes a list of parameters and a return type. 

## _camel case_ 

A naming convention where the first letter of each compound word after the first in a name is capitalized, like camelCase. The convention for names of members in many TypeScript type system constructs, including members of classes and interfaces. 

## _class_ 

JavaScript syntax sugar around functions that assign to a prototype. TypeScript allows working with JavaScript classes. 

## _compile_ 

Turning source code into another format. TypeScript includes a compiler that, in addition to type checking, turns TypeScript source code into JavaScript and/or declaration files. 

See also transpile 

## _conditional type_ 

A type that resolves to one of two possible types, based on an existing type. 

## _const assertion_
`as const` type assertion shorthand that tells TypeScript to use the most literal, read-only possible form of a value’s type. 

## _constituent, constituent type_
One of the types in an intersection or union type. 

## _declaration file_
A file with the _.d.ts_ extension. Declaration files create an ambient context, meaning they can only declare type s and cannot declare implementations. 

See also ambient context 

## _decorator_
An experimental JavaScript proposal to allow annotating a class or class member with a function marked by a `@` . Doing so would have the function be run on that class or class member upon creation. 

## _DefinitelyTyped_ 

The massive repository of community-authored type definitions for packages (DT for short). It contains thousands of _.d.ts_ definitions along with automation around reviewing change proposals and publ is hing updates. Those definitions are publ is hed as packages under the `@types/` organization on npm, such as `@types/react` . 

## _derived interface_ 

An interface that extends at least one other interface, referred to as a base interface. Doing so copies all the members of the base interface into the derived interface. 

## _discriminant_ 

A member of a discriminated union that has the same name but different type in each constituent. 

## _discriminated union, discriminated type union_
A union of types where a “discriminant” member exists with the same name but different value in each constituent type. Checking the value of the discriminant acts as a form of type narrowing. 

## _distributivity_ 

A property of TypeScript’s conditional types when given union template types: their resultant type will be a union of applying that conditional type to each of the constituents (types in the union type). `ConditionalType<T | U>` is the same as `Conditional<T> | Conditional<U>` . 

## _duck typed_ 

A common phrase for how JavaScript’s type system behaves. It comes from the phrase, “If it looks like a duck and quacks like a duck, it’s probably a duck.” It means that JavaScript allows any value to be passed anywhere; if an object is asked for a member that doesn’t exist, the result will be `undefined` . 

See also structurally typed 

## _dynamically typed, dynamic typing_ 

A classification of programming language that does not natively include a type checker. Examples of dynamically typed programming languages include JavaScript and Ruby. 

## _emit, emitted Code_
The output from a compiler, such as _.js_ files often produced by running `tsc` . The TypeScript compiler’s JavaScript and/or declaration file emits can be controlled by its compiler options. 

## _enum_ 

A set of literal values stored in an object with a friendly name for each value. Enums are a rare example of a TypeScript-specific syntax extension to vanilla JavaScript. 

## _evolving_ _`any`_
A special case of implicit `any` for variables who don’t have a type annotation or initial value. Their type will be evolved to whatever they are used with. 

See also implicit `any` 

## _extending an interface_
When an interface declares that it extends another interface. Doing so copies all members of the original interface into the new one. See also interface 

## _function overload, overloaded function_
A way to describe a function able to be called with drastically different sets of parameters. 

## _generic_ 

Allowing a different type to be substituted for a construct each time a new usage of the construct is created. Classes, interfaces, and type aliases may be made generic. 

## _generic type argument, type argument_
A type provided as the type parameter to a generic construct. 

## _generic type parameter, type parameter_
A substituted type for a generic. Generic type parameters may be provided with different type arguments for each instance of the construct but will remain consistent within that instance. 

## _global variable_
A variable that exists in the global scope, such as `setTimeout` in environments such as browsers, Deno, and Node. 

## _IDE, Integrated Development Environment_
Program that provides developer tooling on top of a text editor for source code. IDEs generally come with debuggers, syntax highlighting, and plugins that surface complaints from programming languages such as type errors. This book uses VS Code for its IDE examples, but others include Atom, Emacs, Vim, V is ual Studio, and WebStorm. 

## _implementation signature_
The final signature declared on an overloaded function, used for its implementation’s parameters. 

See also function overload 

## _implicit_ _`any`_
When TypeScript cannot immediately deduce the typeof a class property, function parameter, or variable, it implicitly assumes the type to be `any` . Implicit `any` types for class properties and function parameters may be configured to be type errors using the `noImplicitAny` compiler option. 

## _interface_
A named set of properties. TypeScript will know a value that’s declared to be of a particular interface’s type will have that interface’s declared properties. 

## _interface merging_ 

A property of interfaces that when multiple interfaces with the same name are declared in the same scope, they combine into one interface instead of causing a type error about conflicting names. This is most commonly used by definition authors to augment global interfaces such as `Window` . 

## _intersection type_ 

A type that uses the `&` operator to indicate it has all the properties of both its constituents. 

## _JSDoc_ 

A standard for `/** ... */` block comments that describe pieces of code such as classes, functions, and variables. Often used in JavaScript projects to roughly describe types. 

## _literal_ 

A value that is known to be a d is tinct instance of a primitive. 

## _mapped types_
A type that takes in another type and performs some operation on each member of that type. In other words, it _maps_ from members of one type into a new set of members. 

## _module_ 

A file with a top-level `export` or `import` . These are generally either files in your source code or files in `node_modules/` packages. See also script. 

## _module resolution_ 

The set of steps used to determine what file a module import resolves to. The TypeScript compiler can have this specified by its `moduleResolution` compiler option. 

## _namespace_ 

An old construct in TypeScript that creates a globally available object with “exported” contents available to call as members of that object. Namespaces are a rare example of a TypeScript-specific syntax extension to vanilla JavaScript. These days, they’re mostly used in _.d.ts_ declaration files. 

## _`never`_ 

The TypeScript type representing the bottom type: a type that can have no possible values. 

See also bottom type. 

## _non-null assertion_
A shorthand `!` that asserts a type is not `null` or `undefined` . 

## _`null`_ 

One of the two primitive types in JavaScript that represents a lack of value. `null` represents an intentional lack of value, while `undefined` represents a more general lack of value. 

See also undefined. 

## _optional_ 

A function parameter, class property, or member of an interface or object type that doesn’t need to be provided. Indicated by placing a `?` after its name, or for function parameters and class properties, alternately indicated by providing a default value with a `=` . 

## _overload signature_ 

One of the signatures declared on an overloaded function to describe a way it may be called. 

See also function overload 

## _override_ 

Redeclaring a property on a subclass-derived interface object that already exists on the base. 

## _parameter_ 

A received input, commonly referring to what a function declares. For functions, an _argument_ is the value being passed to a call, while a _parameter_ is the value inside the function. 

See also argument 

## _parameter property_ 

A TypeScript syntax extension for declaring a property assigned to a member property of the same type at the beginning of a class constructor. 

## _Pascal case_ 

A naming convention where the first letter of each compound word in a name is capitalized, like PascalCase. The convention for names of many TypeScript type system constructs, including generics, interfaces, and type aliases. 

## _project references_ 

A feature of TypeScript configuration files where they can reference other configuration files’ projects as dependencies. This allows you to use TypeScript as a build coordinator to enforce a project dependency tree. 

## _primitive_ 

An immutable data type built into JavaScript that is not an object. They are: `null` , `undefined` , `boolean` , `string` , `number` , `bigint` , and `symbol` . 

## _privacy, private field_ 

A feature of JavaScript where class members whose names begin with `#` can only be accessed inside that same class. 

## _readonly_ 

A TypeScript type system feature where adding the `readonly` keyword in front of a class or object member indicates it can’t be reassigned. 

## _refactor_ 

A change to code that keeps most or all of its behaviors the same. The TypeScript language service is able to perform some refactors on source code when asked, such as moving complex lines of code into a `const` variable. 

## _return type_ 

The type that must be returned by a function. If multiple `return` statements exist in the function with different types, it will be a union of all those possible types. If the function cannot possibly return, it will be `never` . 

## _Rick Roll_
An internet meme where users are tricked into listening to and/or watching a music video of Rick Astley’s seminal classic “Never Gonna Give You Up.” I have hidden several in this book. 

See also _https://oreil.ly/rickroll_ 

## _script_ 

Any source code file that is not a module. 

See also module. 

## _strict mode_
A collection of compiler options that increase the amount of strictness and number of checks the TypeScript type checker performs. This can be enabled for `tsc` with the `--strict` flag and in TSConfiguration files with the `"strict": true compilerOption` . 

## _strict null checking_ 

A strict mode for TypeScript where `null` and `undefined` are no longer allowed to be provided to types that don’t explicitly include them. 

See also billion-dollar mistake 

## _structurally typed_ 

A type system where any value that happens to sat is fy a type is allowed to be used as an instance of that type. 

See also duck typed 

## _subclass_ 

A class that extends another class, referred to as a base class. Doing so copies members of the base class prototype to the child class prototype. 

## _target_ 

The TypeScript compiler option to specify how far back in syntax support JavaScript code needs to be transpiled, such as `"es5` or `"es2017"` . Although `target` defaults to `"es3"` for backward compatibility reasons, it’s adv is able use as new JavaScript syntax as possible per your target platform(s), as supporting newer JavaScript features in older environments necessitates creating more JavaScript code. 

## _Thenable_ 

A JavaScript object with a `.then` method that takes in up to two callback functions and returns another Thenable. Most commonly implemented by the built-in `Promise` class, but user-defined classes and objects can work like a Thenable as well. 

## _top type_ 

A type that can represent any possible type in a system. 

See also `any` , `unknown` 

## _transpile_ 

A term for compilation that turns source code from one human-readable programming language into another. TypeScript includes a compiler that turns _.ts_ / _.tsx_ TypeScript source code into _.js_ files, which is sometimes referred to as transpilation. 

See also compile 

## _TSConfig_ 

A JSON configuration file for TypeScript. Most commonly named _tsconfig.json_ or in the pattern _tsconfig.*.json_ . Editors such as VS Code will read from a _tsconfig.json_ file in a directory to determine TypeScript language service configuration options. 

## _tuple_ 

An array of a fixed size where each element is given an explicit type. For example, `[number, string | undefined]` is a tuple of size two where the first element is type `number` and the second element is type `string | undefined` . 

## _type_ 

An understanding of what members and capabilities a value has. These can be primitives such as `string` , literals such as `123` , or more complex shapes like functions and objects. 

## _type annotation_ 

An annotation after a name used to indicate its type. Cons is ts of `:` and the name of a type. 

## _type guard_ 

A piece of runtime logic that can be understood in the type system to only allow some logic if a value is a particular type. 

## _type narrowing_ 

When TypeScript can deduce a more specific type for a value inside a block of code that is gated on a type guard. 

## _type predicate_ 

A function with a return type annotated to act as a type guard. Type predicate functions return a `boolean` value that indicates whether a value is a type. 

## _type system_ 

The set of rules for how a programming language understands what types the constructs in a program may have. 

## _`undefined`_ 

One of the two primitive types in JavaScript that represents a lack of value. `null` represents an intentional lack of value, while `undefined` represents a more general lack of value. 

See also `null` . 

## _union_ 

A type describing a value that can be two or more possible types. Represented by the `|` pipe between each possible type. 

## _`unknown`_ 

The TypeScript concept representing the top type. `unknown` does not allow arbitrary member access without type narrowing. 

See also `any` , top type 

## _visibility_
Specifying whether a class member is v is ible to code outside the class. Indicated before the member’s declaration with the `public` , `protected` , and `private` keywords. V is ibility and its keywords predate JavaScript’s true `#` member privacy and exist only in the TypeScript type system. See also privacy. 

## _void_
A type indicating the lack of returned value from a function, represented by the `void` keyword in TypeScript. Functions are thought of as returning `void` if they have no `return` statements that return a value. 

