# `Deci-mate` a Decimal Conversions Library

A comprehensive Sway library for precise decimal arithmetic operations in the Fuel blockchain ecosystem. This library provides robust, gas-optimized functions for percentage calculations and decimal precision conversions with proper rounding mechanisms.

## Overview

The Decimal Conversions Library addresses the common requirement of performing accurate decimal arithmetic in blockchain applications where floating-point operations are not available. It implements industry-standard rounding algorithms and overflow protection mechanisms suitable for financial and token-related calculations.

## Features

- **Percentage Calculations**: Accurate percentage amount calculations with configurable decimal precision
- **Decimal Conversion**: Seamless conversion between different decimal precisions with proper rounding
- **Overflow Protection**: Comprehensive overflow detection and prevention mechanisms
- **Gas Optimization**: Efficient lookup tables and optimized algorithms for reduced gas consumption
- **Extensive Testing**: Comprehensive test suite covering edge cases and boundary conditions

## Installation

Add this library to your Forc.toml file:

```bash
forc add decimal-conversions
```

```toml
[dependencies]
decimal_conversions = 0.1.0
```

## API Reference

### convert_decimals

Converts an amount between different decimal precisions with proper rounding.

```sway
pub fn convert_decimals(amount: u64, original_decimals: u64, target_decimals: u64) -> u64
```

**Parameters:**

- `amount`: The amount to convert
- `original_decimals`: Source decimal precision
- `target_decimals`: Target decimal precision

**Returns:**

- The converted amount with appropriate scaling and rounding
- Returns original amount if decimals are equal or amount is 0
- Panics if |original_decimals - target_decimals| >= 19 or multiplication would overflow

**Examples:**

```sway
// Scale up: Convert 1 token from 6 to 9 decimals
let result = convert_decimals(1_000_000, 6, 9); // Returns 1_000_000_000

// Scale down: Convert with rounding from 9 to 6 decimals
let result = convert_decimals(1_500_000_000, 9, 6); // Returns 1_500_000
```

### calculate_percent_amount

Calculates the percentage amount with proper rounding using the formula: `(amount * fee_percent) / (100 * 10^decimals)`.

```sway
pub fn calculate_percent_amount(amount: u64, fee_percent: u64, decimals: u64) -> u64
```

**Parameters:**

- `amount`: The base amount to calculate percentage from
- `fee_percent`: The percentage to calculate (e.g., 150 for 1.5%)
- `decimals`: The decimal precision of the result

**Returns:**

- The calculated percentage amount with proper rounding
- Returns 0 if amount or fee_percent is 0
- Panics if decimals >= 19 or multiplication would overflow

**Examples:**

```sway
// 0.5% of 1 token with 6 decimals
let result = calculate_percent_amount(1_000_000, 50, 6); // Returns 5000

// 1% of 100 tokens with 6 decimals  
let result = calculate_percent_amount(100_000_000, 100, 6); // Returns 1_000_000
```

## Implementation Details

### Precision and Limits

- Maximum supported decimal precision: 18 decimals
- Maximum safe exponent for power-of-ten calculations: 19
- Overflow protection implemented for all multiplication operations
- Rounding implemented using banker's rounding (add half divisor before division)

### Gas Optimization

The library implements several gas optimization techniques:

- **Constant Lookup Tables**: Pre-computed values for common operations
- **Conditional Logic Optimization**: Efficient branching for common vs. edge cases
- **Direct Array Access**: Minimized computational overhead

### Error Handling

The library uses Sway's assertion mechanisms for error handling:

- **Overflow Protection**: Assertions prevent arithmetic overflow
- **Boundary Validation**: Input parameter validation for supported ranges
- **Panic Conditions**: Clear panic conditions documented for each function

## Testing

The library includes a comprehensive test suite with 19 test cases covering:

- Zero input handling
- Basic functionality verification
- Rounding behavior validation
- Boundary condition testing
- Overflow protection verification
- Helper function validation
- Edge case scenarios

Run the test suite using:

```bash
forc test
```

## Safety Considerations

### Overflow Protection

All functions implement overflow protection mechanisms:

- Pre-multiplication overflow checks
- Boundary validation for decimal precision parameters
- Safe conversion limits enforcement

### Rounding Behavior

The library implements consistent rounding behavior:

- Banker's rounding for percentage calculations
- Half-up rounding for decimal conversions
- Deterministic results for identical inputs

## Performance Characteristics

### Gas Consumption

Typical gas consumption ranges:

- `calculate_percent_amount`: 500-900 gas units
- `convert_decimals`: 600-1100 gas units
- Variation depends on input values and decimal precision

### Computational Complexity

- Time complexity: O(1) for all operations
- Space complexity: O(1) with constant lookup tables
- No recursive operations or loops

## License

This library is distributed under standard open-source licensing terms. Please refer to the LICENSE file for complete licensing information.

## Contributing

Contributions are welcome through standard pull request procedures. Please ensure all contributions include:

- Comprehensive test coverage
- Documentation updates
- Gas consumption analysis
- Security consideration review

## Version History

### Version 1.0.0

- Initial implementation with core percentage and conversion functions
- Comprehensive test suite implementation
- Gas optimization through constant lookup tables
- Overflow protection mechanisms
