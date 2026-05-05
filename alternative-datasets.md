# 산출물 4 — `alternative-datasets.md` 본문

> 아래 내용을 그대로 `lge-claude-code-3-data-analysis-2026-05-08` repo 루트의 `alternative-datasets.md`로 commit.

---

# 자율 선택제 — 본인 도메인 데이터로 분석하기

> LG전자 임원 클로드 코드 강의 · 2026-05-08 · 실습 3 보조 자료
> 본 repo의 `./data/` Olist 9 CSV가 표준이며, **시연·페어 응용은 Olist로 통일**합니다.
> 본 문서의 12개 데이터셋은 **솔로 챌린지** 또는 **사후 응용** 시 본인 도메인에 맞춰 활용하기 위한 큐레이션입니다.

---

## 0. 자율 선택제 사용 가이드

### 0.1 어떤 경우에 쓰나
- **솔로 챌린지 (실습 마지막 10분)** — 본인 부서/관심 영역과 맞는 데이터셋을 선택해서 single-shot 한 번
- **사후 PoC** — 강의 후 본인 부서 정기 보고서 1개를 PoC로 자동화

### 0.2 사용 흐름
1. 본 문서의 큐레이션 표에서 **본인 분석 유형** (RFM / 이상치 / Seller / 할부) 1개 선정
2. 해당 유형의 **1순위 데이터셋** Kaggle URL 클릭 → 다운로드
3. **선택지 A**: 본 repo를 fork → `./data-custom/` 폴더에 새 CSV 추가 → 작업
4. **선택지 B**: 본인 PC 로컬에서 별도 폴더로 작업 (Cloud 환경 X, 로컬 Claude Code 사용)
5. CLAUDE.md를 도메인에 맞춰 **간단히 수정** (조인 키, 필터 조건만 갱신)

### 0.3 LG 시나리오 가상화 멘트 가이드
공개 데이터셋의 도메인을 **LG 비즈니스 맥락으로 매칭**해서 임원이 실감할 수 있게 합니다.

| 공개 데이터 도메인 | LG 가상화 매칭 멘트 |
|------------------|--------------------|
| 이커머스 (Olist 등) | "LG 공식몰 데이터라고 가정. 가전 카테고리만 필터링했다고 보면" |
| 텔레콤 churn | "U+ 가입자 이탈 분석. LG가전 IoT 연동 가입자로 좁히면" |
| 제조 결함 | "LG 가전 생산 라인 결함 데이터. 라인별/모델별 양산 안정화" |
| IoT/스마트홈 | "LG ThinQ 가전 사용 패턴. 페르소나별 사용 시간대" |
| 금융 거래 | "LG페이/캐피탈 거래 데이터. 세그먼트별 한도 정책" |
| 의료/HR | "임직원 건강검진 데이터. ESG 헬스 KPI" |
| 부동산/지리 | "LG 매장 입지 분석. 매출 vs 인구밀도/소득" |

> **주의** — "이건 가상화 매칭이고, 실제 LG 데이터는 사내 보안 정책으로 별도 트랙입니다"라는 한 문장을 항상 먼저 안내.

---

## 1. 분석 유형 1 — RFM / 고객 세그먼트

> **타겟**: 마케팅 / CRM / 디지털 임원
> **핵심 질문**: 누가 우리 VIP인가? 누가 이탈 직전인가? 어떻게 재참여시킬 것인가?
> **분석 산출**: Recency / Frequency / Monetary 3축 → VIP / Loyal / At-risk / Lost 4 세그먼트 → 세그먼트별 전략

### 1.1 1순위 — Online Retail II (UCI / Kaggle)
- **URL**: https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci
- **설명**: 영국 온라인 retail 트랜잭션 (2009~2011), ~1M 행, InvoiceNo / CustomerID / InvoiceDate / Quantity / UnitPrice / Country
- **왜 1순위?** RFM 분석의 학계·업계 표준 데이터. 노트북 100+ 공개, 한국 강의에서도 자주 사용.
- **컬럼 매핑** (Olist 대비):
  - `CustomerID` ↔ `customer_unique_id`
  - `InvoiceDate` ↔ `order_purchase_timestamp`
  - `Quantity * UnitPrice` ↔ `payment_value`
