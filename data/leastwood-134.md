# Lines of code

https://github.com/code-423n4/2022-03-joyn/blob/c9297ccd925ebb2c44dbc6eaa3effd8db5d2368a/core-contracts/contracts/CoreCollection.sol#L162

https://github.com/code-423n4/2022-03-joyn/blob/c9297ccd925ebb2c44dbc6eaa3effd8db5d2368a/core-contracts/contracts/ERC721Payable.sol#L50-L56

https://github.com/code-423n4/2022-03-joyn/blob/c9297ccd925ebb2c44dbc6eaa3effd8db5d2368a/royalty-vault/contracts/RoyaltyVault.sol#L43-L57

https://github.com/code-423n4/2022-03-joyn/blob/c9297ccd925ebb2c44dbc6eaa3effd8db5d2368a/splits/contracts/Splitter.sol#L231-L240

# Vulnerability details

## Impact

The `transfer()` function is used to send royalty assets to the splitter contract and its recipients. If the vault operates on non-standard ERC20 tokens, its possible for transfers to not revert upon failure. Similarly, `transferFrom()` is used to pull funds from the user when minting an NFT in `CoreCollection.mintToken()`. The `ERC721Payable._handlePayment()` function does not check the return value of `payableToken.transferFrom()`. As a result, if this call does not revert and instead returns false upon a failed transfer, the `mintToken()` function will continue to execute successfully. Therefore, users may be able to mint NFTs for free.

## Recommended Mitigation Steps

Consider integrating `SafeERC20` in all areas of Joyn's code to ensure return values for `transfer()` and `transferFrom()` calls are handled correctly.