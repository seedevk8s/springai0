# 🚀 콘솔 자바 쇼핑몰 프로젝트 - Git 브랜치 전략 가이드

> 13명 취준생 (3팀 구성) | 2주 스프린트 | Pure Java Console Application

## 📋 목차
1. [프로젝트 개요](#1-프로젝트-개요)
2. [팀별 Repository 구성](#2-팀별-repository-구성)
3. [브랜치 전략](#3-브랜치-전략)
4. [패키지별 브랜치 명명 규칙](#4-패키지별-브랜치-명명-규칙)
5. [일일 워크플로우](#5-일일-워크플로우)
6. [커밋 컨벤션](#6-커밋-컨벤션)
7. [2주 타임라인](#7-2주-타임라인)
8. [자주 사용하는 Git 명령어](#8-자주-사용하는-git-명령어)

---

## 1. 프로젝트 개요

### 프로젝트 구조
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
└── README.md
```

### 기술 스택
- **언어**: Java 11+
- **빌드**: 수동 컴파일 또는 IntelliJ/Eclipse
- **데이터 저장**: 파일 시스템 (txt/csv)
- **UI**: 콘솔 인터페이스
- **VCS**: Git & GitHub

---

## 2. 팀별 Repository 구성

### 각 팀 독립 Repository 운영
| 팀 | Repository URL | 팀원 | 중점 사항 |
|---|---|---|---|
| Team 1 | github.com/team1-java/console-mall | 4-5명 | 기본 기능 충실 |
| Team 2 | github.com/team2-java/console-mall | 4-5명 | 사용자 경험 최적화 |
| Team 3 | github.com/team3-java/console-mall | 4-5명 | 관리자 기능 강화 |

### Repository 초기 설정 (팀장 수행)
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

# 5. .gitignore 생성
echo "*.class" > .gitignore
echo "*.iml" >> .gitignore
echo ".idea/" >> .gitignore
echo "data/*.txt" >> .gitignore

# 6. 초기 커밋
git add .
git commit -m "[chore] 프로젝트 초기 구조 설정"
git push origin develop
```

---

## 3. 브랜치 전략

### 브랜치 계층 구조
```
main (배포용)
  └── develop (통합 개발)
        ├── feature/domain-[이니셜]    # 도메인 모델 개발
        ├── feature/controller-[이니셜] # 컨트롤러 개발
        ├── feature/service-[이니셜]    # 서비스 로직 개발
        ├── feature/repository-[이니셜] # 저장소 개발
        ├── feature/util-[이니셜]       # 유틸리티 개발
        └── hotfix/[이슈명]-[이니셜]    # 긴급 버그 수정
```

### 브랜치 보호 규칙
- **main**: PR 필수, 2명 이상 리뷰, 테스트 통과
- **develop**: PR 필수, 1명 이상 리뷰

---

## 4. 패키지별 브랜치 명명 규칙

### 4.1 Domain 패키지 담당
```
feature/domain-user-kjh      # User 엔티티 개발
feature/domain-product-lsy   # Product 엔티티 개발
feature/domain-order-pms     # Order 엔티티 개발
feature/domain-cart-jhe      # Cart 엔티티 개발
```

### 4.2 Controller 패키지 담당
```
feature/controller-main-kjh      # MainController (메인 메뉴)
feature/controller-user-lsy      # UserController (회원 관리)
feature/controller-product-pms   # ProductController (상품 조회)
feature/controller-order-jhe     # OrderController (주문 처리)
feature/controller-admin-cjy     # AdminController (관리자)
```

### 4.3 Service 패키지 담당
```
feature/service-auth-kjh        # AuthenticationService (인증)
feature/service-product-lsy     # ProductService (상품 관리)
feature/service-order-pms       # OrderService (주문 처리)
feature/service-cart-jhe        # CartService (장바구니)
feature/service-report-cjy      # ReportService (통계)
```

### 4.4 Repository 패키지 담당
```
feature/repository-user-kjh     # UserRepository
feature/repository-product-lsy  # ProductRepository
feature/repository-order-pms    # OrderRepository
feature/repository-base-cjy     # BaseRepository (공통 인터페이스)
```

### 4.5 Util 패키지 담당
```
feature/util-file-kjh          # FileManager (파일 입출력)
feature/util-validation-lsy    # InputValidator (입력 검증)
feature/util-console-pms       # ConsoleUtil (콘솔 출력)
feature/util-session-jhe       # SessionManager (세션 관리)
```

### 4.6 Exception 패키지 담당
```
feature/exception-auth-kjh     # AuthenticationException
feature/exception-business-lsy # BusinessException
feature/exception-io-pms       # FileIOException
```

---

## 5. 일일 워크플로우

### 아침 스탠드업 (9:00)
```bash
# 1. develop 브랜치 최신화
git checkout develop
git pull origin develop

# 2. 작업 브랜치 생성
git checkout -b feature/domain-user-kjh

# 3. 작업 시작
```

### 작업 중 (10:00 - 17:00)
```bash
# 작업 단위별 커밋
git add src/javaproject/domain/User.java
git commit -m "[feat] User 엔티티 클래스 구현 - 회원 정보 필드 정의"

# 추가 작업
git add src/javaproject/domain/UserRole.java
git commit -m "[feat] UserRole enum 추가 - USER, ADMIN 권한 구분"
```

### 저녁 PR 생성 (17:00)
```bash
# 푸시
git push origin feature/domain-user-kjh

# GitHub에서 PR 생성: feature/domain-user-kjh → develop
# 팀원 리뷰 요청
```

---

## 6. 커밋 컨벤션

### 커밋 메시지 형식
```
[타입] 제목 - 상세 설명 (선택)

본문 (선택)

이슈: #번호 (선택)
```

### 커밋 타입
| 타입 | 설명 | 예시 |
|---|---|---|
| feat | 새로운 기능 추가 | `[feat] User 로그인 기능 구현` |
| fix | 버그 수정 | `[fix] 상품 검색 시 NullPointerException 해결` |
| docs | 문서 수정 | `[docs] README.md 설치 방법 추가` |
| style | 코드 포맷팅 | `[style] 코드 정렬 및 불필요한 공백 제거` |
| refactor | 코드 리팩토링 | `[refactor] UserService 중복 코드 제거` |
| test | 테스트 코드 | `[test] UserRepository 단위 테스트 추가` |
| chore | 빌드, 설정 변경 | `[chore] .gitignore 수정` |

### 좋은 커밋 메시지 예시
```bash
# ✅ 좋은 예
git commit -m "[feat] User 엔티티 구현 - ID, 비밀번호, 이름, 이메일 필드 추가"
git commit -m "[fix] FileManager 파일 읽기 시 인코딩 문제 해결 - UTF-8 명시"
git commit -m "[refactor] ProductService 검색 로직 개선 - 스트림 API 활용"

# ❌ 나쁜 예
git commit -m "수정"
git commit -m "User 추가"
git commit -m "버그 수정함"
```

---

## 7. 2주 타임라인

### Week 1 (1/6 ~ 1/10)

#### Day 1-2: 프로젝트 설정 & Domain 개발
- Repository 생성 및 팀원 초대
- 프로젝트 구조 설정
- Domain 클래스 구현 (User, Product, Order, Cart)
- 필요한 Enum 클래스 구현

#### Day 3: Repository 계층 구현
- BaseRepository 인터페이스 정의
- UserRepository, ProductRepository 구현
- FileManager 유틸리티 구현
- 파일 입출력 테스트

#### Day 4: Service 계층 구현
- 각 도메인별 Service 클래스 구현
- 비즈니스 로직 구현
- 예외 처리 로직 추가

#### Day 5: Controller 구현 & 통합
- MainController 구현
- 각 기능별 Controller 구현
- 메뉴 시스템 구현
- 1주차 통합 테스트

### Week 2 (1/13 ~ 1/17)

#### Day 6-7: 추가 기능 & 예외 처리
- Custom Exception 클래스 구현
- 입력 검증 강화
- 관리자 기능 구현
- 세션 관리 구현

#### Day 8: 버그 수정 & 최적화
- 통합 테스트
- 버그 수정
- 코드 리팩토링
- 성능 최적화

#### Day 9: 문서화
- JavaDoc 작성
- 사용자 매뉴얼 작성
- README.md 완성
- 테스트 시나리오 문서화

#### Day 10: 최종 점검 & 발표 준비
- 최종 테스트
- Release 브랜치 생성
- main 브랜치 병합
- 발표 자료 준비

---

## 8. 자주 사용하는 Git 명령어

### 기본 작업 플로우
```bash
# Repository 클론
git clone https://github.com/team2-java/console-mall.git
cd console-mall

# 브랜치 생성 및 이동
git checkout develop
git checkout -b feature/domain-user-kjh

# 작업 후 커밋
git add .
git commit -m "[feat] User 클래스 구현"

# 푸시
git push origin feature/domain-user-kjh
```

### 충돌 해결
```bash
# develop 최신 내용 가져오기
git checkout develop
git pull origin develop

# 작업 브랜치로 이동 후 병합
git checkout feature/domain-user-kjh
git merge develop

# 충돌 해결 후
git add .
git commit -m "[fix] merge conflict 해결"
git push origin feature/domain-user-kjh
```

### PR 병합 후 정리
```bash
# develop으로 이동
git checkout develop
git pull origin develop

# 로컬 브랜치 삭제
git branch -d feature/domain-user-kjh

# 원격 브랜치 삭제 (선택사항)
git push origin --delete feature/domain-user-kjh
```

### 유용한 명령어
```bash
# 브랜치 목록 확인
git branch -a

# 커밋 히스토리 확인
git log --oneline --graph

# 변경사항 확인
git status
git diff

# 특정 파일만 스테이징
git add src/javaproject/domain/User.java

# 마지막 커밋 수정
git commit --amend

# 태그 생성 (릴리즈)
git tag -a v1.0.0 -m "첫 번째 릴리즈"
git push origin v1.0.0
```

---

## 📌 핵심 규칙

### ✅ DO
- 매일 아침 develop 브랜치 pull
- 기능 단위로 작은 커밋
- PR 생성 시 상세한 설명 작성
- 코드 리뷰 적극 참여
- 충돌 발생 시 팀원과 소통

### ❌ DON'T
- main 브랜치 직접 커밋
- develop 브랜치 직접 커밋
- 테스트 없이 PR 생성
- 큰 단위의 커밋
- 의미 없는 커밋 메시지

---

## 💡 Pro Tips

### 면접 대비 포인트
1. **Git Flow 경험**: "팀 프로젝트에서 Git Flow를 적용하여 체계적으로 협업했습니다"
2. **코드 리뷰**: "PR을 통한 코드 리뷰로 코드 품질을 향상시켰습니다"
3. **충돌 해결**: "5명이 동시 작업하며 발생한 충돌을 원활히 해결했습니다"
4. **커밋 관리**: "의미 있는 커밋 메시지로 프로젝트 히스토리를 관리했습니다"
5. **브랜치 전략**: "패키지별 브랜치 전략으로 병렬 개발을 수행했습니다"

### 포트폴리오 활용
- GitHub Repository URL 제시
- 커밋 히스토리로 기여도 증명
- PR과 코드 리뷰 내역 제시
- 프로젝트 문서화 능력 어필

---

## 📞 문의 및 지원

- **Slack**: #java-shopping-mall
- **멘토링**: 매일 14:00-15:00
- **코드 리뷰**: 화, 목 17:00

---

**Last Updated**: 2025년 1월
**Version**: 1.0.0