the more important it is for your team to follow a reliable testing workflow.
## **iOS Testing**

[[unit testing]] and [[integration testing]].

### Best practice
- Use [[code coverage tools]]
- [[Automate test]]
- Test all multiple devices (old -> high end devices)

### Test coverge
> the process you use to determine whether you’re testing everything you’re supposed to test.

### Code coverge
> is a metric related to unit testing. The idea is to measure *the percentage of lines and execution paths* in the code covered by at least one test case

- **Line coverge** -> <mark style="background: #FF5582A6;">you can have 100% line coverage but still have untested scenarios in your application</mark>

### Test driven development
> focuses on writing automated tests before implementing the actual code

TDD involves three primary steps: 
- writing a *failing test*
- implementing the **minimum code** necessary to *pass the test*
- and finally refactoring the code while ensuring that *all tests pass*
#### Benifit
**Higher Code Coverage** -> blind spot
 **Early Bug Detection:** -> avoid fix bug in stg or production
 