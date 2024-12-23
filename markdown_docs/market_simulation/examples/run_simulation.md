## FunctionDef run_simulation
# Project Documentation

## Overview

This document provides a comprehensive guide to understanding, setting up, and utilizing the [Project Name] software. It is designed for both developers and beginners who wish to integrate this tool into their projects or learn more about its functionalities.

## Table of Contents

1. **Introduction**
2. **System Requirements**
3. **Installation Guide**
4. **Configuration**
5. **Usage**
6. **API Documentation**
7. **Troubleshooting**
8. **FAQs**
9. **Contributing**
10. **License**

---

### 1. Introduction

[Project Name] is a [brief description of what the project does, its purpose, and key features]. It aims to provide an efficient solution for [problem it solves or task it performs].

### 2. System Requirements

To ensure optimal performance, please meet the following system requirements:

- **Operating Systems**: Windows 10/11, macOS Mojave (10.14) or later, Ubuntu 18.04 LTS or later.
- **Hardware**:
    - Minimum: 2 GB RAM, 500 MB free disk space.
    - Recommended: 4 GB RAM, 1 GB free disk space.
- **Software**: [List any software dependencies such as specific versions of Python, Java, etc.]

### 3. Installation Guide

#### Prerequisites

Ensure that you have the following prerequisites installed on your system:

- [Prerequisite 1]
- [Prerequisite 2]

#### Steps to Install

1. **Download the Software**: Visit the official website or repository and download the latest version of [Project Name].
2. **Extract Files**: Unzip the downloaded file to a directory of your choice.
3. **Run Installer**:
    - On Windows, double-click `setup.exe`.
    - On macOS, open the `.dmg` file and drag the application to your Applications folder.
    - On Linux, run the installation script provided in the extracted files.

### 4. Configuration

Configuration settings can be adjusted through a configuration file located at `[path/to/config/file]`. Below are some common configurations:

- **Setting 1**: [Description]
- **Setting 2**: [Description]

For more detailed information on each setting, refer to the `config.ini` file.

### 5. Usage

#### Basic Usage

To start using [Project Name], follow these steps:

1. Open the application.
2. Navigate to the main dashboard.
3. Select a feature or module from the sidebar.

#### Advanced Features

For advanced usage and features, refer to the following sections:

- **Feature A**: [Description]
- **Feature B**: [Description]

### 6. API Documentation

[Project Name] provides an API for developers who wish to integrate its functionalities into their applications. The API documentation is available at [API Documentation URL].

#### Key Endpoints

- **Endpoint 1**:
    - Description: [What this endpoint does]
    - Method: GET/POST
    - Parameters: [List of parameters]
- **Endpoint 2**:
    - Description: [What this endpoint does]
    - Method: GET/POST
    - Parameters: [List of parameters]

### 7. Troubleshooting

If you encounter any issues while using [Project Name], refer to the troubleshooting section below:

- **Issue A**: [Description]
    - Solution: [Solution]
- **Issue B**: [Description]
    - Solution: [Solution]

For further assistance, please contact our support team at [support email].

### 8. FAQs

#### General Questions

- **Q1**: [Question]
    - **A1**: [Answer]
- **Q2**: [Question]
    - **A2**: [Answer]

#### Technical Questions

- **T1**: [Technical Question]
    - **TA1**: [Technical Answer]
- **T2**: [Technical Question]
    - **TA2**: [Technical Answer]

### 9. Contributing

We welcome contributions from the community! To contribute to [Project Name], please follow these guidelines:

1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Commit your changes and push them to your fork.
4. Submit a pull request with a detailed description of your changes.

### 10. License

[Project Name] is released under the [License Type]. For more information, please refer to the `LICENSE` file in the project repository.

---

This documentation aims to provide clear and concise guidance for users and developers alike. If you have any questions or feedback, feel free to reach out to our support team.
## FunctionDef get_trade_infos(exchange, symbol, start_time, end_time)
**get_trade_infos**: This function retrieves trade information within a specified time frame from the TradeInfoState associated with a given symbol on an exchange.

parameters:
· exchange: An instance of the Exchange class representing the financial market where trading activities occur.
· symbol: A string representing the trading symbol for which trade information is to be retrieved.
· start_time: A Timestamp indicating the beginning of the time frame within which trades should be considered.
· end_time: A Timestamp indicating the end of the time frame within which trades should be considered.

Code Description: The function starts by accessing the TradeInfoState object associated with the specified symbol from the exchange's states. It asserts that the retrieved state is indeed an instance of TradeInfoState to ensure type safety. Then, it retrieves the list of trade information stored in the `trade_infos` attribute of the TradeInfoState. This list contains instances of TradeInfo, each representing a trade executed during continuous auction periods. The function filters this list to include only those trades whose execution time falls within the specified start and end times. Finally, it returns the filtered list of trade information.

Note: Usage points highlight that the `get_trade_infos` function is utilized in market simulations to fetch trade data for analysis or visualization purposes. For example, it can be used to analyze trading patterns, evaluate agent performance, or plot price curves based on executed trades within a specific time frame.

Output Example: The output would be a list of TradeInfo objects representing the trades that occurred within the specified time frame. Each TradeInfo object contains detailed information about the trade, such as the order involved, transactions executed, and a snapshot of the limit order book at the time of the trade.

Example:
```python
trade_infos = get_trade_infos(exchange, 'AAPL', Timestamp("2024-01-01 09:30:00"), Timestamp("2024-01-01 10:30:00"))
# trade_infos might look like this:
[
    TradeInfo(
        order=LimitOrder(...),
        trans=[Transaction(...), Transaction(...)],
        lob_snapshot=OrderbookSnapshot(...)
    ),
    # ... more TradeInfo objects for other trades within the specified time frame
]
```
## FunctionDef plot_price_curves(trade_infos, path)
**plot_price_curves**: This function generates a plot of price curves based on trade information and saves it to a specified file path.

parameters:
· trade_infos: A list of TradeInfo objects, each representing a snapshot of trade information including an order, transactions, and the state of the limit order book (LOB) at a specific point in time.
· path: A Path object indicating where the generated plot should be saved.

Code Description: The function begins by ensuring that the directory for saving the plot exists. It does this by creating any necessary parent directories using `path.parent.mkdir(parents=True, exist_ok=True)`.

Next, it processes the list of TradeInfo objects to extract relevant price information. Specifically, it creates a list of dictionaries where each dictionary contains the time of the order and the last price from the limit order book snapshot at that time. This is done with a list comprehension that filters out any entries where the last price is zero or negative.

The extracted data is then converted into a pandas DataFrame. To smooth the data and make it more interpretable, the function groups the prices by one-minute intervals using `pd.Grouper(key="Time", freq="1T")` and calculates the mean price for each interval with `.mean()`. The resulting DataFrame is reset to have a default integer index.

The function then sets up a plot using seaborn's darkgrid style. It creates a figure and axis object with specified dimensions, plots the time series data of prices over time using `sns.lineplot`, and formats the x-axis to display time in hours and minutes format (`%H:%M`). The title of the plot is set to "Price Trajectory Generated by NoiseAgent".

After customizing the plot's appearance, the function saves it to the specified path using `fig.savefig(str(path))` and closes the figure with `plt.close(fig)` to free up memory. Finally, it prints a confirmation message indicating where the price curves have been saved.

Note: This function is particularly useful for visualizing how prices evolve over time in a simulated trading environment. It can help developers and analysts understand market dynamics and the impact of different trading strategies or agents on price movements. In the provided example, `plot_price_curves` is called within the `run_simulation` function to generate and save a plot of price curves based on trades executed by a NoiseAgent during a simulated trading session.
