# Collateralized Debt Position (CDP) Checklist
## General


- [ ] How are valditaor's data entered into the register of validators? By who how and when are *WithdrawCredentials* setting for the 
validator in **DepositContract**? Is it possible that a deposit transaction is frontrunned by a malicious validator to install immutable 
*WithdrawCredentials*? 
- [ ] The derivatives must be collatorized by 100% ETH. But the amount of ETH may decrease due to slashing. Does the implementation of 
direvative token the support burning? Is depeg *Direvative/UnderlyingToken* possible?
- [ ] TVL of LSD protocols are the largest among all types of DeFi systems. Centralization of key functions leads to great risk. Ideal LSD 
protocol should be fully decentralized (e.g. support multisig). Who can add validator data to the registry? Who can mint or burn 
direvatives? Who can add roles for addresses? Its one address or multisig?
- [ ] How often is `DepositContract.deposit()` called? Who initiates the function call? If a lot of ETH has accumulated on the protocol 
contract balance and `DepositContract.deposit()` is called, then how many iterations of the call can executed? How much gas will these 
iterations require? What amount of ETH will an out of gas issue occur on the contract?
- [ ] Are there operations in contracts where an array with data from all validators is iterated? How much gas does each iteration require? 
Is out of gas issue possible with a large number of array elements (a large number of validators)? 
[Example](https://github.com/code-423n4/2022-09-frax/blob/55ea6b1ef3857a277e2f47d42029bc0f3d6f9173/src/OperatorRegistry.sol#L113-L118)
- [ ] Is there a possibility of exploiting an inflation attack? How are empty pools created? When creating an empty pool are underlying 
tokens deposited? Is it possible to manipulate the price of shares immediately after creating a pool? A good solution to avoid an 
inflationary attack is to create a user with a non-zero balance when creating a pool
- [ ] Is slashing of the operator's balance possible in the protocol? What happens if the amount of slashing penalty is greater than the 
amount of the operator? Can the operator withdraw the balance of collateral tokens while continuing to validate blocks?
- [ ] Special attention should be paid to the functions of the withdrawal staked ETH. Is a reentrancy attack possible during withdrawal?
- [ ] Is a reentrancy attack possible in the users rewards distribution function?
- [ ] Does the accounting of user rewards work correctly? If the protocol calculates the user's reward based on share of the pool, are 
there any issues with the calculation of the share?
- [ ] Who determines the price of the derivative token? If the price is determined by oracles and a publishing transaction of this price is 
sent, is it possible to exploit the sandwich attack?

