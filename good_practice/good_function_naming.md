#good-practice
# 좋은 함수명

> [!NOTE]
> 좋은 함수명은 단순히 함수 내부에서 무엇을 하는지를 설명하는 것이 아니라 1) 도메인/비즈니스 지식을 기반으로 2)호출하는 사람이 이해하기 쉬워 (재사용성 증가)야 한다.
> - bad: getUserRegisteredWithin7Days()
> - good: getNewUser()
> 

## 좋은 함수명이란?
좋은 함수명은 함수의 목적과 동작을 명확하게 표현하는 이름입니다. 함수를 처음 보는 개발자도 이름만 보고 함수가 무슨 일을 하는지 이해할 수 있어야 합니다.

예를 들어:
- `getUserData()` - 사용자 데이터를 가져오는 함수
- `calculateTotalPrice()` - 총 가격을 계산하는 함수
- `validateEmailFormat()` - 이메일 형식을 검증하는 함수

이처럼 함수명만으로도 해당 함수의 역할과 반환값을 예측할 수 있어야 합니다.

## 함수명을 지을 때 주의할 점
1. **동사로 시작하기**
- 함수는 어떤 동작을 수행하므로 동사로 시작
- get, set, is, has, calculate, validate 등 사용
- 예: `getUser()`, `setName()`, `isValid()`, `hasPermission()`

2. **명확하고 구체적인 이름 사용**
- 모호한 이름 피하기 (예: `process()`, `handle()`, `doSomething()`)
- 구체적인 동작 설명 (예: `processPayment()`, `handleUserRegistration()`)
- 약어 사용 자제 (예: `calculateTotal()` vs `calcTot()`)

3. **일관된 네이밍 컨벤션**
- 프로젝트 전체에서 일관된 스타일 유지
- 동일한 동작은 동일한 접두어 사용
- 예: getter/setter는 항상 get/set으로 시작

4. **불리언 함수는 질문 형태로**
- is, has, can, should 등으로 시작
- 예: `isEnabled()`, `hasChildren()`, `canEdit()`
- 반환값이 true/false임을 이름에서 암시

5. **단일 책임 원칙 반영**
- 함수가 수행하는 동작 하나만 이름에 표현
- 여러 동작을 하는 경우 분리 고려
- 예: `saveAndSend()` -> `save()` + `send()`

6. **컨텍스트 중복 피하기**
- 클래스나 모듈의 컨텍스트와 중복되는 이름 피하기
- 예: `class User { getUserName() }` -> `class User { getName() }`

7. **길이는 적절하게**
- 너무 짧지도, 너무 길지도 않게
- 이해하기 쉽되 간결하게
- 일반적으로 2-4개 단어 조합이 적당
