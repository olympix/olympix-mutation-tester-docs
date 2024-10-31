# Solidity Mutation Test Generator

## Overview

Mutation testing is a powerful technique for evaluating the effectiveness of your test suite by introducing small, systematic modifications (mutations) to your source code and verifying if your tests can detect these changes. While code coverage tells you what lines of code are executed by your tests, mutation testing tells you how effective your tests are at catching actual bugs.

### Why Mutation Testing Matters

Traditional metrics like code coverage can provide a false sense of security. Having 100% coverage doesn't necessarily mean your tests are meaningful - they might assert the wrong things or have weak assertions. Mutation testing provides a more meaningful metric by:

1. Measuring test suite effectiveness over time
2. Identifying areas where tests might be insufficient
3. Forcing developers to write more thorough assertions
4. Discovering edge cases that weren't previously considered

### Security Implications

In blockchain and smart contract development, mutation testing is particularly crucial for security. Many historical smart contract hacks occurred due to seemingly minor changes in business logic that weren't caught by existing test suites. Our mutation operators are specifically derived from real-world smart contract exploits - each mutation pattern in our tool corresponds to actual changes that led to significant security breaches in production contracts.

## Installation & Requirements

The mutation test generator is designed to be dependency-free and works with any Forge project. The only prerequisite is having a Forge project with unit tests.

- ✅ No external dependencies
- ✅ Works with standard Forge unit tests out of box.

## CLI Usage

### Basic Command
```bash
generate-mutation-tests [-w <workspace>] [-p <solidity-file>] [-t <timeout>]
```

### Options
- `-w, --workspace-path`: Root project directory path (default: current directory)
- `-p, --path`: Solidity file path to mutate (can be specified multiple times)
- `-t, --timeout`: Timeout in seconds for each mutant test run (default: 300s, range: 10-500s)

The timeout option is crucial as some mutations can cause infinite loops in test execution. Set this to slightly higher than your normal test suite execution time.

## Mutation Operators

Our mutation operators are directly inspired by real-world smart contract exploits. Each operator represents a pattern of change that has historically led to security incidents.

### 1. Arithmetic Operator Mutations
```solidity
// Original
amount + tax
// Mutated
amount - tax
```
**Real-world impact**: Multiple DeFi protocols have been exploited due to arithmetic operator errors, particularly in fee calculations and reward distributions.

### 2. Comparison Operator Mutations
```solidity
// Original
amount > 0
// Mutated
amount < 0
```
**Real-world impact**: Several flash loan attacks succeeded due to incorrect comparison operators in amount validation.

### 3. Logical Operator Mutations
```solidity
// Original
isEnabled && amount > 100
// Mutated
isEnabled || amount > 100
```
**Real-world impact**: Access control vulnerabilities have occurred due to logical operator mistakes in permission checks.

### 4. Logical State Mutations
```solidity
// Original
if(taxEnabled)
// Mutated
if(!taxEnabled)
```
**Real-world impact**: State validation bypasses have led to unauthorized actions in several protocols.

### 5. Variable Assignment Mutations
```solidity
// Original
balances[msg.sender] -= amount
// Mutated
balances[msg.sender] += amount
```
**Real-world impact**: Multiple token contracts have been exploited due to incorrect assignment operators in balance updates.

### 6. Value Assignment Mutations
```solidity
// Original
bool public taxEnabled = true;
// Mutated
bool public taxEnabled = false;
```
**Real-world impact**: Default state values have been exploited in several contracts where initialization values were critical for security.

### 7. Ternary Conditional Mutations
```solidity
// Original
amount < 100 ? amount : 100 - tax
// Mutated
amount < 100 ? 100 - tax : amount
```
**Real-world impact**: Complex conditional logic errors have led to incorrect value calculations in DeFi protocols.

### 8. Modifier Removal Mutations
```solidity
// Original
function toggleTax() public onlyOwner
// Mutated
function toggleTax() public
```

(And many more on the way!)

**Real-world impact**: Several high-profile hacks occurred due to missing or incorrectly applied access control modifiers.

## Security Foundation

Each mutation operator in this tool was carefully selected based on extensive analysis of historical smart contract exploits. By studying security incidents and identifying the precise commits that introduced vulnerabilities, we've created a comprehensive set of mutations that represent real-world attack vectors.

This approach ensures that your test suite is validated against realistic threat models rather than theoretical vulnerabilities.
