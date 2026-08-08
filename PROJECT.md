# 엘림이레교회 홈페이지 — 프로젝트 인계 문서

> 이 문서 하나로 Claude Code에서 바로 이어서 작업할 수 있습니다.
> 작업 시작 전 반드시 이 파일을 읽고, 아래 **작업 원칙**을 지켜주세요.

---

## 1. 프로젝트 개요

| 항목 | 내용 |
|---|---|
| 교회명 | 대한예수교장로회 백석총회 충청노회 **엘림이레교회** |
| 담임목사 | 강성훈 |
| 주소 | 서울특별시 서초구 염곡말길 17-5, 2층 |
| 대표전화 | 02-6053-7644 |
| 설립 | 2018. 2. 25 |
| 희망 도메인 | `elimire.com` (1순위) → `elimire.kr` → `elimire.church` → `elimirechurch.com` → `elimagechurch.kr` |
| 저장소 | `intoedu/Beliefriends-Mission-Network` |
| 배포 | GitHub Pages (도메인 연결 전) |
| 구조 | **단일 파일 HTML SPA** (`index.html` 하나, 약 231KB) |
| 백엔드 | **없음.** Firebase / Supabase / 서버 일절 사용 안 함 |

### 핵심 정체성 (문구 작성 시 반드시 지킬 것)
- 슬로건: **"진리 안에서 행하는 예수부흥을 선포하는 정통개혁주의 교회"** (요삼 1:3)
- 핵심 고백: **"인간 공로 0, 예수 공로 100"**
- 신학 계보: 바울 → 어거스틴 → 칼빈 (예수신학체계)
- 성경관: 66권을 하나님의 영감으로 기록된 **정확무오한 말씀**
- 신앙고백: 사도신경 + 웨스트민스터 신앙고백 + 기독교의 12신조

### 표현 금지 사항
- ❌ "이단·사설이 아니다" 같은 **부정 표현 전면 금지** (목사님 명시 요청)
- ❌ "백석대신" 표기 금지 → 반드시 **"백석총회 충청노회"**
- ❌ 담임목사 개인 휴대전화(010-3869-8940) · 개인 이메일(kpaulk55@gmail.com) **화면 노출 금지**
- ✅ 긍정적 표현으로만 정체성 서술

---

## 2. 작업 원칙 (중요)

1. **없는 사실을 지어내지 말 것.** 교회 자료에 없는 내용(사역 설명, 성구 인용, 연혁, 인물 정보 등)을 추측해서 채우지 않는다. 자료가 없으면 "자료 대기" 플레이스홀더를 유지한다.
2. **Firebase / Supabase / 서버 도입 금지.** 모든 기능은 정적 HTML + 외부 무료 서비스(Google Forms, YouTube, Naver Band)로 해결한다.
3. **단일 파일 유지.** 이미지는 base64 내장. 외부 파일 의존 없이 `index.html` 하나로 동작해야 한다. (`og-image.jpg`·`404.html`·`robots.txt`·`sitemap.xml` 은 예외 — 공유 미리보기와 검색엔진용이라 별도 파일이어야 합니다.)
4. **한글 타이포 주의.** 한글에는 이탤릭이 없다. `em` 태그는 색상 강조만 하고 기울이지 않는다 (영문 모드에서만 `font-style:italic` 적용). 명조 제목의 `line-height`는 1.36 이상.
5. **다국어 동기화.** HTML에 `data-i18n="키"` 추가 시 JS의 `EN` 객체에 같은 키를 반드시 추가한다. 누락 시 영문 모드에서 한글이 그대로 노출된다. **같은 키를 두 번 쓰지 말 것** — 뒤에 쓴 값이 앞의 값을 조용히 덮어써서 엉뚱한 영문이 나온다.
6. **디자인 토큰 임의 변경 금지.** 색상은 `:root` CSS 변수만 사용한다.
7. **운영자용 안내문을 화면에 쓰지 말 것.** "CONFIG에 넣으면", "파일 하단 목록에 추가하면" 같은 문장은 방문자에게 보인다. 운영 안내는 HTML 주석이나 JS 주석으로 남긴다.
8. **예배 시간은 세 곳을 함께 고칠 것.** ① `#visit` 섹션의 예배 카드 ② 푸터 "예배 시간" ③ JS의 `SERVICES` 배열(카운트다운용). 한 곳만 고치면 어긋난다.

