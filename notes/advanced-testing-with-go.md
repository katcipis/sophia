# Advanced Testing with Go

The [talk](https://www.youtube.com/watch?v=yszygk1cpEc)

* Test in layers, all tests (unit/integration/e2e) are needed
* Terraform has 10x more test code than production code
* Table driven tests helps avoiding useless boilerplates
* Golden files to test with human checked correct data (I call it reference files)
* Test helpers, which are basically test fixtures
* A neat idea is to return the teardown function on the setup function
* Always try to test only exported functions (not a rule, but its a good principle)
* Subprocessing mocks reminded me of toxyproxy and some principles on Release It
* Provide testing public APIs for your packages, like functions that creates test config data
* Provide an easy way to run your service in memory (the full service, not only part of it)
