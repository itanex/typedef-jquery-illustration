TypeDef  jQuery Issue Illustration

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

That error is not due to any issue in Typescript. The compiler is working just fine. However, the default settings for TS@2.3.X does not point to the proper libraries that we need. So we need to add them in.