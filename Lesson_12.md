# Занятие 12. Время жизни переменных

## 1-3. Сократил окна "уязвимости" между переменными coords, graphHopperHit, updatedLocation

### Было:
```
private LocationDto getCoords(LocationDto locationDto) {
    String replacedLocation = locationDto.getAddressName().replaceAll(" ", "%20");
    GraphHopperGeocodingDto coords = graphHopperClient.getGeocoding(replacedLocation, locale, key);

    LocationDto updatedLocation = new LocationDto();
    updatedLocation.setAddressName(locationDto.getAddressName());

    GraphHopperHitsDto graphHopperHit;

    List<GraphHopperHitsDto> firstHit = coords.getHits().stream()
            .filter(hit -> locationDto.getAddressName().equals(
                    "".concat(hit.getCity() != null ? hit.getCity() : "")
                    .concat(" ")
                    .concat(hit.getStreet() != null ? hit.getStreet() : "")
                    .concat(" ")
                    .concat(hit.getHousenumber() != null ? hit.getHousenumber() : "")))
            .toList();

    if (firstHit.isEmpty()) {
        graphHopperHit = coords.getHits().get(0);
    } else if (!coords.getHits().isEmpty()) {
        graphHopperHit = firstHit.get(0);
    } else {
        throw CfoException.builder().status(HttpStatus.BAD_REQUEST).message("Нет ни одного адреса").build();
    }

    updatedLocation.setLatitude(graphHopperHit.getPoint().getLat());
    updatedLocation.setLongitude(graphHopperHit.getPoint().getLng());

    return updatedLocation;
}
```

### Стало:
```
private LocationDto getCoords(LocationDto locationDto) {
    String replacedLocation = locationDto.getAddressName().replaceAll(" ", "%20");
    GraphHopperGeocodingDto coords = graphHopperClient.getGeocoding(replacedLocation, locale, key);

    List<GraphHopperHitsDto> firstHit = coords.getHits().stream()
            .filter(hit -> locationDto.getAddressName().equals(
                    "".concat(hit.getCity() != null ? hit.getCity() : "")
                    .concat(" ")
                    .concat(hit.getStreet() != null ? hit.getStreet() : "")
                    .concat(" ")
                    .concat(hit.getHousenumber() != null ? hit.getHousenumber() : "")))
            .toList();
    
    GraphHopperHitsDto graphHopperHit;
    if (firstHit.isEmpty()) {
        graphHopperHit = coords.getHits().get(0);
    } else if (!coords.getHits().isEmpty()) {
        graphHopperHit = firstHit.get(0);
    } else {
        throw CfoException.builder().status(HttpStatus.BAD_REQUEST).message("Нет ни одного адреса").build();
    }
    
    LocationDto updatedLocation = new LocationDto();
    updatedLocation.setLatitude(graphHopperHit.getPoint().getLat());
    updatedLocation.setLongitude(graphHopperHit.getPoint().getLng());
    updatedLocation.setAddressName(locationDto.getAddressName());

    return updatedLocation;
}
```

## 4-6. Сократил окна "уязвимости" между переменными totalWeight, itemCount, closestWarehouse
### Было:
```
...
for (int i = 0; i < solution.length; i++) {
    if ((int) solution[i] == t) {
        totalWeight += items.get(i).getWeight();
        itemCount++;

        closestWarehouse = items.get(i).getWarehouse();

        if (lastVisited >= 0) {
            routeCost += distanceMatrix[lastVisited][i];
        } else {
            routeCost += timeService.getTime(truck.getWarehouse().getLocation(), items.get(i).getLocation());
        }
        lastVisited = i;

        if (totalWeight > truck.getCapacity() || itemCount > truck.getCapacity()) {
            routeCost += timeService.getTime(items.get(lastVisited).getLocation(), closestWarehouse.getLocation());
            totalWeight = items.get(i).getWeight();
            itemCount = 1;
        }
    }
}
...
```

### Стало:
```
...
for (int i = 0; i < solution.length; i++) {
    if ((int) solution[i] == t) {
        if (lastVisited >= 0) {
            routeCost += distanceMatrix[lastVisited][i];
        } else {
            routeCost += timeService.getTime(truck.getWarehouse().getLocation(), items.get(i).getLocation());
        }
        lastVisited = i;

        totalWeight += items.get(i).getWeight();
        itemCount++;
        closestWarehouse = items.get(i).getWarehouse();
        
        if (totalWeight > truck.getCapacity() || itemCount > truck.getCapacity()) {
            routeCost += timeService.getTime(items.get(lastVisited).getLocation(), closestWarehouse.getLocation());
            totalWeight = items.get(i).getWeight();
            itemCount = 1;
        }
    }
}
...
```

## 7. Вынес группу связанных c переменной updatedLocation (из п.1-3) команд в отдельный метод createUpdatedLocation
```
private LocationDto createUpdatedLocation(Double latitude, Double longitude, String addressName) {
    LocationDto updatedLocation = new LocationDto();
    updatedLocation.setLatitude(latitude);
    updatedLocation.setLongitude(longitude);
    updatedLocation.setAddressName(addressName);

    return updatedLocation;
}
```

