# 🚀 팀 4명의 패키지별 브랜치 생성 실습 가이드

> 콘솔 자바 쇼핑몰 프로젝트 - 실제 브랜치 생성 명령어 예시

---

## 👥 팀 구성 및 역할 분담

| 이름 | 이니셜 | 담당 패키지 | 주요 작업 |
|------|--------|------------|-----------|
| **김민수** | kms | domain, repository | User, Product 엔티티 및 저장소 |
| **이지은** | lje | service | 비즈니스 로직 (인증, 주문) |
| **박준호** | pjh | controller | 메뉴 시스템, 사용자 입력 처리 |
| **최서연** | csy | util, exception | 유틸리티, 예외 처리 |

---

## 📅 Day 1 (월요일) - 프로젝트 시작

### 🕘 09:00 - 팀 전체 Repository 클론 및 초기 설정

```bash
# 각 팀원이 자신의 컴퓨터에서 실행
git clone https://github.com/team2-java/console-mall.git
cd console-mall

# 원격 저장소 확인
git remote -v

# develop 브랜치로 이동
git checkout develop

# 최신 상태 확인
git pull origin develop
```

---

## 🌿 개인별 브랜치 생성 명령어

### 1️⃣ 김민수 (kms) - Domain & Repository 담당

```bash
# 현재 develop 브랜치에 있는지 확인
git branch
# * develop

# User 도메인 작업 브랜치 생성
git checkout -b feature/domain-user-kms

# 작업 확인
echo "현재 브랜치: feature/domain-user-kms"
echo "작업 파일: src/javaproject/domain/User.java"

# 첫 번째 커밋
git add src/javaproject/domain/User.java
git commit -m "[feat] User 엔티티 클래스 생성 - 기본 필드 정의"

# 원격 저장소에 푸시
git push -u origin feature/domain-user-kms
```

**오후 작업 - Product 도메인**
```bash
# develop으로 돌아가기
git checkout develop
git pull origin develop

# Product 도메인 브랜치 생성
git checkout -b feature/domain-product-kms

# 작업 후 커밋
git add src/javaproject/domain/Product.java
git add src/javaproject/domain/Category.java
git commit -m "[feat] Product 엔티티 및 Category enum 구현"

# 푸시
git push -u origin feature/domain-product-kms
```

---

### 2️⃣ 이지은 (lje) - Service 담당

```bash
# develop에서 시작
git checkout develop
git pull origin develop

# 인증 서비스 브랜치 생성
git checkout -b feature/service-auth-lje

# 작업 확인
echo "현재 브랜치: feature/service-auth-lje"
echo "작업 파일: src/javaproject/service/AuthenticationService.java"

# 작업 후 커밋
git add src/javaproject/service/AuthenticationService.java
git commit -m "[feat] AuthenticationService 구현 - 로그인/로그아웃 기능"

# 푸시
git push -u origin feature/service-auth-lje
```

**오후 작업 - 주문 서비스**
```bash
# develop 최신화
git checkout develop
git pull origin develop

# 주문 서비스 브랜치 생성
git checkout -b feature/service-order-lje

# 작업 후 커밋
git add src/javaproject/service/OrderService.java
git commit -m "[feat] OrderService 구현 - 주문 생성 및 취소 로직"

# 푸시
git push -u origin feature/service-order-lje
```

---

### 3️⃣ 박준호 (pjh) - Controller 담당

```bash
# develop에서 시작
git checkout develop
git pull origin develop

# 메인 컨트롤러 브랜치 생성
git checkout -b feature/controller-main-pjh

# 작업 확인
echo "현재 브랜치: feature/controller-main-pjh"
echo "작업 파일: src/javaproject/controller/MainController.java"

# 작업 후 커밋
git add src/javaproject/controller/MainController.java
git commit -m "[feat] MainController 구현 - 메인 메뉴 시스템"

# 푸시
git push -u origin feature/controller-main-pjh
```

**오후 작업 - 사용자 컨트롤러**
```bash
# develop 최신화
git checkout develop
git pull origin develop

# 사용자 컨트롤러 브랜치 생성
git checkout -b feature/controller-user-pjh

# 작업 후 커밋
git add src/javaproject/controller/UserController.java
git commit -m "[feat] UserController 구현 - 회원가입/로그인 메뉴"

# 푸시
git push -u origin feature/controller-user-pjh
```

---

### 4️⃣ 최서연 (csy) - Util & Exception 담당

