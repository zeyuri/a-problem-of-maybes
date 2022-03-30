# A problem of maybes

The life is full of maybes. So are some problems that we solve in Tech.

Lets suppose we receives a given json:

```json
{
  "org": "Konoha",
  "full_name": "Konohagakure no Sato",
  "statistics": {
    "population": "5/5",
    "military": "3/5",
    "economy": "2/5"
  },
  "divisions": [
    {
      "type": "MEDIC",
      "member_count": 50,
      "leader": "Sakura Haruno",
      "leader_id": 489
    },
    {
      "type": "ANBU",
      "member_count": 30,
      "leader": "Hatake Kakashi",
      "leader_id": 340
    },
    {
      "type": "ROOT",
      "member_count": 10,
      "leader": "Shimura Danzō",
      "leader_id": 130
    }
  ]
}
```

But now root division has been merged in to Anbu branch, but you have to change this data before you can show it your User Interface.

Oh, Danzõ is dead. So dont forget add the remaining members of root into the member count of Anbu.

Important techinical detail: Our backend that serves this json don't make garantees about the order of the `divisions` list to be consistent between requests.

## Boring stuff - Part one

IMPORTANT: Use latest version of node. At this very moment that I'm writing this node@16.10

Create a directory with any name you want, mine will be `a-problem-of-maybes`:

```bash
mkdir  a-problem-of-maybes
```

Then use npm to make it happen

```bash
npm init -y
```

## Creating a test that fails

This is the stage that we make assumptions of the right answer. We create the expected results.

This is the time we create a failing test that will eventually make us garanties that we can mantain different answers but still valid ones.

Create an index.js file

```bash
touch main.js
```

We instantiate a const with a label of `initialData` and assign it a value of the given json.

```js
//main.js
const initialData = {
  "org": "Konoha",
  "full_name": "Konohagakure no Sato",
  "statistics": {
    "population": "5/5",
    "military": "3/5",
    "economy": "2/5"
  },
  "divisions": [...],
}
```

then we create a function with an label of `mergeRootIntoAnbu`. This function expect data as argument and will be where we're gonna create our answer. Right now it just return a string with a name of some random delicious thing that we can cook in any form and still be satisfied when eating.

```js
//main.js
function mergeRootIntoAnbu(data) {
  return "batata";
}
```

Then we create the expected result

```js
//main.js
const expectedResult = {
  org: "Konoha",
  full_name: "Konohagakure no Sato",
  statistics: {
    population: "5/5",
    military: "3/5",
    economy: "2/5",
  },
  divisions: [
    {
      type: "MEDIC",
      member_count: 50,
      leader: "Sakura Haruno",
      leader_id: 489,
    },
    {
      type: "ANBU",
      member_count: 39,
      leader: "Hatake Kakashi",
      leader_id: 340,
    },
  ],
};
```

Then we make an assertion that we will expect to fail.

```js
//main.js
const result = mergeRootIntoAnbu(initialData);

expect(result).toEqual(expectedResult);
```

Run it using node

```bash
node index.js
```

And we get.... the following output

![first error](./images/first_error.png)

## Boring stuff - part two

Hmm... "expect is not defined". Oh yeah, we forgot to install a test runner to help us to move quickly.

Lets do it:

```bash
npm i -D jest babel-jest @babel/core @babel/preset-env
```

Now create a file called `babel.config.js`

```bash
touch babel.config.js
```

Add to the file the following content

```js
// babel.config.js
module.exports = {
  presets: [["@babel/preset-env", { targets: { node: "current" } }]],
};
```

Almost there, we need to first create a test file, it can be `main.test.js`

```bash
touch main.test.js
```

Now just extract everything but the function definition of `mergeRootIntoAnbu` from ` main.js`

```js
//main.js
export function mergeRootIntoAnbu(data) {
  return "batata";
}
```

Paste everything into `main.test.js`

```js
//main.test.js
import { mergeRootIntoAnbu } from './main'

export const initialData = {...};

const expectedResult = {...};

const result = mergeRootIntoAnbu(initialData);

expect(result).toEqual(expectedResult);

```

We just need to wrap our assertions in a test function to be runned by jest and import the jest helpers `expect` and `test`:

```js
//main.test.js
import { expect, test } from "@jest/globals";
import { mergeRootIntoAnbu } from "./main";

export const initialData = {...};

const expectedResult = {...};

test("mergeRootIntoAnbu should return the expected result", () => {
  const result = mergeRootIntoAnbu(initialData);

  expect(result).toEqual(expectedResult);
});

```

Now we finally run the test

```bash
npx jest main
```

And we got an error, but an useful one:
