# uv 명령어 레퍼런스

> 기준 버전: uv 0.13.0
> 전체 도움말은 `uv help` 또는 `uv <명령어> --help` 로 확인할 수 있습니다.

uv는 Rust로 만든 매우 빠른 Python 패키지/프로젝트 관리 도구입니다.
`pip`, `venv`, `pyenv`, `pipx`, `poetry` 등이 하던 일을 하나로 처리합니다.

---

## 1. 설치 및 uv 자체 관리

```powershell
# 설치 (Windows PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

# 설치 (macOS / Linux)
curl -LsSf https://astral.sh/uv/install.sh | sh
```

| 명령어 | 설명 |
|---|---|
| `uv --version` | 설치된 uv 버전 확인 |
| `uv self update` | uv를 최신 버전으로 업데이트 |
| `uv help` | 전체 명령어 목록 보기 |
| `uv help <명령어>` | 특정 명령어의 자세한 문서 보기 |

---

## 2. 프로젝트 관리 (가장 많이 쓰는 명령)

| 명령어 | 설명 |
|---|---|
| `uv init` | 현재 폴더에 새 프로젝트 생성 (`pyproject.toml`, `.python-version` 등) |
| `uv init <이름>` | `<이름>` 폴더를 만들고 그 안에 프로젝트 생성 |
| `uv init --lib <이름>` | 라이브러리(패키지) 형태로 프로젝트 생성 |
| `uv add <패키지>` | 의존성 추가 + 설치 + `pyproject.toml`/`uv.lock` 갱신 |
| `uv add "<패키지>==1.2.3"` | 특정 버전으로 추가 |
| `uv add --dev <패키지>` | 개발용 의존성으로 추가 (pytest, ruff 등) |
| `uv add -r requirements.txt` | requirements.txt 내용을 한 번에 추가 |
| `uv remove <패키지>` | 의존성 제거 |
| `uv sync` | `uv.lock` 기준으로 `.venv` 를 맞춤 (없으면 생성) |
| `uv lock` | `uv.lock` 파일만 갱신 (설치는 하지 않음) |
| `uv lock --upgrade` | 모든 패키지를 허용 범위 내 최신 버전으로 잠금 갱신 |
| `uv lock --upgrade-package <패키지>` | 특정 패키지만 업그레이드 |
| `uv tree` | 의존성 트리 출력 |
| `uv export --format requirements.txt -o requirements.txt` | lock 파일을 requirements.txt 로 내보내기 |
| `uv version` | 프로젝트 버전 확인 |
| `uv version --bump patch` | 버전 올리기 (`major` / `minor` / `patch`) |

### 실행

| 명령어 | 설명 |
|---|---|
| `uv run <파일>.py` | 프로젝트 가상환경에서 스크립트 실행 (활성화 불필요) |
| `uv run python` | 가상환경의 Python 인터프리터 실행 |
| `uv run <명령>` | 가상환경 안의 명령 실행 (예: `uv run pytest`) |
| `uv run --with <패키지> <명령>` | 일회성으로 패키지를 추가해서 실행 |

> **팁:** `uv run` 을 쓰면 `.venv\Scripts\activate` 를 하지 않아도 됩니다.
> PowerShell 실행 정책 오류를 피하는 가장 쉬운 방법이기도 합니다.

---

## 3. Python 버전 관리

| 명령어 | 설명 |
|---|---|
| `uv python list` | 설치되어 있거나 설치 가능한 Python 목록 |
| `uv python install 3.12` | Python 3.12 설치 |
| `uv python install 3.11 3.12 3.13` | 여러 버전 한 번에 설치 |
| `uv python upgrade` | 설치된 Python을 최신 패치 버전으로 업그레이드 |
| `uv python uninstall 3.11` | Python 3.11 삭제 |
| `uv python pin 3.12` | 현재 프로젝트의 Python 버전 고정 (`.python-version` 생성) |
| `uv python find` | 현재 사용될 Python 경로 확인 |
| `uv python dir` | uv가 Python을 설치하는 폴더 확인 |

---

## 4. 가상환경

| 명령어 | 설명 |
|---|---|
| `uv venv` | 현재 폴더에 `.venv` 생성 |
| `uv venv <이름>` | 지정한 이름으로 가상환경 생성 |
| `uv venv --python 3.12` | 특정 Python 버전으로 가상환경 생성 |

