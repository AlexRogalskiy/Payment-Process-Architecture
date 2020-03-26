# Payment-Process

## Pre-Requirements

- X Currency: Players pay for merchant
- Y Currency: Company receives from Players Payment (X Currency convert to Y Currency)
- Z Currency: Company rewards to Players for buying merchants (Y Currency convert to Z Currency)
- Exchange rate for each transaction base on the rate within 24 hours
- Data store max range is 6 month
- Fault Tolerance within 0% to 1%

## What Could We Go Wrong

- Lack of Payment
- Duplicate of Payment
- Incorrect currency conversion
- Max currency tolerance
- Incorrect payment

### That's Assume

> :bug: Bugs occurs with 0.01%
> :moneybag: Price to fix Bugs cost 15/\$ per each
> :timer_clock: 1M Transaction/day

<p align="center">
  <img src="https://i.imgur.com/H3Mvh5l.png" alt="bugs-report-data"/>
</p>

- The cost for fixing for the bug is accelerating huge difference when transaction increase, Formula: `Cost * Transaction * Percentage of Bugs`

## How Should We Design

### Currency Services

- Our Structure for Currency Services

<p align="center">
  <img src="https://i.imgur.com/dczhgXE.png" alt="currency-services"/>
</p>

- Why and How does the flow works

<p align="center">
  <img src="https://i.imgur.com/JnUbc4T.png" alt="currency-workflow"/>
</p>
