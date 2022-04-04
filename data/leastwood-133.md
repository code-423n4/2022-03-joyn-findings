# Lines of code

https://github.com/code-423n4/2022-03-joyn/blob/main/royalty-vault/contracts/RoyaltyVault.sol#L31-L61
https://github.com/code-423n4/2022-03-joyn/blob/main/splits/contracts/Splitter.sol#L149-L169

# Vulnerability details

## Impact

The `RoyaltyVault.sol` contract interacts with the `Splitter.sol` to send accumulated royalties to the collection's respective recipients. The `sendToSplitter()` function will query the balance of the royalty asset and send the amount after fee deductions to the splitter contract. However, the recipient of these funds expects the entire amount as `Splitter.incrementWindow()` is called on this amount. If fee-on-tokens or any rebasing token is used here, then the splitter contract will receive less than the sent amount. As a result, some users will not be able to claim their entire proportion of the royalties.

## Recommended Mitigation Steps

Consider taking a snapshot of the token balance before and after the transfer and treat the difference between these two amounts as the received token amount. This will be compatible with all types of tokens and avoid any issues where users are unable to withdraw their entire deposited amount.