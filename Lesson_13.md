# Занятие 13. Время связывания переменных

## 1. Использование связывания во время выполнения программы.
```
public class DecionService{
    ...
    @Value(${is-alt-strategy-available})
    private final boolean isAltStrategyAvailable;
    ...
    
    public ResulDecision calculateDecision(...) {
        ...
        if (isAltStrategyAvailable) {
            ...
            
            return ... ;
        }
        
        
    }
}

/*
    Для переменной isAltStrategyAvailable значение берётся из внешнего конфигурационного файла (в consul), 
    чтобы это значение можно было в любой момнет изменить в зависимости от бизнес-требований, не внося изменения в сам код, 
    чтобы не проходить целый "цикл" выполнения задачи на внесение изменений.
*/
```

## 2. Использование связывания во время компиляции программы.
```
...
import static ru. ... .Constatns.MIN_SUM_LOAN;


public class ... { 
    ...
    
    private boolean validateRequest(...) {
        ...
        if (MIN_SUM_LOAN.compareTo(req.getReqSum()) > 0) {
            ...
        }
        ...
    }
    
    ...
}

/*
   Переменная MIN_SUM_LOAN является константой, так как значение изменяется достаточно редко 
   (поэтому нет смысла выносить в конфигурационный файл). Но оно может изменится, поэтому, чтобы потом не заниматься 
   поиском по проекту и не вносить несколько изменений, значение вынесено в константу.
*/
```

## 3. Использование связывания во время выполнения программы
```
@Component
@RequiredArgsConstructor
public class DiscountMatrixCachedRepository {
    private final DiscountMatrixRepository discountMatrixRepository;
    private List<DiscountMatrix> cachedDiscountMatrixTable;
    
    @PostConstruct
    public void cacheDiscountMatrixTable() {
        cachedDiscountMatrixTable = discountMatrixRepository.findAll();
    }
    
    // ...
} 

/*
    В данном примере выполняется кэширование таблицы из БД. Это обусловлено тем, что в пронрамме много раз выполняется 
    обращение к данным из этой таблицы. Это можно сдлеать, так как размер таблицы порядка 100 строк, 
    поэтому при выгрузке всей таблицы на этапе запуска программы (запись в поле cachedDiscountMatrixTable) переполнения памяти не возникнет.
*/
```