---

## 3. 파일 구조

```
Beliefriends-Mission-Network/
├── index.html      ← 전체 사이트 (HTML + CSS + JS + 이미지 base64)
├── 404.html        ← 없는 주소 안내 페이지
├── og-image.jpg    ← 카톡·페북 공유 미리보기 (1200×630)
├── robots.txt      ← 검색엔진 수집 안내
├── sitemap.xml     ← 검색엔진용 주소 목록
├── README.md
└── PROJECT.md      ← 이 문서
```

`index.html` 내부 순서:
1. `<head>` — 메타/OG 태그, JSON-LD 교회 정보, 폰트 로드
2. `<style>` — 전체 CSS (`:root` 디자인 토큰부터 시작)
3. `<body>` — 상단바 → 헤더 → 각 섹션 → 푸터
4. `<script>` — **`CONFIG` 객체** → 콘텐츠 데이터 → 다국어 사전 `EN` → 렌더링 함수 → 이벤트

---

## 4. 디자인 시스템

### 색상 토큰 (`:root`)
```css
--blush : #F0EDE2   /* 기본 배경 (웜 그레이지) */
--blush2: #E5DECB   /* 교차 섹션 배경 */
--card  : #FCFBF4   /* 카드 배경 */
--cream : #FDFCF7   /* 밝은 카드 */
--sand  : #F5EEE0   /* 네비게이션 배경 */
--wine  : #17284A   /* 짙은 군청 (주색) */
--wine2 : #0E1B33   /* 더 짙은 군청 */
--gold  : #B0842E   /* 골드 (밝은 배경용 포인트) */
--gold-soft: #E0C68C /* 밝은 금 (어두운 배경용) */
--ink   : #242833   /* 본문 텍스트 */
--ink2  : #5E6270   /* 보조 텍스트 */
--line  : #DED7C4   /* 테두리 */
```

**색 면적 비율** — 웜 배경 약 60% / 군청 약 25% / 골드 약 15%.
참고 사이트(First Bible Normangee)의 웜·쿨 대비 구조를 그대로 따랐다. 군청 일색으로 만들면 차갑고 밋밋해지므로 웜톤 바탕을 유지할 것.

### 서체
| 용도 | 서체 |
|---|---|
| 한글 제목 | **Noto Serif KR** 700 |
| 라틴 디스플레이/숫자 | **Fraunces** |
| 본문 | **Hanken Grotesk** + **Pretendard Variable** |

### 공통 컴포넌트
- `.btn` / `.btn.ghost` — 라운드 40px 버튼
- `.head` + `.kick` — 섹션 헤더 (골드 라벨 + 양옆 장식선)
- `.qcard` `.tcard` `.apcard` `.mspil` `.pcard` — hover 시 상단 골드 헤어라인
- `.pendbox` `.pend` `.mspend` — **"자료 대기"** 플레이스홀더
- `.rv` — IntersectionObserver 등장 애니메이션 (형제 요소 0.08초씩 순차 지연)
- `body::before` — 전체 종이 그레인 텍스처 (opacity .055)
- `.skip` — 키보드 사용자용 "본문 바로가기" 링크 (탭 키를 누르면 나타남)

### 반응형 분기
| 폭 | 동작 |
|---|---|
| 1241px 이상 | 상단 메뉴 10개 전부 한 줄 |
| 1025 ~ 1240px | 메뉴 간격·글자 축소해서 한 줄 유지 |
| **1024px 이하** | **햄버거 서랍 메뉴로 전환** (닫힌 상태에서는 `visibility:hidden` — 탭 키로 안 잡힘) |
| 680px 이하 | 카드 그리드 1열 |

---

## 5. CONFIG — 운영자가 수정하는 유일한 곳

`<script>` 최상단에 있습니다.

