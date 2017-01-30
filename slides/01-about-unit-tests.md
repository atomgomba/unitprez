## Definition

A method to verify the expected behavior of an *isolated* part of source code.

Note: In Java this "isolated part" means a method or a class. We can also call these tests method tests and class tests respectively.
The notion of isolation is very important, because it makes a unit test a unit test.
It is desired to reduce the amount of executed code to the necessary minimum to test a given feature or functionality.
So for eg. your test should not rely on external code, because you don't want to test someone else's code, but your own code.
And the point is that when a test fails it must be immediately obvious where the root of the problem is.



## Common Structure

```java
// 1. arrange or setup step
Baby baby = new Baby();
// 2. act or action step
baby.removeToy();
// 3. assertion or verification step
assertTrue(baby.isCrying());
```

Note: This is the most common structure of unit tests, but having a verification step is inevitable.
In the arrange step you sort of create the situation which you want to test, then take action and finally inspect the result.



## 3 + 1 Ways to Assert



## 1. Property Change

```java
Baby baby = new Baby();
baby.removeToy();
assertTrue(baby.isCrying());
```

Note: Here we're simply asserting an expected change in an accessible property.



## 2. Return Value

```java
Baby baby = new Baby();
boolean toyGiven = baby.giveToy(null);
assertFalse(toyGiven);
```

Note: It's simple. When asserting return value we verify the value returned by the method we've called in the action step.
In this case we expect the Baby to not to receive the toy if the toy was empty.



## 3. Change in Dependency

```java
Baby baby = new Baby();
Bottle bottle = new MockBottle();
baby.feed(bottle);
assertFalse(bottle.isConsumeCalled());

class MockBottle extends Bottle {
    boolean mConsumeCalled;
    @Override void consume() {
        mConsumeCalled = true;
    }
    boolean isConsumeCalled() {
        return mConsumeCalled;
    }
}
```

Note: Here we're verifying the state of an object which is passed in as a dependency to the method in the action step.
In case our Baby class behaves well, we expect it to consume from the bottle, so to call the bottle's consume method.
This is also simple, but interesting as well, because here we see something called Dependency Injection, which is
very useful when it comes to code extensibility, maintenance and testing.
As you can see we're testing the Baby, so we have to remove the external complexity imposed by the Bottle.
All we want to do is to verify whether the Baby has used the Bottle or not. For this purpose we are
mocking the Bottle which does nothing else than setting the correct flag which we can later inspect.
Creating these mock classes can be tedious work, but fortunately there are tools that can help you with this.
One of the most popular among these is mockito.



### By-pass: Dependency Injection

```java
// injection via constructor
Baby baby = new Baby(new MockBottle());

// injection via setter
Baby baby = new Baby();
baby.giveBottle(new MockBottle());
```

dependencies provided externally

Note: Dependency Injection is a fancy term for a very simple thing. It means that when an object needs another object in order
to correctly operate, we pass in the other object either in the constructor or using a setter. By providing the dependency
externally, we gain the power and flexibility to freely replace them without modifying the original object. So we don't have to modify the Baby
in order to feed it with a bottle of apple juice instead of a bottle of milk, or a small bottle instead of a bigger bottle.
Also during testing time we could provide mocked dependencies with stub methods, reduce their behavior to bare minimum in order to
achieve code isolation.
Constructor injection is preferred over setter injection every time it is feasible. If you inject dependencies in the constructor,
then you can make those fields final and all of them will be already available at construction time.
This helps readability and reliability.



### Don't do this!

```java
class Baby {
    private Bottle mBottle;
    public Baby() {
        // bad code, not testable
        mBottle = new Bottle();
    }
}
```

Note: This is considered bad practice, since here you cannot replace the dependency for the purpose of testing without modifying the original class.



## +1 Exceptions

```java
Baby baby = new Baby();
boolean exceptionWasThrown = false;
try {
    baby.removeToy();
} catch (ToyNotExistsException e) {
    exceptionWasThrown = true;
}
assertTrue(exceptionWasThrown);
```

Note: This case is uncommon and potentially dangerous too because normally when your code throws an exception your app will crash.
However there can be valid use-cases for this, for example when you're developing a library, framework or SDK for that matter,
you might want to give your users the liberty to handle exceptions at another level, in their application code,
instead of deciding how to handle it for them. In this case you want to make sure that under certain circumstances the
code will throw the expected exception. It's generally better for lower level code to delegate exception handling to higher level code.
