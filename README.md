# dependency-diff

Find the differences between two versions of the same library.

###Calculate Differences
```java
JarContents testV1 = JarContents.load(new File("test-0.0.1-SNAPSHOT.jar"));
JarContents testV2 = JarContents.load(new File("test-0.0.2-SNAPSHOT.jar"));
Differences differences = testV1.difference(testV2);
```
Or: 
```java
import static uk.co.optimisticpanda.jarcompare.diff.Differences.differenceBetween;
...
Differences differences = differenceBetween("test-0.0.1-SNAPSHOT.jar")
										.and("test-0.0.2-SNAPSHOT.jar");
```

###Added/Removed Classes:

```java
@Test
public void checkClassChanges() {

	check(differences)

		.additionalClassesAre(
			"com.test.class1.NewClass",
			"com.test.class2.MovedClass",
			"com.test.class1.ClassWithNewNestedClass$NewNestedClass",
			"com.test.class1.ClassWithAdditionalNestedClassesAtSameLevel$NewNestedClassA",
			"com.test.class1.ClassWithAdditionalNestedClassesAtSameLevel$NewNestedClassB",
			"com.test.class1.ClassWithNewNestedNestedClass$NewNestedClass$NewNestedNestedClass",
			"com.test.class1.ClassWithTwoLevelsOfNestingAdded$NewNestedClass",
			"com.test.class1.ClassWithTwoLevelsOfNestingAdded$NewNestedClass$NewNestedNestedClass")

		.removedClassesAre(
			"com.test.class1.MovedClass",
			"com.test.class1.RemovedClass",
			"com.test.class1.RemovedNestedClass$NewNestedClass",
			"com.test.class1.RemovedNestedNestedClass$NewNestedClass$NewNestedNestedClass",
			"com.test.class1.ClassWithTwoLevelsOfNestingRemoved$NewNestedClass",
			"com.test.class1.ClassWithTwoLevelsOfNestingRemoved$NewNestedClass$NewNestedNestedClass");
}
```

###Class Modifiers:

```java
@Test
public void checkClassModifierChanges() throws IOException {

	check(differences)
		
		.classModifiersFor("com.test.classModifiers1.ModifierChangeClass")
			.were(PUBLIC, FINAL).now(PUBLIC).end()
		
		.classModifiersFor("com.test.classModifiers1.ModifierAndSubclassChangeClass")
			.were(PUBLIC).now(PUBLIC, FINAL)
			.subClassModifiersFor("com.test.classModifiers1.ModifierAndSubclassChangeClass$ModifierAndSubclassChangeSubClass")
				.were(PUBLIC).now(PACKAGE, STATIC, FINAL).end()
			.end();
}

```


##TODO
 - Classes
   - [X] New Classes
   - [X] Deleted Classes
   - [X] Class Modifier changed.
   - [ ] Child members changed.  
   - [ ] Change types class -> interface -> annotation  
   
 - Nested Classes
   - [X] New Classes
   - [X] Deleted Classes
   - [ ] Altered Classes (Child members altered or class modifiers changed)  
 
 - Constructors
   - [ ] New Constructors
   - [ ] Removed Constructors
   - [ ] Altered Constructors (Different params or modifiers)
 
 - Fields
   - [ ] New Fields
   - [ ] Removed Fields
   - [ ] Altered Fields (modifiers)

 - Static Fields
   - [ ] New Fields
   - [ ] Removed Fields
   - [ ] Altered Fields (modifiers)

 - Methods
   - [ ] New Methods
   - [ ] Removed Methods
   - [ ] Altered methods (signature or modifiers)

 - Static Methods
   - [ ] New Methods
   - [ ] Removed Methods
   - [ ] Altered methods (signature or modifiers)

 -  [ ] Altered Resources
 -  [ ] Detect moved members.           
 -  [ ] maven pom dependencies?

 
 ##Test cases
 
 - Class 
  - New class  (NewClass)
  - Removed class (RemovedClass)
  - Moved class (MovedClass)
  - new nested class (ClassWithNewNestedClass)
  - new nested nested class (ClassWithNewNestedNestedClass)
  - multiple nested clases at same level  (ClassWithAdditionalNestedClassesAtSameLevel)
  - single class to class with multiple nested classes (ClassWithTwoLevelsOfNestingAdded)
  - class with multiple nested classes to single class (ClassWithTwoLevelsOfNestingRemoved)
  - removed nested class (RemovedNestedClass)
  - removed nested nested class (RemovedNestedNestedClass)

 - Class Modifiers
  - Top level single class changes (ModifierChangeClass)
  - Top level class and nested class changes (ModifierChangeClass.java)
  - Top level class, nested class, nested nested class changes (NestedNestedModifierChangesClass)
  - Nested class changes and top isn't (NestedNestedModifierChangesClass).
  - Nested Nested class changes and top isn't. (NestedNestedClassModifierChangesClass)
 
 - class to interface / interface to class
 - class to enum / enum to class
  
 - Problems
  - Cannot detect StrictFP
