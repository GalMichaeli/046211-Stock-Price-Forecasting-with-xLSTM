<h1 align="center">
    Stock Price Prediction Using xLSTM
    <br>
</h1>


<h3 align="center">
    Harnessing the Power of xLSTM for Time Series Prediction
    <br>
</h3>

<h3 align="center">
    <a href="https://github.com/GalMichaeli">Gal Michaeli</a> •
    <a href="https://github.com/SergeyMashkin">Sergey Mashkin</a>
</h3>

<h4 align="center">Technion ECE 046211 Spring 2024 Final Project</h4>

## Introduction
This project aims to utilize the [xLSTM](https://arxiv.org/abs/2405.04517) block architecture for short, medium and long timeframe prediction of stock prices. xLSTM blocks, and in particular mLSTM blocks, boast improvements over traditional LSTM blocks and Transformer capabilities, mainly due to their matrix memory and covariance update rule. To take full advantage of these improved capabilities, we constructed a model specifically designed for prediction of time series with non-stationary distribution. We based our design on [xLSTMTime](https://arxiv.org/pdf/2407.10240), with some modifications and adaptations.

## Contents
- [Previous Work](#previous-work)
- [Dependencies](#dependencies)
- [How to Run](#how-to-run)
- [Datasets](#datasets)
- [Repository Organization](#repository-orgnization)
- [Refernces](#refernces)
- [Acknowledgements](#acknowledgements)
- [License](#license)

## Previous Work
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
| `cuda` | `12.1` |
| `matplotlib` | `3.7.1` |
| `numpy` | `1.26.4` |
| `pandas` | `2.2.2` |
| `scikit-learn` | `1.5.2` |
| `yfinance` | `0.2.44` |


## How to Run

## Datasets

## Repository Orgnization

## Refernces

## Acknowledgements

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).
