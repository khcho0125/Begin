# `MapStruct`

객체 간의 매핑을 편하게 해주는 라이브러리이다.

특징 - 빠르다.

## Gradle
라이브러리 목록
```java
dependencies {
    implementation 'org.mapstruct:mapstruct:1.4.2.Final'
    annotationProcessor "org.mapstruct:mapstruct-processor:1.4.2.Final"
    annotationProcessor 'org.projectlombok:lombok-mapstruct-binding:0.2.0'
	compileOnly 'org.projectlombok:lombok:1.18.22'
    annotationProcessor 'org.projectlombok:lombok:1.18.22'
}
```

## Generic Mapper example
```java
public interface GenericMapper<D, E> {

    D toDto(E e);
    E toEntity(D d);

    @BeanMapping(nullValuePropertyMappingStrategy = NullValuePropertyMappingStrategy.IGNORE)
    E updateFromDto(D dto, @MappingTarget E entity);
}
```

@BeanMapping : 매핑 정책을 설정한다.

@MappingTarget :  변환하여 객체를 return 하는게 아닌 매개변수로 받아 업데이트할 Target을 설정한다.

```java
@Mapper(componentModel = "spring")
public interface CustomMapper extends GenericMapper<MemberDto, Member> {
}
```

componentModel = "spring" : spring에 맞게 bean으로 등록해준다.

## MapStruct 사용법

객체마다 각각 다른 속성이 존재할 수 있다. 그렇기 때문에 지정을 해줘야 한다.

### @Mapper

Mapper를 사용하기 위해 쓰는 어노테이션이다.

### @Mapping

객체 간의 매핑을 할 때 사용하는 어노테이션이다.

## @Mapping 옵션

target : 매핑하려는 객체(결과)의 필드를 지정한다.

source : 매핑을 시도하는 객체(입력)의 필드를 지정한다.

constant : target을 지정된 값으로 매핑한다.

experssion : target 필드에 함수값을 넣어준다

ex) "java(memberDto.getName() + " is me")"

등등 많은 옵션들이 있다. 쓸만한 옵션들이 많이 보인다.

## 정책

#### unmappedSourcePolicy

Source의 필드가 Target의 필드에 매핑되지 않았을 때 정책이다.

1. IGNORE (default)
2. WARN
3. ERROR

ERROR로 설정시 Source 필드가 사용되지 않으면 컴파일 오류가 생긴다.

#### unmappedTargetPolicy

Target의 필드가 매핑되지 않을 때 정책이다.

1. IGNORE 
2. WARN (default)
3. ERROR

ERROR로 설정시 Target 필드가 사용되지 않으면 컴파일 오류가 생긴다.

#### typeConversionPolicy

타입 변환시 유실이 발생할 수 있을 때 정책이다.

1. IGNORE (default)
2. WARN
3. ERROR

long에서 int로 값을 넘길때 유실이 발생할 수 있다.

#### nullValueMappingStrategy

Source가 null일 때의 정책이다.

1. RETURN_NULL (default)
2. RETURN_DEFAULT

#### nullValuePropertyMappingStrategy

Source의 필드가 null일 때 정책이다.

1. SET_TO_NULL (default)
2. SET_TO_DEFAULT
3. IGNORE
