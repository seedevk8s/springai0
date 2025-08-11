# 🚀 콘솔 자바 쇼핑몰 프로젝트 - 완벽한 Git 가이드 (통합본)

> 13명 취준생 (3팀 구성) | 2주 스프린트 | Pure Java Console Application  
> 실무 레벨의 Git 협업 전략과 실제 커밋 예시

---

## 📋 목차

### Part 1: Git 브랜치 전략
1. [프로젝트 개요](#1-프로젝트-개요)
2. [팀별 Repository 구성](#2-팀별-repository-구성)
3. [브랜치 전략](#3-브랜치-전략)
4. [일일 워크플로우](#4-일일-워크플로우)

### Part 2: 실제 프로젝트 적용
5. [Team 2 실제 구현 사례](#5-team-2-실제-구현-사례)
6. [2주간 실제 커밋 기록](#6-2주간-실제-커밋-기록)

### Part 3: 실무 가이드
7. [커밋 컨벤션](#7-커밋-컨벤션)
8. [Git 명령어 모음](#8-git-명령어-모음)
9. [취업 대비 포인트](#9-취업-대비-포인트)

---

# Part 1: Git 브랜치 전략

## 1. 프로젝트 개요

### 1.1 프로젝트 구조
```
java-shopping-mall/
├── src/
│   └── javaproject/
│       ├── domain/          # 엔티티 클래스 (User, Product, Order, Cart)
│       ├── controller/      # 사용자 입력 처리 및 화면 제어
│       ├── service/         # 비즈니스 로직
│       ├── repository/      # 데이터 저장/조회 (파일 시스템)
│       ├── util/           # 유틸리티 클래스
│       └── exception/      # 커스텀 예외 클래스
├── data/                   # 데이터 파일 저장 디렉토리
├── docs/                   # 프로젝트 문서
├── README.md
└── .gitignore
```

### 1.2 기술 스택
- **언어**: Java 11+
- **빌드**: 수동 컴파일 또는 IntelliJ/Eclipse
- **데이터**: 파일 시스템 (txt/csv)
- **UI**: 콘솔 인터페이스
- **VCS**: Git & GitHub

### 1.3 주요 도메인
- **User**: 사용자 정보 (회원, 관리자)
- **Product**: 상품 정보
- **Order**: 주문 정보
- **Cart**: 장바구니

---

## 2. 팀별 Repository 구성

### 2.1 독립 Repository 운영

| 팀 | Repository URL | 팀원 수 | 개발 중점 |
|---|---|---|---|
| **Team 1** | `github.com/team1-java/console-mall` | 4-5명 | 기본 기능 충실 |
| **Team 2** | `github.com/team2-java/console-mall` | 4-5명 | 사용자 경험 최적화 |
| **Team 3** | `github.com/team3-java/console-mall` | 4-5명 | 관리자 기능 강화 |

### 2.2 Repository 초기 설정 (팀장 수행)

```bash
# 1. GitHub에서 Repository 생성
# 2. 로컬에 클론
git clone https://github.com/team2-java/console-mall.git
cd console-mall

# 3. develop 브랜치 생성
git checkout -b develop
git push -u origin develop

# 4. 프로젝트 구조 생성
mkdir -p src/javaproject/{domain,controller,service,repository,util,exception}
mkdir -p data docs

# 5. .gitignore 설정
cat > .gitignore << EOF
*.class
*.iml
.idea/
.DS_Store
data/*.txt
data/*.csv
out/
target/
EOF

# 6. 초기 커밋
git add .
git commit -m "[chore] 프로젝트 초기 구조 설정"
git push origin develop
```

### 2.3 팀원 초대 및 권한 설정

1. **Settings → Manage access → Add people**
2. **팀원 GitHub ID로 초대**
3. **Role: Write 권한 부여**
4. **브랜치 보호 규칙 설정**
   - main: PR 필수, 2명 이상 리뷰
   - develop: PR 필수, 1명 이상 리뷰

---

## 3. 브랜치 전략

### 3.1 브랜치 계층 구조

```
main (배포용, 최종 완성본)
  └── develop (통합 개발)
        ├── feature/domain-[엔티티]-[이니셜]
        ├── feature/controller-[기능]-[이니셜]
        ├── feature/service-[기능]-[이니셜]
        ├── feature/repository-[엔티티]-[이니셜]
        ├── feature/util-[기능]-[이니셜]
        ├── feature/exception-[타입]-[이니셜]
        ├── hotfix/[이슈명]-[이니셜]
        └── release/v[버전]
```

### 3.2 패키지별 브랜치 명명 규칙

#### Domain 패키지
```bash
feature/domain-user-kjh       # User 엔티티
feature/domain-product-lsy    # Product 엔티티
feature/domain-order-pms      # Order 엔티티
feature/domain-cart-jhe       # Cart 엔티티
```

#### Controller 패키지
```bash
feature/controller-main-kjh       # MainController
feature/controller-user-lsy       # UserController
feature/controller-product-pms    # ProductController
feature/controller-order-jhe      # OrderController
feature/controller-admin-cjy      # AdminController
```

#### Service 패키지
```bash
feature/service-auth-kjh         # AuthenticationService
feature/service-product-lsy      # ProductService
feature/service-order-pms        # OrderService
feature/service-cart-jhe         # CartService
feature/service-report-cjy       # ReportService
```

#### Repository 패키지
```bash
feature/repository-base-cjy      # BaseRepository (인터페이스)
feature/repository-user-kjh      # UserRepository
feature/repository-product-lsy   # ProductRepository
feature/repository-order-pms     # OrderRepository
```

#### Util 패키지
```bash
feature/util-file-kjh            # FileManager
feature/util-validation-lsy      # InputValidator
feature/util-console-pms         # ConsoleUtil
feature/util-session-jhe         # SessionManager
```

---

## 4. 일일 워크플로우

### 4.1 아침 스탠드업 (09:00)

```bash
# 1. develop 최신화
git checkout develop
git pull origin develop

# 2. 어제 작업 확인
git log --oneline --graph -10

# 3. 오늘 작업 브랜치 생성
git checkout -b feature/domain-user-kjh
```

### 4.2 작업 중 (10:00 - 17:00)

```bash
# 작업 단위별 커밋
git add src/javaproject/domain/User.java
git commit -m "[feat] User 엔티티 클래스 구현 - 회원 정보 필드 정의"

# 추가 작업
git add src/javaproject/domain/UserRole.java
git commit -m "[feat] UserRole enum 추가 - USER, ADMIN 권한 구분"

# 테스트
java -cp src javaproject.ShoppingMallApplication
```

### 4.3 저녁 PR 생성 (17:00)

```bash
# 푸시
git push origin feature/domain-user-kjh

# GitHub에서 Pull Request 생성
# feature/domain-user-kjh → develop
# 리뷰어 지정, 설명 작성
```

### 4.4 PR 병합 후 (18:00)

```bash
# develop 최신화
git checkout develop
git pull origin develop

# 로컬 브랜치 정리
git branch -d feature/domain-user-kjh
```

---

# Part 2: 실제 프로젝트 적용

## 5. Team 2 실제 구현 사례

### 5.1 팀 구성 및 역할 분담

| 이름 | 역할 | 담당 패키지 | 주요 작업 |
|---|---|---|---|
| **김지훈** | 팀장 | domain, service | User 시스템, 인증 |
| **이서연** | 개발자 | domain, service | Product 관리 |
| **박민수** | 개발자 | domain, service | Order 처리 |
| **정하은** | 개발자 | controller, util | UI/메뉴 시스템 |
| **최준영** | 개발자 | repository, admin | 데이터 저장, 관리자 |

### 5.2 개인별 브랜치 할당

```yaml
김지훈:
  - feature/domain-user-kjh
  - feature/service-auth-kjh
  - feature/repository-user-kjh

이서연:
  - feature/domain-product-lsy
  - feature/service-product-lsy
  - feature/repository-product-lsy

박민수:
  - feature/domain-order-pms
  - feature/service-order-pms
  - feature/repository-order-pms

정하은:
  - feature/controller-main-jhe
  - feature/util-console-jhe
  - feature/domain-cart-jhe

최준영:
  - feature/repository-base-cjy
  - feature/util-file-cjy
  - feature/controller-admin-cjy
```

---

## 6. 2주간 실제 커밋 기록

### Week 1: 기반 구축 (1/6 ~ 1/10)

#### Day 1 (월) - 프로젝트 초기 설정

| 시간 | 작업자 | 브랜치 | 커밋 메시지 | 파일 |
|------|--------|---------|------------|------|
| 09:30 | 김지훈 | main | `[chore] Initial commit - Java 프로젝트 구조 생성` | .gitignore, README.md |
| 10:00 | 김지훈 | develop | `[docs] 프로젝트 요구사항 및 브랜치 전략 문서화` | REQUIREMENTS.md |
| 11:00 | 이서연 | develop | `[chore] 패키지 구조 설정 - MVC 패턴 적용` | 패키지 디렉토리 |
| 14:00 | 김지훈 | feature/domain-user-kjh | `[feat] User 도메인 클래스 구현` | User.java, UserRole.java |
| 15:00 | 이서연 | feature/domain-product-lsy | `[feat] Product 도메인 클래스 구현` | Product.java, Category.java |
| 15:30 | 정하은 | feature/util-console-jhe | `[feat] ConsoleUtil 클래스 구현 - 메뉴 출력` | ConsoleUtil.java |
| 16:30 | 박민수 | feature/util-file-pms | `[feat] FileManager 유틸리티 구현` | FileManager.java |

#### Day 2 (화) - Domain 클래스 구현

| 시간 | 작업자 | 브랜치 | 커밋 메시지 | 파일 |
|------|--------|---------|------------|------|
| 09:00 | 박민수 | feature/domain-order-pms | `[feat] Order, OrderItem 도메인 구현` | Order.java, OrderItem.java |
| 10:30 | 정하은 | feature/domain-cart-jhe | `[feat] Cart, CartItem 도메인 구현` | Cart.java, CartItem.java |
| 11:00 | 김지훈 | feature/domain-user-kjh | `[fix] User equals(), hashCode() 오버라이딩` | User.java |
| 14:00 | 최준영 | feature/util-session-cjy | `[feat] SessionManager 싱글톤 구현` | SessionManager.java |
| 15:30 | 이서연 | feature/domain-product-lsy | `[refactor] Product 검증 로직 추가` | Product.java |

**PR 생성**:
- PR #1: `feature/domain-user-kjh → develop` (User 도메인 완성)
- PR #2: `feature/domain-product-lsy → develop` (Product 도메인 완성)

#### Day 3 (수) - Repository 계층 구현

| 시간 | 작업자 | 브랜치 | 커밋 메시지 | 파일 |
|------|--------|---------|------------|------|
| 09:00 | 최준영 | feature/repository-base-cjy | `[feat] BaseRepository 인터페이스 정의` | BaseRepository.java |
| 10:00 | 김지훈 | feature/repository-user-kjh | `[feat] UserRepository 구현 - 파일 저장` | UserRepository.java |
| 11:00 | 이서연 | feature/repository-product-lsy | `[feat] ProductRepository 구현` | ProductRepository.java |
| 14:00 | 박민수 | feature/repository-order-pms | `[feat] OrderRepository 구현` | OrderRepository.java |
| 15:30 | 정하은 | feature/repository-cart-jhe | `[feat] CartRepository 싱글톤 구현` | CartRepository.java |

**충돌 해결**:
```bash
# 16:00 - BaseRepository 적용 시 메서드 시그니처 충돌
git checkout develop
git pull origin develop
git checkout feature/repository-user-kjh
git merge develop
# 충돌 해결 후
git add .
git commit -m "[fix] BaseRepository 인터페이스 적용 - 메서드 시그니처 통일"
```

#### Day 4 (목) - Service 계층 구현

| 시간 | 작업자 | 브랜치 | 커밋 메시지 | 파일 |
|------|--------|---------|------------|------|
| 09:00 | 김지훈 | feature/service-auth-kjh | `[feat] AuthenticationService 구현` | AuthenticationService.java |
| 10:30 | 김지훈 | feature/util-password-kjh | `[feat] PasswordUtil 암호화 유틸리티` | PasswordUtil.java |
| 11:00 | 이서연 | feature/service-product-lsy | `[feat] ProductService 검색 기능 구현` | ProductService.java |
| 14:00 | 박민수 | feature/service-order-pms | `[feat] OrderService 주문 처리 로직` | OrderService.java |
| 15:00 | 정하은 | feature/service-cart-jhe | `[feat] CartService 장바구니 관리` | CartService.java |
| 16:30 | 최준영 | feature/service-admin-cjy | `[feat] AdminService 관리자 기능` | AdminService.java |

#### Day 5 (금) - Controller 구현 & 통합

| 시간 | 작업자 | 브랜치 | 커밋 메시지 | 파일 |
|------|--------|---------|------------|------|
| 09:00 | 정하은 | feature/controller-main-jhe | `[feat] MainController 메인 메뉴 구현` | MainController.java |
| 10:00 | 김지훈 | feature/controller-user-kjh | `[feat] UserController 회원 메뉴 구현` | UserController.java |
| 11:00 | 이서연 | feature/controller-product-lsy | `[feat] ProductController 상품 조회` | ProductController.java |
| 14:00 | 김지훈 | develop | `[feat] ShoppingMallApplication 메인 클래스` | ShoppingMallApplication.java |
| 15:30 | 전체 | develop | `[test] 통합 테스트 - 전체 기능 연동 확인` | - |
| 17:00 | 김지훈 | develop | `[chore] Week 1 완료 - 모든 feature 통합` | - |

**마일스톤**: 🎉 1주차 완료 - 핵심 기능 구현 완성!

### Week 2: 완성 및 최적화 (1/13 ~ 1/17)

#### Day 6 (월) - 예외 처리 & 검증

| 시간 | 작업자 | 브랜치 | 커밋 메시지 | 파일 |
|------|--------|---------|------------|------|
| 09:00 | 김지훈 | feature/exception-auth-kjh | `[feat] Custom Exception 클래스 구현` | AuthenticationException.java |
| 10:30 | 이서연 | feature/util-validation-lsy | `[feat] InputValidator 유틸리티 구현` | InputValidator.java |
| 14:00 | 박민수 | feature/exception-io-pms | `[feat] FileIOException 처리 추가` | FileIOException.java |
| 15:30 | 정하은 | hotfix/menu-validation-jhe | `[fix] 메뉴 입력 검증 버그 수정` | MainController.java |

#### Day 7-8 (화-수) - 버그 수정 & 최적화

| 날짜 | 시간 | 작업자 | 브랜치 | 커밋 메시지 |
|------|------|--------|---------|------------|
| Day7 | 09:00 | 이서연 | hotfix/search-bug-lsy | `[fix] 상품 검색 대소문자 구분 해결` |
| Day7 | 14:00 | 박민수 | hotfix/order-stock-pms | `[fix] 주문 시 재고 차감 로직 수정` |
| Day8 | 10:00 | 김지훈 | refactor/code-cleanup-kjh | `[refactor] 중복 코드 제거 및 메서드 추출` |
| Day8 | 15:00 | 최준영 | feature/sample-data-cjy | `[feat] 샘플 데이터 생성 로직 추가` |

#### Day 9 (목) - 문서화 & 테스트

| 시간 | 작업자 | 브랜치 | 커밋 메시지 | 파일 |
|------|--------|---------|------------|------|
| 09:00 | 전체 | develop | `[test] 전체 시나리오 테스트 시작` | - |
| 10:00 | 김지훈 | docs/javadoc-kjh | `[docs] JavaDoc 주석 - User 도메인` | User 관련 클래스 |
| 11:00 | 이서연 | docs/javadoc-lsy | `[docs] JavaDoc 주석 - Product 도메인` | Product 관련 클래스 |
| 14:00 | 정하은 | docs/user-manual-jhe | `[docs] 사용자 매뉴얼 작성` | USER_MANUAL.md |
| 16:00 | 최준영 | docs/readme-cjy | `[docs] README.md 최종 업데이트` | README.md |

#### Day 10 (금) - 배포 준비 & 발표

| 시간 | 작업자 | 브랜치 | 커밋 메시지 | 파일 |
|------|--------|---------|------------|------|
| 09:00 | 김지훈 | release/v1.0.0 | `[release] v1.0.0 릴리즈 브랜치 생성` | - |
| 10:00 | 박민수 | release/v1.0.0 | `[chore] 실행 가능한 JAR 빌드 설정` | MANIFEST.MF |
| 11:00 | 김지훈 | main | `[release] v1.0.0 최종 릴리즈 - main 병합` | - |
| 11:30 | 김지훈 | main | `[tag] v1.0.0 태그 생성` | - |

**마일스톤**: 🏆 프로젝트 완료 - v1.0.0 릴리즈!

---

# Part 3: 실무 가이드

## 7. 커밋 컨벤션

### 7.1 커밋 메시지 형식

```
[타입] 제목 - 상세 설명

본문 (선택)

이슈: #번호
```

### 7.2 커밋 타입 및 예시

| 타입 | 설명 | 실제 예시 |
|---|---|---|
| **feat** | 새로운 기능 | `[feat] User 로그인 기능 구현 - 세션 관리 포함` |
| **fix** | 버그 수정 | `[fix] NullPointerException 해결 - null 체크 추가` |
| **docs** | 문서 수정 | `[docs] API 사용법 README 추가` |
| **style** | 코드 포맷팅 | `[style] Google Java Style Guide 적용` |
| **refactor** | 리팩토링 | `[refactor] if-else를 Strategy 패턴으로 변경` |
| **test** | 테스트 | `[test] UserService 단위 테스트 추가` |
| **chore** | 기타 | `[chore] .gitignore 업데이트` |

### 7.3 좋은 커밋 vs 나쁜 커밋

#### ✅ 좋은 커밋
```bash
git commit -m "[feat] User 회원가입 기능 구현 - 이메일 중복 체크 포함"
git commit -m "[fix] FileManager 한글 깨짐 문제 해결 - UTF-8 인코딩 설정"
git commit -m "[refactor] ProductService 검색 성능 개선 - 인덱스 활용"
```

#### ❌ 나쁜 커밋
```bash
git commit -m "수정"
git commit -m "asdf"
git commit -m "User 추가함"
```

---

## 8. Git 명령어 모음

### 8.1 기본 작업

```bash
# Repository 클론
git clone https://github.com/team2-java/console-mall.git

# 브랜치 생성 및 이동
git checkout -b feature/domain-user-kjh

# 상태 확인
git status

# 변경사항 확인
git diff

# 스테이징
git add .                           # 전체 파일
git add src/javaproject/domain/*.java  # 특정 패턴

# 커밋
git commit -m "[feat] 기능 구현"

# 푸시
git push origin feature/domain-user-kjh
```

### 8.2 브랜치 관리

```bash
# 브랜치 목록
git branch -a                    # 모든 브랜치
git branch -r                    # 원격 브랜치만

# 브랜치 이동
git checkout develop

# 브랜치 삭제
git branch -d feature/old-branch      # 로컬
git push origin --delete feature/old  # 원격

# 브랜치 이름 변경
git branch -m old-name new-name
```

### 8.3 병합 및 충돌 해결

```bash
# develop 최신화 후 병합
git checkout develop
git pull origin develop
git checkout feature/my-branch
git merge develop

# 충돌 해결
# 1. 충돌 파일 수정
# 2. 스테이징
git add .
# 3. 커밋
git commit -m "[fix] merge conflict 해결"

# Rebase (선택사항)
git rebase develop
```

### 8.4 히스토리 관리

```bash
# 로그 확인
git log --oneline                    # 간단히
git log --graph --oneline           # 그래프로
git log --author="김지훈"           # 특정 작성자

# 커밋 수정
git commit --amend                   # 마지막 커밋 수정

# Reset (주의!)
git reset --soft HEAD~1              # 커밋 취소, 변경사항 유지
git reset --hard HEAD~1              # 커밋 취소, 변경사항 삭제

# Stash
git stash                            # 임시 저장
git stash pop                        # 복원
```

### 8.5 태그 및 릴리즈

```bash
# 태그 생성
git tag -a v1.0.0 -m "첫 번째 릴리즈"
git push origin v1.0.0

# 태그 목록
git tag -l

# 태그 삭제
git tag -d v1.0.0                    # 로컬
git push origin --delete v1.0.0      # 원격
```

---

## 9. 취업 대비 포인트

### 9.1 면접 질문 대비

#### Q: "Git Flow에 대해 설명해주세요"
**A**: "저희 프로젝트에서는 Git Flow를 단순화한 전략을 사용했습니다. main 브랜치는 배포용, develop은 통합 개발용으로 사용했고, 기능별로 feature 브랜치를 생성했습니다. 특히 패키지 구조에 맞춰 브랜치를 명명하여 작업 영역을 명확히 구분했습니다."

#### Q: "코드 충돌은 어떻게 해결했나요?"
**A**: "일일 스탠드업 미팅으로 작업 영역을 조율했고, develop 브랜치를 자주 pull하여 충돌을 최소화했습니다. 충돌 발생 시에는 관련 팀원과 직접 소통하여 해결했으며, 총 5건의 충돌을 성공적으로 해결했습니다."

#### Q: "PR 리뷰는 어떻게 진행했나요?"
**A**: "모든 기능은 PR을 통해 병합했으며, 최소 1명 이상의 리뷰를 필수로 했습니다. 코드 스타일, 로직 검증, 테스트 여부를 중점적으로 리뷰했고, 이를 통해 코드 품질을 향상시켰습니다."

### 9.2 포트폴리오 작성 팁

```markdown
## 🚀 Java 콘솔 쇼핑몰 프로젝트

### 프로젝트 개요
- **기간**: 2025.01.06 ~ 2025.01.17 (2주)
- **인원**: 5명 (Team 2)
- **역할**: 팀장, User 도메인 및 인증 시스템 개발
- **GitHub**: [github.com/team2-java/console-mall](https://github.com/team2-java/console-mall)

### 기술 스택
- Java 11, 파일 시스템, MVC 패턴
- Git/GitHub (Git Flow 전략)

### 주요 성과
- ✅ 147개 커밋, 23개 PR 관리
- ✅ 5건의 Merge Conflict 해결
- ✅ 코드 리뷰를 통한 품질 개선
- ✅ JavaDoc 100% 문서화

### 담당 기능
1. **User 도메인 설계 및 구현**
   - 회원가입/로그인 기능
   - 세션 관리 (싱글톤 패턴)
   - 비밀번호 암호화

2. **Git 브랜치 전략 수립**
   - 패키지별 브랜치 전략 도입
   - PR 템플릿 및 리뷰 프로세스 구축

### 문제 해결
- **문제**: Repository 간 순환 참조 발생
- **해결**: 인터페이스 분리 원칙(ISP) 적용으로 의존성 역전
- **결과**: 결합도 감소 및 테스트 용이성 향상
```

### 9.3 GitHub 프로필 관리

1. **README.md 작성**
   - 프로젝트 소개
   - 실행 방법
   - 주요 기능
   - 스크린샷

2. **Wiki 활용**
   - 개발 가이드
   - API 문서
   - 트러블슈팅

3. **Issues & Projects**
   - 이슈 트래킹
   - 마일스톤 관리
   - 칸반 보드 활용

---

## 📊 프로젝트 통계

### 최종 성과
- **총 커밋 수**: 147개
- **Pull Requests**: 23개
- **해결된 이슈**: 18개
- **코드 라인**: 약 13,000줄
- **테스트 커버리지**: 75%

### 팀원별 기여도
| 팀원 | 커밋 수 | PR | 코드 추가 | 코드 삭제 |
|------|---------|-----|-----------|-----------|
| 김지훈 | 35 | 6 | +3,200 | -450 |
| 이서연 | 32 | 5 | +2,800 | -380 |
| 박민수 | 28 | 4 | +2,500 | -320 |
| 정하은 | 27 | 4 | +2,400 | -290 |
| 최준영 | 25 | 4 | +2,100 | -260 |

---

## 🎯 핵심 체크리스트

### 일일 체크리스트
- [ ] 아침에 develop 브랜치 pull
- [ ] feature 브랜치에서 작업
- [ ] 의미 있는 커밋 메시지 작성
- [ ] 테스트 후 PR 생성
- [ ] 팀원 PR 리뷰 참여

### 주간 체크리스트
- [ ] 개인 브랜치 정리
- [ ] 문서 업데이트
- [ ] 코드 리팩토링
- [ ] 통합 테스트
- [ ] 진행 상황 공유

---

## 📚 참고 자료

### Git 학습 자료
- [Pro Git Book](https://git-scm.com/book/ko/v2)
- [GitHub Flow Guide](https://guides.github.com/introduction/flow/)
- [Conventional Commits](https://www.conventionalcommits.org/)

### Java 프로젝트 구조
- [Maven Standard Directory Layout](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html)
- [Java Code Conventions](https://www.oracle.com/java/technologies/javase/codeconventions-contents.html)

---

## 💬 팀 커뮤니케이션

### Slack 채널
- `#general` - 일반 공지
- `#dev` - 개발 논의
- `#pr-review` - PR 리뷰 요청
- `#daily-standup` - 일일 스탠드업

### 회의 일정
- **일일 스탠드업**: 매일 09:00 (15분)
- **주간 회고**: 금요일 17:00 (30분)
- **코드 리뷰**: 화, 목 16:00 (1시간)

---

**Last Updated**: 2025년 1월  
**Version**: 1.0.0  
**Authors**: Team 2 - Java Shopping Mall Project

---

> 💡 **Remember**: "Good code is its own best documentation." - Steve McConnell

> 🚀 **Mission**: 실무와 동일한 수준의 Git 협업을 경험하고, 이를 통해 즉시 현업에 투입 가능한 개발자로 성장하기!