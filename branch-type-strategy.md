# 🌳 Git 브랜치 타입별 전략 가이드

> 콘솔 자바 쇼핑몰 프로젝트 - 브랜치 종류와 사용 전략

---

## 📋 목차
1. [브랜치 타입 개요](#1-브랜치-타입-개요)
2. [메인 브랜치 (Main Branches)](#2-메인-브랜치)
3. [보조 브랜치 (Supporting Branches)](#3-보조-브랜치)
4. [브랜치 타입별 상세 전략](#4-브랜치-타입별-상세-전략)
5. [실제 사용 예시](#5-실제-사용-예시)

---

## 1. 브랜치 타입 개요

### 브랜치 계층 구조
```
main (production)
    └── develop (integration)
            ├── feature/*     (새 기능 개발)
            ├── release/*     (릴리즈 준비)
            ├── hotfix/*      (긴급 수정)
            ├── bugfix/*      (일반 버그 수정)
            ├── refactor/*    (코드 개선)
            ├── test/*        (테스트)
            └── docs/*        (문서화)
```

### 브랜치 타입 요약

| 브랜치 타입 | 목적 | 분기 from | 병합 to | 삭제 시점 |
|------------|------|-----------|---------|-----------|
| **main** | 프로덕션 배포 | - | - | 삭제 안 함 |
| **develop** | 개발 통합 | main | main | 삭제 안 함 |
| **feature** | 기능 개발 | develop | develop | 병합 후 |
| **release** | 릴리즈 준비 | develop | main, develop | 병합 후 |
| **hotfix** | 긴급 수정 | main | main, develop | 병합 후 |
| **bugfix** | 버그 수정 | develop | develop | 병합 후 |
| **refactor** | 코드 개선 | develop | develop | 병합 후 |
| **test** | 테스트 | develop | - | 테스트 후 |
| **docs** | 문서화 | develop | develop | 병합 후 |

---

## 2. 메인 브랜치

### 2.1 main 브랜치
```bash
# 특징
- 항상 배포 가능한 상태 유지
- 직접 커밋 금지
- 태그를 통한 버전 관리

# 접근 권한
- PR을 통해서만 병합
- 2명 이상의 리뷰 필수
- CI/CD 테스트 통과 필수
```

### 2.2 develop 브랜치
```bash
# 생성
git checkout main
git checkout -b develop
git push -u origin develop

# 특징
- 다음 릴리즈를 위한 개발 브랜치
- 모든 기능이 통합되는 브랜치
- feature 브랜치의 base

# 접근 권한
- PR을 통해서만 병합
- 1명 이상의 리뷰 필수
```

---

## 3. 보조 브랜치

### 3.1 feature 브랜치
```bash
# 명명 규칙
feature/[패키지]-[기능명]-[이니셜]

# 예시
feature/domain-user-kms
feature/controller-main-pjh
feature/service-auth-lje
```

### 3.2 release 브랜치
```bash
# 명명 규칙
release/v[메이저].[마이너].[패치]

# 예시
release/v1.0.0
release/v1.1.0
release/v2.0.0
```

### 3.3 hotfix 브랜치
```bash
# 명명 규칙
hotfix/[이슈명]-[이니셜]

# 예시
hotfix/login-error-kms
hotfix/null-pointer-lje
hotfix/file-encoding-pjh
```

### 3.4 bugfix 브랜치
```bash
# 명명 규칙
bugfix/[이슈번호]-[설명]-[이니셜]

# 예시
bugfix/issue-23-cart-calculation-kms
bugfix/issue-45-search-logic-lje
```

### 3.5 refactor 브랜치
```bash
# 명명 규칙
refactor/[대상]-[이니셜]

# 예시
refactor/user-service-kms
refactor/controller-structure-pjh
```

### 3.6 test 브랜치
```bash
# 명명 규칙
test/[테스트대상]-[이니셜]

# 예시
test/integration-kms
test/user-service-lje
```

### 3.7 docs 브랜치
```bash
# 명명 규칙
docs/[문서명]-[이니셜]

# 예시
docs/readme-update-kms
docs/javadoc-lje
docs/api-guide-pjh
```

---

## 4. 브랜치 타입별 상세 전략

### 4.1 Feature 브랜치 전략

#### 생성 및 작업
```bash
# 1. develop에서 분기
git checkout develop
git pull origin develop
git checkout -b feature/domain-user-kms

# 2. 작업 및 커밋
git add src/javaproject/domain/User.java
git commit -m "[feat] User 엔티티 구현"

# 3. 정기적으로 develop 동기화
git fetch origin
git merge origin/develop

# 4. 푸시
git push -u origin feature/domain-user-kms

# 5. PR 생성 (GitHub)
# feature/domain-user-kms → develop
```

#### 병합 후 정리
```bash
# PR 병합 후
git checkout develop
git pull origin develop
git branch -d feature/domain-user-kms
git push origin --delete feature/domain-user-kms  # 원격 브랜치 삭제
```

### 4.2 Release 브랜치 전략

#### 생성 시점
- 현재 develop의 기능이 릴리즈 준비 완료
- 더 이상 새 기능 추가 없음
- 버그 수정과 문서 작업만 진행

```bash
# 1. develop에서 분기
git checkout develop
git pull origin develop
git checkout -b release/v1.0.0

# 2. 버전 정보 업데이트
echo "version=1.0.0" > version.properties
git add version.properties
git commit -m "[release] v1.0.0 버전 정보 업데이트"

# 3. 버그 수정 (있을 경우)
git add .
git commit -m "[fix] 릴리즈 전 버그 수정"

# 4. main과 develop에 병합
# main에 병합
git checkout main
git merge --no-ff release/v1.0.0
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin main --tags

# develop에 병합
git checkout develop
git merge --no-ff release/v1.0.0
git push origin develop

# 5. 릴리즈 브랜치 삭제
git branch -d release/v1.0.0
git push origin --delete release/v1.0.0
```

### 4.3 Hotfix 브랜치 전략

#### 긴급 수정이 필요한 경우
```bash
# 1. main에서 분기 (중요!)
git checkout main
git pull origin main
git checkout -b hotfix/critical-login-error-kms

# 2. 긴급 수정
git add src/javaproject/service/AuthenticationService.java
git commit -m "[hotfix] 로그인 시 NullPointerException 긴급 수정"

# 3. main에 병합
git checkout main
git merge --no-ff hotfix/critical-login-error-kms
git tag -a v1.0.1 -m "Hotfix version 1.0.1"
git push origin main --tags

# 4. develop에도 병합 (중요!)
git checkout develop
git merge --no-ff hotfix/critical-login-error-kms
git push origin develop

# 5. hotfix 브랜치 삭제
git branch -d hotfix/critical-login-error-kms
git push origin --delete hotfix/critical-login-error-kms
```

### 4.4 Bugfix 브랜치 전략

#### 일반적인 버그 수정
```bash
# 1. develop에서 분기
git checkout develop
git pull origin develop
git checkout -b bugfix/issue-23-cart-total-kms

# 2. 버그 수정
git add src/javaproject/service/CartService.java
git commit -m "[fix] #23 장바구니 총액 계산 오류 수정"

# 3. PR 생성
git push -u origin bugfix/issue-23-cart-total-kms
# GitHub에서 PR: bugfix/issue-23-cart-total-kms → develop

# 4. 병합 후 삭제
git checkout develop
git pull origin develop
git branch -d bugfix/issue-23-cart-total-kms
```

### 4.5 Refactor 브랜치 전략

#### 코드 개선 작업
```bash
# 1. develop에서 분기
git checkout develop
git pull origin develop
git checkout -b refactor/service-layer-kms

# 2. 리팩토링 작업
git add .
git commit -m "[refactor] Service 레이어 중복 코드 제거"
git commit -m "[refactor] Strategy 패턴 적용"

# 3. 테스트 확인
# 모든 테스트 통과 확인 필수

# 4. PR 생성
git push -u origin refactor/service-layer-kms
```

---

## 5. 실제 사용 예시

### 5.1 Week 1 - 기능 개발 중심

```bash
# 월요일 - 도메인 개발
김민수: feature/domain-user-kms
이지은: feature/domain-product-lje
박준호: feature/domain-order-pjh
최서연: feature/domain-cart-csy

# 화요일 - 서비스 개발
김민수: feature/service-user-kms
이지은: feature/service-product-lje

# 수요일 - 버그 발견 및 수정
박준호: bugfix/issue-12-order-validation-pjh

# 목요일 - 컨트롤러 개발
최서연: feature/controller-main-csy

# 금요일 - 문서화
김민수: docs/readme-update-kms
```

### 5.2 Week 2 - 안정화 및 릴리즈

```bash
# 월요일 - 버그 수정
이지은: bugfix/issue-34-search-error-lje
박준호: bugfix/issue-35-menu-display-pjh

# 화요일 - 리팩토링
김민수: refactor/repository-pattern-kms

# 수요일 - 릴리즈 준비
최서연: release/v1.0.0

# 목요일 - 긴급 버그 발견
김민수: hotfix/critical-auth-error-kms

# 금요일 - 최종 릴리즈
main 브랜치에 v1.0.0 태그 생성
```

---

## 📊 브랜치 타입별 사용 빈도

### 일반적인 2주 프로젝트 기준

| 브랜치 타입 | 예상 개수 | 비율 |
|------------|-----------|------|
| feature | 15-25개 | 60% |
| bugfix | 5-10개 | 20% |
| refactor | 3-5개 | 10% |
| docs | 3-5개 | 5% |
| release | 1-2개 | 3% |
| hotfix | 0-2개 | 2% |

---

## ⚠️ 브랜치 타입별 주의사항

### Feature
- ✅ 작은 단위로 자주 커밋
- ✅ develop과 자주 동기화
- ❌ 너무 오래 유지하지 않기 (최대 3일)

### Release
- ✅ 새 기능 추가 금지
- ✅ 버그 수정과 문서 작업만
- ❌ develop에서 새 feature 병합 금지

### Hotfix
- ✅ main에서 분기 (중요!)
- ✅ 최소한의 수정만
- ✅ main과 develop 모두 병합
- ❌ 새 기능 추가 금지

### Bugfix
- ✅ Issue 번호 명시
- ✅ 테스트 코드 포함
- ❌ 관련 없는 수정 포함 금지

---

## 🎯 브랜치 전략 선택 가이드

### 상황별 브랜치 선택

| 상황 | 선택할 브랜치 타입 | 예시 |
|------|-------------------|------|
| 새 기능 개발 | feature | 로그인 기능 추가 |
| 배포된 버전 긴급 수정 | hotfix | 프로덕션 에러 |
| 개발 중 버그 수정 | bugfix | 테스트 중 발견된 버그 |
| 코드 구조 개선 | refactor | 중복 코드 제거 |
| 문서 작성/수정 | docs | README 업데이트 |
| 배포 준비 | release | v1.0.0 준비 |
| 실험적 기능 | test | 새 라이브러리 테스트 |

---

## 📝 브랜치 관리 베스트 프랙티스

### 1. 브랜치 생명주기
```
생성 → 작업 → PR → 리뷰 → 병합 → 삭제
```

### 2. 브랜치 정리 주기
- **일일**: feature 브랜치 정리
- **주간**: bugfix, refactor 브랜치 정리
- **릴리즈 후**: release, hotfix 브랜치 정리

### 3. 브랜치 네이밍 체크리스트
- [ ] 타입이 명확한가? (feature/bugfix/...)
- [ ] 작업 내용이 명확한가?
- [ ] 작업자 이니셜이 포함되었는가?
- [ ] 소문자와 하이픈만 사용했는가?

---

이제 브랜치 타입별 전략이 체계적으로 정리되었습니다! 😊