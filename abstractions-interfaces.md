## Задача 1. Библиотека

### Описание
Необходимо разработать иерархию работников библиотеки. Нужно реализовать совмещение нескольких ролей в библиотеке в одном исполнителе через интерфейсы. Каждый объект в программе имеет определенный набор действий. 

Часто программист, создающий объект, не представляет все ситуации, в которых тот будет использоваться.
Также программисту, использующему объект, часто неизвестны все его детали. 

Для передачи информации о том, что должен уметь объект, используются интерфейсы.

Примером интерфейсов в нашей библиотеке может служить понятие роли на проекте.
Каждая роль подразумевает набор определенных операций, которые должен "уметь" объект пользователь — User в программе.

### Функционал программы
1. Создайте иерархию "Пользователи библиотеки" со следующими интерфейсами:
  - Читатель – берет и возвращает книги.
  - Библиотекарь – заказывает книги.
  - Поставщик книг – приносит книги в библиотеку.
  - Администратор – находит и выдает книги и уведомляет о просрочках времени возврата.
2. В методе `public static void main` создайте 2-3 объекта, реализующих эти интерфейсы.

### Дополнительная информация
Учтите, что интерфейсов у пользователя (`User`) может быть несколько.
Объекты класса User могут взаимодействовать друг с другом (например, библиотекарь заказывает у поставщика).

### Пример реализации
1. Создайте 4 интерфейса: `Reader`, `Librarian`, `Supplier`, `Administrator`.
Каждый из них должен содержать описанные выше методы. Например, у администратора должен быть метод `overdueNotification(Reader reader)`.
Методы могут принимать в качестве параметра других user-ов. Например, читатель берет книги у администратора, библиотекарь заказывает у поставщика и т.д.

2. Создайте несколько классов, демонстрирующих использование интерфейсов.
В частности, продемонстрируйте совмещение, например, поставщик, который может быть также и читателем, библиотекарь – администратором и т.д.
В качестве реализации методов можно сделать вывод подробного сообщения в консоль по типу "Библиотекарь Вася заказал у поставщика Петя книгу Игра Престолов".

ОТВЕТ:

```java 
public class Main {
    public static void main(String[] args) {
        Librarian librarian = new Librarian("John");
        librarian.orderBook();
        Supplier supplier = new Supplier("Orda");
        supplier.bringBook();
        Reader reader = new Reader("Steve");
        reader.takeBook();

        librarian.giveBook();


        Administrator administrator = new Administrator("Alex");
        administrator.overdueNotification(reader);



    }
}

public class User {
String name;

public User (String name){
    this.name=name;
};
    public String getName() {
        return name;

}
}

public interface IAdmin {
     void findBook();
     void giveBook();
    void overdueNotification(Reader reader);
}

public interface IReader {
     void takeBook();
    void returnBook();
}

public interface ISupplier {
     void bringBook ();
}

public interface ILibrarian {
    void orderBook ();
}

public class Administrator extends User implements IAdmin, ILibrarian {
    public Administrator(String name) {
        super(name);
    }

    @Override
    public void findBook() {
        System.out.println("Администратор нашел книгу");
    }

    @Override
    public void giveBook() {
        System.out.println("Администратор отдал книгу");
    }

    @Override
    public void overdueNotification(Reader reader) {
        System.out.println("Администратор уведомил пользователя - " + reader.getName());
    }

    @Override
    public void orderBook() {
        System.out.println("Администратор закзал книгу");
    }

    public void overdueNotification(Supplier supplier) {
        System.out.println("Администратор уведомил пользователя - " + supplier.getName());
    }
}

public class Supplier extends User implements ISupplier, IReader {
    public Supplier(String name) {
        super(name);
    }

    @Override
    public void bringBook() {
        System.out.println("Поставщик ппривез книги в библиотеку");
    }

    @Override
    public void takeBook() {
        System.out.println("Поставщик взял книгу");
    }

    @Override
    public void returnBook() {
        System.out.println("Поставщик сделал возврат книги");
    }
}

public class Librarian extends User implements ILibrarian, IAdmin {

    public Librarian(String name) {
        super(name);
    }

    @Override
    public void findBook() {
        System.out.println("Библиотекарь нашел книгу");
    }

    @Override
    public void giveBook() {
        System.out.println("Библиотекарь отдал книгу");
    }

    @Override
    public void overdueNotification(Reader reader) {
        System.out.println("Библиотекарь уведомил пользователя - " + reader.getName());
    }
    public void overdueNotification(Supplier supplier) {
        System.out.println("Администратор уведомил пользователя - " + supplier.getName());
    }
    @Override
    public void orderBook() {
        System.out.println("Библиотекарь заказал книгу");
    }
}

public class Reader extends User implements IReader {
    public Reader(String name) {
        super(name);
    }

    @Override
    public void takeBook() {
        System.out.println("Читатель взял книгу ");
    }

    @Override
    public void returnBook() {
        System.out.println("Читатель сделал возврат книги");
    }


}


```

