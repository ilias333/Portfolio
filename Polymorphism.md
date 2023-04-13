## Задача 1. Игра-шутер

### Описание
Один из проектов — это игра-шутер (~~Half-Life 3, только никому ни слова~~).
У игрока должна быть возможность использовать разные виды оружия, в будущем в игру могут быть добавлены новые.
Необходимо спроектировать иерархию классов, а также систему слотов для оружия у игрока.

### Функционал программы
1. Создание объекта Player у которого будет набор оружия;
2. Возможность у игрока вызвать метод выстрела, внутри которого будут проверки на допустимость номера оружия для выстрела;
3. Классы оружия должны быть в пакете `weapon` (вспомните какие ДВЕ вещи нужно сделать, чтобы поместить классы в пакеты; мы их проходили на втором занятии);
4. Возможность выбора оружия для выстрела в main.

### Процесс реализации

1. Создадим класс игрока и функцию main.

* Класс Player содержит список оружия и метод "_выстрелить_"
```java 
class Player {
    // Указываем тип данных Weapon, который будет храниться в "слотах игрока" 
    private Weapon[] weaponSlots;
    
    public Player() {
        // Снаряжаем нашего игрока
        weaponSlots = new Weapon[] {
            // TODO заполнить слоты оружием
            // new BigGun(),
            // new WaterPistol()
        };
    }
    
    public int getSlotsCount() {
        // length - позволяет узнать, сколько всего слотов с оружием у игрока
        return weaponSlots.length;
    }
    
    public void shotWithWeapon(int slot) {
        // TODO если значение slot больше значения последнего элемента массива, то
        // выйти из метода написав об этом в консоль
        
        // Получаем оружие из выбранного слота
        Weapon weapon = weaponSlots[slot];
        // Огонь!
        weapon.shot();
    }
}
```

* Метод `main`
```java
public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
        Player player = new Player();
        // Как настоящие программисты мы имеем в виду, что исчисление начинается с 0
        System.out.format("У игрока %d слотов с оружием,"
            + " введите номер, чтобы выстрелить,"
            + " -1 чтобы выйти%n", 
            player.getSlotsCount()
        );
        int slot;
        
        // TODO главный цикл игры: 
        // запрашивать номер с клавиатуры 
        // с помощью scanner.nextInt(),
        // пока не будет введено -1
        
        System.out.println("Game over!");
}
```

2. Создадим классы для некоторых видов оружия.
* Базовый класс для всех видов оружия
```java
class Weapon {
    public void shot() {
        // TODO override me!
    }
}
```

> Как "заставить" дочерний класс переопределить поведение некоторых методов базового класса, мы узнаем на следующей лекции.

* Создадим дочерние классы:
    * Пистолет;
    * Автомат;
    * РПГ;
    * Рогатка;
    * Водный пистолет.

* В каждом из дочерних классов переопределите метод `shot()`, чтобы он изменил поведение оружия в соответствии с его типом. Например, чтобы оно выводило в консоль соответствующие выстрелу звуки: `Пив-Пав!`.

