# Занятие 10. Типы данных

```java
/*
Недавно столкнулся с такой проблемой, что при отправке сообщений в корпоративеую шину, на той стороне кириллица приходит в некорректном виде. 
Проблема оказалась в том, что при отправке в заголовке сообщения явно не указывается кодировка. 
Решение проблемы было следующим: при формировании запроса явно указывать соответствующий заголовок с указанием кодировки (UTF-8).
*/
```

```
// Использование логических констант вместо сложных условий
conditionsProperties.getRules().stream()
    .anyMatch(rule -> {
        String strategyRule = rule.get("strategy");
        String categoryRule = rule.get("category");
        BigDecimal limitSumRule = new BigDecimal(rule.get("limitSum"));
        
        boolean isStrategyMatch = strategyRule.equals(...);
        boolean isCategoryMatch = Category.PREMIUM.equals(categoryRule) && ... ;
        boolean isLimitSumMatch = balance.compareTo(limitSumRule) >= 0;
        
        return isStrategyMatch && isCategoryMatch && isLimitSumMatch;
    });
```

```
// Заменил тип вещественных переменных, которые используются для вычислений (градиентный спуск), с Double на BigDecimal для более точных результатов
```

```
// При сравнении вещественных чисел с большим кол-ом знаков после запятой использовал заданную точность
double epsilon = 1e-12;
if (Math.abs(iterSolve - analSolve) < epsilon) {
    ...
}
```

```
// При делении вещественных чисел типа BigDecimal задал тип и точность округления результата
BigDecimal numerator = ... ;
BigDecimal denominator = ... ;
BigDecimal annuityPayment = numerator.divide(denominator, 10, HALF_UP);
```

```
// Добавил предупреждение ошибки деления на 0
double percentageOfPens = ...
if (!customers.isEmpty) {
    percentageOfPens = numOfPens / customers.size() * 100;
}
```

```
// Использование константы вместо магического числа
HTTP_STATUS_OK = 200;
if status == HTTP_STATUS_OK {
    ...
}
```

```
// Заменил тип вещественной переменной с float на double
```

```
// Заменил тип у переменных width и height, которые используются для рисования кнопки на экране, с вещественного на целочисленный, так как кол-во пикселей это целое число
int width;
int height;
```

```
// Убрал ненужное приведение типов (вследствие изменения типов в предыдущем пункте)
// Было:
int area = (int) (button.getWidth() * button.getHeight());
// Стало:
int area = button.getWidth() * button.getHeight();
```

```
// Привёл все текстовые сообщения к одному виду (русификация), так как это был учебный проект и интернационализация не требуется
```

```
// Использование логических констант вместо сложных условий
isServerError = Status.ERROR.valueOf().equals(status) && errors.contains(errorCode);
isTimeout = Status.TIMEOUT.valueOf().equals(status);
if (isServerError || isTimeout) {
    ...
}
```