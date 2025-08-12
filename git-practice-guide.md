# 🎯 Git 브랜치 전략 실습 가이드 - 로컬에서 GitHub까지

> 실제로 따라하면서 익히는 Git 브랜치 전략 완벽 실습

---

## 📋 실습 준비사항

### 필요한 것들
- Git 설치 (git --version으로 확인)
- GitHub 계정
- 텍스트 에디터 (VS Code, IntelliJ 등)
- 터미널/명령 프롬프트

---

## 🚀 Step 1: GitHub Repository 생성 및 초기 설정

### 1-1. GitHub에서 Repository 생성

1. GitHub.com 로그인
2. 우측 상단 `+` → `New repository` 클릭
3. 설정:
   - Repository name: `java-shopping-practice`
   - Public 선택
   - Initialize this repository with: 체크 해제 (중요!)
4. `Create repository` 클릭

### 1-2. 로컬 프로젝트 초기화

```bash
# 1. 프로젝트 폴더 생성
mkdir java-shopping-practice
cd java-shopping-practice

# 2. Git 초기화
git init

# 3. 첫 파일 생성
echo "# Java Shopping Mall Practice" > README.md

# 4. 프로젝트 구조 생성
mkdir -p src/javaproject/{domain,controller,service,repository,util,exception}
mkdir -p data docs

# 5. .gitignore 파일 생성
cat > .gitignore << 'EOF'
*.class
*.iml
.idea/
.DS_Store
data/*.txt
out/
target/
EOF

# 6. 첫 커밋
git add .
git commit -m "Initial commit - 프로젝트 구조 설정"

# 7. GitHub 연결
git remote add origin https://github.com/[YOUR_USERNAME]/java-shopping-practice.git

# 8. main 브랜치로 이름 변경 (필요한 경우)
git branch -M main

# 9. GitHub에 푸시
git push -u origin main
```

---

## 🌿 Step 2: develop 브랜치 생성 및 설정

### 2-1. develop 브랜치 생성

```bash
# 1. 현재 브랜치 확인
git branch
# * main

# 2. develop 브랜치 생성 및 이동
git checkout -b develop

# 3. README 수정
echo "## Development Branch" >> README.md
echo "개발 통합 브랜치입니다." >> README.md

# 4. 커밋
git add README.md
git commit -m "[docs] develop 브랜치 설명 추가"

# 5. GitHub에 develop 브랜치 푸시
git push -u origin develop

# 6. 브랜치 확인
git branch -a
# * develop
#   main
#   remotes/origin/develop
#   remotes/origin/main
```

### 2-2. GitHub에서 기본 브랜치 변경 (선택사항)

1. GitHub Repository → Settings
2. Branches → Default branch
3. main → develop로 변경

---

## 🎨 Step 3: Feature 브랜치 실습

### 3-1. User 도메인 기능 개발

```bash
# 1. develop에서 시작
git checkout develop
git pull origin develop

# 2. feature 브랜치 생성
git checkout -b feature/domain-user-practice

# 3. User.java 파일 생성
cat > src/javaproject/domain/User.java << 'EOF'
package javaproject.domain;

public class User {
    private String id;
    private String password;
    private String name;
    private String email;
    
    // Constructor
    public User(String id, String password, String name, String email) {
        this.id = id;
        this.password = password;
        this.name = name;
        this.email = email;
    }
    
    // Getters and Setters
    public String getId() { return id; }
    public String getName() { return name; }
}
EOF

# 4. 변경사항 확인
git status
git diff

# 5. 커밋
git add src/javaproject/domain/User.java
git commit -m "[feat] User 도메인 클래스 생성 - 기본 필드 정의"

# 6. 추가 작업 - UserRole enum 생성
cat > src/javaproject/domain/UserRole.java << 'EOF'
package javaproject.domain;

public enum UserRole {
    USER("일반사용자"),
    ADMIN("관리자");
    
    private final String description;
    
    UserRole(String description) {
        this.description = description;
    }
}
EOF

# 7. 두 번째 커밋
git add src/javaproject/domain/UserRole.java
git commit -m "[feat] UserRole enum 추가 - 사용자 권한 구분"

# 8. GitHub에 푸시
git push -u origin feature/domain-user-practice

# 9. 푸시 결과 확인
git log --oneline --graph
```

### 3-2. Pull Request 생성 (GitHub 웹에서)

1. GitHub Repository 페이지 접속
2. "Compare & pull request" 버튼 클릭
3. 설정:
   - base: develop ← compare: feature/domain-user-practice
   - Title: `[Feature] User 도메인 구현`
   - Description: 작업 내용 설명
4. "Create pull request" 클릭

### 3-3. PR 병합 후 로컬 정리

