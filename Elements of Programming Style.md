[TOC]

# Writing Readable Code

## Coding Precision

### Naming  Vividly

#### Specific and Colorful Words 

Names should carry enough information to avoid empty. Also, names should be properly named to avoid ambiguity.

##### Specific Words to Avoid Empty

For example:

```java
for (int i; i <= corporation.size(); i++){
    for (int j; j <= department.size(); j++){
        for (int k; k <= group.size(); k++){
            if (corporation[i])
        }
    }
}
```

Another example:

```java

```



##### Colorful Words to Avoid Ambiguity

For example:

```java
public jsonObject getPage(String url){
    //some codes are written here
}
```

The method *getPage* is confusing: where the page is gotten, from local cache? from Internet? or even from a data base?

Another example:

```java
class BinaryTree {
    private int size;
    //some codes are written here
}
```

The private field *size* is confusing: maybe it means the height of the tree, or the number of nodes of the tree, or even the virtual memory space of the tree.

So, it is necessary to find colorful words to describe variables and methods. Here are some examples:

| Word  | Alternatives                                       |
| ----- | -------------------------------------------------- |
| send  | deliver, announce, distribute, route               |
| find  | search, extract, locate, recover                   |
| start | launch, begin, create, open                        |
| make  | create, set up, build, generate, compose, add, new |

##### Attach Units to Ensure Transformation

If your variables are measurements, you are recommended to attach units into the variable. 

For example:

```java
Long start_ms = new Date().getTime();
//Some codes here
Long elapsed_ms = new Date().getTime();
//Some codes here
System.out.println("Load time was" + (start_ms - elapsed_ms)/1000 + "seconds");
```

#### Length is not a Problem

##### Short Name for Short Scope

##### Long Name for Large Scope

### Adding Comments Compactly



## Coding Aesthetics

### Code is Work of Art

Your code is a piece of artwork. Just like an essay or image, you must pay enough attention to the style, blank in your code.  On the other hand, irregularity fill in each masterpiece, but not popular in our codes. To avoid irregularity, the first thing flowing into your mind when you program is to *make similar code look similar*. Since our brains tend to process information in terms of groups and hierachies, you can help a reader quickly digest your code by organizing it the following ways. 

#### Consistent Style

#### Organizing  Declarations into Groups

#### Breaking Methods into Paragraphs

#### Introducing Compact Line Breaks

# Refactoring

## Organizing Data

### Encapsulation

#### Encapsulate Field
##### Why Refactoring
One of the pillars of object-oriented programming is Encapsulation, without which any other objects could get and modify the data of your public object without any checks! Data encapsulation could ensure behaviors are not separated from data and make maintenance become easier.
##### Example
**Before**
```java
public class Person {
private String name;
}
```
**After**
```java
public class Person {
private String name;
//Tool in IDEA：Refactor —> Encapsulate Fields
public String getName(){
return name;
}
public void setName(String name){
this.name = name;
}
}
```
#### Self Encapsulate Field 
##### Why Refactoring


##### Example
**Before**

```java
public class Range {
private int floor,ceiling;
public boolean inRange(int arg){
return arg >= floor && arg <= ceiling;
}
}
```
**After**
```
public class Range {
private int floor;
private int ceiling;

public int getFloor() {
return floor;
}

public void setFloor(int floor) {
this.floor = floor;
}

public int getCeiling() {
return ceiling;
}

public void setCeiling(int ceiling) {
this.ceiling = ceiling;
}
//Tool in IDEA：Refactor —> Encapsulate Fields
public boolean inRange(int arg){
return arg >= getFloor() && arg <= getCeiling();
}

}
```
#### Encapsulate Collection
##### Why Refactoring
##### Example

**Before**

