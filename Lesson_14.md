# Занятие 14. Массивы

## 1. Использование Stream API для более безопасной и удобной работы с массивами
### Было:
```
int[] ages = new int[ageArraySize];

for (int i = 0; i < ages.length; i++) {
    ages[i] = scanner.nextInt();
}

int sum = 0;
for (int i = 0; i < ages.length; i++) {
    sum += ages[i];
}

double averageAge = sum / size;
```

### Стало:
```
int[] ages = IntStream.range(0, ageArraySize)
        .map(age -> scanner.nextInt())
        .toArray();

double averageAge = Arrays.stream(ages).average().orElse(0.0);
```

## 2. Использование цикла foreach вместо классической версии цикла for для более безопасной работы с индексами массива
### Было:

```
for (int i = 0; i < raceTimeInHoursArray.length; i++) {
    ...
}
```

### Стало:
```
for (int i: raceTimeInHoursArray) {
    ...
}
```

## 3. Использование словаря вместо 2 логически связанных массивов
### Было:
```
String[] products = ... ;
double[] prices = ... ;
```

### Стало: 
```
Map<String, Double> shoppingCart = ... ;
```

## 4. Использование множества вместо массива для более эффективной и удобной проверки на принадлежность элемента множеству
### Было:
```
char[] validHexSymbols = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F'};
```

### Стало:
```
Set<Characters> validHexSymbols = Set.of('0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'A', 'B', 'C', 'D', 'E', 'F');
```

## 5. Использование стека вместо массива для организации отмены последовательных операций
### Было:
```
String[] operations = new String[100];

int oparetionIndex = -1;
while (true) {
    ... 
    
    if (!UNDO.equals(operation)) {
        operations[++oparetionIndex] = operation;
    } else {
        operations[oparetionIndex--];
    }
    
    ...
}
```

### Стало:
```
Stack<String> operations = new Stack<>();

while (true) {
    ... 
    
    if (!UNDO.equals(operation)) {
        operations.push(operation);
    } else {
        operations.pop();
    }
    
    ...
}
```