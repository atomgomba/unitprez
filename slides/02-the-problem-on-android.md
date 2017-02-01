## The Problem on Android

when using the SDK in the "standard" way

Note: By "standard way" I mean developing Android applications in the way it is described by the official documentation.
If you look at the code examples on android.com you will notice that the application logic is always contained within
an Activity, a Service and similar kind of classes whose instances are created and managed by the system.
Sometimes they're declared in the manifest, sometimes they're handed to you by a God object's (eg. Context) factory method or
as a method argument. And this won't allow you to complete the steps of a unit test.

---

```java
ArticleViewActivity a = new ArticleViewActivity();
Bundle b = new Bundle();
b.putString("articleId", "bab13");
a.onCreate(b);
assertTrue(a.hasArticle());
```

Note: So normally you would approach creating a unit test like this, but obviously it's impossible.
Moreover, to gain access to these objects you would have to run the test in an emulator or a device.
And that is called an instrumented test. When it comes to Fragments, sure you can instantiate a Fragment yourself,
but if you would like to do yourself a favor you won't be using Fragments anyway.

---


## What about instrumented tests?

* they are integration tests
* focused on asserting UI events
* not to be confused with unit tests

Note: An instrumented test will show you how your code behaves in the context of Android, thus make it an integration test.
Besides a bug in your own code, the test could fail because of problems with the emulator/device, or potential bugs in Android itself, and so on.
And it is focused on asserting user interactions, so instrumented tests and unit tests are not to be confused with each other.

---

## Unit tests are:

* *fast* and lightweight
* easy to setup
  * emulator or device not required
  * easy to use with CI
* reliable
  
Note: Unit tests make debugging easier, they don't require an emulator or a device to run so they're also lightweight and fast.
Speed is quite important and an overall advantage, since the faster your test is, it's more likely you'll use it frequently.
A test that takes minutes to run can soon get frustrating in contrast to one that takes only seconds to complete.
Furthermore, running tests in the JVM also makes it a lot easier to incorporate them into your build process, which is also important.
By including a unit testing step in your CI process, you can let your tests run and report automatically.
Unit tests are also reliable because their success or failure do not depend on behavior of any external systems.

---

## Mock android.jar

* modified version of android.jar
* contains stubs and not actual code
* useful for mocking Android objects
* and when having soft dependencies

Note: The Android SDK provides a modified version of android.jar which contains no actual code other than stubs.
Mock Android objects come in handy when having soft dependencies. A soft dependency is a dependency you need to get your test compiled,
but it will be never called. Sometimes you need a soft dependency in order to create an object, but not to test some of its methods.
The android.jar stub methods throw a RuntimeException when called and it can also be useful to check how much your code depends on Android.

---

**dev.android.com:**  
"This is to make sure you test only your code and do not depend on any particular behavior of the Android platform (that you have not explicitly mocked)."

```java
class TestContext extends MockContext {
    @Override public MockResources getResources() {
        return new TestResources();
    }
}
```
