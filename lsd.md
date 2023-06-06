# Liquid Staking Derivatives (LSD) Checklist
## General

- [ ] How is data inserted into the validators registry? How are `WithdrawCredentials` set for the validator in **DepositContract**? Is it possible that a deposit transaction is frontrunned by a malicious validator to set immutable `WithdrawCredentials`? 
- [ ] The derivatives must be collateralized by 100% ETH. However, the amount of ETH may decrease due to slashing. Does the implementation of the derivative token support burning? Is a depeg between the *Derivative* and *UnderlyingToken* possible?
- [ ]TVL (Total Value Locked) of LSD protocols is the largest among all types of DeFi systems. Centralization of key functions leads to great risk. An ideal LSD protocol should be fully decentralized (e.g., support multisig).Who can add validator data to the registry? Who can mint or burn derivatives? Who can add roles for addresses? Is it a single address or a multisig?
- [ ] How often is `DepositContract.deposit()` called? Who initiates the function call? If a significant amount of ETH has accumulated in the protocol contract balance and `DepositContract.deposit()` is called, how many iterations of the call can be executed? How much gas will these iterations require? At what amount of ETH will an out-of-gas issue occur on the contract?
- [ ] Are there operations in contracts where an array with data from all validators is iterated? How much gas does each iteration require? Is an out-of-gas issue possible with a large number of array elements (i.e., a large number of validators)?
[Example](https://github.com/code-423n4/2022-09-frax/blob/55ea6b1ef3857a277e2f47d42029bc0f3d6f9173/src/OperatorRegistry.sol#L113-L118)
- [ ] Is there a possibility of exploiting an inflation (first deposit) attack? How are empty pools created?  When creating an empty pool, are underlying tokens deposited? Is it possible to manipulate the price of shares immediately after creating a pool? A good solution to avoid an inflationary attack is to create a user with a non-zero balance when creating a pool.
- [ ] Is slashing of the operator's balance possible in the protocol? What happens if the amount of the slashing penalty is greater than the operator's balance? Can the operator withdraw the balance of collateral tokens while continuing to validate blocks?
- [ ] Special attention should be paid to the functions of withdrawing staked ETH. Is a reentrancy attack possible during withdrawal?
- [ ] Is a reentrancy attack possible in the user's rewards distribution function?
- [ ] Does the accounting of user rewards work correctly? If the protocol calculates the user's reward based on their share of the pool, are there any issues with the calculation of the share?
- [ ] Who determines the price of the derivative token? If the price is determined by oracles, is it possible to manipulate the price via the sandwich attack when a price update transaction is sent?
