## Takeaways

* Code in Android classes is impossible to unit test
* An instrumented test is not a unit test
* Refactor your logic into POJOs
    * Well-established architectural patterns you can use  
* Use Dependency Injection
* Make your code assertable
* Write unit tests!

Note: Every new component to a system introduces extra problems and this is the same with unit tests.
Once you have tests you always need to keep them up-to-date with your current code.
Although this can be a hassle, failing tests actually reflect the changes in your code, so you can
double-check the changes you have made.
