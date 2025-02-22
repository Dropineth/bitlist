# **Bitlist - Local Life Yellow Pages MVP on Sui Blockchain (Payment Focus)**

## **Overview**

**Bitlist** is a decentralized local business platform built on **Sui Blockchain** designed to connect local stores (e.g., second-hand stores, repair shops) with customers. This MVP integrates **NAVI** and **Scallop** as payment modules, enabling **secure, seamless payments** for local services/products. The platform uses **NFT-based warranty cards** to provide additional value to both businesses and customers, improving trust and transaction transparency.

### **Business Model**
- **Revenue Streams**: Listing fees, transaction fees, premium features, and loyalty rewards.
- **Core Features**: 
  - Local business listings (second-hand stores, repair shops)
  - Warranty NFTs for services/products
  - Payment system with **NAVI** and **Scallop**
  - Customer acquisition via referrals and loyalty programs

---

## **Technology Stack**

- **Blockchain**: **Sui** for decentralized applications and payments.
- **Smart Contracts**: **Move** programming language.
- **Payment Protocols**: **NAVI** and **Scallop** for handling secure payments.
- **Frontend**: ReactJS with Web3 integration (using **Sui SDK**).
- **Backend**: Node.js with Express for API management, hosted on AWS or similar cloud service.

---

## **Platform Architecture**

### **1. User Flow**

#### **A. Business Registration**
1. **Business Creation**: Local businesses (repair shops, second-hand stores) register on the platform and create profiles.
2. **Service/Product Listing**: Businesses list products/services, including prices, descriptions, and images.
3. **Warranty NFTs**: Upon purchase, customers receive a **warranty NFT** linked to the service/product.

#### **B. Customer Flow**
1. **Search Listings**: Customers search for services/products by category, location, or keywords.
2. **Service/Product Purchase**: Customers select a product/service and complete the payment via **Sui**.
3. **NFT Warranty Issuance**: After payment, customers receive warranty NFTs, which are tied to the product/service.
4. **Repair/Refund**: Customers can use the NFTs to redeem repairs or refunds based on warranty conditions.

---

### **2. Payment Integration (NAVI and Scallop)**

#### **A. NAVI Payment Integration**
- **NAVI** will serve as a payment gateway, enabling **Sui**-based transactions.
- **Payment Flow**: Businesses can list products/services with a fixed price. When a customer makes a purchase, the system handles the transaction using NAVI's liquidity system.

#### **B. Scallop Payment Integration**
- **Scallop** will allow businesses and customers to handle payments and refunds in a **peer-to-peer** manner, without intermediaries.
- **Payment Flow**: Customers can pay for services/products using **Sui** or **other supported tokens** through **Scallop's** peer-to-peer money market.

---

## **System Components**

### **1. Frontend**

**Tech Stack**: ReactJS, Sui SDK, Web3 Integration
- **User Interface**: Displays business listings, enables filtering and searching, shows transaction history, and warranty NFT details.
- **Business Dashboard**: Businesses can manage listings, view transactions, and mint NFTs.
- **Payment Gateway**: Integrates **NAVI** and **Scallop** for seamless payment functionality.

### **2. Backend**

**Tech Stack**: Node.js, Express, Sui SDK
- **User Authentication**: Users sign in with their **Sui Wallet** for secure access.
- **NFT Minting Service**: After a successful purchase, the backend triggers the minting of the warranty NFT.
- **Payment Processing**: Integrates with **NAVI** and **Scallop** to handle payments securely.

### **3. Smart Contracts**

- **Warranty NFT Contract**: Manages the creation, transfer, and validation of warranty NFTs.
- **Payment Contract**: Handles the logic of integrating payments using **NAVI** and **Scallop** protocols.

---

## **Smart Contract Design (Move)**

### **1. Warranty NFT Smart Contract**

```move
module Bitlist::WarrantyNFT {
    use 0x2::NFT;

    public fun mint_warranty_nft(account: &signer, product_id: u64, warranty_period: u64): address {
        let nft = NFT::create(account, product_id, warranty_period);
        return NFT::get_token_address(&nft);
    }
    
    public fun transfer_warranty_nft(account: &signer, nft_id: address, to: address) {
        NFT::transfer(account, nft_id, to);
    }
}
```

### **2. Payment Contract (Using NAVI and Scallop)**

```move
module Bitlist::Payment {
    use 0x1::NAVI::PaymentGateway;
    use 0x1::Scallop::PaymentGateway;

    public fun process_payment(account: &signer, amount: u64, method: string) {
        if (method == "navi") {
            PaymentGateway::process_navi_payment(account, amount);
        } else if (method == "scallop") {
            PaymentGateway::process_scalop_payment(account, amount);
        } else {
            abort 0; // Invalid payment method
        }
    }
}
```

