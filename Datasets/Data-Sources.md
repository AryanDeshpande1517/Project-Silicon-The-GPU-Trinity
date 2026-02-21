# 📂 Dataset Documentation

This project uses publicly available datasets sourced from Kaggle.

## 🖥 GPU Specifications Dataset

**Name:** Graphics Card Full Specs  
**Source:** Kaggle  
**Link:** https://www.kaggle.com/datasets/alanjo/graphics-card-full-specs  

### Description
Contains detailed GPU-level technical specifications including:

- GPU model names  
- Core clock speeds  
- Memory size (VRAM)  
- Unified shader count  
- Memory bandwidth  
- Architecture details  

This dataset was used to analyze hardware evolution and performance trends across NVIDIA, AMD, and Intel.

## 📈 Stock Market Dataset

**Name:** NVIDIA, AMD, Intel Share Prices  
**Source:** Kaggle  
**Link:** https://www.kaggle.com/datasets/kapturovalexander/nvidia-amd-intel-asus-msi-share-prices  

### Description
Contains historical stock market data including:
- Open, High, Low, Close prices  
- Trading volume  
- Date-based financial tracking  

This dataset was used to model growth trends, volatility, and innovation-to-valuation correlation.

## ⚠️ Licensing Notice

The datasets used in this project are publicly available on Kaggle.

Raw dataset files are **not redistributed in this repository** due to Kaggle dataset licensing terms.

To reproduce this project:

1. Download datasets directly from the provided Kaggle links.
2. Import them into Power BI.
3. Apply the data modeling and DAX measures described in the repository.

## 🧹 Data Preparation

All datasets were:

- Cleaned using Power Query (ETL)
- Standardized for date formats
- Modeled using a Star Schema structure
- Connected via a Master Calendar table