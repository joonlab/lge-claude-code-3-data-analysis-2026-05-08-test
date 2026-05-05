# 산출물 3 — Repo `CLAUDE.md` 본문 (v3.1, 200줄 이하)

> 아래 내용을 그대로 `lge-claude-code-3-data-analysis-2026-05-08` repo 루트의 `CLAUDE.md`로 commit. Cloud 환경에서 repo 진입 시 자동 로드된다.
> common-redesigner 공통 표준 §0~§9 + 실습 3 §11 결합. 표준 헤더는 모든 실습 repo에서 동일.

---

# CLAUDE.md — 실습 3 데이터 분석 에이전트

> LG전자 임원 클로드 코드 강의 · 2026-05-08
> Claude가 이 repo에서 모든 작업 시 자동 적용.

## §0. 환경 가정
- 실행 환경: Claude Cloud Default Sandbox
- 작업 디렉토리: 본 repo 루트 (Cloud 진입 시 자동 clone, **로컬 경로 가정 안 함**)
- 데이터: `./data/` (Olist 9 CSV 모두 commit, ODbL 라이선스)
- 사전 도구: `pandas`, `numpy`, `plotly` Sandbox 기본 제공
- 수강생 PC 환경: 무관

## §1. 사용자(수강생) 프로필
- 대상: LG전자 임원 (Python·pandas 비전공)
- 목표: 코드 학습이 아니라 **"AI가 데이터로 무엇을 해낼 수 있는지" 체감**
- 응대 톤: 비즈니스 임원에게 보고하듯, 코드 설명 최소화, **인사이트와 시각화 결과** 강조
- 언어: 한국어 응답 (단, 데이터 컬럼명·카테고리명은 영문 그대로 유지 — 인코딩 안전)

## §2. 출력 디렉토리
- 산출물 경로: `./output/<파일명>.html` (repo 내 상대경로, 실습별 하위 폴더 없음)
- `output/`은 `.gitignore`로 commit 제외 (`.gitkeep`만 추적)
- 수강생은 응답의 다운로드 링크로 로컬 PC에 저장

## §3. 도구 사용 우선순위
1. **`Bash` + `python -c "..."`** — 모든 데이터 처리 (pandas, plotly)
2. **`Write`** — HTML 산출물 저장
3. **`Edit`** — 기존 산출물 수정
4. **`Read`** — `./data/*.csv` 직접 읽기 **영구 금지**. 문서·코드 파일 참조용으로만
5. **`Glob` / `Grep`** — `./data/`·`./output/` 폴더 스캔용

## §4. 응답 형식
- 채팅 응답은 **결과 위주**, 중간 디버그 출력 최소화
- 긴 데이터프레임 절대 dump 금지 (§11.4)
- 작업 완료 시 항상:
  1. 산출물 상대경로 (`./output/...`)
  2. 다운로드 가능 여부
  3. 자가 검증 표 (§11.7)

## §5. 산출물 정책 (HTML 대시보드)
- **단일 self-contained HTML** — `dashboard_<설명>_YYYYMMDD_HHMM.html`
- **Plotly CDN 1개**: `fig.to_html(full_html=False, include_plotlyjs='cdn')` → 50KB 수준
- 사내 방화벽으로 CDN 차단 시 사용자 명시 요청에 한해 `include_plotlyjs=True` (5MB)
- 구성:
  - 상단 KPI 카드 3~5개 (HTML div 또는 `go.Indicator`)
  - 중단 Plotly 차트 2~5개 (`px.bar` / `px.scatter` / `px.pie(hole=0.4)` / `px.treemap` / `px.line`)
  - 하단 AI 인사이트 박스 3~5개 (§11.6)
- 반응형 폭: max-width 1200px (임원 데스크탑 기준)

## §6. 재시도 / 네이밍 규칙
- 같은 분석 재실행 시 파일명에 **하이픈 접미사**: `dashboard_olist_overview_20260508_1015-v2.html`, `-v3.html`
- 타임스탬프만으로 구분 안 될 때만 사용 (1분 내 재실행 등)
- 기존 파일 덮어쓰기 금지 (`-v2`/`-v3` 항상 신규 저장)

## §7. 보안 가이드
- Pro 플랜은 사용자 데이터 학습 비활용 (Anthropic 정책)
- 본 repo의 Olist 데이터는 공개 ODbL이므로 안전
- 단, 수강생이 사내 데이터를 Cloud Sandbox에 업로드하지 않도록 강사 멘트로 환기

## §8. 라이브러리 정책
- **허용**: `pandas`, `numpy`, `plotly` (Express + Graph Objects)
- **금지**: `matplotlib`, `seaborn`, `bokeh` (시각화 일관성, 한글 폰트 이슈)
- **조건부**: `scikit-learn` — 사용자 명시 요청 시만 (RFM 클러스터링 등)

## §9. 금지 사항
- **Sandbox 외부 시스템 변경 금지** (Cloud Sandbox 내부 처리에만 의존)
- **`./data/*.csv` 를 `Read` 도구로 직접 읽기 영구 금지**
- df 전체 dump (`print(df)`, `df.to_string()`, stdout 리디렉트) 영구 금지
- 데이터에 없는 추측 인사이트 작성 금지

---

## §11. 실습 3 전용 분석 규칙

### §11.1 작업 시작 검증
```bash
ls ./data/*.csv | wc -l   # 9 이어야 함
```
9가 아니면 즉시 사용자에게 알리고 작업 중단.

### §11.2 한글 폰트 정책
- Plotly는 브라우저 폰트 사용 → 한글 라벨 OK
- 단, 데이터 컬럼명/카테고리 값은 영문 그대로 유지
- 차트 제목·축 라벨·인사이트 텍스트만 한국어

