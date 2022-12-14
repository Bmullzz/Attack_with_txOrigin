# Performing an Attack using tx.origin

## tx.origin vs msg.sender
### tx.origin
* `tx.origin` is a global variable which returns the address that created the original transaction.
* It is similar to `msg.sender` but with some key differences.
* Incorrect use of `tx.origin` can lead to security vulnerabilities in smart contracts.

### msg.sender
* `msg.sender` returns the immediate account that called the function.
* It can be an external account or another contract calling said function.
* For example, if `User` calls `Contract A`, which then calls `Contract B` within the same transaction, `msg.sender` will be equal to `Contract A` when checked from within `Contract B`. 
* `tx.origin` will be the `User` regardless of where you check it from.

## DOS (Denial Of Service) Attack on a Smart Contract
### What will happen?
* There will be 2 smart contracts - `Good.sol` and `Attack.sol`.  Initially, the owner of `Good.sol` will be a good user.  Using the attack function, `Attack.sol` will be able to change the owner of `Good.sol` to itself, thus denying the good user control of `Good.sol`.

### The Attack
* Refer to `test/attack.js` for the following explanation of the attack.
* Initially, `addr1` will deploy `Good.sol` and will be the owner.
* The attacker will somehow fool the user that controls the private keys of `addr1` to call the `attack()` function within `Attack.sol`.
* When the user calls `attack()` with `addr1`, `tx.origin` is set to `addr1`.
* `attack()` further calls `setOwner()` from `Good.sol` which checks to see if `tx.origin` is indeed the owner. This is `true` because the original transaction was called by `addr1`.
* After verifying the owner, it sets the owner to `Attack.sol`.

## Running the Code
* Run the test locally using `npx hardhat test`.

## Real Life Example
* THORChain users lost millions in $RUNE due to an attacker getting approvals using this attack vector.
* The attacker sent a fake token to user's wallets, then approved that token for sale on Uniswap which would then transfer $RUNE from the user's wallet to the attacker's wallet.
* This was possible because THORChain used `tx.origin` for transfer checks instead of `msg.sender`.
* Link to article about [THORChain Hack #2](https://rekt.news/thorchain-rekt2/)

## Prevention
* This type of attack can be stopped by using `msg.sender` instead of `tx.origin`.
* Example code:
```
function setOwner(address newOwner) public {
    require(msg.sender == owner, "Not owner" );
    owner = newOwner;
}
```