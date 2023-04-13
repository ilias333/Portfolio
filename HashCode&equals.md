#Задачи
Создайте класс Person с полями firstName, lastName и age. Переопределите методы hashCode() и equals() для корректного сравнения объектов класса Person по содержимому полей.

Создайте класс Book с полями title, author и isbn. Переопределите методы hashCode() и equals() для корректного сравнения объектов класса Book по значению поля isbn.

ОТВЕТ:

```java 
public class Main {

    public static void main(String[] args) {
        Person l1 = new Person("Alex", "Sidorov");
        Person l2 = new Person("Alex", "Sidorov");
        System.out.println(l1.hashCode());
        System.out.println(l2.hashCode());
        System.out.println(l1.hashCode() == l2.hashCode());
    }
}

import java.util.Objects;

public class Person {
    private String firstName;
    private String lasName;

    public Person(String firstName, String lasName) {
        this.firstName = firstName;
        this.lasName = lasName;
    }
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return firstName.equals(person.firstName) && lasName.equals(person.lasName);
    }

    @Override
    public int hashCode() {
        return Objects.hash(firstName, lasName);
    }
}
```

```java 

public class Main {

    public static void main(String[] args) {
        Book l1 = new Book ("Alex", "Sidorov", "32");
        Book  l2 = new Book ("Alex", "Sidorov", "33");
        System.out.println(l1.hashCode());
        System.out.println(l2.hashCode());
        System.out.println(l1.hashCode() == l2.hashCode());
    }
}

import java.util.Objects;

public class Book {
    private String title;
    private String author;
    private String isbn;

    public Book(String title, String author, String isbn) {
        this.title = title;
        this.author = author;
        this.isbn = isbn;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Book book = (Book) o;
        return isbn.equals(book.isbn);
    }

    @Override
    public int hashCode() {
        return Objects.hash(isbn);
    }



}


```
