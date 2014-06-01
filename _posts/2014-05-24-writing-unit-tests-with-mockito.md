---
author: psgs
layout: post
excerpt: >
  In order to develop smooth, bug-free applications, testing is a must.
  Many common development work-flows implement rigorous testing processes and emphasise concepts such as test-driven development.
  Testing code, however, isn't always as straight forward as some may think...
categories:
  - development
tags:
  - java
  - mockito
  - junit
  - unit tests
  - tests
  - coverage
reading-time: 5 minutes
title: Writing Unit Tests with Mockito
---

In order to develop smooth, bug-free applications, testing is a must. To accomplish this goal, many modern work-flows implement rigorous testing processes and emphasise concepts such as test-driven development[^1].
Testing code, however, isn't always as straight forward as some may think. There are many different ways to test code, including test processes that verify aspects of the code presented to the end user, and processes that focus on whether the code was executed as it was intended to.
One of the latter methods is unit testing[^2]. Unit testing aims to assess whether the developer wants code to run the way it runs by running the code against certain parameters, and checking the methods run against tests written by the developer.

Since the widespread adoption of unit testing, many developers have created third-party libraries that integrate unit-testing methods into various languages. A few newer languages even support unit-testing natively, such as the "D" language[^3]. In order to write unit tests when developing using the Java language, for example, when creating a complex Bukkit plugin or backend mechanism, a third party library must be used.
One of the most common unit-testing library for Java is JUnit[^4], developed in 1997 by Kent Beck and Erich Gamma. JUnit provides the ability to create tests that use annotations to run separate code before, during and after a test class is initialised. This allows developers to *chronologically* test whether their code is working correctly, as the tests complete.

JUnit, however, doesn't allow for the detailed and easy analysis of methods that were run during the tests. In a JUnit test, variables are checked using an assertion method or keyword that will check whether a variable is as it is intended to be. Using Mockito[^5], however, a 'mock' (pun intended) instance of a class can be created, allowing all sorts of checks to be run against that method.
This may not seem like a very powerful feature at first, however it can allow the developer to write tests that check the amount of times a method was run, what arguments were used, and can even allow a certain method to return a "simulated" value.
Furthermore, the features of JUnit and Mockito can be combined to allow the creation of chronological tests that analyse and assert methods in detail.
Mockito does have several drawbacks, however. When using annotations to "mock" classes, the annotations must be initialised by a constructor. This sometimes requires the creation of a "master-class", that tests would extend. Also, Mockito verifications must be reset after each test, if several tests are run consecutively, in order to stop the verification from impacting the next test.

When writing a test using Mockito and JUnit, the test would usually be structured similar to a regular chronological test, for example:

    String string = "hello";

    @Test
    public void test() {
        StringUtils.addComma(string);
    }

    @After
    public void validate() {
    assert string.equals("hello,");
    }


However, using Mockito, I could check whether the ```addComma()``` method was actually executed, and with which arguments.
I could then structure my test similar to the following:

    StringUtils stringUtils = mock(StringUtils.class);
    String string = "hello";

    @Test
    public void test() {
        stringUtils.addComma(string);
    }

    @After
    public void validate() {
    assert string.equals("hello,");
    
    // This verifies that stringUtils.addComma() was run
    // at least once with the argument "hello"
    verify(stringUtils, atLeastOnce()).addComma("hello");
    
    // This resets Mockito validation when executing several consecutive tests
    validateMockitoUsage();
    }

This way, i'm making sure that my code is both executing the method correctly and returning the correct value, which is very useful for making sure methods that involve complex logic aren't erroneous or broken.

*I hope you enjoyed this introduction to Mockito. If you have any questions, feel free to send me a [tweet](http://twitter.com/psgs00) or an [email](http://github.com/psgs).*
*Have a great day!*

If you've used unit testing frameworks such as EasyMock[^6] before, you may be interested in this article by [Szczepan Faber about Mockito syntax](http://monkeyisland.pl/2008/01/14/mockito/).

[^1]: [Test Driven Development - WikiPedia](http://en.wikipedia.org/wiki/Test-driven_development)
[^2]: [Unit Testing - WikiPedia](http://en.wikipedia.org/wiki/Unit_testing)
[^3]: [Unit Tests - D Language](http://dlang.org/unittest.html)
[^4]: [JUnit - WikiPedia](http://en.wikipedia.org/wiki/JUnit)
[^5]: [Mockito.org](http://mockito.org)
[^6]: [EasyMock.org](http://easymock.org/)