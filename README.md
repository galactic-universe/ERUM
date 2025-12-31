```markdown
# ERUM Token (ERC-20) — Project

Overview
- ERUM: ERC-20 token with AccessControl MINTER_ROLE and Bitcoin-like issuance schedule (initial reward, halving interval, cap).
- No burn functionality.
- TeamVestingFactory to create OpenZeppelin VestingWallets.
- Hardhat project with tests and Etherscan verification support.

Files included
- contracts/ERUM.sol
- contracts/TeamVestingFactory.sol
- scripts/deploy.js
- hardhat.config.js
- package.json
- test/ERUM.test.js
- test/Vesting.test.js
- .env.example

Quick setup
1. Install:
   - npm install

2. Create a .env file (copy .env.example) and set:
   - DEPLOYER_PRIVATE_KEY="0x..."
   - ALCHEMY_API_KEY or INFURA_API_KEY (or POLYGON_RPC)
   - ETHERSCAN_API_KEY
   - MULTISIG_ADDRESS (optional — deploy script will grant MINTER_ROLE to this address if present)
   - REVOKE_DEPLOYER_AFTER_GRANT=true (optional) — if true, deployer will have MINTER_ROLE revoked after granting to MULTISIG_ADDRESS

3. Compile:
   - npx hardhat compile

4. Run tests:
   - npx hardhat test

5. Deploy (example to Goerli / Sepolia / other configured network):
   - npx hardhat run --network <network> scripts/deploy.js

6. Verify contracts (after deploy):
   - npx hardhat verify --network <network> <CONTRACT_ADDRESS> "<constructorArg1>" "<constructorArg2>" ...
   - Example for ERUM:
     npx hardhat verify --network goerli <ERUM_ADDRESS> "Erum Token" "ERUM" 50000000000000000000 210000 21000000000000000000000000

Notes
- The token does not mint automatically — MINTER_ROLE accounts must call `mintTo(address)` periodically to mint rewards from (lastMintedBlock+1) to current block. Recommended: make MINTER_ROLE a multisig and have a keeper service call mintTo to fund treasury/recipients.
- The halving is per block number (Ethereum block cadence, not Bitcoin wall-clock cadence).
- The minting logic enforces the cap and uses epoch iteration (per-halving epoch) to compute rewards efficiently.
- In production, use a multisig (Gnosis Safe) for MINTER_ROLE/DEFAULT_ADMIN_ROLE and consider timelock/governance for sensitive actions.

If you want:
- a script to automatically verify deployed contracts on Etherscan
- automatic role-granting + deployer-role-revocation workflow
- a keeper contract example to automate mintTo
tell me which and I'll add it.
```