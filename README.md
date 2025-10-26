# TokenVault - RWA Token Vault for Chateau Capital
## Overview
TokenVault is a robust and secure smart contract designed to handle deposits and redemptions of Real World Asset (RWA)-backed tokens. Built with modularity and security in mind, it ensures smooth user interactions while providing administrators with essential control features. This contract was developed for Chateau Capital to efficiently manage tokenized real-world assets.
## Key Features
### Secure Deposit and Redemption Requests
- Users can initiate deposits and redemptions with clear request tracking.
- Each request is associated with a unique request ID for easy auditing.
### Role-Based Access Control
- Admin controls for sensitive functions like pausing the contract, setting fees, and managing deposit addresses.
- Prevents unauthorized access to critical operations.
### Emergency Pause Mechanism
- Pause/unpause functionalities ensure contract activities can be halted in emergencies, preventing potential exploits or misbehavior.
### Dynamic Fee Structure
- Adjustable fee system with precision increments of 0.1% (e.g., a value of 25 equals a 2.5% fee).
- Fees can be updated to adapt to changing operational needs.
### Efficient Asset Flow and Transfers
- Deposited tokens are securely transferred to a designated treasury/deposit address after processing.
- Redemptions are automatically handled with adequate balance checks.
### User-Friendly Request Management
- Claimable and pending request checks to provide transparency.
- Users can easily view the status of their deposit or redemption requests.
### Request Cancellation
- Both deposit and redemption requests can be canceled by the user before completion, adding flexibility.
## Technical Highlights
- **Standards Compliance:**
  Implements the [IERC7540](https://eips.ethereum.org/) and [ERC4626](https://eips.ethereum.org/EIPS/eip-4626) standards for vault management.
- **Secure Token Transfers:**
  Utilizes SafeERC20 from OpenZeppelin to prevent common ERC20 token transfer issues.
- **Precision Fee Calculations:**
  Prevents rounding errors with careful calculations ensuring accurate fee deductions.
- **Non-U.S. Access Restriction:**
  In compliance with specific regulatory requirements, the NotAmerica modifier restricts access from U.S. participants.
- **Efficient ID Generation:**
  Optimized deposit and redemption request ID generation ensures uniqueness and prevents collisions.
- **Full Event Logging:**
  Key user actions (deposits, redemptions, cancellations) emit events for transparent off-chain tracking and auditing.
## Contract Functions
### User Functions
- `requestDeposit(assets, receiver)`: Initiates a deposit request.
- `requestRedeem(shares, receiver)`: Initiates a redemption request.
- `cancelDeposit(requestId)`: Cancels a pending deposit.
- `cancelRedeem(requestId)`: Cancels a pending redemption.
- `claimableDepositRequest(requestId)`: Checks claimable deposit assets.
- `claimableRedeemRequest(requestId)`: Checks claimable redemption shares.
### Admin Functions (Requires DEFAULT_ADMIN_ROLE)
- `deposit(requestId, receiver)`: Processes pending deposit requests.
- `redeem(requestId)`: Processes pending redemption requests.
- `pause() / unpause()`: Pauses or resumes contract activity.
- `setFee(_fee)`: Updates transaction fee percentage.
- `setDepositAddress(_depositAddress)`: Changes the target address for asset deposits.
## Events
- `DepositRequest`: Emitted when a deposit request is created.
- `RedeemRequest`: Emitted when a redemption request is initiated.
- `DepositClaimable`: Emitted when a deposit is processed and claimable.
- `RedeemClaimable`: Emitted when a redemption is completed.
- `DepositCancelled`: Emitted when a deposit request is canceled.
- `RedeemCancelled`: Emitted when a redemption request is canceled.
## Fee Structure Example
| Fee Value | Percentage Deducted |
|-----------|---------------------|
| 1 | 0.1% |
| 10 | 1% |
| 25 | 2.5% |
| 100 | 10% |
| 1000 | 100% (max limit) |
The `getPercentage(value)` function calculates the fee based on the set value.
## Usage Flow
- User initiates a deposit:
  Calls `requestDeposit`, funds are moved to the contract, and a request ID is generated.
- Admin processes the deposit:
  Calls `deposit` with the request ID.
  Shares are issued to the user minus fees, and funds are transferred to the treasury address.
- User initiates a redemption:
  Calls `requestRedeem` to convert shares back into assets.
- Admin processes the redemption:
  Calls `redeem`, transferring assets back to the user.
## Security Considerations
- Always use reputable wallet providers when interacting with the contract.
- Deposits and redemptions require admin approval for processing, ensuring oversight.
- The contract includes pausable functionality for handling emergencies.
## Dependencies
- OpenZeppelin Contracts (for security and standardization)
- Custom modules:
  - `NotAmerica`: Regional compliance.
  - `SimpleVault`: ERC4626 tokenized vault logic.
Disclaimer: This contract is provided as-is without warranty. Use at your own risk. Always test thoroughly before deploying in production environments.
