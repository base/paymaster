# Contributing to Base Paymaster

Thank you for your interest in contributing to the Base Paymaster project! This document provides guidelines for contributing to this ERC-4337 paymaster implementation.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Contributing Workflow](#contributing-workflow)
- [Coding Standards](#coding-standards)
- [Testing](#testing)
- [Security](#security)

## Code of Conduct

This project adheres to a standard of professional conduct. By participating, you agree to:

- Be respectful and constructive in all interactions
- Focus on what's best for the community and users
- Accept constructive criticism gracefully
- Prioritize security and safety in all contributions

## Getting Started

### Prerequisites

- [Foundry](https://book.getfoundry.sh/getting-started/installation) - Ethereum development toolkit
- Git
- A code editor (VS Code, Vim, etc.)

### Project Overview

This repository contains verifying paymaster contracts for ERC-4337 account abstraction:

- **Paymaster.sol** - Core verifying paymaster with signature validation
- **LimitingPaymaster.sol** - Extended paymaster with usage limits and rate limiting

The contracts enable gas sponsorship for user operations through cryptographic signatures.

## Development Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/base/paymaster.git
   cd paymaster
   ```

2. **Install dependencies:**
   ```bash
   forge install
   ```

3. **Build the project:**
   ```bash
   forge build
   ```

4. **Run tests:**
   ```bash
   forge test
   ```

## Contributing Workflow

1. **Fork and branch:**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes:**
   - Follow the coding standards below
   - Add tests for new functionality
   - Update documentation as needed

3. **Test thoroughly:**
   ```bash
   forge test -v
   ```

4. **Commit with clear messages:**
   ```bash
   git commit -m "feat: add rate limiting per user"
   ```

5. **Push and create a pull request:**
   ```bash
   git push origin feature/your-feature-name
   ```

## Coding Standards

### Solidity Style

- Follow [Solidity Style Guide](https://docs.soliditylang.org/en/latest/style-guide.html)
- Use Solidity version `0.8.20` as specified in `foundry.toml`
- Enable `via_ir` optimization for gas efficiency
- Use NatSpec comments for all public/external functions:

```solidity
/**
 * @notice Validates a user operation and calculates the required prefund
 * @param userOp The user operation to validate
 * @param userOpHash The hash of the user operation
 * @param requiredPrefund The amount of ETH to prefund
 * @return validationData Packed validation data (sigFailed, validUntil, validAfter)
 * @return context Context passed to postOp
 */
function validatePaymasterUserOp(
    PackedUserOperation calldata userOp,
    bytes32 userOpHash,
    uint256 requiredPrefund
) external returns (bytes memory context, uint256 validationData);
```

### Code Organization

- Keep contracts in `src/`
- Tests in `test/`
- Libraries in `lib/`
- Use remappings for clean imports

### Gas Optimization

- Use `optimizer_runs = 999999` for production deployments
- Prefer `calldata` over `memory` for external function parameters
- Use `via_ir = true` for additional optimizations
- Minimize storage writes

## Testing

### Running Tests

```bash
# Run all tests
forge test

# Run with verbose output
forge test -v

# Run specific test
forge test --match-test testValidatePaymasterUserOp

# Run with gas reporting
forge test --gas-report
```

### Test Requirements

- All new functionality must include tests
- Aim for high coverage of critical paths
- Test edge cases and failure modes
- Use fuzzing where appropriate:

```solidity
function testFuzz_ValidateSignature(bytes32 hash, uint256 key) public {
    // Test with random inputs
}
```

### Test Structure

```solidity
contract PaymasterTest is Test {
    Paymaster paymaster;
    
    function setUp() public {
        // Deploy contracts
        paymaster = new Paymaster();
    }
    
    function test_ValidateUserOp() public {
        // Test implementation
    }
}
```

## Security

### Security Considerations

Paymasters handle real value (ETH for gas). Always consider:

1. **Signature validation** - Ensure all signatures are properly verified
2. **Replay protection** - Prevent reuse of signatures
3. **Rate limiting** - Protect against abuse
4. **Access control** - Restrict sensitive functions
5. **Reentrancy** - Use checks-effects-interactions pattern

### Security Checklist

Before submitting PRs:

- [ ] No unchecked external calls
- [ ] All signatures validated
- [ ] Proper access controls in place
- [ ] No storage collision risks
- [ ] Gas limits considered
- [ ] Edge cases handled

### Vulnerability Disclosure

If you discover a security vulnerability:

1. **DO NOT** open a public issue
2. Email security concerns to the Base team privately
3. Allow time for remediation before disclosure

## Areas for Contribution

### High Priority

- Additional test coverage
- Gas optimization improvements
- Documentation enhancements
- Integration examples

### Medium Priority

- Additional paymaster strategies
- Monitoring and analytics tools
- Developer tooling

### Documentation

- Improve inline comments
- Add usage examples
- Create integration guides
- Document deployment procedures

## Questions?

- Open an issue for bugs or feature requests
- Join the Base Discord for discussions
- Check existing issues and PRs before creating new ones

Thank you for contributing to Base Paymaster!
