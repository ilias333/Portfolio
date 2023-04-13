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
```
## Задача 3. Иерархия "Автомобили"

### Описание
На этот раз рассмотрим предметную область "Автомобили".
Необходимо спроектировать приложение по размещению объявлений о продаже автомобилей. 

Реализуйте иерархию классов автомобилей с помощью наследования. В ней должны быть представлены три группы классов: 

→ по назначению (грузовые/легковые/пассажирские);

→ по типу кузова (седан/универсал/грузовик/автобус);

→ по типу топлива (бензиновые, дизельные, гибридные и электрические).

Необходимо с помощью наследования реализовать программу, в которой будет один базовый класс `VehicleType`, три наследника базового класса  
(`VehicleTypeByPurpose`, `VehicleTypeByBodyTypes`, `VehicleTypeByFuelTypes`), в которых будeт определено значение поля `attribute` каждой группы типов, 
и на каждый класс групп типов — по несколько классов, в которых будет определено конкретное определение типа.

Данный функционал пригодится в случае массовой фильтрации объявлений по какому-то искомому типу.

Также необходимо описать класс `VehicleAd`:
* с базовым набором полей, состоящим из id объявления, model (модели авто) и трех полей с каждым типом, 
* и `AdsService`, в котором будет осуществляться фильтрация объявлений.

### Процесс реализации
1. Создайте Enum класс `VehicleTypeEnum` с 8 возможными типами авто в нашей программе.
```
public enum VehicleTypeEnum {
    TRUCK,CAR,PASSENGER,SEDAN,WAGON,PICKUP,BUS,PETROL,DIESEL,HYBRID,ELECTRIC
}
```
2. Создайте класс `VehicleType` с protected полем `attribute` типа String, конструктором, принимающим один аргумент, и двумя методами.
```
    public String getAttributeOfType() {
          return attribute;
    }
    public String getTypeName() {
          return "Some vehicle type name";
    }
```
3. Создайте трёх наследников класса `VehicleType`. 
Например: `VehicleTypeByPurpose`, класс с конструктором, вызывающий конструктор предка. Обязательно переопределите метод `equals` класса `Object`.

```
public class VehicleTypeByPurpose extends VehicleType {
    public VehicleTypeByPurpose() {
            super("Vehicle type by purpose");
    }
    
    @Override
    public boolean equals(Object object) {
        if (object == null || getClass() != object.getClass()) return false;
    
         VehicleTypeByPurpose that = (VehicleTypeByPurpose) object;
         return attribute != null ? attribute.equals(that.attribute) : false;
     }
}
```
Сделайте самостоятельно остальные два класса: `VehicleTypeByBodyTypes` и `VehicleTypeByFuelTypes` с собственным значением поля `attribute`, присущим данной группе типов автомобилей.
4. Создайте наследников каждого из классов групп типов. В них необходимо переопределить только метод `getTypeName`.

Например:
```
public class PassengerType extends VehicleTypeByPurpose {

    @Override
    public String getTypeName() {
        return VehicleTypeEnum.PASSENGER.name();
    }
}
```
и 
```
public class TruckType extends VehicleTypeByPurpose {

    @Override
    public String getTypeName() {
        return VehicleTypeEnum.TRUCK.name();
    }
}
```

Создайте остальные имплементации.

5. Добавьте в класс `VehicleAd` пять полей.

```
public class VehicleAd {
    private String model;
    private String id;
    private VehicleTypeByPurpose vehicleTypeByPurpose;
    private VehicleTypeByBodyTypes vehicleTypeByBodyTypes;
    private VehicleTypeByFuelTypes vehicleTypeByFuelTypes;

    public VehicleAd(String model, String id, VehicleTypeByPurpose vehicleTypeByPurpose, 
    VehicleTypeByBodyTypes vehicleTypeByBodyTypes, VehicleTypeByFuelTypes vehicleTypeByFuelTypes) {
        this.model = model;
        this.id = id;
        this.vehicleTypeByPurpose = vehicleTypeByPurpose;
        this.vehicleTypeByBodyTypes = vehicleTypeByBodyTypes;
        this.vehicleTypeByFuelTypes = vehicleTypeByFuelTypes;
    }