3. Теперь можно вернуться к классу `Player` и создать по экземпляру каждого оружия.
4. Не забудьте разграничить классы по типу с помощью пакетов, например, все классы с оружием можно вынести в package `weapon`.
```java 
import Game.Player;

import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Player player = new Player();
        // Как настоящие программисты мы имеем в виду, что исчисление начинается с 0
        System.out.format("У игрока %d слотов с оружием,"
                        + " введите номер, чтобы выстрелить,"
                        + " -1 чтобы выйти%n",
                player.getSlotsCount()
        );
        int slot;

        while (true) {
            // Запрашиваем номер оружия у игрока
            slot = scanner.nextInt();
            if (slot == -1) {
                break;
            }
            player.shotWithWeapon(slot);

            System.out.println("Game over!");
        }
    }
}

package Game;

import weapons.*;

public class Player {
    // Указываем тип данных Weapon, который будет храниться в "слотах игрока"
    private Weapon[] weaponSlots;

    public Player() {
        weaponSlots = new Weapon[] {
        new Automate(), new Pistol()
        };
    }

    public int getSlotsCount() {
        // length - позволяет узнать, сколько всего слотов с оружием у игрока
        return weaponSlots.length;
    }

    public void shotWithWeapon(int slot) {
        if (slot < 0) {
            System.out.println("Неверный номер слота!");
            return;
        } else if (slot >= weaponSlots.length) {
            System.out.println("Неверный номер слота!");
            return;
        }
        Weapon weapon = weaponSlots[slot];
        // Огонь!
        weapon.shot();
    }
}
package weapons;

public class Weapon {
    public void shot() {
        System.out.println("Пиу-пиу!");
    }
}



package weapons;

public class Automate extends Weapon {
    @Override
    public void shot() {
        System.out.println("Рататата!");
    }
}

package weapons;

public class Pistol extends Weapon {
    @Override
    public void shot() {
        System.out.println("Паф-паф!");
    }
}


```


## Задача 2. Задача от бухгалтеров

### Описание
Следующая задача пришла от наших бухгалтеров.
Бухгалтерская программа должна уметь проводить операции c различными агентами, как c физическими/юридическими лицами, так и с иностранными компаниями: чп, ип, ооо, зао, ~~иклмн~~, ~~ёпрст~~.
С некоторых операций налог платить не нужно, некоторые облагаются подоходным налогом, с некоторых необходимо уплатить НДС.
Необходимо расширить функциональность класса `Bill` возможностью работы с различными системами налогообложения.

### Дополнительная информация
Практически в любом языке программирования, если вы напишете нечто подобное, вы получите не то, что вы могли ожидать: 
```java 
System.out.println(0.1 + 0.2); // => 0.30000000000000004
```

Для того чтобы производить расчёты с десятичными дробными числами, существует специальный класс `BigDecimal` (большое десятичное). Большое, потому что у него нет стандартных ограничений как у `double (-1.7E+308 до 1.7E+308)` или `int (-2147483648 до 2147483647)`.
Этот класс может хранить число, состоящее из 2,147,483,647 цифр, ~~Карл~~! А десятичное, потому что каждая цифра числа хранится по отдельности, из-за чего не возникает проблем с потерями при переводе между системами счисления.
В задаче предлагается принять некоторые неточности и использовать уже знакомый нам тип double. Плюсиком будет решение задачи `№ 2*` (со звёздочкой).

### Функционал программы

→ Создание нескольких счетов и расчет налогов для них.

### Процесс реализации
1. Класс `Bill`

В системе уже есть класс `Bill`, в который мы добавили поле `TaxType taxType;` и метод `payTaxes()`:

```java
class Bill {
    private double amount;
    private TaxType taxType;
    private TaxService taxService;
    
    public Bill(double amount, TaxType taxType, TaxService taxService) {
        this.amount = amount;
        this.taxType = taxType;
        this.taxService = taxService;
    }
    
    public void payTaxes() {
        // TODO вместо 0.0 посчитать размер налога исходя из TaxType
        double taxAmount = 0.0;
        
        taxService.payOut(taxAmount);
    }
}
```

А также класс налоговой службы:
```java
class TaxService {
    public void payOut(double taxAmount) {
        System.out.format("Уплачен налог в размере %.2f%n", taxAmount);
    }
}
```

2. Создадим классы для различных типов налогообложения.
* Базовый класс
```java 
class TaxType {
    public double calculateTaxFor(double amount) {
        // TODO override me!
        return 0.0;
    }
}
```
* Классы, расширяющие `TaxType`:
     * Подоходный налог, = 13% (`IncomeTaxType`)
     * НДС, = 18% (`VATaxType`)
     * Прогрессивный налог, до 100 тысяч = 10%, больше 100 тысяч = 15% (`ProgressiveTaxType`)

