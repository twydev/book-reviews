# Refactoring
*Improving the Design of Existing Code*

*Martin Fowler, Kent Beck (contributor), John Brant (contributor), William Opdyke, Don Roberts, 2002*

---
### Notes
This post is a quick reminder to myself, describing what I have learnt from the book.

I encourage you to pick up the actual book and read it because the example codes published in the book help to illustrate the key concepts of refactoring, which cannot be entirely conveyed through summary notes.

---
# What is refactoring and why do we refactor codes?

### Chapter 1: Refactoring, A First Example
This chapter showcase a small program that is part of a larger software, responsible for computing rental charges and printing customer statements. The initial version of the program cannot easily incorporate new features or changes to business logic.

**Approach**

1. **Refactor first, extend later**. If the code is not structured in a convenient way to add new changes, refactor it first, before making the changes. The extra effort is worth it in the long run.
2. **Set up tests**. Establish a set of tests to ensure that refactored code generates the same result as the existing code.
3. **Incremental small changes**. Incrementally make small changes and test often, to catch bugs more easily.
4. **Draw a simple UML** for the current program and update it as you make changes help to clarify your thoughts.
5. **Decompose large chunks** of code into smaller methods.
6. **Rename variables** and methods to make your code self-documenting.
7. **Replace temporary variables** with queries (e.g. getters). (Temp variables are tpically problematic, causing too many parameters to be passed around. Problem worsens with long methods.)
8. **Make reusable**. Queries are reusable by the class. Changes in logic only need to be made in one place, which is the query implementation.
9. **Tradeoff between extensible code and optimised code**. Worry about optimisation later. Refactor to make the code easier to maintain is the top priority.
10. **Replacing conditionals with polymorphism**. If the different conditions are subclasses of a main interface, then overriding methods in each subclasses will provide you the desired conditional logic while keeping the code easy to read and maintain.

```Java
// Pseudo code in main object

class Main {
	method businessLogic() {
		if (condition A) {
			doSomethingA();
		} else if (condition B) {
			doSomethingB();
		}
	}
}

// Refactor using polymorphism

class Category {
	interface doSomething();
}

class CatA extends Category {
	method doSomething() {
		/* do something for condition A */
	}
}

class CatB extends Category {
	method doSomething() {
		/* do something for condition B */
	}
}

class Main {

	Category cat;
	/* in constructor create CatA or CatB according to logic */
	
	method businessLogic() {
		cat.doSomething(); // the correct override method will be resolved.
	}
}
```

8. **State Pattern.** *Referenced from Design Patterns by Gang of Four*. If a main object needs to change its behaviour according to changes in its internal state, the behaviour should not be implemented in this main object class.
	- Define seperate object classes that encapsulate state-specific behaviour. The main object class will call these seperate objects to perform the required functions.
	- If states are described by seperate state objects, those behaviour can also be implemented in state objects, and main object will call them to perform the reuqired functions.
9. **Rhythm of Refectoring**: build test > small changes > run test > small changes > run test > ...


### Chapter 2: Principles in Refactoring
This chapter talks about the definitions, benefits, and characteristics of refactoring.

> Refactoring = make software easier to understand and modify, without changing the observable behavior of the software.

You should be aware of which development activities you are performing at any point in time, and never mix the two up:
1. Add function to software - characterised by adding new tests, but no changes to existing code.
2. Refactoring - characterised by only using existing tests, and restructuring existing code.

Why some programs are hard to modify and work with:
- codes are hard to read
- codes have duplicated logic
- codes use complex conditional logic
- codes require additional behavior outside of the program

Benefits of Refactoring:
- Improves design of software
- Makes software easier to understand
- Identify bugs
- Speed up development as it is easier to add new functions to refactored code

Indirection (a.k.a dereferencing) = ability to act on a value through its name, container, reference and etc. instead of directly on the value itself.
- Enables sharing of logic
- Explains intention and implementation separately (self documenting through method and variable naming)
- Isolate changes (implement changes through subclasses)
- Encode conditional logic (via polymorphism)

However, indirection have costs. If there are any intermediate methods or components that used to serve a purpose or are supposed to be polymorphic but has become redundant, they should be taken out.

When to refactor:
- You have identified duplication of codes
- You want to add a new feature
- You need to fix a bug (it is likely that the code was not clear enough for you to spot the bug)
- You are doing code review (delivers more value when everyone can understand the code fully)

Potential problems of refactoring:
- **Database changes** are often hard to manage, especially schema changes. Having an intermediate layer of abstraction to between your program and the database helps minimise modification needed.
- **Interface changes**. All instances of interface needs to be changed if method signature changes. This is especially problematic if interface is published and some interface users are not accessible. You may mark the old interface as deprecated and let the old interface call the new interface. Understanding the trouble, it is better to not publish interfaces prematurely.

When not to refactor:
- when existing code is not even stable enough to work and effort is better spent rewriting the code.
- when you are super close to a deadline. Otherwise, you should always be refactoring.

Refactoring and Design:
- **Speculative design** = an attempt to put all good qualities into a system before any code is written. Like a waterfall model, this process is too easy to guess wrong. Refactoring along the way plug the holes in the initial software design. You will have less pressure to get things right on the first try.
- **Flexible programs** are often more complex to provide flexibility. Instead, implement a simple design that is easy to refactor to a flexible one if there ever is a need.

Refactoring and Performance:
- Well factored code allows changes more quickly, making your efforts to optimise the program more productive.
- Well factored code provides finer granularity for performance analysis.
- Focus on refactoring first, and then only optimise the code after having performance profiling. This will significantly reduce wasted effort to optimise portions of codes that only run once.


---
# Where should we refactor?

### Chapter 3: Bad Smells in Code

---
# How to refactor

### Chapter 4

---
# Guest Contributions

---
# Reference Catalog