```javascript
var CONFIG = {
  autoLatest       : true,                         // true = 말씀 섹션에 채널 최신 영상 자동 표시
  featuredVideo    : "3L1buZ34ePI",                // 히어로 대표 영상 ID
  youtubeChannelId : "UC1qT1lENffxt_Hczg9_-CeQ",
  youtubeUrl       : "https://www.youtube.com/channel/UC1qT1lENffxt_Hczg9_-CeQ",
  bandUrl          : "https://band.us/@elimire",
  blogUrl          : "https://blog.naver.com/paulkang0411",
  googleFormUrl    : "",   // 문의 양식 (…/viewform?embedded=true)
  formNewFamily    : "",   // 새가족 등록
  formVisit        : "",   // 심방 신청
  formPrayer       : "",   // 기도 제목
  formReceipt      : "",   // 기부금영수증 (비공개)
  bankName         : "농협",
  bankAccount      : "301-0240-1263-41",
  bankHolder       : "엘림이레교회",
  bankNameEn       : "Nonghyup Bank",    // 영문 모드 표시용 (비우면 bankName 그대로)
  bankHolderEn     : "Elim Jireh Church" // 영문 모드 표시용 (비우면 bankHolder 그대로)
};
```

빈 값 처리:
- 폼 URL이 비면 해당 카드가 `#connect`(문의)로 연결되고 라벨이 "문의로 연결 →"로 바뀝니다. (언어를 전환해도 유지됩니다)
- `googleFormUrl`이 비면 문의 양식 자리에 대표전화 안내가 표시됩니다.
- `blogUrl`이 비면 블로그 아이콘이 숨겨지고, `bandUrl`이 비면 공지 섹션의 밴드 링크가 숨겨집니다.

---

## 6. 섹션 구성 (의뢰서 메뉴 전부 반영)

| # | 섹션 | id | 상태 |
|---|---|---|---|
| 1 | 히어로 | `#top` | ✅ 영상 썸네일 + 다음 예배 카운트다운 |
| 2 | 빠른 안내 4카드 | — | ✅ |
| 3 | 교회소개 / 인사말 | `#about` | ✅ 목사님 원고 전문 + 사진 |
| 4 | 연혁 타임라인 | `#about` 내 | ✅ 10건 |
| 5 | 신앙고백 12신조 | `#creed` | ✅ HWP 전문 |
| 6 | 예배·오시는 길 | `#visit` | ✅ 예배 5종 + 구글맵 |
| 7 | 사역 안내 | `#ministries` | ✅ 6종 |
| 8 | 섬기는 분들 | `#ministries` 내 | ✅ 8개 직분 |
| 9 | **선교** | `#mission` | ⚠️ 뼈대만 — 자료 대기 |
| 10 | 말씀 (유튜브) | `#sermons` | ✅ 자동연동 |
| 11 | ~~AI 말씀 안내 로드맵~~ | — | ❗ **화면에서 빠져 있음** (아래 10-6 확인) |
| 12 | 공지·소식 | `#news` | ⚠️ 빈 배열 — 자료 대기 |
| 13 | 갤러리 | — | ⚠️ 사진 3자리 비어 있음 |
| 14 | 신청·예약 | `#apply` | ⚠️ 폼 4종 미생성 |
| 15 | 문의하기 | `#connect` | ⚠️ 구글 폼 미생성 |
| 16 | 후원·헌금 | `#give` (배너) / `#connect` 내 계좌 | ✅ 계좌 + 자발성 안내 |
| 17 | FAQ | `#faq` | ⚠️ 빈 배열 — 자료 대기 |

---

## 7. 콘텐츠 데이터 (JS 배열)

`CONFIG` 아래에 있습니다. 배열만 수정하면 화면이 갱신됩니다.

| 배열 | 내용 | 출처 |
|---|---|---|
| `HISTORY` | 연혁 10건 | 교회 약력 문서 |
| `PEOPLE` | 섬기는 분들 8개 직분 | 교회 약력 문서 |
| `CREED` | 기독교의 12신조 전문 | 교회 제공 HWP |
| `SERVICES` | 예배 5종 (카운트다운용) | 회신 자료 |
| `NOTICES` | 공지 (현재 빈 배열) | **미제공** |
| `FAQS` | FAQ (현재 빈 배열) | **미제공** |

`NOTICES` / `FAQS`가 비어 있으면 화면에 자동으로 "자료 대기" 박스가 표시됩니다. 이 동작을 제거하지 마세요.

작성 형식:
```javascript
const NOTICES = [
  {date:"2026.08.10", tag:{ko:"공지",en:"NOTICE"}, ko:"제목", en:"Title"},
];
const FAQS = [
  {q:{ko:"질문",en:"Question"}, a:{ko:"답변",en:"Answer"}},
];
```

---

## 8. 유튜브 연동 — 오류 153 대응 (중요)

**증상:** 영상 자리에 `오류 153 · 동영상 플레이어 구성 오류`

