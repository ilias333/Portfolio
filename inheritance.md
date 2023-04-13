## Задача 1. Иерархия "Статусы книг"

### Описание
Рассмотрим предметную область обычной библиотеки. 
У каждой книги есть один из следующих статусов: взято в пользование (`BORROWED`), доступно (`AVAILABLE`),
задержана (`OVERDUED`), в архиве (`ARCHIVED`).

Из одного статуса доступно только ограниченное число статусов. 
Диаграмма переходов указана здесь: 
![](https://i.imgur.com/EpJ0tOb.jpg)

Необходимо с помощью наследования реализовать программу, в которой будет 4 наследника базового класса `BookMover` по переводу статуса книги из одного в другой.
Данный функционал пригодится в случае массового перевода книг в какой-то статус, но мы пока рассмотрим перевод только одной книги.

Также необходимо будет описать класс Book с базовым набором полей 'title' и 'status'.

### Функционал программы
1. Создание книги с начальным статусом `AVAILABLE`;
2. Возможноcть перевода из статуса `AVAILABLE` только в статус `BORROWED` или `ARCHIVED` (так же в другие статусы согласно схеме);
3. В случае перевода книги в недопустимый для нее статус вывести сообщение: "Перевод книги из статуса 'X' в статус 'Y' невозможен".

### Процесс реализации
1. Создайте Enum класс `Status` с 4 возможными статусами в нашей программе.
2. Создайте класс `BookMover` с дефолтной реализацией метода `moveToStatus`. 
```
protected void moveToStatus(Book book, Status requestedStatus) {
    System.out.println("Moving status...");
}
```
3. Создайте 4 наследника данного класса. 
Например: `FromArchievedStatusMover` — класс, в котором будут происходить проверка и переход книги из статуса `ARCHIVED` в запрашиваемый статус, если он возможен. Если он невозможен, то вывести сообщение: "Перевод книги из статуса 'X' в статус 'Y' невозможен".
Проверку доступности необходимо сделать, используя Enum, созданный на первом шаге, оператор switch и диаграмму переходов.
4. В классе Main.java необходимо будет создать объект класса Book, используя конструктор. Убедитесь, 
что функции перехода были реализованы верно и статус у книги корректный. Например:

```
   Book book = new Book("The Lord of the Rings");
   BookMover fromAvailableStatusMover = new FromAvailableStatusMover();
   fromAvailableStatusMover.moveToStatus(book, Status.BORROWED);
   System.out.println(book.getStatus());
```
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

