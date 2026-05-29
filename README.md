# Upstage Hackathon Team 19

> **Low-code AI Startup Hackathon with Upstage** | 2026.05.23 ~ 2026.05.31
> 주최: Upstage AI Ambassador · 후원: 이화여자대학교 창업지원단

기업향 CS 챗봇 할루시네이션 가드레일 — 챗봇이 생성한 답변 초안을 사내 정책 PDF와 실시간 교차 검증하여, 금액·기간 같은 핵심 조건이 약관과 다르면 자동 차단 + 관리자 대시보드에 위험 알림.

---

## 팀 정보

- **팀 번호:** 19팀
- **팀명:** _TBD_
- **참가 대학:** 서강대 / _TBD_ / _TBD_
- **팀원:**
  - 현욱 (서강대 컴퓨터공학과) — [@TBD](https://github.com/TBD)
  - _TBD_ — [@TBD](https://github.com/TBD)
  - _TBD_ — [@TBD](https://github.com/TBD)

---

## 프로젝트 개요

### 문제

2024년 Air Canada는 자사 챗봇이 잘못 안내한 환불 정책 때문에 캐나다 민사해결재판소에서 패소했다. 2025년 한 해 동안 엔터프라이즈 AI 사용자의 47%가 할루시네이션 콘텐츠를 근거로 주요 의사결정을 내렸고, AI 기반 고객 서비스 봇의 39%가 할루시네이션 관련 오류로 운영이 중단됐다. 2026년 1월 22일 한국에서 AI 기본법이 전면 시행되면서, 금융·보험 영역의 CS 챗봇은 고영향 AI로 분류되어 강화된 책임을 진다.

### 솔루션

챗봇이 만들어낸 답변을 고객에게 전달하기 전, 사내 정책 PDF에서 추출한 룰셋(금액·기간·조건)과 자동 대조한다. 어긋나면 차단 + 관리자 대시보드에 알림.

### 핵심 흐름

```
[정책 PDF 업로드]
  → Document Parse (구조화)
  → Information Extract (룰셋 추출)
  → Google Sheets 저장

[고객 문의 도착]
  → Solar LLM으로 답변 초안 생성 (RAG)
  → 답변에서 조건 추출
  → 룰셋과 대조
  → 위험도 산출 (안전 / 주의 / 위험)
  → 위험 시 차단 + 대시보드 알림
```

---

## 기술 스택

| 영역 | 도구 | 역할 |
|---|---|---|
| AI | **Upstage API** (Solar LLM, Document Parse, Information Extract) | 문서 구조화, 답변 생성, 룰셋 대조 |
| 백엔드 | **n8n** | 워크플로우 오케스트레이션 |
| 프론트엔드 | **Lovable** | 대시보드 UI |
| 데이터 | **Google Sheets** | 룰셋 저장소 (MVP 단계) |

---

## 폴더 구조

```
upstage_hackathon_team19/
├── README.md
├── .gitignore
├── docs/                  # 기획·리서치 문서
│   ├── context.md             # 해커톤·팀 전체 컨텍스트
│   ├── idea_candidate.md      # 아이디어 후보 7개 비교
│   ├── idea3_background.md    # 채택 아이디어 배경 근거
│   └── task_breakdown.md      # 작업 분해 (D-4 ~ D-day)
├── workflows/             # n8n 워크플로우 JSON
│   ├── ingestion.json         # 정책 PDF → 룰셋 등록
│   └── runtime.json           # 고객 문의 → 위험도 산출
├── data/                  # 데모용 가상 데이터
│   ├── policies/              # 가상 회사 정책 PDF
│   └── scenarios/             # 데모 시나리오 (안전/주의/위험)
├── frontend/              # Lovable 관련 메모
│   └── README.md              # 배포 URL, UI 구조
└── submission/            # 최종 제출물
    ├── business_plan.docx     # 서비스 기획서
    └── pitch_deck.pdf         # 발표 자료
```

---

## 일정

| 날짜 | D-day | 작업 |
|---|---|---|
| 5/27 수 | D-4 | 환경 셋업, 아이디어 확정 |
| 5/28 목 | D-3 | Upstage API 단독 테스트, 정책 PDF 초안, 추출 스키마 |
| 5/29 금 | D-2 | n8n 워크플로우(사전 등록 + 런타임) end-to-end |
| 5/30 토 | D-1 | 오프라인 통합 테스트 + 멘토링 (이대 학관 108호, 15:00~22:00) |
| 5/31 일 | D-day | 최종 제출 + 발표 (공덕 서울창업허브, 14:30~) |

---

## 최종 제출물 (4가지)

- [ ] **서비스 기획서** (`submission/business_plan.docx`)
- [ ] **n8n 워크플로우** (`workflows/*.json` + 캡처)
- [ ] **Lovable UI** 배포 URL — _TBD_
- [ ] **발표 자료** (`submission/pitch_deck.pdf`)

---

## 링크

- **공식 노션:** https://decisive-time-1fa.notion.site/Low-code-AI-Startup-Hackathon-with-Upstage-3698e93c02a280c7a501d91c3e18c6fe
- **사전 학습:** https://upflow.upstage.ai/courses
- **카카오톡 채널:** @upstagesinchon
- **Upstage Console:** https://console.upstage.ai
- **n8n:** https://n8n.io
- **Lovable:** https://lovable.dev

---

## 작업 규칙

- 비밀값(API 키, 카드 정보, 패스워드)은 **절대 커밋 금지**. `.env`로 분리하고 `.gitignore`로 추적 제외.
- n8n 워크플로우 export 시 credentials 포함 여부 확인 후 푸시.
- 커밋 메시지는 어떤 트랙(`workflow:`, `docs:`, `data:`, `plan:`)인지 prefix 붙이기.
- 해커톤 종료 후 (5/31 발표 끝나고) public으로 전환 예정.