## Задача 2. Банковские счета

### Описание
Часто в проектировании программ нам удобно опираться на понятия, которые не представлены в реальном мире,
но служат удобной "опорой" для объединения нескольких классов.

Так, например, в банковском деле нет абстрактного понятия "Счет". Каждый счет в банке имеет четкое назначение: сберегательный, кредитный, расчетный.
Но банковская программа работает с общими для счетов операциями как с одинаковыми объектами, и выполняет их, обращаясь к общему типу "Счет",
хотя его и невозможно явно инстанцировать в программе. Реализуйте этот сценарий, опираясь на механизмы полиморфизма.

### Функционал программы
1. Создайте несколько классов — различных счетов на основе общего интерфейса:
  - Сберегательный счет (`SavingsAccount`)
  - Кредитный счет (`CreditAccount`)
  - Расчетный счет (`CheckingAccount`)
2. Выполните перевод с одного счета на другой в методе `public static void main`.

### Пример реализации
1. Создайте абстрактный класс `Account` с тремя методами: заплатить, перевести, пополнить (`pay(int amount)`, `transfer(Account account, int amount)`, `addMoney(int amount)`).
Платеж в нашем случае будет выглядеть просто как списание средств.
2. Добавьте классы Сберегательный, Кредитный, Расчетный (`SavingsAccount`, `CreditAccount`, `CheckingAccount` соответственно) как потомков класса Счет.
В них нужно переопределить методы базового класса. Каждый из них должен хранить баланс. Со сберегательного счета нельзя платить, только переводить и пополнять. Также сберегательный не может уходить в минус.
Кредитный не может иметь положительный баланс – если платить с него, то уходит в минус, чтобы вернуть в 0, надо пополнить его.
Расчетный может выполнять все три операции, но не может уходить в минус.
3. Продемонстрируйте работу счетов. Создайте три переменные типа `Account` и присвойте им три разных типа счетов.

ОТВЕТ:

```java 
public class Main {

    public static void main(String[] args) {
        Account savingsAccount = new SavingsAccount(1000);
        Account creditAccount = new CreditAccount(0);
        Account checkingAccount = new CheckingAccount(2000);


        System.out.println("Initial balances:");
        System.out.println("Savings account: " + savingsAccount.getBalance());
        System.out.println("Credit account: " + creditAccount.getBalance());
        System.out.println("Checking account: " + checkingAccount.getBalance());

        savingsAccount.transfer(checkingAccount, 500);
        creditAccount.pay(501);
        System.out.println("Credit account: " + creditAccount.getBalance());
        checkingAccount.transfer(savingsAccount, 1000);
        creditAccount.addMoney(501);
        System.out.println("Credit account: " + creditAccount.getBalance());
        creditAccount.addMoney(23);
        savingsAccount.pay(232);

        System.out.println("Final balances:");
        System.out.println("Savings account: " + savingsAccount.getBalance());
        System.out.println("Credit account: " + creditAccount.getBalance());
        System.out.println("Checking account: " + checkingAccount.getBalance());

        savingsAccount.transfer(checkingAccount, 1600);
        System.out.println("Savings account: " + savingsAccount.getBalance());

        checkingAccount.transfer(savingsAccount, 1600);
        System.out.println("Checking account: " + checkingAccount.getBalance());

        checkingAccount.addMoney(2323);
        System.out.println("Checking account: " + checkingAccount.getBalance());
    }
}


abstract class Account {
    protected int balance;

    public Account(int balance) {
        this.balance = balance;
    }

    public void pay(int amount) {
        balance = balance - amount;
    }

    public void transfer(Account account, int amount) {
        balance = balance - amount;
        account.addMoney(amount);
    }

    public void addMoney(int amount) {
        balance = balance + amount;
    }

    public int getBalance() {
        return balance;
    }

}

class SavingsAccount extends Account {

    public SavingsAccount(int balance) {
        super(balance);
    }
    @Override
    public void pay(int amount) {
        System.out.println("Ошибка");
    }

    @Override
    public void transfer(Account account, int amount) {
        if (balance - amount >= 0) {
            balance = balance - amount;
            account.addMoney(amount);
        } else {
            System.out.println("Недостаточно средств на сберегательном");
        }
    }

}

class CreditAccount extends Account {
    public CreditAccount(int balance) {
        super(balance);
    }

    @Override
    public void pay(int amount) {
        if (balance - amount <= 0) {
            balance = balance - amount;
        }
    }

    @Override
    public void addMoney(int amount) {
        if (balance < 0) {
            balance += amount;
            if (balance > 0) {
                balance = 0;

            }
        } else {
            System.out.println("У вас нет долгов");
        }
    }
}

class CheckingAccount extends Account {

    public CheckingAccount(int balance) {
        super(balance);
    }
    @Override
    public void pay(int amount) {
        if (balance - amount >= 0) {
            balance = balance - amount;
        } else {
            System.out.println("Недостаточно средств на расчетном");
        }
    }
    @Override
    public void transfer(Account account, int amount) {
        if (balance - amount >= 0) {
            balance = balance - amount;
            account.addMoney(amount);
        } else {
            System.out.println("Недостаточно средств на расчетном");
        }
    }
}


```
## Задача 3. Доп задания
1. Создайте абстрактный класс Shape, который содержит абстрактные методы area() и perimeter(), возвращающие double. Создайте классы Rectangle и Circle, которые наследуются от класса Shape и реализуют его абстрактные методы.

