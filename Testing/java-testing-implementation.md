# Self-testing Library Tutorial
This guide (tutorial?) will go though all of the main points that
you need to know how to use the self-testing libary thing

## 0. Setting up `Robot`
- Add `TestRunner.runTests()` to the END of `Robot.testInit()` (after `CommandScheduler.getInstance().cancelAll()` is called)
- Add `TestRunner.periodic()` to the end of `Robot.testPeriodic()`
- That's it!

## 1a. Implementing the interface
On the subsystem (or class in general) you'd like to test, add `implements TestableSubsystem` to the class declaration; e.g. your class declaration would look a little something like `public class MySubsystem extends SubsystemBase implements TestableSubsystem { etc... }`. VS Code will get mad at you since you have un-implemented methods, so click the class name and hit `ctrl`+`.` to go into quick actions, then press "add unimplemented methods".
  
The method that you implement returns a `TestableCommand`.

## 1b. A note about the `TestableCommand` interface
Like mentioned previously, it's an interface (e.g. cannot be directly instantiated). For ease of use, there is an abstract class called TestBase which abstracts (get it?) a little away for you. There are a few key points:
- `getCurrentResult()` is abstract and REQUIRED to be overridden - it is what determines when the test is considered complete (anything other than TestResult.IN_PROGRESS will end the test)
- `getLoggableResult(TestResult)` returns a string when the test completes, and can include motor/encoder/test information
- `getLogSelection()` defaults to returning `LOG_ALL`, but can be overridden to log other things via a bitmask
- `toString()` is a method inherent to all classes, but here it is helpful to provide a more human-readable name for your test
- `SequencedTest` is an alternative to `TestBase` that allows you to step through a list of tests (if any of the tests fail, the whole sequence will fail). Failed tests will also interrupt the step chain

### 1b i. Instantiating abstract classes
As stated, abstract classes cannot be directly instantiated. this leaves you with two options...      

**Interface Implementation**:
- Make a new class that implements `TestableCommand`, then override all needed methods  

**Subclassing**:
- Make a new class that extends `TestBase`, then override all needed methods    

**Anonymous class**:
- Write `new TestCommand()` like you would like normally, but add curly braces to the end (like `new TestCommand() {}`), and override the needed methods within those curly braces. this anonymous class operates like a normal class (can have instance variables, methods, etc. but is, well, anonymous [cannot be referred to with a name like other classes])

## 1c. What's the point of testing like this?
Anything that is important for the functionality for the robot should be tested, both for pre-match pit check and ensuring everything works okay before committing to main. Make sure to include things like:
- For position control, does the motor's position approach a given setpoint, with minimal error and minimal lag? are the software bounds fully working (try setting the setpoint outside the bounds, and make sure that the motor doesn't actually try to go past those bounds)
- For velocity control, does the motor get up to speed quickly and precisely?
- If you have custom states for a mechanism, are those states correctly calculated? Are edge cases accounted for?

## 2. Registering the `TestableSubsystem` to the `TestRunner`
The `TestRunner` is a singleton class that does what it sounds like (runs its registered tests). to register a test, call `TestRunner.addTest(your TestableSubsystem)` wherever the robot is instantiated (probably `RobotContainer`'s constructor).

## 3. How to run the tests
Go into driver station and run the test mode!  

# Unit Testing Tutorial
This is pretty simple compared to self-testing? I think at least            

## 0. About tests
- Unit Tests are essentially just verifying that something about your code or logic matches some expected outcome
- Pretty broadly applicable here, but one downside is that tests are pure-code only (no on-robot tests here)

## 1. Making sure your workspace is setup
- Ensure that you have a test folder set up properly beside your `main` folder (see Superstructure for example)
- - todo: Add example here
- If one doesn't already exist, create some new class file in that new folder(s) you've made

## 2. Writing tests
- Import the JUnit library `org.junit.jupiter.api.[etc]` in your shiny new class file. Arguably the two most important imports: `org.junit.jupiter.api.Test` and `org.junit.jupiter.api.Assertions.assertEquals` (the latter one is a static import, lots of other handy assertions in the Assertions namespace)
- Create a new `void` method without any arguments (something like `void myFirstTest() { etc... }`)
- Add the `@Test` annotation above the method name (similar to `@Override`)
- For each criteria you want to test, create an assertion (if you wanted to double-check that some `Calculator` class was correctly written, you could put `assertEquals(Calculator.add(1,1), 2)` in that method)

## 3. Running tests
- The beauty of JUnit is that tests are run whenever the code is built, which includes sim and deploying to robot. If ANY tests fail, the code won't be built... so if you write tests for all your logic, you'll never put any faulty logic on the bot!
