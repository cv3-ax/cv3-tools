# 초기 세팅 가이드 (관리자·개발자용)

플러그인을 사내에 배포하기 위한 **마켓플레이스 1회 세팅**입니다. (개별 플러그인 사용법은 각 플러그인 repo 참고 — 예: [sql-builder-plugin](https://github.com/cv3-ax/sql-builder-plugin))

## 구조
```
kei781/sql-builder-plugin    개인 canonical (제작자 소유·관리)
      │ fork
      ▼
cv3-ax/sql-builder-plugin    조직 fork = 플러그인 소스 (회사 관리)
      ▲ github source
cv3-ax/cv3-tools             조직 마켓플레이스 = 사원 진입점  ← 이 repo
```
- 셋 다 **private**. repo에는 플러그인 코드만. DB 접속정보·스키마는 각 PC 로컬(`~/.sql-builder/`)에만 저장.

## 0. 사전
- `cv3-ax` 조직 **owner/admin** 권한
- (연속성) 조직 owner를 **2명 이상** 두기를 권장 — 한 명뿐이면 그 사람 부재 시 조직이 무주공산

## 1. 조직 접근 권한 (팀 read)
1. GitHub → `cv3-ax` → **Teams** → 팀 생성(예: `plugin-users`)
2. 팀에 사용자 초대
3. 팀에 **Read** 권한 부여 — 대상 repo **둘 다**: `cv3-tools`, `sql-builder-plugin`
   - 각 repo → Settings → Collaborators and teams → Add team → Role: **Read**

→ 이후 팀원은 자기 PC의 SSH 키만 GitHub 계정에 등록하면 자동으로 접근됩니다.

## 2. 각 PC 설치 (개발자가 도움)
마켓플레이스 추가 + 플러그인 설치. 요약:
```
git config --global url."git@github.com:".insteadOf "https://github.com/"
/plugin marketplace add git@github.com:cv3-ax/cv3-tools.git
/plugin install sql-builder@cv3-tools
```
`sql-builder-plugin` repo의 `deploy/onboard.ps1`(Windows) / `deploy/onboard.sh`(mac·linux) 로 자동화 가능.

## 3. 업데이트 흐름
1. 제작자가 개인 repo(`kei781/sql-builder-plugin`)에 개선 push
2. 회사가 **fork 동기화**: `cv3-ax/sql-builder-plugin` GitHub 페이지 → **Sync fork** 클릭 (또는 아래 자동화)
3. 사원: `/plugin marketplace update cv3-tools`

### (선택) fork 자동 동기화 Action
`cv3-ax/sql-builder-plugin` 에 `.github/workflows/sync.yml`:
```yaml
name: sync-upstream
on:
  schedule:
    - cron: "0 * * * *"      # 매시 정각
  workflow_dispatch:
jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - run: gh repo sync cv3-ax/sql-builder-plugin --source kei781/sql-builder-plugin --branch main
        env:
          GH_TOKEN: ${{ secrets.SYNC_TOKEN }}
```
> `SYNC_TOKEN`: **kei781/sql-builder-plugin read + cv3-ax/sql-builder-plugin write** 가능한 PAT를 repo secret으로 등록.
> (제작자 개인 repo 접근이 필요하므로, 제작자가 발급한 fine-grained PAT 사용을 권장)

## 4. 새 플러그인 추가
이 repo의 `.claude-plugin/marketplace.json` → `plugins[]` 에 추가 후 push:
```json
{ "name": "<플러그인명>", "source": { "source": "github", "repo": "cv3-ax/<플러그인repo>" }, "description": "..." }
```
사원은 `/plugin marketplace update cv3-tools` 로 새 목록을 받습니다.

## 보안 메모
- 모든 repo private. 마켓플레이스/플러그인 코드에 **비밀 없음**.
- 각 사용자 DB 계정·엔드포인트·스키마는 그 PC의 `~/.sql-builder/` 에만 저장(외부 전송 없음).
- 플러그인 실행은 **읽기 전용**(SELECT/WITH만, READ ONLY 트랜잭션, 타임아웃).
