## VueJs Unit Testing Applied

This cook book is a collection of recipes applying unit testing to real world VueJs applications.

### Prerequisites
* Understanding of unit testing
* Experience with [VueJs](https://vuejs.org)
* Experience with [Vue-test-utils](https://vue-test-utils.vuejs.org/)

### Unit test libraries
[Vue-test-utils](https://vue-test-utils.vuejs.org/) is the core libary that enables unit tesing of Vue applications. Please follow the best practises documented on [Vue-test-utils](https://vue-test-utils.vuejs.org/) website.

[Vue-test-utils-helper](https://github.com/AmpleOrganics/vue-test-utils-helpers) library provides helper functions for mocking components, Vuex and Vue Router.

### Quick refresher on unit testing
> In computer programming, unit testing is a software testing method by which individual units of source code, sets of one or more computer program modules together with associated control data, usage procedures, and operating procedures, are tested to determine whether they are fit for use. [wikipedia](https://en.wikipedia.org/wiki/Unit_testing)

In VueJs world the units that we test are:
* Components (including Views)
* Directives
* Functions (filters, services, utilities)
* Store modules

### Table of contents
* [How to mount a component](/mounting.md)
* [Handling Vuex ](/vuex.md)
* [Handling Vue Router ](/router.md)
* [Project specific mount function](/unit-testing-setup.md)
* [Recipe: Unit test a component with no child components](/component-with-no-child-components.md)
* [Recipe: Unit test a component with child components](/component-with-child-components.md)
