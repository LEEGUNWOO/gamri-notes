
슈퍼타입과 서브타입의 키는 항상 같다. 상위 하위 실체이지만 부모 자식이 아니고 대등함.!!!!

두개의 테이블에 1개이상 그러니 다수의 FK가 존재 가능하다 (다대다) 또는 1:N도 가능
ORDER
-------------
order_id (PK)
customer_id (FK)
employee_id (FK)



---

# DB 복구 방식 정리 (Deferred / Immediate)

## 핵심 요약

```text
지연갱신 (Deferred Update)
→ REDO

즉시갱신 (Immediate Update)
→ UNDO + REDO

즉시갱신 (UNDO only 모델)
→ UNDO
```

---

# 1️⃣ 지연갱신 (Deferred Update)

## 개념

```text
COMMIT 이전에는 DB에 실제 데이터 반영 안함
```

## 흐름

```text
UPDATE
↓
로그 기록
↓
COMMIT
↓
DB 반영
```

## 복구

| 상태       | 처리    |
| -------- | ----- |
| commit   | REDO  |
| X commit | 처리 없음 |

## 이유

```text
DB에 아직 데이터가 기록되지 않았기 때문
```

---

# 2️⃣ 즉시갱신 (Immediate Update)

## 개념

```text
COMMIT 이전에도 DB(버퍼)에 데이터 반영 가능
```

## 흐름

```text
UPDATE
↓
로그 기록
↓
DB 버퍼 변경
↓
COMMIT
```

---

# 즉시갱신 복구 모델

## ① UNDO + REDO (실제 DBMS)

| 상태       | 처리   |
| -------- | ---- |
| commit   | REDO |
| X commit | UNDO |

```text
대부분 DBMS
Oracle / MySQL / PostgreSQL
```

**이유**

```text
디스크 반영 시점이 COMMIT 이후일 수도 있기 때문
```

---

## ② UNDO only 모델 (이론 모델)

| 상태       | 처리        |
| -------- | --------- |
| commit   | 이미 디스크 반영 |
| X commit | UNDO      |

```text
REDO 필요 없음
```

**가정**

```text
UPDATE 시 디스크에도 바로 기록된다고 가정
```

하지만

```text
디스크 쓰기 중 장애 문제
```

때문에 **실제 DBMS에서는 거의 사용 안함**

---
로그는..

|구분|로그|
|---|---|
|지연갱신|new value|
|즉시갱신|old value + new value|

# 실제 DBMS 구조

```text
WAL (Write-Ahead Logging)
```

```text
UPDATE
↓
REDO LOG 기록
↓
Buffer Cache 변경
↓
COMMIT
↓
나중에 Data File 기록
```

복구

```text
commit → REDO
미commit → UNDO
```

---

# 시험 핵심

```text
지연갱신 → REDO
즉시갱신 → UNDO + REDO
```

---


# 연관규칙 (Association Rule)

## 1️⃣ 지지도 (Support)

### 공식

```
Support(A → B) = (A ∩ B) / 전체 거래
```

### 쉬운 의미

```
A와 B가 동시에 나타난 비율
```

즉

```
전체 거래 중
A와 B를 같이 산 비율
```

### 예시

총 거래

```
100
```

|항목|거래|
|---|---|
|A 구매|40|
|B 구매|30|
|A와 B 함께 구매|20|

계산

```
Support = 20 / 100 = 0.2
```

의미

```
전체 거래의 20%가
A와 B를 같이 샀다
```

---

# 2️⃣ 신뢰도 (Confidence)

### 공식

```
Confidence(A → B) = (A ∩ B) / A
```

### 쉬운 의미

```
A를 산 사람 중
B도 산 비율
```

즉 **조건부 확률**

```
P(B | A)
```

### 예시

A 구매

```
40
```

A와 B 함께 구매

```
20
```

계산

```
Confidence = 20 / 40 = 0.5
```

의미

```
A를 산 사람 중
50%가 B도 산다
```

---

# 3️⃣ 향상도 (Lift)

### 공식

```
Lift(A → B) = Confidence(A → B) / P(B)
```

또는

```
Lift = P(A ∩ B) / (P(A) × P(B))
```

### 쉬운 의미

```
A가 있을 때
B 발생 확률이 얼마나 증가했는지
```

즉

```
A와 B의 연관성 정도
```

### 예시

B 구매

```
30
```

총 거래

```
100
```

```
P(B) = 30 / 100 = 0.3
```

신뢰도

```
0.5
```

계산

```
Lift = 0.5 / 0.3 ≈ 1.67
```

의미

```
A를 사면
B 구매 확률이 약 1.67배 증가
```

---

# Lift 해석

|Lift 값|의미|
|---|---|
|Lift > 1|양의 연관|
|Lift = 1|독립|
|Lift < 1|음의 연관|

---

# 시험용 핵심 요약

|지표|계산식|쉬운 의미|
|---|---|---|
|Support|(A ∩ B) / 전체|같이 산 비율|
|Confidence|(A ∩ B) / A|A 중 B 비율|
|Lift|Confidence / P(B)|연관성|


---

ACID

| 약어  | 영문          | 한국어 | 내용                                                                             |
| --- | ----------- | --- | ------------------------------------------------------------------------------ |
| A   | Atomicity   | 원자성 | 트랜잭션은 **전부 수행되거나 전부 수행되지 않아야 한다**. 일부만 수행되는 것은 허용되지 않는다. 실패 시 **Rollback** 된다. |
| C   | Consistency | 일관성 | 트랜잭션 수행 전후에 **데이터베이스는 항상 정의된 제약조건을 만족해야 한다**.                                  |
| I   | Isolation   | 격리성 | 동시에 실행되는 트랜잭션이 **서로의 중간 결과에 영향을 주지 않아야 한다**.                                   |
| D   | Durability  | 지속성 | 트랜잭션이 **Commit 되면 시스템 장애가 발생해도 결과가 영구적으로 보존된다**.                               |
A : All or Nothing
C : Constraint 유지
I : Interference 차단
D : Disk에 영구 저장