```java
import java.util.ArrayList;

public class Teacher {
private ArrayList<String> courses;

public ArrayList<String> getCourses() {
return courses;
}

public void setCourses(ArrayList<String> courses) {
this.courses = courses;
} 
}
```
**After**
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Teacher {
private ArrayList<String> courses;

public List<String> getCourses(){
return Collections.unmodifiableList(courses);
}
public void addCourses(String course){
courses.add(course);
}
public boolean removeCourses(String course){
return courses.remove(course);
}
}
```

### Value or Reference

#### Change Value to Reference
##### Why Refactoring


##### Example
**Before**
```java
public class Currency {
private String code;

public Currency(String code) {
this.code = code;
}

public String getCode() {
return code;
}
}
```
**After**
```java
public class Currency {
private String code;
//Make constructor private
private Currency(String code) {
this.code = code;
}

public String getCode() {
return code;
}
//Add factory method
public static Currency createCurrency(String code){
return new Currency(code);
}
}
```
Remember replace the constructor with factory method in other objects!
#### Change Reference to Value
##### Why Refactoring

##### Example
**Before**
```java
public class Currency {
private String code;

private Currency(String code) {
this.code = code;
}

public String getCode() {
return code;
}

public static Currency createCurrency(String code){
return new Currency(code);
}
}
```
**After**
```java
public class Currency {
private String code;

//Make constructor public
public Currency(String code) {
this.code = code;
}

public String getCode() {
return code;
}

@Override
public int hashCode() {
return code.hashCode();
}

@Override
public boolean equals(Object obj) {
if (! (obj instanceof Currency)) return false;
Currency compared = (Currency) obj;
return (code.equals(compared.code));
}
// Remove factory method
// public static Currency createCurrency(String code){
// return new Currency(code);
// }
}
```

### Data or Object
#### Replace Data Value with Object

##### Why Refactoring

##### Example

**Before**

```java

```

**After**

```java

```



#### Replace Array with Object

##### Why Refactoring

Arrays are an excellent tool for storing homogenious data, but not a good alternate for storing data containing information about an entity (such as name, address, telephone number to customer, although all of them belong to *String*). Therefore, by replacing array with an object, the fields of the class are much easier to document.

##### Example

**Before**

```java
String[] row = new String[2];
row[0] = "Liverpool";
row[1] = "15";
```

**After**

```java
Performance row = new Performance();
row.setName("Liverpool");
row.setWins("15");
```

### Unidirectional or Bidirectional

#### Change Bidirectional to Unidirectional

##### Why Refactoring

Bidirectional associations are much harder to implement and maintain than unidirectional ones. By changing it to unidirectional one, if you assure that one of the classes will not use the other's feature, can reduce dependency between classes and make them easier to maintain.

##### Example

**Before**

```java

```

**After**

```java

```

#### Change Unidirectional to Bidirectional

##### Why Refactoring

You have two classes that need to use features of he other during the process of development, but there only exists unidirectional association designed at the beginning. 

##### Example

**Before**

```java

```

**After**

```java

```

### Type Code is a Bad Choice

#### Replace Type Code with Class

##### Why Refactoring 



##### Example

**Before**

```java
public class Person {
public static final int O = 0;
public static final int A = 1;
public static final int B = 2;
public static final int AB = 3;
private int bloodGroup;

public int getBloodGroup() {
return bloodGroup;
}

public void setBloodGroup(int bloodGroup) {
this.bloodGroup = bloodGroup;
}

public Person(int bloodGroup) {
this.bloodGroup = bloodGroup;
}
}
```

**After**

```java
public class Person {
public static final int O = BloodGroup.O.getCode();
public static final int A = BloodGroup.A.getCode();
public static final int B = BloodGroup.B.getCode();
public static final int AB = BloodGroup.AB.getCode();

private BloodGroup bloodGroup;

public BloodGroup getBloodGroup() {
return bloodGroup;
}

public Person(int bloodGroup) {
this.bloodGroup = BloodGroup.code(bloodGroup);
}
}