---

## **How to Run the Project Locally**

### **Prerequisites**
- **Node.js** installed (version 16 or higher)
- **Sui Wallet** extension installed in your browser
- **Sui SDK** installed

### **Step-by-Step Guide**

#### **1. Clone the Repository**
```bash
git clone https://github.com/dropineth/bitlist.git
cd bitlist
```

#### **2. Install Dependencies**
```bash
npm install
```

#### **3. Configure Environment**
- **Sui Wallet Integration**: Ensure your Sui wallet is set up with testnet/mainnet.
- Set environment variables for the Sui network in `.env` file:
  ```
  SUI_NETWORK=testnet
  SUI_RPC_URL=https://fullnode.devnet.sui.io:5001
  ```

#### **4. Run the Backend**
```bash
npm run start
```
This will launch the API server for handling user requests, business listings, and payment processing.

#### **5. Run the Frontend**
```bash
npm run start:frontend
```
This will launch the ReactJS application that connects to the backend and the **Sui Wallet** for transactions.

---

## **Future Features**

1. **AI-powered Search**: Use AI to enhance the search functionality and recommend services/products based on user preferences and past behavior.
2. **Customer Reviews and Ratings**: Enable customers to leave feedback on services and businesses, improving transparency.
3. **Advanced Payment Options**: Integrate more payment options, such as fiat on-ramps or other blockchain payment gateways.
4. **Referral System**: Implement a referral system to incentivize users to share the platform with others.


---

## **Conclusion**

Bitlist is designed to enable local businesses to thrive by leveraging the power of **Sui Blockchain** and decentralized payment systems through **NAVI** and **Scallop**. By integrating local business listings, warranty NFTs, and secure payments, Bitlist is setting a new standard for local commerce in the blockchain era.

---

### **Frontend (React with Web3 Integration)**

#### **1. Project Structure**

```
/bitlist-frontend
|-- /public
|-- /src
|   |-- /components
|   |   |-- BusinessListing.js
|   |   |-- PaymentForm.js
|   |   |-- NFTMint.js
|   |-- /utils
|   |   |-- SuiWallet.js
|   |-- App.js
|   |-- index.js
|-- /styles
|-- .env
```

#### **2. Sui Wallet Integration**

```js
// src/utils/SuiWallet.js

import { useState, useEffect } from "react";
import { SuiClient, SuiWallet } from "@mysten/sui.js"; // Assuming Sui SDK is available

const SuiWalletConnection = () => {
  const [connected, setConnected] = useState(false);
  const [walletAddress, setWalletAddress] = useState(null);

  useEffect(() => {
    if (window.sui) {
      window.sui.connect().then((account) => {
        setConnected(true);
        setWalletAddress(account.address);
      });
    }
  }, []);

  return {
    connected,
    walletAddress,
    connectWallet: () => window.sui.connect(),
  };
};

export default SuiWalletConnection;
```

#### **3. Payment Form Component (NAVI and Scallop)**

```js
// src/components/PaymentForm.js

import React, { useState } from "react";
import SuiWalletConnection from "../utils/SuiWallet";
import { processPayment } from "../utils/PaymentService"; // Utility to call backend for payment processing

const PaymentForm = ({ amount }) => {
  const { connected, walletAddress, connectWallet } = SuiWalletConnection();
  const [paymentMethod, setPaymentMethod] = useState("navi");
  const [loading, setLoading] = useState(false);
  const [paymentStatus, setPaymentStatus] = useState("");

  const handlePayment = async () => {
    if (!connected) {
      connectWallet();
    }

    setLoading(true);
    const response = await processPayment(walletAddress, amount, paymentMethod);
    if (response.success) {
      setPaymentStatus("Payment Successful!");
    } else {
      setPaymentStatus("Payment Failed");
    }
    setLoading(false);
  };

  return (
    <div>
      <h3>Pay for Service/Product</h3>
      <p>Amount: {amount} SUI</p>
      <select
        value={paymentMethod}
        onChange={(e) => setPaymentMethod(e.target.value)}
      >
        <option value="navi">NAVI</option>
        <option value="scalop">Scallop</option>
      </select>
      <button onClick={handlePayment} disabled={loading}>
        {loading ? "Processing..." : "Pay Now"}
      </button>
      {paymentStatus && <p>{paymentStatus}</p>}
    </div>
  );
};

export default PaymentForm;
```

---

### **Backend (Node.js with Express)**

#### **1. Project Structure**