```bash
# 1. develop으로 이동
git checkout develop

# 2. 원격 develop 내용 가져오기 (PR이 병합된 후)
git pull origin develop

# 3. 병합 확인
git log --oneline --graph -5

# 4. feature 브랜치 삭제
git branch -d feature/domain-user-practice

# 5. 원격 feature 브랜치 삭제
git push origin --delete feature/domain-user-practice

# 6. 정리 확인
git branch -a
```

---

## 🐛 Step 4: Bugfix 브랜치 실습

### 4-1. 버그 수정 시나리오

```bash
# 1. develop에서 시작
git checkout develop
git pull origin develop

# 2. bugfix 브랜치 생성
git checkout -b bugfix/issue-01-user-validation-practice

# 3. User.java 수정 (버그 수정)
cat >> src/javaproject/domain/User.java << 'EOF'

    // 버그 수정: 유효성 검증 메서드 추가
    public boolean isValid() {
        return id != null && !id.isEmpty() 
            && password != null && password.length() >= 6;
    }
EOF

# 4. 커밋
git add src/javaproject/domain/User.java
git commit -m "[fix] #01 User 유효성 검증 누락 수정"

# 5. 테스트 파일 추가 (선택사항)
mkdir -p src/test/javaproject/domain
cat > src/test/javaproject/domain/UserTest.java << 'EOF'
package test.javaproject.domain;

public class UserTest {
    public static void main(String[] args) {
        System.out.println("User validation test passed!");
    }
}
EOF

# 6. 테스트 파일 커밋
git add src/test/javaproject/domain/UserTest.java
git commit -m "[test] User 유효성 검증 테스트 추가"

# 7. GitHub에 푸시
git push -u origin bugfix/issue-01-user-validation-practice

# 8. PR 생성 및 병합 (GitHub 웹)
# PR 제목: [Bugfix] #01 User 유효성 검증 수정
```

---

## 🚨 Step 5: Hotfix 브랜치 실습

### 5-1. 긴급 수정 시나리오

```bash
# 1. main 브랜치에서 시작 (중요!)
git checkout main
git pull origin main

# 2. hotfix 브랜치 생성
git checkout -b hotfix/critical-security-fix

# 3. 긴급 보안 수정
cat > src/javaproject/util/SecurityUtil.java << 'EOF'
package javaproject.util;

public class SecurityUtil {
    // 긴급 보안 패치
    public static String sanitizeInput(String input) {
        if (input == null) return "";
        return input.replaceAll("[<>\"']", "");
    }
}
EOF

# 4. 커밋
git add src/javaproject/util/SecurityUtil.java
git commit -m "[hotfix] 긴급 보안 취약점 패치 - XSS 방지"

# 5. main에 직접 푸시 (또는 PR)
git push -u origin hotfix/critical-security-fix

# 6. main에 병합 (GitHub에서 PR 또는 로컬에서)
git checkout main
git merge --no-ff hotfix/critical-security-fix
git push origin main

# 7. develop에도 병합 (중요!)
git checkout develop
git pull origin develop
git merge --no-ff hotfix/critical-security-fix
git push origin develop

# 8. hotfix 브랜치 삭제
git branch -d hotfix/critical-security-fix
git push origin --delete hotfix/critical-security-fix
```

---

## 🔧 Step 6: Refactor 브랜치 실습

### 6-1. 코드 개선 작업

```bash
# 1. develop에서 시작
git checkout develop
git pull origin develop

# 2. refactor 브랜치 생성
git checkout -b refactor/user-service-structure

# 3. 리팩토링 작업
mkdir -p src/javaproject/service
cat > src/javaproject/service/UserService.java << 'EOF'
package javaproject.service;

import javaproject.domain.User;

public class UserService {
    // 리팩토링: 서비스 레이어 분리
    public User createUser(String id, String password, String name, String email) {
        // 비즈니스 로직
        validateUserInput(id, password, email);
        return new User(id, password, name, email);
    }
    
    private void validateUserInput(String id, String password, String email) {
        if (id == null || id.length() < 4) {
            throw new IllegalArgumentException("ID는 4자 이상이어야 합니다");
        }
        if (password == null || password.length() < 6) {
            throw new IllegalArgumentException("비밀번호는 6자 이상이어야 합니다");
        }
        if (!email.contains("@")) {
            throw new IllegalArgumentException("올바른 이메일 형식이 아닙니다");
        }
    }
}
EOF

# 4. 커밋
git add src/javaproject/service/UserService.java
git commit -m "[refactor] UserService 레이어 분리 - 비즈니스 로직 개선"

# 5. 푸시
git push -u origin refactor/user-service-structure
```

---

## 📦 Step 7: Release 브랜치 실습

### 7-1. 릴리즈 준비

