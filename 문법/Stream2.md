# Stream 2

### Calculation

스트림은 최소, 최대, 합, 평균 등으로 결과를 만들어 낼 수 있다.

```java
long count = IntStream.of(1, 3, 5, 7, 9).count();
long sum = LongStream.of(1, 3, 5, 7, 9).sum();
```

스트림이 비어있는 경우 count와 sum은 0을 출력하면 된다.

하지만 평균, 최소, 최대의 경우는 null이 들어있어 표현을 할 수 없기 때문에 Optional을 사용하여 리턴한다.

```java
OptionalInt min = IntStream.of(1, 3, 5, 7, 9).min();
OptionalInt max = IntStream.of(1, 3, 5, 7, 9).max();
```

스트림에서 ifPresent 메소드를 사용해 Optional 처리를 할 수 있다.

```java
DoubleStream.of(1.1, 2.2, 3.3, 4.4, 5.5)
  .average()
  .ifPresent(System.out::println);
```

### Reduction

스트림은 reduce라는 메서드를 이용해 결과를 만들어낸다.

reduce 메서드는 총 세 가지의 파라미터를 가질 수 있다.

accumulator : 계산 로직, identity : 초기값

combiner : 병렬 스트림에서 나눠 계산한 값을 합치는 로직

1. 인자 하나

    ```java
    Optional<T> reduce(BinaryOperator<T> accumulator);
    ```

    예제에서 두 값을 더하는 람다를 넘겨주고 있다.

    결과는 6(<i>1 + 2 + 3</i>)이 된다.

    ```java
    OptionalInt reduced = IntStream.range(1, 4) // [1, 2, 3]
        .reduce((a, b) -> {
            return Integer.sum(a, b);
        });
    ```
2. 인자 두개

    ```java
    T reduce(T identity, BinaryOperator<T> accumulator);
    ```

    결과는 16(<i>10(초기값) + 1 + 2 + 3</i>)이 된다.

    ```java
    int reducedTwoParams = 
        IntStream.range(1, 4) // [1, 2, 3]
        .reduce(10, Integer::sum);
    ```
    
3. 인자 세개

    ```java
    <U> U reduce(U identity,
        BiFunction<U, ? super T, U> accumulator,
        BinaryOperator<U> combiner);
    ```

    combiner은 병렬 스트림에서만 작동한다.

    ```java
    Integer reducedParams = Stream.of(1, 2, 3)
        .parallelStream() // 병렬스트림
        .reduce(10,
            Integer::sum,
            (a, b) -> {
                System.out.println("combiner was called");
                return a + b;
            });
    ```
    초기값 10에 각 스트림 값을 더한 세개의 값 (<i>10 + 1, 10 + 2, 10 + 3</i>)을 계산한다.

    Combiner은 12 + 13 = 25, 25 + 11 = 36 이렇게 계산이 된다.

    병렬 스트림이 무조건 시퀸셜보다 좋은게 아니다. 간단한 경우에는 부가적인 처리가 필요해 오히려 느릴 수도 있다.

### Collecting

collect 메서드는 Collection 타입의 파라미터를 받아 처리를 한다.


예제
```java
List<Product> productList = 
  Arrays.asList(new Product(23, "potatoes"),
                new Product(14, "orange"),
                new Product(13, "lemon"),
                new Product(23, "bread"),
                new Product(13, "sugar"));
```

#### Collectors.toList()

스트림 작업 결과를 리스트로 변환한다.

```java
List<String> collectorCollection =
  productList.stream()
    .map(Product::getName)
    .collect(Collectors.toList());
// [potatoes, orange, lemon, bread, sugar]
```

#### Collectors.joining()

스트림 작업 결과를 하나의 문자열로 붙일 수 있다.

```java
List<String> collectorCollection =
  productList.stream()
    .map(Product::getName)
    .collect(Collectors.joining());
// potatoesorangelemonbreadsugar
```

Collectors.joining()은 세 개의 인자를 받을 수 있다.

1. delimiter : 요소 중간에 들어가는 구분자
2. prefix : 결과 맨 앞에 붙는 문자
3. suffix : 결과 맨 뒤에 붙는 문자

