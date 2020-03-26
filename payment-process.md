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
  <img src="https://i.imgur.com/t8mUyH9.png" alt="currency-workflow"/>
</p>

The idea is the currency services is doing long pulling to get the latest response (let says every 5 seconds)。

After receiving data from Bank API, currency services will do two different things following CQRs standard, and that is update and display.

Therefore, Command Model stands for receiving data from Bank API and select data from DB simultaneously. Command Model will excutes it to find the best currency rate then update DB.

When DB is update, Query Model will do it's job to display data from DB and publish real-time data to the relevant services.
