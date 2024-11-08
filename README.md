<h1 align="center">
    Stock Price Forecasting Using xLSTM
    <br>
</h1>


<h3 align="center">
    Harnessing the Power of xLSTM for Time Series Forecasting
    <br>
</h3>

<h3 align="center">
    <a href="https://github.com/GalMichaeli">Gal Michaeli</a> •
    <a href="https://github.com/SergeyMashkin">Sergey Mashkin</a>
</h3>

<h4 align="center">Technion ECE 046211 Spring 2024 Final Project</h4>

## Overview
This project aims to utilize the [xLSTM](https://arxiv.org/abs/2405.04517) 
block architecture for prediction of stock prices.
xLSTM blocks, and in particular mLSTM blocks, boast improvements over 
traditional LSTM blocks and Transformer capabilities, mainly due to their matrix 
memory and covariance update rule. To take full advantage of these improved capabilities, 
we constructed a model specifically designed for prediction of time series with non-stationary 
distributions. We based our design on [xLSTMTime](https://arxiv.org/pdf/2407.10240), with some 
modifications and adaptations.

## Contents
- [Previous and Related Work](#previous-and-related-work)
- [Dependencies](#dependencies)
- [How to Run](#how-to-run)
- [Data Acquisition and Preprocessing](#data-acquisition-and-preprocessing)
- [Model Architecture](#model-architecture)
- [Results](#results)
- [Repository Organization](#repository-organization)
- [References](#references)
- [Acknowledgements](#acknowledgements)
- [Disclaimer](#disclaimer)
- [License](#license)

## Previous and Related Work
1. Official xLSTM implementation
   - https://github.com/NX-AI/xlstm/tree/main
2. Official xLSTMTime implementation
   - https://github.com/muslehal/xLSTMTime/tree/main
3. Stock price prediction using RWKV
   - https://github.com/tomer9080/Stock-Prediction-Using-RWKV/tree/main

## Dependencies
| Library  | Version |
| -------- | ------- |
| `Python` | `3.10.12` |
| `torch`  | `2.4.1` |
| `torch-summary` | `1.4.5` |
| `xlstm`    | `1.0.7`  |
| `cuda` | `12.1` |
| `matplotlib` | `3.7.1` |
| `numpy` | `1.26.4` |
| `pandas` | `2.2.2` |
| `scikit-learn` | `1.5.2` |
| `yfinance` | `0.2.44` |
| `Ninja`    | `1.11.1.1` |

## How to Run
Download the notebook and run in Google Colaboratory.

## Configuration and Hyperparameters Used
| Library  | Version |
| -------- | ------- |
| `Batch Size` | `64` |
| `Epochs`  | `50` |
| `Learning Rate Schedule` | `ReduceLROnPlateau` |
| `Init. Learning Rate` | `1e-4` |
| `Train-Val-Test Split`    | `64% - 16% - 20%`  |
| `Scaler` | `MinMaxScaler` |
| `Number of mLSTM blocks` | `3` |
| `Embedding dimension` | `128` |
| `Moving average kernel size` | `25` |
| `Loss Function` | `MSE loss` |
| `Optimization Algorithm` | `RAdam` |
The batch size, learning rate and scheduler hyperparameters were optimized with Optuna.

## Data Acquisition and Preprocessing
For better comparison to similar previous projects, the model was trained on the Coca-Cola daily data, obtained through ``` yfinance ``` API
using the following features: Open, High, Low, Close and Adjusted Close prices, and the daily Volume.
Data preprocessing included applying a simple moving average with window of length 5 and transforming the prices to their natural logarithm.
Due to the length of the time series, we opted to assign only 64% to training, 16% to validation and the remaining 20% to testing.

## Model Architecture
As recommended by [xLSTMTime](https://arxiv.org/pdf/2407.10240), the architecture we used is as depicted in the
following figure.
<p align="center">
  <img src="https://github.com/GalMichaeli/046211-Stock-Price-Forecasting-with-xLSTM/blob/main/assets/model-arch.png" width="1000"/>
</p>

## Results
When viewing the forecast against the ground truth prices for the whole duration of the time series, as shown in the following figure, the model's prediction capabilities are quite impressive:
<p align="center">
  <img src="https://github.com/GalMichaeli/046211-Stock-Price-Forecasting-with-xLSTM/blob/main/assets/general_KO.png"/>
</p>

A closer look into the performance over the **test set** reveals the deviations and inaccuracies of the prediction:
<p align="center">
  <img src="https://github.com/GalMichaeli/046211-Stock-Price-Forecasting-with-xLSTM/blob/main/assets/zoom_KO.png"/>
</p>

Taking the *Coca-Cola*-trained model and forecasting prices of other stocks from different market sectors
yields impressive results as well.

On Amazon stock:
<p align="center">
  <img src="https://github.com/GalMichaeli/046211-Stock-Price-Forecasting-with-xLSTM/blob/main/assets/zoom_AMZN.png"/>
</p>

On Pfizer stock:
<p align="center">
  <img src="https://github.com/GalMichaeli/046211-Stock-Price-Forecasting-with-xLSTM/blob/main/assets/zoom_PFE.png"/>
</p>

Lastly, we experimented with autoregressively forecasting the *Coca-Cola* price 100 days into the future,
resulting in non-satisfactory performance:

https://github.com/user-attachments/assets/9b365589-fe91-4cc0-b0a4-efb39a64f50e

## Performance comparssion with RWKV:
We took existed [RWKV](https://github.com/tomer9080/Stock-Prediction-Using-RWKV/tree/main) model, which is based on transformers and is considered superior to traditional LSTM models. We re-trained RWKV model on the same stock and compared it with xLSTM model.

On Coca-Cola stock:
<p align="center">
  <img src="https://github.com/GalMichaeli/046211-Stock-Price-Forecasting-with-xLSTM/blob/main/assets/compare_KO.png"/>
</p>

On Amazon stock:
<p align="center">
  <img src="https://github.com/GalMichaeli/046211-Stock-Price-Forecasting-with-xLSTM/blob/main/assets/compare_AMZN.png"/>
</p>

On Pfizer stock:
<p align="center">
  <img src="https://github.com/GalMichaeli/046211-Stock-Price-Forecasting-with-xLSTM/blob/main/assets/compare_PFE.png"/>
</p>

Our results indicate that the xLSTM model outperforms the RWKV model, particularly in responding to sudden fluctuations in stock prices.

## Summary & Conclusions:
Our xLSTM model demonstrates impressive forecasting accuracy over short time periods across various types of stocks, outperforming the transformer-based RWKV model. However, the efficiency and accuracy of predictions decline as the forecast horizon extends, with performance decreasing significantly for longer-term forecasts.

## Repository Organization
| Directory | Description |
|-----------|-------------|
| code | Contains .ipynb files used for our model and for producing RWKV data for comparison |
| data | Contains .csv files with data produced by the RWKV model |
| assets | Contains the images displayed in `README.md` |

## Sources & References:
RWKV model: https://github.com/tomer9080/Stock-Prediction-Using-RWKV/tree/main
xLSTMTime documentation: https://arxiv.org/abs/2405.04517
xLSTMTime Git: https://github.com/muslehal/xLSTMTime

## Future Work 
Based on our results, we believe our project demonstrates promising potential in stock prediction and contributes to advancing research on LSTM-based models. However, the forecasting accuracy over longer periods remains suboptimal, highlighting areas for improvement. Future work could involve:
* Experimenting with different model configurations, including varying the number of xLSTM blocks.
* Integrating sLSTM blocks with different head configurations.
* Exploring alternative data preprocessing techniques, such as various feature scaling methods, outlier handling strategies, and categorical encoding for supplementary stock data.

## Disclaimer
This project is not intended to provide financial, trading, and investment advice. No warranties are made regarding the accuracy of the models. Audiences should conduct their due diligence before making any investment decisions using the methods or code presented in this repository.

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).
