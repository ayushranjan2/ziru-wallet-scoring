Wallet Risk Scoring for Compound Protocol
This repository contains a Python-based solution for assessing the risk profiles of 103 wallet addresses interacting with the Compound V3 protocol. It processes a list of wallet addresses, simulates transaction data (due to lack of direct API access), engineers risk-related features, and assigns a risk score from 0 to 1000.
Directory Structure
wallet-risk-scoring/
│
├── data/
│   ├── input/
│   │   └── wallet_ids.csv         # Input wallet addresses (103 wallets)
│   ├── output/
│   │   └── wallet_risk_scores.csv # Output risk scores
│
├── src/
│   └── compound_risk_scoring.py   # Main script for risk scoring
│
├── .gitignore                    # Git ignore file
├── README.md                     # Project documentation
├── requirements.txt              # Python dependencies
└── LICENSE                       # MIT License

Setup

Clone the Repository:
git clone https://github.com/your-username/wallet-risk-scoring.git
cd wallet-risk-scoring


Install Dependencies:Create a virtual environment and install required packages:
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt


Prepare Input Data:

The provided data/input/wallet_ids.csv contains 103 wallet addresses.
Format: A single column wallet_id with Ethereum addresses.


Run the Script:
python src/compound_risk_scoring.py


Outputs data/output/wallet_risk_scores.csv with columns wallet_id and score.



Methodology
Data Collection

Simulation: Transaction data (supply, borrow, repay, withdraw, liquidation) is simulated due to lack of API access.
Real Implementation: Replace fetch_transaction_data with API calls to:
Etherscan API: Query transactions for the Compound V3 contract (0xc3d688B66703497DAA19211EEdff47f25384cdc3).
The Graph Subgraph: Fetch structured data for Compound V3 interactions.



Feature Engineering

Transaction Frequency: Transactions per month (6-month period).
Borrow-to-Supply Ratio: Total borrowed divided by total supplied, indicating leverage.
Liquidation Events: Count of liquidations, a direct risk indicator.
Collateral Factor Adherence: Supply relative to 1.5x borrow requirement.
Average Transaction Size: Mean transaction amount, indicating exposure.
Time Since Last Activity: Days since last transaction, reflecting position monitoring.

Risk Scoring

Model: Weighted sum of normalized features, scaled to [0, 1000].
Weights:
Liquidations: 40%
Borrow-to-Supply Ratio: 25%
Transaction Frequency: 15%
Collateral Factor: 10%
Average Transaction Size: 5%
Days Inactive: 5%


Normalization: Min-Max scaling to [0, 1]. Log transformation for skewed features.

Risk Indicators Justification

Liquidations: Indicate failure to maintain collateral, a critical risk signal.
Borrow-to-Supply Ratio: High leverage increases liquidation risk.
Transaction Frequency: Frequent activity may reflect speculative behavior.
Collateral Factor: Ensures sufficient collateral, a core DeFi risk metric.
Transaction Size: Larger transactions increase exposure to volatility.
Inactivity: Unmonitored positions are risky in volatile markets.

Output
The script generates data/output/wallet_risk_scores.csv with 103 entries, e.g.:
wallet_id,score
0x0039f22efb07a647557c7c5d17854cfd6d489ef3,412
0x06b51c6882b27cb05e712185531c1f74996dd988,387
...
0xfaa0768bde629806739c3a4620656c5d26f44ef2,732
...

Notes

Scalability: The script processes 103 wallets iteratively, scalable to larger lists with batch API calls.
Real-World Use: Integrate Etherscan or The Graph APIs. Add error handling for API rate limits.
Limitations: Simulated data may not reflect real blockchain behavior.

License
MIT License (see LICENSE file).
