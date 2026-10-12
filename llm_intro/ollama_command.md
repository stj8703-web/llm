# Ollama 명령어 레퍼런스

> 이 문서는 Ollama 기본 명령 기준으로 작성되었습니다.
> 설치한 버전의 전체 명령은 `ollama --help` 또는 `ollama <명령어> --help` 로 확인하세요.

Ollama는 LLM을 **내 컴퓨터에서 직접 실행**하게 해 주는 도구입니다.
API 키나 인터넷 없이(모델 다운로드 후) 무료로 쓸 수 있고, 데이터가 외부로 나가지 않습니다.

---

## 0. 기본 개념

| 용어 | 설명 |
|---|---|
| 모델 (model) | 실행할 LLM (예: `llama3.2`, `gemma3`, `qwen3`) |
| 태그 (tag) | 모델의 크기/버전 (예: `llama3.2:1b`, `llama3.2:3b`). 생략하면 `latest` |
| 서버 | 백그라운드에서 모델을 돌리는 프로그램. 기본 주소 `http://localhost:11434` |
| Modelfile | 모델에 시스템 프롬프트·설정을 입혀 나만의 모델을 만드는 설정 파일 |

> 모델 목록은 https://ollama.com/library 에서 찾을 수 있습니다.

---

## 1. 설치

```powershell
# Windows (winget)
winget install Ollama.Ollama

# 또는 https://ollama.com/download 에서 설치 파일 다운로드
```

```bash
# macOS / Linux
curl -fsSL https://ollama.com/install.sh | sh
```

| 명령어 | 설명 |
|---|---|
| `ollama --version` | 설치된 버전 확인 |
| `ollama --help` | 전체 명령어 목록 |

> Windows에서는 설치 후 Ollama가 트레이 아이콘으로 자동 실행되어 서버가 켜져 있습니다.

---

## 2. 모델 받기 · 실행 (가장 많이 쓰는 명령)

| 명령어 | 설명 |
|---|---|
| `ollama run <모델>` | 모델 실행 후 대화 시작 (없으면 자동 다운로드) |
| `ollama run <모델> "질문"` | 대화창 없이 한 번만 질문하고 답 받기 |
| `ollama pull <모델>` | 모델 다운로드만 (실행 안 함) |
| `ollama pull <모델>` (다시) | 모델을 최신 버전으로 업데이트 |

```powershell
ollama run llama3.2          # 기본 크기로 실행
ollama run llama3.2:1b       # 작은 버전(가볍고 빠름)으로 실행
ollama run gemma3 "한국의 수도는?"
```

### 대화창 안에서 쓰는 명령

| 명령 | 설명 |
|---|---|
| `/bye` | 대화 종료 (또는 `Ctrl + d`) |
| `/?` | 도움말 |
| `/clear` | 대화 기록 초기화 |
| `/show info` | 현재 모델 정보 |
| `/set system "..."` | 시스템 프롬프트 설정 |
| `"""` | 여러 줄 입력 시작/끝 |

---

## 3. 모델 관리

| 명령어 | 설명 |
|---|---|
| `ollama list` | 내려받은 모델 목록 (`ollama ls` 도 동일) |
| `ollama ps` | 지금 메모리에 올라가 실행 중인 모델 |
| `ollama stop <모델>` | 실행 중인 모델을 메모리에서 내림 |
| `ollama show <모델>` | 모델 정보 (크기, 파라미터, 컨텍스트 길이 등) |
| `ollama show <모델> --modelfile` | 모델의 Modelfile 내용 보기 |
| `ollama cp <원본> <새이름>` | 모델 복사 |
| `ollama rm <모델>` | 모델 삭제 (디스크 공간 확보) |

> 모델은 수 GB씩 차지합니다. 안 쓰는 모델은 `ollama rm` 으로 지우세요.

---

## 4. 서버

| 명령어 | 설명 |
|---|---|
| `ollama serve` | 서버를 직접 실행 (앱이 이미 켜져 있으면 불필요) |

서버가 켜져 있는지 확인:

```powershell
curl http://localhost:11434
# "Ollama is running" 이 나오면 정상
```

---

## 5. 나만의 모델 만들기 (Modelfile)

`Modelfile` 이라는 이름의 파일을 만듭니다.

```dockerfile
FROM llama3.2

# 답변의 창의성 (0 = 일정함, 1 이상 = 다양함)
PARAMETER temperature 0.3

SYSTEM """
너는 Python 입문자를 돕는 친절한 선생님이야.
항상 한국어로, 예제 코드와 함께 쉽게 설명해.
"""
```

| 명령어 | 설명 |
|---|---|
| `ollama create <새모델> -f Modelfile` | Modelfile로 새 모델 생성 |
| `ollama run <새모델>` | 만든 모델 실행 |

---

## 6. API로 사용하기

Ollama 서버는 `http://localhost:11434` 에서 REST API를 제공합니다.

### curl (PowerShell에서는 `curl.exe` 로 실행)

```powershell
curl.exe http://localhost:11434/api/generate -d '{\"model\": \"llama3.2\", \"prompt\": \"안녕?\", \"stream\": false}'
```

### Python (`ollama` 라이브러리)

```powershell
uv add ollama
```

```python
import ollama

response = ollama.chat(
    model="llama3.2",
    messages=[{"role": "user", "content": "LLM이 뭐야?"}],
)
print(response["message"]["content"])
```

### OpenAI 호환 API

OpenAI 라이브러리로 만든 코드를 주소만 바꿔서 그대로 쓸 수 있습니다.

```python
from openai import OpenAI

client = OpenAI(base_url="http://localhost:11434/v1", api_key="ollama")  # api_key는 아무 값이나

response = client.chat.completions.create(
    model="llama3.2",
    messages=[{"role": "user", "content": "LLM이 뭐야?"}],
)
print(response.choices[0].message.content)
```

---

## 7. 주요 환경 변수

| 변수 | 설명 | 예시 |
|---|---|---|
| `OLLAMA_MODELS` | 모델 저장 위치 변경 (C 드라이브 용량이 부족할 때) | `D:\ollama-models` |
| `OLLAMA_HOST` | 서버 주소/포트 변경 | `0.0.0.0:11434` |
| `OLLAMA_KEEP_ALIVE` | 마지막 사용 후 모델을 메모리에 유지하는 시간 | `10m`, `1h`, `-1`(계속) |

Windows에서 설정 (설정 후 Ollama 앱을 재시작):

```powershell
[Environment]::SetEnvironmentVariable("OLLAMA_MODELS", "D:\ollama-models", "User")
```

> ⚠️ `OLLAMA_HOST=0.0.0.0` 은 같은 네트워크의 다른 기기에서도 접속할 수 있게 엽니다.
> 인증 기능이 없으므로 공용 네트워크에서는 쓰지 마세요.

---

## 8. 모델 고르기 팁

| 내 PC 사양 (RAM 또는 GPU 메모리) | 추천 모델 크기 |
|---|---|
| 8GB | 1B ~ 3B (예: `llama3.2:1b`, `llama3.2:3b`) |
| 16GB | 7B ~ 8B |
| 32GB 이상 | 13B ~ 14B 이상 |

- 숫자(B = Billion, 10억)가 클수록 똑똑하지만 느리고 메모리를 많이 씁니다.
- 처음에는 작은 모델로 시작해서 속도를 보고 키우세요.
- 한국어는 다국어 학습이 잘 된 모델(`qwen`, `gemma` 계열 등)이 대체로 잘합니다.

---

## 9. 자주 쓰는 작업 흐름

### 처음 써 보기

```powershell
ollama run llama3.2:3b     # 다운로드 + 실행
# 대화하다가 /bye 로 종료
ollama list                # 받은 모델 확인
```

### Python 프로젝트에서 쓰기

```powershell
uv add ollama
uv run main.py
```

### 정리하기

```powershell
ollama ps                  # 실행 중인 모델 확인
ollama stop llama3.2:3b    # 메모리에서 내리기
ollama rm llama3.2:3b      # 디스크에서 삭제
```
