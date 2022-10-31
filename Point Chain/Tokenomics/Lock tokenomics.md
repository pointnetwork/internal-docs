# Flexible Locking tokenomics

The kind of tokenomics when user incentives depends on the length of the period for which user locks his tokens in staking.

## Required updates for Staking/delegation module
0. Create a new module `DelegationLock`
1. Add lock period length `MIN` & `MAX` genesis constants.
2. Creat `DelegatorLock` structure.
   1. With `LockCoefficient` parameter
   2. With `LockStart` parameter
3. Create storage `mapping` for `DelegatorLock` where key will be `delegator address` & `validator address`.
4. Map `lockCoefficient` with lock period length where `MAX=1` and `MIN=0+`
5. Calculate Shares as: `Shares = delegated tokens * Lock period coefficient`
6. Use Hook to set default `LockCoefficient` and `LockStart` values when delegation is done using standard cosmosSDK API.
7. Use hook for `unbond` call to check if lock period ended for the delegator. Restrict unbonding if lock period not ended for the delegator.
8. Creat `delegateWithLock` function with an ability to set specific Lock period in borders of min & max period length.
9. Add Hook to calculate correct tokens withdraw amount `shares / Lock period coefficient`
10. Check if recalculated shares does not affect anything else in application in an unexpected way.
11. Create a `Cli api` & `RPC` to get `DelegatorLock` values, `staked tokens amount`, `LockEnd`, `LockLength`.
12. Prepare protobuf files and validations
13. Write module tests.
14. Write application level tests
15. Application update module for backward compatibility.
16. Register module in a Point Application.
