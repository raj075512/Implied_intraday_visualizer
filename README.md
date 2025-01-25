### README: **Intraday Expiry Visualizer for Implied Volatility (IV) Using Reverse Engineering in Black-Scholes-Merton (BSM)**

This repository provides a tool for **calculating and visualizing intraday implied volatility (IV)** using reverse engineering techniques based on the **Black-Scholes-Merton (BSM)** options pricing model. The tool is designed for analyzing **expiry-day IV dynamics**, enabling traders and researchers to gain deeper insights into option price movements and implied volatility behavior.

---
![image](https://github.com/user-attachments/assets/9eae4a4f-126c-4084-ad45-e3c53ee5838b)

## **Overview**

The implied volatility (IV) is a crucial metric in options trading that represents the market's expectations of future volatility. This tool calculates IV by **reverse-engineering the Black-Scholes-Merton (BSM)** formula, taking the **market price of the option** and solving for the volatility parameter.

Key features include:
- **Real-time and historical IV calculation.**
- Visualization of IV trends across strikes, expiries, and time intervals.
- Intraday IV behavior for options nearing expiration.

---

## **How IV Calculation Works**

The **Black-Scholes-Merton model** calculates the theoretical price of an option based on:
1. Current price of the underlying asset (\(S\)),
2. Strike price (\(K\)),
3. Time to expiration (\(T\)),
4. Risk-free interest rate (\(r\)),
5. Volatility (\(\sigma\), the implied volatility).

Given the observed **market price of the option**, we reverse-engineer the BSM formula to solve for \(\sigma\) (implied volatility). The equation for the BSM model is:

### **Call Option Price:**
\[
C = S \cdot N(d_1) - K \cdot e^{-rT} \cdot N(d_2)
\]

### **Put Option Price:**
\[
P = K \cdot e^{-rT} \cdot N(-d_2) - S \cdot N(-d_1)
\]

Where:
\[
d_1 = \frac{\ln(S / K) + (r + \sigma^2 / 2)T}{\sigma \sqrt{T}}, \quad
d_2 = d_1 - \sigma \sqrt{T}
\]

**IV Reverse Engineering:**
- The market price (\(C\) or \(P\)) is known.
- Solve numerically for \(\sigma\) using methods like **Newton-Raphson** or other root-finding algorithms.

---

## **Features**

1. **Reverse-Engineered Implied Volatility Calculation:**
   - Supports both **call** and **put options**.
   - Uses observed market prices to compute IV using the BSM model.

2. **Intraday IV Visualization:**
   - Real-time IV plotting for options approaching expiration.
   - Historical analysis of IV trends across various strikes.

3. **Expiry-Day Behavior Analysis:**
   - Tracks how IV changes intraday on options' expiration day.
   - Useful for detecting **IV crush** or abnormal volatility patterns.

4. **Dynamic IV Skew:**
   - Visualize the **IV skew** (IV across different strikes at the same time).
   - Compare IV for **in-the-money (ITM)**, **at-the-money (ATM)**, and **out-of-the-money (OTM)** options.

5. **Custom Metrics:**
   - Overlay IV charts with **volume**, **open interest**, or the **underlying price** for enhanced context.

6. **Backtesting Framework:**
   - Run historical analyses of IV for specific dates and compare behavior in different market regimes.

---

## **Use Cases**

- **Traders:**
  - Detect intraday IV spikes or collapses to capture trading opportunities.
  - Monitor IV crush during expiry-day trading.

- **Quant Researchers:**
  - Study the relationship between IV, option prices, and market conditions.
  - Analyze IV skew and its predictive power for market movements.

- **Portfolio Managers:**
  - Assess portfolio risk based on implied volatility trends.

---

## **Installation**

### 1. Clone the Repository
```bash
git clone https://github.com/your-repo/intraday-expiry-iv-visualizer.git
cd intraday-expiry-iv-visualizer
```

### 2. Install Dependencies
Install the required Python libraries:
```bash
pip install -r requirements.txt
```

### 3. Data Source Setup
- Connect to a **market data API** (e.g., Alpha Vantage, Interactive Brokers, Quandl) to fetch options price and underlying data.
- Alternatively, load historical data in `.csv` format into the `data/` folder.

---

## **How to Use**

### **1. IV Calculation Workflow**
1. Configure your settings in `config.yaml`:
   - Ticker symbol of the underlying asset.
   - Expiry date and strikes to analyze.
   - Market data source (API key or `.csv` file).

2. Run the main script to calculate IV:
   ```bash
   python main.py
   ```

3. Output:
   - IV trends plotted intraday for selected strikes and expiries.
   - IV skew snapshots across strikes.

### **2. Key Command-Line Options**
- **Ticker and Expiry:**
  ```bash
  python main.py --ticker SPY --expiry 2025-01-26
  ```
- **IV Skew Visualization:**
  ```bash
  python main.py --skew_snapshot True
  ```
- **Historical Backtesting:**
  ```bash
  python main.py --historical True --date 2024-12-15
  ```

---

## **Reverse Engineering Example**

Below is an example of how the tool reverse-engineers implied volatility from market data:

### **Inputs:**
- Ticker: `SPY`
- Market Price: `2.50` (call option price)
- Strike Price: `410`
- Time to Expiry: `0.25` years (3 months)
- Risk-Free Rate: `3%`

### **Calculation:**
The tool solves for \(\sigma\) (IV) iteratively by minimizing the error between the market price and the BSM-calculated price:
\[
\text{Error} = |C_{\text{market}} - C_{\text{BSM}}|
\]

Using the **Newton-Raphson method** or **bisection method**, the tool calculates:
\[
\text{Implied Volatility: } \sigma = 18.23\%
\]

---

## **File Structure**

```
intraday-expiry-iv-visualizer/
│
├── data/                # Data storage (historical or real-time)
├── src/                 # Source code
│   ├── iv_calculator.py # IV reverse engineering logic
│   ├── data_loader.py   # Market data ingestion
│   ├── plotter.py       # Visualization functions
│   ├── config.yaml      # Configurable settings
├── visualizations/      # Output charts
├── requirements.txt     # Python dependencies
├── README.md            # Documentation
└── main.py              # Main script to calculate and visualize IV
```

---

## **Configuration**

### Configurable Parameters in `config.yaml`:
- **Ticker Symbol:** Select the underlying asset (e.g., SPY, AAPL).
- **Expiry Date:** Specify the options expiration date.
- **Strikes:** Choose strikes of interest or let the tool auto-select based on moneyness (ITM/ATM/OTM).
- **Data Source:** Define API keys or file paths for fetching options data.

---

## **Future Enhancements**

- **Volatility Surface:**
  - Extend the tool to visualize IV surfaces (3D plots of strikes, expiries, and IV).

- **Greeks Integration:**
  - Add calculation and visualization of **delta**, **gamma**, **theta**, and **vega**.

- **Multi-Asset Support:**
  - Enable analysis for multiple tickers simultaneously.

- **Machine Learning Models:**
  - Predict future IV behavior using historical patterns.

---

## **Requirements**

- **Python Version:** 3.8+
- **Core Libraries:**
  - `numpy` - Mathematical computations.
  - `scipy` - Numerical solvers for reverse-engineering IV.
  - `pandas` - Data handling.
  - `matplotlib` - Plotting IV trends.
  - `yfinance` (optional) - For fetching historical data.

---

## **License**

This project is licensed under the **MIT License**. See `LICENSE` for details.

---

## **Contributing**

We welcome contributions to enhance the functionality of this tool! To contribute:
1. Fork the repository.
2. Create a new branch.
3. Submit a pull request with your changes.

---

## **Contact**

For any questions, feedback, or issues, please contact:
- **Email:** raj075512@gmail.com
- **Current company**: STokhos research Capital LLP
- **GitHub Issues:** Open an issue in this repository.

---

Happy Analyzing and Trading! 🚀
