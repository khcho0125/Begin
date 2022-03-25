# Stream

간단하게 병렬처리가 가능하고 람다를 활용할 수 있는 기술 중 하나이다.

## 생성하기

### Array -> Stream

```java
String[] arr = new String[]{"a", "b", "c"};
Stream<String> stream = Arrays.stream(arr);
Stream<String> streamOfArrayPart = Arrays.stream(arr, 1, 3);
```

Arrays.stream(array) 로 Stream을 사용할 수 있다.

streamOfArrayPart의 Arrays.stream(array, 1, 3)는 index 1 ~ 2까지의 요소를 가져온다. ex) {"b", "c"}

### Collection -> Stream

컬렉션 타입(List, Set, Collection)의 경우 스트림 메서드를 스트림을 만들수 있다.

```java
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> stream = list.stream();
```

### Stream.empty()

빈 스트림이다. null 대신 사용가능하다.

### Stream.builder()

스트림에 직접 원하는 값을 넣을 수 있다. 마지막에 build 메서드로 스트림을 리턴한다.

```java
Stream<String> builderStream = Stream.<String>builder()
    .add("C").add("C++").add("Java")
    .build();
```

### Stream.generate()

generate 메서드를 이용하면 람다로 값을 넣을 수 있다.

크기가 정해져 있지 않고 무한하기 때문에 최대 크기를 제한해 주어야한다.

```java
Stream<String> generatedStream = 
    Stream.generate(() -> "gen").limit(5); 
```

### Stream.iterate()

iterate 메서드는 초기값과 해당 값을 이루는 람다를 사용하여 요소를 만든다. 예제엔 시작값 30부터 2씩 증가하는 값들이 요소로 생성된다.

generate 메서드와 같이 크기가 정해져 있지 않기 때문에 크기를 제한해 주어야한다.

```java
Stream<Integer> iteratedStream = 
    Stream.iterate(30, n -> n + 2).limit(5);
    // [30, 32, 34, 36, 38]
```

### 타입형 스트림

스트림은 제네릭을 사용한다. 하지만 제네릭을 사용하지 않고도 해당 타입의 스트림을 사용할 수 있다.

range와 rangeClosed은 종료지점을 포함하느냐 안하느냐의 차이이다.

```java
IntStream intStream = IntStream.range(1, 5); // [1, 2, 3, 4]
LongStream longStream = LongStream.rangeClosed(1, 5); // [1, 2, 3, 4, 5]
```

제네릭을 사용하지 않기 때문에 불필요한 오토 박싱(auto-boxing : 형 변환)은 일어나지 않는다.

필요한경우 boxed 메서드를 사용해서 boxing 할 수 있다.

```java
Stream<Integer> boxedIntStream = IntStream.range(1, 5).boxed();
```

Random 클래스로 난수를 생성해 스트림을 만들 수 있다.

```java
IntStream ints = new Random().ints(5);
```

### 문자열 스트림

문자열을 사용하여 스트림을 생성할 수 있다.

```java
IntStream charsStream =
    "Stream".chars(); // [83, 116, 114, 101, 97, 109]
```

정규식을 이용해 문자열을 자르고 요소를 생성할 수 있다.

```java
Stream<String> stringStream =
    Pattern.compile(", ").splitAsStream("Java, Kotlin, Spring");
    // [Java, Kotlin, Spring]
```

### 파일 스트림

자바 NIO의 Files클래스의 lines 메서드는 해당 파일의 각 라인을 String type 요소로 만들어 준다.

```java
Stream<String> lineStream =
    Files.lines(Paths.get("file.txt"), Charset.forName("UTF-8"));
```

### 병렬 스트림 Parallel Stream

Stream 대신 ParallelStream을 사용하여 병렬 스트림을 생성할 수 있다.

```java
// 병렬 스트림 생성
Stream<Product> parallelStream = productList.parallelStream();

// 병렬 여부 확인
boolean isParallel = parallelStream.isParallel();

// 배열을 사용한 병렬 스트림
Stream<Integer> parallelArray = Arrays.stream(arr).parallel();
```

