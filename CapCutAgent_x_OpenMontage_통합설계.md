# CapCut Agent(DraftCut) × OpenMontage 통합 설계

> 재빈님이 만든 CapCut Agent를 분석해 OpenMontage와 붙이는 방법.
> 위치: 원본 `D:\peaceworks\lecture_builder\capcut_agent\` / 분석본 `…\영상 편집 자동화\capcut_agent\`
> 작성: 2026-06-25

---

## 1. CapCut Agent(DraftCut)가 하는 일 — 요약
- **입력**: 내가 찍은 실제 영상(mp4/mov) → **출력**: CapCut **드래프트**(컷+자막). mp4 아님. CapCut에서 마무리.
- **핵심 지능**:
  - **VAD(Silero) 무발화 감지** — 음량 아닌 "말소리 유무"로 판단 (발소리·바람만 있는 구간도 컷)
  - **LLM(claude-sonnet) 맥락 판단** — NG/잔말(3회 반복)·자조 혼잣말·긴 데드에어 컷, **의도적 침묵은 보존**
  - **한국어 자막** — 절경계 한 줄 분할 + **맞춤법 교정** + G마켓산스/검정바/중앙하단
  - **pycapcut 드래프트 빌더** + 폰트 주입 + tm_duration 패치(함정 해결)
- **모드**: edit(컷+자막) / sub(자막만) / cut(무음 점프컷) / lesson(아바타) / discussion(독서토론+Gemini 이미지)
- 로컬 처리(업로드X), GPU 전사(faster-whisper large-v3-turbo), FastAPI+pywebview 데스크톱 앱
- 모듈화(`cca/`): probe·transcribe·vad·subtitle·edit_decide·timeline·jumpcut·build·draftpath
- **상품화 중**: "DraftCut Local v1" (Lemon Squeezy 라이선스, $49 등) — 별도 상용 제품

## 2. 왜 OpenMontage와 완벽 보완인가 (빈칸이 맞물림)

| 항목 | CapCut Agent (DraftCut) | OpenMontage |
|---|---|---|
| 방향 | **실제 촬영본 편집 (A)** | **AI 생성 (B)** |
| 입력 | 내가 찍은 영상 | 주제 / 대본 |
| 강점 | 무음·NG·데드에어 컷, 한국어 자막(맞춤법) | 이미지·영상·나레이션·음악 생성, 모션그래픽 |
| 출력 | **CapCut 드래프트** ✓ | mp4 렌더 / 에셋 |
| 캡컷 핸드오프 | **이미 함** ✓ | 없음 ← 갭 |

→ **CapCut Agent가 OpenMontage의 최대 빈칸(실제 촬영본 편집 + 캡컷 드래프트 핸드오프 + 한국어 자막 정교함)을 정확히 채운다.**
→ 반대로 **OpenMontage가 CapCut Agent의 빈칸(AI 생성·모션그래픽)을 채운다.**
(우리가 모은 REF-4·10·11·13의 "편집 자동화 + 캡컷 핸드오프"를 재빈님이 이미 구현해 둔 셈)

## 3. 통합 방법 — 3가지 옵션

### 옵션 A. 역할 분담(병행 운영) ★지금 당장, 추천
- **내가 찍은 영상**(세차 시연·강의·토킹) → **CapCut Agent**로 컷+자막 → CapCut 드래프트
- **AI 생성 영상**(소스 없음) → **OpenMontage**로 생성
- 둘 다 **CapCut에서 마무리**가 종착점. 합치지 않고 **메뉴처럼 골라 씀.**
- 장점: 즉시 가능, 각자 완성도 그대로. 라이선스 충돌 없음.

### 옵션 B. OpenMontage 핸드오프 백엔드로 CapCut Agent의 드래프트 빌더 재사용 ▶중기
- OpenMontage가 만든 에셋(이미지/나레이션/음악/자막)을 → CapCut Agent의 `cca/build.py(build_draft)`·pycapcut로 넘겨 → **CapCut 드래프트** 생성.
- = OpenMontage의 "A-7 캡컷 핸드오프"를 **재빈님의 검증된 pycapcut 코드로 구현.**
- 느슨한 연동(별도 CLI/프로세스 호출 + 공통 파일포맷)으로 붙이면 안전.

### 옵션 C. 지능 공유 ▶부분
- CapCut Agent의 **한국어 자막 스타일·맞춤법 교정·VAD 컷 규칙**을 OpenMontage 지침/코드에 반영.
- OpenMontage가 쓴 대본·나레이션을 CapCut Agent가 활용.

## 4. 추천 아키텍처
```
[생성] OpenMontage ─┐
                    ├─→ (공통) CapCut 드래프트 ─→ CapCut에서 후반편집·내보내기
