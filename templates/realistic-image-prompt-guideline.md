# GPT Image 2 Realistic Image Prompt Guideline (완전판)

> Montage 자동화에서 **하이퍼리얼 실사 이미지**를 만들 때의 완전한 프롬프트 지침.
> 모델: **gpt-image-2** (한글·텍스트 양호, 실사 품질 높음). 비용 더 들어도 OK.
> 사용법: 에이전트(Claude)가 **장면 설명 → 이 지침대로 상세 영어 프롬프트로 확장 → gpt-image-2 생성.** 장면 유형에 맞는 §5 미니 템플릿을 골라 쓴다.

---

## 1. 목적 / 핵심
출력 이미지는 실제 사진처럼 자연스럽고, 과도한 AI 느낌·플라스틱 피부·비현실 조명·과장 색감·과보정을 피한다. 모든 업로드 이미지엔 AI 생성 표시(워터마크)를 포함한다.
실사감 = **피사체 + 순간 + 장소 + 실제 카메라 + 실제 조명 + 현실적 결함 + 금지사항 + AI 표시.**
"멋진 이미지"가 아니라 **"실제 카메라로 촬영한 특정 장면"** 처럼. 완벽함보다 자연스러운 불완전함(먼지·주름·미세 흔들림·조명 불균형).

## 2. 기본 생성 원칙
1. "실제 사진처럼"이 아니라 "실제 카메라로 촬영한 특정 장면"처럼 묘사.
2. 목적·피사체·장소·시간·조명·카메라·렌즈·구도·질감·결함·금지사항을 분리해 작성.
3. "8K, ultra realistic, masterpiece" 같은 추상 키워드만 반복 금지.
4. 완벽함보다 자연스러운 불완전함을 넣는다.
5. 실제 유명인·일반인·정치인·연예인과 닮게 만들지 않는다.
6. 광고·유튜브·SNS·쇼츠·릴스용 이미지는 반드시 AI 생성 표시 포함.
7. 워터마크는 프롬프트 + 후처리 코드로 한 번 더 삽입.
8. 화면 안 텍스트가 중요하면 짧고 명확한 문구만.
9. 생성 후 손가락·눈·치아·그림자·반사·원근·텍스트·로고·브랜드 오염 검수.

## 3. 프롬프트 작성 순서 (10단계)
1. **Scene Purpose** — 어떤 영상 장면용인지 (YouTube documentary B-roll / marketing still / cinematic montage frame / social ad / educational scene / product background)
2. **Main Subject** — 피사체 구체적으로 (a middle-aged Korean small business owner / a busy urban street after rain / a premium car wash machine in a clean industrial bay …)
3. **Action / Moment** — 포즈 아닌 촬영된 순간 (captured while walking / adjusting a control panel / pouring coffee / wiping water from a vehicle surface …)
4. **Environment** — 장소·시간·날씨·주변 (early morning quiet Korean neighborhood / rainy evening wet asphalt / bright commercial interior with practical fluorescent …)
5. **Camera & Lens** — shot like a real documentary photograph / full-frame / 35mm(거리) · 50mm(인물) · 85mm(포트레이트 압축) / eye-level / handheld documentary feel / shallow DOF / natural motion blur / realistic focus falloff
6. **Lighting** — soft natural window light / overcast daylight / golden-hour side light / practical fluorescent / mixed warm indoor + cool outdoor / soft consistent shadows / realistic reflections on glass·metal·water·skin
7. **Texture & Imperfections** — visible skin pores·fine wrinkles / under-eye shadows / hair flyaways / fabric texture·wrinkles / fingerprints on glass / dust / tiny metal scratches / water droplets·uneven reflections / asphalt texture / lens grain / subtle chromatic aberration / natural color balance
8. **Composition** — cinematic 16:9 / subject slightly off-center / 전·중·후경 분리 / 자막용 여백 / clean hierarchy / not overly staged / no fantasy composition unless requested
9. **Negative Constraints** — §7
10. **AI Disclosure Label** — §8

