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

## Reference



# Refactoring

## Composing Your Methods

### Extract or Inline

#### Extract Method
##### Why Refactor
##### Example
**Before**

```java
static void printTotalPrice(){
    ArrayList<Double> unitPrice = new ArrayList<Double>();
    unitPrice.add(2.8);
    unitPrice.add(3.5);
    unitPrice.add(4.0);
    double totalPrice = 0.0;

    //print head
    System.out.println("************************");
    System.out.println("****Print Total Price***");
    System.out.println("************************");

    // calculate total price
    for (Double unit : unitPrice){
        totalPrice += unit;
    }
    //print total price
    System.out.println("The total price is "+ totalPrice);
}
```
**After**
Step1:
```java
static void printTotalPrice(){
    ArrayList<Double> unitPrice = new ArrayList<Double>();
    unitPrice.add(2.8);
    unitPrice.add(3.5);
    unitPrice.add(4.0);
    double totalPrice = 0.0;

    //print head
    printHead();

    // calculate total price
    for (Double unit : unitPrice){
        totalPrice += unit;
    }
    //print total price
    System.out.println("The total price is "+ totalPrice);
}

//No local variable
//Solution: just extract the block to a new method
private static void printHead() {
    System.out.println("************************");
    System.out.println("****Print Total Price***");
    System.out.println("************************");
}
```
Step2:
```java
static void printTotalPrice(){
    ArrayList<Double> unitPrice = new ArrayList<Double>();
    unitPrice.add(2.8);
    unitPrice.add(3.5);
    unitPrice.add(4.0);
    double totalPrice = 0.0;

    //print head
    printHead();

    // calculate total price
    for (Double unit : unitPrice){
        totalPrice += unit;
    }
    //print total price
    printTotalPrice(totalPrice);
}

private static void printHead() {
    System.out.println("************************");
    System.out.println("****Print Total Price***");
    System.out.println("************************");
}

// the method only read the local variable
// solution: put the local variable into parameter list
static void printTotalPrice(double totalPrice) {
    System.out.println("The total price is "+ totalPrice);
}
```
Step3:
```java
static void printTotalPrice(){
    ArrayList<Double> unitPrice = new ArrayList<Double>();
    unitPrice.add(2.8);
    unitPrice.add(3.5);
    unitPrice.add(4.0);
    double totalPrice = 0.0;

    //print head
    printHead();

    // calculate total price
    totalPrice = calculateTotalPrice(unitPrice);
    //print total price
    printTotalPrice(totalPrice);
}

private static void printHead() {
    System.out.println("************************");
    System.out.println("****Print Total Price***");
    System.out.println("************************");
}

private static void printTotalPrice(double totalPrice) {
    System.out.println("The total price is "+ totalPrice);
}

//第三种情况是局部变量有赋值
//该情况又分为三种
//其一是这个变量仅在被提炼代码块中使用
//解决方法：将临时变量的声明移入代码块
//其二是这个变量在被提代码块之后未被使用
//解决方法：直接在被提函数中修改该变量即可
//其三是这个变量在被提代码块之后还有使用
//解决方法：让目标函数返回该变量改变后的值
private static double calculateTotalPrice(ArrayList<Double> unitPrice) {
    double result = 0.0;
    for (Double unit : unitPrice){
        result += unit;
    }
    return result;
}
```
#### Inline Method


###  Small Temp makes Big Mistake
#### Replace Temp with Query
##### Why Refactoring


##### Example
**Before**
```java
public class TempToQuery {
    final static int QUANTITY = 10;
    final static double ITEMPRICE = 2.5;
    public static void main(String[] args){
        System.out.println(getPrice());
    }
    static double getPrice(){
        double basePrice = QUANTITY * ITEMPRICE;
        double discountFactor;
        if (basePrice > 1000) {
            discountFactor = 0.95;
        }
        else {
            discountFactor = 0.98;
        }
        return basePrice * discountFactor;
    }
}
```
**After**
```java
public class TempToQuery {
    final static int QUANTITY = 10;
    final static double ITEMPRICE = 2.5;
    public static void main(String[] args){
        System.out.println(getPrice());
    }
    static double getPrice(){
        double discountFactor = DiscountFactor();
        return BasePrice() * discountFactor;
    }

    private static double BasePrice() {
        return QUANTITY * ITEMPRICE;
    }

    // we must use inline temp during replacing temp with query
    private static double DiscountFactor() {
        return BasePrice() > 1000 ? 0.95 : 0.98;
    }
}
```

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

## Dealing with Generalization

### Home for Field

#### Pull Up Field
##### why Refactoring
##### Example
**Before**

