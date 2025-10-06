# **CryptoCyte (CD355)**
### Verified Smart Contract Repository

**Contract Type:** ERC-20 Upgradeable Token  
**Network Compatibility:** Ethereum / EVM-compatible networks  
**Compiler Version:** Solidity ^0.8.20  
**License:** MIT  
**Security Contact:** [security@Bolton-Strid.org](mailto:security@Bolton-Strid.org)

---

## **Overview**

**CryptoCyte (CD355)** is an upgradeable ERC-20 token designed for healthcare and compliance-based ecosystems.  
It integrates dual-wallet authorization, guardian-based freeze mechanisms, and strict whitelist and pausing controls.  
The architecture ensures transparency, immutability, and verifiable governance for institutional and clinical digital environments.

---

## **Key Features**

- Dual-wallet authorization (primary + guardian)  
- Guardian-controlled freeze/unfreeze of balances  
- Global and wallet-level pause capability  
- Smart contract whitelisting  
- Role-based governance framework  
- Controlled minting and burning  
- UUPS upgradeable architecture  
- Reentrancy-protected transaction logic  
- Includes EIP-2612 `permit()` structure (nonces inactive until `permit()` is implemented)

---

## **Technical Summary**

| Parameter | Description |
|------------|-------------|
| **Name** | CryptoCyte |
| **Symbol** | CD355 |
| **Decimals** | 6 |
| **Standard** | ERC-20 (Upgradeable) |
| **Upgrade Mechanism** | UUPS (Authorized via `UPGRADER_ROLE`) |
| **Deployer Role** | `DEFAULT_ADMIN_ROLE` |
| **Security Layers** | ReentrancyGuard, Role-based access, Whitelist enforcement |
| **Audits Performed** | HashLock • TokenSniffer • Honeypot.is |

---

## **Governance Model**

CryptoCyte employs a structured, multi-role governance framework:  

- `DEFAULT_ADMIN_ROLE` governs access and role assignment.  
- `MINTER_ROLE` controls issuance.  
- `PAUSER_ROLE` handles global and wallet-level suspension.  
- `UPGRADER_ROLE` authorizes UUPS-based contract upgrades.  

All role assignments and administrative actions are recorded on-chain for complete auditability.

---

## **Transparency and Verification**

Type	Tool	Verification Link
Audit	HashLock	View Report
Audit	TokenSniffer	View Report
Audit	Honeypot.is	View Report
Immutable Docs	OpenTimeStamps	Whitepaper • Lite Paper

All project documents are immutably timestamped for independent verification.

---

## **Legal and Compliance**

CryptoCyte (CD355) is an ERC20 token designed to functiona as a virtual currency.

No warranties are provided regarding liquidity, market performance, or speculative value.

Use of this codebase implies agreement to the Hold Harmless and Indemnification Statement of the Bolton-Strid Foundation.

All external audit tools (HashLock, TokenSniffer, Honeypot.is) are independent third-party services; their findings are advisory and not legally binding.

The Foundation assumes no liability for interpretations of third-party results.

---

## **Repository Structure**

/contracts
    └── CryptoCyte.sol
/docs
    ├── Whitepaper.pdf
    ├── Governance_Policy.pdf
    ├── Roadmap.pdf
/README.md

---

## **Contact**

Entity: Bolton-Strid Foundation
Email: info@Bolton-Strid.org
Website: https://bolton-strid.org
Organization Type: Nonprofit Healthcare and Technology Foundation

© 2025 Bolton-Strid Foundation. All rights reserved.

---

## **Solidity API and Function Overview**

```solidity
// Initialization
function initialize(address initialOwner)
    - Assigns DEFAULT_ADMIN_ROLE, MINTER_ROLE, PAUSER_ROLE, and UPGRADER_ROLE to the deploying address. Must be called once post-deployment.

// Decimals
function decimals() public pure override returns (uint8)
    - Sets precision to 6 decimals.

// Whitelist Management
function whitelistWallets(address primary, address guardian) external onlyRole(DEFAULT_ADMIN_ROLE)
    - Registers a primary and guardian wallet pair.
function getGuardian(address primary) external view returns (address)
    - Returns guardian associated with a primary wallet.
function isWalletWhitelisted(address wallet) external view returns (bool)
    - Returns whitelist status of any wallet.

// Freeze / Unfreeze
function freeze(address primary, uint256 amount) external nonReentrant
function unfreeze(address primary, uint256 amount) external nonReentrant
    - Locks or unlocks wallet balances; only callable by guardian.

// UUPS Upgrade Control
function approveUpgrade() external onlyRole(UPGRADER_ROLE)
function _authorizeUpgrade(address newImplementation) internal override onlyRole(UPGRADER_ROLE)
    - Approves and authorizes upgrade through governance structure.

// Pause Controls
function pause() external onlyRole(PAUSER_ROLE)
function unpause() external onlyRole(PAUSER_ROLE)
    - Globally pause or unpause transfers.

// Minting and Burning
function mint(address to, uint256 amount) external onlyRole(MINTER_ROLE)
function burn(address account, uint256 amount) external onlyRole(DEFAULT_ADMIN_ROLE)
    - Controlled issuance and destruction of tokens.

// Wallet-Level Pause
function pauseWallet(address account) external onlyRole(PAUSER_ROLE)
function unpauseWallet(address account) external onlyRole(PAUSER_ROLE)
function isWalletPaused(address account) external view returns (bool)
    - Pauses or resumes wallet activity independently.

// Smart Contract Whitelist
function addContractToWhitelist(address contractAddress) external onlyRole(DEFAULT_ADMIN_ROLE)
function removeContractFromWhitelist(address contractAddress) external onlyRole(DEFAULT_ADMIN_ROLE)
function isContractWhitelisted(address contractAddress) external view returns (bool)
    - Restricts on-chain interactions to approved contracts only.

// Balance Accessors
function frozenBalance(address account) public view returns (uint256)
function unfrozenBalance(address account) public view returns (uint256)
function spendableBalance(address account) public view returns (uint256)
    - Displays frozen, unfrozen, and available balances for transparency.

// Internal Enforcement
function _update(address from, address to, uint256 amount) internal override
    - Enforces whitelist, pause, and freeze restrictions. Prevents unapproved or noncompliant transfers.

// Utility
function _isContract(address account) internal view returns (bool)
    - Returns true if the address contains bytecode (contract detection).
Security Architecture
Component	Purpose
ReentrancyGuardUpgradeable	Prevents recursive execution exploits
AccessControlUpgradeable	Defines role-based permissions
ERC20PausableUpgradeable	Enables contract-level suspension
UUPSUpgradeable	Provides controlled upgradeability
Permit Framework	Nonce system present but inactive until permit() is implemented



