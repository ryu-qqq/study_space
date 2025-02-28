# CS 파트 

# 🛠️ JVM의 밑바닥까지 파헤치기 - 내 코드 한 줄이 JVM에서 미치는 영향

## **📌 공부 목적**
> 단순히 코드를 작성하는 것이 아니라, **객체 생성, 변수 할당, 메서드 호출이 JVM에서 어떻게 처리되는지** 심층적으로 분석한다.  
> 특히 **가비지 컬렉터(GC)가 어떻게 동작하며, 효율적으로 관리하기 위해 코드 레벨에서 어떤 최적화를 할 수 있는지**를 탐구한다.  
> 또한, **내가 만든 로깅 시스템이 대용량 트래픽 환경에서도 안정적으로 동작하는지 검증하고, 언제부터 문제가 발생하는지 분석한다.**  
> 이를 위해 **동시성(Concurrency) 문제와 Java 내부 동작을 파헤쳐, Thread-Safety를 보장하는 메커니즘을 깊이 이해하는 것**이 목표다.

---

## **🔥 이 연구를 진행하는 이유 (Why?)**
✅ **대용량 트래픽에서도 내가 만든 라이브러리는 안전한가?**  
✅ **ThreadLocal을 다수의 Virtual Thread가 사용하게 되면 성능에 어떤 영향을 미치는가?**  
✅ **객체가 무한정 생성되는 구조가 시스템의 리소스를 얼마나 잡아먹는가?**  
✅ **요청이 많아질수록, 내가 만든 라이브러리가 시스템을 죽이는 구조는 아닌가?**  
✅ **운영 환경에서 신뢰할 수 있는 정량화된 스펙을 확보해야 한다.**

---

## **📖 이 연구가 중요한 이유 (Impact)**
> **"내가 만든 시스템을 무턱대고 사용할 수는 없다."**  
> **"내가 만든 라이브러리가 실제 운영 환경에서도 안정적인지 검증해야 한다."**

✅ **토스 같은 실무 환경에서, 개발자가 만든 시스템은 반드시 신뢰할 수 있는 스펙을 가져야 한다.**  
✅ **내 시스템이 고객 경험과 서비스 품질에 직접적인 영향을 미치기 때문.**  
✅ **운영팀, QA팀, 그리고 다른 개발자들이 내가 만든 라이브러리를 안심하고 사용할 수 있도록, 정확한 성능 지표를 확보해야 한다.**  
✅ **이를 위해, JVM 관점에서 깊이 파고들어 객관적인 데이터를 기반으로 검증한다.**

---

## **📖 학습 진행 방법**
### **1️⃣. JVM과 객체의 생명주기 분석**
> "new" 키워드 하나가 JVM에서 어떤 과정을 거치는가?  
> 객체가 Eden → Survivor → Old Generation으로 이동하는 과정과 GC가 이를 정리하는 방식

---

### **2️⃣. ThreadLocal & LogEntryContextManager의 GC 최적화**
> "ThreadLocal은 정말 성능에 부담을 주는가?"  
> **ThreadLocal이 GC에서 어떻게 관리되는지, 그리고 이를 최적화할 방법이 있는지** 분석

---

### **3️⃣. Virtual Thread에서의 GC 및 Context 전파 분석**
> "Virtual Thread에서는 GC가 어떻게 동작할까?"  
> 기존 ThreadPool 기반 시스템과 Virtual Thread 기반 시스템의 GC 부하 비교

---

### **4️⃣. Prometheus Core & JVM 메트릭 수집 방식 분석 (JNI 활용 여부 탐색)**
> "JVM의 메트릭을 수집하는 과정에서 JNI가 어떻게 활용되는가?"  
> Prometheus Core의 내부 동작을 분석하고, 메트릭 수집 과정에서 JNI가 개입하는 부분을 파악하여  
> **JVM과 Native Code의 상호작용을 이해하고 최적화 가능성을 탐색한다.**

---

### **5️⃣. 대용량 요청 환경에서 내 라이브러리의 성능 검증**
> "대량 트래픽이 들어오면 내 로깅 시스템은 언제부터 무리가 가기 시작할까?"  
> 내 라이브러리가 **실제 고트래픽 환경에서도 성능을 유지하는지, 어느 시점에서 한계가 오는지** 분석

---

### **6️⃣. 동시성(Concurrency) & Thread-Safety 분석**
> "내 로깅 시스템은 멀티스레드 환경에서 안전한가?"  
> "Java 내부에서 동시성을 어떻게 보장하는가?"  
> **대용량 트래픽에서 발생할 수 있는 동시성 문제를 분석하고, 이를 해결하기 위해 Java의 Concurrent 패키지를 깊이 파헤친다.**

