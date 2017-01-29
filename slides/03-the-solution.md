## Writing unit testable code

* POJO classes (Martin Fowler)
* Clean Architecture (Robert C. Martin)
* Dependency Injection
* assertable code

Note: I will not talk about Clean Architecture, because it is an extensive topic, but it is a collection of principles to design your
application in a way to make it independent of any underlying or external systems, frameworks and what not. It is mentioned only to
encourage you to search for the man's talk on YouTube.
POJO stands for Plain Old Java Object and it basically means an instance of a class that has no dependencies other than the ones provided by the Java language.
In our current context it means a class without any Android-related dependencies. Of course in practice it's not always
possible to avoid Android completely, but in case of soft dependencies you can use the mock android.jar.
So if you put your business logic (or in fact any non-UI logic you would like to test) into a POJO,
instead of an Activity for example, you will gain complete control over the lifetime of your object and thus make unit testing possible.
So the whole point of making your code unit testable is to refactor logic into POJO classes and make those classes assertable in ways I have shown previously.
Of course you still can't get away from the platform completely because you have a launcher Activity for example, you need to rely on user interactions,
presentation, navigation and so on, but you can still use those Android classes as mere execution entry points for your own code.



## Ready-made solutions

Well-established and widely discussed architectural patterns:

* MVP (Model-View-Presenter)
* MVVM (Model-View-ViewModel)
* MVI (Model-View-Intent)
* VIPER (View-Interactor-Presenter-Entity-Router)
* etc.

Note: Fortunately smart people have already came up with architectural patterns we can just take and use.
There are already many articles about them, you can find many example code and descriptions of overall experiences using them.
Though it seems that MVP is the most popular one (for eg. you can find MVP examples on Google Codelabs too under Android Testing Codelab),
you don't have to go dogmatic about that.
Feel free to experiment and mix and match concepts and try to find the best architecture that suits you and your use-case,
fits your maintenance needs, size of your project and so on.
The whole point is still to make your code unit testable by making it independent of Android.



## Q&D MVP example

```java
class ArticleActivity extends Activity implements ArticleView {
    @Inject ArticlePresenter mPresenter;
    @Override void showArticle(ArticleModel model) {
        // show article data on UI
    }
}

class ArticlePresenter {
    public ArticlePresenter(ArticleView view) {
        mView = view;
    }
}
```
