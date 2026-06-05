블로그 포스트의 영문 번역본(.en.md)을 생성합니다.

## 진행 순서

**1단계 - 번역할 포스트 결정:**
- 인자가 제공된 경우: 해당 슬러그를 사용합니다.
- 인자가 없는 경우: Bash 툴로 `content/posts/` 를 스캔해 `.en.md`가 없는 `.md` 파일 목록을 찾아 AskUserQuestion으로 선택하게 합니다.

**2단계 - 원본 파일 읽기:**
`content/posts/{slug}.md` 를 Read 툴로 읽습니다.

**3단계 - 번역:**
아래 규칙을 따라 번역합니다:
- frontmatter의 `title`: 영문으로 번역
- frontmatter의 `date`, `draft`: 그대로 유지
- frontmatter의 `tags`: 영문으로 번역 (예: `["개발"]` → `["development"]`)
- 본문: 전체 영문 번역. 코드 블록 내용은 번역하지 않고 그대로 유지. 코드 블록 위아래 설명 텍스트만 번역.

**4단계 - 파일 생성:**
Write 툴로 `content/posts/{slug}.en.md` 에 저장합니다.

번역 완료 후 파일 경로와 번역된 제목을 보여주고, `hugo server` 로 미리보기 가능함을 안내합니다.
