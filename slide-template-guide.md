# 슬라이드 템플릿 가이드 (v1.1)

> 대상 파일: `slide-template.html`
> 이 문서의 목적: 이 템플릿을 처음 보는 사람(또는 Claude)이 내용만 바꿔서 바로 발표자료를 완성할 수 있게 안내한다.

---

## 0. 이게 뭔가요

`slide-template.html` 하나만 있으면 끝나는 16:9 슬라이드 발표자료 틀이다.

- 빌드 도구·서버·인터넷 연결 없이 더블클릭으로 브라우저에서 바로 열린다
- 방향키·스와이프·화면 우측 상단 버튼으로 슬라이드 넘기기
- 슬라이드를 넘길 때 카드/표/단계가 순서대로 나타나는 애니메이션 내장
- `Cmd/Ctrl + P`로 그대로 PDF 인쇄 가능 (슬라이드 1장 = PDF 1페이지)

---

## 1. 가장 빠른 사용법 — Claude에게 통째로 맡기기

다른 사람에게 이 템플릿을 줄 때는 `slide-template.html` + 이 가이드(`slide-template-guide.md`) 두 파일을 같이 전달한다. 그 사람은 Claude(또는 Claude Code)에게 이렇게 요청하면 된다:

```
첨부한 slide-template.html을 템플릿으로 써서 발표자료를 만들어줘.
slide-template-guide.md에 클래스 사용법이 정리되어 있으니 그 규칙을 그대로 따라줘.
내용은 다음과 같아: [여기에 발표 주제, 슬라이드별로 하고 싶은 말을 자유롭게 적기]
```

Claude는 템플릿 안의 `[ ]` 괄호로 된 자리표시자 텍스트를 실제 내용으로 바꾸고, 필요하면 슬라이드를 더 추가하거나 빼는 식으로 작업하면 된다. **CSS·JS는 그대로 두고 `<body>` 안의 `<div class="slide">...</div>` 블록만 수정/추가/삭제하는 것이 원칙이다.**

---

## 2. 파일 구조

```html
<head>
  <style> ... 디자인 시스템 전체 (색·폭트·애니메이션·인쇄 규칙) ... </style>
</head>
<body>
  <div id="deck"><div class="stage">
    <div class="slide active">...SLIDE 00 표지...</div>
    <div class="slide">...SLIDE 01...</div>
    <div class="slide">...SLIDE 02...</div>
    ... (필요한 만큼 반복) ...
    <div class="slide">...SLIDE 07 — 구분 장 예시...</div>
    <div class="slide">...마무리 인사...</div>
  </div></div>
  <div id="progress"></div>      ← 상단 진행률 바 (자동)
  <div id="nav">...</div>        ← 우측 상단 이전/다음 버튼 (자동)
  <script> ... 내비게이션·진행률·페이지번호 자동 계산 ... </script>
</body>
```

**슬라이드를 추가/삭제해도 번호를 손으로 고칠 필요가 없다.** `<script>`가 로드 시점에 `.slide` 개수를 세서 우측 상단 카운터·상단 진행률 바·각 슬라이드 좌상단의 "n / 전체" 표시를 전부 자동으로 채운다. 기본값은 **전체를 한 줄로 세는 단일 카운터**다 (표지부터 마무리까지 모두 포함해 1/9, 2/9 ...). 발표가 길어서 "본편 + 부록"처럼 섹션을 나누고 싶다면 8절을 참고한다.

---

## 3. 새 슬라이드를 추가하는 법

1. 기존 `<div class="slide">...</div>` 블록 하나를 통째로 복사한다 (제일 비슷한 패턴 골라서)
2. 원하는 위치에 붙여넣는다 — **순서 = `<body>`에 쓰인 순서**가 곧 발표 순서다
3. `<div class="sn">SLIDE 0X</div>`의 번호만 새로 매긴다 (뒤에 자동으로 붙는 "· n / 전체"는 그대로 둔다 — 스크립트가 채워줌)
4. 내용을 채운다

표지(SLIDE 00)와 마무리 인사 슬라이드는 `.sn`이 없는 대신 `.cv-foot`(좌하단 발표자 정보 / 우하단 날짜)을 쓴다. 이 두 슬라이드는 페이지 번호가 `.cv-foot-r` 안에 자동으로 들어간다.

---

## 4. 자주 쓰는 클래스 목록