class BloodGroup {
public static final BloodGroup O = new BloodGroup(0);
public static final BloodGroup A = new BloodGroup(1);
public static final BloodGroup B = new BloodGroup(2);
public static final BloodGroup AB = new BloodGroup(3);
public static final BloodGroup[] group = {O,A,B,AB};

private final int code;

public BloodGroup(int code) {
this.code = code;
}

public int getCode() {
return code;
}

public static BloodGroup code(int arg){
return group[arg];
}
}
```

#### Replace Type Code with Subclass

##### Why Refactoring 



##### Example

**Before**

```java
public class Person {
public static final int ENGINEER = 0;
public static final int TEACHER = 1;
public static final int DOCTOR = 2;

private int type;

public Person(int type) {
this.type = type;
}

public int getType() {
return type;
}
}
```

**After**

```java
public class Person {
public static final int ENGINEER = 0;
public static final int TEACHER = 1;
public static final int DOCTOR = 2;

private int type;
//make constructor private
public Person(int type) {
this.type = type;
}
//add factory method
static Person create(int type){
switch (type) {
case ENGINEER:
return new Engineer();
case TEACHER:
return new Teacher();
case DOCTOR:
return new Doctor();
default:
throw new IllegalArgumentException("Incorrect Type!!!");
}
}

public int getType() {
return type;
}
}

class Engineer extends Person {
int getType(){
return Person.ENGINEER;
}
}

//the same formal subclass for Teacher and Doctor...
```

#### Replace Type Code with Strategy

##### Why Refactoring 



##### Example

**Before**

```java

```

**After**

```java

```

## Simplifying Conditional Expression

### Shorten Conditional Expression

#### Decompose Conditional

##### Why Refactoring

Long pieces of codes are hard to read and understand.  Things get even worse when the code block is filled with *if-else* or *switch*.  Therefore, extracting long conditional expression to short explicit and descriptive method could make it easier for other person who may maintain the code later.

##### Example

**Before**

```java
if (date.before(SUMMER_START) || date.after(SUMMER_END)) {
  charge = quantity * winterRate + winterServiceCharge;
}
else {
  charge = quantity * summerRate;
}
```

**After**

```java
boolean isSummer(date) {
    return !date.before(SUMMER_START) || date.after(SUMMER_END)
}
double getWinterCharge() {
    return quantity * winterRate + winterServiceCharge;
}
double getSummerCharge() {
    return quantity * summerRate;
}

if (isSummer(date)) {
  charge = getSummerCharge();
}
else {
  charge = getWinterCharge();
}

```

#### Consolidate Duplicate Parts

##### Why Refactoring

Duplicate control flow codes and conditional fragments are perfect place for *Long Method* to hide.  By consolidating all operators, you can now isolate this complex expression in a new method with a name that explains the conditional’s purpose. Also, moving the duplicated parts from control flow to to the beginning or end of the branch, depending on whether it changes the result of the subsequent code, can achive great deduplication. 

##### Example

**Before**

```java
double disabilityAmount() {
  if (seniority < 2) {
    return 0;
  }
  if (monthsDisabled > 12) {
    return 0;
  }
  if (isPartTime) {
    return 0;
  }
  if (isSpecialDeal()) {
  total = price * 0.95;
  send();
  }
  else {
    total = price * 0.98;
    send();
  }
}

