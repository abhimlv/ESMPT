# 📊 Evaluation of Stock Market Prediction Techniques 
![Python](https://img.shields.io/badge/Python-3.11-blue)
![License](https://img.shields.io/badge/License-MIT-green)

This project compares the predictive performance of **ARIMA**, **LSTM**, and **Temporal Convolutional Network (TCN)** models on the Dow Jones Industrial Average (DJIA). A **multimodal dataset** combining technical indicators, sentiment scores, and intent signals from news and Reddit was engineered to forecast closing prices across **three future horizons**: `t+1`, `t+3`, and `t+7`.

📄 [Read the Full Report (PDF)](https://github.com/abhimlv/ESMPT/blob/main/Documentation/Report/Evaluation_of_Stock_Market_Prediction_Techniques_Report___Abhishek_Malaviya%20(1).pdf)

---

## 📚 Table of Contents
- [Dataset & Features](#-dataset--features)
- [Modeling Approach](#-modeling-approach)
- [Model Results](#-model-results-train--test)
- [SHAP Analysis](#-shap-analysis-tcn)
- [Key Takeaways](#-key-takeaways)
- [Future Work](#-future-work)
- [Citation](#-citation)
- [Resources](#-resources)

---

## 📦 Dataset & Features

- **Dataset Size**: ~1,900 trading days
- **Sources**: Kaggle DJIA, financial news, Reddit discussions
- **Features**:
  - Technical: Log Returns, Volatility (Log10), Percentage Change
  - Sentiment: VADER, FinBERT, Smart Sentiment
  - Intent: Buying, Selling, Fear, Uncertainty, Urgency, etc.
- **Normalization**: All sentiment and intent features scaled to [-1, 1] ranges

📂 [Dataset on Hugging Face](https://huggingface.co/datasets/abhimlv/ESMPT-Dataset)

---

## 🧪 Modeling Approach

- **Prediction Targets**:
  - t+1
  - t+3
  - t+7
- **Train/Test Split**: Chronological 80/20 split
- **Evaluation Metrics**: R², RMSE, MAE
- **Pipeline**:
  - Feature engineering → Normalization
  - Sequence generation (60 for LSTM, 30 for TCN)
  - Model training → Testing → Residual analysis

---

## 🔍 Model Results (Train & Test)

### ARIMA

| Horizon | Train R² | Train RMSE | Test R² | Test RMSE |
|---------|----------|------------|---------|-----------|
| t+1     | 0.9921   | 238.87     | 0.8655  | 228.79    |
| t+3     | 0.9877   | 297.54     | 0.7433  | 316.38    |
| t+7     | 0.9800   | 380.06     | 0.5662  | 413.11    |

> ARIMA excelled in short-term forecasts but declined sharply for longer horizons due to lack of external signal integration.

![ARIMA Forecast t+1](https://raw.githubusercontent.com/abhimlv/ESMPT/main/Plots/arima_test_t_plus_1.png)

---

### LSTM (Simple)

| Horizon | Train R² | Train RMSE | Test R² | Test RMSE |
|---------|----------|------------|---------|-----------|
| t+1     | 0.9881   | 294.14     | 0.7606  | 319.58    |
| t+3     | 0.9863   | 315.44     | 0.2669  | 559.01    |
| t+7     | 0.9855   | 323.81     | 0.4156  | 501.07    |

> Simple LSTM generalized well for t+1 but struggled beyond that, underfitting in volatile regimes.

![Simple LSTM Forecast t+1](https://raw.githubusercontent.com/abhimlv/ESMPT/main/Plots/lstm_test_t_plus_1_simple.png)

---

### LSTM (Stacked)

| Horizon | Train R² | Train RMSE | Test R² | Test RMSE |
|---------|----------|------------|---------|-----------|
| t+1     | 0.9826   | 355.83     | 0.3267  | 535.94    |
| t+3     | 0.9858   | 320.75     | 0.3985  | 506.37    |
| t+7     | 0.9700   | 466.63     | 0.4252  | 496.95    |

> While overfitting at t+1, stacked LSTM showed improved stability at longer horizons, especially t+7.

![Stacked LSTM Forecast t+7](https://raw.githubusercontent.com/abhimlv/ESMPT/main/Plots/lstm_test_t_plus_7_stacked.png)

---

### Temporal Convolutional Network (TCN)

| Horizon | Train R² | Train RMSE | Test R² | Test RMSE |
|---------|----------|------------|---------|-----------|
| t+1     | 0.9953   | 186.12     | 0.8767  | 225.27    |
| t+3     | 0.9910   | 257.10     | 0.7584  | 316.20    |
| t+7     | 0.9831   | 351.86     | 0.5483  | 434.50    |

> TCN consistently outperformed all other models at t+1 and t+3, showing superior temporal generalization and faster trend adaptation.

![TCN Forecast t+1](https://raw.githubusercontent.com/abhimlv/ESMPT/main/Plots/tcn_test_t_plus_1.png)

---

## 📊 SHAP Analysis (TCN)

- **t+1**: Smart Reddit Sentiment, Greed Intent dominated  
- **t+3**: Reddit Prediction Intent, News Urgency Intent had highest weight  
- **t+7**: Aggregated sentiment took over, technical signals ranked low

![SHAP Importance t+1](https://raw.githubusercontent.com/abhimlv/ESMPT/main/Plots/tcn_logret_tplus1_shap_kernel.png)

---

## 📈 Model Comparison Summary

| Model       | Best Horizon | Test R² | Comment                        |
|-------------|--------------|---------|--------------------------------|
| ARIMA       | t+1          | 0.8655  | Best short-term linear baseline |
| LSTM Simple | t+1          | 0.7606  | Good on short-term, degrades later |
| LSTM Stacked| t+7          | 0.4252  | Handles longer horizons better |
| TCN         | t+1, t+3     | 0.8767 / 0.7584 | Best overall performer |

---

## 🧠 Key Takeaways

- Multimodal features (especially Reddit and news intent) boost predictive power.
- TCN dominates short-term forecasting with minimal lag and high generalization.
- ARIMA remains a solid short-term benchmark.
- Stacked LSTM shows resilience in volatile mid-horizon targets.

---

## 🚀 Future Work

- Integrate high-frequency trading and macroeconomic data
- Explore hybrid ensembles (ARIMA + TCN + LSTM)
- Add attention-based forecasting models like Informer/Transformer
- Deploy as a **live dashboard** using Streamlit + Hugging Face APIs


---

## 📝 Citation

> Malaviya, A. (2025). *Evaluation of Stock Market Prediction Techniques.* Dublin City University.

📄 [Download Report (PDF)](https://github.com/abhimlv/ESMPT/blob/main/Documentation/Report/Evaluation_of_Stock_Market_Prediction_Techniques_Report___Abhishek_Malaviya%20(1).pdf)


---

## 📂 Resources

- 💻 [Ready Tensor Publication](https://app.readytensor.ai/publications/evaluation-of-stock-market-prediction-techniques-zVwmjJA8eDi3)
- 🤖 [Trained models on Hugging Face](https://huggingface.co/abhimlv/ESMPT-Models)
- 📦 [Dataset on Hugging Face](https://huggingface.co/datasets/abhimlv/ESMPT-Dataset)
- 📄 [Full Report (PDF)](https://github.com/abhimlv/ESMPT/blob/main/Documentation/Report/Evaluation_of_Stock_Market_Prediction_Techniques_Report___Abhishek_Malaviya%20(1).pdf)
- 📝 [Literature Review (PDF)](https://github.com/abhimlv/ESMPT/blob/main/Documentation/Lit%20Review/Evaluation%20of%20Stock%20Market%20Prediction%20Techniques%20Lit%20Review%20(5).pdf)

---

## 🤝 Contributing
Feel free to open issues or submit pull requests. For major changes, please open a discussion first.

## 📬 Contact
For queries, email: abhishek.malaviya2@mail.dcu.ie
