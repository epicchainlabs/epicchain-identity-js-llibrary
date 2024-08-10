# EpicChain Identity JavaScript SDK

<p align="center">
  <a href="https://github.com/epicchainlabs/epicchain-identity-js-llibrary/blob/master/LICENSE">
    <img src="https://img.shields.io/badge/license-MIT-blue.svg?color=green">
  </a>
</p>

## Overview

Welcome to the **EpicChain Identity JavaScript SDK**! This sophisticated library is designed to provide seamless integration with the EpicChain blockchain platform, offering powerful tools for managing self-sovereign identities. By leveraging this SDK, developers can easily handle various identity-related functionalities directly within their JavaScript applications. Whether you're working on a web app, a Node.js server, or any other JavaScript environment, the EpicChain Identity SDK is here to help you manage wallets, issue and verify claims, and interact with self-sovereign identities on the EpicChain network.

## Key Features

The **EpicChain Identity JavaScript SDK** offers an array of features designed to simplify and enhance your development experience with EpicChain. Here’s a closer look at what you can do with the SDK:

### Wallet Management

- **Create and Manage Wallets:** Effortlessly create new wallets and manage existing ones. The SDK supports various operations including wallet creation, encryption, and decryption.
- **Decentralized Identifiers (DIDs):** Generate and manage DIDs for self-sovereign identity applications, supporting both public and private networks.
- **Encrypted Storage:** Securely encrypt and store wallet data, ensuring that sensitive information is protected against unauthorized access.

### Claims Management

- **Issue Claims:** Create and issue claims that are associated with self-sovereign identities. The SDK supports the creation of both revocable and non-revocable claims.
- **Add and Revoke Claims:** Add new claims to a wallet and revoke existing claims as needed, providing flexibility in managing identity data.
- **Claim Export and Import:** Export and import claims in a secure format, enabling easy backup and migration of identity data.

### Identity Verification

- **Verify Claims:** Validate claims offline using issuer public keys, and perform online verification to check the legitimacy and status of claims.
- **Trust Verification:** Ensure that the issuer of a claim is trusted by checking against a Root of Trust, which helps in maintaining the integrity of identity data.

### Integration with Self-Sovereign Identity Solutions

- **Integrate Easily:** The SDK is designed to integrate smoothly with EpicChain’s self-sovereign identity solutions, making it easier to incorporate decentralized identity management into your applications.

## Getting Started

To start using the **EpicChain Identity JavaScript SDK**, follow the steps below. This guide will walk you through the setup and usage of the SDK, ensuring you are ready to integrate EpicChain’s identity solutions into your projects.

### Prerequisites

Before beginning, ensure you have the following tools installed:

1. **Node.js:** The JavaScript runtime environment needed to execute server-side code.
2. **Yarn:** A package manager for managing project dependencies and scripts.

### Setup Instructions

Follow these steps to set up and configure the SDK in your development environment:

#### Step 1: Install Dependencies

Navigate to the `epicchain-identity-js-llibrary` directory and install the necessary dependencies:

```bash
cd /path/to/epicchain-identity-js-llibrary
yarn
yarn bootstrap
yarn build
yarn dist
```

This sequence of commands will install all required packages, build the project, and prepare it for use.

#### Step 2: Install Additional Dependencies

In the `epicchain-identity-js-llibrary` directory, install additional dependencies that are required for the SDK to function correctly:

```bash
yarn add -P loglevel bignumber.js axios
yarn
yarn build
```

This step ensures that all additional packages are installed and the project is built with the latest updates.

## Installation

To include the EpicChain Identity JavaScript SDK in your project, use the following npm command:

```bash
npm install @epicchainlabs/epicchain-identity-js-llibrary --save
```

This command will add the SDK to your project’s dependencies, making it available for use in your code.

## Usage

Once installed, you can use the SDK in various environments. Below are examples of how to use the SDK in different contexts:

### Node.js Environment

To use the SDK in a Node.js environment, first import the library:

```javascript
const seraphId = require('@epicchainlabs/epicchain-identity-js-llibrary');
```

#### Managing Seraph ID Wallets

- **Create a New Wallet:**

  ```javascript
  const wallet = new seraphId.SeraphIDWallet({ name: 'MyWallet' });
  ```

  This command creates a new wallet with the specified name.

- **Generate a New DID:**

  ```javascript
  const myDID = wallet.createDID(seraphId.DIDNetwork.PrivateNet);
  ```

  This generates a new DID for the private network. Replace `PrivateNet` with the appropriate network if needed.

- **Add a Claim to Wallet:**

  ```javascript
  wallet.addClaim(claim);
  ```

  Add a new claim to the wallet. Ensure that the `claim` object is properly formatted according to your requirements.

- **Encrypt and Export Wallet:**

  ```javascript
  wallet.encrypt('password');
  const exportedWalletJSON = JSON.stringify(wallet.export());
  ```

  Encrypt the wallet with a password and export the wallet data as a JSON string for backup or transfer.

- **Import and Decrypt Wallet:**

  ```javascript
  const wallet = new seraphId.SeraphIDWallet(JSON.parse(exportedWalletJSON));
  wallet.decrypt('password');
  const allClaims = wallet.getAllClaims(myDID);
  const claim = wallet.getClaim('claimId');
  ```

  Import a wallet from a JSON string, decrypt it with the password, and access claims associated with a specific DID.