```bash
# 1. develop에서 시작
git checkout develop
git pull origin develop

# 2. release 브랜치 생성
git checkout -b release/v1.0.0

# 3. 버전 파일 생성
echo "version=1.0.0" > version.properties
echo "release_date=2025-01-17" >> version.properties

# 4. README 업데이트
cat >> README.md << 'EOF'

## Version History
- v1.0.0 (2025-01-17): 첫 번째 정식 릴리즈
  - User 도메인 구현
  - 기본 서비스 레이어 구현
  - 보안 패치 적용
EOF

# 5. 커밋
git add version.properties README.md
git commit -m "[release] v1.0.0 릴리즈 준비"

# 6. 푸시
git push -u origin release/v1.0.0

# 7. main에 병합 (PR 또는 직접)
git checkout main
git pull origin main
git merge --no-ff release/v1.0.0
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin main --tags

# 8. develop에도 병합
git checkout develop
git merge --no-ff release/v1.0.0
git push origin develop

# 9. release 브랜치 삭제
git branch -d release/v1.0.0
git push origin --delete release/v1.0.0
```

---

## 📝 Step 8: Docs 브랜치 실습

### 8-1. 문서 작업

```bash
# 1. develop에서 시작
git checkout develop
git pull origin develop

# 2. docs 브랜치 생성
git checkout -b docs/api-documentation

# 3. API 문서 생성
cat > docs/API.md << 'EOF'
# API Documentation

## User API

### Create User
- Endpoint: `/api/users`
- Method: POST
- Body: 
  ```json
  {
    "id": "user123",
    "password": "pass123",
    "name": "홍길동",
    "email": "hong@example.com"
  }
  ```

### Get User
- Endpoint: `/api/users/{id}`
- Method: GET
EOF

# 4. 커밋
git add docs/API.md
git commit -m "[docs] API 문서 작성 - User API 엔드포인트"

# 5. 푸시 및 PR
git push -u origin docs/api-documentation
```

---

## 🧪 Step 9: Test 브랜치 실습

### 9-1. 실험적 기능 테스트

```bash
# 1. develop에서 시작
git checkout develop
git pull origin develop

# 2. test 브랜치 생성
git checkout -b test/new-feature-experiment

# 3. 실험적 코드 작성
cat > src/javaproject/experimental/CacheManager.java << 'EOF'
package javaproject.experimental;

// 실험적 기능: 캐시 관리자
public class CacheManager {
    // 이 코드는 테스트 용도입니다
    public void testCache() {
        System.out.println("Cache test");
    }
}
EOF

# 4. 커밋
git add src/javaproject/experimental/
git commit -m "[test] 캐시 매니저 실험 - 성능 테스트용"

# 5. 푸시 (PR 생성하지 않음)
git push -u origin test/new-feature-experiment

# 6. 테스트 완료 후 브랜치 삭제
git checkout develop
git branch -D test/new-feature-experiment  # -D 강제 삭제
git push origin --delete test/new-feature-experiment
```

---

## 📊 Step 10: 전체 브랜치 현황 확인

### 10-1. 다양한 확인 명령어

```bash
# 1. 로컬 브랜치 목록
git branch
  develop
* main

# 2. 원격 브랜치 포함 전체 목록
git branch -a
  develop
* main
  remotes/origin/develop
  remotes/origin/main

# 3. 브랜치별 최근 커밋 확인
git branch -v
  develop 3a4b5c6 [docs] develop 브랜치 설명 추가
* main    1a2b3c4 [release] v1.0.0 릴리즈 준비

# 4. 브랜치 그래프 보기
git log --graph --pretty=oneline --abbrev-commit --all

# 5. 브랜치 생성자와 날짜 확인
git for-each-ref --format='%(committerdate:short) %(authorname) %(refname:short)' --sort=-committerdate refs/heads/

# 6. 원격 저장소 정보
git remote -v
origin  https://github.com/username/java-shopping-practice.git (fetch)
origin  https://github.com/username/java-shopping-practice.git (push)

# 7. 태그 목록
git tag -l
v1.0.0

# 8. 현재 상태 종합 확인
git status
On branch main
Your branch is up to date with 'origin/main'.
nothing to commit, working tree clean
```

---

## 🔄 Step 11: 충돌 해결 실습

### 11-1. 의도적 충돌 생성 및 해결

