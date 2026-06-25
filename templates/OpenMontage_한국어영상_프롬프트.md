# OpenMontage 한국어 영상 제작 프롬프트 (첫 테스트용)

> 사용법: OpenMontage 폴더(`~/Developer/OpenMontage`)를 Cursor 또는 Claude Code로 열고,
> 아래 내용을 AI 채팅에 그대로 붙여넣으면 됨. 주제·길이만 바꿔 재사용.

---

OpenMontage로 짧은 한국어 정보 영상을 한 편 만들어줘.

[영상 사양]
- 주제: "아침 5분 스트레칭의 3가지 효과"  (원하면 바꿔도 됨)
- 길이: 약 20~25초
- 화면비: 9:16 세로 (숏폼용)

[제작 방식 — 비용 최소화]
- 이미지 기반으로 제작해줘: gpt-image-1(OpenAI)로 이미지 생성 → Remotion으로 애니메이트(줌/켄번즈/패럴랙스).
- 비싼 영상 생성 모델(Veo, Kling, Seedance 등)은 쓰지 마.

[나레이션 — 한국어]
- ElevenLabs 사용.
- voice_id: 4JJwo477JUAx3HV0T7n7  (yohan koo)
- model_id: eleven_multilingual_v2
- voice_settings: stability 0.5, similarity_boost 0.85, style 0.0, use_speaker_boost true

[자막 — 한국어]
- 한국어 단어 단위 자막을 영상에 넣어줘.
- 한국어 폰트(Pretendard 또는 Noto Sans KR)를 사용해서 글자가 깨지지 않게 해줘.

[음악]
- 잔잔한 로열티프리 배경음악(볼륨 낮게).

[중요]
- 시작 전에 예상 비용과 제작 계획(어떤 모델·몇 컷·길이)을 먼저 보여주고, 내 승인을 받은 뒤에 생성을 시작해줘.
- 비용은 최대한 낮게 유지해줘.