```

**After**

```java
double disabilityAmount() {
  if (isFree()) {
    return 0;
  }
  if (isSpecialDeal()) {
  total = price * 0.95;
  }
  else {
    total = price * 0.98;
  }
}
//Move the duplciated part to the end of the branch
send()
boolean isFree(){
    return seniority < 2 || monthsDisabled > 12 || isPartTime;
}
```

### Shorten Conditional Branches

#### Replace Nested Conditional with Guard Clauses

##### Why Refactoring

It is difficult to figure out what each conditional does and how,  since the “normal” flow of code execution is not immediately obvious.  

To simplify the situation, isolate the special cases into separate  conditions that immediately end execution and return a null value if the  guard clauses are true. In effect, your mission here is to make the  structure flat.

##### Example 

**Before**

```java
public double getPayAmount() {
  double result;
  if (isDead){
    result = deadAmount();
  }
  else {
    if (isSeparated){
      result = separatedAmount();
    }
    else {
      if (isRetired){
        result = retiredAmount();
      }
      else{
        result = normalPayAmount();
      }
    }
  }
  return result;
}
```

**After**

```java
public double getPayAmount() {
	if(isDead) return deadAmount();
    if(isSeparated) return separatedAmount();
    if(isRetired) return retiredAmount();
    return normalAmount();
}
```

#### Replace Conditional with Polymorphism

##### Why Refactoring

If a new object property or type appears, you will need to search for and add code in all similar conditionals.  What a tedious task!  Remember, instead of asking an object about its state and then performing actions based on this, it is much easier to simply tell the object what it needs to do and let it decide for itself how to do that. By replacing conditional expression with polymorphism, all you need to do is add a new subclass without touching the existing code (*Open/Closed Principle*) when you want to add a new execution variant.

##### Example

**Before**

```java
class Bird {
  private final type;  
  double getSpeed() {
    switch (type) {
      case EUROPEAN:
        return getBaseSpeed();
      case AFRICAN:
        return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
      case NORWEGIAN_BLUE:
        return (isNailed) ? 0 : getBaseSpeed(voltage);
    }
    throw new RuntimeException("Should be unreachable");
  }
}
```

**After**

```java
interface Bird {
    double getSpeed();
}
class EuropeanBird extends Bird {
    double getSpeed(){
        return getBaseSpeed();
    }
}
class AfricanBird extends Bird {
    double getSpeed(){
        return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
    }
}
class NorwegianBlueBird extends Bird {
    double getSpeed(){
        return (isNailed) ? 0 : getBaseSpeed(voltage);
    }
}

// Somewhere in client code
speed = bird.getSpeed();
```

## Simplifying Method Calls

#### Rename Method

##### Why Refactor
A bad name would be misleading and bing poor code readability. Maybe someone create it in a rush at the very begining, or perhaps its functionality changed gradually in the development process.
##### Example
**Before**

```java
public class Manager {
    private String officeAeraCode;
    private String officeNumber;
    
    public String getTelephoneNumber(){
        return officeAeraCode+officeNumber;
    }
}
```
**After**
```java
public class Manager {
    private String officeAeraCode;
    private String officeNumber;

    //Tool in IDEA:Refactor -> Rename
    public String getOfficeTelephoneNumber(){
        return officeAeraCode+officeNumber;
    }
}
```
#### Add/remove parameters
##### Why Refactoring
You need to make some changes to your method and these changes require adding or removing some data that was previously not avaliable/unnecessary.
##### Example
**Before**

```java
public class Manager {
    private String officeAeraCode;
    private String officeNumber;
    private String name;

    public String getOfficeTelephoneNumber(String department){
        return name + department + officeAeraCode + officeNumber;
    }
}
```
**After**
```java
public class Manager {
    private String officeAeraCode;
    private String officeNumber;
    private String name;
    
    //make the old method call the new one
    //it is recommended to inject a null value into the added parameters
    @Deprecated
    public String getOfficeTelephoneNumber(String department){
        return getOfficeTelephoneNumber(department,null);
    }
    // copy the old method to a new one and remain the name unchanged 
    public String getOfficeTelephoneNumber(String department,String office){
        return name + department + office + officeAeraCode + officeNumber;
    }
}
```
#### Preserve whole object
##### Why Refactoring
##### Example 
**Before**
```java

```


#### Introduce Parameter Object
##### Why Refactoring
Identical groups of parameters are often encountered in some methods, increasing the possiblity of long parameters and code duplication. By consolidating parameters in a single class, you can also find more benefits such as moving previous behavious into the created object. So remember moving behaviours and related operations, otherwise the bad smell of *Data Class* begins. 
##### Example
**Before**

```java
import java.util.Date;

public class Entry {
    private final Date chargeDate;
    private final double value;

    public Entry(Date chargeDate, double value) {
        this.chargeDate = chargeDate;
        this.value = value;
    }

    public Date getChargeDate() {
        return chargeDate;
    }

    public double getValue() {
        return value;
    }

    public calculate(Date start,Date end){
        if (getChargeDate() >= start && getChargeDate() <= end) return (end - getChargeDate())* getValue();
        if (getChargeDate() < start) return (end - start)*getValue();
        return 0;
    }
}
```
**After**
```java
import java.util.Date;