### §11.3 표준 샘플링 + 5 CSV 로드 패턴 ⭐⭐⭐
명시적 다른 패턴 요청이 없는 한 다음을 사용:

```python
import pandas as pd
DATA = './data'

# 샘플링 (시드 고정)
orders = pd.read_csv(f'{DATA}/olist_orders_dataset.csv').sample(n=20000, random_state=42)
reviews = pd.read_csv(f'{DATA}/olist_order_reviews_dataset.csv').sample(n=20000, random_state=42)
items = pd.read_csv(f'{DATA}/olist_order_items_dataset.csv').sample(n=20000, random_state=42)

# 전체 로드 (가벼움)
products = pd.read_csv(f'{DATA}/olist_products_dataset.csv')           # 33K
customers = pd.read_csv(f'{DATA}/olist_customers_dataset.csv')         # 100K
payments = pd.read_csv(f'{DATA}/olist_order_payments_dataset.csv')     # 104K
translation = pd.read_csv(f'{DATA}/product_category_name_translation.csv')

# 제외: geolocation (1M 행 메모리), sellers (시연 외; 페어 응용 C에서만 별도 로드 3K)
```

**조인 표준 순서**:
```python
payments_agg = payments.groupby('order_id').agg(
    payment_value=('payment_value','sum'),
    payment_type=('payment_type','first'),
    payment_installments=('payment_installments','max')).reset_index()

df = (orders
      .merge(reviews, on='order_id', how='left')
      .merge(items, on='order_id', how='left')
      .merge(products, on='product_id', how='left')
      .merge(translation, on='product_category_name', how='left')
      .merge(customers, on='customer_id', how='left')
      .merge(payments_agg, on='order_id', how='left'))
```

**필터 표준**:
```python
df = df[df['order_status']=='delivered'].dropna(subset=['order_delivered_customer_date'])
df['order_purchase_timestamp'] = pd.to_datetime(df['order_purchase_timestamp'])
df['order_delivered_customer_date'] = pd.to_datetime(df['order_delivered_customer_date'])
df['delivery_days'] = (df['order_delivered_customer_date'] - df['order_purchase_timestamp']).dt.days
df = df[df['delivery_days'] >= 0]
```

### §11.4 출력 정책 (df dump 금지) ⭐⭐⭐
**허용**: `df.shape` / `df.head(5)` / `df.info()` / `df.describe()` / `groupby` 결과 / `value_counts()` 상위 N개
**금지**: `print(df)` / `df.to_string()` / stdout 리디렉트 / `Read`로 `./data/*.csv` 직접 읽기

위반 시 즉시 작업 중단하고 사용자에게 위반 내용 명시.

### §11.5 시각화 표준
- 카테고리/주(state) 비교 → `px.bar` 또는 `px.treemap`
- 시계열 → `px.line` (`pd.Grouper(freq='M')`)
- 분포 → `px.histogram` 또는 `px.box`
- 두 변수 관계 → `px.scatter`
- 비중 → `px.pie(hole=0.4)` (도넛)
- 모든 차트에 호버 정보 (`hover_data` 또는 `text_auto=True`)
- 차트 크기: `height=400` 권장

### §11.6 인사이트 형식 (3-line 패턴)
각 인사이트는 **[관찰] / [수치 근거] / [시사점]** 3-line 엄수:
```
1. [지리적 집중 ⚠] São Paulo 단일 주가 전체 GMV의 43.5%를 차지
   [수치 근거] n=99,441 주문 중 SP 47,829건, GMV R$5.92M / 전체 R$13.6M
   [시사점] 27개 주 중 1위 — 북부(NO/AM/PA) 진출이 다음 확장 레버
```
- 추측 금지 (예: "고객들이 LG 가전을 좋아할 것이다")
- 일반론 금지 (반드시 본 데이터 수치 인용)
- 5개 초과 금지 (임원 한 화면 흡수 한도 3~5개)

### §11.7 자가 검증 표 (응답 마지막 필수 출력)
```
| 항목 | 기준 | 결과 | ✓/✗ |
|------|------|------|-----|
| KPI 카드 수 | ≥ 3 | 4 | ✓ |
| 차트 수 | ≥ 2 | 5 | ✓ |
| 인사이트 수 | 3~5 | 5 | ✓ |
| df 전체 dump | 0회 | 0 | ✓ |
| 수치 검산 | 일치 | OK | ✓ |
| 한글 깨짐 | 0 | 0 | ✓ |
| 산출물 경로 | `./output/dashboard_*.html` | 실제경로 | ✓ |
```
모두 ✓이어야 작업 완료. ✗가 있으면 자동 보정 후 재출력.

### §11.8 시연 timeout 3단계 fallback
토큰 초과/응답 지연 시 자동 후퇴:
1. **1단계**: 샘플 절반 — orders/reviews 20K→10K, items 20K→12K
2. **2단계**: 단계적 분석 2회 콜 — Call 1 (orders+reviews 지역/만족), Call 2 (items+products+translation 카테고리/매출)
3. **3단계** (최후): 강사 사전 백업 dashboard.html 사용 안내

각 단계 진입 시 **"§11.8 fallback N단계 진입"** 메시지 응답에 포함.

### §11.9 자율 선택제
수강생이 본인 도메인 데이터 요청 시: `alternative-datasets.md` 큐레이션 표 1개 추천. 본 repo `./data/` 구조와 다르므로 표준 샘플링/조인 적용 안 됨 — 새 CLAUDE.md를 짧게 안내.

---

> 표준 코드 스니펫(`load_olist_standard()`, `build_dashboard()`)은 `README.md` §부록 참조. 변경 이력은 git log.
