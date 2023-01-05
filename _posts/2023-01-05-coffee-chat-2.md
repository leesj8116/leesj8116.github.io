---
title: "git branch 전략, 그리고 merge 방식"
categories: "posts"
tags:
  - git
  - 공유
---

## git branch 전략에 대해서

[git 브랜치 전략에 대해서](https://tecoble.techcourse.co.kr/post/2021-07-15-git-branch/)

### 목록

- Git flow : 5개의 branch로 관리 (feature, develop, release, hotfix, master)
- Github flow : 1개의 branch로 관리 (master)
- gitlab flow : 3개의 branch로 관리 (master, feature, prodution)

## Git Merge

[Git Merge](https://tecoble.techcourse.co.kr/post/2022-10-23-git-merge/)

### ****fast-forward merge vs non fast-forward merge****

- fast-forward merge : 기존의 변경 사항이 없을 때, head만 옮겨서 merge 시키기
- non fast-forward merge : 서로간의 커밋을 모두 반영한 새로운 커밋 생성

### github의 merge 제공 방식

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6ea04105-608b-48d5-a87c-70ac6436df4a/Untitled.png)

- Create a merge commit : 위의 non fast-forward merge
- Squash and merge : 각각의 커밋을 하나의 커밋으로 묶어서 merge 시키기
    - 이점 : 메인 브런치에서 큰 흐름을 관리하기 좋음 (세세한 커밋은 삭제되고 merge 되기 때문에)
- Rebase and merge : 메인 브런치의 수정 사항을 반영한 뒤 나의 결과물을 merge
    - 이점 : 최신 결과물을 반영하면서 작업하기 좋음

## gitmoji

[gitmoji](https://gitmoji.dev/)

제목 앞에 성격에 해당하는 이모티콘을 붙여, 커밋 내용을 확인하는데 도움을 줍니다

### 예시
정확하지는 않지만, 서로 협의하여 어떠한 이모티콘을 붙일지 정하면 끝
- 🌱 : 프로젝트 생성 후 생긴 파일들 같은 기초 파일 추가
- 💡 : 주석이 추가/수정되었을 때
- 💬 : 텍스트/리터럴이 추가/수정되었을 때
- 🐛 : 버그 수정
- 🔥 : 파일 또는 코드 삭제
- ➕ / ➖ : 라이브러리 추가 / 삭제
- ⬆️ / ⬇️ : 라이브러리 버전 업그레이드 / 다운그레이드
- 📝 : Readme와 같은 문서 수정시