```java

public class Unit {
    private int Id;

    public Unit(int id) {
        Id = id;
    }

    public int getId() {
        return Id;
    }

    public void setId(int id) {
        Id = id;
    }
}

class Soldier extends Unit {
    String weapon;

    public Soldier(int id, String weapon) {
        super(id);
        this.weapon = weapon;
    }
}

class Tank extends Unit {
    String weapon;

    public Tank(int id, String weapon) {
        super(id);
        this.weapon = weapon;
    }
}
```
**After**
```java
public class Unit {
    //tool in IDEA: Refactor -> pull members up
    private String weapon;
    private int Id;

    public Unit(int id, String weapon) {
        Id = id;
        this.weapon = weapon;
    }

    public int getId() {
        return Id;
    }

    public void setId(int id) {
        Id = id;
    }

    public String getWeapon() {
        return weapon;
    }

    public void setWeapon(String weapon) {
        this.weapon = weapon;
    }
}

class Soldier extends Unit {

    public Soldier(int id, String weapon) {
        super(id, weapon);
    }
}

class Tank extends Unit {

    public Tank(int id, String weapon) {
        super(id, weapon);
    }
}
```
#### Push Down Field
##### why Refactoring
##### Example
**Before**
```java


```
**After**
```java


```
### Home for Method
#### Pull Up Method
##### Why Refactoring
##### Example
**Before**
```java


```
**After**
```java


```
#### Push Down Method
##### why Refactoring
##### Example
**Before**
```java


```
**After**
```java


```
### Extract Generality
#### Extract Subclass
##### why Refactoring
##### Example
**Before**
```java


```
**After**
```java


```
#### Extract Superclass
##### why Refactoring
##### Example
**Before**
```java


```
**After**
```java


```
#### Extract Interface
##### why Refactoring
##### Example
**Before**
```java


```
**After**
```java


```

## Reference



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



## Writing Testable Codes



## Reference

1. https://www.vogella.com/tutorials/Mockito/article.html

# Design Pattern

## Creational Patterns

Java supplies default constructors and default constructor behavior in some cases :

- If a class has no declared constructor, Java will supply a default one (a public constructor with no argument);
- If a constructor declaration does not use a variation of this() or super() to explicitly invoke another constructor, Java effectively inserts super() with no arguments;
- You can only invoke constructor to instantiate by *new* operator or by reflection;
- constructors are effective only if the user of your class knows which class to instantiate and knows the required fields for instantiating an object. 

### Factory Method

**Factory Method** is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

*Comparison between several definitions*

- **Static creation method** is a creation method declared as `static`. In other words, it can be called on a class and doesn’t require an object to be created.
- The **Factory Method Pattern **is a creational design pattern that provides an interface for creating objects but allows subclasses to  alter the type of an object that will be created.
- The **Simple factory** pattern  (also known as parameterized factory method )describes a class that has one creation method with a large conditional that based on method parameters chooses which product class to instantiate and then return.
- The **Abstract Factory**  is a creational design pattern that allows producing families of related or dependent objects without specifying their concrete classes.

#### Application Scenario

Imagine that youare creating an application both for Windows operating system. The first version of your app GUI works well. After a while, you app becomes pretty popular so you must make your app GUI work in MacOS.

How about the code? At present, most of your code is coupled to the Win styled classes. Adding Mac sytled classes into the app would require making changes to the entire codebase. Copying the code to make another Mac version is really a big task ! Maybe one day you need a Linux version, probably you eed to make all of these changes again.

The Factory Method pattern suggests that you replace direct object construction calls (using the new operator) with calls to a special factory method. Don’t worry: the objects are still created via the new operator, but it’s being called from within the factory method. Objects returned by a factory method are often referred to as “products.”

#### Structure

