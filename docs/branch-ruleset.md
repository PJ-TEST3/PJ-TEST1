# 📌 **Git Branch Ruleset**

## **1. 브랜치 구조**

| 브랜치 유형 | 역할                            |
| ----------- | ------------------------------- |
| `main`      | 배포용 안정적인 브랜치          |
| `develop`   | 개발 중인 최신 기능을 통합      |
| `feature/*` | 새로운 기능 개발 브랜치         |
| `bugfix/*`  | 버그 수정 브랜치                |
| `hotfix/*`  | 긴급 수정 (배포 중인 버그 수정) |
| `release/*` | 배포 전 테스트 및 안정화 브랜치 |

---

## **2. 브랜치 명명 규칙 (Pattern)**

일관된 네이밍 규칙을 유지하기 위해 브랜치 이름을 패턴에 맞춰 사용합니다.

### **🔹 Feature Branch**

- 새로운 기능을 개발할 때 사용
- 패턴: `feature/{issue번호}-{설명}`
- 예시:
  ```
  feature/123-user-login
  feature/456-api-rate-limiting
  ```

### **🔹 Bugfix Branch**

- 개발 중 발견된 버그를 수정할 때 사용
- 패턴: `bugfix/{issue번호}-{설명}`
- 예시:
  ```
  bugfix/789-fix-login-error
  bugfix/101-correct-date-format
  ```

### **🔹 Hotfix Branch**

- 긴급 버그를 수정할 때 사용 (배포된 코드 수정)
- 패턴: `hotfix/{버전}-{설명}`
- 예시:
  ```
  hotfix/1.2.1-patch-security
  hotfix/2.0.3-fix-memory-leak
  ```

### **🔹 Release Branch**

- 배포 전 테스트 및 버전 정리
- 패턴: `release/{버전}`
- 예시:
  ```
  release/1.0.0
  release/2.1.0
  ```

### **🔹 Support Branch (선택적)**

- LTS(Long Term Support) 버전 유지보수
- 패턴: `support/{버전}`
- 예시:
  ```
  support/1.0
  support/2.3
  ```

---

## **3. Branch Workflow (작업 흐름)**

### **📌 Feature/Bugfix Workflow**

1. `develop` 브랜치에서 새로운 브랜치를 생성
   ```
   git checkout develop
   git checkout -b feature/123-user-login
   ```
2. 작업 완료 후 `develop`에 병합 (PR 제출)
   ```
   git checkout develop
   git merge feature/123-user-login
   git branch -d feature/123-user-login
   ```
3. 병합 후 브랜치는 삭제

### **📌 Release Workflow**

1. 배포 준비가 완료되면 `release` 브랜치를 생성
   ```
   git checkout develop
   git checkout -b release/1.0.0
   ```
2. QA 및 안정화 후 `main`으로 병합
   ```
   git checkout main
   git merge release/1.0.0
   git tag -a v1.0.0 -m "Release version 1.0.0"
   git push origin v1.0.0
   ```
3. `develop`에도 병합 후 브랜치 삭제
   ```
   git checkout develop
   git merge main
   git branch -d release/1.0.0
   ```

### **📌 Hotfix Workflow**

1. `main`에서 긴급 수정 브랜치 생성
   ```
   git checkout main
   git checkout -b hotfix/1.2.1-patch-security
   ```
2. 수정 완료 후 `main`과 `develop`에 병합
   ```
   git checkout main
   git merge hotfix/1.2.1-patch-security
   git checkout develop
   git merge hotfix/1.2.1-patch-security
   git branch -d hotfix/1.2.1-patch-security
   ```

---

## **4. Commit 규칙**

커밋 메시지는 다음 형식을 따릅니다.

```
[타입] #이슈번호 - 메시지
```

### **✅ Commit Type 예시**

| 타입       | 설명                             |
| ---------- | -------------------------------- |
| `feat`     | 새로운 기능 추가                 |
| `fix`      | 버그 수정                        |
| `docs`     | 문서 수정                        |
| `style`    | 코드 스타일 변경 (공백, 포맷 등) |
| `refactor` | 코드 리팩토링 (기능 변경 없음)   |
| `test`     | 테스트 코드 추가/수정            |
| `chore`    | 기타 변경 사항 (빌드, CI/CD 등)  |

**예시**

```
feat: #123 - Add user authentication API
fix: #456 - Fix login bug in mobile version
docs: #789 - Update README with installation guide
```

---

## **5. PR & Merge 규칙**

- PR 제목은 브랜치 네이밍과 유사한 패턴을 따름
  ```
  [Feature] #123 - User Login 기능 추가
  [Bugfix] #456 - 로그인 에러 수정
  ```
- 코드 리뷰 후 Squash & Merge 권장
- `main` 브랜치는 직접 push 금지, PR을 통한 병합만 허용
- Merge 전략:
  - `feature/*` → `develop`
  - `bugfix/*` → `develop`
  - `hotfix/*` → `main` + `develop`
  - `release/*` → `main` + `develop`

---

## **6. 보호된 브랜치 설정**

- `main`, `develop` 브랜치는 보호(Protected Branch) 설정
  - 직접 `push` 금지
  - 최소 1~2명의 코드 리뷰 필수
  - `squash merge` 권장
- `force push` 금지

---
