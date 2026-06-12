# VeriFLow — CS 챗봇 할루시네이션 가드레일

> AI 챗봇이 사내 정책과 상충하는 답변을 내보내기 **전에** 실시간 교차 검증하고 차단하는 시스템
> Low-code AI Startup Hackathon with Upstage (2026.05) · 팀 백스테이지 (19팀)

## 문제

기업 CS 챗봇의 가장 큰 리스크는 내부 규정(환불·보상 정책 등)과 다른 답변을 안내하는 할루시네이션이다. Air Canada 챗봇 오안내 판례(2024)처럼 단 한 번의 잘못된 안내가 법적 책임으로 직결되며, 다수 기업이 사람의 수작업 검수(human-in-the-loop)로 이를 막고 있다. VeriFLow는 이 검수를 n8n 파이프라인으로 자동화한다.

## 동작 방식

### 1. 정책 등록 파이프라인 (관리자가 정책 PDF 업로드 시)
```
정책 PDF 업로드 (Webhook)
 ├─ Document Parse → 정책 전문 markdown 변환 → 시트 저장   ← 검증 기준(전문)
 └─ Information Extract 2-pass
     ├─ 1차: 정책 섹션/카테고리 추출 (환불·교환·배송 등)
     └─ 2차: 섹션별 세부 룰셋 추출 (기간·금액·조건·예외·증빙)
         → 18컬럼 구조화 룰셋 시트 저장                      ← 관리자 감사 기준값
```

### 2. 실시간 검증 파이프라인 (고객 문의 시)
```
고객 질문 (Webhook)
 → Solar LLM 1차 답변 생성
 → 정책 전문 조회 (Google Sheets)
 → Solar LLM 교차 검증: 답변 vs 정책 전문 대조
    → GREEN  정책 일치 → 원본 답변 전송
    → YELLOW 단서/예외 누락 → 자동 보정된 안전 답변 전송 + 경고 표시
    → RED    정책 불일치 → 전송 차단 + 상담원 연결 안내
 → YELLOW/RED 건은 주제·의도·감정·시급성 분류 후 감사 로그 적재
```

### 3. 관리자 대시보드 파이프라인
룰셋 + 위험 로그를 결합해 케이스별 권장 조치(적용 정책 기준값, 안전 답변 기준, 에스컬레이션 조건)를 생성. Lovable로 구현된 대시보드에서 RED/YELLOW 비율 통계와 로그 상세를 확인.

## 핵심 설계 포인트

- **전문 대조 방식**: 좁은 스키마로 숫자 몇 개만 추출해 비교하는 대신, Document Parse가 변환한 정책 전문을 통째로 검증 기준으로 사용 — 표 항목·예외 조건 누락 없이 검증
- **2-pass Information Extract**: 1차로 섹션 구조를 잡고 2차로 섹션별 상세 룰을 뽑아, 단일 호출 대비 추출 누락 감소
- **이중 용도 데이터**: 같은 정책 PDF에서 검증용 전문(Document Parse)과 감사용 룰셋(Information Extract)을 분리 생성
- **방어적 파싱**: 모든 LLM 응답에 파싱 실패 폴백 (실패 시 YELLOW + 상담원 연결로 안전 측 분기)

## 기술 스택

| 구분 | 기술 |
|---|---|
| 워크플로우 | n8n (웹훅 3종: 정책 등록 / 고객 문의 / 관리자 조회) |
| LLM | Upstage Solar Pro 2 (생성 · 검증 · 분류) |
| 문서 처리 | Upstage Document Parse, Information Extract |
| 데이터 | Google Sheets (정책 전문 / 구조화 룰셋 / 감사 로그) |
| 프론트엔드 | Lovable (고객 챗 UI + 관리자 대시보드) |

## 팀 구성 및 역할

- **양현욱** — 백엔드: n8n 검증 워크플로우 설계, Upstage API 연동
- 김민주 — 백엔드: n8n 워크플로우, Upstage API
- 전지은 — 프론트엔드: Lovable UI

## 파일

- `workflow.sanitized.json` — n8n 워크플로우 (credential·웹훅·시트 ID는 placeholder 처리)

> 참고: 해커톤 n8n 인스턴스가 만료되어 라이브 데모는 제공되지 않습니다. 워크플로우 JSON은 n8n에 import하여 구조를 확인할 수 있습니다.
