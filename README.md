# PLC Design Review Package Generator

## 비정형 수주서 기반 IO500급 PLC 설계 검토 패키지 자동 생성 샘플

이 저장소는 현장에서 받을 수 있는 **간단 메모형 수주서**를 입력으로 받아, PLC 엔지니어가 1차 검토할 수 있는 설계 초안 패키지를 자동 구성한 포트폴리오 샘플입니다.

대상 설비는 **폐수처리 자동화 설비**이며, 생성 기준은 **LS XGT / XG5000 검토용**입니다.

본 산출물은 실제 현장 RUN 확정본이 아니라, 선임 엔지니어가 검토하기 전 단계의 **REVIEW ONLY 설계 검토 자료**입니다.

---

## 1. 입력 수주서 개요

입력은 상세 설계서가 아니라 아래와 같은 형태의 비정형 메모입니다.

```text
폐수 처리 쪽 자동화 하나 잡아주세요.

탱크가 여러 개 있고 펌프, 밸브, 약품 투입, 교반기,
슬러지 배출, 컨베이어 조금 있습니다.

자동 운전으로 바꾸고 HMI에서 상태 보고 수동도 가능하게 해주세요.

원수 유입 탱크, 약품 투입 탱크, 중화 탱크,
세정/린스 탱크, 슬러지 배출 라인, 최종 배출 라인이 있습니다.

pH, 온도, 유량, 압력, 수위 정도는 봐야 합니다.
펌프랑 밸브가 많고, IO는 예비 포함 500점 정도로 잡아주세요.

LS XGT 기준이면 좋고,
나중에 XG5000에 넣을 수 있게 주소표와 ST/Ladder 초안,
도면도 검토용으로 뽑아주세요.

HMI는 운전 화면, 알람 화면, IO 모니터 정도면 됩니다.
모든 결과물은 REVIEW ONLY로 표시해주세요.
confirmed_for_plc_run=false 유지해주세요.
```

---

## 2. 생성 결과 요약

| 항목 | 내용 |
|---|---|
| Case ID | PLC-20260512-TYPE4-IO500-WW-001 |
| 수주서 유형 | 간단 메모형 비정형 수주서 |
| 설비 유형 | 폐수처리 자동화 설비 |
| IO 규모 | IO500급 |
| 벤더 기준 | LS XGT / XG5000 검토용 |
| 결과물 성격 | PLC 설계 1차 초안 / REVIEW ONLY |
| 현장 RUN 여부 | confirmed_for_plc_run=false |
| 검토 필요 여부 | review_required=true |

---

## 3. 공개 산출물 구성

본 저장소에는 포트폴리오 확인에 필요한 대표 산출물만 포함합니다.

```text
README.md

01_review_pdf/
  IO500_REVIEW_ONLY_PACKAGE.pdf

02_hmi/
  hmi_tag_table.csv

03_xg5000/
  xg5000_variable_description_import.csv

screenshots/
  001.jpg
  002.jpg
```

---

## 4. 파일별 설명

### 4.1 `01_review_pdf/IO500_REVIEW_ONLY_PACKAGE.pdf`

IO500급 PLC 설계 검토 패키지를 하나의 PDF로 묶은 공개용 산출물입니다.

포함 예시:

- DI Wiring 도면 후보
- DO Wiring 도면 후보
- Safety Circuit 후보
- Panel Layout 후보
- Network / Communication Review 후보
- Ladder Visual Review
- XP-Builder / XGT Panel 스타일 HMI Preview
- Alarm Monitor Preview
- Manual Operation Preview
- Engineering Closeout Checklist

모든 페이지는 **REVIEW ONLY** 검토용 성격을 유지합니다.

---

### 4.2 `02_hmi/hmi_tag_table.csv`

HMI 작화 검토를 위한 태그 후보 목록입니다.

포함 예시:

- 수위 표시용 아날로그 태그
- 운전/정지/알람 상태 램프
- 안전 조건 표시
- IO 상태 모니터링 태그
- 알람 이력 화면 후보
- 내부 상태 표시용 HMI 태그 후보

본 파일은 XP-Builder 최종 프로젝트 파일이 아니라 **작화 검토용 태그 후보 CSV**입니다.

---

### 4.3 `03_xg5000/xg5000_variable_description_import.csv`

LS XG5000에서 변수/설명 반입을 검토하기 위한 CSV 후보 파일입니다.

포함 예시:

- 변수명
- 타입
- 디바이스 주소
- 사용 유무
- HMI 연결 후보
- 설명문

실제 `.xgw` 또는 `.xgp` 프로젝트 파일을 자동 생성하는 단계가 아니라, XG5000 테스트 프로젝트에서 변수/설명 Import 가능성을 검토하기 위한 파일입니다.

