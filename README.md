# staking_program: stake SOL, SPL tokens and NFTs on Solana

## Description
This project implements a staking program with the Anchor framework on Solana. Users can stake native SOL, NFTs and SPL tokens to earn rewards. The program handles configuration, staking and unstaking, and pays out rewards as a reward token when a position is unstaked.

## NFT freezing
When an NFT is staked, it stays in the user's wallet but is frozen, so it can't be transferred or modified until the user unstakes it.

### Freezing and thawing NFTs
- **Stake NFT**: the program delegates the NFT to itself and freezes it (`FreezeDelegatedAccount`).
- **Unstake NFT**: the program thaws the NFT (`ThawDelegatedAccount`) and revokes the delegation.

## SOL staking
When SOL is staked, it is transferred to a stake account PDA and locked there until the user unstakes it.

### Staking and unstaking SOL
- **Stake SOL**: a System Program `transfer` moves the SOL into the stake account PDA.
- **Unstake SOL**: the stake account is closed and the SOL is returned to the user.

## SPL token staking
When SPL tokens are staked, they are transferred to an associated token account owned by the stake account PDA and locked there until the user unstakes them.

### Staking and unstaking SPL tokens
- **Stake SPL tokens**: `transfer_checked` moves the tokens into the stake account PDA's token account.
- **Unstake SPL tokens**: `transfer_checked` returns the tokens to the user and the token account is closed.

## Rewards
- Each asset type earns points at its own rate (`points_per_nft_stake`, `points_per_sol_stake`, `points_per_spl_stake`), set in the config.
- **Flexible staking**: you can unstake at any time and earn the base rate for the time staked.
- **Locked staking**: you choose a lock period up front and earn extra yield for it. The lock period must be at least `min_freeze_period` (the tests use 60 seconds).
- Rewards are minted to the user as the reward token when they unstake.

## Installation

To install the dependencies, run:

```bash
yarn install
```

or

```bash
npm install
```

## Usage

Build the program:

```bash
anchor build
```

Run the tests:

```bash
anchor test
```

Deploy:

```bash
anchor deploy
```
