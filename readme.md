# TypeDef  jQuery Issue Illustration

With the changes to the TypeScript compiler and the way that the jQuery type definition file has changed its dependencies, there are issues when you try to compile when the @types/jquery library is installed.

This project illustrates that issue and how to resolve it. Before we start, the project needs to be initialized.

```BASH
$ npm i
$ bower i
$ npm install -g typescript@2.0.0
```
Installing the 2.0.0 version of typescript will illustrate that there are massive issues in the index.d.ts of jquery.

```BASH
$ tsc

node_modules/@types/angular/index.d.ts(668,64): error TS1110: Type expected.
... about 400 errors later
node_modules/@types/jquery/index.d.ts(4455,1): error TS1128: Declaration or statement expected.
```

Now thats the first issue. The next step is to experience what it is like to have the latest (2.3.x). This latest version has all the needed items at its disposal, but alas, there is still one step.

```BASH
$ npm install -g typescript@2.3.4
... installed
$ tsc
node_modules/@types/jquery/index.d.ts(2370,84): error TS2304: Cannot find name 'Iterable'.
```

That error is not due to any issue in Typescript. The compiler is working just fine. However, the default settings for TS@2.3.X does not point to the proper libraries that are needed by the jQuery library.

> Excerpt from the [Typescript configuration documentation](https://www.typescriptlang.org/docs/handbook/compiler-options.html)
>
> Note: If `--lib` is not specified a default library is injected. The default library injected is: 
>
> ► For **--target ES5**: `DOM,ES5,ScriptHost`
>
> ► For **--target ES6**: `DOM,ES6,DOM.Iterable,ScriptHost`

Typescript targets ES3 by default, and the tsconfig.json file is initialized with ES5 by default. This is where the issues stem from. There are a few solutions to this.

## Why not just `--target ES6`?

[View Branch](/itanex/typedef-jquery-illustration/tree/es6)

```json
{
    ...
    "compilerOptions": {
        ...
        "target": "es6",
        ...
    }
    ...
}
```

> [ES5 Browser Coverage](http://caniuse.com/#search=es5)
>
> [ES6 Browser Coverage](http://caniuse.com/#search=es6)

## Bring in the libraries you need?

[View Branch](/itanex/typedef-jquery-illustration/tree/es5)

```json
{
    ...
    "compilerOptions": {
        ...
        "target": "es5",
        "lib": ["es5", "dom", "es2015.iterable"],
        ...
    }
    ...
}
```
### Configuration files override they do not extend.

There is something to realize about this path also, unlike compatibility issues, we now have to be explicit in all the libraries we need for our code to compile, this is actually a good thing as we need to be aware of what we are doing. Coding blindly and throwing code over the wall never has served anyone much benefit.

