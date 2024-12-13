### Solidity Data Types in Detail

Solidity, as a statically-typed language, requires the type of each variable to be specified. Below is a detailed explanation of the data types available in Solidity:

#### 1. **Value Types**

These types directly hold their data in the stack.

##### a. **Boolean**

- Represents `true` or `false` values.
- Example: `bool isActive = true;`

##### b. **Integer**

- Can be signed (`int`) or unsigned (`uint`).
- Signed integers (`int`) allow both positive and negative values, while unsigned integers (`uint`) only allow non-negative values.
- Sizes range from 8 bits to 256 bits, in steps of 8 (e.g., `uint8`, `int16`, `uint256`). The default size is 256 bits.
- Example: `uint256 balance = 1000;` or `int8 delta = -10;`

##### c. **Address**

- Holds Ethereum addresses, which are 20 bytes long.
- Addresses have member functions like `balance` (to check Ether balance) and `transfer`/`send` (to transfer Ether).
- Example: `address payable recipient = 0xAb8483F64d9C6d1EcF9b849Ae675734C6aDbcA94;`

##### d. **Fixed-Point Numbers** (currently not fully supported)

- Includes `fixed` and `ufixed` for signed and unsigned fixed-point numbers.

##### e. **Bytes**

- Fixed-size byte arrays (`bytes1`, `bytes2`, ..., `bytes32`).
- Example: `bytes4 data = 0x12345678;`
- `bytes` is a dynamically-sized byte array.

##### f. **Enums**

- Used to define custom data types with a fixed set of possible values.
- Example:
  ```solidity
  enum Status { Pending, Shipped, Delivered }
  Status currentStatus = Status.Pending;
  ```

#### 2. **Reference Types**

These types hold references to data stored in memory or storage.

##### a. **Arrays**

- Can be fixed-size or dynamic.
- Example:
  ```solidity
  uint[] dynamicArray;
  uint[5] fixedArray;
  ```
- Arrays have utility functions like `push` (for dynamic arrays) and can be indexed.

##### b. **Strings**

- Dynamically-sized UTF-8 encoded data.
- Example: `string greeting = "Hello, Solidity!";`

##### c. **Mapping**

- A key-value store where keys are mapped to values.
- Keys can be of simple types (e.g., `uint`, `address`), and values can be any type.
- Example:
  ```solidity
  mapping(address => uint256) public balances;
  ```

##### d. **Structs**

- Used to define complex data types.
- Example:
  ```solidity
  struct User {
      string name;
      uint age;
  }
  User user = User("Alice", 30);
  ```

#### 3. **Special Data Types**

##### a. **Function**

- Functions can be stored in variables and passed as arguments.
- Example:
  ```solidity
  function add(uint a, uint b) public pure returns (uint) {
      return a + b;
  }
  ```

##### b. **Ether Units**

- Solidity supports Ether units: `wei`, `gwei`, and `ether`.
- Example: `1 ether == 10**18 wei`

##### c. **Time Units**

- Solidity includes units like `seconds`, `minutes`, `hours`, `days`, and `weeks`.
- Example: `1 days == 86400 seconds`

#### Notes:

- Memory vs. Storage: Value types are stored in the stack, while reference types use memory or storage.
- Visibility: Variables can have visibility specifiers like `public`, `private`, `internal`, and `external`.
