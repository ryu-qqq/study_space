# 리플렉션 (Reflection API)에 대해서 알아보기
🔗 https://blog.naver.com/d_d_o_l_/223777172196

리플렉션을 이용하여 엑셀 다운로드를 구현해봤지만, 문득 '리플렉션을 자주 이용하면 비용이 비싸기에 자주 사용해서는 안된다'라는 말이 왜 비싼건지, 왜 자주 사용하면 안된다는건지 궁금하였습니다.

그 과정에서 생각보다 내가 리플렉션을 잘 모르고 있구나를 인지를 해보고 이번 기회에 제대로 공부를 해보고자 정리해보게 되었습니다.

---

# 1. 리플렉션이란?
`런타임 환경`에서 클래스, 메서드, 필드, 생성자 등의 정보를 `동적`으로 조사하고, 조회할 수 있는 기능을 의미합니다.
보통, `컴파일 타임`에 정보(클래스, 메서드 호출 등)를 `런타임에 결정`하고 `활용`하고 싶을 때, 사용됩니다.
## 1-1. 컴파일 타임과, 런타임의 차이
처음, '런타임에 결정'한다는 이야기가 무슨 이야긴지 와닿지 않았었는데요. 먼저 런타임과 컴파일에 대한 정리부터 해보고자 합니다.
- 컴파일 타임 (Compile Time) : 코드가 컴파일될 때 결정되는 정보를 의미합니다. 즉, 실제 코드가 실행되기 전을 의미합니다.
    - 대표적인 예로, overload
- 런타임 (Runtime) : 코드가 실행되는 도중에 결정되는 정보를 의미합니다.

###### 예시
```java
final String className = "Sample";

Class<?> clazz = Class.forName(className);
```
- **컴파일 타임**
    - className이 "Sample"이라는 값을 가지는 사실
    - Class.forName(className)이 호출되는 코드가 존재한다는 사실
    - 위의 사실 모두, `JVM이 실행되기 전` 이미 작동된다는 것이 `확정`난 것을 의미합니다.
- **런타임**
    - JVM의 클래스 로딩 된 목록 중에서 "Samlple"이란 이름을 가진 클래스를 찾음
    - 해당 클래스를 메모리에 로드하고, `Class<?>` 객체에 할당
    - 이는 `런타임` 시점이 아니라면 알 수 없는 정보입니다.

그렇기에 컴파일 때 확정 난 데이터를 바탕으로, 런타임 지정하여 활용하고 싶을 때 리플렉션을 활용할 수 있게 됩니다.

## 1-2. 리플렉션의 주요 기능
리플렉션은 정적으로 코드를 작성하지 않더라도, 런타임에 여러 메타 데이터를 활용하여 다룰 수 있는 기능을 제공하고 있습니다.
### 1. 클래스 정보 조사
클래스 이름, 패키지 정보, 부모 클래스, 인터페이스 등 메타 정보를 조회할 수 있습니다.

### 2. 필드 정보 조회 및 조작
클래스 내부의 모든 필드 목록을 조회할 수 있습니다.
추가 설정을 통하여 `private`로 선언된 필드에서 접근이 가능합니다.
단, private 필드에 직접 접근을 함으로써 `캡슐화 위반`이 될 수 있습니다.

### 3. 메서드 정보 조회 및 호출
클래스 내부의 모든 메서드를 조회할 수 있습니다.
역시 추가 설정을 통해, `JVM 보안 검사를 우회`하여 `private`로 선언된 메서드에도 접근이 가능합니다.
`invoke()` 메서드를 이용하여 실행할 수 있습니다.

### 4. 생성자 정보 조회 및 인스턴스 생성
클래스의 생성자 정보를 조회하고, 추가로 private 생성자 호출도 가능합니다.
이를 통해 객체를 동적으로 생성할 수 있습니다.

### 5. 어노테이션 정보 읽기
클래스, 필드, 메서드, 생성자에 선언된 어노테이션 정보를 조회할 수 있습니다.

# 2. 리플렉션 사용법
리플렉션 API를 이용하기 위하여 별 다른 의존성 없이 단순 `java.lang.reflect.*`를 이용하여 사용할 수 있습니다.

## 2-1. 클래스 정보 조회
### Class 객체 정보 가져오기
#### - Class.forName(클래스명)
클래스 이름을 문자열로 받아 `Class<T>` 객체로 생성합니다.
`클래스가 동적으로 결정될 때` 주로 사용합니다.

