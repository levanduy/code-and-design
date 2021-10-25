#  Code is clean -Java
 
##  Contents
 
 1. [ Introduction ](#Introduction)
 2. [ Variable ](#Variable)
 3. [ Function ](#Function)
 4. [ Object and data structure ](#Object and data structure)
 5. [ Class ](#Class)
 6. [ SOLID ](#solid)
 7. [ TEST ](#测试)
 8. [ Error handling ](#Error handling)
 9. [ Format ](#Format)
 10. [ Comment ](#Comment)
 11. [ tools ](#tools)
 
 
##  Introduction
 
![ A funny picture that uses the number of complaints you read when you read the code to evaluate the quality of the software ](https://user-gold-cdn.xitu.io/2020/3/1/17093d303918263e?w=500&h=471&f=jpeg&s =45533)
 
Will be derived from Robert C. Martin's [ *Clean Code* ](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
The principles of software engineering are adapted to Java. This is not a coding style guide, it is a production using Java
A guide to readable, reusable, and reconfigurable software.
 
Every principle here is not mandatory, and even fewer can be widely recognized. These are just guidelines, but they are
*Clean Code* The author's many years of experience.
 
Our software engineering industry has only been in the short span of 50 years, and there is still a lot to learn from. When the software architecture is as old as the building architecture,
Maybe we will have hard and fast rules to follow. And now, let these guides serve as the basis for the Java code produced by you and your team
The standard of quality.
 
One more thing: knowing that these guidelines will not immediately make you a better software developer, and using them for many years will not
It does not mean that you will no longer make mistakes. Each piece of code is a draft at the beginning, and is shaped into its final form like wet clay. Finally when we
When reviewing the code with your partners, clear out those imperfections. Don’t blame yourself for the draft code that needs improvement at the beginning, but on those generations.
Code to start.
 
## **Variables** 
 
###  Use meaningful and readable variable names
 
**not good:**
```
 String yyyymmdstr = new SimpleDateFormat("YYYY/MM/DD").format(new Date());
```
 
**OK:**
```
 String currentDate = new SimpleDateFormat("YYYY/MM/DD").format(new Date());
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Use the same vocabulary for variables of the same type
 
**not good:**
```
 getUserInfo();
 getClientData();
 getCustomerRecord();
```
 
**OK:**
```
 getUser();
```
**[ ⬆ back to top ](#Code Neat-Java)**
###  Use searchable names
 
We have to read much more code than we have to write, so the readability and searchability of the code we write is very important. Not used
Meaningful variable names will make our programs difficult to understand and will hurt our readers, so please use searchable variable names.
 
**not good:**
```
// Fuck, what the hell is 86400000?
 setTimeout(blastOff, 86400000);
```
 
**OK:**
```
// Declare them as global constants.
 public static final int MILLISECONDS_IN_A_DAY = 86400000;
 setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Use explanatory variables
**not good:**
```
 String address = "One Infinite Loop, Cupertino 95014";
 String cityZipCodeRegex = "/^[^,\\\\]+[,\\\\\\s]+(.+?)\\s*(\\d{5})?$/";
 saveCityZipCode(address.split(cityZipCodeRegex)[0],
 address.split(cityZipCodeRegex)[1]);
```
 
**OK:**
```
 String address = "One Infinite Loop, Cupertino 95014";
 String cityZipCodeRegex = "/^[^,\\\\]+[,\\\\\\s]+(.+?)\\s*(\\d{5})?$/";
 String city = address.split(cityZipCodeRegex)[0];
 String zipCode = address.split(cityZipCodeRegex)[1];
 saveCityZipCode(city, zipCode);
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Avoid mental mapping
Display is better than implicit
 
**not good:**
```
 String [] l = {"Austin", "New York", "San Francisco"};
 for (int i = 0; i <l.length; i++) {
 String li = l[i];
 doStuff();
 doSomeOtherStuff();
 // ...
 // ...
 // ...
 // Wait, what is `$li` for again?
 dispatch(li);
 }
```
 
**OK:**
```
 String[] locations = {"Austin", "New York", "San Francisco"};
 for (String location: locations) {
 doStuff();
 doSomeOtherStuff();
 // ...
 // ...
 // ...
 dispatch(location);
 }
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Do not add unnecessary context
 
If your class name/object name is meaningful, don't repeat it in the variable name.
 
**not good:**
```
 class Car {
 public String carMake = "Honda";
 public String carModel = "Accord";
 public String carColor = "Blue";
 }
 void paintCar(Car car) {
 car.carColor = "Red";
 }
```
 
**OK:**
``` 
 class Car {
 public String make = "Honda";
 public String model = "Accord";
 public String color = "Blue";
 }
 void paintCar(Car car) {
 car.color = "Red";
 }
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
## **Functions** 
 
###  Function parameters (two or less ideal)
 
Limiting the number of function parameters is very important, because this will make your function easier to test. Once more than three parameters will cause the group
The combination explodes, because you have to write a large number of test cases for each parameter.
 
No parameter is ideal, one or two parameters are also possible, three parameters should be avoided, and more than three should be reconstructed. usually,
If you have more than two function parameters, it means your function is trying to do too many things. If not, in most cases one
More advanced objects may satisfy the demand.
 
When you find yourself needing a large number of parameters, you can use an object.
 
**not good:**
```
void createMenu(String title,String body,String buttonText,boolean cancellable){}
```
 
**OK** :
```
 class MenuConfig{
 String title;
 String body;
 String buttonText;
 boolean cancellable;
 }
 void createMenu(MenuConfig menuConfig){}
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
 
###  Function should only do one thing
 
This is the most important rule in software engineering. When functions need to do more, they will be more difficult to write, test, and reason about.
When you can isolate a function to only one action, they can be easily refactored and your code will be easier to read. like
If you strictly follow this article in this guide, you will be ahead of many developers.
 
**not good:**
```
 public void emailClients(List<Client> clients) {
 for (Client client: clients) {
 Client clientRecord = repository.findOne(client.getId());
 if (clientRecord.isActive()){
 email(client);
 }
 }
 }
```
 
**OK:**
```
 public void emailClients(List<Client> clients) {
 for (Client client: clients) {
 if (isActiveClient(client)) {
 email(client);
 }
 }
 }
 private boolean isActiveClient(Client client) {
 Client clientRecord = repository.findOne(client.getId());
 return clientRecord.isActive();
 }
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###The  function name should explain what it is going to do
 
**not good:**
```
 private void addToDate(Date date, int month){
 //..
 }
 Date date = new Date();
 // It's hard to to tell from the method name what is added
 addToDate(date, 1);
```
 
**OK:**
```
 private void addMonthToDate(Date date, int month){
 //..
 }
 Date date = new Date();
 addMonthToDate(1, date);
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Functions should have only one level of abstraction
 
When there is more than one level of abstraction in your function, your function usually does too much. Splitting the function will improve reusability and testability.
 
**not good:**
```
 void parseBetterJSAlternative(String code){
 String[] REGECES={};
 String[] statements=code.split(" ");
 String[] tokens={};
 for(String regex: Arrays.asList(REGECES)){
 for(String statement:Arrays.asList(statements)){
 //...
 }
 }
 String[] ast={};
 for(String token:Arrays.asList(tokens)){
 //lex ...
 }
 for(String node:Arrays.asList(ast)){
 //parse ...
 }
 }
```
 
**OK:**
```
 String[] tokenize(String code){
 String[] REGECES={};
 String[] statements=code.split(" ");
 String[] tokens={};
 for(String regex: Arrays.asList(REGECES)){
 for(String statement:Arrays.asList(statements)){
 //tokens push
 }
 }
 return tokens;
 }
 String[] lexer(String[] tokens){
 String[] ast={};
 for(String token:Arrays.asList(tokens)){
 //ast push
 }
 return ast;
 }
 void parseBetterJSAlternative(String code){
 String[] tokens=tokenize(code);
 String[] ast=lexer(tokens);
 for(String node:Arrays.asList(ast)){
 //parse ...
 }
 }
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Remove redundant code
 
Do your best to avoid redundant code. Redundant code is bad because it means there are multiple places when you need to modify some logic
Need to be modified.
 
Imagine that you are running a restaurant and you need to record all the stocks of tomatoes, onions, garlic, various spices and so on. If you have much
A list of records, you have to update multiple lists when you cook a dish with tomatoes. If you only have one list, there is only one place to change
new!
 
You have redundant code usually because you have two or more slightly different things that share most of them, but their differences force you to use
Use two or more independent functions to handle most of the same things. Removing redundant code means creating a system that can handle these differences
Abstract function/module/class.
 
Getting this abstraction right is the key, which is why you should follow the SOLID of the *Classes* chapter. Bad abstraction is better than redundancy
The remaining code is worse, so proceed with caution. Now that you have said that, if you can make a good abstraction, then do it. Don't repeat
You yourself, otherwise you will find that when you want to modify one thing, you need to modify multiple places at all times.
 
**not good:**
```
 void showDeveloperList(List<Developer> developers){
 for(Developer developer:developers){
 render(new Data(developer.expectedSalary,developer.experience,developer.githubLink));
 }
 }
 void showManagerrList(List<Manager> managers){
 for(Manager manager:managers){
 render(new Data(manager.expectedSalary,manager.experience,manager.portfolio));
 }
 }
```
 
**OK:**
```
 void showList(List<Employee> employees){
 for(Employee employee:employees){
 Data data=new Data(employee.expectedSalary,employee.experience,employee.githubLink);
 String portfolio=employee.portfolio;
 if("manager".equals(employee)){
 portfolio=employee.portfolio;
 }
 data.portfolio=portfolio;
 render(data);
 }
 }
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Don't use marker bits as function parameters
 
The flag bit tells your users that this function has done more than one thing. The function should only do one thing. If your function is because of a boolean
Different code paths appear, please split them.
 
**not good:**
```
void createFile(String name,boolean temp){
 if(temp){
 new File("./temp"+name);
 }else{
 new File(name);
 }
 }
```
 
**OK:**
```
void createFile(String name){
 new File(name);
 }
void createTempFile(String name){
 new File("./temp"+name);
 }
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Avoid side effects
 
If a function does anything but accepts a value and then returns one or more values, it will have side effects. It may be
Write to a file, modify a global variable, or accidentally connect all your money to a stranger.
 
Now you do need side effects occasionally in your program. Just like the code above, you may need to write to a file. What you need to do is set
What you have to do in Sinochem, don’t let multiple functions or classes write to a specific file, use one service to implement it, one and only one
indivual.
 
The point is to avoid these common easy-to-make mistakes, such as sharing state between objects without using any structure, and writing anywhere.
The mutable data types without centralization cause side effects. If you can do this, then you will be happier than other code farmers.
 
**not good:**
```
 String name="Ryan McDermott";
 void splitIntoFirstAndLastName(){
 name=name.split(" ").toString();
 }
 splitIntoFirstAndLastName();
 System.out.println(name);
```
 
**OK:**
```
 String name="Ryan McDermott";
 String splitIntoFirstAndLastName(){
 return name.split(" ").toString();
 }
 String newName=splitIntoFirstAndLastName();
 System.out.println(name);
 System.out.println(newName);
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
 
###  Functional programming is better than imperative programming
 
Functional languages ​​are more concise
And it's easier to test, please use it when you can use functional programming style.
 
**not good:**
```
 List<Integer> programmerOutput=new ArrayList<>();
 programmerOutput.add(500);
 programmerOutput.add(1500);
 programmerOutput.add(150);
 programmerOutput.add(1000);
 int totalOutput=0;
 for(int i=0;i<programmerOutput.size();i++){
 totalOutput+=programmerOutput.get(i);
 }
```
 
**OK:**
```
 List<Integer> programmerOutput=new ArrayList<>();
 programmerOutput.add(500);
 programmerOutput.add(1500);
 programmerOutput.add(150);
 programmerOutput.add(1000);
 int totalOutput = programmerOutput.stream().filter(programmer -> programmer> 500).mapToInt(programmer -> programmer).sum();
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Encapsulate conditional statements
 
**not good:**
```
 if(fsm.state.equals("fetching")&&listNode.isEmpty(){
 //...
 }
```
 
**OK:**
```
 void shouldShowSpinner(Fsm fsm, String listNode) {
 return fsm.state.equals("fetching")&&listNode.isEmpty();
 }
 if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
 // ...
 }
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Avoid negative conditions
 
**not good:**
```
 void isDOMNodeNotPresent(Node node) {
 // ...
 }
 if (!isDOMNodeNotPresent(node)) {
 // ...
 }
```
 
**OK:**
```
 void isDOMNodePresent(Node node) {
 // ...
 }
 if (isDOMNodePresent(node)) {
 // ...
 }
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Avoid conditional statements
 
This seems like an impossible task. When hearing this for the first time, most people will say: " I can still expect me to do it without the `if` statement.
What?” The answer is that in most cases you can use polymorphism to accomplish the same task. The second question is usually “Okay, then it’s great.
But why do I want to do that?" The answer is the idea of ​​the last clean code we learned: A function should only do one thing.
When you have a class/function that uses an `if` statement, you are telling your users that your function does more than one thing. Remember: just make one
matter.
 
**not good:**
```
 class Airplane{
 int getCurisingAltitude(){
 switch(this.type){
 case "777":
 return this.getMaxAltitude()-this.getPassengerCount();
 case "Air Force One":
 return this.getMaxAltitude();
 case "Cessna":
 return this.getMaxAltitude()-this.getFuelExpenditure();
 }
 }
 }
```
 
**OK:**
```
 class Airplane {
 // ...
 }
 class Boeing777 extends Airplane {
 // ...
 int getCruisingAltitude() {
 return this.getMaxAltitude()-this.getPassengerCount();
 }
 }
 class AirForceOne extends Airplane {
 // ...
 int getCruisingAltitude() {
 return this.getMaxAltitude();
 }
 }
 class Cessna extends Airplane {
 // ...
 int getCruisingAltitude() {
 return this.getMaxAltitude()-this.getFuelExpenditure();
 }
 }
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Remove zombie code
 
Dead code is just as bad as redundant code. There is no reason to keep it in the code base. If it will not be called, delete it. When you need
When it does, it is still saved in the version history.
 
**not good:**
```
 void oldRequestModule(String url) {
 // ...
 }
 void newRequestModule(String url) {
 // ...
 }
 String req = newRequestModule;
 inventoryTracker("apples", req, "www.inventory-awesome.io");
```
 
**OK:**
```
 void newRequestModule(String url) {
 // ...
 }
 String req = newRequestModule;
 inventoryTracker("apples", req, "www.inventory-awesome.io");
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
## **Objects and Data Structures** 
 
###  Using getters and setters
 
 Use getters and setters to access data on an object than simply look up properties on an object
Much better. "Why?" You may ask, well, the reasons are listed below:
 
* When you want to do more things behind getting an object property, you don't need to look up and modify every access in the code base;
* Use `set` to make adding verification easy;
* Package internal realization;
* When using getting and setting, it is easy to add logs and error handling;
* Inherit this class, you can override the default function;
* You can delay loading the properties of the object, for example, get it from the server.
 
**not good:**
```
 class BankAccount{
 public int balance=1000;
 }
 BankAccount bankAccount=new BankAccount();
 bankAccount.balance-=100;
```
 
**OK:**
```
class BankAccount{
 private int blance=1000;
 public int getBlance() {
 return blance;
 }
 public void setBlance(int blance) {
 if(verifyIfAmountCanBeSetted(blance)){
 this.blance = blance;
 }
 }
 void verifyIfAmountCanBeSetted(int amount){
 //...
 }
 }
 BankAccount bankAccount=new BankAccount();
 bankAccount.setBlance(2000);
 int balance=bankAccount.getBlance();
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
## **Class** 
 
###  Using method chain
 
This mode is very useful in Java, and you can see it in many libraries such as Glide and OkHttp.
It makes your code expressive and reduces verbosity. For this reason, I said, use the method chain and then look at your code
How concise it will become. In your class/method, simply return `this` at the end of each method , and then you can put the
Other methods are chained together.
 
**not good:**
```
 class Car{
 private String make;
 private String model;
 private String color;
 public void setMake(String make) {
 this.make = make;
 }
 public void setModel(String model) {
 this.model = model;
 }
 public void setColor(String color) {
 this.color = color;
 }
 public void save(){
 console.log(this.make, this.model, this.color);
 }
 }
 Car car=new Car();
 car.setColor("pink");
 car.setMake("Ford");
 car.setModel("F-150");
 car.save();
```
 
**OK:**
```
 class Car{
 private String make;
 private String model;
 private String color;
 public Car setMake(String make) {
 this.make = make;
 return this;
 }
 public Car setModel(String model) {
 this.model = model;
 return this;
 }
 public Car setColor(String color) {
 this.color = color;
 return this;
 }
 public Car save(){
 console.log(this.make, this.model, this.color);
 return this;
 }
 }
 Car car=new Car()
 .setColor("pink")
 .setMake("Ford")
 .setModel("F-150")
 .save();
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Combination takes precedence over inheritance
 
As stated in [ *Design Pattern Gang of Four* ](https://en.wikipedia.org/wiki/Design_Patterns), if possible,
You should use composition in preference to inheritance. There are many good reasons to use inheritance, and there are many good reasons to use composition. This motto
The point is, if your instinctive view is inheritance, then please think about whether composition can better model your problem. In many cases it really
Can.
 
Then you might think, "When should I switch to inheritance?" It depends on the question you have, but here is a decent list that says
Explain when inheritance is better than composition:
 
1. Your inheritance means "is a" relationship rather than "has a" relationship (human -> animal vs user -> user details);
2. You can reuse the code from the base class (people can act like all animals);
3. You want to make a global modification to the subclass through the base class (change the calorie consumption of all animals when they act);
 
**not good:**
```
 class Employee{
 private String name;
 private String email;
 }
 // Not good because employees "have" tax rate data, and EmployeeTaxData is not an Employee type.
 class EmployeeTaxData extends Employee{
 private String ssn;
 private String salary;
 }
```
 
**OK:**
```
 class EmployeeTaxData{
 private String ssn;
 private String salary;
 public EmployeeTaxData(String ssn, String salary) {
 this.ssn = ssn;
 this.salary = salary;
 }
 }
class Employee{
 private String name;
 private String email;
 private EmployeeTaxData taxData;
 void setTaxData(String ssn,String salary){
 this.taxData=new EmployeeTaxData(ssn,salary);
 }
 }
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
## **SOLID** 
 
###  Single Responsibility Principle (SRP)
 
As the Code Cleaner says, "Never have more than one reason to modify a class". Fill a class with many functions, just like you are sailing
You can only bring one suitcase in your class. In doing so, your class will not have ideal cohesion, and there will be too many reasons to modify it.
It’s important to minimize the number of times you need to modify a class, because if a class has too many features, once you modify a small part of it,
It will be difficult to figure out what impact it will have on other modules in the code base.
 
**not good:**
```
 class UserSettings {
 User user;
 void changeSettings(UserSettings settings) {
 if (this.verifyCredentials()) {
 // ...
 }
 }
 void verifyCredentials() {
 // ...
 }
 }
```
 
**OK:**
```
 User user;
 UserAuth auth;
 public UserSettings(User user) {
 this.user = user;
 this.auth = new UserAuth(user);
 }
 void changeSettings(UserSettings settings) {
 if (this.auth.verifyCredentials()) {
 // ...
 }
 }
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Opening and closing principle (OCP)
 
Bertrand Meyer said, "Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification." This
what does it mean? This principle basically states that you should allow users to add functionality without modifying existing code.
 
**not good:**
```
 class AjaxAdapter extends Adapter {
 private String name;
 public AjaxAdapter() {
 this.name = "ajaxAdapter";
 }
 }
 class NodeAdapter extends Adapter {
 private String name;
 public NodeAdapter() {
 this.name = "nodeAdapter";
 }
 }
 class HttpRequester {
 public HttpRequester(Adapter adapter) {
 this.adapter = adapter;
 }
 void fetch(String url) {
 if ("ajaxAdapter".equals(this.adapter.name)) {
 makeAjaxCall(url);
 } else if ("httpNodeAdapter".equals(this.adapter.name)) {
 makeHttpCall(url);
 }
 }
 }
 void makeAjaxCall(String url) {
 // request and return promise
 }
 void makeHttpCall(String url) {
 // request and return promise
 }
```
 
**OK:**
```
 class AjaxAdapter extends Adapter {
 private String name;
 public AjaxAdapter() {
 this.name = "ajaxAdapter";
 }
 void request(String url){
 }
 }
 class NodeAdapter extends Adapter {
 private String name;
 public NodeAdapter() {
 this.name = "nodeAdapter";
 }
 void request(String url){
 }
 }
 class HttpRequester {
 public HttpRequester(Adapter adapter) {
 this.adapter = adapter;
 }
 void fetch(String url) {
 this.adapter.request(url);
 }
 }
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###Liskov  Substitution Principle (LSP)
 
This is aimed at a terrifying intent in a very simple one. Its formal definition is: "If S is a subtype of T, then the class
An object of type T can be replaced by an object of type S (for example, an object of type S can be used as a substitute for type T). No need
To modify the desired nature of the target program (correctness, task performance, etc.). "This is even a terrifying definition.
 
The best explanation is that if you have a base class and a subclass, the base class and the word class can be interchanged without producing incorrect results. This can
There may be some doubts, let's take a look at this classic example of squares and rectangles. Mathematically speaking, a square is a rectangle,
But if you use the "is-a" relationship to implement inheritance, you will soon run into trouble.
 
**not good:**
```
 class Rectangle {
 protected int width;
 protected int height;
 public Rectangle() {
 this.width = 0;
 this.height = 0;
 }
 void setColor(String color) {
 // ...
 }
 void render(int area) {
 // ...
 }
 void setWidth(int width) {
 this.width = width;
 }
 void setHeight(int height) {
 this.height = height;
 }
 int getArea() {
 return this.width * this.height;
 }
 }
 class Square extends Rectangle {
 void setWidth(int width) {
 this.width = width;
 this.height = width;
 }
 void setHeight(int height) {
 this.width = height;
 this.height = height;
 }
 }
 void renderLargeRectangles(List<Rectangle> rectangles) {
 for(Rectangle rectangle:rectangles) {
 rectangle.setWidth(4);
 rectangle.setHeight(5);
 int area = rectangle.getArea(); // BAD: Will return 25 for Square. Should be 20.
 rectangle.render(area);
 }
 }
 List<Rectangle> rectangles=new ArrayList<>();
 rectangles.add(new Rectangle());
 rectangles.add(new Rectangle());
 rectangles.add(new Square());
 renderLargeRectangles(rectangles);
```
 
**OK:**
```
 class Shape {
 void setColor(String color) {
 // ...
 }
 int getArea() {
 }
 void render(int area) {
 // ...
 }
 }
 class Rectangle extends Shape {
 private int width;
 private int height;
 public Rectangle(int width, int height) {
 super();
 this.width = width;
 this.height = height;
 }
 int getArea() {
 return this.width * this.height;
 }
 }
 class Square extends Shape {
 private int length;
 public Square(int length) {
 super();
 this.length = length;
 }
 int getArea() {
 return this.length * this.length;
 }
 }
 void renderLargeShapes(List<Shape> shapes) {
 for(Shape shape:shapes) {
 int area = shape.getArea();
 shape.render(area);
 }
 }
 List<Shape> shapes=new ArrayList<>();
 shapes.add(new Rectangle(4,5));
 shapes.add(new Rectangle(4,5));
 shapes.add(new Square(5));
 renderLargeShapes(shapes);
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Interface Isolation Principle (ISP)
 
The interface isolation principle says that "clients should not be forced to rely on interfaces they don't need."
 
**not good:**
```
 interface I {
 public void method1();
 public void method2();
 public void method3();
 public void method4();
 public void method5();
 }
 class A{
 public void depend1(I i){
 i.method1();
 }
 public void depend2(I i){
 i.method2();
 }
 public void depend3(I i){
 i.method3();
 }
 }
 class B{
 public void depend1(I i){
 i.method1();
 }
 public void depend2(I i){
 i.method4();
 }
 public void depend3(I i){
 i.method5();
 }
 }
 class C implements I{
 public void method1() {
 System.out.println("Class B implements method 1 of interface I");
 }
 public void method2() {
 System.out.println("Class B implements method 2 of interface I");
 }
 public void method3() {
 System.out.println("Class B implements method 3 of interface I");
 }
 //For class B, method4 and method5 are not necessary, but because there are these two methods in interface A,
 //So even if the method bodies of these two methods are empty during the implementation process, these two methods that have no effect must be implemented.
 public void method4() {}
 public void method5() {}
 }
 class D implements I{
 public void method1() {
 System.out.println("Class D implements method 1 of interface I");
 }
 //For class D, method2 and method3 are not necessary, but because there are these two methods in interface A,
 //So even if the method bodies of these two methods are empty during the implementation process, these two methods that have no effect must be implemented.
 public void method2() {}
 public void method3() {}
 public void method4() {
 System.out.println("Class D implements method 4 of interface I");
 }
 public void method5() {
 System.out.println("Class D implements method 5 of interface I");
 }
 }
```
 
**OK:**
```
 interface I1 {
 public void method1();
 }
 interface I2 {
 public void method2();
 public void method3();
 }
 interface I3 {
 public void method4();
 public void method5();
 }
 class A{
 public void depend1(I1 i){
 i.method1();
 }
 public void depend2(I2 i){
 i.method2();
 }
 public void depend3(I2 i){
 i.method3();
 }
 }
 class B{
 public void depend1(I1 i){
 i.method1();
 }
 public void depend2(I3 i){
 i.method4();
 }
 public void depend3(I3 i){
 i.method5();
 }
 }
 class C implements I1, I2{
 public void method1() {
 System.out.println("Class B implements method 1 of interface I1");
 }
 public void method2() {
 System.out.println("Class B implements method 2 of interface I2");
 }
 public void method3() {
 System.out.println("Class B implements method 3 of interface I2");
 }
 }
 class D implements I1, I3{
 public void method1() {
 System.out.println("Class D implements method 1 of interface I1");
 }
 public void method4() {
 System.out.println("Class D implements method 4 of interface I3");
 }
 public void method5() {
 System.out.println("Class D implements method 5 of interface I3");
 }
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Dependency Inversion Principle (DIP)
 
This principle states two important things:
 
1. High-level modules should not depend on low-level modules, both should be dependent and abstract;
2. Abstraction should not depend on concrete realization, concrete realization should depend on abstraction.
 
This will be difficult to understand at first, but if you have used Angular.js, you should have seen this achieved through dependency injection.
This principle, although they are not the same concept, relying on the principle of inversion to keep high-level modules away from the details and creation of low-level modules can be achieved through DI
to fulfill. The huge benefit of this is to reduce the coupling between the modules. Coupling is a very bad development model, because it makes the code difficult
Refactoring.
 
As mentioned above, JavaScript has no interfaces, so the abstraction that is relied on is an implicit contract. That is, the methods of an object/class and
The attribute is directly exposed to another object/class. In the following example, the implicit contract `InventoryTracker` of any Request module
There will be a `requestItems` method.
 
**not good:**
```
 class InventoryRequester {
 private String REQ_METHODS;
 public InventoryRequester() {
 this.REQ_METHODS = "HTTP";
 }
 void requestItem(String item) {
 // ...
 }
 }
 class InventoryTracker {
 private List<String> items;
 private InventoryRequester requester;
 public InventoryTracker(List<String> items) {
 this.items = items;
 // Bad: We have created a dependency on the specific implementation of the request, we only have a requestItems method depending on
 // Rely on a request method'request'
 this.requester = new InventoryRequester();
 }
 void requestItems() {
 this.items.stream().forEach(item->this.requester.requestItem(item));
 }
 }
 List<String> items=new ArrayList<>();
 items.add("apples");
 items.add("bananas");
 InventoryTracker inventoryTracker = new InventoryTracker(items);
 inventoryTracker.requestItems();
```
 
**OK:**
```
 interface Requester{
 void requestItem(String item);
 }
 class InventoryTracker {
 List<String> items;
 Requester requester;
 public InventoryTracker(List<String> items, Requester requester) {
 this.items = items;
 this.requester = requester;
 }
 void requestItems() {
 this.items.stream().forEach(item->requester.requestItem(item));
 }
 }
 class InventoryRequesterV1 implements Requester{
 String REQ_METHODS;
 public InventoryRequesterV1() {
 this.REQ_METHODS="HTTP";
 }
 @Override
 public void requestItem(String item) {
 // ...
 }
 }
 class InventoryRequesterV2 implements Requester{
 String REQ_METHODS;
 public InventoryRequesterV2() {
 this.REQ_METHODS="WS";
 }
 @Override
 public void requestItem(String item) {
 // ...
 }
 }
 // By creating external dependencies and injecting them, we can easily replace them with a brand new request module that uses WebSockets.
 List<String> items=new ArrayList<>();
 items.add("apples");
 items.add("bananas");
 InventoryTracker inventoryTracker = new InventoryTracker(items, new InventoryRequesterV2());
 inventoryTracker.requestItems();
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
## **TEST** 
 
Testing is more important than release. If you don't test or you don't test enough, you can't confirm that nothing breaks every time you publish.
The amount of testing is determined by your team, but having 100% coverage (including all statements and branches) is why you can achieve high confidence
And inner peace. This means that an additional great testing framework is needed, and a good [ coverage tool ](http://gotwarlost.github.io/istanbul/) is needed .
 
There is no reason not to write tests. There are [a lot of excellent JS testing frameworks ](http://jstherightway.org/#testing-tools),
Just choose the one that suits your team. After selecting the testing framework for the team, the next goal is to produce each new feature/module
Write tests in blocks. If you tend to test-driven development (TDD), that’s great, but the point is to make sure you’re launching any features or refactoring
Before constructing an existing function, the required target coverage was reached.
 
###  One test one concept
 
**not good:**
```
 void testMakeMomentJSGreatAgain(){
 Date date;
 date = new MakeMomentJSGreatAgain("1/1/2015");
 date.addDays(30);
 Assert.equal(date.getString(),"1/31/2015").
 date = new MakeMomentJSGreatAgain("2/1/2016");
 date.addDays(28);
 Assert.equal(date.getString(),"02/29/2016");
 date = new MakeMomentJSGreatAgain("2/1/2015");
 date.addDays(28);
 Assert.equal(date.getString(),"03/01/2015");
 }
```
 
**OK:**
```
 void testThirtyDayMonths(){
 Date date = new MakeMomentJSGreatAgain("1/1/2015");
 date.addDays(30);
 Assert.equal(date.getString(),"1/31/2015");
 }
 void testLeapYear(){
 Date date = new MakeMomentJSGreatAgain("2/1/2016");
 date.addDays(28);
 Assert.equal(date.getString(),"02/29/2016");
 }
 void testNonLeapYear(){
 Date date = new MakeMomentJSGreatAgain("2/1/2015");
 date.addDays(28);
 Assert.equal(date.getString(),"03/01/2015");
 }
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
 
## **Error Handling** 
 
Throwing errors is a good thing! They mean that when your program has an error, it can be successfully confirmed when it runs, and by stopping the execution of the current stack
The above function will let you know, terminate the current process (in Node), and prompt you with a stack trace in the console.
 
###  Don't ignore the caught errors
 
Doing nothing on the captured errors will not give you the ability to fix the errors or respond to the errors. Log errors to the console ( `console.log` )
It is not very good, because it is often lost in the massive console output. If you wrap any piece of code with `try/catch` then
It means that you think it might be wrong here, so you should have a fix plan, or a code path when the error occurs.
 
**not good:**
```
 try {
 functionThatMightThrow();
 } catch (Exception error) {
 console.log(error);
 }
```
 
**OK:**
```
 try {
 functionThatMightThrow();
 } catch (Exception error) {
 // One option (more noisy than console.log):
 console.error(error);
 // Another option:
 notifyUserOfError(error);
 // Another option:
 reportErrorToService(error);
 // OR do all three!
 }
```
 
 
## **Formatting** 
 
Formatting is subjective. Just like other rules, there are no hard and fast rules that you must follow. The point is not to argue about the format, this
There are [a lot of tools ](http://standardjs.com/rules.html) to format automatically, just use one of them! because
Arguing about formatting as an engineer is a waste of time and money.
 
For issues that the automatic formatting tool cannot cover (indentation, tabs or spaces, double quotes or single quotes, etc.), here are some guidelines.
 
###  Use consistent capitalization
 
JavaScript is untyped, so capitalization tells you a lot about your variables, functions, etc. These rules are subjective,
So your team can choose what they want. The point is, whatever you choose, be consistent.
 
**not good:**
```
 int DAYS_IN_WEEK = 7;
 int daysInMonth = 30;
 String[] songs = {"Back In Black", "Stairway to Heaven", "Hey Jude"};
 String[] Artists = {"ACDC", "Led Zeppelin", "The Beatles"};
 void eraseDatabase() {}
 void restore_database() {}
 class animal {}
 class Alpaca {}
```
 
**OK:**
```
 int DAYS_IN_WEEK = 7;
 int DAYS_IN_MONTH = 30;
 String[] songs = {"Back In Black", "Stairway to Heaven", "Hey Jude"};
 String[] artists = {"ACDC", "Led Zeppelin", "The Beatles"};
 void eraseDatabase() {}
 void restoreDatabase() {}
 class Animal {}
 class Alpaca {}
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
 
###  The caller and the callee of the function should be close
 
If one function calls another, the vertical positions of the two functions should be close in the code. Ideally, keep the called function in the
Right above the calling function. We tend to read the code from top to bottom, just like reading a chapter in a newspaper. For this reason, keep your code available
To read in this way.
 
**not good:**
```
 class PerformanceReview {
 String employee;
 public PerformanceReview(String employee) {
 this.employee = employee;
 }
 String lookupPeers() {
 return db.lookup(this.employee, "peers");
 }
 String lookupManager() {
 return db.lookup(this.employee, "manager");
 }
 void getPeerReviews() {
 String peers = this.lookupPeers();
 // ...
 }
 public void perfReview() {
 this.getPeerReviews();
 this.getManagerReview();
 this.getSelfReview();
 }
 void getManagerReview() {
 String manager = this.lookupManager();
 }
 void getSelfReview() {
 // ...
 }
 }
 
 PerformanceReview review = new PerformanceReview("user");
 review.perfReview();
```
 
**OK:**
```
class PerformanceReview {
 String employee;
 public PerformanceReview(String employee) {
 this.employee = employee;
 }
 void perfReview() {
 this.getPeerReviews();
 this.getManagerReview();
 this.getSelfReview();
 }
 void getPeerReviews() {
 String peers = this.lookupPeers();
 // ...
 }
 String lookupPeers() {
 return db.lookup(this.employee, "peers");
 }
 void getManagerReview() {
 String manager = this.lookupManager();
 }
 String lookupManager() {
 return db.lookup(this.employee, "manager");
 }
 public void getSelfReview() {
 // ...
 }
 }
 
 PerformanceReview review = new PerformanceReview("user");
 review.perfReview();
```
 
**[ ⬆ back to top ](#Code Neat-Java)**
 
## **Notes** 
 
###  Only comment on things that contain complex business logic
 
Comments are a justification for the code, not a requirement. In most cases, good code is documentation.
 
**not good:**
```
void hashIt(String data) {
 // The hash
 long hash = 0;
 // Length of string
 int length = data.length();
 // Loop through every character in data
 for (int i = 0; i <length; i++) {
 // Get character code.
 char mChar = data.charAt(i);
 // Make the hash
 hash = ((hash << 5)-hash) + mChar;
 // Convert to 32-bit integer
 hash &= hash;
 }
 }
```
 
**OK:**
```
 void hashIt(String data) {
 long hash = 0;
 int length = data.length();
 for (int i = 0; i <length; i++) {
 char mchar = data.charAt(i);
 hash = ((hash << 5)-hash) + mchar;
 // Convert to 32-bit integer
 hash &= hash;
 }
 }
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Don't save commented out code in the code base
 
Because there is version control, just leave the old code in the history.
 
**not good:**
```
 doStuff();
 // doOtherStuff();
 // doSomeMoreStuff();
 // doSoMuchStuff();
```
 
**OK:**
```
 doStuff();
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Do not have log-style comments
 
Remember, use version control! No need for zombie code, commented out code, especially log-style comments. Use `git log` to
Get history.
 
**not good:**
```
 /**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Removed type-checking (LI)
 * 2015-03-14: Added combine with type-checking (JR)
 */
 
 void combine(String a, String b) {
 return a + b;
 }
```
 
**OK:**
```
 void combine(String a, String b) {
 return a + b;
 }
```
**[ ⬆ back to top ](#Code Neat-Java)**
 
###  Avoid placeholders
 
They only add interference. Let function and variable names and proper indentation and formatting provide visual structure to your code.
 
**not good:**
```
 ///////////////////////////////////////////////// ///////////////////////////////
 // Scope Model Instantiation
 ///////////////////////////////////////////////// ///////////////////////////////
 String[] model = {"foo","bar"};
 ///////////////////////////////////////////////// ///////////////////////////////
 // Action setup
 ///////////////////////////////////////////////// ///////////////////////////////
 void action(){
 //...
 }
```
 
**OK:**
```
 String[] model = {"foo","bar"};
 void action(){
 //...
 }
```
 
**[ ⬆ back to top ](#Code Neat-Java)**
-
 
## **Tools** 
 
###  [ Lint ](https://developer.android.google.cn/studio/write/lint)
 
Android Studio provides a code scanning tool called Lint, which can help you find and correct problems with the quality of the code structure without actually executing the application or writing test cases. The system will report each problem detected by the tool and provide a description message and severity level of the problem so that you can quickly identify key improvements that need to be prioritized. In addition, you can lower the severity level of the problem to ignore issues that are not related to the project, or increase the severity level to highlight specific issues.
 
The Lint tool can check your Android project source files for potential errors, and whether they need to be optimized and improved in terms of correctness, security, performance, ease of use, accessibility, and internationalization.
 
![](https://user-gold-cdn.xitu.io/2020/3/4/170a34230f1219ec?w=604&h=287&f=png&s=29918)
 
Lint not only supports the Java language, but also supports the normative checks of Kotlin, C/C++, etc., and is the code scanning tool officially recommended by Android.
 
###  [ CheckStyle ](https://checkstyle.org/)
 
CheckStyle, as a plug-in for verifying code specifications, can not only use the development specifications given by the default configuration, such as Sun’s and Google’s development specifications, but also import plugins like Ali’s development specifications. In fact, every company has different development specification requirements, so most companies will give their own check specifications, which can be achieved by importing the given checkstyle.xml file. CheckStyle assists in judging whether the code format meets the specifications and ensures that the code style developed by team members is consistent.
 
The main content of CheckStyle inspection
- Javadoc comments
- Naming Conventions
- Title
- Import Statement
- Volume Size
- blank
- Modifiers
- Block
- code issues
- Class Design
- Mixed examination (including some useful such as non-essential System.out and printstackTrace)
 
As can be seen from the above, most of the functions provided by CheckStyle are checks on code specifications. But CheckStyle currently only supports checking the Java language.
 
 
![](https://user-gold-cdn.xitu.io/2020/3/4/170a3a215261376c?w=1510&h=778&f=png&s=223360)
 
###  [ SonarLint ](https://www.sonarlint.org/)
 
SonarLint is an IDE extension that can help you detect and fix quality issues when writing code, and fix them before submitting the code.
 
- Error detection
 
Benefit from thousands of existing rules; common errors, tricky errors and known vulnerabilities can be detected
 
- instant feedback
 
Just like a spell checker, problems will be detected and reported when coding
 
- Extensive documentation
 
Accurately point out the problem and provide you with suggestions for solutions
 
![](https://user-gold-cdn.xitu.io/2020/3/4/170a47f863d2a8ef?w=2746&h=1306&f=png&s=389802)
 
###  [ Alibaba Java Coding Guidelines ](https://github.com/alibaba/p3c)
 
Alibaba officially released the scanning plug-in of the "Alibaba Java Development Protocol" at the Hangzhou Yunqi Conference on October 14. After the plug-in scans the code, the code that does not conform to the specification is displayed in the following three levels: Blocker/Critical/Major. Even on IDEA, the plug-in also provides real-time detection based on the inspection mechanism, which allows you to write code quickly while at the same time. Found the problem. For historical codes, some rules implement batch one-click repair.
 
- Blocker
 
That is, the system cannot be executed, crashed or is severely insufficient in resources, application modules cannot be started or abnormally exited, cannot be tested, and the system is unstable.
Severe screen display, memory leak, user data loss or destruction, system crash/freeze/freeze, module failure to start or exit abnormally, serious numerical calculation errors, serious discrepancies between function design and requirements, and other errors that cannot be tested, such as server 500 errors
 
- Critical
 
That is to say, the system function or operation is affected, and the main function has serious defects, but it will not affect the system stability.
Function not implemented, function error, system refresh error, data communication error, slight numerical calculation error, incorrect words or spelling errors that affect the function and interface, security issues
 
- Major
 
Namely interface, performance defect, compatibility. Operation interface errors (including the definition of column names in the data window and whether the meanings are consistent), errors under boundary conditions, prompt information errors (including no information, information prompt errors, etc.), long-term operation without progress prompts, system not optimized (performance Problem), poor cursor jump settings, wrong mouse (cursor) positioning, compatibility issues
 
![](https://user-gold-cdn.xitu.io/2020/3/4/170a39d986563f1b?w=635&h=308&f=png&s=13685)
 
**Different code checking tools have the same principle, but different focuses. In actual project development, you can choose and combine according to your own team and project situation, and define your own team specifications. **
 
**[ ⬆ back to top ](#Code Neat-Java)**
-
 
#  Puzzle
Under the pressure of deadlines, programmers had to pursue fast development speed, so they created chaos for the code, but thought that they could not be faster because of this.
 
Creating chaos will only slow you down immediately and make you miss the deadline. The only way to meet deadlines and do it fast is to always keep the code as clean as possible.
