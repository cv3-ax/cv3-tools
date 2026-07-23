# cv3-tools 마켓플레이스 가이드

## 1. 마켓플레이스 사용법 (설치)

1. **bot 계정 등록** — 사내 GitHub bot(서비스) 계정을 `cv3-ax` 조직에 **read** 로 추가.
2. **bot에 SSH 키 등록** — 그 PC의 SSH 공개키를 bot 계정 GitHub에 등록.
3. Claude Code에서:
   ```
   /plugin marketplace add git@github.com:cv3-ax/cv3-tools.git
   /plugin install sql-builder@cv3-tools
   ```

> 💡 플러그인 소스(private)를 SSH로 받도록 한 번만: `git config --global url."git@github.com:".insteadOf "https://github.com/"`

## 2. 마켓플레이스에 플러그인 등록법

1. `.claude-plugin/marketplace.json` 의 `plugins` 에 추가:
   ```json
   {
     "name": "<플러그인명>",
     "source": { "source": "github", "repo": "cv3-ax/<플러그인repo>" },
     "description": "..."
   }
   ```
2. **커밋 & 푸시** → 팀원들에게 **`/plugin marketplace update cv3-tools`** 실행하라고 전파.
