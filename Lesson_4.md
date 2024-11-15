# Занятие 3. Имена переменных

## 7.1
### Было:
```
boolean flag = remove(_value);
while (flag) {
    flag = remove(_value);
}
```

### Стало:
```
boolean isRemoved = remove(_value);
while (isRemoved) {
    isRemoved = remove(_value);
}
```

### Было:
```
flag = validateInputData(data);
if (flag) {
    ...
}
```

### Стало:
```
isInputDataCorrect = validateInputData(data);
if (isInputDataCorrect) {
    ...
}
```

### Было:
```
sub = checkSubscribtion(user);
if (sub) {
    ...
}
```

### Стало:
```
isSubscriber = checkSubscribtion(user);
if (isSubscriber) {
    ...
}
```

### Было:
```
flagIsActve
```

### Стало:
```
isActive
```

### Было:
```
boolean bool = checkAge(...);
```

### Стало:
```
boolean isPensioner = checkAge(...);
```

## 7.2
```
boolean found = false;
for(Movie movie: movies) {
    found = movieRepository.existsByIdAndAndAvgRatingGreaterThanEqual(movie.Id, 9);
}

if (found) {
    ...
}
```

```
boolean error = false;
try {
    error = calculatePdn(...);
}
catch (RuntimeException e) {
    log.warn(...);
}

if (error) {
    ...
}
```

## 7.3
```
for (Node node = list.head; node != null; node = node.next) {
    ...
}
// итерация по элементам связного списка
```

## 7.4
```
moveFromHeadNode, moveFromTailNode
// переменные в функции для инвертирования порядка элементов в двусвязном списке
```

```
LocalTime beginOfOperation = LocalTime.now()

...

LocalTime endOfOperation = LocalTime.now()
LocalTime operationTime = endOfOperation - beginOfOperation;

return operationTime;
```

## 7.5
### Было:
```
if (minStack.peek() != null && val.intValue() > minStack.peek().intValue()) {
    T tmp = minStack.pop();
    minStack.pop();
    minStack.push(tmp);
} else {
    minStack.pop();
}
```
### Стало:
```
if (minStack.peek() != null && val.intValue() > minStack.peek().intValue()) {
    T topElement = minStack.pop();
    minStack.pop();
    minStack.push(topElement);
} else {
    minStack.pop();
}
```

### Было:
```
public void spin(Queue<T> queue, int N) {
    for (int i = 0; i < N; i++) {
        T e = queue.dequeue();
        queue.enqueue(e);
    }
}
```

### Стало:
```
public void spin(Queue<T> queue, int N) {
    for (int i = 0; i < N; i++) {
        queue.enqueue(queue.dequeue());
    }
}
```