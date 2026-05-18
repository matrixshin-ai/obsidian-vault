Obsidian 출력 경로: C:\Users\신민식\Documents\Obsidian Vault\Portfolio.md

---

## 설정 과정

### 1. Google Drive API 활성화
- GCP 프로젝트: Youtube Auto (기존 프로젝트 재사용)
- Google Cloud Console → API 및 서비스 → 라이브러리 → Google Drive API → 사용 설정

### 2. OAuth 인증 설정
- 기존 Desktop client 1 재사용
- Client secret 신규 발급 (기존 secret 다운로드 불가 — Google 보안 정책 변경)
- credentials.json 으로 저장

### 3. 스크립트 생성 및 최초 인증
- C:\portfolio-sync\ 폴더 생성
- sync_portfolio.py 작성
- 최초 실행 시 브라우저 OAuth 인증 완료 → token.json 자동 생성

### 4. 경로 오류 수정
- 초기 설정 경로: C:\Users\신민식\Documents\ObsidianVault\ (공백 없음, 잘못된 경로)
- 실제 Vault 경로: C:\Users\신민식\Documents\Obsidian Vault\ (공백 있음)
- .obsidian 폴더 탐색으로 실제 경로 확인 후 스크립트 수정

### 5. Windows 작업 스케줄러 등록
- 실행 시간: 매일 03:00 KST
- 프로그램: python
- 인수: sync_portfolio.py
- 시작 위치: C:\portfolio-sync\

---

## 비용

| 항목 | 비용 |
|------|------|
| Google Drive API | 무료 |
| Python + 작업 스케줄러 | 무료 |
| Obsidian (로컬 vault) | 무료 |
| 합계 | $0 |

---

## 동작 방식

- 매일 03:00 KST 자동 실행
- Google Drive Portfolio 문서 내용을 읽어 last_snapshot.txt와 비교
- 내용 동일 → 아무 작업 없이 종료
- 내용 변경 → Portfolio.md 갱신 + last_snapshot.txt 업데이트

---

## 참고: CONTROL_TOWER와의 연관성

- ocr-automation과 동일한 패턴 (Windows Task Scheduler + Python 로컬 스크립트)
- 추후 AI agent가 Obsidian의 Portfolio.md만 읽으면 항상 최신 포트폴리오 내용 접근 가능
- Google Drive API 인증 구조를 다른 문서 동기화에도 동일하게 적용 가능
