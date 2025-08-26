# compute-ts-starter-kit

Features
REST API Server with Express.js and TypeScript
Swagger Documentation at /docs for interactive API testing
Official 0G AI Services with verified provider addresses
Automatic Ledger Management with startup initialization
TEE Verification for enhanced trust and security
Single-use Authentication headers for secure requests
Comprehensive Test Script for learning and debugging
BigInt Serialization for blockchain data compatibility
Enhanced Error Handling with troubleshooting guidance
🤖 Official 0G AI Services
The starter kit includes pre-configured access to official 0G AI services:

Model	Provider Address	Description	Verification
llama-3.3-70b-instruct	0xf07240Efa67755B5311bc75784a061eDB47165Dd	State-of-the-art 70B parameter model for general AI tasks	TEE (TeeML)
deepseek-r1-70b	0x3feE5a4dd5FDb8a32dDA97Bed899830605dBD9D3	Advanced reasoning model optimized for complex problem solving	TEE (TeeML)
📁 Repository Structure
0g-compute-starter-kit/
├── src/
│   ├── config/
│   │   └── swagger.ts           # Swagger/OpenAPI configuration
│   ├── controllers/
│   │   ├── accountController.ts # Account management endpoints
│   │   └── serviceController.ts # AI service endpoints
│   ├── routes/
│   │   ├── accountRoutes.ts     # Account route definitions
│   │   └── serviceRoutes.ts     # Service route definitions
│   ├── services/
│   │   └── brokerService.ts     # Core 0G broker integration
│   ├── index.ts                 # Express app entry point
│   └── startup.ts               # Application initialization
├── demo-compute-flow.ts         # Comprehensive demo script
├── DEMO_SCRIPT.md              # Demo script documentation
├── package.json                # Project configuration
├── tsconfig.json               # TypeScript configuration
└── README.md                   # This file
🚀 Quick Start
Prerequisites
Node.js 16+ and npm
Testnet ETH for transactions (Get from faucet)
Ethereum wallet with private key
Installation
Clone the repository:
git clone https://github.com/yourusername/0g-compute-starter-kit.git
cd 0g-compute-starter-kit
Install dependencies:
npm install
Set up environment variables:
# Create .env file
cp .env.example .env  # if available, or create manually
Add your configuration to .env:

PRIVATE_KEY=your_private_key_here_without_0x_prefix
PORT=4000
NODE_ENV=development
Build the project:
npm run build
Start the server:
npm start
Access the API:
REST API: http://localhost:4000
Swagger UI: http://localhost:4000/docs
🧪 Run the Complete Flow
Run the comprehensive demo script to see the entire 0G compute workflow:

npm run demo
This script demonstrates:

Wallet and broker initialization
Ledger account setup with funding
Service discovery and provider acknowledgment
AI query submission with payment processing
TEE verification and cost tracking
See DEMO_SCRIPT.md for detailed documentation.

📚 API Endpoints
Account Management
GET /api/account/info
Get current account information and ledger balance.

Response:

{
  "success": true,
  "accountInfo": {
    "ledgerInfo": ["balance_in_wei"],
    "infers": [],
    "fines": []
  }
}
POST /api/account/deposit
Deposit funds to your ledger account.

Request:

{
  "amount": 0.1
}
Response:

{
  "success": true,
  "message": "Deposit successful"
}
POST /api/account/refund
Request refund for unused funds.

Request:

{
  "amount": 0.05
}
AI Services
GET /api/services/list
List all available AI services with pricing and verification status.

Response:

{
  "success": true,
  "services": [
    {
      "provider": "0xf07240Efa67755B5311bc75784a061eDB47165Dd",
      "model": "llama-3.3-70b-instruct",
      "serviceType": "inference",
      "url": "https://...",
      "inputPrice": "1000000000000000",
      "outputPrice": "2000000000000000",
      "verifiability": "TeeML",
      "isOfficial": true,
      "isVerifiable": true
    }
  ]
}
POST /api/services/acknowledge-provider
Acknowledge a provider before using their services (required once per provider).

Request:

{
  "providerAddress": "0xf07240Efa67755B5311bc75784a061eDB47165Dd"
}
POST /api/services/query
Send a query to an AI service.

Request:

{
  "providerAddress": "0xf07240Efa67755B5311bc75784a061eDB47165Dd",
  "query": "What is the capital of France?",
  "fallbackFee": 0.01
}
Response:

{
  "success": true,
  "response": {
    "content": "The capital of France is Paris.",
    "metadata": {
      "model": "llama-3.3-70b-instruct",
      "isValid": true,
      "provider": "0xf07240Efa67755B5311bc75784a061eDB47165Dd",
      "chatId": "chatcmpl-..."
    }
  }
}
POST /api/services/settle-fee
Manually settle fees (legacy support for specific error cases).

Request:

{
  "providerAddress": "0xf07240Efa67755B5311bc75784a061eDB47165Dd",
  "fee": 0.000001
}
🔧 Development Scripts
# Development
npm run dev          # Start development server with hot reload
npm run watch        # Start development server with file watching
npm run serve        # Alternative development command

# Production
npm run build        # Compile TypeScript to JavaScript
npm start           # Start production server

# Testing
npm run demo   # Run comprehensive workflow demo
🏗️ Core Architecture
Broker Service
The brokerService is a singleton that manages all interactions with the 0G Compute Network:

Wallet Management: Automatic wallet initialization with ethers.js
Provider Operations: Service discovery and provider acknowledgment
Query Processing: AI query submission with authentication
Payment Handling: Automatic micropayments and verification
Error Management: Enhanced error messages with troubleshooting
Application Initialization
On startup, the application automatically:

