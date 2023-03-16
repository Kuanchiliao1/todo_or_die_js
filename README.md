## Usage
Install the package via npm:
```npm install todo-or-die-js```

Once installed, you can import and mark TODOs for yourself anywhere you like:
```js
const TodoOrDie = require('todo-or-die-js');

TodoOrDie("Update after APIv2 goes live", new Date(2019, 1, 4));
```
### The Old Way of Procrastination

In the Bad Old Days™, if you had a bit of code you knew you needed to change later, you might leave yourself a code comment to remind yourself to change it. For example:

```js
// TODO: remember to delete after JS app has propagated
function show() {
  redirectToRootPath();
}
```

This was bad. The comment did nothing to remind yourself or anyone else to actually delete the code. The continued existence of the redirect eventually resulted in an actual support incident (long story).

### The New Way to Put Off Coding

So, to solve this problem, we have the todo-or-die-js package.

To use it, try replacing one of your TODO comments with something like this:

```js
const TodoOrDie = require('todo-or-die-js');

TodoOrDie("delete after JS app has propagated", new Date(2019, 1, 4));
function show() {
  redirectToRootPath();
}
```

Nothing will happen at all until February 4th, at which point the package will throw an error whenever this function is called until someone deals with it.

You may also pass a condition, either as a function or a boolean test:

```js
const TodoOrDie = require('todo-or-die-js');

class User {
  isOneMillionthUser() {
    return this.id === 1000000;
  }
}

TodoOrDie("delete after someone wins", () => User.count > 1000000);
```

You can also pass both a date and a condition (where both must be met for an error to be thrown) or, I guess, neither (where an error will be thrown as soon as TodoOrDie is invoked).

### What kind of error?

The package will throw a TodoOrDie.OverdueError whenever a TODO is overdue. The message looks like this:

```js
TODO: "Visit Wisconsin" came due on 2016-11-09. Do it!
```

### Customizing the Behavior

```js
const TodoOrDie = require('todo-or-die-js');

TodoOrDie.config({
  die: (message, dueAt) => {
    if (message.includes("Karen")) {
      throw new Error("Hey Karen, your code's broke");
    }
  }
});
```

Now, any TodoOrDie() invocations in your codebase (other than Karen's) will be ignored. (You can restore the default behavior with TodoOrDie.reset()).

### When is this useful?

This package may come in handy whenever you know the code will need to change, but it can't be changed just yet, and you lack some other reliable means of ensuring yourself (or your team) will actually follow through on making the change later.

Some common examples:

- A feature flag was added to the app a long time ago, but the old code path is still present, even after the flag had been enabled for everyone. Except now there's also a useless TODO: delete comment to keep it company
- A failing test was blocking the build and someone felt an urgent pressure to deploy the app anyway. So, rather than fix the test, Bill commented it out "for now"
- You're a real funny person and you think it'd be hilarious to make a bunch of Aaron's tests start failing on Christmas morning

### Pro-tip

JavaScript's Date object is useful, but don't think you're going to be able to do this and actually accomplish anything:

```js
const TodoOrDie = require('todo-or-die-js');

const twoWeeksFromNow = new Date();
twoWeeksFromNow.setDate(twoWeeksFromNow.getDate() + 14);

TodoOrDie("Update after APIv2 goes live", twoWeeksFromNow);
```

It will never be two weeks from now. Consider using a specific date to set your deadline.