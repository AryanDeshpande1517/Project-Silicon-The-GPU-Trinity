# 📊 DAX Measures Documentation

This document outlines the key DAX measures used in **Project Silicon: The GPU Trinity**.

The measures are grouped into:
- Hardware Analytics  
- Top GPU Identification  
- Financial Analytics  
- Time Intelligence  
- Growth & Performance Metrics  
- Statistical Modeling  
- UI Formatting  

All measures were implemented inside Power BI using DAX.

# 🖥 Hardware Analytics

## Avg Clock Speed
Calculates the average GPU core clock speed.

```DAX
Avg Clock Speed = 
AVERAGE('gpu_specs'[gpuClock])
```

## Avg VRAM (GB)
Computes the average memory size (in GB) across GPUs.

```DAX
Avg VRAM (GB) = 
AVERAGE('gpu_specs'[memSize])
```

## Model Count
Counts total GPU models available.

```DAX
Model Count = 
COUNT('gpu_specs'[productName])
```

# 🏆 Top GPU Identification
These measures dynamically identify the most powerful GPU per brand based on unified shader count.

## Top NVIDIA GPU

```DAX
Top NVIDIA GPU =
CALCULATE(
    FIRSTNONBLANK('gpu_specs'[productName], 1),
    TOPN(
        1,
        FILTER(
            'gpu_specs',
            CONTAINSSTRING('gpu_specs'[productName], "GeForce")
        ),
        'gpu_specs'[unifiedShader],
        DESC
    )
)
```

## Top AMD GPU

```DAX
Top AMD GPU =
CALCULATE(
    FIRSTNONBLANK('gpu_specs'[productName], 1),
    TOPN(
        1,
        FILTER(
            'gpu_specs',
            CONTAINSSTRING('gpu_specs'[productName], "Radeon") &&
            NOT(CONTAINSSTRING('gpu_specs'[productName], "Mobile")) &&
            NOT(CONTAINSSTRING('gpu_specs'[productName], "Pro"))
        ),
        'gpu_specs'[unifiedShader],
        DESC
    )
)
```

## Top Intel GPU

```DAX
Top Intel GPU =
CALCULATE(
    FIRSTNONBLANK('gpu_specs'[productName], 1),
    TOPN(
        1,
        FILTER(
            'gpu_specs',
            CONTAINSSTRING('gpu_specs'[productName], "Arc")
        ),
        'gpu_specs'[unifiedShader],
        DESC
    )
)
```

# 📈 Financial Analytics

## Avg Stock Price

```DAX
Avg Stock Price = 
AVERAGE('Master_Stocks'[Close])
```

## All-Time High

```DAX
All-Time High = 
MAX('Master_Stocks'[High])
```

## Total Volume

```DAX
Total Volume = 
SUM('Master_Stocks'[Volume])
```

## Stock Volatility
Measures population standard deviation of closing prices.

```DAX
Stock Volatility = 
STDEV.P('Master_Stocks'[Close])
```

# 📅 Time Intelligence

## Master Calendar
Creates a dynamic calendar table based on stock date range.

```DAX
Master Calendar =
CALENDAR(
    MIN('Master_Stocks'[Date]),
    MAX('Master_Stocks'[Date])
)
```

## Year Column

```DAX
Year = 
YEAR('Master Calendar'[Date])
```

# 📊 Growth & Performance Metrics

## Growth Multiplier
Computes total percentage growth between earliest and latest dates in context.

```DAX
Growth Multiplier =
VAR MinDate = MIN('Master_Stocks'[Date])
VAR MaxDate = MAX('Master_Stocks'[Date])

VAR StartPrice =
    CALCULATE(
        AVERAGE('Master_Stocks'[Close]),
        'Master_Stocks'[Date] = MinDate
    )

VAR EndPrice =
    CALCULATE(
        AVERAGE('Master_Stocks'[Close]),
        'Master_Stocks'[Date] = MaxDate
    )

RETURN
    DIVIDE(EndPrice - StartPrice, StartPrice)
```

## Total Growth %

```DAX
Total Growth % =
VAR StartPrice =
    CALCULATE(
        AVERAGE('Master_Stocks'[Close]),
        FILTER(
            'Master_Stocks',
            'Master_Stocks'[Date] = MIN('Master_Stocks'[Date])
        )
    )

VAR EndPrice =
    CALCULATE(
        AVERAGE('Master_Stocks'[Close]),
        FILTER(
            'Master_Stocks',
            'Master_Stocks'[Date] = MAX('Master_Stocks'[Date])
        )
    )

RETURN
    DIVIDE(EndPrice - StartPrice, StartPrice)
```

# 📊 Statistical Modeling

## Correlation (Price vs VRAM)

Implements Pearson correlation between average stock price and average VRAM size.

```DAX
Correlation (Price vs VRAM) =
VAR CorrelationTable =
    FILTER(
        SUMMARIZE(
            'Master Calendar',
            'Master Calendar'[Year],
            "Stock_Price", [Avg Stock Price],
            "VRAM_Size", [Avg VRAM (GB)]
        ),
        NOT(ISBLANK([Stock_Price])) &&
        NOT(ISBLANK([VRAM_Size]))
    )

VAR Avg_Price =
    AVERAGEX(CorrelationTable, [Stock_Price])

VAR Avg_VRAM =
    AVERAGEX(CorrelationTable, [VRAM_Size])

VAR Numerator =
    SUMX(
        CorrelationTable,
        ([Stock_Price] - Avg_Price) *
        ([VRAM_Size] - Avg_VRAM)
    )

VAR Denominator =
    SQRT(
        SUMX(
            CorrelationTable,
            ([Stock_Price] - Avg_Price)^2
        ) *
        SUMX(
            CorrelationTable,
            ([VRAM_Size] - Avg_VRAM)^2
        )
    )

RETURN
    DIVIDE(Numerator, Denominator)
```

# 🎨 UI Formatting Measures

Used for dynamic brand-based conditional formatting inside visuals:

- Border_NVIDIA  
- Border_AMD  
- Border_Intel  
- Border_ASUS  

These measures dynamically apply company brand colors to visual elements based on selected context.

# 📝 Notes

- All measures were implemented inside Power BI.  
- Data transformations were performed using Power Query (ETL).  
- The data model follows a Star Schema design.  
- Statistical modeling (correlation) was implemented purely in DAX without external libraries.