다시 순차(sequential)모드로 돌리고 싶다면 sequntial 메서드를 사용한다.

```java
IntStream intStream = intStream.sequential();
```

### 스트림 연결하기

Stream.concat 메서드를 이용해 스트림을 결합할 수 있다.
```java
Stream<String> stream1 = Stream.of("Java", "C", "Kotlin");
Stream<String> stream2 = Stream.of("Python", "Go", "Swift");
Stream<String> conat = Stream.concat(stream1, stream2);
// [Java, C, Kotlin, Python, Go, Swift]
```

## 가공하기

아래의 예제이다.

```java
List<String> names = Arrays.asList("Eric", "Elena", "Java");
```

### Filtering

스트림내 요소를 하나씩 검사해 걸러내는 작업이다. 인자에 boolean을 리턴하는 검사식이 들어가면 된다.

```java
Stream<String> stream = names.stream().filter(name -> name.contains("a")); // [Elena, Java]
```

### Mapping

맵은 요소를 하나씩 특정 값으로 변환해 준다. 값을 변환하기 위해 람다를 인자로 받는다.

```java
Stream<String> stream = names.stream().map(String::toUpperCase);
// [ERIC, ELENA, JAVA]
```

요소 내 들어있는 상품 개체의 가격을 꺼내올 수 있다. '상품' -> '상품의 가격' 으로 매핑

```java
Stream<String> stream = names.stream().map(String::getPrice);
// [9990, 13000, 2600, 29900]
```

#### flatMap

flatMap은 중첩 구조를 한 단계 제거하고 단일 컬렉션으로 만들어주는 역할이다.

```java
List<List<String>> list = 
  Arrays.asList(Arrays.asList("a"), 
                Arrays.asList("b"));
// [[a], [b]]
```

예시와 같이 중첩된 리스트가 있을 때 flatMap으로 중첩을 제거한 후 작업할 수 있다.

```java
List<String> flatList = 
  list.stream()
  .flatMap(Collection::stream)
  .collect(Collectors.toList());
// [a, b]
```

객체로 예시를 들어보겠다.

```java
students.stream()
    .flatMapToInt(student -> IntStream.of(
        student.getKor(), 
        student.getEng(), 
        student.getMath()))
  .average()
  .ifPresent(avg -> 
    System.out.println(Math.round(avg * 10)/10.0));
```

위 예제의 순서이다.
1. flatMapToInt
    
    student 객체의 정보를 뽑아 중첩 구조의 스트림으로 만든다.

2. average

    스트림의 평균을 구해 스트림 중첩제거

3. ifPresent

    평균을 반올림 하여 출력

### Sorting

정렬은 Comparator를 이용한다.

```java
Stream<T> sorted();
Stream<T> sorted(Comparator<? super T> comparator);
```

인자 없이 호출시 오름차순 정렬

```java
List<String> lang = 
  Arrays.asList("Java", "Scala", "Groovy", "Python", "Go", "Swift");

lang.stream()
  .sorted()
  .collect(Collectors.toList());
// [Go, Groovy, Java, Python, Scala, Swift]

lang.stream()
  .sorted(Comparator.reverseOrder()) // 역순
  .collect(Collectors.toList());
// [Swift, Scala, Python, Java, Groovy, Go]
```

문자열 길이를 기준으로 정렬시

```java
lang.stream()
  .sorted(Comparator.comparingInt(String::length))
  .collect(Collectors.toList());
// [Go, Java, Scala, Swift, Groovy, Python]

lang.stream()
  .sorted((s1, s2) -> s2.length() - s1.length())
  .collect(Collectors.toList());
// [Groovy, Python, Scala, Swift, Java, Go]
```

### peek

요소들을 각각 대상으로 특정 행동을 수행하는 메서드인 peek는 그냥 확인해본다는 뜻으로 결과를 반환하지않는다.

그렇기 때문에 작업을 처리하는 중간에 결과를 확인할 때 사용할 수 있다.

```java
int sum = IntStream.of(1, 3, 5, 7, 9)
  .peek(System.out::println)
  .sum();
```

* ### To be continue