```bash
# 1. feature 브랜치 A 생성
git checkout develop
git checkout -b feature/update-user-a

# User.java 수정
echo "    // Feature A 추가" >> src/javaproject/domain/User.java
git add .
git commit -m "[feat] Feature A 추가"
git push -u origin feature/update-user-a

# 2. 다른 feature 브랜치 B 생성
git checkout develop
git checkout -b feature/update-user-b

# 같은 파일 같은 위치 수정
echo "    // Feature B 추가" >> src/javaproject/domain/User.java
git add .
git commit -m "[feat] Feature B 추가"
git push -u origin feature/update-user-b

# 3. Feature A를 develop에 병합 (GitHub PR 또는 로컬)
git checkout develop
git merge feature/update-user-a
git push origin develop

# 4. Feature B에서 충돌 발생
git checkout feature/update-user-b
git merge develop
# CONFLICT 발생!

# 5. 충돌 확인
git status
# both modified: src/javaproject/domain/User.java

# 6. 파일 열어서 충돌 해결
# <<<<<<< HEAD
#     // Feature B 추가
# =======
#     // Feature A 추가
# >>>>>>> develop

# 7. 수동으로 병합 (둘 다 유지)
# 파일 수정 후:
#     // Feature A 추가
#     // Feature B 추가

# 8. 충돌 해결 완료
git add src/javaproject/domain/User.java
git commit -m "[fix] merge conflict 해결 - Feature A와 B 통합"
git push origin feature/update-user-b
```

---

## 💾 Step 12: 백업 및 복구

### 12-1. 실수 복구 방법

```bash
# 1. 커밋 취소 (아직 push 안 한 경우)
git reset --soft HEAD~1  # 마지막 커밋 취소, 변경사항 유지
git reset --hard HEAD~1  # 마지막 커밋 취소, 변경사항 삭제

# 2. 이미 push한 커밋 되돌리기
git revert HEAD  # 새로운 커밋으로 이전 커밋 취소
git push origin [branch-name]

# 3. 삭제한 브랜치 복구
git reflog  # 모든 참조 기록 확인
git checkout -b [branch-name] [commit-hash]

# 4. Stash 활용 (임시 저장)
git stash  # 현재 변경사항 임시 저장
git stash list  # 저장 목록 확인
git stash pop  # 최근 저장 복원
git stash drop  # 최근 저장 삭제

# 5. 특정 파일만 이전 상태로
git checkout HEAD~1 -- src/javaproject/domain/User.java
```

---

## 🎯 실습 체크리스트

### 완료 확인
- [ ] GitHub Repository 생성
- [ ] 로컬 Git 초기화 및 연결
- [ ] main 브랜치 생성 및 푸시
- [ ] develop 브랜치 생성 및 푸시
- [ ] feature 브랜치 생성, 작업, PR, 병합
- [ ] bugfix 브랜치 실습
- [ ] hotfix 브랜치 실습
- [ ] refactor 브랜치 실습
- [ ] release 브랜치 및 태그 생성
- [ ] docs 브랜치 실습
- [ ] test 브랜치 실습
- [ ] 충돌 해결 경험
- [ ] 브랜치 삭제 및 정리

---

## 📚 추가 학습 자료

### Git 설정 최적화

```bash
# 사용자 정보 설정
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# 유용한 별칭 설정
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

# 에디터 설정
git config --global core.editor "code --wait"  # VS Code

# 줄바꿈 설정
git config --global core.autocrlf true  # Windows
git config --global core.autocrlf input  # Mac/Linux
```

### 유용한 Git 명령어 조합

```bash
# 최근 5개 커밋 한 줄로 보기
git log --oneline -5

# 특정 파일의 변경 이력
git log --follow src/javaproject/domain/User.java

# 특정 작성자의 커밋만 보기
git log --author="Your Name"

# 오늘 작업한 커밋 보기
git log --since="6am"

# 브랜치 간 차이 확인
git diff develop..feature/domain-user

# 아직 push하지 않은 커밋 확인
git log origin/develop..develop
```

---

## ⚠️ 주의사항 및 트러블슈팅

### 자주 발생하는 문제와 해결

1. **Permission denied (publickey)**
   ```bash
   # SSH 키 생성
   ssh-keygen -t ed25519 -C "your.email@example.com"
   # GitHub에 SSH 키 추가
   ```

2. **Updates were rejected**
   ```bash
   # 원격 변경사항 먼저 가져오기
   git pull origin [branch-name]
   # 또는 강제 푸시 (주의!)
   git push -f origin [branch-name]
   ```

3. **Detached HEAD**
   ```bash
   # 브랜치로 돌아가기
   git checkout develop
   # 또는 새 브랜치 생성
   git checkout -b new-branch
   ```

---

## 🎉 실습 완료!

축하합니다! 이제 Git 브랜치 전략을 실제로 실습해보았습니다.
이 가이드를 반복 연습하면 실무에서도 자신있게 Git을 사용할 수 있습니다!

**다음 단계:**
1. 실제 Java 코드로 프로젝트 구현
2. 팀원과 함께 협업 연습
3. Issue 트래킹 연동
4. CI/CD 파이프라인 구축

---

**Last Updated**: 2025년 1월
**Practice Repository**: github.com/[your-username]/java-shopping-practice