예로, 외부 설정파일이나 입력값 기반으로 클래스 로드를하기 적합합니다.
만약, 입력 된 이름에 해당하는 클래스가 없을 경우, `ClassNotFoundException`이 발생하게 됩니다.
`static` 블록이 존재하는 클래스의 경우, 메서드 호출하는 즉시 클래스가 로드되며 `static` 블록이 실행되게 됩니다.

###### 예시
```java
final String className = "Sample";

Class<?> clazz = Class.forName(className); 
```

#### - 클래스명.class
`컴파일 타임에 특정 클래스가 정해져있을 때` 사용합니다.
해당 클래스 **존재 여부 판단 필요 없이** 바로 가져올 수 있습니다.
단, `동적 클래스의 로딩은 불가능`합니다.
즉, 실행 중에 변경할 수 없는 **고정된 클래스만** 가져올 수 있습니다.

###### 예시
```java
Class<?> clazz = Sample.class;
```

#### - 객체.getClass()
`이미 생성된 객체`에서 가져오는 방식입니다.
객체가 **존재할 때**에만 사용할 수 있습니다.
런타임에 특정 객체의 클래스 타입이 알고 싶을 때, 사용하기 적합합니다.

###### 예시
```java
final Sample sample = new Sample();  
  
Class<?> clazz = sample.getClass();
```
제네릭 타입 처리할 때, `T.class` 형식은 처리할 수 없기 때문에, `obj.getClass()` 형식으로 사용할 수 있씁니다.

### Class 이름 출력 방식
클래스 구조는 아래와 같은 예제를 바탕으로 결과 값을 작성하였습니다.
```java
package example;

class Parent {  
    static class Child {  
  
    }
}

@Test  
void claszzTest() {
	//given  
	final Parent parent = new Parent();  
	final Child child = new Parent.Child();  
	  
	// when  
	Class<?> clazz = child.getClass();
	...
}
```

| 메서드                | 설명                                                                                               | 결과                     |
| ------------------ | ------------------------------------------------------------------------------------------------ | ---------------------- |
| getName()          | 전체 클래스 이름 (패키지 포함)                                                                               | `example.Sample$Child` |
| getSimpleName()    | 간단한 클래스 이름 (패키지 제외)                                                                              | `Child`                |
| getCanonicalName() | 사람이 읽기 쉬운 정식 클래스 이름<br>익명클래스나, 내부 클래스 모두 읽기 쉬운 방식으로 출력<br>`getName()`과 비슷하나, 내부 클래스가 있을 때 차이 난다. | `example.Sample.Child` |
| getTypeName()      | 타입 이름 출력                                                                                         | `example.Sample$Child` |
| getPackage()       | 패키지 정보 출력                                                                                        | `package example`      |
| getPackageName()   | 패키지 이름 출력                                                                                        | `example`              |

### 부모 클래스 정보 조회
부모 클래스 정보도 조회가 가능합니다.

```java
class Parent {  
  
}  
  
class Child extends Parent {  
  
}
```
###### 예시
```java
final Child child = new Child();  
  
Class<?> clazz = child.getClass();

System.out.println(clazz.getSuperclass());

// 결과
// 결과 : class example.Parent
```

### 구현한 인터페이스 목록
`implements` 인터페이스 목록 조회가 가능합니다.
```java
interface Vehicle {  
  
}  
  
class Car implements Vehicle {  
  
}
```

###### 예시
```java
Class<?>[] classes = Car.class.getInterfaces();  
  
for (Class<?> c : classes) {  
    System.out.println(c.getName());  
}

// 결과
// example.Vehicle
```

## 2-2. 필드 정보 조회 및 조사
### 일반 필드 가져오기
클래스 내부에 `public`으로 선언된 필드를, `Field` 형식으로 가져올 수 있습니다.
```java
class Person {  
    public String name;
	String id;  
	protected String email;  
	private int age;
}
```
###### 예시
```java
Class<?> clazz = Person.class;  
  
Field name = clazz.getField("name");

System.out.println(name.getName());

// 결과
// name
```

단, 접근 제어자가 `default`, `protected`, `private`인 경우는 Exception이 발생하게 됩니다.
```java
Field id = clazz.getField("id"); //  ❌ NoSuchFieldException 발생!

// 결과
// java.lang.NoSuchFieldException: id
// at java.base/java.lang.Class.getField(Class.java:1999)
// ...
```

