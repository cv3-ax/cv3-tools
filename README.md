# cv3-tools — CV3-AX 사내 Claude Code 플러그인 마켓플레이스

사원들이 **이 repo 하나만 추가**하면 사내 플러그인들을 설치할 수 있습니다. 앞으로 플러그인이 늘어나면
[`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) 의 `plugins` 배열에 항목만 추가하면 됩니다.

- private repo이므로 사용자 GitHub 계정에 **SSH 공개키 등록** + `cv3-ax` 조직 **read 권한**이 필요합니다.
  (이 마켓플레이스 repo와, 각 플러그인 소스 repo 양쪽에 read 필요 — 둘 다 cv3-ax 조직이라 팀 read면 충족)

## 설치 (각 PC 1회)

```
/plugin marketplace add git@github.com:cv3-ax/cv3-tools.git
/plugin install sql-builder@cv3-tools
```

> 💡 플러그인 소스(`github` 타입)를 SSH로 일관되게 받아오려면, 한 번만:
> ```
> git config --global url."git@github.com:".insteadOf "https://github.com/"
> ```
> (github https 요청을 SSH로 자동 치환 → private repo도 SSH 키로 인증)

## 업데이트
```
/plugin marketplace update cv3-tools
```

## 수록 플러그인

| 플러그인 | 소스 | 설명 |
|----------|------|------|
| `sql-builder` | [`cv3-ax/sql-builder-plugin`](https://github.com/cv3-ax/sql-builder-plugin) | 자연어 → 읽기 전용 SQL → CSV (MySQL). `/sql-setup` → `/sql-query` |

## 새 플러그인 추가 방법
`.claude-plugin/marketplace.json` 의 `plugins` 에 추가:
```json
{
  "name": "<플러그인명>",
  "source": { "source": "github", "repo": "cv3-ax/<플러그인repo>" },
  "description": "..."
}
```
커밋·푸시하면 사원들은 `/plugin marketplace update cv3-tools` 로 목록을 갱신받습니다.

---
- 플러그인 소스는 `cv3-ax` 조직 repo(회사 관리)를 가리킵니다. 각 플러그인의 개인 canonical(예: 개발자 개인 repo)과는 별개로, 회사 배포용은 여기서 관리합니다.