public class Entry {
    private final Date chargeDate;
    private final double value;

    public Entry(Date chargeDate, double value) {
        this.chargeDate = chargeDate;
        this.value = value;
    }

    public Date getChargeDate() {
        return chargeDate;
    }

    public double getValue() {
        return value;
    }
    //replace the identical parameters with object
    public calculate(DateRange dateRange){
        if (getChargeDate() >= dateRange.getStart() && getChargeDate() <= dateRange.getEnd()) return (dateRange.getEnd() - getChargeDate())* getValue();
        if (getChargeDate() < dateRange.getStart()) return (dateRange.getEnd() - dateRange.getStart())*getValue();
        return 0;
    }
}
class DateRange {
    private final Date start;
    private final Date end;

    public Date getStart() {
        return start;
    }

    public Date getEnd() {
        return end;
    }

    public DateRange(Date start, Date end) {
        this.start = start;
        this.end = end;
    }
}
```

# Unit Testing

## Why Unit Testing

### Benefits of Unit Testing

- Verify instantly

  Each time When you write a small bit of code, if you add a unit test that verifies the small change you added or made to the system, you can check the influence and find out whether you can move on without waiting long just by running unit tests.

- Escort refactoring

- Unit testing as documentation

### Criteria for Unit Testing

You can avoid many of the pitfalls that unit testers often drop into by following the FIRST principles of unit testing:

- [F]ast
- [I]solated
- [R]epeatable
- [S]elf-validating
- [T]imely

## Pragmatic Unit Testing

### Unit Testing Foundations

#### Basic Framework of a Test Class

A **test class**  always contains several **test methods** (annotated by @Test ) and **lifecycle methods** (annotated by @BeforeEach).

A standard test class just looks like this.

```java
import static org.junit.jupiter.api.Assertions.assertEquals;

import example.util.Calculator;

import org.junit.jupiter.api.Test;

class MyFirstJUnitJupiterTests {

