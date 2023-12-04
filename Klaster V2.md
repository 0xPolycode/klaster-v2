# Klaster - Complete Network Abstraction Framework for EVM Blockchains

In 2023., interacting with blockchain can be very confusing. There is a growing number of L1s & rollups and interacting with assets requires constant network switching, bridging of assets & 
worrying about which chain we're using.

Blockchain architecture which achieves mass adoption needs to be completely abstracted away. There are no half measures here - as long as the user knows which blockchain they're interacting with,
or even a step further - as long as the user knows they're interacting with a blockchain _at all_ - blockchain apps will have horrible UX.

At Klaster, we're building a Solidity + TypeScript SDK, which enables any developer to easily build a completely network abstracted dApp, without having any in-depth knowledge of cross-chain
technology.

Currently in production is _Klaster V1_ - V1 of the protocol uses Chainlink CCIP to relay messages across blockchains, while enabling developers to

* Give their smart contracts accounts on remote blockchains - enabling them to send / receive assets on networks on which they're not deployed to
* Easily encode transactions into cross-chain transactions, through our developer-friendly TypeScript SDK.

Klaster V1 is already a big step forward in terms of blockchain usability, as demonstrated by our [Klaster Safe extension](https://safe.klaster.io)

<img width="757" alt="Screenshot 2023-12-04 at 15 22 53" src="https://github.com/0xPolycode/klaster-v2/assets/129866940/f7ff28e1-1a1e-4dfb-ade6-920913386043">

This extension uses Klaster technology to enable _any_ Safe, whether new or already deployed - to create _cross-chain subaccounts_ - blockchain addresses controlled by the Master Safe &
available, with the same address, on multiple blockchain networks. This enables a Safe deployed on e.g. Ethereum to receive & send assets (& execute arbitrary transactions) on many other 
EVM networks (notably Polygon, Optimism, Arbitrum, BASE, Avalanche, BNB).

In the same way that Klaster enables this Safe to have a cross-chain sub-account, the Klaster protocol enables this for _any_ smart contract. Examples of use cases include:

* A DAO controlling assets on multiple blockchains
* Running a cross-chain token sale or crowdfunding campaign, with a single multisig or crowdfunding contract controlling the entire logic
* Deploying smart contracts accross mutliple blockchains through a single "deployer" multisig

Klaster makes writing cross-chain apps effortless. 

## Examples

To demonstrate just how easy it is to make cross-chain apps through Klaster, here are a few examples:

Solidity example
```solidity
contract MyContract {
    address public klaster = '0xKLASTER_SINGLETON_ADDRESS"

    execute() {
        var gateway = KlasterGatewaySingleton(this.klaster)
        gateway.execute(...)
    }
}
```

TypeScript Example
```ts
import { KlasterSDK } from 'klaster-sdk'

const provider = new ethers.providers.Web3Provider(window.ethereum)
const destinationChain = KlasterSDK.Chains.OP_Mainnet

const crossChainTx = KlasterSDK.encode({
   value: ethers.utils.parseUnit(1, 'ether')
      from: '0xMyAccount'
      to: '0xReceiver'
   },
   destinationChain
)

await provider.send('eth_sendTransaction', crossChainTx)
```