```bash
# develop에서 시작
git checkout develop
git pull origin develop

# 파일 유틸리티 브랜치 생성
git checkout -b feature/util-file-csy

# 작업 확인
echo "현재 브랜치: feature/util-file-csy"
echo "작업 파일: src/javaproject/util/FileManager.java"

# 작업 후 커밋
git add src/javaproject/util/FileManager.java
git commit -m "[feat] FileManager 유틸리티 구현 - 파일 읽기/쓰기"

# 푸시
git push -u origin feature/util-file-csy
```

**오후 작업 - 예외 처리**
```bash
# develop 최신화
git checkout develop
git pull origin develop

# 예외 처리 브랜치 생성
git checkout -b feature/exception-auth-csy

# 작업 후 커밋
git add src/javaproject/exception/AuthenticationException.java
git add src/javaproject/exception/UnauthorizedException.java
git commit -m "[feat] 인증 관련 예외 클래스 구현"

# 푸시
git push -u origin feature/exception-auth-csy
```

---

## 📅 Day 2 (화요일) - Repository 계층 작업

### 김민수 (kms) - Repository 구현

```bash
# 아침 시작 - develop 최신화
git checkout develop
git pull origin develop

# UserRepository 브랜치 생성
git checkout -b feature/repository-user-kms

# 작업
echo "작업 중: UserRepository.java"
git add src/javaproject/repository/UserRepository.java
git commit -m "[feat] UserRepository 싱글톤 구현 - 파일 저장 로직 포함"

# 푸시
git push -u origin feature/repository-user-kms
```

---

## 📅 Day 3 (수요일) - 통합 작업

### 상황: 여러 브랜치 작업 완료 후 통합

```bash
# 김민수 - User 도메인 PR 생성 준비
git checkout feature/domain-user-kms
git pull origin develop  # 최신 develop 내용 가져오기
git push origin feature/domain-user-kms
# GitHub에서 PR 생성: feature/domain-user-kms → develop

# 이지은 - 인증 서비스 PR 생성 준비
git checkout feature/service-auth-lje
git pull origin develop
git push origin feature/service-auth-lje
# GitHub에서 PR 생성: feature/service-auth-lje → develop

# 박준호 - 메인 컨트롤러 PR 생성 준비
git checkout feature/controller-main-pjh
git pull origin develop
git push origin feature/controller-main-pjh
# GitHub에서 PR 생성: feature/controller-main-pjh → develop

# 최서연 - 파일 유틸리티 PR 생성 준비
git checkout feature/util-file-csy
git pull origin develop
git push origin feature/util-file-csy
# GitHub에서 PR 생성: feature/util-file-csy → develop
```

---

## 🔄 충돌 해결 시나리오

### 상황: 김민수와 이지은이 User 클래스를 동시 수정

**김민수의 브랜치에서:**
```bash
# feature/domain-user-kms 브랜치
git checkout feature/domain-user-kms

# develop의 최신 변경사항 병합 시도
git pull origin develop

# 충돌 발생!
# Auto-merging src/javaproject/domain/User.java
# CONFLICT (content): Merge conflict in src/javaproject/domain/User.java

# 충돌 확인
git status
# Unmerged paths:
#   both modified: src/javaproject/domain/User.java

# 파일 열어서 충돌 해결
# VS Code 또는 IntelliJ에서 충돌 마커 확인
# <<<<<<< HEAD
# 김민수의 코드
# =======
# 이지은의 코드
# >>>>>>> develop

# 충돌 해결 후
git add src/javaproject/domain/User.java
git commit -m "[fix] User 클래스 충돌 해결 - 필드 병합"
git push origin feature/domain-user-kms
```

---

## 📊 일일 브랜치 현황 확인

### 전체 팀원이 확인하는 명령어

```bash
# 모든 브랜치 확인 (로컬 + 원격)
git branch -a

# 출력 예시:
# * develop
#   main
#   remotes/origin/develop
#   remotes/origin/feature/controller-main-pjh
#   remotes/origin/feature/controller-user-pjh
#   remotes/origin/feature/domain-product-kms
#   remotes/origin/feature/domain-user-kms
#   remotes/origin/feature/exception-auth-csy
#   remotes/origin/feature/repository-user-kms
#   remotes/origin/feature/service-auth-lje
#   remotes/origin/feature/service-order-lje
#   remotes/origin/feature/util-file-csy
#   remotes/origin/main

# 원격 브랜치만 확인
git branch -r

# 최근 커밋 내역과 함께 브랜치 확인
git branch -av

# 브랜치별 최근 커밋 확인
git for-each-ref --sort=committerdate refs/heads/ --format='%(committerdate:short) %(refname:short) %(authorname)'
```

---

## 🎯 Day 5 (금요일) - 작업 완료 및 정리

### 모든 팀원: PR 병합 후 브랜치 정리

