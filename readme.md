# npm 7 workspace install issue

Running `npm install` on a workspace project works correctly the first time.

Running `npm install` on the workspace a second time deletes everything in the workspace's `node_modules` directory.

Running `npm install` a third time says the install is "up to date" even though nothing is installed.


**Expected results**

It is expected that `npm install` works similarly for a workspace project as for a regular, non-workspace project.

i.e. It is expected that running `npm install` again on a workspace project should install new dependencies that have been added to workspace package.json files or leave the installed dependencies as-is if there are no changes.

It is unexpected that `npm install` empties out `/node_modules` when running the second time.

To re-install dependencies into the workspace project you must delete the workspace `/node_modules/*` and `/package-lock.json` or run `npm update`.

It is unexpected that the user should run `npm install` the first time but remember to run `npm update` any subsequent time as this is different to how npm works inside a regular project (i.e. it is expected that it is possible to run `npm install` multiple times in a workspace project).


## Steps to reproduce

1. Clone this repository
2. From this workspace directory run `npm install`
   - dependencies are installed correctly
3. Run `npm install` again
   - all packages are removed from `node_modules`
4. Any subsequent `npm install` says that it is up to date even though `node_modules` is empty


** Final result:** 

`npm_modules` is empty except for a `.package-lock.json` file and nothing is installed.


----------------------------------------


## Example output

```
$ npm --version
7.0.0-beta.10

$ node --version
v12.16.2

$ npm install

added 3 packages, and audited 5 packages in 507ms

found 0 vulnerabilities


$ npm install

removed 3 packages in 218ms

found 0 vulnerabilities


$ npm install

up to date in 102ms

found 0 vulnerabilities
```



--------------------------------------

## Contents of generated lock files:


```json
// /package-lock.json

{
  "name": "npm7-test",
  "lockfileVersion": 2,
  "requires": true,
  "packages": {
    "": {
      "workspaces": [
        "package-a",
        "package-b"
      ]
    }
  }
}
```


```json
// /node_modules/.package-lock.json
{
  "name": "npm7-workspace-test",
  "lockfileVersion": 2,
  "requires": true,
  "packages": {}
}

```
