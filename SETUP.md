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

5. **업데이트 (새 버전이 공지될 때마다)**:
   ```
   /plugin marketplace update cv3-tools
   ```
   이후 `/plugin` → 설치된 `sql-builder` 선택 → **Update**. (터미널에서는 `claude plugin update sql-builder@cv3-tools`)
   반영 후 Claude Code를 재시작하세요.

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

### 플러그인 새 버전 배포 시

업데이트는 **버전 번호로만 감지**됩니다(커밋이 앞서가도 버전이 같으면 "already at the latest").

1. 플러그인 repo에서 `.claude-plugin/plugin.json` 의 `version` 을 올려 main 에 머지.
2. 이 repo `.claude-plugin/marketplace.json` 의 해당 플러그인 `version` 을 **같은 값으로** 맞추고 `metadata.version` 도 올림.
3. 커밋 & 푸시 → 팀원들에게 위 **5. 업데이트** 절차 전파.
