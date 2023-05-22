# Collateralized Debt Position (CDP) Checklist
## General

- [ ] If a user's CDP is closed (e.g., when the user has paid off all the debt), what happens to the vault entry in storage? Is the storage variable erased? Are there checks in the code sections where the variable is used to ensure its existence?
- [ ] Is it possible to have a condition when a user cannot repay their loan?
- [ ] How is the pool value calculated? How is the fee paid by borrowers for the loan distributed? If a portion goes to the lender (depositor) and another portion goes to the pool, is the calculation of the pool value correctly implemented?
- [ ] If ERC4626 is implemented in the protocol, is there a possibility of vulnerability to an inflation (first deposit) attack?

## Collaterals
### General

- [ ] Some tokens have a black list (e.g. USDT). How can this affect the protocol?
- [ ] Some tokens can be `pause()`d. How can this affect the protocol?
- [ ] Does the protocol consider the implementation differences of the `transfer()` function for tokens, such as BNB's restriction on transferring to a zero address or transferring an amount of zero?
- [ ] The transferred token can take a fee for the transfer. How can this affect the protocol?
- [ ] Some tokens may not revert in `transfer()` and `transferFrom()`, does the protocol use SafeERC20?
- [ ] Does the protocol account for the change in account balance caused by rebasing tokens such as aTokens, UBI, AMPL, and similar tokens?

### ERC20 tokens

- [ ] Does the contract consider that tokens in the market can have different decimals? How does this affect the calculation of the collateral value?
- [ ] If ERC777, ERC721, ERC677 or other ERCs with callbacks are used in the market, is a reentrancy attack possible?
- [ ] Is it possible to deposit one stablecoin as collateral in the protocol and withdraw a different stablecoin? Is there a potential for arbitrage in this scenario?
- [ ] How does the protocol calculate the value of the same stablecoin? Does the price calculation take into consideration the possibility of a depeg or deviation from the intended pegged value?

### NFT: liquid ERC721 tokens from the BAYC, MAYC, CryptoPunks, Azuki and other collections

- [ ] If a CryptoPunks collection that does not support the `transferFrom()` function is used as collateral, is it safe to transfer tokens to the vault? When calling the `offerPunkForSaleToAddress()` function, there is a possibility that ownership of the token can be front-runned.
- [ ] If `onERC721Received()` is implemented in ERC721, is re-entrancy possible?

### Uniswap LP positions

- [ ] If the protocol allows the use of LP tokens as collateral, then how does it determine the value of these LP tokens? Red flag - `pool.getReserves()`
- [ ] What type of LP token(0.01%, 0.05%, 0.3%, 1%) is used in the protocol? Do the contracts consider that there are several pairs for the same tokens in Uniswap?

### Earn tokens

- [ ] If tokens that are pegged to any asset (renBTC, HitBTC, aBTC, etc.) are used in the market (for example, as collateral), then how will the protocol behave during the depeg? Will it count the price 1:1?
- [ ] When the protocol integrates earn protocols for earning on collateral, is the mechanics of calculating shares implemented correctly?
- [ ] How is the user's share counted if the collateral is staked somewhere? Is it possible to manipulate the calculation?

## Oracles
### General

- [ ] Can the asking price be zero? Is there any processing of such a case?
- [ ] Are the decimals of the received prices processed correctly?

### Chainlink

- [ ] Is getting prices wrapped in a try/catch? Are there alternative ways to get a price, considering that multi-signature wallets in Chainlink can block access to data feed?

### Uniswap TWAP

- [ ] TWAP is subject to the manipulations, especially if a small time window is used. Is TWAP manipulation possible? How much will it cost to the attacker?

### Virtual Price (amountTokenA/amountTokenB)

- [ ] How do external non-protocol oracle contracts calculate the price? Special attention should be paid to the price of vToken/Token. Red-Flag - `balanceOf()` in the price calculation method trace.

## Markets
### Hybrid markets

- [ ] If one pool is hacked, how will it affect other pools? Does the protocol isolate liquidity?

## Interest rate
### General 

- [ ] When is the interest rate calculated? Before or after closing the vault?

## Liquidations
### General

- [ ] Can the user liquidate their own position with a profit? How can the health ratio change, apart from typical cases such as withdrawal of collateral, taking on additional debt, or a significant drop in the price of collateral?
- [ ] When is the interest rate calculated? Before or after liquidation of the vault?
- [ ] Is there a risk of re-entrancy in the ERC721 withdrawal method before checking for a normal health ratio? If the health ratio is checked after calling safeTransferFrom() with the implementation of onERC721Received(), is there any way to bypass the health ratio check in the callback?

### Auction liquidations

- [ ] If an auction is used during liquidation and the liquidated person has the ability to prematurely complete the auction by proving their solvency, can he or she take a flashloan in the process?
- [ ] Is there enough input data validation in the auction start function? Is it possible to launch an unfinished auction?
- [ ] Is the math correct when only a portion of the collateral from the liquidated vault is put up for auction?
- [ ] If users send tokens to a contract when creating a bid in an auction, what happens if their bid is interrupted? Is the interrupted person's money returned to them? What happens if the creator of the auction prematurely completes it (for example, by repaying the entire debt)? Is the bid returned to the last bidder?
