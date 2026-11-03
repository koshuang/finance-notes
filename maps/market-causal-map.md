# 市場因果網路圖

這份圖是 `finance-notes` 的核心入口之一，用來回答一個問題：**什麼因素會透過什麼機制，影響市場價格、風險與籌碼？**

> 箭頭表示「常見影響方向」，不是保證結果。市場中的因果通常會被時間、估值、政策、流動性與市場預期共同改變。

```mermaid
flowchart TD
    subgraph Macro[總體環境]
        Inflation[通膨]
        Rates[利率]
        Growth[經濟成長]
        Policy[財政 / 貨幣政策]
        Liquidity[市場流動性]
    end

    subgraph Fundamentals[基本面]
        Revenue[企業營收]
        Earnings[企業獲利]
        Expectations[市場預期]
    end

    subgraph Valuation[估值]
        DiscountRate[折現率]
        ValuationMultiple[估值倍數<br/>P/E、P/B 等]
    end

    subgraph Positioning[資金與部位]
        Foreign[外資]
        Funds[投信 / 基金]
        Margin[融資]
        Shorts[放空部位]
        Leverage[市場槓桿]
    end

    subgraph Psychology[市場心理]
        FOMO[FOMO / 追高]
        Fear[恐慌]
        Capitulation[投降式賣出<br/>Capitulation]
    end

    subgraph PriceAction[價格與市場行為]
        Demand[買盤]
        Supply[賣壓]
        Price[股價 / 指數]
        Volatility[波動率]
    end

    Inflation -->|偏高時可能促使| Rates
    Policy --> Rates
    Policy --> Liquidity
    Growth --> Revenue
    Revenue --> Earnings

    Rates -->|提高| DiscountRate
    DiscountRate -->|壓縮| ValuationMultiple
    Earnings --> Price
    Expectations --> ValuationMultiple
    ValuationMultiple --> Price

    Liquidity -->|增加可用資金| Demand
    Foreign --> Demand
    Foreign --> Supply
    Funds --> Demand
    Funds --> Supply

    Price -->|持續上漲時容易引發| FOMO
    FOMO --> Demand
    FOMO --> Margin
    Margin --> Leverage
    Leverage -->|放大行情| Price

    Price -->|快速下跌時容易引發| Fear
    Fear --> Supply
    Fear --> Volatility
    Volatility -->|提高風險感受| Fear

    Price -->|跌破融資戶承受範圍| MarginCall[追繳 / 停損 / 斷頭]
    Margin --> MarginCall
    MarginCall --> Supply
    Supply -->|增加| PriceDown[價格進一步下跌]
    PriceDown --> Fear
    PriceDown --> MarginCall

    PriceDown -->|極端時| Capitulation
    Capitulation --> Supply
    Capitulation -->|槓桿與弱手被清洗| Deleveraging[去槓桿]
    Deleveraging -->|降低| Leverage
    Deleveraging -->|強迫性賣壓減弱| SupplyEase[賣壓減輕]

    Shorts -->|空單增加| Supply
    Price -->|上漲迫使回補| ShortCover[空單回補]
    ShortCover --> Demand

    Demand -->|推升| Price
    Supply -->|壓低| Price
```

## 目前先抓住的核心因果鏈

### 1. 利率 → 估值

```mermaid
flowchart LR
    A[通膨偏高] --> B[利率偏高]
    B --> C[折現率提高]
    C --> D[估值倍數下降]
    D --> E[股價承壓]
```

### 2. 融資 → 多殺多

```mermaid
flowchart LR
    A[股價下跌]
    --> B[融資戶虧損放大]
    --> C[停損 / 追繳 / 斷頭]
    --> D[賣壓增加]
    --> E[股價進一步下跌]
    --> A
```

### 3. 恐慌 → 去槓桿 → 賣壓減弱

```mermaid
flowchart LR
    A[恐慌下跌]
    --> B[弱手與槓桿部位出場]
    --> C[去槓桿]
    --> D[強迫性賣壓逐步減少]
    --> E[若基本面未惡化<br/>反彈條件改善]
```

## 使用方式

之後每一個主題都應該盡量回答：

1. **Trigger**：什麼事情先發生？
2. **Mechanism**：中間透過什麼機制傳導？
3. **Observable evidence**：市場上可以觀察什麼資料？
4. **Feedback loop**：是否存在正回饋或負回饋？
5. **Invalidation**：什麼條件下這條因果鏈不成立？

未來新增的主題頁，應該回連到這張因果網路，而不是成為孤立筆記。
