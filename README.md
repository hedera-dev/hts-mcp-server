# Hedera MCP Server

A Model Context Protocol (MCP) server for interacting with the Hedera network. This server provides tools for creating Hedera wallets, checking account balances, building transactions, and sending signed transactions.

## ⚠️ Security Warning

**This is a demo implementation and should NOT be used in production.** The current implementation has several security vulnerabilities:

- Private keys are sent in response bodies
- No encryption of sensitive data
- No transaction verification mechanisms

This implementation is meant to demonstrate the MCP concept and should be properly secured before being used in a production environment.

## Main Objectives

The primary purpose of this MCP server is to demonstrate how transaction construction and execution can be handled by the MCP server, with the client only needing to verify the transaction. Specifically:

1. Transaction construction happens entirely on the MCP server
2. Clients signs the constructed transactions
3. The MCP server handles transaction submission to the Hedera network
4. This architecture allows for centralized transaction logic and reduces client-side complexity

## Prerequisites

- Node.js (v18 or higher)
- npm or yarn
- A Hedera account (for testnet or mainnet)

## Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/hedera-dev/hedera-mcp-server.git
   cd hedera-mcp-server
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Create a `.env` file in the root directory with your Hedera credentials:
   ```
   HEDERA_OPERATOR_ID=your-operator-account-id
   HEDERA_OPERATOR_KEY=your-operator-private-key
   HEDERA_NETWORK=testnet  # or mainnet
   PORT=3000  # optional, defaults to 3000
   ```

## Building the Application

Compile the TypeScript code:

```bash
npm run build
```

For development with automatic recompilation:

```bash
npm run dev
```

## Running the Server

Start the server:

```bash
npm start
```

The server will be available at http://localhost:3000 (or the port specified in your .env file).

## Testing with the Test Client

The repository includes a test client script that demonstrates how to connect to the MCP server and use its tools. This client provides a complete end-to-end flow demonstrating all available tools.

Run the test client:

```bash
node test-client.js
```

The test client will:
1. Connect to the MCP server
2. List available tools
3. Create a new Hedera wallet (create-wallet tool)
4. Check the balance of the new account (check-balance tool)
5. Build a transaction transferring the account's entire balance to the operator account (build-transaction tool)
6. Sign the transaction on the client side using the Hedera SDK
7. Submit the signed transaction to the Hedera network (send-transaction tool)
8. Display the transaction result

This demonstrates the complete lifecycle of interacting with the Hedera network through the MCP server, from account creation to transaction submission.

## Available MCP Tools

The server provides the following tools:

1. **create-wallet**: Creates a new Hedera account with a minimal initial balance
   - No input parameters required
   - Returns account ID, public key, and private key

2. **check-balance**: Checks the balance of a Hedera account
   - Input: `accountId` (string)
   - Returns the account balance in tinybars

3. **build-transaction**: Builds a transfer transaction (without signing)
   - Inputs:
     - `senderAccountId` (string)
     - `recipientAccountId` (string)
     - `amount` (number, in tinybars)
   - Returns a base64-encoded transaction

4. **send-transaction**: Sends a signed transaction to the Hedera network
   - Input: `signedTransaction` (string, base64-encoded)
   - Returns transaction status and ID

## Deployment

A simple deployment script is included:

```bash
./deploy.sh
```

Make sure to make it executable first:

```bash
chmod +x deploy.sh
```