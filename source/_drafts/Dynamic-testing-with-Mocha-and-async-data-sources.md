title: Dynamic testing with Mocha and async data sources
tags:

- programming

---

Mocha+Chai are fantastic libraries for BDD or TDD development, but don't have a well documented method to do dynamic data driven testing. For example if you need to

```javascript
describe("a suite", function() {
  console.log("in describe: " + this.constructor.name);
  for (let i = 1; i < 10; i++)
    this.addTest(
      new mocha.Test("a dynamically added test" + i, function() {
        expect(true).to.equal(true);
      })
    );

  before(function() {
    console.log("in before: " + this.constructor.name);
  });

  it("is cool", function() {
    console.log(this.constructor.name);
  });
});
```

```javascript
describe("a suite", function() {
  for (let i = 1; i < 10; i++) {
    this.addTest(
      new mocha.Test("a dynamically added test #" + i, function() {
        expect(true).to.equal(true);
      })
    );
  }

  it("is a predefined test", function() {
    console.log(this.constructor.name);
  });
});
```