- **LG 가상화 멘트**: "LG 공식몰 충성고객 분석. 영국 데이터지만 RFM 로직은 한국 가전 고객에 그대로 적용 가능."
- **추천 분석**:
  - VIP / Loyal / At-risk / Lost 4 세그먼트
  - 세그먼트별 카테고리 선호도 트리맵
  - At-risk 세그먼트 재참여 캠페인 ROI 시뮬레이션

### 1.2 대안 1 — Telco Customer Churn (IBM Sample)
- **URL**: https://www.kaggle.com/datasets/blastchar/telco-customer-churn
- **설명**: 미국 텔레콤 7,043 고객 churn 데이터, 21개 컬럼 (계약 유형 / 결제 방식 / 서비스 / churn 라벨)
- **분석 변형**: RFM 대신 **CLV(Customer Lifetime Value) + Churn 위험 점수** 2축. tenure / MonthlyCharges / TotalCharges 활용.
- **LG 가상화 멘트**: "U+ 가입자 데이터라고 보면. 약정 만료 + churn 위험 = 보존 마케팅 타겟."

### 1.3 대안 2 — Instacart Online Grocery (Kaggle)
- **URL**: https://www.kaggle.com/c/instacart-market-basket-analysis
- **설명**: 미국 식료품 배달 3M+ 주문, user_id / order_dow / days_since_prior_order
- **분석 변형**: RFM에 **장바구니 패턴(Market Basket Analysis)** 추가. apriori 알고리즘 (mlxtend) 또는 단순 co-occurrence.
- **LG 가상화 멘트**: "ThinQ 앱 + 액세서리 동시구매 분석이라고 가정. '세탁기 산 사람이 그 다음 산 것은?'"

---

## 2. 분석 유형 2 — 이상치 / 품질 이슈 탐지

> **타겟**: 품질 / 제조 / 운영 임원
> **핵심 질문**: 정상 분포에서 벗어난 케이스는 어디에서 발생하나? 어떤 변수가 이상의 사전 신호인가?
> **분석 산출**: 시계열 이상치 탐지 + 카테고리/지역별 이상 발생률 + 사전 신호 변수 후보

### 2.1 1순위 — Predicting Manufacturing Defects (Kaggle)
- **URL**: https://www.kaggle.com/datasets/rabieelkharoua/predicting-manufacturing-defects-dataset
- **설명**: 합성 제조 데이터 ~3,000행, ProductionVolume / ProductionCost / SupplierQuality / DeliveryDelay / DefectRate / QualityScore / DefectStatus 등 16컬럼
- **왜 1순위?** LG 제조 DNA 매칭. 결함률(DefectRate) 분포 + 어떤 변수가 결함과 가장 상관 있는지 즉각 도출.
- **컬럼 매핑** (Olist 대비):
  - `DefectRate` ↔ `review_score` (낮을수록 문제)
  - `DeliveryDelay` ↔ `delivery_days`
  - `SupplierQuality` ↔ (Olist에 직접 매핑 없음)
- **LG 가상화 멘트**: "LG 가전 생산라인 데이터로 가정. SupplierQuality가 낮을수록 DefectRate가 어떻게 올라가는지 — 공급사 관리 KPI."
- **추천 분석**:
  - DefectRate 히스토그램 + 상위/하위 5% 컷
  - 변수별 DefectRate 상관 산점도
  - 라인/공급사별 결함률 비교 막대