    //for creating new Car
    public VehicleAd(String model) {
        this.model = model;
    }

    public VehicleTypeByPurpose getVehicleTypeByPurpose() {
            return vehicleTypeByPurpose;
    }
    
    public VehicleTypeByBodyTypes getVehicleTypeByBodyTypes() {
                return vehicleTypeByBodyTypes;
    }
    
    public VehicleTypeByFuelTypes getVehicleTypeByFuelTypes() {
                    return vehicleTypeByFuelTypes;
    }

    public String getModel() {
        return model;
    }

    public String getId() {
        return id;
    }

    public String toString(){
        return this.model;
    }
}
```

6. Создайте сервис `AdsService`, в котором можно будет отфильтровать объявления по каждому типу. 

```
public class AdsService {
    private VehicleAd[] adList;

    public void setAdList(VehicleAd[] adList) {
        this.adList = adList;
    }

    public void filterByVehicleTypeByPurpose(VehicleTypeByPurpose vehicleType) {
         for (VehicleAd ad : adList) {
            if (ad.getVehicleTypeByPurpose().equals(vehicleType)) {
              System.out.println("Объявление № " + ad.getId() + " подходит под данный фильтр с атрибутом: тип авто - " 
              + vehicleType.getTypeName() + ", атрибут фильтра " + vehicleType.getAttributeOfType());
            } else {
            System.out.println("Объявление № " + ad.getId() + " не прошло фильтр: тип авто - " + vehicleType.getTypeName() + ", критерий- " + 
                                            vehicleType.getAttributeOfType() + ", так как имеет тип авто " +
                                            ad.getVehicleTypeByPurpose().getTypeName());
            }
        }
  }
  
  public void filterByVehicleTypeByBodyTypes(VehicleTypeByBodyTypes vehicleType) {
           //TODO 
    }
    
  public void filterByVehicleTypeByFuelTypes(VehicleTypeByFuelTypes vehicleType) {
           //TODO 
    }
  
}
```

7. В классе Main.java создайте несколько объектов класса `VehicleAd`, используя конструктор, и убедитесь, 
что функция фильтрации была реализована верно. 
Например:

```
    AdsService adsService = new AdsService();
    VehicleAd volvoAd = new VehicleAd("Volvo", "123", new PassengerType(), 
       new SedanType(), new PetrolType());
    VehicleAd kamazAd = new VehicleAd("Kamaz", "45", new TruckType(), 
          new PickupType(), new DieselType());
    
    adsService.setAdList(new VehicleAd[] {volvoAd, kamazAd});
   
    adsService.filterByVehicleTypeByPurpose(new PassengerType());
   
    adsService.filterByVehicleTypeByPurpose(new TruckType());
    
    //TODO Создайте объявление с типами CAR, SEDAN, PETROL и отфильтруйте объявления с бензиновым топливом
                  
```

ОТВЕТ:

```java 
public class Main {

    public static void main(String[] args) {
        AdsService adsService = new AdsService();
        VehicleAd volvoAd = new VehicleAd("Volvo", "123", new PassengerType(),
                new SedanType(), new PetrolType());
        VehicleAd kamazAd = new VehicleAd("Kamaz", "45", new TruckType(),
                new PickupType(), new DieselType());

        adsService.setAdList(new VehicleAd[] {volvoAd, kamazAd});

        adsService.filterByVehicleTypeByPurpose(new PassengerType());

        adsService.filterByVehicleTypeByPurpose(new TruckType());
    }
}

public enum VehicleTypeEnum {
    TRUCK,CAR,PASSENGER,SEDAN,WAGON,PICKUP,BUS,PETROL,DIESEL,HYBRID,ELECTRIC
}

public class AdsService {
    private VehicleAd[] adList;

