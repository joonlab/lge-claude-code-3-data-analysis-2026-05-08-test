# LG전자 임원 Claude Code 실습 3 — 데이터 분석 에이전트

![Claude Code](https://img.shields.io/badge/Claude%20Code-Pro%2FMax-D97706?style=flat-square)
![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=flat-square&logo=python&logoColor=white)
![Plotly](https://img.shields.io/badge/Plotly-Interactive-3F4F75?style=flat-square&logo=plotly&logoColor=white)
![License](https://img.shields.io/badge/License-MIT%20%2B%20ODbL-2EA44F?style=flat-square)

> AI에게 CSV를 던지고, 임원 의사결정용 인터랙티브 대시보드를 single-shot 1회로 받아내는 실습입니다.

---

## 실습 목표

- **데이터 분석가 1주일 작업**(샘플링·조인·EDA·시각화·인사이트 도출)을 Claude Code single-shot **90초**로 압축하는 흐름을 체험합니다.
- 멀티 테이블(9 CSV) 데이터를 **전략적 샘플링 + 표준 조인 패턴**으로 토큰 한계 안에서 안전하게 다루는 방법을 익힙니다.
- 비즈니스 질문 한 줄 → **단일 self-contained HTML 대시보드**로 답하는 임원 보고용 산출물 패턴을 학습합니다.

---

## 무엇을 하는가

브라질 e-commerce 플랫폼 **Olist**의 9개 CSV 멀티 테이블 데이터(주문·고객·상품·결제·리뷰·판매자·지역·카테고리)를 입력으로,
Claude Code가 **전략적 샘플링 → 조인 → EDA → Plotly 인터랙티브 대시보드 생성**을 한 번의 프롬프트로 수행합니다.
분석가가 주피터에서 1주일에 걸쳐 만드는 임원 보고용 대시보드를, AI가 90초 안에 single-shot으로 산출합니다.

---

## 결과물 형태

수강생이 받게 되는 산출물은 **단일 self-contained HTML 파일** 1개입니다.

- **포맷**: `output/dashboard_*.html` — Plotly CDN 사용, 200KB 이하, 브라우저 더블클릭만으로 동작
- **상단 KPI 카드 4개**: 총 GMV / 평균 배송시간 / 평균 리뷰 점수 / 활성 고객 수
- **중단 차트 4-5개** (인터랙티브):
  - 주(state)별 매출 막대 차트 (Top 10)
  - 배송시간 vs 리뷰 점수 산점도
  - 결제 수단 도넛 차트
  - 카테고리 트리맵
  - 월별 GMV 시계열
- **하단 AI 자동 인사이트 박스 3-5개**: 수치 근거를 동반한 [관찰 → 수치 → 시사점] 구조 카드
- **모든 차트 인터랙티브**: 호버 시 정확 수치 툴팁, 클릭 시 드릴다운, 줌/팬 지원

---

## Quick Start (Cloud Sandbox)

1. **Claude Desktop App** 실행 (Pro/Max 구독 필수)
2. 좌상단 모드 토글에서 **Cloud → Default** 선택 (Sandbox 자동 활성화)
3. 우상단 Working Directory에서 본 repo (`lge-claude-code-3-data-analysis-2026-05-08-test`) 선택 → 자동 clone
4. `CLAUDE.md` 자동 로드 확인 (채팅창 상단 인디케이터)
5. 강사 시연 프롬프트 입력 → Enter (single-shot 실행)
6. `./output/dashboard_*.html` 응답 링크 클릭 → 로컬 다운로드 → 브라우저에서 인터랙티브 동작 확인

> Python 설치, 패키지 설치, 데이터 다운로드 모두 **불필요**합니다. Cloud Sandbox가 모든 환경을 자동 구성합니다.

---

## 폴더 구조

```
lge-claude-code-3-data-analysis-2026-05-08-test/
├── README.md
├── CLAUDE.md                         # 분석 규칙 자동 로드 (샘플링·조인·산출물 표준)
├── LICENSE                           # MIT (코드/문서) + ODbL (Olist 데이터)
├── data/                             # Olist 9 CSV 사전 동봉
│   ├── olist_orders_dataset.csv
│   ├── olist_order_items_dataset.csv
│   ├── olist_customers_dataset.csv
│   ├── olist_products_dataset.csv
│   ├── olist_order_payments_dataset.csv
│   ├── olist_order_reviews_dataset.csv
│   ├── olist_sellers_dataset.csv
│   ├── olist_geolocation_dataset.csv
│   └── product_category_name_translation.csv
├── alternative-datasets.md           # 자율 선택제 12개 데이터셋 큐레이션
├── .gitignore
└── output/
    └── .gitkeep                      # 결과물 저장 폴더 (실제 .html은 ignore)
```

---

## 자율 선택제 — 본인 도메인 데이터로 확장

표준 시연은 Olist 9 CSV로 통일되지만, 본인 업무 도메인에 가까운 데이터로 응용 분석을 시도하고 싶다면 [`alternative-datasets.md`](./alternative-datasets.md)를 참고하세요.

| 분석 유형 | 임원 시각 | 1순위 + 대안 |
|----------|---------|-------------|
| **RFM 세그먼트** | 마케팅 | Olist + 대안 2개 |
| **이상치 탐지** | 운영 | Olist + 대안 2개 |
| **Seller 성과 분석** | 영업 | Olist + 대안 2개 |
| **할부/결제 패턴** | 재무 | Olist + 대안 2개 |

총 **4개 분석 유형 × 3개 후보 = 12개 데이터셋**을 큐레이션했습니다. 본인 도메인(영업·마케팅·제조·HR·재무 등)에 맞춰 선택할 수 있도록 Kaggle URL과 적용 가이드를 함께 제공합니다.

> 표준 시연 및 페어 응용은 **Olist 9 CSV**로 통일합니다 (강사 라이브 통제 범위).
> 자율 선택제는 **솔로 챌린지** 시간 또는 **사후 응용**용으로 권장합니다.

---

## 사전 준비물

- **Claude Desktop App** (macOS / Windows)
- **Pro 또는 Max 구독** (Cloud Sandbox 사용 권한 필요)
- 별도 Python·패키지·데이터 다운로드 **불필요**

---

## 참고 문서

- [`CLAUDE.md`](./CLAUDE.md) — 분석 규칙 자동 로드 (샘플링·조인·시각화·산출물 표준 패턴)
- [`alternative-datasets.md`](./alternative-datasets.md) — 자율 선택제 12개 데이터셋 큐레이션

---

## Data Sources

- **Brazilian E-Commerce Public Dataset by Olist** — [Kaggle](https://www.kaggle.com/datasets/olist/brazilian-ecommerce)
- **라이선스**: ODbL (Open Database License, Public Domain Dedication)
- **규모**: 9개 CSV 멀티 테이블, 100,000+ 주문, 32,000+ 고객 (2016~2018)
- **구성**: 주문(orders) / 주문 항목(order_items) / 고객(customers) / 상품(products) / 결제(payments) / 리뷰(reviews) / 판매자(sellers) / 지역(geolocation) / 카테고리 번역(category_translation)
- 본 repo의 `data/`에 **사전 동봉**되어 있어 수강생이 별도로 다운로드할 필요가 없습니다.

---

## License

- **코드 / 문서**: MIT License
- **Olist 데이터**: ODbL (Open Database License, Public Domain) — 원 출처 [Kaggle](https://www.kaggle.com/datasets/olist/brazilian-ecommerce)

---

## 문의

- **운영**: LG전자 인재육성팀
- **강사**: 박준 ([@joonlab](https://github.com/joonlab)) · CMDS_JoonLab
- **이슈/문의**: 본 repo의 GitHub Issues

---

*LG전자 임원 Claude Code 강의 · 2026-05-08 · 실습 3 데이터 분석 에이전트*