### 2.2 대안 1 — Credit Card Fraud Detection (Kaggle)
- **URL**: https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud
- **설명**: 유럽 카드사 2일간 트랜잭션 284K행, 28개 PCA 익명화 컬럼 + Amount + Class(0/1)
- **분석 변형**: 이상 거래 비율(0.17%) 탐지. ROC/PR 곡선보다는 **시간대별 / 금액 구간별 fraud rate**를 임원 친화 차트로.
- **LG 가상화 멘트**: "LG페이 거래에서 이상 거래 패턴 — 시간대/금액/지역으로 본 fraud profile."

### 2.3 대안 2 — Steel Plates Faults (UCI)
- **URL**: https://www.kaggle.com/datasets/uciml/faulty-steel-plates
- **설명**: 철판 결함 1,941 샘플, 27개 기하/물리 피처 + 7개 결함 타입
- **분석 변형**: 결함 타입 분포 + 가장 흔한 결함의 사전 신호 변수 Top 5
- **LG 가상화 멘트**: "디스플레이 패널 결함 데이터라고 가정. 결함 타입별 발생률과 검사 우선순위 재정렬."

---

## 3. 분석 유형 3 — Seller / 파트너 성과 분석

> **타겟**: 영업 / 파트너십 / 채널 임원
> **핵심 질문**: 어떤 파트너가 우리 매출의 주축인가? 누가 성장 잠재력 vs 누가 리스크인가?
> **분석 산출**: 파트너별 매출/마진/만족도 3축 + 성장 잠재력 점수 + 우선 관리 대상 5개

### 3.1 1순위 — Olist Seller (본 repo 활용 응용 C)
- **URL**: 본 repo `./data/olist_sellers_dataset.csv` + `olist_order_items_dataset.csv` (3K 행 sellers, 113K items)
- **설명**: 표준 시연에서는 sellers를 제외하지만, 페어 응용 C에서 별도 로드. 이미 repo 안에 있는 데이터.
- **왜 1순위?** **추가 다운로드 불필요** — Olist 9 CSV 안에 seller 데이터가 있으므로 자율 선택제 진입 장벽 0.
- **추천 분석**:
  - Top 20 seller 매출 막대
  - seller_state별 평균 배송시간 vs 리뷰 점수 산점도
  - seller당 카테고리 분산도 (집중형 vs 다각화형)
  - "성장 잠재력 높은 seller 5개 선정 + 근거"

### 3.2 대안 1 — Superstore Sales (Kaggle / Tableau)
- **URL**: https://www.kaggle.com/datasets/vivek468/superstore-dataset-final
- **설명**: 미국 소매 9,994 주문, Customer ID / Segment / Region / Category / Sub-Category / Sales / Profit / Discount / Ship Mode
- **분석 변형**: Seller 대신 **Sub-Category × Region** 매트릭스로 매출 vs 마진 분석. Discount의 마진 잠식 효과 시각화.
- **LG 가상화 멘트**: "LG 가전 채널별/카테고리별 마진 분석. 어디서 할인을 줄여야 마진이 살아나는지."

### 3.3 대안 2 — Yelp Open Dataset (subset)
- **URL**: https://www.kaggle.com/datasets/yelp-dataset/yelp-dataset
- **설명**: Yelp 비즈니스 150K + 리뷰 6.9M (subset 사용 권장 — 50K 리뷰 샘플)
- **분석 변형**: 비즈니스(=seller) 별 평균 별점 + 리뷰 수 + 카테고리. 파레토 (상위 20%가 매출의 X%) 분석.
- **LG 가상화 멘트**: "LG 서비스센터 리뷰 데이터로 가정. 지점별 만족도 분포 + 개선 우선 5개."

---

## 4. 분석 유형 4 — 할부 / 결제 전략

> **타겟**: 재무 / CFO / 결제 전략 임원
> **핵심 질문**: 할부가 객단가를 얼마나 끌어올리나? 어떤 결제수단이 회전율 측면에서 유리한가?
> **분석 산출**: 할부개월 vs 객단가 상관 + 결제수단별 만족도 + 정책 변경 시나리오 시뮬레이션