### 그 외 필드 가져오기
public 외의 접근 제어자가 `default`, `protected`, `private` 의 경우 `.getDeclaredFiled(이름)`형식으로 필드 정보를 가져올 수 있습니다.

###### 예시
```java
Field id = clazz.getDeclaredField("id");  
System.out.println(id.getName());  
  
Field email = clazz.getDeclaredField("email");  
System.out.println(email.getName());

Field age = clazz.getDeclaredField("age");
System.out.println(age.getName());

// 결과
// id
// email
// age
```


### 값 가져오기
간단하게 `.get`을 이용하여 가져올 수 있습니다.
```java
Person person = new Person("홍길동", 13);  
  
Class<?> clazz = person.getClass();

Field field = clazz.getField("name");  
String name = (String) field.get(person);  
  
System.out.println(name);

// 결과
// 홍길동
```

단, `private`의 경우에는 `.get `을 이용하여 값을 가져오려고 하면, 아래와 같은 Exception이 발생하게 됩니다.
```
class kr. cannot access a member of class example.Person with modifiers "private"
java.lang.IllegalAccessException: class kr.ControllerTest cannot access a member of class example.Person with modifiers "private"
```
이를 위해서는 별도 설정을 해주어야 합니다.
#### .setAccessible(true)
`private`으로 제한이 되어있는 필드에 접근하기 위해서는 해당 옵션을 설정 해 주어야 합니다.
```java
Field field = clazz.getDeclaredField("age");  
field.setAccessible(true);  
  
int age = (int) field.get(person);

System.out.println(age);

// 결과
// 13
```
해당 설정을 해줌으로써, `JVM 보안 검사`를 우회하여 접근을 할 수 있게 됩니다.
`private` 필드에 직접 접근하는만큼, `캡슐화 위반`에 조심해야 합니다.

### 값 변경하기
field의 값을 직접 `.set`을 통해서 할당을 해줄 수 있습니다.
```java
Person person = new Person("홍길동", 13);  
  
Class<?> clazz = person.getClass();  
Field field = clazz.getDeclaredField("age");  
field.setAccessible(true);  
  
field.set(person, 99);  
  
System.out.println(person.getAge());

// 결과
// 99
```


## 2-3. 메서드 정보 조회 및 실행
### private 메서드 가져오기
field와 비슷하게 `.getDeclaredMethod`를 이용하여, `Method` 타입으로 반환하여 사용할 수 있습니다.
###### 예시
```java
Person person = new Person("홍길동", 13);  
  
Class<?> clazz = person.getClass();  
Method method = clazz.getDeclaredMethod("getAge");  
  
System.out.println(method.getName());

// 결과
// getAge
```

## 2-4. 생성자 정보 조회 및 인스턴스 생성
`Constructor` 형식으로 위와 동일한 방식으로 생성자를 가져올 수 있습니다.
### private 생성자 가져오기
```java
Person person = new Person("홍길동", 13);  
  
Class<?> clazz = person.getClass();  
Constructor<?> constructor = clazz.getConstructor(String.class, int.class);  
  
System.out.println(constructor.getName());

// 출력
// com.sample.Person
```

### 인스턴스 생성하기
`.newInstance`를 이용하여 직접 인스턴스를 동적으로 생성할 수 있습니다.
```java
Class<?> clazz = Person.class;  
Constructor<?> constructor = clazz.getConstructor(String.class, int.class);  
Person person = (Person) constructor.newInstance("홍길동", 13);  
  
System.out.println(person.getName());

// 결과
// 홍길동
```

# 3. 리플렉션 단점과 주의점
## 3-1. 단점
- 성능 문제 : 일반적인 메서드 호출보다 느립니다.
    -  `invoke` 등 메서드 사용하게 되면, `오버헤드`가 발생하게 됩니다.
- 캡슐화 위반 : private로 선언한 메서드나 필드에 접근이 가능합니다.
    - 이로인해 `보안 문제`가 발생할 수 있습니다.
- 컴파일 타임 안정성 부족 : 존재하지 않는 메서드나 필드를 호출할 경우, `런타임 오류`가 발생하게 됩니다.

## 3-2.사용을 피해야하는 경우
- 주로 대량 데이터 처리 등, `성능`이 중요할 때
- 암호화, 인증 관련 로직 등 `보안`이 중요할 때
- 컴파일 타임에 검증이 필요할 때

