# Занятие 11. Переменные и их значения

## 1. Перенёс объявление и инициализацию переменной previous непосредственно перед циклом
### Было:
```
public boolean remove(int _value) {
    ...

    Node current = this.head;
    Node previous = this.head;

    if (current.value == _value) {
        this.head = current.next;
        if (this.head == null) {
            this.tail = null;
        }
        return true;
    }

    while (current.value != _value) {
        if (current.next == null) {
            return false;
        }

        previous = current;
        current = current.next;
    }

    ...
}
```

### Стало:
```
public boolean remove(int _value) {
    ...

    Node current = this.head;
    if (current.value == _value) {
        this.head = current.next;
        if (this.head == null) {
            this.tail = null;
        }
        return true;
    }

    Node previous = this.head;
    while (current.value != _value) {
        if (current.next == null) {
            return false;
        }

        previous = current;
        current = current.next;
    }

    ...
}
```

## 2. Объявил переменную isPremium, которая используется как хранилище для часто используемого значения, как константу
### Было:
```
boolean isPremium = req.isPremium() != null && req.isPremium();
```

### Стало:
```
final boolean isPremium = req.isPremium() != null && req.isPremium();
```

## 3. Перенёс объявление и инициализацию переменной reqSum непосредственно к месту её первого использования
### Было:
```
BigDecimal reqSum = req.getSum() == null ? ... : req.getSum(); 

... // много кода

BigDecimal discount = calculateDiscount(reqSum, ...);
```

### Стало:
```
... // много кода

BigDecimal reqSum = req.getSum() == null ? ... : req.getSum(); 
BigDecimal discount = calculateDiscount(reqSum, ...);
```

## 4. Добавил проверку допустимых значений для переменной, отвечающей за срок кредита
```
assert termInMonths >= 12 && termInMonths <= 60 : "Некорректный срок кредита: допустимый диапазон от 12 до 60 месяцев";
```

## 5. Перенёс объявление и инициализацию переменных непосредственно перед циклом
### Было:
```
int stepUpCounter = 0;
int stepDownCounter = 0;
int stepRightCounter = 0;
int stepLeftCounter = 0;

... // много кода

for (Movement movement: movements) {
... // код, использующий эти переменные
}
```

### Стало:
```
... // много кода

int stepUpCounter = 0;
int stepDownCounter = 0;
int stepRightCounter = 0;
int stepLeftCounter = 0;

for (Movement movement: movements) {
... // код, использующий эти переменные
}
```

## 6. Добавил проверку допустимых значений для переменной, отвечающей за процентную ставку по кредиту
```
assert rate.compareTo(BigDecimal.ZERO) > 0 : "Некорректный ставка по кредиту: ставка должна быть положительной";
```

## 7. Перенёс объявление переменной-счётчика непосредственно в заголовок цикла

### Было:
```
int i;
for (i = 0; i < byteArray.length; i++) {
    ...
}
```

### Стало:
```
for (int i = 0; i < byteArray.length; i++) {
    ...
}
```

## 8. Присвоил переменной недопустимое значение после завершения работы с ней
```
String FIOWithoutSpaces = customer.getLastName() + customer.getName() + customer.getPatronymic();
customerDto.setFIO(FIOWithoutSpaces);
FIOWithoutSpaces = "***ERROR***";

... // много кода
```
 
## 9. Добавил проверку допустимых значений для переменных, отвечающих за сумму заказа со скидкой и без скидки
```
BigDecimal sumWithoutDiscount = ... ;
BigDecimal sumWithDiscount = ... ;
assert sumWithoutDiscount.compareTo(sumWithDiscount) >= 0 : "Сумма заказа без скидки меньше суммы заказа со скидкой";
```

## 10. Объявил переменную messageEncoding, которая используется как хранилище для часто используемого значения, как константу
### Было:
```
String messageEncoding = messageConfig.getEncoding();
```

### Стало:
```
final String messageEncoding = messageConfig.getEncoding();
```

## 11. Перенёс объявление и инициализацию переменной temperatureList непосредственно к месту её первого использования

## Было:
```
List<Temperature> temperatureList = weatherApi.call(...);

... // много кода

temperatureList.stream() 
    ... ;
```

## 12. Присвоил переменной недопустимое значение после завершения работы с ней

```
File logData = ... 

... // работа с переменной logData

logData = null // завершение работы с переменной logData

...
```

## 13. Объявил переменную maxSpeed, которая используется как хранилище для часто используемого значения, как константу

### Было:
```
int maxSpeedInKmH = 120;
```

### Стало:
```
final int maxSpeedInKmH = 120;
```

## 14. Добавил проверку допустимых значений для переменных, отвечающих за координаты точки на экране
```
assert coordX > 0 && coordY > 0 : "Координаты должны быть неотрицательными"
```

## 15. Перенёс объявление и инициализацию переменных coordX и coordY непосредственно к месту их первого использования

### Было:
```
int coordX = ... ;
int coordY = ... ;

... // много кода

defineSector(coordX, coordY);
```

### Стало:
```
... // много кода

int coordX = ... ;
int coordY = ... ;
defineSector(coordX, coordY);
```