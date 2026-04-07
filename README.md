# Security Skills — Claude Code Plugin

당근페이 보안팀 전용 Claude Code 플러그인. Okta, Splunk, CrowdStrike Falcon, Bitdefender, Palo Alto, EHR(mate-api) 등 보안 솔루션을 자연어로 조회하고, 보안성 검토 지식베이스를 활용하는 스킬 모음.

---

## 목차

- [설치](#설치)
- [최초 인증 설정](#최초-인증-설정)
- [스킬 사용법](#스킬-사용법)
- [주의사항 — 올리면 안 되는 것](#주의사항--올리면-안-되는-것)

---

## 설치

**요구사항**: Claude Code CLI 최신 버전

```bash
# 플러그인 설치
claude plugin install security-skills/security-skills

# 설치 확인
claude plugin list
```

---

## 최초 인증 설정

인증 정보는 **로컬(`~/.security-claude/`)에만 저장**되며, 저장소에는 절대 포함되지 않는다.  
사용 전 각 솔루션별로 아래 스킬을 실행해 인증을 등록한다.

### Okta

```
/security-skills:okta-auth
```

입력 항목:
- Okta 도메인 (예: `yourcompany.okta.com`)
- API 토큰 ([Okta Admin → Security → API → Tokens](https://yourcompany.okta.com/admin/access/api/tokens)에서 발급)

저장 위치: `~/.security-claude/okta-auth.json`

---

### Splunk

```
/security-skills:splunk-auth
```

입력 항목:
- Splunk 호스트 (예: `https://splunk.yourcompany.com:8089`)
- API 토큰 또는 사용자명/비밀번호

저장 위치: `~/.security-claude/splunk-auth.json`

---

### CrowdStrike Falcon

```
/security-skills:av-falcon-auth
```

입력 항목:
- Client ID
- Client Secret
- Base URL (예: `https://api.crowdstrike.com`)

저장 위치: `~/.security-claude/falcon-auth.json`

---

### Bitdefender GravityZone

```
/security-skills:av-bitdefender-auth
```

입력 항목:
- API Key
- Control Center URL

저장 위치: `~/.security-claude/bitdefender-auth.json`

---

### Palo Alto 방화벽

```
/security-skills:paloalto-auth
```

입력 항목:
- Firewall IP/Hostname
- API Key (`https://<firewall>/api/?type=keygen`으로 발급)

저장 위치: `~/.security-claude/paloalto-auth.json`

---

### EHR (mate-api)

```
/security-skills:ehr-auth
```

입력 항목:
- mate-api Base URL
- JWT 토큰

저장 위치: `~/.security-claude/ehr-auth.json`

---

## 스킬 사용법

### Okta

| 스킬 | 설명 | 예시 |
|------|------|------|
| `okta-user-lookup` | 이메일/이름으로 사용자 검색 | "john@company.com 계정 조회해줘" |
| `okta-user-auth-analysis` | 사용자 인증 행동 분석 | "john 계정 로그인 패턴 분석해줘" |
| `okta-app-access` | 앱 접근 권한 확인 | "john이 어떤 앱에 접근 가능한지 확인해줘" |
| `okta-group-lookup` | 그룹 검색 | "Security 그룹 멤버 목록 보여줘" |
| `okta-inactive-users` | 비활성 사용자 탐지 | "90일 이상 로그인 없는 계정 찾아줘" |
| `okta-unlock` | 계정 잠금 해제/비밀번호 초기화 | "john 계정 잠금 해제해줘" |
| `okta-logs` | 시스템 로그 쿼리 | "최근 24시간 실패한 로그인 조회해줘" |

### Splunk

| 스킬 | 설명 | 예시 |
|------|------|------|
| `splunk-query` | SPL 또는 자연어로 Splunk 쿼리 | "어제 이상 로그인 시도 SPL 짜줘" |

### CrowdStrike Falcon

| 스킬 | 설명 | 예시 |
|------|------|------|
| `av-falcon-device-lookup` | 호스트 검색 | "john-macbook 호스트 정보 조회해줘" |
| `av-falcon-detections` | 탐지 기록 조회 | "오늘 탐지된 위협 목록 보여줘" |

### Bitdefender GravityZone

| 스킬 | 설명 | 예시 |
|------|------|------|
| `av-bitdefender-device-lookup` | 엔드포인트 검색 | "john 노트북 Bitdefender 상태 확인해줘" |
| `av-bitdefender-threats` | 위협 정보 조회 | "이번 주 탐지된 위협 목록 보여줘" |

### Palo Alto

| 스킬 | 설명 | 예시 |
|------|------|------|
| `paloalto-globalprotect` | GlobalProtect VPN 세션 조회 | "현재 VPN 접속 중인 사용자 목록 보여줘" |
| `paloalto-hip` | 단말 보안 상태(HIP) 확인 | "john 노트북 HIP 상태 확인해줘" |
| `paloalto-hip-objects` | HIP 객체 쿼리 | "HIP 정책 목록 조회해줘" |

### EHR (mate-api)

| 스킬 | 설명 | 예시 |
|------|------|------|
| `ehr-lookup` | 직원 정보 조회 | "john@company.com 직원 정보 조회해줘" |

### 보안성 검토

| 스킬 | 설명 | 예시 |
|------|------|------|
| `review-search` | 키워드/카테고리/REV-ID로 검토 검색 | "API 보안 검토 사례 찾아줘" |
| `review-create` | 신규 보안성 검토 수행 | "n8n 자동화 도구 보안성 검토해줘" |
| `review-query` | REV-ID로 검토 조회 | "REV-2026-004 검토 내용 보여줘" |
| `review-index-update` | 지식베이스 인덱스 갱신 | "검토 인덱스 업데이트해줘" |

---

## 주의사항 — 올리면 안 되는 것

아래 파일/디렉토리는 `.gitignore`에 등록되어 있으며, **절대 커밋하지 않는다.**

| 경로 | 이유 |
|------|------|
| `~/.security-claude/*.json` | Okta/Splunk/Falcon 등 API 토큰/자격증명 |
| `.env`, `.env.local` | 환경변수 파일 |
| `.gstack/` | 브라우저 자동화 토큰 포함 |
| `.omc/state/`, `.omc/sessions/` | 개인 세션 기록 |
| `.claude/settings.local.json` | 개인 Claude Code 로컬 설정 |

> 실수로 커밋한 경우 즉시 보안팀에 알리고, 해당 API 키를 재발급할 것.

---

## 문의

보안팀 Slack: `#security`
