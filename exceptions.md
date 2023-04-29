## Задача 1. Проверка доступа к ресурсу

### Описание
В этом задании мы напишем программу для проверки доступа к ресурсу. Во время запуска программы нужно запросить логин или пароль пользователя. Если один из введеных параметров не совпадает (логин/пароль), то нужно выбросить checked исключение UserNotFoundException. Если возраст пользователя менее 18 лет, то нужно выбросить исключение AccessDeniedException, а если 18 и больше лет - вывести сообщение "Доступ предоставлен". <br>
<br>
Массив пользователей для авторизации нужно описать до запуска программы. Каждая запись пользователя содержит поля: login, password, age (возраст) и email.
 
### Функционал программы
1. Создание Scanner для чтения логина и пароля пользователя из консоли;
2. Создание checked исключения UserNotFoundException;
3. Создание checked исключения AccessDeniedException;
4. Выбрасывать ошибку UserNotFoundException, если логин или пароль введены не верно;
5. Выбрасывать ошибку AccessDeniedException, если возраст пользователя меньше 18 лет;
5. Если ошибок не возникло, вывести сообщение "Доступ предоставлен".

### Процесс реализации
1. Создадим класс User, в котором будем хранить инфомрацию о логине, пароле и возрасте пользователя: 
class User, login, password, email, age;
2. Создадим класс исключение UserNotFoundException на основе базового класса Exception. Это исключение будем использовать, если пользователь ввел неверный логин или пароль:
```java
public class UserNotFoundException extends Exception {
    public UserNotFoundException(String message) {
        super(message);
    }
}
```
3. Аналогичным образом создадим класс исключения AccessDeniedException
4. Создадим класс Main, в котором создадим метод getUsers, этот метод должен возвращать список заранее созданных пользователей:
```java
public static User[] getUsers() {
    User user1 = new User("jhon", "jhon@gmail.com", "pass", 24);
    ...
    return new User[]{user1, ...};
}
```
5. Создадим в классе Main метод getUserByLoginAndPassword(String login, String password), в этом методе нужно найти соответствие пары логина и пароля пользователя из массива, возвращаемого методом getUsers. Если пользователь не найден, выбрасываем исключение UserNotFoundException, если найден - возвращаем найденного пользователя:
```java
public static User getUserByLoginAndPassword(String login, String password) throws UserNotFoundException {
    User[] users = getUsers();
    for (User user : users) {
        ...
    }
    throw new UserNotFoundException("User not found");    
}   
```
6. Создадим к классу Main еще один метод validateUser для проверки возрастра пользователя. Если возраст менее 18 лет, метод должен выбросить исключение AccessDeniedException:
```java
public static void validateUser(User user) throws AccessDeniedException
``` 
7. Добавим последний метод в классе Main для запуска программы public static void main(String[] args) throws UserNotFoundException, AccessDeniedException
В нем нужно запросить логин и пароль пользователя, проверить есть ли данная пара "логин и пароль" в массиве пользователей и последним шагом провалидировать возраст.
```java
    public static void main(String[] args) throws UserNotFoundException, AccessDeniedException {

        Scanner scanner = new Scanner(System.in);

        System.out.println("Введите логин");
        String login = scanner.nextLine();
        System.out.println("Введите пароль");
        String password = scanner.nextLine();

        //Проверить логин и пароль
        
        //Вызвать методы валидации пользователя
        
        System.out.println("Доступ предоставлен");
    }

```
8. Программа завершена. Отличная работа!

ОТВЕТ:

```java 
public class User {
   private String login;
   private String password;
   private String email;
   private int age;

    public User(String login, String email, String password, int age) {
        this.login = login;
        this.password = password;
        this.age = age;
        this.email = email;
    }

    public String getLogin() {
        return login;
    }

    public String getPassword() {
        return password;
    }

    public String getEmail() {
        return email;
    }

    public int getAge() {
        return age;
    }
}

public class UserNotFoundException extends Exception {
    public UserNotFoundException(String message) {
        super(message);
    }
}

public class AccessDeniedException  extends Exception {
    public AccessDeniedException(String message) {
        super(message);
    }
}

import java.util.Scanner;

public class Main {

        public static User[] getUsers() {
            User user1 = new User("jhon", "jhon@gmail.com", "pass", 24);
            User user2 = new User("jane", "jane@gmail.com", "pass123", 17);
            User user3 = new User("bob", "bob@gmail.com", "password", 31);
            return new User[]{user1, user2, user3};
        }


    public static User getUserByLoginAndPassword(String login, String password) throws UserNotFoundException {
        User[] users = getUsers();
        for (User user : users) {
            if (user.getLogin().equalsIgnoreCase(login) && user.getPassword().equals(password)) {
                return user;
            }
        }
            throw new UserNotFoundException("User not found");
        }

    public static void validateUser(User user) throws AccessDeniedException {

        if (user.getAge() < 18) {
            throw new AccessDeniedException("Доступ запрещен");
        }
    }
    public static void main(String[] args) {

        Scanner scanner = new Scanner(System.in);

        try {
            System.out.println("Введите логин");
            String login = scanner.nextLine();
            System.out.println("Введите пароль");
            String password = scanner.nextLine();

            User user = getUserByLoginAndPassword(login, password);
            validateUser(user);

            System.out.println("Доступ предоставлен");
        } catch (UserNotFoundException e) {
            System.out.println(e.getMessage());
        } catch (AccessDeniedException e) {
            System.out.println(e.getMessage());
        } finally {
            scanner.close();
        }
    }


    }




```