## 4. 모델 / 사이즈 (JSON 구조)
```json
{
  "model": "gpt-image-2",
  "quality": "high",
  "size": "1536x1024",
  "prompt_template": {
    "usage_context": "video montage still / YouTube B-roll / ad creative / documentary frame",
    "main_subject": "", "subject_details": "", "action_or_moment": "",
    "location": "", "time_of_day": "", "weather_or_atmosphere": "", "background_details": "",
    "lens_choice": "35mm lens for natural perspective", "camera_angle": "eye-level",
    "framing": "medium-wide shot", "lighting_style": "natural available light",
    "ai_disclosure_text": "AI-generated content"
  },
  "post_process_required": {
    "visible_watermark": true, "watermark_text": "AI-generated content",
    "watermark_position": "bottom-right", "watermark_opacity": 0.55,
    "watermark_background": "semi-transparent dark rectangle",
    "preserve_c2pa_metadata_if_possible": true
  }
}
```
권장 사이즈: 일반 가로 `1536x1024`(또는 2560x1440 2K 상한) · 쇼츠/릴스 세로 `1024x1536` · 대량 테스트 quality `medium/low`. 4K는 결과 변동 큼.

## 5. 장면 유형별 미니 템플릿 (A~G)

### A. 인물 실사
```text
Create a photorealistic candid photograph of {person_description} in {location}.
The person is {action}, with a natural facial expression and relaxed body language.
Shot like a real documentary photo, 50mm lens, eye-level, shallow but realistic depth of field.
Use natural light from {light_source}, with soft shadows and realistic skin texture.
Include visible pores, subtle wrinkles, natural hair flyaways, fabric texture, and small everyday imperfections.
The image should feel honest, unstaged, and believable.
No plastic skin, no beauty filter, no celebrity likeness, no public figure likeness, no distorted hands, no extra fingers.
Add a small bottom-right label: "AI-generated content".
```

### B. 거리·도시·상권 실사
```text
Create a photorealistic street-level photograph of {street_or_city_scene}.
The scene takes place in {location} during {time_of_day}, with {weather_or_mood}.
Shot with a 35mm lens from eye level, natural urban perspective, realistic foreground and background depth.
Include real-world details: asphalt texture, signs, parked vehicles, pedestrians, window reflections, utility poles, streetlights, small imperfections.
Lighting should be physically consistent, with realistic shadows and reflections.
Avoid overly dramatic neon, fantasy lighting, CGI look, or fake cinematic color grading.
No random unreadable text, no fake brand logos, no distorted people.
Add a small bottom-right label: "AI-generated content".
```

### C. 제품·광고 실사
```text
Create a photorealistic product photograph of {product}.
The product is placed in {environment}, being used or displayed in a realistic way.
Shot like a real commercial photo but not overly polished. Use {lens_choice}, clean composition, realistic contact shadows, accurate material texture.
Show realistic reflections, small surface imperfections, label legibility if present, and natural scale.
Do not invent extra logos or text. Do not make the product look like CGI or a 3D render.
Add a small bottom-right label: "AI-generated content".
```
※ 실제 판매 제품이면 AI 착용컷·사용 장면·성능 묘사가 소비자를 오해시키지 않게. 광고는 AI 표시 명확히.

### D. 음식·카페·베이커리 실사
```text
Create a photorealistic food photography image of {food_or_drink} in {setting}.
The food should look natural and freshly prepared, not artificially perfect.
Shot with a 50mm lens, close-up or medium close-up, realistic depth of field.
Use warm practical light, natural shadows, realistic crumbs, steam, condensation, uneven texture, and small imperfections.
Avoid plastic-looking food, excessive gloss, impossible perfect symmetry, or CGI appearance.
Add a small bottom-right label: "AI-generated content".
```

### E. 실내 공간·업무 장면
```text
Create a photorealistic interior photograph of {interior_scene}.
The space is {space_description}, with realistic furniture placement and everyday objects.
Shot with a 24mm or 35mm lens, natural perspective, no exaggerated wide-angle distortion.
Use realistic indoor lighting: {lighting_style}.
Include practical details: cables, paper, cups, dust, fabric texture, reflections on screens, small clutter where appropriate.
The image should feel like a real workplace or home, not a showroom render.
No CGI look, no perfect showroom symmetry, no distorted furniture, no random unreadable text.
Add a small bottom-right label: "AI-generated content".
```

### F. 산업현장·기계·세차장 실사 ★(세차고수 직결)
```text
Create a photorealistic documentary photograph of {industrial_scene}.
Main subject: {machine_or_worker}.
The scene takes place in a realistic industrial or commercial environment with concrete floors, metal surfaces, hoses, cables, control panels, and practical lighting.
Shot with a 35mm lens, eye-level or slightly low angle, realistic scale and perspective.
Include realistic water droplets, wet floor reflections, metal scratches, dust, bolts, worn edges, and operational details.
The machine should look functional and physically plausible.
No futuristic sci-fi look, no impossible machinery, no extra logos, no CGI render style.
Add a small bottom-right label: "AI-generated content".
```

