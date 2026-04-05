# Shopee Price Tracker

[![Bright Data](https://img.shields.io/badge/Powered%20by-Bright%20Data-blue?style=flat-square)](https://brightdata.co.kr)
[![Shopee Price Tracker](https://img.shields.io/badge/Shopee%20Price%20Tracker-Managed%20Solution-orange?style=flat-square)](https://brightdata.co.kr/products/insights/price-tracker/shopee)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow?style=flat-square)](https://python.org)

[![Bright Insights Price Tracker](https://raw.githubusercontent.com/danielshashko/bright-insights-assets/main/price-tracker-hero-v2.png)](https://brightdata.co.kr/products/insights/price-tracker/shopee)

동남아시아와 대만 전역의 대표적인 e-commerce 플랫폼인 Shopee의 실시간 가격 추적. 시작하는 방법은 두 가지입니다: **완전 관리형** 인텔리전스 플랫폼 또는 자체 파이프라인을 구축할 수 있는 **셀프서비스 API**.

---

## Option 1: Bright Insights - AI 기반 가격 추적 (권장)

**[Bright Insights](https://brightdata.co.kr/products/insights/price-tracker/shopee)**는 Bright Data의 완전 관리형 리테일 인텔리전스 플랫폼입니다. scraper를 구축할 필요도, 인프라를 유지할 필요도 없습니다. 구조화되고 분석 준비가 완료된 가격 데이터가 대시보드, 데이터 피드 또는 BI 도구로 바로 전달됩니다.

**팀이 Bright Insights를 선택하는 이유:**
- 🚀 **설정 불필요** - 바로 사용할 수 있는 대시보드와 데이터 피드로 몇 분 안에 운영 시작
- 🤖 **AI 기반 추천** - 대화형 AI assistant가 수백만 개의 데이터 포인트를 즉시 실행 가능한 인사이트로 전환
- ⚡ **실시간 모니터링** - 시간 단위부터 일 단위까지의 refresh 주기와 즉시 알림(email, Slack, webhook)
- 🌍 **무제한 확장성** - 모든 웹사이트, 모든 지역, 모든 refresh 빈도 지원
- 🔗 **Plug-and-play integrations** - AWS, GCP, Databricks, Snowflake 등과 연동
- 🛡️ **완전 관리형** - Bright Data가 schema 변경, 사이트 업데이트, 데이터 품질을 자동으로 처리

**주요 사용 사례:**
- ✅ 수백만 개 SKU에 걸쳐 **Shopee 가격을 실시간 모니터링**
- ✅ **경쟁사 가격 추적** 및 할인 패턴 식별
- ✅ Shopee에서 경쟁력을 유지하기 위한 **자동 가격 재조정**
- ✅ MAP 정책 준수 모니터링 및 가격 위반 감지
- ✅ 경쟁사 프로모션 및 프로모션 동향 추적
- ✅ 정제되고 표준화된 데이터를 동적 가격 책정 알고리즘 또는 AI 모델에 직접 공급

> **월 $250부터 - [맞춤 견적 받기 →](https://brightdata.co.kr/products/insights/price-tracker/shopee)**

---

## Option 2: Web Scraper API를 통한 셀프서비스

직접 파이프라인을 구축하고 싶으신가요? Bright Data의 **Web Scraper API**는 proxy나 scraping 인프라를 관리하지 않고도 Shopee 상품 데이터(가격, 재고 여부, 리뷰 등)에 programmatic access를 제공합니다.

### Prerequisites

- Python 3.9 이상
- [Bright Data account](https://brightdata.co.kr) (free trial 제공)
- Bright Data **API token** ([발급 방법](https://docs.brightdata.co.kr/general/account/account-settings#api-token))
- Shopee 상품용 Bright Data **Web Scraper ID** ([Web Scrapers control panel에서 확인](https://brightdata.co.kr/cp/scrapers))

### Setup

1. **이 repository 복제**

   ```bash
   git clone https://github.com/bright-kr/shopee-price-tracker.git
   cd shopee-price-tracker
   ```

2. **dependencies 설치**

   ```bash
   pip install -r requirements.txt
   ```

3. **credentials 구성**

   `.env.example`을 `.env`로 복사한 뒤 값을 입력하세요:

   ```bash
   cp .env.example .env
   ```

   ```env
   BRIGHTDATA_API_TOKEN=your_api_token_here
   BRIGHTDATA_DATASET_ID=your_dataset_id_here
   ```

   > **Web Scraper ID 찾기**
   > [Bright Data Control Panel](https://brightdata.co.kr/cp/scrapers)에 로그인한 후 **Web Scrapers**로 이동하세요.
   > "Shopee"를 검색하고 Web Scraper ID를 복사하세요(형식: `gd_xxxxxxxxxxxx`).

---

## Usage

### 1. URL로 특정 상품 추적

Shopee 상품 URL 목록을 전달하여 구조화된 가격 데이터를 가져옵니다:

```python
from price_tracker import track_prices

urls = [
    "https://shopee.com/sample-product-i.12345.67890",
    # Add more product URLs here
]

results = track_prices(urls)
for item in results:
    print(f"{item.get('title')} - {item.get('final_price', item.get('price'))} {item.get('currency', '')}")
```

또는 직접 실행:

```bash
python price_tracker.py
```

### 2. 키워드로 상품 검색

키워드 검색과 일치하는 상품을 찾습니다:

```python
from price_tracker import discover_by_keyword

results = discover_by_keyword("laptop", limit=50)
```

### 3. 카테고리 URL로 상품 탐색

Shopee 카테고리 페이지의 모든 상품을 수집합니다:

```python
from price_tracker import discover_by_category

results = discover_by_category(
    "https://shopee.com/category/example",
    limit=100,
)
```

---

## Output Fields

각 결과 레코드에는 다음 필드가 포함됩니다:

| Field | Description |
|-------|-------------|
| `url` | 상품 페이지 URL |
| `title` | 상품명 / 제목 |
| `brand` | 브랜드 또는 제조사 |
| `initial_price` | 원래 가격 / 정가 |
| `final_price` | 현재 판매 가격 |
| `currency` | 통화 코드 (예: USD, EUR) |
| `discount` | 할인 금액 또는 할인율 |
| `in_stock` | 상품 구매 가능 여부 |
| `rating` | 평균 별점 |
| `reviews_count` | 총 리뷰 수 |
| `seller_name` | 판매자 이름 |
| `images` | 상품 이미지 URL 배열 |
| `description` | 상품 설명 텍스트 |
| `timestamp` | 데이터 수집 타임스탬프 |

### Sample output

```json
[
  {
    "url": "https://shopee.com/sample-product-i.12345.67890",
    "title": "Example Product Name",
    "brand": "Example Brand",
    "initial_price": 59.99,
    "final_price": 44.99,
    "currency": "USD",
    "discount": "25%",
    "in_stock": true,
    "rating": 4.5,
    "reviews_count": 1234,
    "images": ["https://shopee.com/images/product1.jpg"],
    "description": "Product description text...",
    "timestamp": "2025-01-15T10:30:00Z"
  }
]
```

---

## Advanced Options

`trigger_collection()` 함수는 데이터 수집을 제어하기 위한 선택적 parameter를 받습니다:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | - | 반환할 최대 레코드 수 |
| `include_errors` | boolean | `true` | 결과에 오류 보고 포함 |
| `notify` | string (URL) | - | snapshot 준비 완료 시 호출할 webhook URL |
| `format` | string | `json` | 출력 형식: `json`, `csv` 또는 `ndjson` |

옵션 사용 예시:

```python
from price_tracker import trigger_collection, get_results

inputs = [{"url": "https://shopee.com/sample-product-i.12345.67890"}]
snapshot_id = trigger_collection(inputs, limit=200, notify="https://your-webhook.com/hook")
results = get_results(snapshot_id)
```

---

## Resources

- 🌟 [Shopee Price Tracker - Bright Insights (Managed)](https://brightdata.co.kr/products/insights/price-tracker/shopee)
- 🔧 [Shopee Scraper API](https://brightdata.co.kr/products/web-scraper/shopee)
- 📖 [Bright Data Web Scraper API Documentation](https://docs.brightdata.co.kr/scraping-automation/web-scraper-api/overview)
- 🗄️ [Web Scrapers Control Panel](https://brightdata.co.kr/cp/scrapers)
- 🔑 [How to get an API token](https://docs.brightdata.co.kr/general/account/account-settings#api-token)
- 🌐 [Bright Data Homepage](https://brightdata.co.kr)

---

*[Bright Data](https://brightdata.co.kr)로 구축 - 업계를 선도하는 웹 데이터 플랫폼.*