📌 **공부할 내용**
- **Thread-Safety 개념과 동시성 이슈 (Race Condition, Visibility, Reordering)**
- **Synchronized, Lock, ReentrantLock 내부 코드 분석**
- **ConcurrentHashMap & CopyOnWriteArrayList 같은 동시성 컬렉션 분석**
- **Java 21에서 제공하는 새로운 동시성 기능 분석**
- **Virtual Thread 환경에서 동시성 처리가 기존 ThreadPool과 어떻게 다른지 연구**
- **Spring의 @Transactional, AOP 기반 비동기 처리 방식 분석**

---

## **🚀 최종 목표**
📌 JVM 내부 동작을 고려하여 최적화된 코드 작성 습관을 기른다.  
📌 내가 만든 로깅 시스템이 GC 및 성능에 미치는 영향을 정량적으로 분석한다.  
📌 Virtual Thread & 기존 Thread의 성능 차이를 JVM 관점에서 실험하여 실무 적용 가능성을 평가한다.  
📌 Prometheus Core 분석을 통해 JNI와 JVM의 상호작용을 이해하고, JVM 메트릭 수집의 최적화 방향을 탐색한다.  
📌 **대용량 트래픽에서도 내 로깅 시스템이 정상 동작하는지 검증하고, 문제 발생 임계점을 찾는다.**  
📌 **Java Concurrent 패키지를 분석하여 동시성 문제를 해결하는 원리를 학습하고, 실제 운영 환경에서의 적용 가능성을 검토한다.**  
📌 **Spring 라이브러리 내부 코드 분석을 통해, 트랜잭션 및 AOP 기반 동작 원리를 파악한다.**

---