### 4.1 1순위 — Olist Payments (본 repo 활용 응용 D)
- **URL**: 본 repo `./data/olist_order_payments_dataset.csv` (104K 행)
- **설명**: payment_type (credit_card / boleto / voucher / debit_card) + payment_installments (1~24) + payment_value
- **왜 1순위?** **추가 다운로드 불필요**. payment_installments 컬럼이 있어 할부 분석에 즉시 적합.
- **추천 분석**:
  - 할부개월 분포 + 객단가 상관 산점도
  - 할부 vs 일괄 결제의 평균 리뷰 점수 비교
  - 결제수단별 (credit_card / boleto / voucher / debit) 할부 패턴
  - "할부 정책 변경 시 예상 GMV 임팩트" 3 시나리오

### 4.2 대안 1 — Default of Credit Card Clients (UCI)
- **URL**: https://www.kaggle.com/datasets/uciml/default-of-credit-card-clients-dataset
- **설명**: 대만 카드사 30K 고객 6개월 한도/결제/연체 + 다음달 default 라벨
- **분석 변형**: 한도 사용률 vs default 위험 + 결제 패턴 클러스터. **할부 한도 정책 가이드**로 변환.
- **LG 가상화 멘트**: "LG캐피탈/할부 정책 시뮬레이션. 어떤 사용 패턴이 default 위험을 끌어올리는지."

### 4.3 대안 2 — Bank Marketing (UCI)
- **URL**: https://www.kaggle.com/datasets/janiobachmann/bank-marketing-dataset
- **설명**: 포르투갈 은행 텔레마케팅 4.5만 통화, 고객 프로필 + 통화 결과 + 정기예금 가입 여부
- **분석 변형**: 결제 자체보다는 **금융상품 가입 전환율** 분석. 콜 전략 ROI.
- **LG 가상화 멘트**: "LG 금융상품(보험/렌탈) 텔레마케팅 가정. 어떤 프로필에 어떤 시간대 콜이 효과적인지."

---

## 5. 큐레이션 요약 표 (12개)

