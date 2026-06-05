---
title: "블로그 글 자동 번역하는 클로드 붙이기"
date: 2026-06-05T12:35:01+09:00
tags: ["Hugo", "Claude", "다국어"]
draft: false
---

한국어로 글을 쓰면 Claude가 읽고 영문 번역본을 만들어주는 시스템을 블로그에 붙였다. 상단 언어 버튼 하나로 한/영 전환이 된다.

## 왜 만들었나

한국어로만 글을 올리면 검색 유입이 한국어권으로만 제한된다. 그렇다고 영문 번역을 매번 직접 하기엔 번거롭다. Claude가 이미 문맥을 이해하고 있으니 번역도 시키면 된다는 생각에서 시작했다.

## 구조

Hugo는 기본적으로 다국어를 지원한다. 같은 디렉토리에 파일명만 다르게 두면 된다.

```
content/posts/
  my-post.md        ← 한국어 원본
  my-post.en.md     ← 영문 번역본 (Claude가 생성)
```

두 파일이 존재하면 PaperMod 테마가 상단에 언어 전환 버튼을 자동으로 표시한다.

## Hugo 다국어 설정

`hugo.toml`에 언어 설정을 추가했다.

```toml
defaultContentLanguage = 'ko'

[languages]
  [languages.ko]
    languageName = '한국어'
    weight = 1
  [languages.en]
    languageName = 'English'
    weight = 2
    title = 'asackan'
    [languages.en.params]
      homeInfoParams.Title = 'asackan'
      homeInfoParams.Content = 'A space to record and think'
```

기본 언어를 한국어로 설정하고, 영어를 두 번째 언어로 등록한다. PaperMod는 이 설정만으로 헤더에 언어 전환 버튼을 렌더링한다.

## `/trans` 커맨드

Claude Code 커스텀 커맨드로 `/trans`를 만들었다. 실행하면:

1. 번역되지 않은 포스트 목록을 스캔
2. 선택한 `.md` 파일을 읽음
3. Claude가 본문 전체를 영문으로 번역
4. `{slug}.en.md` 파일로 저장

코드 블록 내부는 번역하지 않고, 제목·태그·본문 텍스트만 번역한다.

## `/deploy` 업데이트

배포 전에 번역 누락 여부를 자동으로 체크하도록 `/deploy` 커맨드를 수정했다. `draft: false`인 포스트 중 `.en.md`가 없으면 경고를 띄운다.

```
⚠️  영문 번역이 없는 공개 포스트:
- auto-translate-with-claude
/trans 로 번역 후 배포를 권장합니다.
```

## 사용 흐름

```
1. 한국어로 글 작성
2. /form  → frontmatter 생성
3. /trans → 영문 번역본 생성
4. /deploy → 배포 (번역 누락 시 경고)
```

번역본 퀄리티는 Claude가 문맥을 파악하기 때문에 단순 기계 번역보다 낫다. 기술 용어나 코드 관련 설명도 자연스럽게 처리된다.
