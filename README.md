# notion-widgets

노션 등에 임베드해서 쓰는 개인용 미니 위젯 모음입니다.
각 위젯은 **HTML 파일 하나**로 동작하며, URL 쿼리 파라미터로 내용과 색을 바꿉니다.
빌드 과정이나 서버 없이 GitHub Pages로 호스팅합니다.

```
https://chungsungyeon.github.io/notion-widgets/{위젯이름}/?key=value&key=value
```

---

## 위젯 목록

| 위젯 | 설명 | 예시 링크 |
|---|---|---|
| [dday](#dday) | 특정 날짜까지 남은 날(D-n) / 지난 날(D+n) | `/dday/?date=20270101&content=BYE2027` |
| _(추가 예정)_ | | |

---

## 폴더 구조

```
my-widgets/
├── README.md
├── dday/
│   └── index.html
└── {위젯이름}/
    └── index.html
```

- 위젯 하나 = 폴더 하나 + `index.html`
- 폴더 이름이 곧 URL 경로가 됩니다 (`/dday/` → `dday/index.html`)
- 폴더 이름은 **영문 소문자 + 하이픈**으로 씁니다 (예: `year-progress`)

---

## 공통 파라미터

모든 위젯이 **같은 이름, 같은 형식**으로 지원하는 파라미터입니다.
새 위젯을 만들 때도 이 규칙을 따릅니다.

| 파라미터 | 형식 | 기본값 | 설명 |
|---|---|---|---|
| `color` | hex, `#` 없이 (`000000`, `fff`) | `000000` | 배경색 |
| `text` | hex, `#` 없이 | `FFFFFF` | 글자색 |
| `title` | 문자열 | 위젯마다 다름 | 표시할 문구/라벨 |

> 노션 다크모드 배경과 맞추려면 `color=191919`, 라이트모드는 `color=ffffff`

---

## 파라미터 작성 규칙

- **이름**: 영문 소문자, 여러 단어는 붙여 쓰기 (`startdate`, `fontsize`)
- **날짜**: `YYYYMMDD` (예: `20270101`)
- **색**: `#` 없는 hex (URL에서 `#`은 특별한 의미라서 빼고 씀)
- **불리언**: `1` / `0` (예: `showdate=0`)
- **한글·공백·특수문자**: 브라우저 주소창에 그대로 입력해도 자동 인코딩됨.
  직접 링크를 만들 때는 인코딩 필요 (`종강` → `%EC%A2%85%EA%B0%95`, 공백 → `%20`)
- 파라미터가 없거나 형식이 틀려도 **기본값으로 정상 표시**되어야 함

---

## 새 위젯 추가하기

1. `{위젯이름}/` 폴더를 만들어서, 안에 `index.html` 파일을 넣는다.
2. `index.html`에서 위젯 전용 파라미터와 그리는 로직을 작성합니다
3. 브라우저로 파일을 직접 열어서 확인합니다
   (`file:///.../year-progress/index.html?color=191919&text=eeeeee`)
4. push → 1~2분 후 `https://chungsungyeon.github.io/notion-widgets/year-progress/` 에서 확인

### 위젯 코드 체크리스트

- [ ] `URLSearchParams`로 파라미터를 읽고, 모든 값에 기본값이 있음
- [ ] 색 값은 hex 정규식으로 검사 후 적용
- [ ] 사용자 입력 문자열은 `innerHTML`이 아니라 **`textContent`** 로 넣음
- [ ] 외부 라이브러리·API 키·개인정보 없음 (public 레포)
- [ ] 임베드 크기가 작아도 깨지지 않음 (`clamp()`, `vw` 등으로 반응형)
- [ ] 노션 라이트/다크 배경 둘 다에서 확인

---

## 배포

- **Settings → Pages → Source: Deploy from a branch → `main` / `(root)`**
- `main`에 push하면 자동 반영 (1~2분)
- 변경이 안 보이면 강력 새로고침 (`Ctrl+Shift+R` / `Cmd+Shift+R`)
- 레포는 public이어야 함 (무료 계정 기준)

## 노션에 임베드

1. 노션 페이지에서 `/embed` 입력 → **Embed** 블록 선택
2. 위젯 URL 붙여넣기
3. 블록 모서리를 드래그해서 크기 조절

---

## 위젯별 문서

### dday

특정 날짜까지 남은 날 수를 표시합니다. 당일은 `D-DAY`, 지난 날짜는 `D+n`.

| 파라미터 | 형식 | 기본값 | 설명 |
|---|---|---|---|
| `date` | `YYYYMMDD` | `20270101` | 기준 날짜 |
| `content` | 문자열 | (없음) | 숫자 아래 표시할 문구 |
| `background` | hex | `000000` | 배경색 |
| `text` | hex | `FFFFFF` | 글자색 |

```
/dday/?date=20270101&content=BYE2027&color=000000&text=CCEEFF
```
