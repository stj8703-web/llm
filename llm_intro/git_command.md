# Git 명령어 레퍼런스

> 기준 버전: git 2.56.0 (Windows)
> 전체 도움말은 `git help` 또는 `git <명령어> --help` 로 확인할 수 있습니다.

Git은 파일의 변경 이력을 저장하고 관리하는 버전 관리 도구입니다.
언제 무엇을 바꿨는지 기록하고, 이전 상태로 되돌리거나, 여러 사람이 함께 작업할 수 있게 해 줍니다.

---

## 0. 기본 개념

| 용어 | 설명 |
|---|---|
| 저장소 (repository) | Git이 관리하는 프로젝트 폴더 (`.git` 폴더에 이력이 저장됨) |
| 작업 디렉터리 (working directory) | 지금 편집 중인 실제 파일들 |
| 스테이징 영역 (staging area) | 다음 커밋에 넣을 변경 사항을 모아 두는 곳 |
| 커밋 (commit) | 변경 사항을 하나의 기록으로 저장한 것 |
| 브랜치 (branch) | 독립적인 작업 흐름 (기본 이름: `main` 또는 `master`) |
| 원격 저장소 (remote) | GitHub 등 다른 곳에 있는 저장소 (기본 이름: `origin`) |

변경 사항이 흘러가는 순서:

```
작업 디렉터리 --(git add)--> 스테이징 영역 --(git commit)--> 로컬 저장소 --(git push)--> 원격 저장소
```

---

## 1. 처음 설정 (한 번만)

| 명령어 | 설명 |
|---|---|
| `git config --global user.name "이름"` | 커밋에 기록될 이름 설정 |
| `git config --global user.email "메일"` | 커밋에 기록될 이메일 설정 |
| `git config --global init.defaultBranch main` | 새 저장소의 기본 브랜치 이름을 `main` 으로 |
| `git config --global core.autocrlf true` | (Windows) 줄바꿈 문자 자동 변환 |
| `git config --global core.quotepath false` | 한글 파일 이름이 깨지지 않게 표시 |
| `git config --list` | 현재 설정 전체 보기 |

---

## 2. 저장소 만들기 / 가져오기

| 명령어 | 설명 |
|---|---|
| `git init` | 현재 폴더를 Git 저장소로 만듦 |
| `git clone <주소>` | 원격 저장소를 내 컴퓨터로 복제 |
| `git clone <주소> <폴더명>` | 지정한 폴더 이름으로 복제 |

---

## 3. 기본 작업 (가장 많이 쓰는 명령)

| 명령어 | 설명 |
|---|---|
| `git status` | 현재 상태 확인 (변경된 파일, 스테이징 여부) |
| `git add <파일>` | 파일을 스테이징 영역에 추가 |
| `git add .` | 현재 폴더의 모든 변경 사항 추가 |
| `git add -p` | 변경 내용을 조각별로 확인하며 골라서 추가 |
| `git commit -m "메시지"` | 스테이징된 내용을 커밋 |
| `git commit -am "메시지"` | 추적 중인 파일의 변경을 add + commit 한 번에 (새 파일은 제외) |
| `git commit --amend` | 마지막 커밋 수정 (메시지 변경, 빠뜨린 파일 추가) |
| `git rm <파일>` | 파일 삭제 + 삭제 사실을 스테이징 |
| `git rm --cached <파일>` | 파일은 남기고 Git 추적만 중단 |
| `git mv <이전> <새이름>` | 파일 이름 변경 / 이동 |

---

## 4. 변경 내용 · 이력 보기

| 명령어 | 설명 |
|---|---|
| `git diff` | 아직 스테이징하지 않은 변경 내용 |
| `git diff --staged` | 스테이징된(커밋 예정인) 변경 내용 |
| `git diff <브랜치1> <브랜치2>` | 두 브랜치 비교 |
| `git log` | 커밋 이력 |
| `git log --oneline` | 커밋 이력을 한 줄씩 간단히 |
| `git log --oneline --graph --all` | 모든 브랜치 이력을 그래프로 |
| `git log -p <파일>` | 특정 파일의 변경 이력과 내용 |
| `git show <커밋>` | 특정 커밋의 상세 내용 |
| `git blame <파일>` | 각 줄을 누가, 언제 마지막으로 바꿨는지 |

> `git log` 화면에서 나가려면 `q` 를 누르세요.

---

## 5. 브랜치

| 명령어 | 설명 |
|---|---|
| `git branch` | 로컬 브랜치 목록 (`*` 가 현재 브랜치) |
| `git branch -a` | 원격 브랜치까지 모두 보기 |
| `git branch <이름>` | 새 브랜치 생성 (이동은 안 함) |
| `git switch <이름>` | 브랜치 이동 |
| `git switch -c <이름>` | 새 브랜치를 만들고 바로 이동 |
| `git branch -m <새이름>` | 현재 브랜치 이름 변경 |
| `git branch -d <이름>` | 브랜치 삭제 (병합된 경우만) |
| `git branch -D <이름>` | 브랜치 강제 삭제 |
| `git merge <브랜치>` | 지정한 브랜치를 현재 브랜치에 병합 |
| `git rebase <브랜치>` | 현재 브랜치의 커밋을 지정한 브랜치 위로 옮김 |

