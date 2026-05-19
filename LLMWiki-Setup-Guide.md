# LLM Wiki 구축 가이드 (민식님 작업 정리)
> 작성일: 2026-05-19

---

## 1. 개념 이해

### Karpathy의 LLM Wiki란?
- 원문: Andrej Karpathy의 GitHub gist (2025년 12월)
- 핵심 아이디어: **"Obsidian은 IDE, LLM은 프로그래머, Wiki는 코드베이스"**
- 기존 방식: 사람이 지식 베이스를 관리하고 AI에게 질문
- LLM Wiki 방식: AI가 지식 베이스를 구축·유지하고 사람은 재료만 제공

### 3계층 구조
```
sources/     ← 원본 자료 (읽기 전용)
    ↓ ingest
concepts/    ← LLM이 자동 생성하는 Wiki 페이지
    ↓ query
schema/      ← Wiki 구조 설정
```

---

## 2. 환경 구성

### 필수 도구
| 도구 | 용도 |
|------|------|
| Obsidian | Vault 뷰어/편집기 |
| Claude Code (터미널) | Wiki 작업 엔진 |
| GitHub | 두 PC 동기화 |
| Obsidian Git 플러그인 | 자동 커밋/푸시 |

### Obsidian 플러그인
| 플러그인 | 용도 | 설치 방법 |
|---------|------|---------|
| Claude Code Integration | Obsidian 내 AI 어시스턴트 | BRAT → `deivid11/obsidian-claude-code-plugin` |
| Claudian | ~~사이드 채팅창~~ (제거) | - |
| Git (Vinzent) | 자동 동기화 | 커뮤니티 플러그인 |
| Zotero Integration | PDF/논문 → MD | 커뮤니티 플러그인 |
| BRAT | 베타 플러그인 설치 | 커뮤니티 플러그인 |

### Claude Code Integration 설치 시 주의사항 (Windows)
- WinGet 설치 경로는 작동 안 함 (`exit code 1`)
- `.cmd` 파일도 작동 안 함 (`spawn EINVAL`)
- **해결책:** npm으로 설치 후 네이티브 바이너리 직접 지정
```
C:\npm-global\node_modules\@anthropic-ai\claude-code\bin\claude.exe
```

---

## 3. LLM Wiki 설치

### 설치 명령어
```bash
cd "Documents\Obsidian Vault"
git clone https://github.com/Ar9av/obsidian-wiki.git obsidian-wiki
cd obsidian-wiki
bash setup.sh
```

### 설정 질문
- Source documents: **Skip for now**
- QMD search: **No / Skip**

### 설치 후 생성되는 폴더
```
obsidian-vault/
├── concepts/      ← Wiki 자동 생성 페이지
├── sources/       ← 원본 자료
├── Clippings/     ← Web Clipper 저장 폴더
├── obsidian-wiki/ ← LLM Wiki 시스템 (.gitignore 제외)
└── ...
```

### .env 설정
```
OBSIDIAN_VAULT_PATH=C:\Users\신민식\Documents\Obsidian Vault
OBSIDIAN_SOURCES_DIR=C:\Users\신민식\Documents\Obsidian Vault\Clippings
```

---

## 4. 자료 수집 워크플로우

### 웹 기사 수집
1. Chrome에서 기사 읽기
2. **Obsidian Web Clipper** 아이콘 클릭
3. `Clippings/` 폴더에 `.md` 파일로 자동 저장
4. GitHub 자동 동기화 (10분마다)

### PDF/논문 수집
1. **Zotero** + Chrome Connector로 저장
2. **Better BibTeX** 플러그인 필요
3. Zotero Integration으로 Obsidian에 가져오기
> ⚠️ 웹 기사 본문은 자동 추출 안 됨. 웹 기사는 Web Clipper 사용 권장

### 직접 작성
- Vault에 MD 파일 직접 작성
- `sources/` 또는 `Economy/`, `JAMnomics/` 등 해당 폴더에 저장

---

## 5. LLM Wiki 핵심 명령어

| 명령어 | 용도 |
|--------|------|
| `wiki-status` | 현재 상태 확인 (미수집 자료, 페이지 수 등) |
| `wiki-ingest` | 새 자료를 Wiki로 변환 |
| `wiki-query` | Wiki에 질문 |
| `wiki-lint` | Wiki 품질 검사 (모순, 고립 페이지 감지) |
| `wiki-update` | Wiki 업데이트 |

> **실행 위치:** 터미널 Claude Code에서 실행 (Obsidian 내부 Claude Code 아님)

---

## 6. GitHub 동기화

### 초기 설정
```bash
git init
git remote add origin https://github.com/matrixshin-ai/obsidian-vault.git
git push -u origin main
```

### .gitignore 제외 항목
```
*.png, *.jpg, *.gif, *.mp4, *.pdf  ← 바이너리
obsidian-wiki/                      ← LLM Wiki 시스템
.obsidian/workspace.json            ← 개인 설정
```

### 자동 동기화 (Obsidian Git 플러그인)
- Auto pull: 10분
- Auto commit: 10분
- Auto push: 10분

### 사무실 PC 초기 설정
```bash
git clone https://github.com/matrixshin-ai/obsidian-vault.git
```
→ Obsidian에서 해당 폴더 열기

---

## 7. 두 도구의 역할 구분

| 작업 | 도구 |
|------|------|
| 현재 노트 편집/요약 | Obsidian Claude Code Integration |
| wiki-ingest, wiki-status | 터미널 Claude Code |
| Git 작업, 플러그인 설치 | 터미널 Claude Code |
| 노트에 대한 질문 | Obsidian Claude Code Integration |
| 장시간 대규모 작업 | 터미널 Claude Code |

---

## 8. 향후 계획

- [ ] Economy 폴더 전체 ingest
- [ ] JAMnomics ingest (Economy와 cross-link)
- [ ] `/wiki-lint` 실행으로 품질 점검
- [ ] Life Wiki 별도 Vault 구성 (개인 일상용)
- [ ] Zotero PDF 워크플로우 개선
