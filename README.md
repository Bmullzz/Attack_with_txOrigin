# Performing an Attack using tx.origin

## tx.origin
-`tx.origin` is a global variable which returns the address that created the original transaction.
-It is similar to `msg.sender` but with some key differences.
-Incorrect use of `tx.origin` can lead to security vulnerabilities in smart contracts.

### msg.sender
-`msg.sender`

Try running some of the following tasks:

```shell
npx hardhat help
npx hardhat test
REPORT_GAS=true npx hardhat test
npx hardhat node
npx hardhat run scripts/deploy.js
```
