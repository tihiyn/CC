# Занятие 8. ООП и интерфейсы

## 3.1
```java
public class Line {
    private double k;
    private double b;
    
    private Line(double x1, double y1, double x2, double y2) {
        this.k = calculateSlopeCoeff(x1, y1, x2, y2);
        this.b = calculateFreeTerm(x1, y1, x2, y2);
    }
    
    private Line(double k, double b) {
        this.k = k;
        this.b = b;
    }
    
    public static Line ForTwoPoints(double x1, double y1, double x2, double y2) {
        return new Line(x1, y1, x2, y2);
    }
    
    public static Line ForSlopeCoeff(double k, double b) {
        return new Line(k, b);
    } 
}
```

```java
public class Rectangle {
    private double width;
    private double height;
    
    private Rectangle(double side) {
        this.width = side;
        this.height = side;
    }
    
    private Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    public static Rectangle ForSquare(double side) {
        return new Rectangle(side); 
    }
    
    public static Rectangle ForRectangle(double width, double height) {
        return new Rectangle(width, height);
    }
}
```

```java
public class BankAccount {
    private String number;
    private String holder;
    private double balance;
    
    private BankAccount() {
        this.number = "0000000000";
        this.holder = "None";
        this.balance = 0.0;
    }
    
    private BankAccount(String number, String holder) {
        this.number = number;
        this.holder = holder;
        this.balance = 0.0;
    }
    
    private BankAccount(String number, String holder, double balance) {
        this.number = number;
        this.holder = holder;
        this.balance = balance;
    }
    
    public static ForEmptyData() {
        return new BankAccount();
    }
    
    public static ForWithoutBallance(String number, String holder) {
        return new BankAccount(number, holder);
    }

    public static ForFullData(String number, String holder, double balance) {
        return new BankAccount(number, holder, balance);
    }
}
```

## 3.2

```
UserRepository - UserRepositoryImpl
```

```
ResponseMapper - ResponseMapperImpl
```

```
Strategy - MainStrategy, AlternativeStrategy
```