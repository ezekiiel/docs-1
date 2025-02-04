---
order: 4
description: >-
  Now it's time for the fun stuff. Let's configure and get this contract
  up-and-running.
---

# Initialise the Contract

Now we've uploaded the contract, now we need to initialise it.

We're using the Poodle Coin example here - `$POOD` was the first meme coin deployed to a Juno testnet.

{% hint style="info" %}
Choose another name rather than Poodle Coin/POOD, as this is likely already taken on the testnet.
{% endhint %}

## Generate JSON with arguments

To generate the JSON, you can use `jq`, or, if you're more familiar with JS/node, write a hash and encode it using the node CLI.

This example uses the `node` REPL. If you have `node` installed, just type `node` in the terminal and hit enter to access it.

```javascript
> const initHash = {
  name: "Poodle Coin",
  symbol: "POOD",
  decimals: 6,
  initial_balances: [
    { address: "<validator-self-delegate-address>", amount: "12345678000"},
  ]
};
< undefined
> JSON.stringify(initHash);
< '{"name":"Poodle Coin","symbol":"POOD","decimals":6,"initial_balances":[{"address":"<validator-self-delegate-address>","amount":"12345678000"}]}'
```

## Instantiate the contract

Note also that the `--amount` is used to initialise the new account associated with the contract.

In the example below, `6` is the value of `$CODE_ID`.

```bash
junod tx wasm instantiate 6 \
    '{"name":"Poodle Coin","symbol":"POOD","decimals":6,"initial_balances":[{"address":"<validator-self-delegate-address>","amount":"12345678000"}]}' \
    --amount 50000ujuno  --label "Poodlecoin erc20" --from <your-key> --chain-id <chain-id> --gas auto -y
```

If you have set `$CODE_ID` in your shell, you can instead run:

```bash
junod tx wasm instantiate $CODE_ID \
    '{"name":"Poodle Coin","symbol":"POOD","decimals":6,"initial_balances":[{"address":"<validator-self-delegate-address>","amount":"12345678000"}]}' \
    --amount 50000ujuno  --label "Poodlecoin erc20" --from <your-key> --chain-id <chain-id> --gas auto -y
```

If this succeeds, look in the output and get contract address from output e.g `juno1a2b....` or run:

```bash
CONTRACT_ADDR=$(junod query wasm list-contract-by-code $CODE_ID --output json | jq -r '.contracts[0]')
```

This will allow you to query using the value of `$CONTRACT_ADDR`

```bash
junod query wasm contract $CONTRACT_ADDR
```