```bash
# PR이 병합된 후 각자 실행

# 김민수
git checkout develop
git pull origin develop
git branch -d feature/domain-user-kms        # 로컬 브랜치 삭제
git branch -d feature/domain-product-kms
git branch -d feature/repository-user-kms
git push origin --delete feature/domain-user-kms  # 원격 브랜치 삭제 (선택)

# 이지은
git checkout develop
git pull origin develop
git branch -d feature/service-auth-lje
git branch -d feature/service-order-lje

# 박준호
git checkout develop
git pull origin develop
git branch -d feature/controller-main-pjh
git branch -d feature/controller-user-pjh

# 최서연
git checkout develop
git pull origin develop
git branch -d feature/util-file-csy
git branch -d feature/exception-auth-csy
```

---

## 📋 일일 체크리스트 (팀원별)

### 아침 루틴 (09:00)
```bash
# 1. develop 브랜치로 이동
git checkout develop

# 2. 최신 변경사항 받기
git pull origin develop

# 3. 오늘 작업할 브랜치 생성
git checkout -b feature/[패키지]-[작업명]-[이니셜]

# 4. 브랜치 확인
git branch
```

### 점심 전 커밋 (12:00)
```bash
# 1. 작업 상태 확인
git status

# 2. 변경사항 확인
git diff

# 3. 스테이징 및 커밋
git add .
git commit -m "[타입] 작업 내용 - 상세 설명"

# 4. 푸시
git push -u origin feature/[브랜치명]
```

### 퇴근 전 정리 (18:00)
```bash
# 1. 마지막 커밋
git add .
git commit -m "[타입] 오늘 작업 완료 - 내용"

# 2. 푸시
git push origin feature/[브랜치명]

# 3. PR 생성 (GitHub 웹에서)
# 또는 다음 날 아침에 생성
```

---

## 💡 Pro Tips

### 1. 브랜치 이름 자동완성 활용
```bash
# Tab 키로 자동완성
git checkout fea[Tab]  # feature/로 자동완성
git checkout feature/dom[Tab]  # feature/domain-으로 자동완성
```

### 2. 별칭(Alias) 설정으로 시간 절약
```bash
# Git 별칭 설정
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status

# 사용 예
git co develop           # git checkout develop
git br -a               # git branch -a
git ci -m "[feat] 추가"  # git commit -m "[feat] 추가"
git st                  # git status
```

### 3. 브랜치 생성과 이동 한 번에
```bash
# -b 옵션으로 생성과 동시에 이동
git checkout -b feature/domain-cart-kms

# 위 명령어는 아래 두 명령어와 동일
# git branch feature/domain-cart-kms
# git checkout feature/domain-cart-kms
```

---

## ⚠️ 주의사항

### 하지 말아야 할 것들

```bash
# ❌ develop이나 main에 직접 커밋
git checkout develop
git add .
git commit -m "직접 커밋"  # 절대 금지!

# ❌ 다른 사람 브랜치에 직접 푸시
git push origin feature/domain-user-lje  # 이지은 브랜치에 김민수가 푸시 X

# ❌ force push 사용
git push -f origin feature/domain-user-kms  # 위험!

# ❌ 테스트 없이 PR 생성
# 항상 로컬에서 테스트 후 PR 생성
```

### 해야 할 것들

```bash
# ✅ 항상 자신의 브랜치에서 작업
git checkout feature/domain-user-kms

# ✅ 정기적으로 develop 브랜치 pull
git checkout develop
git pull origin develop

# ✅ 의미 있는 커밋 메시지
git commit -m "[feat] User 엔티티 구현 - 회원 정보 필드 추가"

# ✅ PR 생성 전 충돌 확인
git pull origin develop
# 충돌 해결 후 push
```

---

## 📈 주간 브랜치 통계 예시

### 금요일 기준 팀 현황

| 팀원 | 생성된 브랜치 수 | 병합된 PR | 진행 중 |
|------|-----------------|-----------|---------|
| 김민수 | 5 | 3 | 2 |
| 이지은 | 4 | 3 | 1 |
| 박준호 | 4 | 2 | 2 |
| 최서연 | 3 | 2 | 1 |

### 패키지별 진행률

| 패키지 | 완료 | 진행 중 | 계획 |
|--------|------|---------|------|
| domain | 4/5 | 1 | 0 |
| controller | 2/4 | 2 | 0 |
| service | 3/5 | 1 | 1 |
| repository | 2/4 | 1 | 1 |
| util | 3/3 | 0 | 0 |
| exception | 2/3 | 1 | 0 |

---

이제 4명의 팀원이 실제로 어떻게 브랜치를 생성하고 작업하는지 구체적인 명령어와 함께 확인할 수 있습니다! 😊