#### Issuing Claims

- **Create Issuer Instance:**

  ```javascript
  const issuer = new seraphId.SeraphIDIssuer('issuerSmartContractScriptHash', 'http://localhost:10332', seraphId.DIDNetwork.PrivateNet, 5195086);
  ```

  Create an instance of the issuer with the specified smart contract script hash, network URL, network type, and contract version.

- **Register a New Schema and Issue a Claim:**

  ```javascript
  issuer.registerNewSchema('schemaName', ['firstName', 'lastName', 'age'], true, 'issuerPrivateKey');
  const claim = issuer.createClaim('claimId', 'schemaName', { 'firstName': 'John', 'lastName': 'Doe', 'age': 26 }, myDID);
  issuer.issueClaim(claim, 'issuerPrivateKey');
  ```

  Register a new credentials schema and issue a claim based on this schema. Replace placeholders with actual values.

- **Revoke a Claim:**

  ```javascript
  issuer.revokeClaimById('claimId', 'issuerPrivateKey');
  ```

  Revoke an existing claim by its ID, ensuring the issuer’s private key is provided for authorization.

#### Verifying Claims

- **Create Verifier Instance:**

  ```javascript
  const verifier = new seraphId.SeraphIDVerifier('issuerSmartContractScriptHash', 'http://localhost:10332', seraphId.DIDNetwork.PrivateNet, 5195086);
  ```

  Instantiate a verifier with the smart contract script hash, network URL, and other required parameters.

- **Verify and Validate Claims:**

  ```javascript
  const schema = verifier.getSchemaDetails('schemaName');
  const verified = verifier.verifyOffline(claim, 'issuerPublicKey');
  const valid = verifier.validateClaim(claim, function customClaimValidator(clm) {
      return clm.attributes.age > 18;
  });
  const trusted = verifier.isIssuerTrusted('scriptHashOfRoTSmartContract', claim.issuerDID, claim.schemaName);
  ```

  Retrieve schema details, verify claims offline using issuer’s public key, validate claims with custom functions, and check if the issuer is trusted.

#### Root of Trust

- **Create Root of Trust Instance:**

  ```javascript
  const rot = new seraphId.SeraphIDRootOfTrust('rotSmartContractScriptHash', 'http://localhost:10332', seraphId.DIDNetwork.PrivateNet, 5195086);
  ```

  Create an instance of the Root of Trust with the specified smart contract script hash and network details.

- **Register and Deactivate Issuers:**

  ```javascript
  rot.registerIssuer('did:epicchainid:priv:XrTATFdLEVbbcAwfiQZF7qqLHMfWPa3XxA', 'SchemaName', 'rootOfTrustPrivateKey');
  rot.deactivateIssuer('did:epicchainid:priv:XrTATFdLEVbbcAwfiQZF7qqLHMfWPa3XxA', 'SchemaName', 'rootOfTrustPrivateKey');
  const trusted = rot.isTrusted('did:epicchainid:priv:XrTATFdLEVbbcAwfiQZF7qqLHMfWPa3XxA', 'SchemaName');
  ```

  Register issuers with the Root of Trust and deactivate them as necessary. Check the trust status of issuers.

# Contributing

We welcome contributions to the EpicChain Identity JavaScript SDK. If you’d like to contribute, please follow these guidelines:

## Setup

To get started with development, clone the repository and install dependencies:

```bash
git clone https://github.com/epicchainlabs/epicchain-identity-js-llibrary

aph-id-sdk.git
cd epicchain-identity-js-llibrary
yarn
yarn build
```

Ensure you have the following prerequisites:

- **Yarn (v1.16.0 or higher):** For managing project dependencies.
- **Node.js (latest LTS version):** Required for running and building the project.

## Testing

Before running unit tests, ensure:

- **Smart Contracts Deployment:** Both the Issuer and RootOfTrust smart contracts should be deployed on your network.
- **Test Data:** Network information and test data should be correctly set up in `__tests__/test-data.json`.

Run the tests using:

```bash
yarn test
```

This will execute all unit tests to verify the functionality of the SDK.

# References

For more information and resources related to Seraph ID and EpicChain, please refer to the following:

- **[Seraph ID Official Page](https://www.seraphid.io):** Learn more about self-sovereign identity and its applications.
- **[Seraph ID Demo Application](https://github.com/epicchainlabs/seraph-id-demo):** Explore a demo application showcasing the use of Seraph ID.
- **[Seraph ID Smart Contract Templates](https://github.com/epicchainlabs/seraph-id-smart-contracts):** Access smart contract templates and examples for implementing Seraph ID solutions.
- **[Seraph ID Browser Extension](https://github.com/epicchainlabs/seraph-id-chrome-extension):** Download and use the Seraph ID extension for Chrome.
- **[Seraph ID DID Resolver](https://github.com/epicchainlabs/seraph-id-did-driver):** Integrate with the Seraph ID DID resolver for enhanced DID management.

# License

The EpicChain Identity JavaScript SDK is licensed under the [MIT License](https://github.com/epicchainlabs/epicchain-identity-js-llibrary/blob/master/LICENSE). This open-source license allows you to use, modify, and distribute the SDK under the terms of the MIT License.
