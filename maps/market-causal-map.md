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
        FundamentalImprove[基本面惡化趨緩 / 改善]
    end

    subgraph Valuation[估值]
        DiscountRate[折現率]
        ValuationMultiple[估值倍數<br/>P/E、P/B 等]
        AttractiveValuation[估值進入有吸引力區間]
    end

    subgraph Positioning[資金與部位]
        Foreign[外資]
        Funds[投信 / 基金]
        Margin[融資]
        Shorts[放空部位]
        Leverage[市場槓桿]
        Deleveraging[去槓桿]
    end

    subgraph Psychology[市場心理]
        FOMO[FOMO / 追高]
        Fear[恐慌]
        Capitulation[投降式賣出<br/>Capitulation]
        BadNewsAbsorbed[利空不再造成新低]
    end

    subgraph PriceAction[價格與市場行為]
        Demand[買盤]
        Supply[賣壓]
        Price[股價 / 指數]
        Volatility[波動率]
        Stabilization[止穩]
        Rebound[反彈]
        Reversal[趨勢反轉]
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
    Capitulation -->|槓桿與弱手被清洗| Deleveraging
    Deleveraging -->|降低| Leverage
    Deleveraging -->|強迫性賣壓減弱| SupplyEase[賣壓減輕]

    SupplyEase --> BadNewsAbsorbed
    FundamentalImprove --> BadNewsAbsorbed
    BadNewsAbsorbed -->|價格開始鈍化| Stabilization
    Stabilization -->|若新買盤出現| Rebound

    AttractiveValuation --> Demand
    Liquidity -->|改善時| Rebound
    Shorts -->|回補時| Demand
    Rebound -->|基本面 / 流動性 / 籌碼持續確認| Reversal

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

### 4. 落底 → 止穩 → 反彈 → 反轉

```mermaid
flowchart TD
    A[價格持續下跌]
    --> B[恐慌 / 停損 / 斷頭 / Forced Selling]
    --> C[弱手與高槓桿部位出場]
    --> D[融資下降 / 去槓桿]
    --> E[強迫性賣壓減弱]

    E --> F{還有新的重大利空嗎?}

    F -->|有| G[基本面 / 政策 / 流動性繼續惡化]
    G --> H[價格可能繼續破底]

    F -->|沒有或已被充分反映| I[價格開始不再對利空敏感]
    I --> J[跌幅縮小]
    J --> K[成交量 / 波動率逐步下降]
    K --> L[低點不再明顯下移]
    L --> M[市場進入止穩階段]

    M --> N{是否出現新的買盤催化?}
    N -->|尚未| O[底部整理 / 橫盤]
    N -->|有| P[買盤增加]
    P --> Q[反彈開始]

    Q --> R{反彈是否能持續?}
    R -->|基本面改善| S[企業獲利 / 景氣預期改善]
    R -->|流動性改善| T[降息 / 資金回流 / 風險偏好回升]
    R -->|籌碼改善| U[融資低檔 / 空單回補 / 賣壓枯竭]
    R -->|技術面改善| V[突破關鍵壓力 / 高低點墊高]

    S --> W[反彈更可能演變成趨勢反轉]
    T --> W
    U --> W
    V --> W
```

### 5. 利空不再創新低

```mermaid
flowchart LR
    A[利空出現]
    --> B{價格是否再創新低?}

    B -->|是| C[市場仍在 Price Discovery / 去槓桿]
    B -->|否| D[賣壓可能接近耗盡]
    D --> E[止穩機率提高]
```

### 6. 下跌迴圈如何切換成落底迴圈

```mermaid
flowchart LR
    A[價格下跌]
    --> B[恐慌 / 追繳 / 斷頭]
    --> C[賣壓增加]
    --> D[價格進一步下跌]

    D --> E[去槓桿]
    --> F[弱手出場]
    --> G[強迫性賣壓下降]
    --> H[利空不再創新低]
    --> I[止穩]

    I --> J[資金回流 / 空單回補 / 基本面改善]
    --> K[反彈]
    --> L{是否有持續性支撐?}

    L -->|有| M[趨勢反轉]
    L -->|沒有| N[Dead Cat Bounce / 再次測底]
```

## 落底、止穩、反彈、反轉不是同一件事

```mermaid
flowchart LR
    A[落底 Bottoming]
    -->|下跌動能開始耗盡| B[止穩 Stabilization]
    -->|價格不再持續創低| C[反彈 Rebound]
    -->|買盤回來| D[趨勢反轉 Reversal]
```

- **落底 Bottoming**：賣壓與去槓桿機制逐步耗盡的過程。
- **止穩 Stabilization**：價格開始不再持續創低，但不代表已經轉多。
- **反彈 Rebound**：短期買盤開始大於賣壓，價格向上修復。
- **趨勢反轉 Reversal**：基本面、流動性、籌碼或價格結構開始支持更持續的上升趨勢。

> 關鍵觀念：**落底不是因為「跌很多了」，而是因為導致下跌的賣壓機制開始耗盡。**

## 使用方式

之後每一個主題都應該盡量回答：

1. **Trigger**：什麼事情先發生？
2. **Mechanism**：中間透過什麼機制傳導？
3. **Observable evidence**：市場上可以觀察什麼資料？
4. **Feedback loop**：是否存在正回饋或負回饋？
5. **Invalidation**：什麼條件下這條因果鏈不成立？

未來新增的主題頁，應該回連到這張因果網路，而不是成為孤立筆記。