**원인:** `index.html`을 브라우저에서 **직접 열면**(`file://`) 유튜브가 임베드를 차단합니다. 유튜브는 요청 출처(origin)를 확인하는데 로컬 파일은 출처가 없기 때문입니다. **영상이나 채널 설정 문제가 아닙니다.**

**해결:** HTTP(S) 주소로 접속하면 정상 재생됩니다.
```bash
python3 -m http.server 8000
# → http://localhost:8000 으로 접속
```
또는 GitHub Pages 배포 후 확인.

**현재 코드 상태:** `IS_FILE` 플래그로 `file://` 접속을 감지하여, 오류 화면 대신 안내 문구와 "유튜브에서 보기" 버튼을 표시합니다. 임베드 도메인은 `youtube-nocookie.com`을 사용합니다. 히어로 썸네일은 `maxresdefault` → `hqdefault` 순으로 시도하고, 둘 다 실패하면 깨진 이미지 대신 안내 자리를 그대로 둡니다.

**그래도 안 되면 확인할 것:**
1. 유튜브 스튜디오 → 설정 → 채널 → 고급 설정 → 퍼가기 허용 여부
2. 개별 영상의 "퍼가기 허용" 체크 여부
3. 채널 업로드 재생목록 ID = `UC` → `UU` 치환 (`UC1qT1...` → `UU1qT1...`)

---

## 9. 남은 작업 (우선순위)

### A. 즉시 가능
- [ ] `elimire.com` 도메인 조회 → 불가 시 대안 순차 확인
- [ ] **도메인 확정 후 주소 5곳 일괄 수정** — `index.html`의 `og:url` · `og:image` · `canonical` · JSON-LD의 `url`/`logo`, 그리고 `sitemap.xml` · `robots.txt`. (`index.html` 상단 주석에 위치를 적어두었습니다)
- [ ] GitHub Pages 배포 설정 + `CNAME` 파일 생성 (도메인 확정 후)
- [ ] 구글 폼 5종 생성 후 CONFIG에 URL 입력
  - 문의하기 / 새가족 등록 / 심방 신청 / 기도 제목 / 기부금영수증(비공개)
  - **기부금영수증 폼은 주민번호 등 민감정보를 다루므로 응답 시트 접근 권한 제한 필수**
- [ ] 문의 접수 시 이메일 알림 설정 (구글 폼 기본 기능)
- [ ] 네이버 웹마스터도구 · 구글 서치콘솔 등록 → `<head>`의 `naver-site-verification` / `google-site-verification` 값 입력
- [ ] 네이버 지도 / 구글 지도 업체 등록

### B. 자료 도착 후
- [ ] **교회 사진** — 외관, 예배당 내부, 주일예배, 성도 교제, 주요 행사
  - 갤러리 3자리, 선교 현장 1자리가 비어 있습니다
  - 초상권 확인 안 된 사진은 얼굴이 크게 드러나지 않게 처리 (목사님 요청)
  - 미성년자 포함 사진은 별도 확인 후 사용
- [ ] **담임목사 약력** — 학력, 신학교, 안수 연도, 사역 이력 → 사진 아래 프로필 블록 신설
- [ ] **선교 자료** — 선교사 정보, 사역 내용, 현장 사진, 기도 제목, 선교헌금 계좌 분리 여부
- [ ] **공지 초기 콘텐츠 3~5건**
- [ ] **FAQ 문항**
- [ ] **OG 이미지 교체 검토** — 현재 `og-image.jpg`는 로고와 슬로건으로 만든 임시본입니다. 교회 사진이 도착하면 사진 기반으로 다시 만드는 편이 좋습니다.

### C. 중장기
- [ ] 카카오톡 알림톡 연동 (문의 접수 시 담당자 알림)
- [ ] 개인정보처리방침 페이지
- [ ] AI 말씀 안내 1단계 (설교 검색·요약) — 서버리스 함수 필요, 별도 설계

---

## 10. 미해결 확인 사항

작업 전 교회에 확인이 필요한 항목입니다. **임의로 판단하지 말 것.**