    private final Calculator calculator = new Calculator();
	@BeforeEach
    void setUp() throws Exception{
    }
    @AfterEach
    void tearDown()throws Exception{
    }
    @Test
    void addition() {
        assertEquals(2, calculator.add(1, 1));
    }
}
```

#### Location of a Test Class

We all know that a test class is combine with one or more target production classes. Tests depend on the production-system code, but the dependency goes only in that direction. The production system has no knowledge of the tests. Unit testing is solely a programmer activity. No customers, end users, or non-programmers will typically see or run your tests.

 You need to decide where the tests go within your project. You have at least three options:

- Tests in same directory and package as production code

  You need to identify them by name (for example, Test*.class) or you need to write a bit of reflective code that identifies test classes. Keeping the tests in the same directory also bloats the number of files you must wade through in a directory listing.

- Tests in separate directory with package mirroring that of production code

   Each test ends up in the same package as the target class that it verifies. The test class can access package-level elements of the target class if necessary.

#### Annotations to Decorate a Test Class

Annotations for JUnit could be classified into 3 subclasses：

1. To describe a test method's function and parameters

   | Annotation           | Description                                                  |
   | -------------------- | :----------------------------------------------------------- |
   | `@Test`              | Denotes that a method is a test method. Unlike JUnit 4’s `@Test`  annotation, this annotation does not declare any attributes, since test  extensions in JUnit Jupiter operate based on their own dedicated  annotations. Such methods are *inherited* unless they are *overridden*. |
   | `@ParameterizedTest` | Denotes that a method is a [parameterized test](https://junit.org/junit5/docs/current/user-guide/#writing-tests-parameterized-tests). Such methods are *inherited* unless they are *overridden*. |
   | `@RepeatedTest`      | Denotes that a method is a test template for a [repeated test](https://junit.org/junit5/docs/current/user-guide/#writing-tests-repeated-tests). Such methods are *inherited* unless they are *overridden*. |
   | `@TestFactory`       | Denotes that a method is a test factory for [dynamic tests](https://junit.org/junit5/docs/current/user-guide/#writing-tests-dynamic-tests). Such methods are *inherited* unless they are *overridden*. |
   | `@TestTemplate`      | Denotes that a method is a [template for test cases](https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-templates) designed to be invoked multiple times depending on the number of invocation contexts returned by the registered [providers](https://junit.org/junit5/docs/current/user-guide/#extensions-test-templates). Such methods are *inherited* unless they are *overridden*. |
   | `@TestMethodOrder`   | Used to configure the [test method execution order](https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-execution-order) for the annotated test class; similar to JUnit 4’s `@FixMethodOrder`. Such annotations are *inherited*. |
   | `@TestInstance`      | Used to configure the [test instance lifecycle](https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-instance-lifecycle) for the annotated test class. Such annotations are *inherited*. |

2. to describe a method's lifecycle

   | Annotation    | Description                                                  |
   | ------------- | ------------------------------------------------------------ |
   | `@BeforeEach` | Denotes that the annotated method should be executed *before* **each** `@Test`, `@RepeatedTest`, `@ParameterizedTest`, or `@TestFactory` method in the current class; analogous to JUnit 4’s `@Before`. Such methods are *inherited* unless they are *overridden*. |
   | `@AfterEach`  | Denotes that the annotated method should be executed *after* **each** `@Test`, `@RepeatedTest`, `@ParameterizedTest`, or `@TestFactory` method in the current class; analogous to JUnit 4’s `@After`. Such methods are *inherited* unless they are *overridden*. |
   | `@BeforeAll`  | Denotes that the annotated method should be executed *before* **all** `@Test`, `@RepeatedTest`, `@ParameterizedTest`, and `@TestFactory` methods in the current class; analogous to JUnit 4’s `@BeforeClass`. Such methods are *inherited* (unless they are *hidden* or *overridden*) and must be `static` (unless the "per-class" [test instance lifecycle](https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-instance-lifecycle) is used). |
   | `@AfterAll`   | Denotes that the annotated method should be executed *after* **all** `@Test`, `@RepeatedTest`, `@ParameterizedTest`, and `@TestFactory` methods in the current class; analogous to JUnit 4’s `@AfterClass`. Such methods are *inherited* (unless they are *hidden* or *overridden*) and must be `static` (unless the "per-class" [test instance lifecycle](https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-instance-lifecycle) is used). |

3. Other additional functions

   | Annotation           | Description                                                  |
   | -------------------- | ------------------------------------------------------------ |
   | `@Nested`            | Denotes that the annotated class is a non-static [nested test class](https://junit.org/junit5/docs/current/user-guide/#writing-tests-nested). `@BeforeAll` and `@AfterAll` methods cannot be used directly in a `@Nested` test class unless the "per-class" [test instance lifecycle](https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-instance-lifecycle) is used. Such annotations are not *inherited*. |
   | `@Tag`               | Used to declare [tags for filtering tests](https://junit.org/junit5/docs/current/user-guide/#writing-tests-tagging-and-filtering), either at the class or method level; analogous to test groups in TestNG or Categories in JUnit 4. Such annotations are *inherited* at the class level but not at the method level. |
   | `@Disabled`          | Used to [disable](https://junit.org/junit5/docs/current/user-guide/#writing-tests-disabling) a test class or test method; analogous to JUnit 4’s `@Ignore`. Such annotations are not *inherited*. |
   | `@Timeout`           | Used  to fail a test, test factory, test template, or lifecycle method if its  execution exceeds a given duration. Such annotations are *inherited*. |
   | `@ExtendWith`        | Used to [register extensions declaratively](https://junit.org/junit5/docs/current/user-guide/#extensions-registration-declarative). Such annotations are *inherited*. |
   | `@RegisterExtension` | Used to [register extensions programmatically](https://junit.org/junit5/docs/current/user-guide/#extensions-registration-programmatic) via fields. Such fields are *inherited* unless they are *shadowed*. |
   | `@TempDir`           | Used to supply a [temporary directory](https://junit.org/junit5/docs/current/user-guide/#writing-tests-built-in-extensions-TempDirectory) via field injection or parameter injection in a lifecycle method or test method; located in the `org.junit.jupiter.api.io` package. |

#### Name Test Methods Properly

 Reasonable test names probably consist of up to seven or so words. Longer names quickly become dense sentences that are tough to swallow. If many of your test names are long, it might be a hint that your design is amiss.
The cooler, more descriptive names all follow the form:
*doingSomeOperationGeneratesSomeResult*
You might also use a slightly different form such as:
*someResultOccursUnderSomeCondition*
Or you might decide to go with the given-when-then naming pattern, which
derives from a process known as behavior-driven development:
*givenSomeContextWhenDoingSomeBehaviorThenSomeResultOccurs*

### Organizing Unit Tests

#### Keep Unit Tests Consistent with AAA

#### Test Behaviors not Test Methods

#### Common Initialization and Cleanup

## Unit Testing Challenge

### What is Mock

### How to Mock

#### Prerequisites

There are several frameworks for mocking your objects, such as *powermock*, *easymock* and *mockito*. *Mockito* is recommended in this tourial since it is easier to learn and more popular (you can check its stars in Github).

Before using this framework, you should add it as dependency on your project.

If you use Gradle in a Java project, add the following dependency to the Gradle build file.

```groovy
repositories { jcenter() }
dependencies { testImplementation 'org.mockito:mockito-core:2.7.22' }
```

If you use Maven in a Java project, add the following dependency to the Maven build file.

```xml
		<dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-all</artifactId>
            <version>${mokito.version}</version>
            <scope>test</scope>
		</dependency>