2. Создайте интерфейс Drawable с одним методом draw(). Реализуйте этот интерфейс в классах Rectangle и Circle из предыдущей задачи.

3. Создайте абстрактный класс Animal с абстрактным методом makeSound(). Создайте интерфейс Swimmable с методом swim(). Создайте классы Dog, Cat, и Duck, которые наследуются от класса Animal и реализуют его абстрактный метод makeSound(). Класс Duck должен также реализовывать интерфейс Swimmable.

4. Создайте интерфейс Growable с методами grow() и shrink(). Добавьте дефолтный метод resetSize(), который возвращает объект к его начальному размеру. Создайте классы Plant и Animal, реализующие интерфейс Growable.

5. Создайте класс Student, содержащий имя и оценку. Реализуйте интерфейс Comparable<Student> в классе Student, сравнивая студентов по их оценкам. Создайте список студентов и отсортируйте его.
  
ОТВЕТЫ:

```java 
  public class Main {

    public static void main(String[] args) {
        Rectangle rectangle = new Rectangle(10, 5);
        System.out.println("Rectangle area: " + rectangle.area());
        System.out.println("Rectangle perimeter: " + rectangle.perimeter());
        rectangle.draw();

        Circle circle = new Circle(3);
        System.out.println("Circle area: " + circle.area());
        System.out.println("Circle perimeter: " + circle.perimeter());
        circle.draw();
    }
}





public abstract class Shape {
    public abstract double area();
    public abstract double perimeter();
}

public interface Drawable {
    void draw ();
}

public class Rectangle extends Shape implements Drawable{
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    @Override
    public double area() {
        return width*height;
    }

    @Override
    public double perimeter() {
        return 2 * (width + height);
    }

    @Override
    public void draw() {
        System.out.println("Drawing a rectangle");
    }
}

public class Circle extends Shape implements Drawable{
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double area() {
        return Math.PI * radius * radius;
    }

    @Override
    public double perimeter() {
        return 2 * Math.PI * radius;
    }

    @Override
    public void draw() {
        System.out.println("Drawing a circle");
    }

    }
3
public class Main {

    public static void main(String[] args) {
   Dog dog = new Dog();
   Cat cat = new Cat();
   Duck duck = new Duck();

   dog.makeSound();
   cat.makeSound();
   duck.swim();
    }
}

public abstract class Animal {
    public abstract void makeSound();
}

public interface Swimmable {
    void swim();
}

public class Duck extends Animal implements Swimmable{
    @Override
    public void makeSound() {
        System.out.println("Кря кря");
    }

    @Override
    public void swim() {
        System.out.println("Утка плывет");
    }
}

public class Dog extends Animal{
    @Override
    public void makeSound() {
        System.out.println("Гав гав");
    }
}

public class Cat extends Animal{
    @Override
    public void makeSound() {
        System.out.println("Мяу");
    }
}

4
	public interface Growable {
    void grow();
    void shrink();

    default void resetSize() {
        System.out.println("Размер обнулился");
    }
}

public class Plant implements Growable {
    private int size;

    public Plant(int size) {
        this.size = size;
    }

    @Override
    public void grow() {
        size += 1;
    }

    @Override
    public void shrink() {
        size -= 1;
    }

    @Override
    public void resetSize() {
        size = 0;
    }
}

public class Animal implements Growable{
    private int size;

    public Animal(int size) {
        this.size = size;
    }

    @Override
    public void grow() {
        size += 1;
    }

    @Override
    public void shrink() {
        size -= 1;
    }

    @Override
    public void resetSize() {
        size = 0;
    }
}
5.


```