## 8-10. Сократил область видимости переменной numOfAnimals, сократил окно уязвимости для переменной file и явно указал область видимости для переменной numOfAnimals с помощью фигурных скобок
### Было:
```
@Logging(value = "Поиск животных с минимальной стоимостью", entering = true, exiting = true)
public List<String> findMinCostAnimals() throws IllegalCollectionSizeException {
    Path path = Paths.get("src/main/resources/results/findMinCostAnimals.json");
    File file = new File(path.toString());

    long numOfAnimals = animalStorage.values().stream()
            .mapToLong(List::size)
            .sum();

    List<String> minCostAnimalsList = animalStorage.entrySet().stream()
            .filter(entry -> entry.getKey() != null)
            .flatMap(entry -> entry.getValue().stream())
            .sorted(Comparator.comparingDouble(animal -> animal.getCost().doubleValue()))
            .map(Animal::getName)
            .limit(3)
            .sorted(Comparator.reverseOrder())
            .toList();

    try {
        objectMapper.writeValue(file, minCostAnimalsList);
    } catch (IOException e) {
        throw new RuntimeException(e);
    }

    if (numOfAnimals >= 3) {
        return minCostAnimalsList;
    } else {
        throw new IllegalCollectionSizeException("Low number of animals; Expected: 3, actual: ", numOfAnimals);
    }
}
```

### Стало:
```
@Logging(value = "Поиск животных с минимальной стоимостью", entering = true, exiting = true)
public List<String> findMinCostAnimals() throws IllegalCollectionSizeException {
    // check numOfAnimals
    {
        long numOfAnimals = animalStorage.values().stream()
                .mapToLong(List::size)
                .sum();
        if (numOfAnimals < 3) {
            throw new IllegalCollectionSizeException("Low number of animals; Expected: 3, actual: ", numOfAnimals);
        }
    }

    List<String> minCostAnimalsList = animalStorage.entrySet().stream()
            .filter(entry -> entry.getKey() != null)
            .flatMap(entry -> entry.getValue().stream())
            .sorted(Comparator.comparingDouble(animal -> animal.getCost().doubleValue()))
            .map(Animal::getName)
            .limit(3)
            .sorted(Comparator.reverseOrder())
            .toList();

    Path path = Paths.get("src/main/resources/results/findMinCostAnimals.json");
    File file = new File(path.toString());
    try {
        objectMapper.writeValue(file, minCostAnimalsList);
    } catch (IOException e) {
        throw new RuntimeException(e);
    }

    return minCostAnimalsList;
}
```

## 11. "Сузил" область видимости метода с package-private до private

### Было:
```
void processTemplates(List<Template> templates) {
    ...
}
```

### Стало:
```
private void processTemplates(List<Template> templates) {
    ...
}
```

## 12. Явно указал область видимости переменной s
```
public boolean isPalindrom(String s) {
    Deque<Character> deque = new Deque<>();

    // fill deque from input string
    {
        s = s.toLowerCase().replaceAll(" ", "");

        for (char b : s.toCharArray()) {
            deque.addTail(b);
        }
    }


    while (deque.size() > 1) {
        if (!deque.removeFront().equals(deque.removeTail())) {
            return false;
        }
    }

    return true;
}
```

## 13. Вынес группу связанных команд в отдельный метод
### Было:
```
public double[] optimize(int scoutBees, int workerBees, int maxIterations,
                         double[][] distanceMatrix, List<AgentDto> agents, List<ItemDto> items) {
    double[][] population = new double[scoutBees][];
    double[] costs = new double[scoutBees];

    for (int i = 0; i < scoutBees; i++) {
        population[i] = randomSolution(agents, items);
        costs[i] = transportationCostService.calculateCost(population[i], distanceMatrix, agents, items);
    }

    for (int iteration = 0; iteration < maxIterations; iteration++) {
        for (int i = 0; i < scoutBees; i++) {
            for (int j = 0; j < workerBees; j++) {
                double[] neighbor = generateNeighborSolution(population[i], agents, items);
                double neighborCost = transportationCostService.calculateCost(neighbor, distanceMatrix, agents, items);

                if (neighborCost < costs[i]) {
                    population[i] = neighbor;
                    costs[i] = neighborCost;
                }
            }
        }
    }

    int bestIndex = 0;
    for (int i = 1; i < scoutBees; i++) {
        if (costs[i] < costs[bestIndex]) {
            bestIndex = i;
        }
    }

    return population[bestIndex];
}
```

### Стало:
```
public double[] optimize(int scoutBees, int workerBees, int maxIterations,
                         double[][] distanceMatrix, List<AgentDto> agents, List<ItemDto> items) {
    double[][] population = new double[scoutBees][];
    double[] costs = new double[scoutBees];

    for (int i = 0; i < scoutBees; i++) {
        population[i] = randomSolution(agents, items);
        costs[i] = transportationCostService.calculateCost(population[i], distanceMatrix, agents, items);
    }

    for (int iteration = 0; iteration < maxIterations; iteration++) {
        for (int i = 0; i < scoutBees; i++) {
            for (int j = 0; j < workerBees; j++) {
                double[] neighbor = generateNeighborSolution(population[i], agents, items);
                double neighborCost = transportationCostService.calculateCost(neighbor, distanceMatrix, agents, items);

                if (neighborCost < costs[i]) {
                    population[i] = neighbor;
                    costs[i] = neighborCost;
                }
            }
        }
    }

    return population[calcBestIndex(costs, scoutBees)];
}

private int calcBestIndex(double[] costs, int scoutBees) {
    int bestIndex = 0;
    for (int i = 1; i < scoutBees; i++) {
        if (costs[i] < costs[bestIndex]) {
            bestIndex = i;
        }
    }

    return bestIndex;
}
```

## 14. Выполнил разбиение групп связанных команд на отдельные функции (assignInitConditions, solveSystem, compareWithAnalSolution) в методе для решения СЛАУ методом простой итерации

## 15. "Сузил" область видимости метода optimizeRout с public до private, т.к. метод используется только внутри класса 