3. В методе `main` создадим несколько счетов и оплатим с них налоги в налоговую службу.

```java
public static void main(String[] args) {
    TaxService taxService = new TaxService();
    Bill[] payments = new Bill[] {
        // TODO создать платежи с различным типами налогообложения
    };
    for (int i = 0; i < payments.length; ++i) {
        Bill bill = payments[i];
        bill.payTaxes();
    }
}
```

## Задача 2* 

Здесь необходимо реализовать тот же функционал, но используя вместо double –> BigDecimal.

Работа с ним может показаться необычной и странной. Например, их нельзя сложить, используя оператор `+` (в Java запрещено перегружать операторы), или сравнить с помощью `==` (так как это объект, произойдёт сравнение ссылок, а не значений объектов). Вместо этого мы должны использовать методы `.add(…)` и `.compareTo(…)` соответственно.

Экземпляр этого класса можно создать с помощью `new BigDecimal("0.1")` или `BigDecimal.valueOf(0.2)`.
Как и `String`, экземпляры этого класса неизменяемые (иммутабельные), а методы `.add(…)`, `.multiply(…)` возвращают новый объект, содержащий результат операции.

Например, чтобы сложить 0.1 рубля и 0.2 рубля и получить ожидаемые 30 копеек, мы могли бы написать код:

```java
BigDecimal first = new BigDecimal("0.10");
BigDecimal second = BigDecimal.valueOf(0.2);

BigDecimal sum = first.add(second);

System.out.println(sum.toString()); // => 0.30
```

ОТВЕТ:
```java 

  import java.math.BigDecimal;
  public class Main {

    public static void main(String[] args) {
        TaxService taxService = new TaxService();
        Bill[] payments = new Bill[] {
                new Bill(new BigDecimal ("1000"), new IncomeTaxType(), taxService),
                new Bill(new BigDecimal ("1500"), new VATaxType(), taxService),
                new Bill(new BigDecimal ("15000"), new ProgressiveTaxType(), taxService),
        };
        for (int i = 0; i < payments.length; ++i) {
            Bill bill = payments[i];
            bill.payTaxes();
        }
    }
}

import java.math.BigDecimal;

public class Bill {
    private BigDecimal amount;
    private TaxType taxType;
    private TaxService taxService;

    public Bill(BigDecimal amount, TaxType taxType, TaxService taxService) {
        this.amount = amount;
        this.taxType = taxType;
        this.taxService = taxService;
    }

    public void payTaxes() {
        // TODO вместо 0.0 посчитать размер налога исходя из TaxType
        BigDecimal taxAmount = taxType.calculateTaxFor(amount);

        taxService.payOut(taxAmount);
    }
}

import java.math.BigDecimal;

public class TaxService {
    public void payOut(BigDecimal taxAmount) {
        System.out.format("Уплачен налог в размере %.2f%n", taxAmount);
    }
}

import java.math.BigDecimal;

public class TaxType {
    public BigDecimal calculateTaxFor(BigDecimal amount) {
        // TODO override me!
        return BigDecimal.ZERO;
    }
}

import java.math.BigDecimal;

public class IncomeTaxType extends TaxType{
    @Override
    public BigDecimal calculateTaxFor(BigDecimal amount) {
        return amount.multiply(BigDecimal.valueOf(0.13));
    }
}

import java.math.BigDecimal;

public class VATaxType extends TaxType {
    @Override
    public BigDecimal calculateTaxFor(BigDecimal amount) {
       return amount.multiply(BigDecimal.valueOf(0.18));
    }
}
 
import java.math.BigDecimal;

public class ProgressiveTaxType extends TaxType {
    @Override
    public BigDecimal calculateTaxFor(BigDecimal amount) {
        if (amount.compareTo(BigDecimal.valueOf(0)) <= 100000) {
            return amount.multiply(BigDecimal.valueOf(0.1));
        } else {
            return amount.multiply(BigDecimal.valueOf(0.15));
        }
    }
}