[편집] CapCut Agent ─┘
```
- **단기**: 옵션 A(역할 분담)로 바로 운영. (세차/강의 = CapCut Agent, AI영상 = OpenMontage)
- **중기**: 옵션 B로 OpenMontage 핸드오프를 CapCut Agent build_draft에 연결 → 출력 포맷 통일(둘 다 캡컷 드래프트).

## 5. ⚠️ 라이선스 주의 (중요)
- **CapCut Agent = 재빈님 상용 제품(DraftCut)**, **OpenMontage = AGPL-3.0(카피레프트).**
- 둘을 **코드로 합쳐 배포/판매**하면 AGPL이 CapCut Agent까지 전파될 수 있음 → **상품이 오픈소스 강제됨.**
- 따라서:
  - **상용 제품(DraftCut)에는 OpenMontage 코드를 넣지 말 것.** 독립 유지.
  - 통합은 **나만 쓰는 개인 워크플로**에서, **느슨한 연동(별도 프로세스·CLI·파일포맷)** 으로. (코드 병합 X)
  - 나만 쓰면 AGPL 의무 없음 — 단 상품화 라인과 섞이지 않게 분리.

## 6. 실행 단계
1. **(지금) 옵션 A 운영**: 실제 촬영본은 CapCut Agent, AI 생성은 OpenMontage.
2. CapCut Agent의 **한국어 자막/컷 규칙**을 OpenMontage CLAUDE.md에도 참고 반영(옵션 C 일부).
3. (원하면) **옵션 B 프로토타입**: OpenMontage 에셋 → CapCut Agent build_draft로 캡컷 드래프트 만들기. ※느슨한 연동.
4. 상용 DraftCut 라인은 **OpenMontage와 코드 분리** 유지.

---

## 7. 연동 인터페이스 (정밀 — 재빈님 기술문서 기반)
> 결론: **HTTP `/process`가 권장 연동법이자 라이선스 안전(느슨한 결합).**

### 7-1. 권장: HTTP `/process` + SSE  (포트 8765, localhost 전용, CORS 없음)
- 서버 실행: `python run_app.py` 또는 `uvicorn app:app --port 8765`
- 호출: `GET http://127.0.0.1:8765/process?path={영상}&mode=edit&preset=default&ai_grade=fast`
- 응답: SSE(text/event-stream) — `step`(run/done, progress 0~1) 반복 → `result{…}`
- **결과 폴더명은 반드시 `result.draft`에서 취득** (호출마다 타임스탬프로 달라짐 — 추측 금지)
- 제어/보조: `/control?action=stop|pause|resume`, `/export_srt?draft={name}`, `/usage`, `/diagnostics`, `/pick`
- 🔑 **이 방식 = 별도 프로세스 + HTTP = 느슨한 결합 → AGPL 안전.** Montage는 "클라이언트로 호출"만 (코드 병합 아님).

### 7-2. 모드(7) — `mode=`
`sub`(자막) · `edit`(자동편집: 컷+자막) · `cut`(무음컷) · `tight`(슈퍼타이트) · `lesson`(아바타) · `discussion`(독서토론+이미지) · `shorts`(쇼츠 하이라이트). ※ `lecture`/`translate`는 별도 엔드포인트, `merge`는 파라미터.

### 7-3. 주요 파라미터
`mode` · `preset`(stable/default/lecture/shorts…) · `cut_strength`(weak/default/strong) · `subtitle_length`(short~28자) · `ai_grade`(fast=Haiku/smart=Sonnet/best=Opus) · `srt`(주면 전사 스킵) · `merge`(폴더 이어붙임) · `broll` · `shorts_count` 등.

### 7-4. `result` 필드(핵심)
`draft`(폴더명 ★) · `mode` · `subs` · `kept`/`removed`(초) · `segments[]` · (edit)`cuts[]` · (shorts)`highlights[]` · (discussion)`image_cost_usd` · 토큰/비용.

### 7-5. 출력 산출물
`%LOCALAPPDATA%\CapCut\User Data\Projects\com.lveditor.draft\{draft}\` → `draft_content.json`(타임라인) + `draft_meta_info.json`(tm_duration 패치됨) + `{draft}.srt`.

### 7-6. 운영 주의 (중요)
- **서버가 떠 있어야** 호출됨 (포트 8765 하나, 단일 사용자 가정).
- **CapCut 재시작(adopt)** — CapCut 실행 중이면 새 드래프트를 즉시 못 볼 수 있음.
- 드래프트는 **항상 고유**(덮어쓰기 X) → 폴더 누적, Montage 측 정리 정책 필요.
- 코드 수정 후 **서버 완전 재시작**(stale 모듈 주의).
- 과금 = LLM/이미지 호출(키 사용 시). 키 없으면 폴백(전부 keep) 무과금.
- (참고) 전사는 **ElevenLabs Scribe → faster-whisper 폴백** — 우리 ElevenLabs 라인과 동일.

### 7-7. Montage 연동 추천 흐름 (옵션 B 구체화)
1. **내 촬영본 편집**: 그냥 CapCut Agent 단독(옵션 A). UI 또는 `/process?mode=edit`.
2. **AI 생성물 + 캡컷 마무리**: OpenMontage가 만든 영상/에셋 → (필요시 임시 mp4) → CapCut Agent `/process?mode=sub`(또는 edit) 호출 → `result.draft`로 폴더 확인 → CapCut에서 마무리.
3. 셋 다 **HTTP 호출**이라 코드 병합 없음 = 상용 제품 라인 보호.
