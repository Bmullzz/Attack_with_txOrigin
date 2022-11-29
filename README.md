# Performing an Attack using tx.origin

## tx.origin
* `tx.origin` is a global variable which returns the address that created the original transaction.
* It is similar to `msg.sender` but with some key differences.
* Incorrect use of `tx.origin` can lead to security vulnerabilities in smart contracts.

### msg.sender
* `msg.sender` returns the immediate account that called the function.
* It can be an external account or another contract calling said function.
* For example, if `User` calls `Contract A`, which then calls `Contract B` within the same transaction, `msg.sender` will be equal to `Contract A` when checked from within `Contract B`.  `tx.origin` will be the `User` regardless of where you check it from.

Try running some of the following tasks:

```shell
npx hardhat help
npx hardhat test
REPORT_GAS=true npx hardhat test
npx hardhat node
npx hardhat run scripts/deploy.js
```
