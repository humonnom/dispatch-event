# dispatch-event

## 개요

이 GitHub Action은 포크된 저장소에 `main_updated` 이벤트를 트리거하는 간단하고 효과적인 도구입니다. 자동 동기화를 위한 도구입니다.

## 기능

- fork된 레포에 `main_updated` 깃헙 이벤트를 발생시킵니다.
- fork된 레포에서 이 이벤트를 받아서 처리하는데에 [humonnom/sync-fork](https://github.com/marketplace/actions/auto-sync-forked-repo)를 사용할 수 있습니다.
- [humonnom/sync-fork](https://github.com/marketplace/actions/auto-sync-forked-repo)액션과 함께 사용하면 저장소 동기화 메커니즘을 간편하게 구현할 수 있습니다.

## 입력 파라미터

| 파라미터 | 설명 | 필수 여부 |
|----------|------|-----------|
| `forked_repository_owner` | 포크된 저장소의 소유자 | **필수** |
| `forked_repository_name` | 포크된 저장소의 이름 | **필수** |
| `forked_repository_token` | 포크된 저장소의 개인 액세스 토큰 | **필수** |

## 사용 예시

```yaml
- name: Sync Forked Repository
  uses: humonnom/dispatch-event@v0.1.0-alpha.2
  with:
    forked_repository_owner: 'target-owner'
    forked_repository_name: 'target-repo'
    forked_repository_token: ${{ secrets.REPO_TOKEN }}
```

## 워크플로우 예제

```yaml
name: Sync Main Branch

on:
  push:
    branches: [ main ]

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Trigger Sync Event
        uses: humonnom/dispatch-event@v0.1.0-alpha.2
        with:
          forked_repository_owner: 'target-owner'
          forked_repository_name: 'target-repo'
          forked_repository_token: ${{ secrets.REPO_TOKEN }}
```

## 주의사항

- 개인 액세스 토큰은 반드시 GitHub Secrets에 저장
- 토큰은 포크된 저장소에 대한 `repo` 스코프 권한 필요
   - Actions: Read & write
   - Contents: Read & write
   - Metadata: Read-only(mendatory)
- 이벤트 트리거는 GitHub API를 통해 수행됨

## 라이선스

MIT 라이선스를 따릅니다.

## 기여

이슈 또는 풀 리퀘스트를 통해 개선 제안 가능합니다.
