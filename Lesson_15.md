# Занятие 15. Комментарии

## 3.1.1
```
public class MinDeque<T extends Number> {
    private List<T> storage;
    // вспомогательная deque для хранения элементов таким образом, чтобы на одном из концов deque находился минимальный элемент и доступ к нему осуществлялся за O(1)
    public Deque<T> minDeque;
    
    ...
}
```

## 3.1.2
```
# умножаем количество на 2, т.к. этот товар продается в паре
cost = quantity * 2 * price;
```

## 3.1.3
```
// используем бинарный поиск, так как список отсортирован
Collections.binarySearch(sortedList, targetValue);
```

## 3.1.4
```
// используем старый метод из библиотеки, так как новый API еще не поддерживается
ExternalLibHandler handler = new ExternalLibHandler();
handler.processDataLegacyFormat(data);
```

## 3.1.5
```
public int findGCD(int a, int b) {
    // используем алгоритм Евклида для нахождения НОД (наибольшего общего делителя)
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```

## 3.1.6
```
private static class Slot {
    public String value;
    // испоьзование "соли" (случайного значения) для каждого слота с целью защиты от DDos-атак
    public int salt;
    
    ...
}
```

## 3.1.7
```
// использовать закэшированный репозиторий, так как размер таблицы employee небольшой, а кол-во обращений к таблице велико
List<Employee> employees = employeeCachedRepository.findByRoleAndLevel(role, level);
```

## 3.2.1
### Было:
```
# предобработка данных
...
# удаление пропусков
...
# удаление выбросов
...
# удаление ненужных столбцов
...
```

### Стало:
```
def preprocess_data(df):
    ...
    remove_gaps(df)
    remove_outliers(df);
    remove_unnecessary_cols(df);
    ...
```

## 3.2.2
### Было:
```
// заполнение массива допустимых символов
for (int i = 0; i < 10; i++) {
    C[i] = (char) (i + 48);
}

for (int i = 10; i < 15; i++) {
    C[i] = (char) (i + 55);
}
```

### Стало:
```
private final Set<Characters> validHexSymbols = Set.of('0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F');
```

## 3.2.3
### Было:
```
// проверка возраста на совершеннолетие
if (age >= 18) {
    System.out.println("Adult");
}

```

### Стало:
```
public boolean isAdult(int age) {
    return age >= RUS_MAJORITY_AGE;
}
```

## 3.2.4
### Было:
```
public void readFile(String filePath) {
    File file = new File(filePath);
    // файл должен быть меньше 10 МБ из-за ограничений памяти
    if (file.length() > 10 * 1024 * 1024) {
        throw new ...
    }
    ...
}
```

### Стало:
```
public void readFile(String filePath) {
    File file = new File(filePath);
    
    if (file.length() > MAX_AVAILABLE_FILE_SIZE) {
        throw new ...
    }
    ...
}
```

## 3.2.5
### Было:
```
// проверка на то, что значение isPremium не null и истинно
if (customer.isPremium() != null && customer.isPremium() == true) {
    ...
}
```

### Стало:
```
if (Boolean.TRUE.equals(customer.isPremium())) {
    ...
}
```