1. **예배 시간 불일치**
   - 교회 간판: 주일 오전 11시 / 오후 2시, 수요 오후 7시 30분
   - 회신 자료(현재 사이트 적용): 주일 1부 10:40 / 2부 13:30, 수요 오후 8시
   - → 어느 쪽이 현재 시간표인지 확인 필요
   - 참고: 카운트다운용 `SERVICES` 배열에 주일 2부(13:30)가 빠져 있던 것을 채워 넣었고, "아침기도회"로 되어 있던 이름을 화면 표기와 같은 **"새벽기도회"** 로 통일했습니다. 실제 명칭이 무엇인지 확인 부탁드립니다.

2. **반주자 성함** — 회신서 "유서영" / 밴드 소개 "유소영" (현재 **유소영** 적용)

3. **문의 수신 이메일** — 도메인 확정 후 Google Workspace 가입 예정. 현재 미정.

4. **AI 기능 사용료** — 3단계 진입 시 API 비용 발생. 1~2단계는 비용 없음.

5. **계약 조건 7가지** — 목사님이 회신서에서 질의한 항목 (도메인·서버 명의, 권한 이관, 제작비·유지비, 백업, 개인정보처리방침·2FA, AI 사용료, 타 업체 이전 가능 여부). 담당자가 답변할 사안.

6. **AI 말씀 안내 로드맵 섹션이 빠진 것이 의도인지 확인 필요** — 이전 버전에는 있었고 영문 번역(`ai.*` 키 18개)도 그대로 남아 있는데, 현재 HTML에는 해당 섹션이 없습니다. 인사말(`about.p4`)에는 여전히 AI 언급이 있습니다.
   - 의도적으로 뺀 것이면 → `EN` 객체의 `ai.*` 키를 지워도 됩니다
   - 실수로 빠진 것이면 → 한글 원문을 다시 받아서 복원해야 합니다 (영문에서 역번역하지 말 것 — 작업 원칙 1번)

---

## 11. 자주 하는 작업 방법

**섹션 추가**
1. HTML에 `<section class="pad" id="…">` 추가 (기존 `.head` + `.kick` 패턴 사용)
2. CSS는 `/* reveal */` 주석 바로 앞에 추가
3. `data-i18n` 키를 `EN` 객체에 동일하게 추가
4. 네비게이션 링크 2곳(헤더 `#menu`, 푸터 바로가기) 모두 추가

**이미지 추가**
```python
import base64
b64 = base64.b64encode(open('사진.jpg','rb').read()).decode()
# <img src="data:image/jpeg;base64,{b64}">
```
웹 최적화 후 삽입할 것 (가로 760~880px, JPEG 품질 80~84).

**다국어 키 검사 (누락 + 중복)**
```python
import re
from collections import Counter
h = open('index.html', encoding='utf-8').read()
used = set(re.findall(r'data-i18n="([^"]+)"', h))
body = re.search(r'const EN\s*=\s*\{(.*?)\n\};', h, re.S).group(1)
keys = re.findall(r'"([\w.]+)"\s*:', body)
print('누락:', sorted(k for k in used if k not in set(keys)))
print('중복:', {k: c for k, c in Counter(keys).items() if c > 1})
```
> 중복 키는 뒤에 쓴 값이 앞을 덮어써서 **엉뚱한 영문이 표시되는 조용한 버그**를 만듭니다. 반드시 0건이어야 합니다.

**JS 문법 검사**
```bash
python3 -c "h=open('index.html',encoding='utf-8').read(); open('t.js','w').write(h.split('<script>')[1].split('</script>')[0])"
node --check t.js
```

**JSON-LD 검사**
```python
import re, json
h = open('index.html', encoding='utf-8').read()
json.loads(re.search(r'<script type="application/ld\+json">(.*?)</script>', h, re.S).group(1))
```

---

## 12. 참고

- 디자인 레퍼런스: `https://koreayjk.github.io/aim/church/fbcn`
  (레이아웃·컴포넌트 구조를 차용, 색상만 초록 → 군청으로 치환)
- 로고: ELIMIRE 워드마크 + "Presbyterian church". 군청/화이트 두 버전이 base64로 내장되어 있음
- 온라인 채널
  - 유튜브: `https://www.youtube.com/channel/UC1qT1lENffxt_Hczg9_-CeQ`
  - 밴드: `https://band.us/@elimire`
  - 블로그: `https://blog.naver.com/paulkang0411`
- 이 저장소는 원래 빌리프랜즈미션네트워크(`bfworld.net`) 사이트였습니다. 해당 파일은 커밋 `abb3f6c` 이전 기록에 남아 있습니다.

---

*최종 수정: 2026-08-08*
