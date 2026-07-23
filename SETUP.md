# cv3-tools 마켓플레이스 가이드

## 1. 마켓플레이스 사용법 (설치)

1. **bot 계정 등록(전역 최초 1회)** — 사내 GitHub bot(서비스) 계정을 `cv3-ax` 조직에 **read** 로 추가.
2. **bot 공개키 등록(전역 최초 1회)** — bot SSH 키쌍의 **공개키**를 bot 계정 GitHub(Settings → SSH and GPG keys)에 등록.
3. pc에 `.ssh/`에 bot등록한 SSH 개인 키 세팅 **(pc당 1회)**
4. **Claude Code에서 등록·설치**:

   마켓플레이스 등록 **(pc당 1회)**:
   ```
   /plugin marketplace add git@github.com:cv3-ax/cv3-tools.git
   ```
   플러그인 설치 **(플러그인 추가시마다 1회)**:
   ```
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
