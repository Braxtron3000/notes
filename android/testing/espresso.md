# [Espresso Overview](https://developer.android.com/training/testing/espresso)

example of an espresso test:
```
@Test
public void greeterSaysHello() {
    onView(withId(R.id.name_field)).perform(typeText("Steve"));
    onView(withId(R.id.greet_button)).perform(click());
    onView(withText("Hello Steve!")).check(matches(isDisplayed()));
}
```

basically to recap [this 5 min video](https://www.youtube.com/watch?v=kL3MCQV2M2s&t=148s). The guy creates a junit test in the testing directory.

The methods and objects he used were pretty straight forward yet complicated because of their diverse datastructures (lots of classes that use generics).

Basically with esspresso you can use methods to tell the emulator to perform certain tasks on the app such as click a certain button or scroll up.

there's also like a ton of libraries that esspresso offers for specification of certain tests.


# [setting up](https://developer.android.com/training/testing/espresso/setup)

1. get rid of animation stuff (it's kinda sketchy for testing)
  * basically get rid of all animation scale stuff on your device's **settings > Developer** options
2. add espresso dependencies

>androidTestImplementation 'androidx.test.espresso:espresso-core:3.1.0' <br>
>androidTestImplementation 'androidx.test:runner:1.1.0' <br>
>androidTestImplementation 'androidx.test:rules:1.1.0'
  
3. set instrumentation runner

> testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"

# turn off analytics

analytics tells google the name of the package and helps google know how many peeps are using espresso as well as the "volume of usage"

here's how to turn off this nosy things.
run this instrumentation command

>adb shell am instrument -e disableAnalytics true






# [basics](https://developer.android.com/training/testing/espresso/basics#components)

main components of expresso

component| what it does
--|--|
Espresso | entry point with interactions with views (onview(), or onData())
ViewMatcher | uses the Matcher<? superView> interface for the purpose of locating a view.
ViewActions | action objects that can be passed to the ViewInteraction.perform() method such as click()
ViewAssertions | asserts the state of the currently selected view.


## a basic example

```
@RunWith(AndroidJUnit4.class)
@LargeTest
public class HelloWorldEspressoTest {

    @Rule
    public ActivityScenarioRule<MainActivity> activityRule =
            new ActivityScenarioRule<>(MainActivity.class);

    @Test
    public void listGoesOverTheFold() {
        onView(withText("Hello world!")).check(matches(isDisplayed()));
    }
}
```