---

### 4.4 `screenshots/`

XG5000 화면에서 변수/설명 반입 결과를 확인한 스크린샷입니다.

스크린샷은 다음을 보여줍니다.

- XG5000 변수/설명 화면 반입 결과
- M 디바이스 기반 내부 변수 후보
- Alarm latch/helper 변수 후보
- Timer done helper 변수 후보
- 한글 설명문 반입 상태

---

## 5. 자동 생성 범위

| 영역 | 생성 내용 |
|---|---|
| IO | DI/DO/AI/AO 주소 후보, Spare IO, 설명문 |
| 전장 | DI/DO 배선도 후보, Safety 후보, Panel Layout 후보 |
| Logic | ST/Ladder 초안, Ladder Visual Review |
| HMI | XP-Builder 스타일 Main / Alarm / Manual Preview |
| Alarm | Alarm latch/helper 후보, 알람 이력 화면 후보 |
| XG5000 | 변수/설명 Import CSV 후보 |
| QA | Engineering Closeout Checklist, REVIEW REQUIRED 항목 |

---

## 6. REVIEW ONLY 원칙

본 저장소의 산출물은 **현장 최종본이 아닙니다.**

반드시 아래 절차가 필요합니다.

1. 현장 IO 실사
2. 실제 PLC 모듈 및 슬롯 확정
3. 전장 도면 및 단자대 번호 확정
4. MCCB / MC / SMPS / 전선 굵기 확정
5. 안전 릴레이 / Safety Controller / PLr / SIL 검토
6. XG5000 Import 테스트
7. Build / Compile 검토
8. XP-Builder HMI 태그 바인딩 검토
9. 오프라인 시뮬레이션
10. 현장 시운전 전 최종 승인

---

## 7. Engineering Closeout 항목

PDF 마지막 Closeout 페이지에는 현장 확정이 필요한 항목을 별도로 남깁니다.

| 구분 | 검토 항목 | 상태 |
|---|---|---|
| Electrical | MCCB / MC 용량 | REVIEW REQUIRED |
| Electrical | SMPS 24VDC 용량 | REVIEW REQUIRED |
| Electrical | 전선 굵기 / 단자 규격 | REVIEW REQUIRED |
| Safety | Safety Relay / Controller 모델 | REVIEW REQUIRED |
| PLC | XG5000 변수/주소 매핑 | IMPORT REVIEW REQUIRED |
| HMI | XP-Builder HMI Tag Binding | HMI BINDING REQUIRED |
| Network | IP / Protocol / Station | NETWORK REVIEW REQUIRED |
| Test | Dry-run / Simulation / I/O Test | FIELD TEST REQUIRED |

---

## 8. 본 저장소에 포함하지 않는 것

아래 항목은 공개하지 않습니다.

- 원본 PLC Package JSON 전체
- 도메인맵 원본
- 스키마 / 실행헌법 원본
- 생성기 원본 코드
- 랩핑기 원본 코드
- 벤더 어댑터 원본 코드
- 내부 QA Manifest 전체
- 전체 ZIP 원본
- 학습 데이터셋
- 모델 파일

이 저장소는 엔진 원본 공개가 아니라, 엔진이 생성한 **검토용 산출물 샘플**을 보여주기 위한 포트폴리오입니다.

---

## 9. 포트폴리오 설명 문장

비정형 간단 메모형 수주서를 기준으로 IO500급 폐수처리 자동화 PLC 설계 검토 패키지를 자동 구성했습니다. 결과물은 REVIEW ONLY PDF 패키지, HMI Tag Table, XG5000 변수 설명 Import CSV로 축약해 공개했으며, 실제 현장 RUN 확정본이 아니라 선임 엔지니어가 검토할 수 있는 1차 설계 초안 패키지입니다.

---

## 10. 사용 목적

이 프로젝트는 다음 역량을 보여주기 위한 포트폴리오입니다.

- 비정형 수주서 해석
- 대형 IO 규모 PLC 설계 자료 구조화
- LS XGT / XG5000 기준 주소·변수 후보 생성
- HMI 태그 후보 생성
- 전장 도면 후보 PDF 생성
- Ladder Visual Review 생성
- REVIEW ONLY 기준의 안전한 공개 패키지 구성
- 엔지니어 검토 전 단계의 설계 초안 자동화

---

## 11. 주의

본 샘플은 학습 및 포트폴리오 목적의 검토용 자료입니다.

실제 설비에 적용하기 위해서는 반드시 현장 엔지니어, 전기 담당자, 안전 담당자의 검토와 XG5000 Build/Compile, XP-Builder HMI 바인딩, 오프라인 테스트, 현장 시운전 절차가 필요합니다.
