---
name: fullstack-code-reviewer
description: >
  풀스택 코드 리뷰, 보안 점검, 성능 최적화, 컨벤션 체크에 사용한다.
  "코드 리뷰해줘", "이 코드 문제 있어?", "보안 취약점 확인해줘",
  "React 코드 최적화해줘", "Spring Boot API 점검해줘" 같은 요청에 실행한다.
---

# 풀스택 코드 리뷰어

## 개요

vive-md 기술 스택 가이드(React, Vue, Next.js, Spring Boot, 보안, 디자인시스템)
총 6개 문서 ~17,000줄을 기반으로 한 전문 코드 리뷰 스킬.

---

## 리뷰 우선순위

```
CRITICAL  → 즉시 수정 (보안 취약점, 데이터 손실, 앱 크래시)
HIGH      → 빠른 수정 (성능 심각한 저하, 잘못된 비즈니스 로직)
MEDIUM    → 다음 PR에 수정 (코드 품질, 패턴 위반)
LOW       → 기술 부채 (리팩토링 권장, 최적화 기회)
INFO      → 제안 사항 (더 나은 방법 있음)
```

---

## 공통 체크리스트 (모든 스택)

### 보안 (CRITICAL)
- [ ] SQL Injection: 파라미터화 쿼리 사용 여부
- [ ] XSS: 사용자 입력 sanitize/escape 여부
- [ ] 인증/인가: 미인증 접근 가능한 엔드포인트 없는가
- [ ] 민감 정보: 하드코딩된 API Key, 비밀번호 없는가
- [ ] HTTPS: 프로덕션에서 HTTP 사용 없는가
- [ ] 의존성: 알려진 취약점 있는 패키지 없는가 (`npm audit`, `mvn dependency-check`)

### 코드 품질 (HIGH~MEDIUM)
- [ ] 함수/메서드가 단일 책임 원칙을 따르는가
- [ ] 중복 코드 없는가 (DRY)
- [ ] 매직 넘버/문자열 상수화 여부
- [ ] 에러 처리: 시스템 경계(입력, 외부 API)에서만 처리하는가
- [ ] 타입 안전성: `any` 남용 없는가 (TypeScript)

### 성능 (MEDIUM)
- [ ] N+1 쿼리 문제 없는가
- [ ] 불필요한 API 호출 없는가
- [ ] 메모리 누수 가능성 없는가

---

## 스택별 전문 체크리스트

### React 18+ / Next.js 14+

```typescript
// BAD: 불필요한 리렌더링
const Component = ({ data }) => {
  const processed = data.map(item => transform(item)); // 매 렌더링마다 실행
  return <List items={processed} />;
};

// GOOD: useMemo로 최적화
const Component = ({ data }) => {
  const processed = useMemo(() => data.map(item => transform(item)), [data]);
  return <List items={processed} />;
};
```

체크 항목:
- [ ] `useEffect` 의존성 배열 누락 없는가
- [ ] 큰 리스트에 가상화(react-window) 적용 여부
- [ ] `key` prop이 index가 아닌 고유 ID인가
- [ ] Server Components vs Client Components 적절히 분리 (Next.js)
- [ ] `use client` 최소화 (불필요한 클라이언트 번들 증가 방지)
- [ ] Image: `next/image` 사용 여부 (최적화)

### Vue 3

체크 항목:
- [ ] Composition API 사용 (Options API 혼용 지양)
- [ ] `ref` vs `reactive` 적절히 선택
- [ ] `v-for`에 `:key` 설정 여부
- [ ] `watchEffect` vs `watch` 올바른 선택
- [ ] Pinia 스토어 구조가 도메인별로 분리되어 있는가

### Spring Boot

```java
// BAD: N+1 쿼리
List<User> users = userRepository.findAll();
users.forEach(u -> u.getOrders().size()); // N번 추가 쿼리

// GOOD: fetch join
@Query("SELECT u FROM User u JOIN FETCH u.orders")
List<User> findAllWithOrders();
```

체크 항목:
- [ ] `@Transactional` 범위가 적절한가 (너무 넓거나 좁지 않은가)
- [ ] JPA N+1 쿼리 문제 (`@EntityGraph` 또는 fetch join 사용)
- [ ] 민감 정보가 API 응답에 포함되지 않는가 (DTO 분리)
- [ ] 글로벌 예외 핸들러 `@ControllerAdvice` 사용 여부
- [ ] 입력 검증: `@Valid` + `@NotNull` 등 적용 여부
- [ ] CORS 설정이 프로덕션에서 `*`(와일드카드) 아닌가

### API 설계

- [ ] REST 원칙 준수 (GET은 조회만, POST로 생성, PATCH로 부분 업데이트)
- [ ] HTTP 상태코드 올바른가 (200, 201, 400, 401, 403, 404, 500)
- [ ] 페이지네이션 적용 (대용량 목록 응답)
- [ ] API 버저닝 전략 있는가 (`/api/v1/...`)
- [ ] Rate Limiting 적용 여부

---

## 리뷰 결과 출력 형식

```markdown
## 코드 리뷰 결과

### 🔴 CRITICAL (즉시 수정)
- `파일경로:줄번호` — 문제 설명
  → 수정 방법: ...

### 🟠 HIGH
- `파일경로:줄번호` — 문제 설명
  → 수정 방법: ...

### 🟡 MEDIUM
...

### 💡 제안
...

### ✅ 잘 된 점
...
```

---

## 리뷰 전 확인할 것

1. 어떤 스택인가? (React, Vue, Spring Boot, 혼합?)
2. PR의 목적은? (신기능, 버그 수정, 리팩토링)
3. 특별히 집중해서 봐야 할 부분이 있는가?
4. 코드 전체인가, 특정 파일만인가?
