# FlareHaus Protocol

A decentralized trading guilds platform where users pool their liquidity together and securely agree on how to utilize it for yield generation through collective decision-making and risk-managed trade proposals.

---

## Overview

FlareHaus enables the creation and management of trading guilds—groups of users who share liquidity in a pooled environment. Guild members can propose trades, vote to approve proposals, and execute approved trades. Profits and losses from trades are collectively distributed or deducted based on member stakes. A portion of profits can be locked in a vesting contract for future distribution, ensuring long-term incentives.

---

## Protocol Components

### Guilds

A **Guild** represents a collective of users pooling funds to participate in trading activities.

- **Owner:** The creator of the guild, controls initial parameters.
- **Name & Description:** Metadata for guild identification.
- **Entry Threshold:** Minimum stake required to join.
- **Member Cap:** Maximum number of members allowed (optional).
- **Members:** Lists of member names, addresses, and their respective stakes.
- **Pool:** Total liquidity pooled by members.
- **Risk Threshold:** Defines max risk as a percentage of the guild’s pooled funds allowed per trade.

---

### Trade Proposals

Guild members can propose trades specifying:

- **Amount:** The stake to be used for the trade, subject to the guild's risk threshold.
- **Description:** Details about the trade.
- **Voting:** Members vote Yes/No on proposals. Approval requires a majority vote or unanimous consent for two-member guilds.
- **Execution:** Only the proposing trader can execute an approved trade, receiving the proposed amount from the pool.

---

### Profit and Loss Distribution

After trade execution:

- The trader returns funds with profit or loss.
- Profits are split: 
  - 40% goes to a vesting contract for long-term member rewards.
  - 40% of the remnant 60% from overall profit goes as a trader’s share.
  - Remaining profits are distributed to guild members proportional to stakes.
- Losses reduce member stakes and the pool accordingly, with 40% attributed to the trader's personal loss.

---

### Vesting Contract (FlareVesting)

The vesting contract manages long-term distribution of profits locked from trade returns.

- **Vesting Schedules:** Created per member with total claimable amounts, start time, and cliff duration.
- **Vesting Duration:** Up to 1 year (31536000 seconds), with weekly vesting increments.
- **Claiming:** Members claim vested tokens periodically after the cliff.
- **Cancellation:** Members can cancel vesting schedules, triggering fund return back to the guild pool.
- **Cycle Reset:** The vesting contract resets all schedules after a yearly cycle.

---

## Key Functions

### FlareHaus (Guild Management)

- `createGuild`: Initialize a guild with parameters and required initial stake.
- `joinGuild`: Join an existing guild by meeting entry threshold.
- `topUpStake`: Increase a member’s stake.
- `proposeTrade`: Submit a trade proposal within risk limits.
- `voteProposal`: Vote on active trade proposals.
- `executeProposal`: Execute an approved trade proposal.
- `returnTradeFunds`: Return trade funds after execution, handle profit/loss distribution and vesting calls.
- `vestingCancelled`: Handles vesting cancellation refunds.
- `GuildIds`: Returns list of all guild IDs.
- `GuildData`: Returns guild data and trade proposals for a given guild.

### FlareVesting (Profit Vesting)

- `Vesting`: Creates vesting schedules for guild members based on distributed funds.
- `vestedAmount`: Calculates vested amount available to claim at current time.
- `claim`: Allows members to claim vested funds.
- `cancelSchedule`: Cancels vesting schedules, returning unclaimed funds to guild contract.
- `resetCycle`: Resets vesting schedules annually.
- Rejects direct native token transfers to prevent accidental fund loss.

---

## Usage Flow

1. **Create Guild**: A user creates a guild, pays initial stake, and sets parameters including risk and member caps.
2. **Join Guild**: Users join by depositing funds meeting the entry threshold.
3. **Propose & Vote**: Members propose trades and vote on execution.
4. **Execute Trade**: The approved trade is executed; funds are sent to the trader.
5. **Return Funds**: Trader returns funds with profit/loss; profits partly vested and rewards distributed.
6. **Claim Vested Rewards**: Members claim vested profits gradually over a defined period.
7. **Cancel Vesting (Optional)**: Members can cancel vesting schedules to retrieve unclaimed funds back to the guild.

---

## Security & Risk Controls

- **Entry Thresholds:** Prevents underfunded members.
- **Member Cap:** Limits guild size to manage risk.
- **Risk Threshold:** Caps trade amount relative to pooled liquidity.
- **Votes Required:** Majority vote to approve trades, ensuring democratic control.
- **Vesting:** Encourages long-term guild participation and limits immediate profit withdrawal.