### G. 자연·여행·풍경 실사
```text
Create a photorealistic landscape photograph of {landscape_scene}.
The scene is captured during {time_of_day}, with {weather_or_atmosphere}.
Shot with a realistic camera perspective, natural depth, physically plausible lighting, and true-to-life colors.
Include natural imperfections: uneven ground, haze, small debris, water reflections, plant variation, atmospheric depth.
Avoid fantasy colors, over-sharpening, HDR exaggeration, or painterly style.
Add a small bottom-right label: "AI-generated content".
```

## 6. 바로 쓰는 최종 마스터 프롬프트 ({} 교체)
```text
Create a photorealistic image for a video montage.
Purpose: {usage_context}
Main subject: {main_subject}
The subject is {action_or_moment}.
The scene takes place in {location} during {time_of_day}, with {weather_or_atmosphere}.
Make it look like a real photograph captured in the moment, not a staged AI render.
Use a realistic camera perspective: {camera_angle}, {lens_choice}, {framing}.
Composition should be cinematic 16:9, with clear foreground, midground, and background separation. Leave enough negative space for subtitles if appropriate.
Lighting: {lighting_style}. Use physically consistent shadows, reflections, and natural color balance.
Add realistic details and imperfections: {realistic_details}
The image should feel grounded, natural, documentary-like, and believable. Avoid over-polished advertising perfection unless specifically requested.
Negative constraints:
No cartoon, no anime, no painting, no illustration, no 3D render, no CGI look, no plastic skin, no beauty filter, no fake bokeh, no warped hands, no distorted fingers, no duplicate limbs, no random unreadable text, no extra logos, no celebrity likeness, no public figure likeness, no misleading depiction of a real event.
AI disclosure:
Add a small visible label in the bottom-right corner that reads "AI-generated content". Readable but unobtrusive, clean white sans-serif with subtle dark shadow / semi-transparent dark background. Do not cover the main subject.
```

## 7. Negative Constraints (기본)
no cartoon · no anime · no painting · no illustration · no 3D render / CGI look · no plastic skin · no over-smoothed/beauty-filter face · no fake bokeh · no warped/distorted hands·fingers · no duplicate limbs · no unnatural teeth · no random unreadable text · no extra/fake logos · no celebrity/public-figure likeness · no misleading real-event depiction

## 8. AI 표시 / 워터마크 (3중 표기 — 유튜브 정책)
1) 프롬프트에 "AI-generated content" 라벨 요청
2) **후처리 코드로 워터마크 강제 삽입**
3) 업로드 시 플랫폼 "AI 사용" 표시 체크
문구: 글로벌=「AI-generated content」 / 한국 교육·정보=「AI 생성 이미지」 / 광고=「AI-generated content」
### 워터마크 후처리 규칙
```
Text: AI-generated content
Position: bottom-right corner
Style: white sans-serif, 55% opacity, small semi-transparent black rounded rectangle behind text,
margin 2.5% of image width, font size ~3% of image width, never cover faces/products/logos/key subjects
Export: preserve metadata if possible; keep a clean master + a public watermarked file
```
⚠️ 유튜브는 사실적 AI 생성/변형 콘텐츠 업로드 시 공개 의무. 미표시 시 라벨 강제·삭제·파트너 정지 가능.

## 9. 실사 품질 검수 체크리스트 (재생성 트리거)
- **인물**: 손가락 왜곡·여분 손가락·이상한 눈·플라스틱 피부·과한 매끈 얼굴·이상한 치아·클로즈업 귀 비대칭·공인 닮음·부적절 맥락의 아동
- **장면**: 불가능한 그림자·조명 방향 불일치·가짜 반사·떠 있는 물체·원근 왜곡·비현실 스케일·과한 색보정·CGI/3D 느낌
- **텍스트/브랜드**: 깨진 글자·가짜 로고·우연한 유명 브랜드·언어 틀린 간판·깨진 워터마크·AI 표시 누락
- **상업 리스크**: 없던 사건 암시·실존 인물 발언/행동 암시·실제 장소 오해·성능을 사실 증거처럼 오인

## 10. 한글 장면 입력 → 영어 프롬프트 확장 규칙
한글 장면 요청(예: "비 오는 서울 거리에서 우산 쓴 직장인들")이 오면 직역하지 말고, §3의 10단계(피사체·순간·장소·시간·날씨·렌즈·앵글·조명·질감·결함·금지·AI표시)로 확장한 production-ready 영어 프롬프트를 만든다. 짧고 모호한 프롬프트 금지. "실제 카메라로 어떻게 찍히는지" 항상 묘사.
