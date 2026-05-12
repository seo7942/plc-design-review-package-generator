# XG5000 Import Ready Checklist — Turn1

## 목적

기존 XGT 수동 반입/검토용 Export를 XG5000 실제 Import 시도 가능한 파일세트로 확장하기 위한 준비 체크리스트입니다.

현재 Turn1은 기본 폴더와 리포트 생성 단계입니다. 실제 `.xgw` 또는 `.xgp` 프로젝트 파일은 생성하지 않습니다.

## Case

```text
case_id: PLC-20260512-WASTEWATER-IO200-001
vendor_target: LS XGT / XG5000
actual_xgw_generated: False
actual_xgp_generated: False
overall_status: PASS
```

## 현재 데이터 요약

```text
io_total: 200
ladder_network_count: 30
st_draft_present: True
st_draft_chars: 3518
address_hold_count: 0
address_review_count: 0
```

## XG5000 Import Ready 진행 순서

1. `01_xgt_io_symbol/xgt_address_review.csv`에서 HOLD 항목 확인
2. `01_xgt_io_symbol/xgt_symbol_table.csv`를 XG5000 Import CSV 후보로 정밀화
3. Direct Variable Comment Import CSV 생성
4. ST Program Import 후보 TXT 생성
5. IL Import 후보 파일 생성 여부 검토
6. XG5000에서 실제 Import 테스트
7. Build/Compile 로그 저장
8. 후속 Compile Error Review Adapter에서 오류 분류

## 이번 Turn1 범위

- [x] `05_xg5000_import_ready` 폴더 생성
- [x] `xg5000_import_ready_report.json` 생성
- [x] `xg5000_import_checklist.md` 생성
- [x] 실제 `.xgw/.xgp` 미생성 정책 유지

## 다음 Turn2 예정

- [x] `xg5000_global_variable_import.csv` 생성
- [x] `xg5000_local_variable_import.csv` 생성
- [x] `xg5000_direct_variable_comment_import.csv` 생성
- [x] XG5000 CSV Import용 컬럼 정밀화

## 안전 고지

이 폴더의 산출물은 XG5000 Import 준비용 후보입니다. 실제 설비 RUN/다운로드 전에는 반드시 XG5000 Build/Compile, 오프라인 시뮬레이션, 현장 엔지니어 검수가 필요합니다.
