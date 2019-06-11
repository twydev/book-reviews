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

```
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


---
# Where should we refactor?

### Chapter 3

---
# How to refactor

### Chapter 4

---
# Guest Contributions

---
# Reference Catalog