# 프로젝트 메모 — Claude Code 가이드 슬라이드

## 이게 뭔가요
`claude-code-guide.html` — "AI와 함께 일하는 법: Claude Code" 강의용 16:9 슬라이드 덱. 단일 HTML 파일, 빌드 없이 더블클릭으로 바로 열림.

## 파일 구조
- `claude-code-guide.html` — 실제 발표 슬라이드 (24장)
- `claude-code-guide.pdf` — 위 파일을 그대로 인쇄한 배포용 PDF
- `tech-guide.md` / `.pdf` — 이 덱의 기술 스택·CSS/JS 패턴·작업 유의사항 (가장 자세한 문서, 옵시디언용)
- `slide-template.html` — 범용 재사용 템플릿 (9장, 이 덱에서 검증된 패턴만 추출)
- `slide-template-guide.md` / `.pdf` — 템플릿 사용법
- `Sample_PORTFOLIO_TEMPLATE_GUIDE.md` — 실습 1(포트폴리오 만들기)에서 참조하는 별도 자료

## 현재 슬라이드 구조 (24장)
```
SLIDE 00 표지
SLIDE 01~10 본편          ← data-group="main"   — 독립 번호 1/10~10/10
실습 구분 장
실습 1~2                  ← data-group="practice" — 독립 번호 1/2~2/2
Appendix 구분 장
APPENDIX A1~A7 (A3는 1/2·2/2 두 장) ← data-group="appendix" — 독립 번호 1/8~8/8
SLIDE END 마무리 인사
```
표지/구분장/마무리는 그룹이 없고 전체 절대 위치(`n/24`)로 번호가 매겨진다. 자세한 구현은 `tech-guide.md` 5-3절.

## 작업할 때 꼭 알아야 할 것
- **`.si > .sb + .tip` 구조를 반드시 지킬 것** — `.tip`은 `.sb`의 형제 요소여야 함 (안 그러면 하단 정렬이 깨짐)
- **슬라이드 순서/번호를 바꾼 뒤엔 `grep -n "Appendix A"`로 전체 검색** — 다른 슬라이드가 옛 번호를 참조하고 있을 수 있음 (이 프로젝트에서 여러 번 겪은 버그)
- **타이틀(`.h2`) 폰트 크기는 전체 5vw로 통일됨** — 새 슬라이드 추가 시 다른 크기 쓰지 말 것. 제목이 길면 폰트를 줄이지 말고 텍스트를 줄여서 한 줄 유지
- **PDF/git push는 사용자가 명시적으로 지시할 때만 실행** — 구조 변경 작업 중간에 자동으로 하지 않기로 합의되어 있음
- 변경 사항은 `tech-guide.md` 8절(작업 시 유의사항)·9절(변경 히스토리)에 누적 기록

## Git
원격: `https://github.com/yekim0223/class.git` (main 브랜치). 회사 컴퓨터에서도 clone해서 편집 가능. `.bkit/`, `.claude/`, `.DS_Store`는 `.gitignore`로 제외됨 (로컬 도구 상태, 커밋 대상 아님).

## 다음에 이어서 작업할 때
이 파일 + `tech-guide.md`를 먼저 읽고 시작하면 전체 구조를 빠르게 파악할 수 있다. 콘텐츠 수정은 `claude-code-guide.html`만 건드리고, 구조가 바뀌면 `tech-guide.md`도 같이 업데이트할 것.
