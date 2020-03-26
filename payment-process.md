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

### Store && Reward Services

- Our Structure for Store && Reward Servcies

<p align="center">
  <img src="https://i.imgur.com/7A8ZhIr.png" alt="store-reward-services">
</p>

##### What is this all about? Why Event-Driven Architecture?

First, Store Services receive events from previous Services then it store data into its own local database.
By which means, each services store its own data from event-bus where services can store the additional data by adding more fields in its DB. We call that `Availabiility`.

Second, all the datas which database are storing can be called as event-logs. It stores all the incoming logging of all events. Therefore, its also `easy-to-rollback` by using its event-logs. That's assume if you have a bugs happend in Timestamp `X`, you only need to go to that Timestamp `X` and running events one by one for debugging.

Third, if you are going to create new services for new logic to replace old services. You only need to change the event-bus and Event database point to new services.
Which is a very smooth and fast `replacement`

Last, the most import one. `Transaction` guarantee!!!
Event-Driven gives you a transaction guarantee are either at least one or at most one.

## Whole System

<p align="center">
  <img src="https://i.imgur.com/gjKXTas.png" alt="entire-system">
</p>

1. Frist, Client sending request for storing X currency.
2. API Gateway receiving request and pass it to event Bus.
3. `Store Services` receive each requests from event-bus, meanwhile receiving X -> Y currency exchange rate from `currency services` (We assuming it's a long pull every 5 seconds). Then `Store Services` convert X currency to Y currency
4. `Reward Services` receive each requests from event-bus, meawhile receiving Y -> Z currency exchange rate from `currency services` (We assuming it's a long pull every 5 seconds). Then `Reward Services` convert Y currency to Z currency then sending out back to players.
