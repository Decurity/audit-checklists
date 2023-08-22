# Automated Market Maker protocols (AMM) Checklist
## General

- [ ] Does the AMM use any forked code, e.g. from Uniswap code base? Use contract-diff.xyz to compare the code.
- [ ] Are there any potential problems with rounding in formulas of product constants?
- [ ] `transfer()`, `transferFrom()` and even `safeTransfer()` functions can lead to the re-entrancy if called on untrusted tokens or hook-on-transfer tokens such as ERC777 and ERC677. And even if the `nonReentrant` modifier is used, a cross-contract view re-entrancy attack is possible. Is CEI pattern followed when updating `reserves[]`?
- [ ] Most AMMs support flashloan functionality (e.g. FlashSwaps in Uniswap), is callback function called after token transfer and not before?
- [ ] In some tokens `transfer()` and `transferFrom()` may return `false` instead of `revert()`. Is the return value of these functions checked?
- [ ] Does the AMM update the user's token balances using signed integer? Can `-int(amount)` lead to an integer overflow?
- [ ] Does the AMM support fee-on-transfer tokens? Is this fee taken into account in the swaps?
- [ ] Can arbitrary calls be made from the user input?

## TWAMM
- [ ] Does the TWAMM support rebasing tokens? What happens if during a long-term swap there is a balance change?
- [ ] Is there a check for the sufficient liquidity before the swap?

## Integration

- [ ] Callback functions in the external contracts that are called by the `swap()` function are required to check the address of the calling contract. Does the contract that integrates an AMM callback have this check? Are there any ways to bypass the check?
- [ ] Protocols that integrate AMMs for token swaps should calculate the `minAmountOut` before the swap. Are external oracles used? Is a sandwich attack possible?