Checks for existing ledger accounts
Creates accounts with initial funding if needed (0.01 ETH default)
Logs initialization status
Starts the Express server
Authentication Flow
Provider Acknowledgment: Required once per provider
Header Generation: Single-use authentication headers per request
Query Submission: OpenAI-compatible API calls
Response Processing: TEE verification and payment settlement
🔒 Security Best Practices
Environment Variables: Store private keys securely in .env
Input Validation: All endpoints validate request parameters
Error Sanitization: Error messages don't expose sensitive data
Single-use Headers: Authentication headers prevent replay attacks
Network Validation: RPC endpoint verification
🚨 Error Handling
Common Issues and Solutions
Provider Acknowledgment Required
curl -X POST http://localhost:4000/api/services/acknowledge-provider \
  -H "Content-Type: application/json" \
  -d '{"providerAddress": "0xf07240Efa67755B5311bc75784a061eDB47165Dd"}'
Insufficient Balance
# Check balance
curl http://localhost:4000/api/account/info

# Add funds
curl -X POST http://localhost:4000/api/account/deposit \
  -H "Content-Type: application/json" \
  -d '{"amount": 0.1}'
Provider Not Responding
Get alternative providers:

curl http://localhost:4000/api/services/list
Headers Already Used
The system automatically generates new headers for each request. This error indicates a system issue - retry the request.

Legacy Error: Unsettled Previous Fee
If you encounter:

Error: invalid previousOutputFee: expected 0.00000000000000015900000000000001138, got 0
Use the settle-fee endpoint with the exact amount:

{
  "providerAddress": "0x3feE5a4dd5FDb8a32dDA97Bed899830605dBD9D3",
  "fee": 0.00000000000000015900000000000001138
}
📋 Example Usage
Complete Workflow with cURL
Check available services:
curl http://localhost:4000/api/services/list
Check account balance:
curl http://localhost:4000/api/account/info
Acknowledge a provider:
curl -X POST http://localhost:4000/api/services/acknowledge-provider \
  -H "Content-Type: application/json" \
  -d '{"providerAddress": "0xf07240Efa67755B5311bc75784a061eDB47165Dd"}'
Send a query:
curl -X POST http://localhost:4000/api/services/query \
  -H "Content-Type: application/json" \
  -d '{
    "providerAddress": "0xf07240Efa67755B5311bc75784a061eDB47165Dd",
    "query": "Explain quantum computing in simple terms",
    "fallbackFee": 0.01
  }'
Integration Example
import { ethers } from 'ethers';
import { createZGComputeNetworkBroker } from '@0glabs/0g-serving-broker';
import OpenAI from 'openai';

// Initialize broker
const provider = new ethers.JsonRpcProvider('https://evmrpc-testnet.0g.ai');
const wallet = new ethers.Wallet(process.env.PRIVATE_KEY!, provider);
const broker = await createZGComputeNetworkBroker(wallet);

// Fund account
await broker.ledger.addLedger(0.1);

// Acknowledge provider
const providerAddress = '0xf07240Efa67755B5311bc75784a061eDB47165Dd';
await broker.inference.acknowledgeProviderSigner(providerAddress);

// Get service info
const { endpoint, model } = await broker.inference.getServiceMetadata(providerAddress);
const headers = await broker.inference.getRequestHeaders(providerAddress, query);

// Send query
const openai = new OpenAI({ baseURL: endpoint, apiKey: '', defaultHeaders: headers });
const completion = await openai.chat.completions.create({
  messages: [{ role: 'user', content: 'Hello, AI!' }],
  model: model,
});

// Process response
const isValid = await broker.inference.processResponse(
  providerAddress,
  completion.choices[0].message.content,
  completion.id
);
🌐 Network Configuration
Testnet RPC: https://evmrpc-testnet.0g.ai
Faucet: https://faucet.0g.ai
Chain ID: 16600 (0G Testnet)
📦 Dependencies
Core Dependencies
@0glabs/0g-serving-broker - 0G Compute Network SDK
ethers - Ethereum wallet and provider functionality
openai - OpenAI-compatible API client
express - Web framework for REST API
dotenv - Environment variable management
crypto-js - Cryptographic utilities
Development Dependencies
typescript - TypeScript compiler
ts-node - TypeScript execution for Node.js
nodemon - Development server with hot reload
@types/* - TypeScript type definitions
🎯 Use Cases
This starter kit is perfect for:

Web Applications requiring AI integration
API Services with decentralized AI backends
Prototyping AI applications with micropayments
Learning 0G Compute Network integration
Testing different AI models and providers
🔄 Branch Structure
Main Branch (Current)
REST API implementation with Express framework and Swagger documentation.

CLI Branch
Command-line interface implementation:

git checkout cli-version
🛠️ Troubleshooting
Common Setup Issues
Missing Private Key: Ensure PRIVATE_KEY is set in .env
Insufficient ETH: Get testnet ETH from the faucet
Network Issues: Check connectivity to 0G testnet
Port Conflicts: Change PORT in .env if 4000 is in use
Performance Tips
Provider Selection: Use official providers for best reliability
Balance Management: Maintain sufficient OG tokens for queries
Error Handling: Implement proper retry logic in production
Rate Limiting: Consider implementing rate limits for public APIs
🔗 Additional Resources
0G Compute Documentation: https://docs.0g.ai/build-with-0g/compute-network
SDK Examples: https://github.com/0glabs/compute-examples
Discord Support: https://discord.gg/0glabs
Demo Script Guide: DEMO_SCRIPT.md
📝 License
This project is licensed under the MIT License - see the LICENSE file for details.
