# [overview of tests](https://developer.android.com/training/testing/fundamentals#small-tests)

testing is good because it helps you fail faster to correct mistakes and it helps to boost the production debugging speed of your app.

in order for effective tests you need to understand [modularity]()


there are three types of tests
1. small tests
2. medium tests
3. large tests

the larger the test is the more time it takes but also the more assured the outcome is. Meaning that the largest test best shows the fidelity of the app. 

It's like finals in classes. You have to go through each chapter's test. Then you have a midterm test and finally a final covering everything.

tests should be carried out in the *tests* directories of the application project.

 *androidTest* directory is for integration, end-to-end and other tests where the java virtual machine alone isn't sufficient. The *test* directory is for unit tests which run on the local machine so small tests

## [small tests](https://developer.android.com/training/testing/fundamentals#small-tests)

* class based.
* [*Roboelectric*](http://robolectric.org/)

## [medium tests](https://developer.android.com/training/testing/fundamentals#medium-tests)

* validating layout xml, or view/viewmodel relations
* repository layer
* vertical slices, interactions on a particular screen
* fragment stuff (multifragments!)
* [*espresso intents*](https://developer.android.com/training/testing/espresso/intents)
  * an extension to espresso which enables validation and stubbing of intents.
* also just regular [*espresso*](https://developer.android.com/training/testing/espresso) is good too
  * performs actions on view objects
  * accessibility needs testing (like for the blind)
  * recyclerview & adapterview objects
  * outgoing intents validation
  * verify structure of DOM within webview objects

## [large tests](https://developer.android.com/training/testing/fundamentals#large-tests)

* closest to testing the full app itself
* divide large test suites by team ownership, functional verticals?, or user goals
* it's typcically better to test on an emulated device or firebase test lab because it's quicker and easier.
* espresso helps with large tests too
  * completes workflows that cross apps process boundaries
  * long running background operations
  * performing off device tests