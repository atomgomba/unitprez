## Definition

* method or class in Java
  * or default interface method since Java 8
* isolation: reduce code to minimum
* no external code
* cause of failure immediately obvious

## Change in Dependency

* using mock to test the Baby only
* check if method has been called using a flag

## By-pass: Dependency Injection

* provide deps from outside
* change behavior without change to dependent class
* inject mocks at testing time
* constructor injection is preferred

## The Problem on Android

* code in Android classes
* traditional unit testing impossible
* these are handed over to developer
  * manifest, factory method, method argument
* to have these objects requires emulator/device
* Fragments can be instantiated, but they have problems

## What about instrumented tests?

* show behavior in the context of Android
* integration test
* focused on testing user interactions
* not to be confused with unit tests

## Unit tests are

* fast and lightweight: no device/emulator required
* speed is very important
* easy to use with CI
* run tests automatically on server
* reliable because no external dependency

## Mock android.jar

* stub methods, not actual code
* useful when having soft dependencies
* or mocking Android classes
* throw RuntimeException, test isolation of your code
* example to mock getResources().getString() on the JVM

## Writing unit testable code

* Clean Architecture (Robert C. Martin)
* POJO classes (Martin Fowler)
* Dependency Injection
* use Android classes as execution entry points
* assertable code

## Ready-made solutions

* widely discussed
* MVP is very popular
  * Google Codelabs/Android Testing Codelab
  * roll your own
* point is to be independent of Android
* plain-old OOP principles
  * separation of concerns
  * polymorphism
  * Liskov substitution
  * and so on

## Q&D MVP example

* presenter governs view
* Activity isolated via interface
* DI framework required
* constructor injection not available
* just to give an idea how separation works

## Takeaways

* Code in Android classes is impossible to unit test
* An instrumented test is not a unit test
* Refactor your logic into POJOs
    * Well-established architectural patterns you can use  
* Use Dependency Injection
* Make your code assertable
* Write unit tests!
