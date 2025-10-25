# 🧠 PredictChain — Documentation & Architecture Overview

## 📘 Overview

**PredictChain** is a decentralized prediction platform that integrates **AI models** with **blockchain verification**.
It enables users to make, verify, and reward accurate predictions in a transparent and tamper-proof way.

The system connects:

* 🧮 **AI Model Layer** — handles data analysis and prediction logic.
* 🔗 **Blockchain Layer** — stores and verifies prediction results.
* 🌐 **Frontend DApp** — provides user interaction and visualization.
* ⚙️ **Backend/API Layer** — bridges model predictions with blockchain smart contracts.

---

## 🏗️ Architecture Diagram (Conceptual)

```
          ┌────────────────────────┐
          │      Frontend (DApp)   │
          │  React / Next.js / Web3│
          └────────────┬───────────┘
                       │
                       ▼
          ┌────────────────────────┐
          │  Backend / API Server  │
          │  Python (FastAPI)      │
          │  ML Models (PyTorch)   │
          └────────────┬───────────┘
                       │
                       ▼
          ┌────────────────────────┐
          │  Smart Contracts (Sol.)│
          │  PredictionStorage.sol │
          │  RewardSystem.sol      │
          └────────────┬───────────┘
                       │
                       ▼
          ┌────────────────────────┐
          │   Blockchain Network    │
          │   (Ethereum / Sepolia)  │
          └────────────────────────┘
```

---

## ⚙️ Smart Contracts

### `PredictionStorage.sol`

Handles creation, storage, and verification of predictions.

**Main Functions**

| Function                                       | Description                         |
| ---------------------------------------------- | ----------------------------------- |
| `submitPrediction(string _id, bytes32 _hash)`  | Submits a hashed prediction         |
| `verifyPrediction(string _id, string _result)` | Confirms if prediction was accurate |
| `getPrediction(string _id)`                    | Fetches stored prediction data      |

---

### `RewardSystem.sol`

Distributes tokens or rewards to users who made correct predictions.

**Main Functions**

| Function                         | Description                           |
| -------------------------------- | ------------------------------------- |
| `stake(uint amount)`             | Stake tokens to participate           |
| `claimReward(uint predictionId)` | Claim reward for a correct prediction |
| `distributeRewards()`            | Handles periodic reward distribution  |

---

## 🧮 Model Training & Prediction Pipeline

1. **Data Collection:** Market or event data pulled from APIs.
2. **Preprocessing:** Cleans data, removes outliers, normalizes features.
3. **Model Training:**

   * LSTM or Transformer model trained to predict future outcomes.
   * Tracked with experiment logging (Weights & Biases / TensorBoard).
4. **Prediction Output:**

   * Model outputs a prediction (e.g., price direction).
   * The result is hashed (`keccak256`) and stored on-chain.
5. **Verification:**

   * After outcome is known, true value is revealed and validated against stored hash.

---

## 🧩 Data Flow

```
User → DApp → Backend (API) → AI Model
     → Prediction Hash → Smart Contract
     → Blockchain Record → Frontend Display
```

---

## 🔐 Environment Variables

Create a `.env` file in both **backend** and **frontend** directories.

**Backend (.env):**

```
OPENAI_API_KEY=your_api_key
MODEL_PATH=models/latest_model.pt
ETH_PRIVATE_KEY=your_wallet_private_key
CONTRACT_ADDRESS=0xYourContractAddress
NETWORK_URL=https://sepolia.infura.io/v3/your_key
```

**Frontend (.env):**

```
NEXT_PUBLIC_CONTRACT_ADDRESS=0xYourContractAddress
NEXT_PUBLIC_NETWORK=sepolia
```

---

## 🚀 Local Setup Instructions

### 1️⃣ Clone the repo

```bash
git clone https://github.com/<your-username>/PredictChain.git
cd PredictChain
```

### 2️⃣ Install dependencies

```bash
# Smart contracts
cd contracts && npm install

# Backend
cd ../backend && pip install -r requirements.txt

# Frontend
cd ../frontend && npm install
```

### 3️⃣ Run the project

```bash
# Backend API
uvicorn app.main:app --reload

# Frontend DApp
npm run dev
```

### 4️⃣ Deploy contracts

```bash
npx hardhat run scripts/deploy.js --network sepolia
```

---

## 🧪 Testing

Run smart contract tests:

```bash
npx hardhat test
```

Run backend tests:

```bash
pytest
```

---

## 📈 Future Enhancements

* Add on-chain staking and reward system
* Integrate Chainlink for trusted oracle data
* Improve model explainability with SHAP/LIME
* Enable DAO governance for prediction topics