```

#### Creating Your Mocks/Spies

##### Creating Mocks/Spies in Code

**NOTE: By adding the org.mokito.Mokito.* as an static import,  you can use methods directly and improve the readablity of your test code.**

```java

```

##### Creating Mocks/Spies with Annotations

```java

```

##### Creating Mocks/Spies of Final Classes



##### Creating Mocks/Spies with Default Answers



#### Stubbing Behaviors of Mocks/Spies

##### Stubbing Methods with Return Values

1. "When thenReturn" and "When thenThrow" and "When thenAnswer"

![whenThenReturn10](https://www.vogella.com/tutorials/Mockito/img/xwhenThenReturn10.png.pagespeed.ic._WMKaUwdIq.png)

Mocks can return different values depending on arguments passed into a method. 

The when(….).thenReturn(….) method chain is used to specify a return value ( result ) for a method call with predefined parameters.

The when(….).thenThrow(….) method chain is used to specify a throwable exception  ( result )  for a method call with predefined parameters.

The when(….).thenAnswer(….) method chain is used to invoke additional actions (behaviors) for a method call with predefined parameters.

```java
import static org.mockito.Mockito.*;
import static org.junit.Assert.*;

@Test
public void test1()  {
        //  create mock
        MyClass test = mock(MyClass.class);

        // define return value for method getUniqueId()
        when(test.getUniqueId()).thenReturn(43);

        // use mock in test....
        assertEquals(test.getUniqueId(), 43);
}
// demonstrates the return of multiple values
@Test
public void testMoreThanOneReturnValue()  {
        Iterator<String> i= mock(Iterator.class);
        when(i.next()).thenReturn("Mockito").thenReturn("rocks");
        String result= i.next()+" "+i.next();
        //assert
        assertEquals("Mockito rocks", result);
}
// this test demonstrates how to return values based on the input
@Test
public void testReturnValueDependentOnMethodParameter()  {
        Comparable<String> c= mock(Comparable.class);
        when(c.compareTo("Mockito")).thenReturn(1);
        when(c.compareTo("Eclipse")).thenReturn(2);
        //assert
        assertEquals(1, c.compareTo("Mockito"));
}
// this test demonstrates how to return values independent of the input value

@Test
public void testReturnValueInDependentOnMethodParameter()  {
        Comparable<Integer> c= mock(Comparable.class);
        when(c.compareTo(anyInt())).thenReturn(-1);
        //assert
        assertEquals(-1, c.compareTo(9));
}
// return a value based on the type of the provide parameter