    public void setAdList(VehicleAd[] adList) {
        this.adList = adList;
    }

    public void filterByVehicleTypeByPurpose(VehicleTypeByPurpose vehicleType) {
        for (VehicleAd ad : adList) {
            if (ad.getVehicleTypeByPurpose().equals(vehicleType)) {
                System.out.println("Объявление № " + ad.getId() + " подходит под данный фильтр с атрибутом: тип авто - "
                        + vehicleType.getTypeName() + ", атрибут фильтра " + vehicleType.getAttributeOfType());
            } else {
                System.out.println("Объявление № " + ad.getId() + " не прошло фильтр: тип авто - " + vehicleType.getTypeName() + ", критерий- " +
                        vehicleType.getAttributeOfType() + ", так как имеет тип авто " +
                        ad.getVehicleTypeByPurpose().getTypeName());
            }
        }
    }

    public void filterByVehicleTypeByBodyTypes(VehicleTypeByBodyTypes vehicleType) {
        for (VehicleAd ad : adList) {
            if (ad.getVehicleTypeByBodyTypes().equals(vehicleType)) {
                System.out.println("Объявление № " + ad.getId() + " подходит под данный фильтр с атрибутом: тип авто - "
                        + vehicleType.getTypeName() + ", атрибут фильтра " + vehicleType.getAttributeOfType());
            } else {
                System.out.println("Объявление № " + ad.getId() + " не прошло фильтр: тип авто - " + vehicleType.getTypeName() + ", критерий- " +
                        vehicleType.getAttributeOfType() + ", так как имеет тип авто " +
                        ad.getVehicleTypeByBodyTypes().getTypeName());
            }
        }
    }

    public void filterByVehicleTypeByFuelTypes(VehicleTypeByFuelTypes vehicleType) {
        for (VehicleAd ad : adList) {
            if (ad.getVehicleTypeByFuelTypes().equals(vehicleType)) {
                System.out.println("Объявление № " + ad.getId() + " подходит под данный фильтр с атрибутом: тип авто - "
                        + vehicleType.getTypeName() + ", атрибут фильтра " + vehicleType.getAttributeOfType());
            } else {
                System.out.println("Объявление № " + ad.getId() + " не прошло фильтр: тип авто - " + vehicleType.getTypeName() + ", критерий- " +
                        vehicleType.getAttributeOfType() + ", так как имеет тип авто " +
                        ad.getVehicleTypeByFuelTypes().getTypeName());
            }
        }
    }
}

public class VehicleType {
    protected String attribute;

    public VehicleType(String attribute) {
        this.attribute = attribute;
    }

    public String getAttributeOfType() {
        return attribute;
    }
    public String getTypeName() {
        return "Some vehicle type name";
    }
}


public class VehicleTypeByFuelTypes extends VehicleType {
    public VehicleTypeByFuelTypes() {
        super("Vehicle type by fuel");
    }
    @Override
    public boolean equals(Object object) {
        if (object == null || getClass() != object.getClass()) return false;

        VehicleTypeByFuelTypes that = (VehicleTypeByFuelTypes) object;
        return attribute != null ? attribute.equals(that.attribute) : false;
    }
}

public class VehicleTypeByBodyTypes extends VehicleType {
    public VehicleTypeByBodyTypes() {
        super("Vehicle type by body");
    }

    @Override
    public boolean equals(Object object) {
        if (object == null || getClass() != object.getClass()) return false;

        VehicleTypeByBodyTypes that = (VehicleTypeByBodyTypes) object;
        return attribute != null ? attribute.equals(that.attribute) : false;
    }
}

public class VehicleTypeByPurpose extends VehicleType {
    public VehicleTypeByPurpose() {
        super("Vehicle type by purpose");
    }