![The structure of the Factory Method pattern](https://refactoring.guru/images/patterns/diagrams/factory-method/structure.png)

#### Sample Code

*buttons/Button.java*

```java
public interface Button {
    void click();
}
```

*buttons/WinButton.java*

```java
public WinButton implements Button {
    @Override
    void click(){System.out.println("this is a win button!")};
}
```

*buttons/MacButton.java*

```java
public MacButton implements Button {
    @Override
    void click(){System.out.println("this is a mac button!")};
}
```

*factory/GUIfactory.java*

```java
public interface GUIfactory {
    Button createButton();
}
```

factory/WinFactory.java*

```java
public WinFactory implements GUIfactory {
    @Override
    Button createButton(){
        return new WinButton();
    }
}
```

*factory/MacFactory.java*

```java
public MacFactory implements GUIfactory {
    @Override
    Button createButton(){
        return new MacButton();
    }
}
```

*Application.java*

```java
public Application　{
    private Button button;
    
    public Application (GUIfactory factory){
        this.button = factory.createButton();
    }
    
    public void render(){
        this.button.click();
    }
}
```

*Client.java*

```java
public class Demo {
    /**
     * Application picks the factory type and creates it in run time (usually at
     * initialization stage), depending on the configuration or environment
     * variables.
     */
    private static Application configureApplication() {
        Application app;
        GUIFactory factory;
        String osName = System.getProperty("os.name").toLowerCase();
        if (osName.contains("mac")) {
            factory = new MacOSFactory();
            app = new Application(factory);
        } else {
            factory = new WindowsFactory();
            app = new Application(factory);
        }
        return app;
    }

    public static void main(String[] args) {
        Application app = configureApplication();
        app.render();
    }
}
```

### Abstract Factory

**Abstract Factory** is a creational design pattern that lets you produce families of related objects without specifying their concrete classes.

#### Application Scenario

Imagine that you are faced with a more complex situation. Button can not represent the whole GUI of your application, you must include inputbox, checkbox and other labels. Obviously, the former factory pattern could not work efficiently to solve this problem. 

**Abstract Factory** pattern suggests explicitly declaring interfaces for each distinct product of the product family (e.g., button, checkbox, inputbox). Then you can make all variants of products follow those interfaces. For example, all button variants can implement the `Button` interface. Next step is to declare the *Abstract Factory*—an interface with a list of creation methods for all products that are part of the product family (for example, `createButton`, `createInpuBox` and `createChackBox`). These methods must return **abstract** product types represented by the interfaces we extracted previously. For each variant of a product family, we create a separate factory class based on the `AbstractFactory` interface. A factory is a class that returns products of a particular kind. 

#### Structure

![Abstract Factory design pattern](https://refactoring.guru/images/patterns/diagrams/abstract-factory/structure.png)

#### Sample Code

*buttons/Button.java*

```java
public interface Button {
    void click();
}
```

*input/InputBox.java*

```java
public interface InputBox {
    void input();
}
```

*buttons/WinButton.java*

```java
public WinButton implements Button {
    @Override
    void click(){System.out.println("this is a win button!")};
}
```

*buttons/MacButton.java*

```java
public MacButton implements Button {
    @Override
    void click(){System.out.println("this is a mac button!")};
}
```

*input/WinInputBox.java*

```java
public WinInputBox implements InputBox {
    @Override
    void input(){System.out.println("this is a win inputbox!")};
}
```

*input/MacInputBox.java*

```java
public MacInputBox implements InputBox {
    @Override
    void input(){System.out.println("this is a mac inputbox!")};
}
```

*factory/GUIfactory.java*

```java
public interface GUIfactory {
    Button createButton();
    InputBox createInputBox();
}
```

*factory/WinFactory.java*

```java
public WinFactory implements GUIfactory {
    @Override
    Button createButton(){
        return new WinButton();
    }
    @Override
    InputBox createInputBox(){
        return new WinInputBox();
    }
}
```

*factory/MacFactory.java*

```java
public MacFactory implements GUIfactory {
    @Override
    Button createButton(){
        return new MacButton();
    }
    @Override
    InputBox createInputBox(){
        return new MacInputBox();
    }
}
```

*Application.java*

```java
public Application　{
    private Button button;
    private InputBox inputbox;
    
    public Application (GUIfactory factory){
        this.button = factory.createButton();
        this.inputbox = factory.createInputBox();
    }
    
    public void render(){
        this.button.click();
        this.inputbox.input();
    }
}
```

*Client.java*

```java
public class Demo {
    /**
     * Application picks the factory type and creates it in run time (usually at
     * initialization stage), depending on the configuration or environment
     * variables.
     */
    private static Application configureApplication() {
        Application app;
        GUIFactory factory;
        String osName = System.getProperty("os.name").toLowerCase();
        if (osName.contains("mac")) {
            factory = new MacOSFactory();
            app = new Application(factory);
        } else {
            factory = new WindowsFactory();
            app = new Application(factory);
        }
        return app;
    }

    public static void main(String[] args) {
        Application app = configureApplication();
        app.render();
    }
}
```

### Singleton

**Singleton** is a creational design pattern that lets you ensure that a class has only one instance, while providing a global access point to this instance.

#### Application Scenario

**Ensure that a class has just a single instance**. Why would anyone want to control how many instances a class has? The most common reason for this is to control access to some shared resource—for example, a database or a file.

**Provide a global access point to that instance**.  Remember those global variables that you (all right, me) used to store  some essential objects? While they’re very handy, they’re also very  unsafe since any code can potentially overwrite the contents of those  variables and crash the app.

Just like a global variable, the Singleton pattern lets you access  some object from anywhere in the program. However, it also protects that  instance from being overwritten by other code.

#### Structure

![The structure of the Singleton pattern](https://refactoring.guru/images/patterns/diagrams/singleton/structure-en.png)

#### Sample Code

```java
public final class Singleton {
    private static Singleton instance;
    public String value;

    private Singleton(String value) {
        // The following code emulates slow initialization.
        try {
            Thread.sleep(1000);
        } catch (InterruptedException ex) {
            ex.printStackTrace();
        }
        this.value = value;
    }

    public static Singleton getInstance(String value) {
        if (instance == null) {
            instance = new Singleton(value);
        }
        return instance;
    }
}
```



### Builder

The intend of **Builder Pattern**   is to allow step-by-step construction of a target object when you acquire the parameters for a constructor gradually and production of various types and representations of an object just from the same construction code.

#### Application Scenario

Imagine that you want to create a `House` object. To build a simple house, you need to construct four walls and a floor, install a door, fit a pair of windows, and build a roof. But what if you want a bigger, brighter 
house with a backyard and other goodies (like a heating system, plumbing, and electrical wiring)?

The simplest solution is to extend the base `House` class and create a set of subclasses, but numerous subclasses are really difficult to maintain! Another approach that does not involve breeding subclasses is to create a giant constructor right in the base `House` class with all possible parameters that control the house object. However, th ugly constructor is accompanied by a long list of useless parameters.

#### Structure

![Structure of the Builder design pattern](https://refactoring.guru/images/patterns/diagrams/builder/structure.png)

#### Sample Code

*Computer.java*

```java
public class Person {
    /*name required*/
    private final String name;
    /*gender required*/
    private final String gender;
    /*age not required*/
    private final String age;
    /*choes not required*/
    private final String shoes;
    /*clothes not required*/
    private final String clothes;
    /*money not required*/
    private final String money;
    /*house not reuqired*/
    private final String house;
    /*car not required*/
    private final String car;
    /*carrer not required*/
    private final String career;


    private Person(Builder builder) {
        this.name = builder.name;
        this.gender = builder.gender;
        this.age = builder.age;
        this.shoes = builder.shoes;
        this.clothes = builder.clothes;
        this.money = builder.money;
        this.house = builder.house;
        this.car = builder.car;
        this.career = builder.career;
    }

    public static class Builder {
        private final String name;
        private final String gender;
        private String age;
        private String shoes;
        private String clothes;
        private String money;
        private String house;
        private String car;
        private String career;

        public Builder(String name,String gender) {
            this.name = name;
            this.gender = gender;
        }

        public Builder age(String age) {
            this.age = age;
            return this;
        }

        public Builder car(String car) {
            this.car = car;
            return this;
        }

        public Builder shoes(String shoes) {
            this.shoes = shoes;
            return this;
        }

        public Builder clothes(String clothes) {
            this.clothes = clothes;
            return this;
        }

        public Builder money(String money) {
            this.money = money;
            return this;
        }

        public Builder house(String house) {
            this.house = house;
            return this;
        }

        public Builder career(String career) {
            this.career = career;
            return this;
        }

        public Person build(){
            return new Person(this);
        }
    }
```

As a result, you can create a person by this way:

```java
Person person = new Person.Builder("Jack","Male")
                .age("12")
                .money("10000")
                .car("Audi")
                .build();
```

### Prototype

**Prototype** is a creational design pattern that lets you copy existing objects without making your code dependent on their classes.

#### Application Scenario

Imagine that you have an object, and you want to create an exact copy of it.  How would you do it? First, you have to create a new object of the same  class. Then you have to go through all the fields of the original object  and copy their values over to the new object. However, not all objects can be copied that way  because some of the  fields may be private and not visible from outside of the object itself.

The Prototype pattern delegates the cloning process to the actual objects that are being cloned. The pattern declares a common interface `clone`  to create an object of the current class and  carries over all of the field values of the old object into the new one since access of private fields belonging to the same  class is reasonable. An object that supports cloning is called a *prototype*. 

#### Structure

![The structure of the Prototype design pattern](https://refactoring.guru/images/patterns/diagrams/prototype/structure.png)

#### Sample Code

Any class (java.lang.Object #clone())can implement this interface (java.lang.Cloneable) to become cloneable.

*Shape.java*

```java
import java.util.Objects;

public abstract class Shape {
    public int x;
    public int y;
    public String color;

    public Shape() {
    }

    public Shape(Shape target) {
        if (target != null) {
            this.x = target.x;
            this.y = target.y;
            this.color = target.color;
        }
    }

    public abstract Shape clone();

}
```

*Circle.java*

```java
public class Circle extends Shape {
    public int radius;

    public Circle() {
    }

    public Circle(Circle target) {
        super(target);
        if (target != null) {
            this.radius = target.radius;
        }
    }

    @Override
    public Shape clone() {
        return new Circle(this);
    }
}
```

*Demo.java*

```java
public class Demo {
    public static void main(String[] args) {
    List<Shape> list = new List<>();
    
     Circle circle = new Circle();
     circle.x = 10;
     circle.y = 20;
     circle.radius = 15;
     circle.color = "red";
     shapes.add(circle);

     Circle anotherCircle = (Circle) circle.clone();
     shapes.add(anotherCircle);
        
    }
```



## Structural Patterns

### Adapter

**Adapter** is a structural design pattern that allows objects with incompatible interfaces to collaborate.

#### Application Scenario

Imagine that you’re creating a stock market monitoring app. The app  downloads the stock data from multiple sources in XML format and then  displays nice-looking charts and diagrams for the user.

At the same time,  you decide to improve the app by integrating a smart  3rd-party analytics library, which requires the data to be in JSON format. 

You can create an *adapter*. This is a special object that converts the interface of one object so that another object can understand it.

#### Structure

![Structure of the Adapter design pattern (the object adapter)](https://refactoring.guru/images/patterns/diagrams/adapter/structure-object-adapter.png)

#### Sample Code

For some common examples, you can find in *java.util.Arrays.asList,  java.util.Collections.List*.

*RoundHole.java*

```java
public class RoundHole {
    double radis;

    public RoundHole(double radis) {
        this.radis = radis;
    }

    public double getRadis() {
        return radis;
    }

    public boolean fitted(RoundPeg peg){
        return (this.radis <= peg.getRadis())? true:false;
    }
}
```

*RounfPeg.java*

```java
public class RoundPeg {
    double radis;

    public RoundPeg(double radis) {
        this.radis = radis;
    }

    public RoundPeg() {
    }

    public double getRadis() {
        return radis;
    }
}
```

*SquarePeg.java*

```java
public class SquarePeg {
    double width;

    public SquarePeg(double width) {
        this.width = width;
    }

    public double getWidth() {
        return width;
    }
}
```

*RoundAdaptor.java*

```java
public class RoundAdaptor extends RoundPeg {
    SquarePeg squarePeg;

    public RoundAdaptor(SquarePeg squarePeg) {
        super();
        this.squarePeg = squarePeg;
    }

    @Override
    public double getRadis() {
        return Math.sqrt(Math.pow(squarePeg.getWidth()/2,2)*2);
    }
}

```

*Client.java*

```java
public class Client {

    public static void main(String[] args){
        RoundHole hole = new RoundHole(5);
        RoundPeg rpeg = new RoundPeg(5);
        if (hole.fitted(rpeg)) {
            System.out.println("Round peg r5 fits round hole r5.");
        }

        SquarePeg smallSqPeg = new SquarePeg(2);
        SquarePeg largeSqPeg = new SquarePeg(20);

        RoundAdaptor small = new RoundAdaptor(smallSqPeg);
        RoundAdaptor large = new RoundAdaptor(largeSqPeg);

        if (hole.fitted(small)){
            System.out.println("Small square peg fits round hole r5.");
        }

        if (hole.fitted(large)){
            System.out.println("Large square peg fits round hole r5.");
        }

    }
}
```

### Bridge 

**Bridge** is a structural design pattern that divides business logic or huge class into separate class hierarchies that can be developed independently.

#### Application Scenario



#### Structure

![Bridge design pattern](https://refactoring.guru/images/patterns/diagrams/bridge/structure-en.png)

#### Sample Code

*System.java*

```java
public interface System {
    String assertSystem();
}

class Windows implements System {
    @Override
    public String assertSystem(){
        return " In Windows System ";
    }
}

 class Linux implements System {
    @Override
    public String assertSystem(){
        return " In Linux System ";
    }
}

 class Mac implements System {
    @Override
    public String assertSystem(){
        return " In Mac System ";
    }
}
```

*Image.java*

```java
public class Image {
    System system;

    public Image(System system) {
        this.system = system;
    }

    public void print(){

    }
}

class JPEG extends Image {

    public JPEG(System system) {
        super(system);
    }

    @Override
    public void print(){
        java.lang.System.out.println(system.assertSystem() + "I am a JPEG image!");
    }
}

class PNG extends Image {

    public PNG(System system) {
        super(system);
    }

    @Override
    public void print(){
        java.lang.System.out.println(system.assertSystem() + "I am a PNG image!");
    }
}

class GIF extends Image {

    public GIF(System system) {
        super(system);
    }

    @Override
    public void print(){
        java.lang.System.out.println(system.assertSystem() + "I am a GIF image!");
    }
}
```

*Client.java*

```java
public class Client {
    public static void main(String[] args){
        Image jpeg = new JPEG(new Windows());
        Image png = new PNG(new Mac());

        jpeg.print();
        png.print();

    }
}
```



### Composite

The **composite pattern** is a structural pattern that groups a set of objects into a tree structure so that they may be manipulated as though they were one object. It contains three main compositions:

- The component protocol ensures all constructs in the tree can be treated the same way.
- A leaf is a component of the tree that does not have child elements.
- A composite is a container that can hold leaf objects and composites.

#### Application Scenario

Imagine that you have two types of objects: `Gifts` and `Boxes`. A `Box` can contain several `Gifts` as well as a number of smaller `Boxes`. These little `Boxes` can also hold some `Gifts` or even smaller `Boxes`, and so on. Say you decide to the sum price of the whole box.  The biggest box could contain simple gifts without any wrapping, as well as  boxes stuffed with gifts...and other boxes. How would you determine  the total price?

The Composite pattern suggests that you work with `Products` and `Boxes` through a common interface which declares a method for calculating the total price.

#### Structure

![Structure of the Composite design pattern](https://refactoring.guru/images/patterns/diagrams/composite/structure-en.png)

#### Sample Code

*Composite.java*

```java
public interface Composite {
    float calculateSum();
}
```

*Box.java*

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Box implements Composite {
    List<Composite> children = new ArrayList<>();

    public Box(Composite... children){
        add(children);
    }

    void add(Composite component){
        children.add(component);
    }

    void add(Composite... components){
        children.addAll(Arrays.asList(components));
    }

    @Override
    public float calculateSum() {
        if (children.isEmpty()) {
            return 0;
        } else {
            float sum = 0;
            for (Composite child : children) {
                sum = sum + child.calculateSum();
            }
            return sum;
        }
    }
}
```

*Gift.java*

```java
public class Gift implements Composite{
    float price;

    public Gift(float price) {
        this.price = price;
    }

    @Override
    public float calculateSum() {
        return price;
    }
}

```

*Demo.java*

```java
public class Demo {
    public static void main(String[] args){
        Box box = new Box(
                new Gift((float) 5.5),
                new Box(new Gift((float) 2.0), new Gift(3)),
                new Gift((float) 4.5),
                new Box(new Gift((float) 1.0))
        );
        System.out.println(box.calculateSum());
    }
}
```

### Decorator

#### Application Scenario

Imagine that you are working on a notification library which lets other programs notify their users about important events. At first, all notifications are sent in emails. However, you realize that users of the library expect more than just email notifications. Many of them would like to receive an SMS about critical 
issues. Others would like to be notified on Facebook. You tried to address that problem by creating special subclasses which combined several notification methods within one class. However, it quickly became apparent that this approach would bloat the code immensely, not only the library code but the client code as well.



#### Structure

![Structure of the Decorator design pattern](https://refactoring.guru/images/patterns/diagrams/decorator/structure.png)

#### Sample Code

*Beverage.class*

```java
public abstract class Beverage {
    protected String description = "Unknow Beverage";

    public String getDescription() {
        return description;
    }

    public abstract double cost();
}
```

*Coco.class*

```java
public class Coco extends Beverage {
    public Coco(){
        description = "Coco";
    }

    public double cost(){
        return 0.85;
    }
}
```

*Coffee.class*

```java
public class Coffee extends Beverage {
    public Coffee(){
        description = "Coffee";
    }

    public double cost(){
        return 0.65;
    }
}
```

*Decorator.class*

```java
public abstract class Decorator extends Beverage {
    protected Beverage beverage;

    public Decorator(Beverage beverage) {
        this.beverage = beverage;
    }
}
```

*Moka.class*

```java
public class Moka extends Decorator {

    public Moka(Beverage beverage){
        super(beverage);
    }

    @Override
    public String getDescription() {
        return beverage.getDescription() + "Moka";
    }

    @Override
    public double cost() {
        return beverage.cost() + 0.15;
    }
}
```

*Soy.class*

```java
public class Soy extends Decorator {

    public Soy(Beverage beverage){
        super(beverage);
    }

    @Override
    public String getDescription() {
        return beverage.getDescription() + "Soy";
    }

    @Override
    public double cost() {
        return beverage.cost() + 0.25;
    }
}
```

*Client.class*

```java
public class Client {
    public static void main(String[] args){
        Beverage first_cup = new Coco();
        System.out.println("The first cup is " + first_cup.getDescription() + "and you must pay" + first_cup.cost() + "dollars!");

        Beverage second_cup = new Coco();
        second_cup = new Soy(second_cup);
        System.out.println("The second cup is " + second_cup.getDescription() + "and you must pay" + second_cup.cost() + "dollars!");

        Beverage third_cup = new Coffee();
        third_cup = new Moka(third_cup);
        third_cup = new Soy(third_cup);
        System.out.println("The third cup is " + third_cup.getDescription() + "and you must pay" + third_cup.cost() + "dollars!");

    }
}
```

### Facade 

#### Application Scenario

Imagine that you must make your code work with a broad set of objects that belong to a sophisticated library or framework. Ordinarily, you would need to initialize all of those objects, keep track of dependencies, execute methods in the correct order, and so on.

A facade is a class that provides a simple interface to a complex subsystem which contains lots of moving parts. A facade might provide limited functionality in comparison to working with the subsystem directly. However, it includes only those features that clients really care about.

#### Structure

![Structure of the Facade design pattern](https://refactoring.guru/images/patterns/diagrams/facade/structure.png)

#### Sample Code

*Cpu.class*

```java
public class Cpu {
    String brand;

    public Cpu(String brand) {
        this.brand = brand;
    }

    public String getBrand() {
        return brand + ("Cpu");
    }

    public void run(){
        System.out.println(getBrand() + " " + "is running!");
    }
}
```

*Memory.class*

```java
public class Memory {
    String brand;

    public Memory(String brand) {
        this.brand = brand;
    }

    public String getBrand() {
        return brand + ("Memory");
    }

    public void run(){
        System.out.println(getBrand() + " " + "is running!");
    }
}
```

*HardDrive.class*

```java
public class HardDrive {
    String brand;

    public HardDrive(String brand) {
        this.brand = brand;
    }

    public String getBrand() {
        return brand + ("HardDrive");
    }

    public void run(){
        System.out.println(getBrand() + " " + "is running!");
    }
}

```

*Facade.class*

```java
public class Facade {
    private Cpu cpu;
    private Memory memory;
    private  HardDrive hardDrive;

    public Facade(Cpu cpu, Memory memory, HardDrive hardDrive) {
        this.cpu = cpu;
        this.memory = memory;
        this.hardDrive = hardDrive;
    }

    public void run(){
        cpu.run();
        memory.run();
        hardDrive.run();
    }
}
```

*Computer.class*

```java
public class Computer {
    Facade facade;

    public Computer(Facade facade) {
        this.facade = facade;
    }

    public void run(){
        facade.run();
    }
}
```

*Client.class*

```java
public class Client {
    public static void main(String[] args) {
        Cpu cpu = new Cpu("Intel");
        Memory memory = new Memory("Sandisk");
        HardDrive hardDrive = new HardDrive("Toshiba");

        Facade facade = new Facade(cpu,memory,hardDrive);

        Computer computer = new Computer(facade);

        computer.run();
    }
}
```

### Flyweight

**Flyweight** is a structural design pattern that allows programs to support vast quantities of objects by keeping their memory consumption low. 

Use the Flyweight pattern only when your program must support a huge number of similar objects which barely fit into available RAM.

#### Application Scenario

#### Structure

![Structure of the Flyweight design pattern](https://refactoring.guru/images/patterns/diagrams/flyweight/structure.png)

#### Sample Code

*Tree.java*

```java
import java.awt.*;

public class Tree {
    private int x;
    private int y;
    private TreeType type;

    public Tree(int x, int y, TreeType type) {
        this.x = x;
        this.y = y;
        this.type = type;
    }

    public void draw(Graphics g) {
        type.draw(g, x, y);
    }
}
```

*TreeType.java*

```java
import java.awt.*;
import java.util.HashMap;
import java.util.Map;

public class TreeType {
    private String name;
    private Color color;
    private  static Map<String,TreeType> map = new HashMap<>();

    private TreeType(String name, Color color) {
        this.name = name;
        this.color = color;
    }

    public static TreeType TreeTypeFactory(String name, Color color){
        TreeType result = map.get(name);
        if (result == null){
            map.put(name,new TreeType(name,color));
            result = map.get(name);
        }
        return result;
    }

    public void draw(Graphics g, int x, int y) {
        g.setColor(Color.BLACK);
        g.fillRect(x - 1, y, 3, 5);
        g.setColor(color);
        g.fillOval(x - 5, y - 10, 10, 10);
    }
}
```

*Forest.java*

```java
import javax.swing.*;
import java.awt.*;
import java.util.ArrayList;
import java.util.List;

public class Forest extends JFrame {
    private List<Tree> trees = new ArrayList<>();

    public void plantTree(int x, int y, String name, Color color) {
        TreeType type = TreeType.TreeTypeFactory(name, color);
        Tree tree = new Tree(x, y, type);
        trees.add(tree);
    }

    @Override
    public void paint(Graphics graphics) {
        for (Tree tree : trees) {
            tree.draw(graphics);
        }
    }
}
```

*Client.java*

```java
import java.awt.*;

public class Client {
    static int CANVAS_SIZE = 500;
    static int TREES_TO_DRAW = 1000000;
    static int TREE_TYPES = 2;

    public static void main(String[] args) {
        Forest forest = new Forest();
        for (int i = 0; i < Math.floor(TREES_TO_DRAW / TREE_TYPES); i++) {
            forest.plantTree(random(0, CANVAS_SIZE), random(0, CANVAS_SIZE),
                    "Summer Oak", Color.GREEN);
            forest.plantTree(random(0, CANVAS_SIZE), random(0, CANVAS_SIZE),
                    "Autumn Oak", Color.ORANGE);
        }
        forest.setSize(CANVAS_SIZE, CANVAS_SIZE);
        forest.setVisible(true);

        System.out.println(TREES_TO_DRAW + " trees drawn");
        System.out.println("---------------------");
        System.out.println("Memory usage:");
        System.out.println("Tree size (8 bytes) * " + TREES_TO_DRAW);
        System.out.println("+ TreeTypes size (~30 bytes) * " + TREE_TYPES + "");
        System.out.println("---------------------");
        System.out.println("Total: " + ((TREES_TO_DRAW * 8 + TREE_TYPES * 30) / 1024 / 1024) +
                "MB (instead of " + ((TREES_TO_DRAW * 38) / 1024 / 1024) + "MB)");
    }

    private static int random(int min, int max) {
        return min + (int) (Math.random() * ((max - min) + 1));
    }
}
```

### Proxy

**Proxy** is a structural design pattern that provides an object that acts as a substitute for a real service object used by a client. A proxy receives client requests, does some work (access control, caching, etc.) and then passes the request to a service object.

#### Application Scenario

Imagine that you have a massive object that consumes a vast amount of system resources. You need it from time to time, but not always. An approach is to solve this problem is to implement lazy initialization: you can call every client to create this object when it is needed. But the disadvantage is also clear: a lot of code duplication emerge. 

The **Proxy pattern** suggests that you create a new proxy class with the same interface as an original service object.  Upon receiving a request from a client, the proxy creates a real service object and delegates all the work to it. The benefit is that you can excute something either before or after the primary logic of the class without changing that class!

Credit cards is an example of proxy.  Both credit cards and cash implement the payment interface. With the credit card,  customers feel comfortable because there is no need for them to carry loads of cash around. And shop owner is also happy since the income from a transaction gets added electronically to the shop’s bank account without the risk of losing the deposit or getting robbed on the way to the bank.

#### Structure

![Structure of the Proxy design pattern](https://refactoring.guru/images/patterns/diagrams/proxy/structure.png)

#### Sample Code

*Star.java*

```java
public interface Star {
    void signContract();
    void bookTicket();
    void sing();
    void collectMoney();
}
```

*RealStar.java*

```java
public class RealStar implements Star {
    @Override
    public void signContract() {
        System.out.println("real star is signing a contract");
    }

    @Override
    public void bookTicket() {
        System.out.println("real star is booking a ticket");
    }

    @Override
    public void sing() {
        System.out.println("real star is singing");
    }

    @Override
    public void collectMoney() {
        System.out.println("real star is collecting money");
    }
}
```

*Proxy.java*

```java
public class Proxy implements Star {
    private Star star;

    public Proxy(Star star) {
        this.star = star;
    }


    @Override
    public void signContract() {
        System.out.println("proxy is signing a contract");
    }

    @Override
    public void bookTicket() {
        System.out.println("proxy is booking a ticket");
    }

    @Override
    public void sing() {
        star.sing();
    }

    @Override
    public void collectMoney() {
        System.out.println("Proxy is collecting money");
    }
}
```

*Client.java*

```java
public class Client {
    public static void main(String[] args){
        Star real = new RealStar();
        Star proxy = new Proxy(real);

        proxy.signContract();
        proxy.bookTicket();
        proxy.sing();
        proxy.collectMoney();
    }
}
```

Or you can implements *InvocationHandler* to achive dynamic proxy.

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

public class StarHandler implements InvocationHandler {
    private Star realstar;

    public StarHandler(Star star) {
        this.realstar = star;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        method.invoke(realstar,args);
        return null;
    }
}
```



## Behavioral Patterns

### Chain of Responsbility

**Chain of Responsibility** is a behavioral design pattern that lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass 
it to the next handler in the chain.

#### Application Scenario

Image that you prepare to design a on-line order system. Apparently, this system requires strict access in order to main safety.  So you must design a module to check the request sent by users: such as username and password, filters for repeated requests from the same IP address in short time, and check that make sure that the request pass through to the system only if there’s no suitable cached response.

The **Chain of Responsibility** relies on transforming particular behaviors into stand-alone objects called *handlers*. In this case, each check should be extracted to its own class with a single method that performs the check. The request, along with its data, is passed to this method as an argument. It is easily to allocate the burden of check function to each handler and make each one play a certain role.

#### Structure

![Structure of the Chain Of Responsibility design pattern](https://refactoring.guru/images/patterns/diagrams/chain-of-responsibility/structure-indexed.png)

#### Sample Code

*Handler.java*

```java
public abstract class Handler {
    Handler next;

    public Handler(Handler next) {
        this.next = next;
    }

    public Handler() {
    }

    protected boolean check(String username, String password){
        if (this.next == null){
            return true;
        }
        return next.check(username,password);
    }
}
```

*TokenHandler.java*

```java
public class TokenHandler extends Handler {

    public TokenHandler(Handler next) {
        super(next);
    }

    @Override
    protected boolean check(String username, String password) {
        System.out.println("Local Token is ok!");
        return next.check(username, password);
    }
}
```

*TimeHandler.java*

```java
public class TimeHandler extends Handler {

    public TimeHandler(Handler next) {
        super(next);
    }

    @Override
    protected boolean check(String username, String password) {
        System.out.println("No rick in time period!");
        return next.check(username, password);
    }
}
```

*RoleCheckHandler.java*

```java
public class RoleCheckHandler extends  Handler {
    public RoleCheckHandler(Handler next) {
        super(next);
    }

    public RoleCheckHandler() {
        super();
    }

    @Override
    protected boolean check(String username, String password) {

        System.out.println("Username is in Data and the Password is correct!");
        if (next == null) return true;
        return next.check(username,password);
    }
}
```

*Request.java*

```java
public class Request {

    String username;
    String password;

    Handler checkHandler;

    public Request(String username, String password) {
        this.username = username;
        this.password = password;
    }

    public void setCheckHandler(Handler checkHandler) {
        this.checkHandler = checkHandler;
    }

    public boolean login(){
        return checkHandler.check(this.username,this.password);
    }
}
```

*Client.java*

```java
public class TimeHandler extends Handler {

    public TimeHandler(Handler next) {
        super(next);
    }

    @Override
    protected boolean check(String username, String password) {
        System.out.println("No rick in time period!");
        return next.check(username, password);
    }
}
```



### Command 



#### Application Scenario



#### Structure



#### Sample Code