| 유형 | 순위 | 데이터셋 | Kaggle URL | 매칭 임원 |
|------|------|---------|-----------|----------|
| **RFM** | ⭐ 1순위 | Online Retail II | [link](https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci) | 마케팅/CRM |
| RFM | 대안 1 | Telco Customer Churn | [link](https://www.kaggle.com/datasets/blastchar/telco-customer-churn) | 텔레콤/통신 |
| RFM | 대안 2 | Instacart Market Basket | [link](https://www.kaggle.com/c/instacart-market-basket-analysis) | 디지털/이커머스 |
| **이상치** | ⭐ 1순위 | Manufacturing Defects | [link](https://www.kaggle.com/datasets/rabieelkharoua/predicting-manufacturing-defects-dataset) | 품질/제조 |
| 이상치 | 대안 1 | Credit Card Fraud | [link](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud) | 결제/리스크 |
| 이상치 | 대안 2 | Steel Plates Faults | [link](https://www.kaggle.com/datasets/uciml/faulty-steel-plates) | 디스플레이/소재 |
| **Seller** | ⭐ 1순위 | Olist Seller (repo 내부) | (본 repo) | 영업/파트너십 |
| Seller | 대안 1 | Superstore Sales | [link](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final) | 채널/유통 |
| Seller | 대안 2 | Yelp Open Dataset | [link](https://www.kaggle.com/datasets/yelp-dataset/yelp-dataset) | CS/서비스 |
| **할부** | ⭐ 1순위 | Olist Payments (repo 내부) | (본 repo) | 재무/CFO |
| 할부 | 대안 1 | UCI Credit Card Default | [link](https://www.kaggle.com/datasets/uciml/default-of-credit-card-clients-dataset) | 금융/캐피탈 |
| 할부 | 대안 2 | Bank Marketing | [link](https://www.kaggle.com/datasets/janiobachmann/bank-marketing-dataset) | 영업/금융상품 |

---

## 6. CLAUDE.md 도메인 적응 가이드 (최소 수정)

자율 선택제 데이터셋을 사용할 때 본 repo의 `CLAUDE.md` §11을 그대로 쓰지 말고, 다음 4개 항목만 수정해서 새 데이터에 맞춥니다.

### 6.1 §11.3 (샘플링 + 조인)
- 새 데이터 행 수가 50K 미만이면 → 샘플링 X, 전체 로드
- 50K~500K → `sample(n=20000, random_state=42)`
- 500K 이상 → `sample(n=50000)` + chunked 로드
- 조인 키 / 순서 새로 명시

### 6.2 §11.4 (출력 정책)
- df dump 금지는 도메인 무관 동일 유지

### 6.3 §11.5 (시각화 표준)
- 도메인별 차트 매칭 추가:
  - RFM → 산점도 (R vs F) + 트리맵 (세그먼트별 매출)
  - 이상치 → 박스플롯 + 시계열 라인
  - Seller → 막대 Top 20 + 산점도 (만족 vs 매출)
  - 할부 → 히스토그램 + 산점도 + 시나리오 막대

### 6.4 §11.6 (인사이트 형식)
- [관찰] → [수치 근거] → [시사점] 패턴은 **도메인 무관 동일**
- 단, 시사점은 본인 부서 액션으로 구체화

---

## 7. 자주 묻는 질문 (자율 선택제)

### Q1. Cloud 환경에서 본 repo 외 데이터를 어떻게 쓰나?
A. 다음 2가지 방법:
- **A안**: 본 repo를 본인 GitHub로 fork → `./data-custom/` 폴더에 새 CSV commit → Cloud에서 fork된 repo 선택
- **B안**: 로컬 Claude Code (Desktop App → Local Default)에서 `~/Desktop/practice-3-custom/` 폴더로 별도 작업 (Python 사전 설치 필요)

### Q2. 데이터가 너무 크면 (1GB 이상)?
A. (1) `usecols=[...]` 로 필요 컬럼만 로드, (2) `sample(n=50000)` 강제, (3) 정 안되면 미니 샘플 추출 후 commit.

### Q3. 데이터가 한글이면 한글 폰트 이슈?
A. Plotly는 브라우저 폰트 사용이라 OK. 단, 컬럼명에 공백·특수문자가 있으면 Python 변수로 쓸 때 `df['컬럼 명']` 형태로 명시.

### Q4. 라이선스가 걱정되는데?
A. Kaggle 데이터셋은 Public Domain (ODbL/CC0) 또는 Kaggle 라이선스가 명시되어 있음. **상업 활용은 본 데이터셋 라이선스 페이지를 확인하세요.** 본 강의(교육 목적)는 모두 사용 가능.

### Q5. 분석 결과를 사내에 공유해도 되나?
A. **공개 데이터 분석 결과**는 자유롭게. 단, "이 분석 패턴을 LG 사내 데이터에 적용한 결과"는 사내 보안 정책에 따라 처리.

---

## 8. 다음 단계 — Quick Win 추천

본인 부서 정기 보고서 1개를 PoC로 자동화하는 흐름:

1. **Week 1**: 본인 부서의 매주 만드는 보고서 1개 식별 (Excel/PPT)
2. **Week 2**: 데이터를 합성/익명화 → 본인 GitHub private repo로 commit
3. **Week 3**: 본 repo의 CLAUDE.md를 도메인 적응 → 새 repo에 배치
4. **Week 4**: single-shot 한 번으로 보고서 자동화 검증

> **성공 지표**: "이번 주 보고서 만드는데 60분 → 5분"이면 PoC 성공. 사내 Claude Enterprise 도입 검토로 진행.

---

*v1.0 (2026-05-08) — 실습 3 자율 선택제 12개 데이터셋 큐레이션. RFM / 이상치 / Seller / 할부 4개 분석 유형 × 1순위+대안 2개. LG 가상화 멘트 가이드 포함.*
