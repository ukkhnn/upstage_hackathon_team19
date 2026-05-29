# Frontend (Lovable)

> Lovable로 제작한 대시보드 UI. 코드는 Lovable이 호스팅하므로 이 폴더에는 메모만 둠.

## 배포 URL

_TBD_ (Lovable publish 후 채워넣기)

## 화면 구성 (예정)

1. **정책 PDF 업로드** — 드래그앤드롭 영역, 업로드 진행률, 추출된 룰셋 미리보기
2. **고객 문의 시뮬레이터** — 문의 텍스트 입력 → 챗봇 답변 초안 + 위험도 표시
3. **위험 알림 대시보드** — 차단된 답변 로그, 위험 유형별 통계, 정책별 위반 빈도

## n8n 연동

- **Webhook 입력:** Lovable에서 입력 받은 데이터를 n8n Webhook 트리거로 POST
- **응답 JSON 스키마:** `workflows/runtime.json` 참고
