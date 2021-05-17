# xdai-kleros-liquid
Kleros Courts on xDai

# Testing PNK on Sokol-Kovan

MetaMask setup for Sokol: 
- Network Name: POA Sokol Testnet
- New RPC URL: https://sokol.poa.network
- ChainID: 0x4d (77)
- Symbol: SPOA
- Block Explorer URL: https://blockscout.com/poa/sokol

In order to execute transactions on Sokol, you will need the native currency SPOA. Request some in the Sokol [faucet](https://faucet-sokol.herokuapp.com/).

## Deployed Addresses

Sokol:
- Wrapped PNK: `0x9Bc02E9f5291adbb1f0Cf5C15ab1B400dccd3665`
- xKlerosLiquid: `0xb701ff19fBD9702DD7Ca099Ee7D0D42a2612baB5`
- xKlerosLiquidExtraViews: `0x1Aab57DD59afc98bff1D939b34974E9c8fc705fF`
- PolicyRegistry: `0x0Bee63bC7220d0Bacd8A3c9d6B6511126CDfe58f`

xDai:
- Wrapped PNK: `<none>`
- xKlerosLiquid: `<none>`
- xKlerosLiquidExtraViews: `<none>`
- PolicyRegistry: `<none>`

## Stake
1. Get Kovan PNK from [faucet](https://kovan.etherscan.io/address/0x4e95b2e0ecb3bd394e1dddd775504820a746d3bd#writeContract). The Kovan PNK contract address is `0x1ee318dbc19267dbce08f54a66ab198f73ede356`.
1. [approve](https://kovan.etherscan.io/address/0x1ee318dbc19267dbce08f54a66ab198f73ede356#writeContract) the amount of PNK that you want to stake to the foreign token bridge contract (`0xa960d095470f7509955d5402e36d9db984b5c8e2`).
1. [relayTokensAndCall](https://kovan.etherscan.io/address/0xa960d095470f7509955d5402e36d9db984b5c8e2#writeProxyContract) in order to bridge the tokens to Sokol. Use token: `0x1ee318dbc19267dbce08f54a66ab198f73ede356`, _receiver: `0x9Bc02E9f5291adbb1f0Cf5C15ab1B400dccd3665`, _value: amount of PNK to bridge, _data: the address that will receive the tokens on Sokol padded to 32 bytes.
1. Now, you are ready to [stake](https://blockscout.com/poa/sokol/address/0xb701ff19fBD9702DD7Ca099Ee7D0D42a2612baB5/write-proxy) in the Sokol general court by calling `setStake`.

## Stake (using Omnibridge)
1. Get Kovan PNK from [faucet](https://kovan.etherscan.io/address/0x4e95b2e0ecb3bd394e1dddd775504820a746d3bd#writeContract). The Kovan PNK contract address is `0x1ee318dbc19267dbce08f54a66ab198f73ede356`.
1. Go to the [token bridge](https://sokol-omnibridge.web.app/bridge) and convert Kovan PNK into Sokol PNK. The Sokol PNK contract address is `0x184A7Fc4fa965D18Af84C6d97dfed8C4561ff8c2`.
1. Sokol PNK are not directly used for staking. Instead, the token is wrapped in order to let xKlerosLiquid (the courts) be the controller of the PNK contract. To convert Sokol PNK into Sokol Wrapped PNK, first [approve](https://blockscout.com/poa/sokol/address/0x184A7Fc4fa965D18Af84C6d97dfed8C4561ff8c2/write-proxy) the address `0x9Bc02E9f5291adbb1f0Cf5C15ab1B400dccd3665` the amount you want to convert and then call [deposit](https://blockscout.com/poa/sokol/address/0x9Bc02E9f5291adbb1f0Cf5C15ab1B400dccd3665/write-proxy).
1. Now, you are ready to [stake](https://blockscout.com/poa/sokol/address/0xb701ff19fBD9702DD7Ca099Ee7D0D42a2612baB5/write-proxy) in the Sokol general court by calling `setStake`.

## Unstake
1. Unstake from the Sokol general court. 
1. You can use `withdrawAndConvertToPNK()` [method](https://blockscout.com/poa/sokol/address/0x9Bc02E9f5291adbb1f0Cf5C15ab1B400dccd3665/write-contract) to directly convert the Sokol Wrapped PNK into Kovan PNK. If you use this method, it is possible to set the receiver. On the Kovan chain, the tokens have to be claimed though. Go to the [token bridge](https://sokol-omnibridge.web.app/bridge), connect MetaMask with your Kovan account and claim your tokens.
1. If you rather use the token bridge, call `withdraw()` [method](https://blockscout.com/poa/sokol/address/0x9Bc02E9f5291adbb1f0Cf5C15ab1B400dccd3665/write-contract) instead. Then go to the [token bridge](https://sokol-omnibridge.web.app/bridge) convert your tokens.
