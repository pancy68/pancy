Base offers low fees, fast finality and deep integration with Coinbase. Starting this public repo to document my experiments and contributions to the Base Guild builder path.
chore: create initial folder structure for Base experiments

Added folders for contracts/, scripts/ and docs/. Preparing the repo to start writing and deploying simple Solidity contracts on Base Sepolia and Mainnet.
feat: implement Counter contract

Added a simple Counter.sol with increment, decrement and getCount functions. Good starting point for testing deployments on Base.
feat: emit events on Counter increment and decrement

Added Incremented and Decremented events so changes can be easily tracked on Base block explorers.
refactor: clean up Counter contract code

Improved variable naming and added NatSpec comments for better readability. Preparing the contract for public use on Base.
feat: add max count limit to Counter

Added a maximum value check to prevent the counter from exceeding a defined limit. Improves robustness for Base deployments.
feat: allow setting custom step size in Counter

Added the ability to increment or decrement by a custom amount instead of just 1. Makes the contract more flexible on Base.
feat: add historical count tracking to Counter

Stored previous counter values so users can query past states. Useful for analytics and testing on Base.
