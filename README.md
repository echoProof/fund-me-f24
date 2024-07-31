# FundMe Smart Contract

The **FundMe** smart contract allows users to fund the contract with Ether and enables the contract owner to withdraw the funds. It utilizes Chainlink's price feed to convert the amount funded to a minimum threshold in USD.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Contract Structure](#contract-structure)
- [License](#license)

## Overview

The FundMe contract is designed to accept Ether contributions from multiple users, ensuring that each contribution meets a minimum threshold when converted to USD. The contract owner can withdraw the total funds contributed by users.

## Features

- **Funding**: Users can contribute Ether to the contract, which will be recorded.
- **Minimum Contribution**: Contributions must meet a minimum threshold (5 USD) to be accepted.
- **Owner Withdrawals**: Only the contract owner can withdraw the accumulated Ether.
- **Fallback & Receive Functions**: The contract automatically accepts Ether sent directly to it via the fallback or receive functions.

## Getting Started

To deploy the FundMe contract, you'll need:

1. **Solidity Environment**: Use tools like [Remix](https://remix.ethereum.org/) or a local development environment with Hardhat or Truffle.
2. **Chainlink Price Feed**: Obtain the address of a Chainlink price feed for ETH/USD based on your network (e.g., Ethereum Mainnet, Goerli, etc.).

### Prerequisites

- Install the required dependencies:
  ```bash
  npm install @chainlink/contracts
  ```

## Usage

1. **Deploy the Contract**:
   - Deploy the contract with the price feed address as an argument:
     ```solidity
     FundMe fundMe = new FundMe(priceFeedAddress);
     ```

2. **Fund the Contract**:
   - Call the `fund()` function and send Ether to the contract:
     ```solidity
     fundMe.fund{value: msg.value}();
     ```

3. **Withdraw Funds**:
   - The owner can call the `withdraw()` or `cheaperWithdraw()` function to withdraw funds from the contract:
     ```solidity
     fundMe.withdraw();
     ```

## Contract Structure

- **State Variables**:
  - `s_addressToAmountFunded`: Tracks the amount funded by each address.
  - `s_funders`: Array of all funders' addresses.
  - `i_owner`: Immutable variable for the owner's address.
  - `MINIMUM_USD`: Constant value defining the minimum contribution in USD.
  - `s_priceFeed`: Chainlink price feed interface for ETH/USD conversion.

- **Functions**:
  - `fund()`: Allows users to send Ether to the contract.
  - `getVersion()`: Returns the version of the price feed.
  - `cheaperWithdraw()`: Efficient method for the owner to withdraw funds.
  - `withdraw()`: Withdraw funds by resetting the contributions of funders.
  - `fallback()`: Automatically calls `fund()` when Ether is sent to the contract.
  - `receive()`: Automatically calls `fund()` when Ether is sent directly.
  - `getAddressToAmountFunded()`: Returns the funding amount of a specific address.
  - `getFunder()`: Returns the address of a funder by index.
  - `getOwner()`: Returns the owner's address.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.