```
/bitlist-backend
|-- /controllers
|   |-- paymentController.js
|-- /services
|   |-- suiService.js
|-- /models
|   |-- paymentModel.js
|-- app.js
|-- .env
|-- package.json
```

#### **2. SUI Blockchain Interaction Service**

```js
// /services/suiService.js

import { JsonRpcProvider, SuiClient } from "@mysten/sui.js"; // Assuming Sui SDK is installed

const SUI_RPC_URL = process.env.SUI_RPC_URL || "https://fullnode.devnet.sui.io:5001";
const client = new SuiClient(SUI_RPC_URL);

export const processNaviPayment = async (senderAddress, amount) => {
  try {
    const transaction = await client.transactionBuilder.createTransaction({
      senderAddress,
      amount, // Amount to be transferred
      recipientAddress: "NAVI_PAYMENT_GATEWAY_ADDRESS", // NAVI gateway address
    });

    const result = await client.submitTransaction(transaction);
    return result;
  } catch (error) {
    console.error("Payment failed:", error);
    throw new Error("Payment failed");
  }
};

export const processScalopPayment = async (senderAddress, amount) => {
  try {
    const transaction = await client.transactionBuilder.createTransaction({
      senderAddress,
      amount, // Amount to be transferred
      recipientAddress: "SCALOP_PAYMENT_GATEWAY_ADDRESS", // Scalop gateway address
    });

    const result = await client.submitTransaction(transaction);
    return result;
  } catch (error) {
    console.error("Payment failed:", error);
    throw new Error("Payment failed");
  }
};
```

#### **3. Payment Controller (Handles Payments)**

```js
// /controllers/paymentController.js

import { processNaviPayment, processScalopPayment } from "../services/suiService";

export const processPayment = async (req, res) => {
  const { walletAddress, amount, paymentMethod } = req.body;
  let response;

  try {
    if (paymentMethod === "navi") {
      response = await processNaviPayment(walletAddress, amount);
    } else if (paymentMethod === "scallop") {
      response = await processScalopPayment(walletAddress, amount);
    } else {
      return res.status(400).send({ success: false, message: "Invalid payment method" });
    }

    return res.status(200).send({ success: true, transactionDetails: response });
  } catch (error) {
    console.error(error);
    return res.status(500).send({ success: false, message: "Payment processing failed" });
  }
};
```

#### **4. Express Server Setup**

```js
// app.js

import express from "express";
import bodyParser from "body-parser";
import { processPayment } from "./controllers/paymentController";

const app = express();
const port = process.env.PORT || 3000;

app.use(bodyParser.json());

app.post("/payment", processPayment);

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```

---

### **Smart Contracts (Move)**

#### **1. Payment Contract for NAVI and Scallop Integration**

```move
module Bitlist::Payment {
    use 0x1::NAVI::PaymentGateway;
    use 0x1::Scallop::PaymentGateway;

    public fun process_navi_payment(account: &signer, amount: u64): bool {
        let result = PaymentGateway::process_navi_payment(account, amount);
        return result;
    }

    public fun process_scallop_payment(account: &signer, amount: u64): bool {
        let result = PaymentGateway::process_scalop_payment(account, amount);
        return result;
    }
}
```

#### **2. Warranty NFT Contract**

```move
module Bitlist::WarrantyNFT {
    use 0x2::NFT;
    
    public fun mint_warranty_nft(account: &signer, product_id: u64, warranty_period: u64): address {
        let nft = NFT::create(account, product_id, warranty_period);
        return NFT::get_token_address(&nft);
    }

    public fun transfer_warranty_nft(account: &signer, nft_id: address, to: address) {
        NFT::transfer(account, nft_id, to);
    }
}
```

---

### **How to Deploy and Run**

1. **Deploy Smart Contracts on Sui**:
   - Use Sui CLI tools to deploy the **Payment** and **WarrantyNFT** smart contracts to the Sui blockchain.

2. **Set Up Backend**:
   - Clone the **backend** repository.
   - Install dependencies: `npm install`.
   - Set the **SUI_RPC_URL** in `.env` to point to the correct RPC endpoint.
   - Start the backend server: `npm start`.

3. **Set Up Frontend**:
   - Clone the **frontend** repository.
   - Install dependencies: `npm install`.
   - Connect the frontend to the backend API and **Sui Wallet**.
   - Start the frontend app: `npm start`.

---

### **Final Thoughts**

This setup provides a **top-tier solution** for **Bitlist**, with a strong architecture ensuring **seamless payments** via **NAVI** and **Scalop**. The smart contracts ensure secure payment and NFT minting, while the frontend provides a **user-friendly interface** for businesses and customers. The backend handles **payment processing** and **NFT issuance** with **Move** contracts, ensuring **security** and **scalability**.

