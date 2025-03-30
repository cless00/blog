# Assert 사용

> [!NOTE]
> 코틀린에서 require(), check(), assert()를 적극 사용하자.

## require()
require()는 함수의 파라미터를 검증하는데 사용되는 함수

### 주요 특징
- 파라미터가 유효한지 검사하는 용도로 사용
- 조건이 false일 경우 IllegalArgumentException 발생
- 런타임에 항상 검사가 수행됨

## check()
check()는 상태를 검증하는데 사용되는 함수

### 주요 특징
- 현재 상태가 유효한지 검사하는 용도로 사용
- 조건이 false일 경우 IllegalStateException 발생
- 런타임에 항상 검사가 수행됨
- 비즈니스 로직의 상태를 검증하는데 적합

## assert()
assert()는 코드가 정상적으로 동작하는지 검증하는데 사용되는 함수

### 주요 특징
- 코드가 정상적으로 동작하는지 검증하는 용도로 사용
- 조건이 false일 경우 AssertionError 발생
- JVM의 -ea(enable assertion) 옵션이 활성화된 경우에만 동작
- 테스트 환경에서만 사용하는 것이 권장됨