```java
List<String> collectorCollection =
  productList.stream()
    .map(Product::getName)
    .collect(Collectors.joining(", ", "<", ">"));
// <potatoes, orange, lemon, bread, sugar>
```

#### Collectors.averageingInt()

숫자 값의 평균을 낸다.

```java
Double averageAmount = 
  productList.stream()
    .collect(Collectors.averagingInt(Product::getAmount));
// 17.2
```

### Collectors.summingInt()

숫자값의 합을 낸다.

```java
Integer summingAmount = 
  productList.stream()
    .collect(Collectors.summingInt(Product::getAmount));
// 86
```

#### Collectors.summarizingInt()

정보를 한번에 얻을 수 있는 메서드이다.

```java
IntSummaryStatistics statistics = 
  productList.stream()
    .collect(Collectors.summarizingInt(Product::getAmount));
/* 
result
    {count=5, sum=86, min=13, average=17.200000, max=23}
    get 메서드로 가져올 수 있다.
*/
```

#### Collectors.groupingBy()

특정 조건으로 그룹지을 수 있다.

수량을 기준으로 매핑을 해보면, 같은 수량일때 리스트로 묶어준다.
```java
Map<Integer, List<Product>> collectorMapOfLists =
  productList.stream()
    .collect(Collectors.groupingBy(Product::getAmount));
/*
result
    {23=[Product{amount=23, name='potatoes'}, 
        Product{amount=23, name='bread'}], 
    13=[Product{amount=13, name='lemon'}, 
        Product{amount=13, name='sugar'}], 
    14=[Product{amount=14, name='orange'}]}
*/
```

#### Collectors.partitioningBy()

인자를 받아서 boolean 값을 리턴한다.

```java
Map<Boolean, List<Product>> mapPartitioned = 
  productList.stream()
    .collect(Collectors.partitioningBy(el -> el.getAmount() > 15));

/*
result
    {false=[Product{amount=14, name='orange'}, 
            Product{amount=13, name='lemon'}, 
            Product{amount=13, name='sugar'}], 
    true=[Product{amount=23, name='potatoes'}, 
            Product{amount=23, name='bread'}]}
*/
```

결과도 true와 false 두 가지로 나눠진다.

#### Collectors.collectingAndThen()

특정 타입으로 결과를 수집한 후 추가 작업이 필요할 때 사용된다.

Collectors.toSet()으로 결과를 Set으로 수집한 후 수정불가한 Set으로 변환해주는 예제이다.

```java
Set<Product> unmodifiableSet = 
  productList.stream()
    .collect(Collectors.collectingAndThen(Collectors.toSet(),
                Collections::unmodifiableSet));
```

#### Collectors.of()

직접 Collector를 만들 수도 있다.

supplier에 생성자를 넘겨주고, accumulator에는 리스트에 add메서드를 넘겨준다. 마지막으로 Combiner를 이용해 리스트를 합쳐준다.

```java
Collector<Product, ?, LinkedList<Product>> toLinkedList = Collector.of(LinkedList::new,
                LinkedList::add,
                (first, second) -> {
                    first.addAll(second);
                    return first;
                });

// 사용법
LinkedList<Product> linkedListOfPersons = productList.stream()
        .collect(toLinkedList);

// 확인
linkedListOfPersons.stream().
        foreach(t -> System.out::println(t.name))
```

### Matching

조건을 체크한 후 결과를 리턴한다.

* 하나라도 조건을 만족하는가? (<i>anyMatch</i>)
* 모두 조건을 만족하는가? (<i>allMatch</i>)
* 모두 조건을 만족하지 않는가? (<i>noneMatch</i>)

```java
List<String> names = Arrays.asList("apple", "amazon", "plane");

boolean anyMatch = names.stream()
  .anyMatch(name -> name.contains("a"));
boolean allMatch = names.stream()
  .allMatch(name -> name.length() > 3);
boolean noneMatch = names.stream()
  .noneMatch(name -> name.endsWith("s"));
```

결과는 모두 true이다.

### foreach

요소를 돌면서 실행되는 작업이다. peek는 중간 작업이고 foreach는 최종 작업이라는 차이가 있다.

```java
names.stream().forEach(System.out::println);
```
