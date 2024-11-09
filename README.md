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
- [Usage](#usage)
- [Configuration and Hyperparameters](#configuration-and-hyperparameters)
- [Data Acquisition and Preprocessing](#data-acquisition-and-preprocessing)
- [Model Architecture](#model-architecture)
- [Results](#results)
- [Conclusion](#conclusion)
- [Repository Organization](#repository-organization)
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


## Usage
Download the notebook located in the ```code``` directory and run in Google Colaboratory.
Alternatively, you can directly
<a href="https://colab.research.google.com/github/GalMichaeli/046211-Stock-Price-Forecasting-with-xLSTM/blob/main/code/xlstm-stock-price-forecasting.ipynb">
  <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

To train and forecast a stock of your choice, say comapny *ExWhyZee* with ticker *XYZ*,
run all cells in the notebook down to (exclusive) the section named "Forecasting Coca-Cola",
then do the following.

Instantiate the model:
``` 
XYZ_trainer = trainer('ExWhyZee', 'XYZ')
``` 

Train the model:
```
XYZ_trainer.train()
```

View a plot of the training and validation losses over training epochs:
```
XYZ_trainer.plot_losses()
```

View the model's forecast on the test set:
```
XYZ_trainer.plot_set('test')
```

Or view the model's forecast on the entrie data series:
```
XYZ_trainer.plot_set('all')
```
**Note**: Currently 'test' and 'all' are the only supported plotting formats.

You can modify the training hyperparamters to your liking in ```config_dict["training"]```.

**Note**: Modifying other values in ``` config_dict ``` may produce unexpected results or errors.


## Configuration and Hyperparameters
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
For better comparison to similar previous projects, the model was trained and tested on the Coca-Cola, Pfizer and Amazon daily data, obtained through the *Yahoo Finance* API
Python library ```yfinance```. Data features used were: Open, High, Low, Close and Adjusted Close prices, and the daily Volume.
Data preprocessing included applying a simple moving average with window size 10, and transforming the **prices** to their natural logarithm.
Due to the length of the time series, we opted to assign only 64% to training, 16% to validation and the remaining 20% to testing.


## Model Architecture
As recommended by [xLSTMTime](https://arxiv.org/pdf/2407.10240), the architecture we used is as depicted in the
following figure.
<p align="center">
  <img src="https://github.com/GalMichaeli/046211-Stock-Price-Forecasting-with-xLSTM/blob/main/assets/model-arch.png" width="1000"/>
</p>


## Results
The model's prediction capabilities on the whole time series can be seen in the following figure, which shows the performance on *Coca-Cola* stock data:
<p align="center">
  <img src="https://github.com/GalMichaeli/046211-Stock-Price-Forecasting-with-xLSTM/blob/main/assets/ko-all.png"/>
</p>

A closer look into the performance over the **test set** reveals the deviations and inaccuracies of the prediction:
<p align="center">
  <img src="https://github.com/GalMichaeli/046211-Stock-Price-Forecasting-with-xLSTM/blob/main/assets/ko-test.png"/>
</p>

Additionaly, we experimented with autoregressive forecasting 100 days into the future on *Coca-Cola* data,
resulting in non-satisfactory performance:

https://github.com/user-attachments/assets/9b365589-fe91-4cc0-b0a4-efb39a64f50e

To assess the generalization capability of our model, we trained it on three independent stocks—Apple, Microsoft, and Walmart Inc.—and evaluated its performance on other stocks not included in the training set. We then compared the output of this model to that of a model trained specifically on the individual stock being tested.
<p align="center">
  <img src="https://github.com/GalMichaeli/046211-Stock-Price-Forecasting-with-xLSTM/blob/main/assets/ko-multi.png"/>
</p>
Displayed here are the results of the model (trained on Apple, Microsoft, and Walmart) predicting Coca-Cola's stock prices. As shown, the model's predictions are highly accurate and closely align with actual prices, demonstrating that its performance does not significantly differ from a model trained exclusively on Coca-Cola's stock. This indicates a strong ability to generalize across different stocks.


### Comparison with RWKV
We examined our model's performance against an [RWKV](https://github.com/tomer9080/Stock-Prediction-Using-RWKV/tree/main) model
(considered to be superior to traditional LSTM models). The Comparison yielded the following result for *Coca-Cola* stock:

<p align="center">
  <img src="https://github.com/GalMichaeli/046211-Stock-Price-Forecasting-with-xLSTM/blob/main/assets/ko-rwkv.png"/>
</p>

More results can be viewed in the assets directory of this repository.


## Conclusion
Our xLSTM model demonstrates impressive forecasting accuracy over short time periods across various types of stock, outperforming the RWKV model.
However, the efficiency and accuracy of predictions decline with autoregressive forecasting, with performance decreasing significantly for longer-term forecasts.


## Repository Organization
| Directory | Description |
|-----------|-------------|
| code | Contains implementation .ipynb file used for our model |
| data | Contains .csv files with data produced by the RWKV model |
| assets | Contains the images and video displayed in `README.md` and additional results images |


## Future Work 
Based on our results, we believe our project demonstrates promising potential in stock prediction and contributes to advancing research on LSTM-based models. However, the forecasting accuracy over longer periods remains suboptimal, highlighting areas for improvement. Future work could involve:
* Experimenting with different model configurations, including varying the number of xLSTM blocks.
* Integrating sLSTM blocks with different head configurations.
* Exploring alternative data preprocessing techniques, such as various feature scaling methods, outlier handling strategies, and categorical encoding for supplementary stock data.

## Disclaimer
This project is not intended to provide financial, trading, and investment advice. No warranties are made regarding the accuracy of the models. Audiences should conduct their due diligence before making any investment decisions using the methods or code presented in this repository.

## License
This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).