| 클래스 | 용도 | 템플릿에서 쓴 위치 |
|---|---|---|
| `.si` | 슬라이드 안쪽 전체 (헤더+본문+TIP을 감싸는 필수 컨테이너) | 모든 슬라이드 |
| `.sn` | 좌상단 "SLIDE 0X" / "APPENDIX A1" 라벨 | 모든 콘텐츠 슬라이드 |
| `.h2` + `<em>` | 슬라이드 제목 (한 줄 기준, `<em>`으로 감싸면 골드색 강조) | 모든 콘텐츠 슬라이드 |
| `.hsub` | 제목 아래 한 줄 부제 | 모든 콘텐츠 슬라이드 |
| `.sb` | 본문 영역 (남는 공간을 전부 차지, 항상 가운데 정렬) | 모든 콘텐츠 슬라이드 |
| `.tip` | 맨 아래 핵심 한 줄 박스 — **반드시 `.sb`의 형제 요소로, `.sb` 안에 넣지 말 것** | 모든 콘텐츠 슬라이드 |
| `.g3` / `.g2` | 3칸/2칸 그리드 | SLIDE 01, 05 |
| `.card` + `.c-gold`/`.c-teal`/`.c-coral`/`.c-purple`/`.c-blue` | 카드 + 상단 강조선 색 | SLIDE 01 |
| `.bdg` | 작은 알약형 배지 | SLIDE 01, 04 |
| `.flow` + `.fbox` + `.farr` | 가로 단계 흐름 (박스-화살표-박스) | SLIDE 02 |
| `.steps` + `.step` + `.sdot` + `.stxt .st`/`.sd` | 번호 동그라미 + 세로 단계 리스트 | SLIDE 03 |
| `.tbl` | 비교 표 (헤더·셀 모두 자동 가운데 정렬) | SLIDE 04 |
| `.tip-card` + `.tc-t`/`.tc-d`/`.tc-b` | 그리드 안에 들어가는 팁 카드 | SLIDE 05 |
| `.port-box` + `.pb-t`/`.pb-i` | 라벨 + 대시(—) 리스트 박스 | (필요시 사용) |
| `a.lnk` | 링크 텍스트 색·밑줄 | SLIDE 05 |
| `.tags` + `.tag` | 표지의 알약형 키워드 태그 | SLIDE 00 |

**색상 톤 규칙:** 이 디자인은 골드/오렌지(`#EF9F27`, `#BA7517`) 단일 톤이 기본이다. `.c-teal`/`.c-coral`/`.c-purple`/`.c-blue`는 정말 다른 카테고리를 구분해야 할 때만 1~2곳 제한적으로 쓰고, 같은 슬라이드 안에서 의미 없이 여러 색을 섞지 않는다 (안 그러면 전체 가이드 톤과 어긋나 보인다).

---

## 5. 애니메이션 클래스 — 언제, 어떻게 쓰나

| 클래스 | 효과 | 붙이는 위치 |
|---|---|---|
| `.reveal-row` | 자식 요소들이 0.06초씩 순서대로 나타남 | 카드/그리드/표를 감싸는 **부모**에 붙임 |
| `.reveal-flow` | 첫 박스 먼저, 이후 "화살표+다음박스"가 0.5초마다 한 쌍씩 같이 나타남 | 가로 단계 흐름(`.flow`)을 감싸는 **부모**에 붙임 |
| `.tline` | 줄 단위로 위에서부터 써내려가듯 나타남 | 지침/코드 전문처럼 **줄마다 직접** 붙이고 `animation-delay`를 0.2초씩 늘려가며 지정 |
| `.emph-blink` | 5초간 깜빡이다 멈춤 (무한반복 아님) | 슬라이드당 **딱 한 곳**, 진짜 중요한 단어/숫자에만 |
| `.glow-link` | 5초간 은은하게 빛났다가 멈춤 | 눈에 잘 안 띄는 링크에 추가로 붙여서 시선을 끔 |

**주의:** `.emph-blink`를 한 슬라이드에 여러 곳 쓰면 정작 중요한 곳이 묻혀서 효과가 떨어진다. 슬라이드당 1개로 제한할 것.

---

## 6. 인쇄 / PDF로 내보내기

- 브라우저에서 `Cmd+P`(Mac) / `Ctrl+P`(Win) → "PDF로 저장"
- `@media print` 규칙이 이미 들어있어서: 내비게이션·진행률 바는 자동으로 숨겨지고, 슬라이드 1장이 PDF 1페이지가 된다
- 인쇄 대화상자에서 **용지 크기를 "기본값(Default)"으로 두면** 16:9 비율(13.333in × 7.5in)에 맞춰 위아래 여백 없이 꽉 채워진다. A4/Letter로 직접 바꾸면 비율이 달라져 여백이 생길 수 있다

---

## 7. 완성 후 체크리스트

