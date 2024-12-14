The **ERC Standards** (Ethereum Request for Comments) are technical standards used for building Ethereum-based tokens and smart contracts. Each standard specifies rules and interfaces that developers must follow to ensure compatibility and interoperability with other Ethereum-based systems, such as wallets, exchanges, and decentralized applications (dApps).

Here’s a detailed explanation of the two popular ERC standards:

---

## **1. ERC-20: Fungible Token Standard**

The ERC-20 standard is designed for **fungible tokens**, meaning all tokens are identical in type and value. Examples include cryptocurrencies like USDT, DAI, and many ICO tokens.

### **Key Characteristics**

1. **Fungibility**: Each token is interchangeable with another of the same type.
2. **Divisibility**: Tokens can be divided into smaller units, depending on the `decimals` value.
3. **Standardization**: Ensures compatibility with wallets, dApps, and exchanges.

### **Functions and Events**

ERC-20 defines a set of mandatory functions and events that a token contract must implement:

#### **Mandatory Functions**

1. **`totalSupply()`**  
   Returns the total number of tokens in circulation.

   ```solidity
   function totalSupply() public view returns (uint256);
   ```

2. **`balanceOf(address account)`**  
   Returns the token balance of a given address.

   ```solidity
   function balanceOf(address account) public view returns (uint256);
   ```

3. **`transfer(address recipient, uint256 amount)`**  
   Transfers tokens to another address.

   ```solidity
   function transfer(address recipient, uint256 amount) public returns (bool);
   ```

4. **`approve(address spender, uint256 amount)`**  
   Allows a third-party (spender) to withdraw a specified amount of tokens from the caller’s account.

   ```solidity
   function approve(address spender, uint256 amount) public returns (bool);
   ```

5. **`transferFrom(address sender, address recipient, uint256 amount)`**  
   Moves tokens on behalf of the owner, typically used by dApps.

   ```solidity
   function transferFrom(address sender, address recipient, uint256 amount) public returns (bool);
   ```

6. **`allowance(address owner, address spender)`**  
   Checks how many tokens a spender is allowed to use from the owner's account.
   ```solidity
   function allowance(address owner, address spender) public view returns (uint256);
   ```

#### **Events**

1. **`Transfer(address indexed from, address indexed to, uint256 value)`**  
   Emitted when tokens are transferred.

2. **`Approval(address indexed owner, address indexed spender, uint256 value)`**  
   Emitted when an allowance is set using the `approve` function.

---

### **ERC-20 Example Code**

Here’s a minimal implementation of an ERC-20 token:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ERC20Token {
    string public name = "MyToken";
    string public symbol = "MTK";
    uint8 public decimals = 18;
    uint256 public totalSupply;

    mapping(address => uint256) public balances;
    mapping(address => mapping(address => uint256)) public allowances;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor(uint256 _initialSupply) {
        totalSupply = _initialSupply * 10 ** uint256(decimals);
        balances[msg.sender] = totalSupply;
    }

    function balanceOf(address account) public view returns (uint256) {
        return balances[account];
    }

    function transfer(address recipient, uint256 amount) public returns (bool) {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        balances[msg.sender] -= amount;
        balances[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint256 amount) public returns (bool) {
        allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public returns (bool) {
        require(balances[sender] >= amount, "Insufficient balance");
        require(allowances[sender][msg.sender] >= amount, "Allowance exceeded");
        balances[sender] -= amount;
        balances[recipient] += amount;
        allowances[sender][msg.sender] -= amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) public view returns (uint256) {
        return allowances[owner][spender];
    }
}
```

---

## **2. ERC-721: Non-Fungible Token (NFT) Standard**

The ERC-721 standard is used for **non-fungible tokens (NFTs)**, which are unique and indivisible. Examples include digital art, in-game items, and real estate tokens.

### **Key Characteristics**

1. **Uniqueness**: Each token has a unique `tokenId`.
2. **Indivisibility**: Tokens cannot be split into smaller units.
3. **Ownership**: Each token is owned by a single address at a time.

### **Functions and Events**

ERC-721 defines a set of mandatory and optional functions and events.

#### **Mandatory Functions**

1. **`balanceOf(address owner)`**  
   Returns the number of NFTs owned by an address.

2. **`ownerOf(uint256 tokenId)`**  
   Returns the owner of a specific token.

3. **`safeTransferFrom(address from, address to, uint256 tokenId)`**  
   Transfers ownership of a token, ensuring the recipient can handle NFTs.

4. **`transferFrom(address from, address to, uint256 tokenId)`**  
   Transfers ownership of a token without safety checks.

5. **`approve(address to, uint256 tokenId)`**  
   Approves another address to manage a specific token.

6. **`setApprovalForAll(address operator, bool approved)`**  
   Approves or removes an operator to manage all tokens of the caller.

7. **`getApproved(uint256 tokenId)`**  
   Returns the address approved to manage a specific token.

8. **`isApprovedForAll(address owner, address operator)`**  
   Checks if an operator is approved to manage all tokens of an owner.

#### **Events**

1. **`Transfer(address indexed from, address indexed to, uint256 indexed tokenId)`**  
   Emitted when an NFT is transferred.

2. **`Approval(address indexed owner, address indexed approved, uint256 indexed tokenId)`**  
   Emitted when a token is approved for transfer.

3. **`ApprovalForAll(address indexed owner, address indexed operator, bool approved)`**  
   Emitted when an operator is approved or revoked for all tokens of an owner.

---

### **ERC-721 Example Code**

Here’s a minimal implementation of an ERC-721 token:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

contract MyNFT is ERC721 {
    uint256 public nextTokenId;
    address public admin;

    constructor() ERC721("MyNFT", "MNFT") {
        admin = msg.sender;
    }

    function mint(address to) external {
        require(msg.sender == admin, "Only admin can mint");
        _safeMint(to, nextTokenId);
        nextTokenId++;
    }
}
```

---

### **Comparison: ERC-20 vs ERC-721**

| Feature            | ERC-20                 | ERC-721           |
| ------------------ | ---------------------- | ----------------- |
| **Token Type**     | Fungible               | Non-Fungible      |
| **Divisibility**   | Divisible              | Indivisible       |
| **Use Cases**      | Cryptocurrencies, ICOs | Digital art, NFTs |
| **Identification** | No unique ID           | Unique `tokenId`  |

---

These standards make Ethereum tokens universally compatible and power a vast ecosystem of applications and marketplaces.
