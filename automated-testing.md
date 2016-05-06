# Automated Testing: Beyond the basics

Jim Holmes (@aJimHolmes)

## A Story of Woe

Interesting book to read - The Art of Unit Testing in .NET

Over time tests become slow, brittle and WTFITBWWH (What The F*** Is Tested By What Where How)

## How to fix things

First and foremost, be thoughtful. Reduce overlap but be careful about coverage. Depending on the complexity of the architecture and the already existing test coverage, it might be too much of an investment to add any automation test.

"TDD is magic bacon and unicorns and fixes everything." - Quote of the day

Have to find the appropriate level to test things. Developers love happy path testing so have to be cognizant about missing edge cases.

Don't test stupid stuff - don't test low value, unoptimized things or dependencies. 3rd party tools and services are already tested!

Test code *is production code*!

The SOLID principle - The most important thing to keep in mind is the Single Responsibility Principle. If working with Cucumber, step definitions can get out of hand unless created thoughtfully.

Step definitions should focus on domain objects rather than actions with domain objects. Page objects should only know about the UI but they should not know anything about the tests, i.e. they should not have asserts.

The other thing to remember is keeping the tests moist. "Keep your system DRY and keep your test moist". Want to avoid too much abstraction.

Another Interesting read - Continuous Delivery

## Test Data

Avoid using the other tests to set up data for a test. In other words, make no assumption about the existence of the data.

Use baseline data sets to get started and then customize from there. The baseline datasets are assets so they should be checked into source control.

Use custom APIs to help setup and teardown test data. Use something like Faker to set up real-looking random data for UI.

## Configuration

Freaking Stupid Asses Pain in the Butt Async...

Instead of putting `sleep()` everywhere, figure out what the signal of actions getting completed is and watch for that kind of stuff. This might require changes in production code but will be better for the long run.

Another way to get around this would be to intercept the action going out to a service or something but that doesn't ascertain that the service did something with your data.

An oracle or a heuristic is going down to the persistence layer and checking the single source of truth.

## Speed Issues

Parallelization solves everything, right?! Fix the system that the tests run over rather than running tests in parallel.

## Takeaways

* Be thoughtful about coverage
* Manage Your Data
* Build out some custom APIs