@Test
public void testReturnValueInDependentOnMethodParameter2()  {
        Comparable<Todo> c= mock(Comparable.class);
        when(c.compareTo(isA(Todo.class))).thenReturn(0);
        //assert
        assertEquals(0, c.compareTo(new Todo(1)));
}
```

2. "DoReturn When" and "DoThrow When" and "DoAnswer When"

Just like when(….).thenReturn(….) method chain, the DoReturn(...).when(...) method chain is also used to specify a return value for a method call with predefined parameters.

```java

```

##### Stubbing Methods to Throw Exceptions

##### Stubbing Methods with Answers

##### Stubbing Methods to Call Real Methods

##### Stubbing Final Methods

##### Stubbing Static Methods

#### Verifying Test Doubles/Behaviors

##### Verifying Behaviors

##### Verifying Results

## Refactoring Unit Tests

## Integration Testing

An integration test is done to demonstrate that different pieces of the system work together. Integration tests cover whole applications, and they require much more effort to put together. They usually require 
resources like database instances and hardware to be allocated for them. The integration tests do a more convincing job of demonstrating the system works (especially to non-programmers) than a set of unit tests can, at least to the extent the integration test environment resembles production.

## Writing Testable Codes

### Integrating Spring TestContext



```java
@ExtendWith(SpringExtension.class)
@SpringBootTest
public class MyAppTest {
  //some code here
}
```



### Testing the Database CRUD

#### You Need a Helper



### Testing with a Mock Environment

By default, `@SpringBootTest` does not start the server. If you have web endpoints that you want to test against this mock environment, you can additionally configure @MockMvc as shown in the following example:

```java
import tdd.customer.domain.model.Customer;
import tdd.customer.share.testhelper.JdbcHelper;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.MediaType;
import org.springframework.test.context.junit.jupiter.SpringExtension;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultHandlers.print;

import static org.assertj.core.api.Assertions.*;

import static tdd.customer.share.testhelper.Utils.*;

@ExtendWith(SpringExtension.class)
@SpringBootTest
@AutoConfigureMockMvc
class CustomerControllerJdbcIT {

    @Autowired
    private MockMvc mockMvc;

    @Autowired
    private JdbcHelper helper;

    @BeforeEach
    void initDb() {
        helper.clearCustomerTable();
    }

    @Test
    void create_couldCreateCustomer() throws Exception {

        Customer expected = new Customer("Simth", "Jason");

        Long createdId = Long.valueOf(
                mockMvc.perform(
                        post("/api/customers")
                                .contentType(MediaType.APPLICATION_JSON)
                                .content(asJsonString(expected)))
                        .andDo(print())
                        .andReturn()
                        .getResponse()
                        .getContentAsString()
        );

        Customer actual = helper.findCustomerFromDbById(createdId);

        assertThat(actual).isEqualToComparingOnlyGivenFields(
                expected
                , "firstName"
                , "lastName");
    }

    @Test
    void findById_CouldFindCertainCustomer() throws Exception {
        String expected = asJsonString(
                helper.createCustomerInDb(
                        1L, "Andrew", "Jordan")
        );

        String actual = mockMvc
                .perform(get("/api/customers/1"))
                .andDo(print())
                .andReturn()
                .getResponse()
                .getContentAsString();

        assertJson(actual, expected);
    }

    @Test
    void findAll_couldFindAllCustomer() throws Exception {

        Customer Smith = helper.createCustomerInDb(1L, "Simth", "Jason");
        Customer Jordan = helper.createCustomerInDb(2L, "Andrew", "Jordan");

        String expected =asJsonString(new Customer[]{Smith, Jordan});

        String actual = mockMvc
                .perform(get("/api/customers"))
                .andDo(print())
                .andReturn()
                .getResponse()
                .getContentAsString();

        assertJson(actual, expected);
    }
}
```



## Reference

1. https://www.vogella.com/tutorials/Mockito/article.html
2. 