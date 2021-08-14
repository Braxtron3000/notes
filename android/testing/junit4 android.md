# General Junit Rules
JUnit is a testing framework not managed by android but they have made some things that are made specifically for android as listed in this document.

for general rules to JUnit visit this [page](https://junit.org/junit5/docs/current/user-guide/)
```
```
# JUnit4 rules with AndroidX Test

android has some rules that go with using the JUnit testing framework. JUnit is used in android through the *anroidJUnitRunner* class.
Esspresso can be used with this as well.

there are two rules

1. Activity Scenario Rule
   * provides a functional testing of a single activity. This is why this example is considered a large test and the next example is a medium.
   * to access the activity in your test logic use the callback runnable to ***ActivityScenarioRule.getScenario().onActivity()***.

    example
    ```
    @RunWith(AndroidJUnit4.class)
    @LargeTest
    public class MyClassTest {
        @Rule
        public ActivityScenarioRule<MyClass> activityRule =
                new ActivityScenarioRule(MyClass.class);

        @Test
        public void myClassMethod_ReturnsTrue() { ... }
    }
    ```

2. ServiceTestRule
   * provides a simplified way to start up & shut down your service.
    example notice that's its a medium test

    ```
        @RunWith(AndroidJUnit4.class)
    @MediumTest
    public class MyServiceTest {
        @Rule
        public final ServiceTestRule serviceRule = new ServiceTestRule();

        @Test
        public void testWithStartedService() {
            serviceRule.startService(
                    new Intent(ApplicationProvider.getApplicationContext(),
                    MyService.class));
            // Add your test code here.
        }

        @Test
        public void testWithBoundService() {
            IBinder binder = serviceRule.bindService(
                    new Intent(ApplicationProvider.getApplicationContext(),
                    MyService.class));
            MyService service = ((MyService.LocalBinder) binder).getService();
            assertThat(service.doSomethingToReturnTrue()).isTrue();
        }
    }
    ```
# [AndroidJUnitRunner](https://developer.android.com/training/testing/junit-runner)

this is a JUnit test runner for JUnit 3 or 4. 

it helps with
* writing JUnit tests
* Accessing instrumentation information
* filtering tests
* sharding tests

each AndroidJUnit test class must have a @runwith above it's declaration like so..
>@RunWith(AndroidJUnit4.class)<br>
>@LargeTest<br>
>public class ChangeTextBehaviorTest {<br>

# [android test orchetrator](https://developer.android.com/training/testing/junit-runner#using-android-test-orchestrator)
this is something we may want to look at later but not right now cause it's weird.

# filter tests & other shortcuts

to access the app context..
> ApplicationProvider.getApplicationContext()

in addition to the filters JUnit 4 provides android provides some android-specific annotations

filter | description |
 --| --|
@RequiresDevice | specifies the test should only be on an actual device & not an emulator
@SdkSuppress | suppresses the test from being run on a lower API level than the given level ex: ***@SDKSuppress(minSdkVersion=23)***
@SmallTest,@MediumTest,@LargeTest| classifies how long the test should take to run, and how frequently you can run the test

## [shard tests](https://developer.android.com/training/testing/junit-runner#sharding-tests)
these help split the test suite into multiple shards so it's easier to tests belonging to same shard together as a group.
