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

        Book book = new Book("The Lord of the Rings");
        BookMover fromAvailableStatusMover = new FromAvailableStatusMover();
        fromAvailableStatusMover.moveToStatus(book, Status.BORROWED);
        System.out.println(book.getStatus());
        BookMover fromArchivedStatusMover = new FromArchivedStatusMover();
        fromArchivedStatusMover.moveToStatus(book, Status.AVAILABLE);
        System.out.println(book.getStatus());
        BookMover fromOverduedStatusMover = new FromOverduedStatusMover();
        fromOverduedStatusMover.moveToStatus(book, Status.ARCHIVED);
        System.out.println(book.getStatus());
        BookMover fromBorrowedStatusMover = new FromBorrowedStatusMover();
        fromBorrowedStatusMover.moveToStatus(book, Status.OVERDUED);
        System.out.println(book.getStatus());

    }
}

public enum Status {
    AVAILABLE, BORROWED, OVERDUED, ARCHIVED
}

public class Book{
    private String title;
    public Status status;

    Book(String title, Status status){
        this.title=title;
        this.status=status;}

    Book(String title){
        this.title=title;}

    public Status getStatus(){
        return status;

    }
}

public class BookMover {
    protected void moveToStatus(Book book, Status requestedStatus) {
        System.out.println("Moving status...");
    }
}

public class FromOverduedStatusMover extends BookMover {
    @Override
    public void moveToStatus(Book book, Status requestedStatus) {
        {
            switch (requestedStatus) {

                case AVAILABLE:
                    book.status = Status.AVAILABLE;
                    break;

                case BORROWED:
                    break;
                default:
                    System.out.println("Из текущего статуса OVERDUED книгу нельзя перевести в BORROWED");
                    break;

                case ARCHIVED:
                    book.status = Status.ARCHIVED;
                    break;

                case OVERDUED:
                    System.out.println("Из текущего статуса OVERDUED книгу нельзя перевести в OVERDUED");
                    break;
            }
        }
    }
}

public class FromBorrowedStatusMover extends BookMover {

    @Override
    public void moveToStatus(Book book, Status requestedStatus) {
        if (book.getStatus() == Status.BORROWED){
            switch (requestedStatus){
                case AVAILABLE:
                    book.status = Status.AVAILABLE;
                    break;
                case BORROWED:
                    System.out.println("Из текущего статуса BORROWED книгу нельзя перевести в BORROWED");
                    break;
                case ARCHIVED:
                    book.status = Status.ARCHIVED;
                    break;
                case OVERDUED:
                    book.status = Status.OVERDUED;
                    break;
            }
        }
    }
}

public class FromAvailableStatusMover extends BookMover {

    @Override
    public void moveToStatus(Book book, Status requestedStatus) {
        switch (requestedStatus){

            case AVAILABLE:
                break;
            default:
                System.out.println("Из текущего статуса AVAILABLE книгу нельзя перевести в AVAILABLE");
                break;
            case BORROWED:
                book.status = Status.BORROWED;
                break;
            case ARCHIVED:
                book.status = Status.ARCHIVED;
                break;
            case OVERDUED:
                System.out.println("Из текущего статуса AVAILABLE книгу нельзя перевести в OVERDUED");
                break;
        }
    }
}

public class FromArchivedStatusMover extends BookMover{
    @Override
    public void moveToStatus(Book book, Status requestedStatus) {
        switch (requestedStatus){
            case ARCHIVED:
                System.out.println("Из текущего статуса ARCHIVED книгу нельзя перевести в ARCHIVED");
                break;
            case AVAILABLE:
                book.status = Status.AVAILABLE;
                break;
            case BORROWED:
                System.out.println("Из текущего статуса ARCHIVED книгу нельзя перевести в BORROWED");
                break;
            case OVERDUED:
                System.out.println("Из текущего статуса ARCHIVED книгу нельзя перевести в OVERDUED");
                break;
        }
    }
}