- [ ] 모든 `[괄호]` 자리표시자를 실제 텍스트로 바꿨는가
- [ ] 슬라이드 제목(`.h2`)이 한 줄을 넘기지 않는가 (`white-space:nowrap`이 기본이라 너무 길면 화면 밖으로 잘릴 수 있음 — 길면 `font-size`를 줄이거나 줄바꿈 허용)
- [ ] 카드/표 안의 글자가 가운데 정렬되어 있는가 (대시 `—` 로 시작하는 리스트만 예외적으로 좌측 정렬)
- [ ] 슬라이드 내용이 박스 밖으로 넘치지 않는가 — 브라우저 창을 줄여보거나, 실제 인쇄 미리보기로 한 번 확인
- [ ] `.emph-blink`가 슬라이드당 1곳을 넘지 않는가
- [ ] 외부 링크(`<a href>`)에 `target="_blank" rel="noopener"`를 넣었는가
- [ ] 표지/마무리 슬라이드의 발표자 이름·날짜를 실제 값으로 채웠는가
- [ ] 슬라이드 제목(`.h2`) 폰트 크기가 슬라이드마다 다르지 않은가 — 제목이 길어서 안 들어가면 **폰트를 줄이지 말고 텍스트를 줄여서** 같은 크기를 유지할 것 (크기가 들쭉날쭉하면 위계가 흐트러져 보임)

---

## 8. 응용 — 발표가 길어서 섹션을 나눌 때

본편 뒤에 "부록"이나 "참고 자료"처럼 별도 섹션을 붙이고 싶을 때 쓰는 패턴이다. 두 가지를 같이 적용한다.

**1) 구분 장(divider) 슬라이드로 전환을 표시한다** — 템플릿의 `SLIDE 07`이 그 예시다. 표지/마무리와 같은 방식(`.h1` 공용 클래스, `.sn` 없음)으로 만들어서 톤을 맞춘다.

```html
<div class="slide">
  <div class="glow"></div>
  <div class="si">
    <div class="sb" style="justify-content:center; align-items:center; text-align:center;">
      <div style="font-size:0.92vw; font-weight:500; color:#BA7517; letter-spacing:4px; margin-bottom:2.8%;">[상단 라벨]</div>
      <div class="h1" style="margin-bottom:2.2%;">[섹션 제목]</div>
      <div style="font-size:1.25vw; color:rgba(255,255,255,0.42);">[한 줄 설명]</div>
    </div>
  </div>
</div>
```

**2) 섹션마다 번호를 독립적으로 세고 싶다면** `data-group` 속성 + 그룹별 카운터로 바꾼다. 본편 슬라이드에는 `data-group="main"`, 부록 슬라이드에는 `data-group="appendix"`를 붙이고, 기존 페이지번호 주입 스크립트를 아래로 교체한다:

```js
['main', 'appendix'].forEach(group => {
  const groupSlides = Array.from(slides).filter(s => s.dataset.group === group);
  groupSlides.forEach((slide, i) => {
    const label = (i + 1) + ' / ' + groupSlides.length;
    const sn = slide.querySelector('.sn');
    if (sn) sn.insertAdjacentHTML('beforeend', ' <span class="pn">· ' + label + '</span>');
  });
});
// 구분장처럼 그룹이 없는 슬라이드는 전체 절대 위치를 그대로 보여줌
slides.forEach((slide, i) => {
  if (slide.dataset.group) return;
  const label = (i + 1) + ' / ' + total;
  const sn = slide.querySelector('.sn');
  if (sn) sn.insertAdjacentHTML('beforeend', ' <span class="pn">· ' + label + '</span>');
});
```

이렇게 하면 본편은 "SLIDE 05 · 5/5"로 깔끔하게 마감되고, 부록은 다시 "APPENDIX A1 · 1/3"부터 독립적으로 세진다 — 본편 분량이 바뀌어도 부록 번호가 같이 밀리지 않는다. (`claude-code-guide.html`에서 실제로 쓰인 패턴이다. `tech-guide.md` 5-3절에 더 자세한 설명이 있다.)

---

## 9. 변경 히스토리

### v1.0 — 최초 작성
- `claude-code-guide.html` 제작 과정에서 검증된 디자인 시스템·애니메이션·인쇄 규칙을 범용 템플릿으로 추출

### v1.1 — 구분 장 + 그룹별 번호 패턴 추가
- `SLIDE 07` 구분 장 예시 슬라이드 추가 (8장 → 9장)
- 8절에 "섹션 나누기" 응용법 추가 — 구분 장 마크업 + `data-group` 기반 그룹별 독립 번호 매기기
- 체크리스트에 제목 폰트 크기 통일 항목 추가
