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
