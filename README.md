### Function Form for Terminating Assertion Properties

The below list of assertions are property getters that assert immediately on access. Because of that, they were written to be used by terminating the assertion chain with a property access.

    expect(true).to.be.true;
    foo.should.be.ok;

This syntax is definitely aesthetically pleasing but, if you are linting your test code, your linter will complain with an error something like "Expected an assignment or function call and instead saw an expression." Since the linter doesn't know about the property getter it assumes this line has no side-effects, and throws a warning in case you made a mistake.

Squelching these errors is not a good solution as test code is getting to be just as important as, if not more than, production code. Catching syntactical errors in tests using static analysis is a great tool to help make sure that your tests are well-defined and free of typos.

A better option was to provide a function-call form for these assertions so that the code's intent is more clear and the linters stop complaining about something looking off. This form is added in addition to the existing property access form and does not impact existing test code.

    expect(true).to.be.true();
    foo.should.be.ok();

These forms can also be mixed, but the chain must always be terminated in the function form or assertions up to that point in the chain will not execute.

    expect(true).to.be.true.and.not.false();
    expect(true).to.be.true().and.not.false();

#### Custom Error Messages

With this function form for terminating properties you can also provide custom error messages to show when the assertion fails. This works whether the assertion is somewhere mid-chain or at the end.

    expect(true).to.be.true.and.not.false('Reason: Paradox');
    expect(true).to.be.true('The fabric of logic has torn').and.not.false();

#### Affected Assertions

The following assertions are modified by this plugin to now use the function-call form:

* ok
* true
* false
* null
* undefined
* exist
* empty
* arguments
* Arguments

#### Caveats

This breaks both the `length` and `arguments` asserts when they are in the chain following one of these. To work around this limitation, do the `length` or `arguments` asserts early in the chain or just do two lines of asserts.

#### Other Plugins

This will unfortunately not apply to any other Chai plugins that use the property syntax. In order to work around the issue of lint errors when using other plugins, this plugin also provides an `ensure` function that basically does nothing but looks like a function on the end of your chain.

    spy.should.have.been.calledOnce.ensure();

