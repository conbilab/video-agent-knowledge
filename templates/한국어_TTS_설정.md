# 한국어 TTS — 검증된 설정 (자연스러움 OK)

> 2026-06-24 테스트로 확정. 웹사이트급 자연스러운 한국어 음성이 나오는 조합.
> OpenMontage/에이전트에서 한국어 나레이션 만들 때 이 설정을 쓰도록 지정할 것.

## ✅ 정답 조합 (ElevenLabs)
- **모델**: `eleven_multilingual_v2`  ← v3 아님(v3는 API에서 불안정·어색)
- **voice_settings**: stability `0.5` / similarity_boost `0.85` / style `0.0` / use_speaker_boost `true`
- 전제: ElevenLabs **유료 플랜**(무료는 API 402로 막힘)

### 보이스 (voice_id) — 재빈님 승인
- ⭐ **1순위(메인): `yohan koo` — `4JJwo477JUAx3HV0T7n7`** ← 제일 자연스러움
- 차선: `8jHHF8rMqMlg8if2mOUe`
- 차선: `pb3lVZVjdFWbkhPKlelB`

## ❌ 어색했던 원인 (피할 것)
- 약한 커뮤니티 클론 보이스(예: Deokpal) 자동 선택
- voice_settings 누락(기본값) → 밋밋·어색
- `eleven_v3` 모델 사용 → API에서는 불안정해서 오히려 이상함

## 무료 대안 (퀄리티 차선)
- `edge-tts`(MS Azure, 무료·키불필요): `--voice ko-KR-SunHiNeural` — 쓸 만하나 yohan koo보다 못함
- Piper(로컬 무료): 한국어 약함 → 비추천

## 참고: 더 자연스러운 한국어 특화 후보(미검증)
- Supertone/Voicebox(바탕화면에 있음), Typecast, Naver CLOVA Voice
