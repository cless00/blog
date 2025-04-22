#fullstack #mern
# MERN stack

### MERN 스택이란?
MERN은 웹 애플리케이션 개발에 사용되는 4가지 핵심 기술의 조합이다.

1. **MongoDB**
- NoSQL 데이터베이스
- JSON 형태의 문서 기반 데이터 저장
- 유연한 스키마 구조
- 높은 확장성과 성능

2. **Express.js**
- Node.js 웹 애플리케이션 프레임워크
- REST API 구축 용이
- 미들웨어 지원
- 라우팅 기능 제공

3. **React**
- Facebook에서 개발한 프론트엔드 라이브러리
- 컴포넌트 기반 UI 개발
- Virtual DOM으로 높은 성능
- 재사용 가능한 UI 컴포넌트

4. **Node.js**
- 서버 사이드 JavaScript 런타임
- 비동기 이벤트 기반 처리
- NPM을 통한 패키지 관리
- 높은 확장성

### MERN 스택의 장점

1. **JavaScript 기반**
- 전체 스택이 JavaScript/TypeScript 사용
- 학습 곡선 완만
- 코드 재사용성 높음

2. **효율적인 개발**
- JSON 데이터 형식 통일
- 풍부한 라이브러리 생태계
- 실시간 애플리케이션 개발 용이

3. **확장성**
- 수평적 확장 가능
- 클라우드 플랫폼 지원
- 마이크로서비스 아키텍처 적합

### 일반적인 아키텍처

```
클라이언트 층 (React)
    ↕
API 층 (Express + Node.js)
    ↕
데이터베이스 층 (MongoDB)
```

### 주요 사용 사례
1. 단일 페이지 애플리케이션 (SPA)
2. 소셜 미디어 플랫폼
3. 실시간 대시보드
4. 전자상거래 웹사이트
5. 협업 도구

### 개발 시작하기
```bash
# 기본 개발 환경 설정
1. Node.js 설치
2. MongoDB 설치
3. Create React App 사용하여 프로젝트 생성
4. Express 서버 설정
```

### 개발 팁
1. 보안 관련
- 환경 변수 사용
- 인증/인가 구현
- 데이터 유효성 검사

2. 성능 최적화
- 캐싱 전략 수립
- 코드 분할
- 레이지 로딩 적용

3. 개발 도구
- MongoDB Compass
- Postman/Insomnia
- React Developer Tools
- Redux DevTools

## [연습하기](practice_mern.md)