> 예전 문서에서 보이는 `git checkout <브랜치>` 도 같은 일을 하지만,
> 요즘은 역할이 분명한 `git switch` (브랜치 이동) / `git restore` (파일 되돌리기) 를 권장합니다.

---

## 6. 원격 저장소 (GitHub 등)

| 명령어 | 설명 |
|---|---|
| `git remote -v` | 연결된 원격 저장소 목록 |
| `git remote add origin <주소>` | 원격 저장소 연결 |
| `git push` | 로컬 커밋을 원격에 업로드 |
| `git push -u origin <브랜치>` | 처음 push 할 때 (이후에는 `git push` 만으로 됨) |
| `git pull` | 원격의 변경을 가져와서 병합 (fetch + merge) |
| `git pull --rebase` | 원격의 변경을 가져와서 내 커밋을 그 위로 재배치 |
| `git fetch` | 원격의 변경을 가져오기만 함 (병합 안 함) |

---

## 7. 되돌리기

| 명령어 | 설명 | 위험도 |
|---|---|---|
| `git restore <파일>` | 수정한 파일을 마지막 커밋 상태로 되돌림 | ⚠️ 수정 내용 사라짐 |
| `git restore --staged <파일>` | 스테이징 취소 (수정 내용은 유지) | 안전 |
| `git reset --soft HEAD~1` | 마지막 커밋 취소, 변경은 스테이징 상태로 유지 | 안전 |
| `git reset HEAD~1` | 마지막 커밋 취소, 변경은 작업 디렉터리에 유지 | 안전 |
| `git reset --hard HEAD~1` | 마지막 커밋과 변경 내용 모두 삭제 | ⚠️ 복구 어려움 |
| `git revert <커밋>` | 해당 커밋을 되돌리는 새 커밋 생성 | 안전 (이미 push 한 경우 권장) |
| `git clean -n` | 추적하지 않는 파일 중 지울 대상 미리 보기 | 안전 |
| `git clean -fd` | 추적하지 않는 파일·폴더 삭제 | ⚠️ 복구 불가 |

> **이미 push 한 커밋은 `reset` 대신 `revert` 를 쓰세요.**
> `reset` 으로 이력을 바꾸면 다른 사람의 저장소와 어긋납니다.

> **실수로 날렸을 때:** `git reflog` 로 최근 HEAD 이동 기록을 보고
> `git reset --hard <기록의 커밋>` 으로 되살릴 수 있습니다. (커밋했던 내용만 해당)

---

## 8. 임시 저장 (stash)

작업 중인 내용을 커밋하지 않고 잠깐 치워 둘 때 씁니다.

| 명령어 | 설명 |
|---|---|
| `git stash` | 현재 변경 사항을 임시 저장하고 작업 디렉터리를 깨끗하게 |
| `git stash -u` | 추적하지 않는 새 파일까지 포함해서 임시 저장 |
| `git stash list` | 임시 저장 목록 |
| `git stash pop` | 가장 최근 임시 저장을 꺼내서 적용 (목록에서 삭제) |
| `git stash apply` | 꺼내서 적용 (목록에는 남김) |
| `git stash drop` | 가장 최근 임시 저장 삭제 |

---

## 9. 태그

| 명령어 | 설명 |
|---|---|
| `git tag` | 태그 목록 |
| `git tag v1.0.0` | 현재 커밋에 태그 추가 |
| `git tag -a v1.0.0 -m "메시지"` | 설명이 있는 태그 추가 |
| `git push origin v1.0.0` | 태그를 원격에 업로드 |
| `git push --tags` | 모든 태그 업로드 |

---

## 10. .gitignore

Git이 추적하지 않을 파일을 적어 두는 파일입니다. 프로젝트 최상위에 둡니다.

```gitignore
# Python
.venv/
__pycache__/
*.pyc

# 환경 변수 / 비밀 정보
.env

# 에디터 / OS
.vscode/
.DS_Store
Thumbs.db
```

> 이미 커밋된 파일은 `.gitignore` 에 추가해도 계속 추적됩니다.
> `git rm --cached <파일>` 로 추적을 끊은 뒤 커밋하세요.

---

## 11. 자주 쓰는 작업 흐름

### 새 프로젝트를 GitHub에 올리기

```powershell
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/<사용자>/<저장소>.git
git push -u origin main
```

### 매일 하는 작업

```powershell
git pull                      # 최신 내용 받기
# ... 코드 수정 ...
git status                    # 무엇이 바뀌었는지 확인
git add .
git commit -m "기능 추가: ..."
git push
```

### 브랜치에서 기능 개발 후 합치기

```powershell
git switch -c feature/login   # 새 브랜치에서 작업 시작
# ... 코드 수정 & 커밋 ...
git switch main
git pull
git merge feature/login
git push
git branch -d feature/login   # 다 쓴 브랜치 정리
```

---

## 12. 좋은 커밋 메시지

- 첫 줄은 50자 안팎으로, 무엇을 했는지 요약
- 필요하면 한 줄 비우고 왜 바꿨는지 자세히
- 한 커밋에는 하나의 목적만

```
로그인 실패 시 오류 메시지 표시

비밀번호가 틀려도 아무 반응이 없어서 사용자가 혼란스러워함.
실패 원인에 따라 다른 메시지를 보여주도록 수정.
```
