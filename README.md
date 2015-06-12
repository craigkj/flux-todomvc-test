# Flux TodoMVC Example

> An application architecture for React utilizing a unidirectional data flow.

**NOTE**: This is a copy of the flux repo here:
https://github.com/facebook/flux/tree/master/examples/flux-todomvc/ which i copied to demonstrate how you can configure jest to run tests from a separate directory.

Running the tests
-------

As with the 'proper' version
```
npm install
npm test
```

and you should see the tests pass.

Test config changes
-------
Jest tutorials all keep their tests inline with the code being tested, something like this:

```
js
--components
---- my-component.js
---- tests
------ my-component-test.js
```

In this repo the tests have been moved into a separate directory to the code being tested.

```
js
--components
---- my-component.js
tests
-- components
---- my-component-test.js
```

however supporting this with jest requires more config that you might think... In the correct version of the repo the jest config looks like this:

```
"jest": {
  "rootDir": "js""
 }
```

But just changing the rootDir to **/tests/** wont work. It looks like jest needs its rootDir to be a parent of both the test and the js (src) files. The tests link to the files directly so there is no obvious reason for the test failures.

Instead the following works:

```
"jest": {
  "rootDir": "./",
  "testPathDirs": ["./"],
  "testDirectoryName": "tests",
  "unmockedModulePathPatterns" : ["/node_modules/"]
}
```

Just setting the rootDir to "./" causes a number of errors, instead it appears you need to tell jest to look at the project root, but only looks for tests in the 'tests' directory we set up (overriding the ```__tests__``` default). 'unmockedModulePathPatterns' actually has 'node_modules' set by default