## **📚 참고 자료**
- 📖 *"JVM의 밑바닥까지 파헤치기"*
- 📖 *"Java Concurrency in Practice"*
- 📖 **Spring Framework 내부 코드 분석 (GitHub 공식 레포지토리)**
    - [Spring Core](https://github.com/spring-projects/spring-framework/tree/main/spring-core)
    - [Spring AOP](https://github.com/spring-projects/spring-framework/tree/main/spring-aop)
    - [Spring Transaction](https://github.com/spring-projects/spring-framework/tree/main/spring-tx)
- 📖 **Java 공식 라이브러리 분석**
    - [java.util.concurrent (Java 21 공식 문서)](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/concurrent/package-summary.html)
    - [Virtual Thread 관련 공식 문서](https://openjdk.org/jeps/425)  




# DB 파트

# 🛠️ MySQL 내부 구조 및 성능 최적화 연구

## **📌 공부 목적**
> 서버 통합을 고려하면서 **MySQL의 성능 최적화**가 중요한 과제로 떠오름.  
> 특히, **대량 트랜잭션 처리 및 멀티스레드 환경에서의 동작 방식**을 연구하여,  
> **Bulk Insert, 트랜잭션 일관성, 인덱스 최적화, InnoDB 내부 구조를 깊이 이해하는 것**이 목표다.

---

## **🔥 학습 목표 & 접근 방식**
✅ **멀티스레드 환경에서 `LAST_INSERT_ID()`의 일관성 보장 여부 분석**  
✅ **트랜잭션 격리 수준 (Isolation Level)에 따른 데이터 정합성 검증**  
✅ **InnoDB의 MVCC (Multi-Version Concurrency Control) 동작 방식 분석**  
✅ **Bulk Insert의 성능 튜닝 및 InnoDB vs MyISAM 성능 비교**  
✅ **Redo Log & Undo Log의 역할 및 성능 최적화 전략 탐색**  
✅ **MySQL에서 Row Lock & Table Lock의 차이 및 성능 테스트**  
✅ **MySQL Replication & Sharding 아키텍처 분석**

---

## **📖 이 연구를 진행하는 이유 (Why?)**
✅ **대량 트래픽에서도 안정적인 데이터 정합성을 유지하려면 어떻게 설계해야 하는가?**  
✅ **MySQL 내부에서 트랜잭션이 어떻게 처리되는지 정확히 이해해야 한다.**  
✅ **Bulk Insert가 실제 성능에 미치는 영향을 정량적으로 분석해야 한다.**  
✅ **Row Lock & Table Lock의 차이를 이해하고, 어떻게 적용해야 하는지 결정해야 한다.**  
✅ **MySQL 내부 동작을 깊이 이해해야, 운영 환경에서 발생할 수 있는 문제를 예측하고 대응할 수 있다.**

---

## **📖 학습 진행 방법**
### **1️⃣. 트랜잭션 & 격리 수준 (Isolation Level) 심층 분석**
> MySQL의 `READ UNCOMMITTED`, `READ COMMITTED`, `REPEATABLE READ`, `SERIALIZABLE` 차이를 이해하고,  
> **멀티스레드 환경에서 트랜잭션 충돌이 어떻게 발생하는지 실험을 통해 검증한다.**

---

### **2️⃣. InnoDB 내부 아키텍처 분석**
> "InnoDB는 데이터 일관성을 어떻게 보장하는가?"  
> **MVCC(Multi-Version Concurrency Control) & Undo Log & Redo Log의 동작 방식 심층 분석**

---

### **3️⃣. MySQL에서 동시성 처리 & 락 메커니즘 연구**
> "Row Lock vs Table Lock: 언제, 어떻게 사용해야 하는가?"  
> **InnoDB와 MyISAM에서 Lock의 동작 차이 및 성능 테스트**

---

### **5️⃣. MySQL Replication & Sharding 아키텍처 분석**
> "고트래픽 환경에서 MySQL을 확장하는 방법은?"  
> **MySQL Replication(마스터-슬레이브) 및 Sharding 전략 분석**

---

## **🚀 최종 목표**
📌 MySQL 내부 동작을 정확히 이해하고, 성능 최적화를 위한 튜닝 전략을 습득한다.  
📌 **트랜잭션 일관성을 유지하면서도 성능을 극대화하는 방법을 연구한다.**  
📌 **대량 데이터 처리(Bulk Insert & 대량 트랜잭션 처리)에 최적화된 설계를 검토한다.**  
📌 **MySQL의 락 메커니즘(Row Lock, Table Lock)을 깊이 이해하고, 실무에서 적절히 적용하는 법을 익힌다.**  
📌 **MySQL을 수평적으로 확장하기 위한 Replication & Sharding 아키텍처를 학습한다.**

---

## **📚 참고 자료**
- 📖 *"High Performance MySQL"*
- 📖 **MySQL 공식 문서 **
  - [https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-isolation.html](https://dev.mysql.com/doc/refman/8.0/en)



# 아키텍쳐 파트

## **📌 공부 목적**
> 현재 사용하는 기술 스택(Spring + MySQL)만으로 **대용량 트래픽을 감당할 수 있는 최적의 구조를 찾는다.**  
> 만약 한계를 맞이한다면, **추가적으로 어떤 설계적 개선이 필요한지 분석한다.**  
> 또한, **잘 설계된 시스템이 테스트 코드 작성에 미치는 영향을 탐구하고, 효과적인 테스트 전략을 연구한다.**

---

## **🔥 학습 목표 & 접근 방식**
✅ **Spring + MySQL만으로 최대 성능을 끌어올릴 방법 탐색**  
✅ **대량 요청 처리 시, 현재 시스템의 병목이 어디에서 발생하는지 분석**  
✅ **애플리케이션 레벨에서 튜닝 가능한 성능 최적화 실험 (Thread Pool, Connection Pool, Query Optimization)**  
✅ **부하 테스트를 통해 MySQL의 한계점 및 개선 방향 탐색**  
✅ **이후 확장이 필요하다면, 어떤 설계적 접근이 필요한지 고민**  
✅ **시스템 설계와 테스트 코드 작성의 상관관계 연구**

---

## **📖 7️⃣. 대용량 트래픽 & 시스템 확장 실험**
> "Spring + MySQL만으로 최대 성능을 뽑아내려면 어떻게 해야 하는가?"  
> "한계를 맞이하면, 어떤 아키텍처적 개선이 필요한가?"

📌 **실험할 내용**
- **생각해보기**

📌 **한계를 맞이한다면?**
- **DB가 병목이면, Read Replica 추가로 해결 가능한가?**
- **Application 레이어에서 트래픽을 분산하는 방법은?**

---

## **📖 8️⃣. 시스템 설계와 테스트 코드의 관계 연구 (테스트 코드 챕터 - 우선순위 낮음)**
> "잘 설계된 시스템이 테스트 코드 작성에 미치는 영향은?"  
> "단순히 테스트 코드를 작성하는 것이 아니라, 어떻게 하면 쉽게 테스트할 수 있는 구조를 만들 수 있을까?"

📌 **실험할 내용**
- **"SOLID 원칙을 적용하면 테스트 코드 작성이 쉬워지는가?"**
- **"Spring의 DI (Dependency Injection) 설계를 최적화하면 Mocking 없이 테스트할 수 있는가?"**
- **"Integration Test vs Unit Test의 경계를 정확히 구분할 수 있는 방법은?"**
- **"테스트 코드도 유지보수 대상이다. 이를 효율적으로 관리하는 전략은?"**

📌 **결론: 테스트 코드는 설계의 결과물이다.**
> **테스트가 쉽다면, 시스템이 잘 설계된 것이다.**  
> **테스트 코드가 어렵다면, 시스템 설계가 개선될 여지가 있다는 의미다.**

---

## **🚀 최종 목표**
📌 Spring + MySQL 환경에서 **최대한의 성능을 끌어낼 방법을 찾는다.**  
📌 부하 테스트를 통해 시스템이 한계를 맞이하는 순간을 정량적으로 분석한다.  
📌 이후 확장성을 고려할 필요가 있다면, 어떤 아키텍처적 접근이 필요한지 연구한다.  
📌 테스트 코드 작성의 본질을 탐구하고, **쉽게 테스트할 수 있는 시스템 설계를 고민한다.**

---

## **📚 참고 자료**
- 📖 *"Junit In Action 3rd"*
