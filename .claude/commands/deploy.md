블로그 변경사항을 GitHub에 배포합니다.

## 진행 순서

**0단계 - 번역 완료 여부 확인:**
Bash 툴로 `content/posts/` 를 스캔합니다.
- `draft: false` 인 `.md` 파일 중 대응하는 `.en.md` 파일이 없는 것을 찾습니다.
- 미번역 포스트가 있으면 목록을 보여주고 경고합니다:
  ```
  ⚠️  영문 번역이 없는 공개 포스트:
  - {slug}
  /trans 로 번역 후 배포를 권장합니다. 그래도 진행할까요?
  ```
- 사용자가 계속 진행 의사를 밝히면 다음 단계로 넘어갑니다.

**1단계 - 변경사항 확인:**
`git status`와 `git diff --stat`을 실행해 변경된 파일 목록을 사용자에게 보여줍니다.

**2단계 - 커밋 메시지 생성:**
변경된 `content/posts/` 파일을 기준으로 커밋 메시지를 자동 생성합니다.
- 새 글이면: `post: {글 제목}`
- 글 수정이면: `update: {글 제목}`
- 번역 추가이면: `translate: {글 제목}`
- 설정 변경이면: `config: {변경 내용 요약}`

생성한 커밋 메시지를 사용자에게 보여주고 진행 여부를 확인합니다.

**3단계 - 배포:**
사용자가 확인하면 아래 순서로 실행합니다:
1. `git add` (content/, hugo.toml 등 관련 파일만 선택적으로)
2. `git commit -m "{커밋 메시지}\n\nCo-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>"`
3. `git push origin main`

push 완료 후 GitHub Actions URL(`https://github.com/asackan/asackan.github.io/actions`)을 안내하고, 약 1~2분 후 `https://asackan.github.io/` 에 반영된다고 알립니다.