## 3-3. invoke()는 왜 오버헤드가 발생하는가?
단점에서 설명했던, invoke() 호출 시 어떠한 오버헤드가 발생하는지에 대해서 궁금하였습니다.
일반적인 메서드 호출과 리플렉션을 사용한 메서드 호출을 비교하여, 왜 오버헤드가 발생하는지 정리해보았습니다.

직접 메서드를 호출하게 되면 아래와 같이 간단하게 처리할 수 있습니다.
```java
Person person = new Person("홍길동", 13);  
  
int age = person.getAge();  
  
System.out.println("홍길동의 나이는... : " + age);
```

하지만 리플렉션 API를 이용하여 처리하게 되면,
```java
Person person = new Person("홍길동", 13);  
  
Class<?> clazz = Person.class;  
Method method = clazz.getMethod("getAge");  
  
int age = (int) method.invoke(person);  
  
System.out.println("홍길동의 나이는... : " + age);
```
###### invoke()가 느린 이유
- **1. 메서드 탐색** → 클래스 내부에서 메서드 목록을 검색해야 함
- **2. 보안 검사** → private 메서드라면 JVM 보안 검사를 수행해야 함
- **3. 매개변수 변환** → 기본형(int)은 Object로 감싸지고, 실행 후 다시 변환해야 함
- **4. JIT 최적화 제외** → invoke()는 런타임에 메서드를 찾으므로 JIT 컴파일러가 최적화하지 않음
- **5. 실행 결과 변환** → 반환값이 Object 타입이므로 다시 변환해야 함

이러한 과정을 거쳐 메서드가 실행이 되기 때문에, `오버헤드`가 발생한다고 하며,
리플렉션 API를 이용했을 때, 비용이 크게 든다는 위와 같은 이유와 동일합니다.

# 4. 리플렉션 사용시 최적화 방법
자주 리플렉션을 사용하는 클래스의 경우, 스프링이 실행될 때, `@PostConstruct`를 이용하여 미리 캐싱하는 방식으로 오버헤드를 줄일 수 있습니다.

```java
@Component  
public class ClassCache {  
  
    private final Map<Class<?>, Map<String, Field>> classFieldCache = new HashMap<>();  
  
    @PostConstruct  
    public void init() {  
        System.out.println("🔄 클래스 필드 정보 캐싱 시작...");  
  
        cacheClassFields(Person.class);  
  
        System.out.println("✅ 캐싱 완료: " + classFieldCache);  
    }  
  
    private void cacheClassFields(Class<?> clazz) {  
        Map<String, Field> fieldMap = new HashMap<>();  
        for (Field field : clazz.getDeclaredFields()) {  
            field.setAccessible(true);  
            fieldMap.put(field.getName(), field);  
        }  
        classFieldCache.put(clazz, fieldMap);  
    }  
  
    public Field getField(Class<?> clazz, String fieldName) {  
        return classFieldCache.getOrDefault(clazz, new HashMap<>()).get(fieldName);  
    }  
}
```
`@PostConstruct`는 해당 클래스가 스프링 빈으로 등록되고, 의존성 주입이 완료된 후 한 번 실행됩니다.
이를 통해 최초 한 번 초기화를 하고 이후에는 빠르게 가져다 쓰는 방식으로 보다 빠르게 메타 데이터를 활용할 수 있게 됩니다.

```java
@Autowired  
private ClassCache classCache;  
  
void test() {  
    Field field = classCache.getField(Person.class, "age");  
  
    System.out.println(field.getName());  
}

// 결과
// age
```

---

이를 통해 리플렉션에 대해서 정리를 해보았습니다.
기존에 알던 내용에서 이번 시간을 통해 더 자세하게 내용을 덧붙일 수 있어 좋았습니다.

특히, 이전에 어노테이션을 이용해 만들었던 라이브러리들에 대해서도 개선하면 좋은 점에 대해서 인사이트를 얻게 된 것 같습니다.

추가로, `JVM 보안 매니저(Security Manager)` 라는 용어도 처음 듣게 되었는데,
이 부분도 이후에 같이 정리해보면 좋겠다고 생각이 들었습니다. 👍🏻

보니까, 자바 17부터는 사용을 할 수 없다고 하는데, 왜 사용을 하지 않게 되었는지가 궁금합니다.