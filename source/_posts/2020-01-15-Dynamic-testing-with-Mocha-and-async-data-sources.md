title: Dynamic testing with Mocha and async data sources
date: 2020-01-15 12:37:06
tags:

- programming
- mocha
- chai
- testing

---

Mocha & Chai are fantastic libraries for BDD or TDD development, but don't have a well documented method to do **data driven testing**. I define data driven testing here as dynamically generated tests, duplicated with the _structure and content of the data itself_ being the varying input.

The tricky thing about using these libraries for this purpose is that the test suite generation itself expects all the tests to be defined in advance. As a result, looping over the data and using the regular describe(), each(), it(), etc does not go well.

<!-- more -->

To tackle this problem my general approach is:

1. Expose the data to Javascript. You might choose to directly connect to the legacy data, or cache somewhere. I decided to cache in MongoDB for performance (testing > 5 billion records in around 10 minutes) and to lower load on data source.
2. Define tests for each unit of data (e.g. usually a database row or single "record" from your business logic).
3. Loop over these tests and generate a new Mocha test for all your target data imperatively, rather than declaratively.

For example: say you have a legacy database that is missing strict schema enforcement, or uses strings and unusual data structures to store business configuration. As gross as this is it's more common than you'd expect, particularly in large, old organisations. As a result, before (or perhaps until) you move to a more modern database, you will need to verify the relationships and structure of the data aligns with what you would expect to avoid runtime issues. If you want to do so before runtime as a housekeeping task, enter **data driven testing**.

This is roughly the structure I landed on:

```javascript
async () => {
  // load data from cache
  const User = require("mongoose").model("Transaction");
  const users = await User.find();

  users.forEach(user => {
    // create a suite per user record
    describe(`user: ${user.dbid}`, function() {
      for (let i = 1; i < 10; i++)
        this.addTest(
          new mocha.Test("a dynamically added test" + i, function() {
            // define all the things you would expect the data structure to match
            expect(user.name).to.equal(true);
          })
        );
    });
  });
};
```

You then get nice reporting on the exact record and nature of unexpected data, and correct accordingly in the data. I'd love to hear other exotic uses of testing suites!
