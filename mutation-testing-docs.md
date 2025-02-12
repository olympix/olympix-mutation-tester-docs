# Solidity Mutation Test Generator

## Overview

Mutation testing is a powerful technique for evaluating the effectiveness of your test suite by introducing small, systematic modifications (mutations) to your source code and verifying if your tests can detect these changes. While code coverage tells you what lines of code are executed by your tests, mutation testing tells you how effective your tests are at catching actual bugs.

### Why Mutation Testing Matters

Traditional metrics like code coverage can provide a false sense of security. Having 100% coverage doesn't necessarily mean your tests are meaningful—they might assert the wrong things or have weak assertions. Mutation testing provides a more meaningful metric by:

1. Measuring test suite effectiveness over time  
2. Identifying areas where tests might be insufficient  
3. Forcing developers to write more thorough assertions  
4. Discovering edge cases that weren't previously considered  

### Security Implications

In blockchain and smart contract development, mutation testing is particularly crucial for security. Many historical smart contract hacks occurred due to seemingly minor changes in business logic that weren't caught by existing test suites. Our mutation operators are specifically derived from real-world smart contract exploits—each mutation pattern in our tool corresponds to actual changes that led to significant security breaches in production contracts.

---

## Installation & Requirements

The mutation test generator is designed to be dependency-free and works with any Forge project. The only prerequisite is having a Forge project with unit tests.

- ✅ No external dependencies  
- ✅ Works with standard Forge unit tests out of the box.

---

## CLI Usage

### Basic Command

```bash
generate-mutation-tests [-w <workspace>] [-p <solidity-file>] [-t <timeout>]

Options
	•	-w, --workspace-path: Root project directory path (default: current directory)
	•	-p, --path: Solidity file path to mutate (can be specified multiple times)
	•	-t, --timeout: Timeout in seconds for each mutant test run (default: 300s, range: 10-500s)

	Tip: The timeout option is crucial as some mutations can cause infinite loops in test execution. Set this to slightly higher than your normal test suite execution time.
```
Mutation Operators

Our mutation operators are directly inspired by real-world smart contract exploits. Each operator represents a pattern of change that has historically led to security incidents.

Below is a comprehensive list of all currently supported operators.

1. Arithmetic Operator Mutations

```solidity 
// Original
amount + tax
// Mutated
amount - tax
```

	•	Description: Replaces arithmetic operators in expressions:
	•	+ ↔ -
	•	* ↔ /
	•	% → /
	•	** → *

2. Comparison Operator Mutations
```solidity 
// Original
amount > 0
// Mutated
amount < 0
```
	•	Description: Inverts comparison operators:
	•	== ↔ !=
	•	> ↔ <
	•	>= → <
	•	<= → >

3. Logical Operator Mutations (AND ↔ OR)
```solidity 
// Original
require(isEnabled && amount > 100)
// Mutated
require(isEnabled || amount > 100)
```

	•	Description: Swaps logical operators:
	•	&& ↔ ||
	•	|| ↔ && 

4. Condition Negation Mutations
```solidity 
// Original
if (taxEnabled)
// Mutated
if (!taxEnabled)
```

	•	Description: Negates a condition by adding or removing the ! operator.

5. Ternary Conditional Mutations
```solidity 
// Original
amount < 100 ? amount : 100 - tax
// Mutated
amount < 100 ? 100 - tax : amount
```
	•	Description: Swaps the “true” and “false” branches in a ternary expression (?:).

6. Function Call Mutations (delegatecall → call)
```solidity 
// Original
(address).delegatecall(data)
// Mutated
(address).call(data)
```
	•	Description: Replaces delegatecall with call.

7. Hex Number Literal Mutations
```solidity 
// Original
0xabcd1234
// Mutated 1 (→ 0)
0x0
// Mutated 2 (→ Another Random Hex)
0xdef12345
```
	•	Description:
	1.	Replaces any hex literal with 0x0.
	2.	Replaces a hex literal with a different randomly chosen hex literal found in the same function.

8. Remove emit Statement
```solidity 
// Original
emit Transfer(msg.sender, recipient, amount);
// Mutated
// (empty string)
```
	•	Description: Removes the entire emit statement.

9. Remove delete Operator
```solidity 
// Original
delete myStruct;
// Mutated
// (empty string)
```
	•	Description: Removes the delete keyword.

10. Storage Location Mutations
```solidity 
// Original
uint[] storage x;
// Mutated
uint[] memory x;
```

	•	Description: Swaps the variable declaration storage location:
	•	storage → memory
	•	memory → storage

11. Variable Assignment Operator Replacement
```solidity 
// Original
balances[msg.sender] += amount;
// Mutated
balances[msg.sender] -= amount;
```

	•	Description: Swaps += with -= (and vice versa).

12. State Variable Initialization Changes
```solidity 
// Original
bool public taxEnabled = true;
// Mutated
bool public taxEnabled = false;

// Original
uint public maxSupply = 1000;
// Mutated
uint public maxSupply = 1001;

// Original
string public greeting = "Hello";
// Mutated
string public greeting = "Mutation text"
```
	•	Description: Mutates state variable initial values based on type.

13. Modifier Removal Mutations
```solidity 
// Original
function toggleTax() public onlyOwner {
    // ...
}

// Mutated
function toggleTax() public {
    // ...
}
```
	•	Description: Removes function modifiers

 14. Address swap Mutations
```solidity
// Original
address(0xaaaaaaaaaaaaaaaaaaaaa).call();
// Mutated
address(0xbbbbbbbbbbbbbbbbbbbbb).call(); // where 0xbbbbbbbbbbbbbbbbbbbbb is another address in the current function
```
	•	Description: Swap addresses

Security Foundation

Each mutation operator in this tool was carefully selected based on extensive analysis of historical smart contract exploits. By studying security incidents and identifying the precise commits that introduced vulnerabilities, we’ve created a comprehensive set of mutations that represent real-world attack vectors.

This approach ensures that your test suite is validated against realistic threat models rather than purely theoretical vulnerabilities.