    @Override
    public boolean equals(Object object) {
        if (object == null || getClass() != object.getClass()) return false;

        VehicleTypeByPurpose that = (VehicleTypeByPurpose) object;
        return attribute != null ? attribute.equals(that.attribute) : false;
    }
}
public class VehicleAd {
    private String model;
    private String id;
    private VehicleTypeByPurpose vehicleTypeByPurpose;
    private VehicleTypeByBodyTypes vehicleTypeByBodyTypes;
    private VehicleTypeByFuelTypes vehicleTypeByFuelTypes;

    public VehicleAd(String model, String id, VehicleTypeByPurpose vehicleTypeByPurpose,
                     VehicleTypeByBodyTypes vehicleTypeByBodyTypes, VehicleTypeByFuelTypes vehicleTypeByFuelTypes) {
        this.model = model;
        this.id = id;
        this.vehicleTypeByPurpose = vehicleTypeByPurpose;
        this.vehicleTypeByBodyTypes = vehicleTypeByBodyTypes;
        this.vehicleTypeByFuelTypes = vehicleTypeByFuelTypes;
    }

    //for creating new Car
    public VehicleAd(String model) {
        this.model = model;
    }

    public VehicleTypeByPurpose getVehicleTypeByPurpose() {
        return vehicleTypeByPurpose;
    }

    public VehicleTypeByBodyTypes getVehicleTypeByBodyTypes() {
        return vehicleTypeByBodyTypes;
    }

    public VehicleTypeByFuelTypes getVehicleTypeByFuelTypes() {
        return vehicleTypeByFuelTypes;
    }

    public String getModel() {
        return model;
    }

    public String getId() {
        return id;
    }

    public String toString(){
        return this.model;
    }
}

public class VehicleAd {
    private String model;
    private String id;
    private VehicleTypeByPurpose vehicleTypeByPurpose;
    private VehicleTypeByBodyTypes vehicleTypeByBodyTypes;
    private VehicleTypeByFuelTypes vehicleTypeByFuelTypes;

    public VehicleAd(String model, String id, VehicleTypeByPurpose vehicleTypeByPurpose,
                     VehicleTypeByBodyTypes vehicleTypeByBodyTypes, VehicleTypeByFuelTypes vehicleTypeByFuelTypes) {
        this.model = model;
        this.id = id;
        this.vehicleTypeByPurpose = vehicleTypeByPurpose;
        this.vehicleTypeByBodyTypes = vehicleTypeByBodyTypes;
        this.vehicleTypeByFuelTypes = vehicleTypeByFuelTypes;
    }

    //for creating new Car
    public VehicleAd(String model) {
        this.model = model;
    }

    public VehicleTypeByPurpose getVehicleTypeByPurpose() {
        return vehicleTypeByPurpose;
    }

    public VehicleTypeByBodyTypes getVehicleTypeByBodyTypes() {
        return vehicleTypeByBodyTypes;
    }

    public VehicleTypeByFuelTypes getVehicleTypeByFuelTypes() {
        return vehicleTypeByFuelTypes;
    }

    public String getModel() {
        return model;
    }

    public String getId() {
        return id;
    }

    public String toString(){
        return this.model;
    }
}

public class DieselType extends VehicleTypeByFuelTypes {
    @Override
    public String getTypeName() {
        return VehicleTypeEnum.DIESEL.name();
    }
}

public class PetrolType extends VehicleTypeByFuelTypes {
    @Override
    public String getTypeName() {
        return VehicleTypeEnum.PETROL.name();
    }
}

public class SedanType extends VehicleTypeByBodyTypes {

    @Override
    public String getTypeName() {
        return VehicleTypeEnum.SEDAN.name();
    }
}

public class TruckType extends VehicleTypeByPurpose {

    @Override
    public String getTypeName() {
        return VehicleTypeEnum.TRUCK.name();
    }
}

public class PassengerType extends VehicleTypeByPurpose {

    @Override
    public String getTypeName() {
        return VehicleTypeEnum.PASSENGER.name();
    }
}

public class PickupType extends VehicleTypeByBodyTypes {

    @Override
    public String getTypeName() {
        return VehicleTypeEnum.PICKUP.name();
    }
}


