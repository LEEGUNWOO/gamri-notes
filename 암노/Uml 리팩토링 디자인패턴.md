 # 유즈케이스 다이어그램 USE CASE Diagram


![[Pasted image 20260309161656.png]]

요구사항을 정의할때 무엇을 할지 정의함

사용자 관점 요구사항정의

액터 : 외부에서 내부시스템에 접근하는 사용자나 시스템 등

  `기준 Use Case  ----<<include>>---->  포함 Use Case``\
                   : 포함관계 주 유즈케이스에서 필수로 사용하는 기능(공통 필수)
                   기준 유즈케이스는 포함유즈케이스 결과에 의존함
   `확장 Use Case ----<<extend>>----> 기준 Use Case`
                   : 확장 관계 선택관계 분기 선택적으로 수행됨)
                   기준 유즈케이스는 포함유즈케이스 결과에 의존하지 않음
                   일반화(추상 상속관계랑은 다름. 헷갈리지말자)

순서 : 시스템 범위 정의 > 액터식별 > 유즈케이스식별 > 관계 설정 및 다이어그램 작성>> 유즈케이스명세 작성

액터가 원하는 최종결과만 나타내는것이 바람직 하며 중간계산을 위한 기능은 도출하지 않는다

유즈케이스 내에 이벤트 흐름 전체는 모두 액터가 사용해야한다. 

액터끼리 관계 또는 유즈케이스에서 액터로의 흐름도 가능하다.

# 클래스 다이어그램 (Class Diagram)

![[Pasted image 20260310153251.png]]


|구분|질문|의미|표기|
|---|---|---|---|
|**가시성(Visibility)**|**누가 접근할 수 있나?**|접근 범위|`+ - # ~`|
|**스코프(Scope)**|**누구 소유인가?**|객체별인지, 클래스 공유인지|없음 / **밑줄**|
|**추상(Abstract)**|**구현이 미완성인가?**|하위 클래스가 구현 필요|_기울임꼴_|
|**읽기 전용(ReadOnly)**|**값 변경 가능한가?**|수정 불가|`{readOnly}`|

---

# 1. 가시성(Visibility)

**접근 권한**이다.  
즉 **밖에서 보이냐 / 못 건드리냐** 를 나타낸다.

| 표기  | 의미                         |
| --- | -------------------------- |
| `+` | public : 어디서나 접근 가능        |
| `-` | private : 자기 클래스 내부만 가능    |
| `#` | protected : 자기 + 하위 클래스 가능 |
| `~` | package : 같은 패키지 가능        |

예:

```text
- name : String
+ getName() : String
```

- `name` 은 외부 직접 접근 불가
    
- `getName()` 은 외부 호출 가능
    

---

# 2. 스코프(Scope)

**누가 가지는가** 를 뜻한다.

## 인스턴스 스코프(Instance Scope)

- **객체마다 따로 존재**
    
- **표시 없음**
    

예:

```text
name : String
```

학생 A의 `name`, 학생 B의 `name` 따로 있음

## 클래스 스코프(Class Scope)

- **클래스가 하나만 가짐**
    
- **밑줄**
    
- `static`
    

예:

```text
count : int
```

(밑줄)

학생 객체가 3개여도 `count` 는 1개만 존재

---

# 3. 추상(Abstract)

**아직 완성 안 된 것**이다.  
하위 클래스가 구체적으로 구현해야 한다.

- **추상 클래스**
    
- **추상 메서드**
    

표기: _기울임꼴_

예:

```text
*move()*
```

---

# 4. 읽기 전용(ReadOnly)

**값 변경 불가**

표기:

```text
id : String {readOnly}
```

이건 **static이랑 다르다**

- `static` = 공유
    
- `readOnly` = 수정 불가
    

---

# 핵심 구분

## 1) 가시성 vs 스코프

- **가시성**: 누가 접근 가능?
    
- **스코프**: 객체별인가? 클래스 공유인가?
    

## 2) static vs readOnly

- **static**: 하나를 공유
    
- **readOnly**: 값을 못 바꿈
    

## 3) 속성칸 = 인스턴스?

- 아님
    
- **속성칸은 그냥 속성 영역**
    
- 그 안의 각 속성이
    
    - 인스턴스일 수도 있고
        
    - static일 수도 있음
        


# 5. 다중도 표기

| 표기            | 의미     |
| ------------- | ------ |
| `1`           | 정확히 하나 |
| `0..1`        | 없거나 하나 |
| `*` 또는 `0..*` | 0개 이상  |
| `1..*`        | 1개 이상  |
|`n..m`|n개 이상 m개 이하|

---

# 한 줄씩 외우기

- **가시성 = 접근 범위**
    
- **스코프 = 소유 범위**
    
- **추상 = 미완성**
    
- **readOnly = 수정 불가**
    
- **밑줄 = 클래스 스코프(static)**
    
- **표시 없음 = 인스턴스 스코프**
    

---

# 예시 한 번에 보기

```text
Student
----------------
- name : String
- age : int
- count : int        ← 밑줄
- id : String {readOnly}
----------------
+ getName() : String
+ setAge(age:int) : void
*calcGrade()* : int
```

해석:

- `name`, `age` → 객체마다 따로 존재
    
- `count` → 모든 객체가 공유(static)
    
- `id` → 값 변경 불가
    
- `getName()` → 외부 호출 가능
    
- `calcGrade()` → 추상 메서드
    

---
요청한 방식으로 **Obsidian에 바로 붙여넣기 가능한 표**로 정리했습니다.  
(코드 예제 포함)

---

# Aggregation vs Composition (시험용 정리)

|구분|Aggregation (집합연관, Aggregation)|Composition (복합연관, Composition)|
|---|---|---|
|UML 표기|◇ (빈 다이아몬드)|◆ (채운 다이아몬드)|
|관계 강도|약한 관계 (Weak)|강한 관계 (Strong)|
|객체 생성 위치|**외부에서 생성 후 전달**|**내부에서 객체 생성**|
|객체 생명주기|독립적 존재 가능|전체 객체에 종속|
|객체 공유|여러 객체에서 공유 가능|공유 불가|
|예시 개념|Team – Player|Car – Engine|
|UML 구조|Whole ◇── Part|Whole ◆── Part|

---

# Aggregation 코드 예제

```java
class Engine {
}

class Car {
    private Engine engine;

    Car(Engine engine){
        this.engine = engine;
    }
}
```

설명

```text
Engine 객체를 외부에서 생성 후 Car에 전달
→ 약한 관계
→ Aggregation
```

UML

```text
Car ◇------ Engine
```

---

# Composition 코드 예제

```java
class Engine {
}

class Car {
    private Engine engine = new Engine();
}
```

또는

```java
class Car {
    private Engine engine;

    Car(){
        engine = new Engine();
    }
}
```

설명

```text
Car 내부에서 Engine 객체 생성
→ 강한 관계
→ Composition
```

UML

```text
Car ◆------ Engine
```


외부에서 객체 전달 → Aggregation ◇ (약한 관계)  
내부에서 new 생성 → Composition ◆ (강한 관계)

코드 기준 판단

|코드 패턴|UML 관계|
|---|---|
|`this.part = part` (외부 주입)|Aggregation ◇|
|`part = new Part()` (내부 생성)|Composition ◆|

예

// Aggregation  
class Car {  
    Engine engine;  
    Car(Engine engine){  
        this.engine = engine;  
    }  
}

// Composition  
class Car {  
    Engine engine = new Engine();  
}

---

 