가상환경 활성화 (직접 활성화가 필요할 때만):

```powershell
# Windows PowerShell
.venv\Scripts\activate

# Windows CMD
.venv\Scripts\activate.bat

# macOS / Linux
source .venv/bin/activate

# 비활성화
deactivate
```

---

## 5. 도구(CLI) 실행 및 설치 — pipx 대체

| 명령어 | 설명 |
|---|---|
| `uvx <도구>` | 도구를 임시 환경에서 바로 실행 (`uv tool run` 의 줄임) |
| `uvx <도구>@<버전>` | 특정 버전으로 실행 (예: `uvx ruff@0.6.0 check`) |
| `uv tool install <도구>` | 도구를 전역으로 설치 (PATH에서 바로 사용) |
| `uv tool list` | 설치된 도구 목록 |
| `uv tool upgrade <도구>` | 도구 업그레이드 (`--all` 로 전체) |
| `uv tool uninstall <도구>` | 도구 삭제 |
| `uv tool update-shell` | 도구 폴더를 PATH에 추가 |

---

## 6. pip 호환 인터페이스

기존 `pip` 사용법과 거의 같습니다. 앞에 `uv` 만 붙이면 됩니다.

| 명령어 | 설명 |
|---|---|
| `uv pip install <패키지>` | 패키지 설치 |
| `uv pip install -r requirements.txt` | requirements 파일로 설치 |
| `uv pip uninstall <패키지>` | 패키지 삭제 |
| `uv pip list` | 설치된 패키지 목록 (표 형식) |
| `uv pip freeze` | 설치된 패키지 목록 (requirements 형식) |
| `uv pip show <패키지>` | 패키지 상세 정보 |
| `uv pip tree` | 설치된 패키지 의존성 트리 |
| `uv pip check` | 설치된 패키지 간 호환성 검사 |
| `uv pip compile requirements.in -o requirements.txt` | 버전을 고정한 requirements.txt 생성 |
| `uv pip sync requirements.txt` | 환경을 requirements.txt 와 정확히 일치시킴 (나머지 삭제) |

> `uv add` 와 `uv pip install` 의 차이:
> `uv add` 는 `pyproject.toml` 에 기록하고, `uv pip install` 은 기록하지 않고 설치만 합니다.
> 프로젝트에서는 `uv add` 를 쓰는 것을 권장합니다.

---

## 7. 코드 품질 · 보안

| 명령어 | 설명 |
|---|---|
| `uv format` | 프로젝트 Python 코드 포맷팅 |
| `uv check` | 프로젝트 검사 실행 |
| `uv audit` | 의존성 보안 취약점 검사 |

---

## 8. 빌드 · 배포

| 명령어 | 설명 |
|---|---|
| `uv build` | 소스 배포판(sdist)과 wheel 빌드 (`dist/` 폴더) |
| `uv publish` | 빌드한 패키지를 PyPI 등에 업로드 |

---

## 9. 캐시 관리

| 명령어 | 설명 |
|---|---|
| `uv cache dir` | 캐시 폴더 위치 확인 |
| `uv cache clean` | 캐시 전체 삭제 |
| `uv cache clean <패키지>` | 특정 패키지 캐시만 삭제 |
| `uv cache prune` | 사용하지 않는 캐시 정리 |

---

## 10. 자주 쓰는 작업 흐름

### 새 프로젝트 시작

```powershell
uv init my-project
cd my-project
uv add requests
uv run main.py
```

### 다른 사람의 프로젝트 받아서 실행

```powershell
git clone <저장소 주소>
cd <프로젝트>
uv sync          # uv.lock 기준으로 환경 구성
uv run main.py
```

### 기존 requirements.txt 프로젝트를 uv로 옮기기

```powershell
uv init
uv add -r requirements.txt
```

---

## 11. 주요 파일

| 파일 | 역할 |
|---|---|
| `pyproject.toml` | 프로젝트 정보와 의존성 목록 (직접 편집 가능) |
| `uv.lock` | 정확한 설치 버전이 고정된 잠금 파일 (직접 편집 X, git에 커밋 O) |
| `.python-version` | 프로젝트에서 사용할 Python 버전 |
| `.venv/` | 가상환경 폴더 (git에 커밋 X) |
