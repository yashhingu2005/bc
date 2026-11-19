# Blockchain Practical Journal — Simplified Practicals Guide (P1 to P13)

**Student:** Yash Hingu (`31011123021` — LYCS)  
**Project Folder Location:** `C:\Users\Admin\.gemini\antigravity\scratch\blockchain_practicals_simplified\`

---

## Index of All 13 Practicals

| Practical | Aim / Topic | Stack / Tool Used |
| :--- | :--- | :--- |
| **P1** | Basic Blockchain in JavaScript (1A: Addition, 1B: Prime Number, 1C: Happy Number) | Node.js (`crypto` SHA-256) |
| **P2** | Blockchain Programs in Python (2A: Armstrong, 2B: Anagram, 2C: Bank Account) | Python 3 (`hashlib`, `datetime`) |
| **P3** | Solidity Basics (3A: Hello World, 3B: Arithmetic, 3C: Struct, 3D: Mapping, 3E: Palindrome) | Remix IDE (`Solidity ^0.8.21`) |
| **P4** | Solidity Contracts (4A: Bank Account, 4B: Perfect Number, 4C: Voting System) | Remix IDE (`Solidity ^0.8.20`) |
| **P5** | Solidity Contracts (5A: Transferring Ether, 5B: Student Management & Grades) | Remix IDE (`Solidity ^0.8.21`) |
| **P6** | Solidity Contracts (6A: Quiz Application with Events, 6B: Vehicle & Bike Inheritance) | Remix IDE (`Solidity ^0.8.21`) |
| **P7** | Addition DApp in VS Code using Truffle, Ganache & Web3.js | Truffle + Ganache (`7545`) + HTML/JS |
| **P8** | Connecting MetaMask with Ganache & Prime Number DApp using Truffle | Truffle + Ganache + MetaMask + Web3.js |
| **P9** | Decentralized Voting System Console DApp using Solidity & Java Web3j | Ganache + Remix + Maven (`Web3j`) |
| **P10** | Solidity Inheritance (10A: Online Shopping Store, 10B: Student Course Registration) | Remix IDE (`Solidity ^0.8.0`) |
| **P11** | Solidity Polymorphism & Events (11A: Vehicle Rental `virtual`/`override`, 11B: Crowdfunding) | Remix IDE (`Solidity ^0.8.20`) |
| **P12** | Developing and Deploying an Addition DApp using Hardhat & Ethers.js | Hardhat + Ethers.js v6 + MetaMask |
| **P13** | Decentralized Web Application for Perfect Number using Hardhat & Mocha Tests | Hardhat + Chai/Mocha + Ethers.js v6 |

---

## P1: Basic Blockchain in JavaScript (Addition, Prime Number, Happy Number)

**Aim:** To implement a basic blockchain in JavaScript where each block stores data, calculates a SHA-256 hash, and links to the previous block using its previous hash.

### Step-by-Step Setup & Execution
```powershell
mkdir P1_JS_Blockchain -Force
cd P1_JS_Blockchain
node --version
```
*(Run each part below using `node add_bc.js`, `node prime_bc.js`, and `node happy_bc.js` — each combines `add.js` and `test.js` into a single runnable file, or you can keep `add.js` + `test.js` separate.)*

### 1A: Basic Addition Blockchain (`add_bc.js`)
```javascript
const c = require('crypto');

class Block {
    constructor(id, n, n1, t, ph = "") {
        this.id = id; this.n = n; this.n1 = n1;
        this.result = n + n1; this.t = t; this.ph = ph;
        this.hash = this.calch();
    }
    calch() {
        return c.createHash('sha256')
            .update(String(this.id) + this.n + this.n1 + this.result + this.t + this.ph)
            .digest("hex");
    }
}

class Blockchain {
    constructor() { this.chain = [new Block(1, 0, 0, Date.now())]; }
    getCBlock() { return this.chain[this.chain.length - 1]; }
    addBlock(b) {
        b.ph = this.getCBlock().hash;
        b.hash = b.calch();
        this.chain.push(b);
    }
}

const bc = new Blockchain();
bc.addBlock(new Block(2, 23, 76, Date.now()));
console.log(bc.chain);
```

### 1B: Prime Number Blockchain (`prime_bc.js`)
```javascript
const c = require('crypto');

class Block {
    constructor(id, n, n1, t, ph = "") {
        this.id = id; this.n = n; this.n1 = n1;
        this.result = n + n1; this.t = t;
        this.statement = this.isPrime(n) ? `${n} is Prime` : `${n} is not Prime`;
        this.ph = ph; this.hash = this.calch();
    }
    isPrime(n) {
        if (n < 2) return false;
        for (let i = 2; i <= Math.sqrt(n); i++) if (n % i === 0) return false;
        return true;
    }
    calch() {
        return c.createHash('sha256')
            .update(String(this.id) + this.n + this.n1 + this.result + this.t + this.statement + this.ph)
            .digest("hex");
    }
}

class Blockchain {
    constructor() { this.chain = [new Block(1, 0, 0, Date.now())]; }
    getCBlock() { return this.chain[this.chain.length - 1]; }
    addBlock(b) {
        b.ph = this.getCBlock().hash;
        b.hash = b.calch();
        this.chain.push(b);
    }
}

const bc = new Blockchain();
bc.addBlock(new Block(2, 23, 76, Date.now()));
bc.addBlock(new Block(3, 24, 50, Date.now()));
bc.addBlock(new Block(4, 53, 20, Date.now()));
console.log(bc.chain);
```

### 1C: Happy Number Blockchain (`happy_bc.js`)
```javascript
const c = require('crypto');

class Block {
    constructor(id, n, t, ph = "") {
        this.id = id; this.n = n; this.t = t;
        this.statement = this.isHappy(n) ? `${n} is Happy` : `${n} is not Happy`;
        this.ph = ph; this.hash = this.calch();
    }
    isHappy(n) {
        let seen = new Set();
        while (n !== 1 && !seen.has(n)) {
            seen.add(n);
            n = String(n).split('').reduce((s, d) => s + d * d, 0);
        }
        return n === 1;
    }
    calch() {
        return c.createHash('sha256')
            .update(String(this.id) + this.n + this.t + this.statement + this.ph)
            .digest("hex");
    }
}

class Blockchain {
    constructor() { this.chain = [new Block(1, 0, 0)]; }
    getCBlock() { return this.chain[this.chain.length - 1]; }
    addBlock(b) {
        if (b.isHappy(b.n)) {
            b.ph = this.getCBlock().hash;
            b.hash = b.calch();
            this.chain.push(b);
            console.log(b.n + " Block added");
        } else {
            console.log(b.n + " Block not added");
        }
    }
}

const a = new Blockchain();
a.addBlock(new Block(2, 13, Date.now()));
a.addBlock(new Block(3, 24, Date.now()));
a.addBlock(new Block(4, 19, Date.now()));
console.log(a.chain);
```

**Run Commands:**
```powershell
node add_bc.js
node prime_bc.js
node happy_bc.js
```

---

## P2: Blockchain Programs in Python (Armstrong, Anagram, Bank Account)

**Aim:** Implement blockchain programs in Python for Armstrong numbers, Anagram validation, and Bank Account creation/deposit/withdrawal.

### Step-by-Step Setup & Execution
```powershell
mkdir P2_Python_Blockchain -Force
cd P2_Python_Blockchain
python --version
```

### 2A: Armstrong Numbers Blockchain (`armstrong_bc.py`)
```python
import hashlib, datetime

class Block:
    def __init__(s, i, n, ph="0"):
        s.i, s.t, s.n, s.ph = i, str(datetime.datetime.now()), n, ph
        s.h = hashlib.sha256(f"{s.i}{s.t}{s.n}{s.ph}".encode()).hexdigest()
    def check(s):
        digits = [int(d) for d in str(s.n)]
        return "Armstrong" if sum(d ** len(digits) for d in digits) == s.n else "Not Armstrong"

class Blockchain:
    def __init__(s): s.chain = [Block(0, 0)]
    def addBlock(s, n): s.chain.append(Block(len(s.chain), n, s.chain[-1].h))
    def disp(s):
        for b in s.chain:
            print(f"\nBlock: {b.i}\nTimestamp: {b.t}\nNumber: {b.n}\nHash: {b.h}\nPrevious Hash: {b.ph}\nResult: {b.check()}\n" + "-"*70)

b = Blockchain()
for num in [153, 45, 370, 123, 407]: b.addBlock(num)
b.disp()
```

### 2B: Anagram Blockchain (`anagram_bc.py`)
```python
import hashlib, datetime

class Block:
    def __init__(s, i, pair, ph="0"):
        s.i, s.t, s.n, s.ph = i, str(datetime.datetime.now()), pair, ph
        s.h = s.calhash()
    def calhash(s):
        return hashlib.sha256(f"{s.i}{s.t}{s.n}{s.ph}".encode()).hexdigest()
    def check(s):
        return "Anagram" if sorted(s.n[0].lower()) == sorted(s.n[1].lower()) else "Not Anagram"

class Blockchain:
    def __init__(s): s.chain = [Block(0, ("0", "0"))]
    def addBlock(s, a, b):
        temp = Block(len(s.chain), (a, b), s.chain[-1].h)
        if temp.check() == "Anagram":
            s.chain.append(temp); print("Anagram - Block Added")
        else:
            print("Not Anagram - Block Not Added")
    def disp(s):
        for b in s.chain:
            print(f"\nBlock: {b.i}\nTimestamp: {b.t}\nLetters: {b.n[0]} and {b.n[1]}\nHash: {b.h}\nPrevious Hash: {b.ph}\nResult: {b.check()}\n" + "-"*70)
    def validate(s):
        return all(s.chain[i].h == s.chain[i].calhash() and s.chain[i].ph == s.chain[i-1].h for i in range(1, len(s.chain)))

b = Blockchain()
for w1, w2 in [("earth","heart"), ("hello","world"), ("evil","vile"), ("python","typhon"), ("night","thing")]:
    b.addBlock(w1, w2)
b.disp()
print("Blockchain is valid" if b.validate() else "Blockchain is not valid")
```

### 2C: Account Creation, Deposit & Withdrawal Blockchain (`bank_bc.py`)
```python
import hashlib, datetime

class Block:
    def __init__(s, i, bal, op, amt, ph="0"):
        s.i, s.t, s.balance, s.operation, s.amount, s.ph = i, str(datetime.datetime.now()), bal, op, amt, ph
        s.h = s.calhash()
    def calhash(s):
        return hashlib.sha256(f"{s.i}{s.t}{s.balance}{s.operation}{s.amount}{s.ph}".encode()).hexdigest()

class Blockchain:
    def __init__(s, init_amt=0):
        s.chain = [Block(0, init_amt, "Account Created", init_amt)]
    def deposit(s, amt):
        new_bal = s.chain[-1].balance + amt
        s.chain.append(Block(len(s.chain), new_bal, "Deposit", amt, s.chain[-1].h))
        print(f"Deposit of {amt} successful. New balance: {new_bal}")
    def withdraw(s, amt):
        cur = s.chain[-1].balance
        if amt > cur:
            print(f"Withdrawal rejected: insufficient balance ({cur})")
            return
        new_bal = cur - amt
        s.chain.append(Block(len(s.chain), new_bal, "Withdraw", amt, s.chain[-1].h))
        print(f"Withdrawal of {amt} successful. New balance: {new_bal}")
    def disp(s):
        for b in s.chain:
            print(f"\nBlock: {b.i}\nTimeStamp: {b.t}\nOperation: {b.operation}\nAmount: {b.amount}\nBalance after: {b.balance}\nPrevious hash: {b.ph}\nCurrent Hash: {b.h}\n" + "-"*65)
    def is_valid(s):
        return all(s.chain[i].h == s.chain[i].calhash() and s.chain[i].ph == s.chain[i-1].h for i in range(1, len(s.chain)))

acc = Blockchain(1500)
acc.deposit(700); acc.withdraw(400); acc.deposit(250); acc.withdraw(1000)
acc.disp()
print("Is the blockchain valid?", acc.is_valid())
```

**Run Commands:**
```powershell
python armstrong_bc.py
python anagram_bc.py
python bank_bc.py
```

---

## P3: Solidity Basics in Remix IDE (Hello World, Arithmetic, Struct, Mapping, Palindrome)

**Aim:** Write and deploy basic Solidity smart contracts in Remix IDE (`https://remix.ethereum.org`).

### Step-by-Step Setup in Remix IDE (Applies to P3, P4, P5, P6, P10, P11)
1. Open **Remix IDE** (`https://remix.ethereum.org`).
2. Under `contracts/`, create a new `.sol` file (e.g., `Practical3.sol`) and paste the contract code.
3. Open the **Solidity Compiler** tab (left sidebar) $\rightarrow$ select compiler version `0.8.21` $\rightarrow$ click **Compile**.
4. Open the **Deploy & Run Transactions** tab $\rightarrow$ set Environment to **Remix VM (Cancun / Osaka)** $\rightarrow$ click **Deploy**.
5. Expand the deployed contract under **Deployed Contracts** to call functions.

### 3A: Hello World (`msgC.sol`)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;

contract msgC {
    string public m;
    function setMessage(string memory n) public {
        m = n;
    }
}
```
* **Test in Remix:** Call `setMessage("hello world")` $\rightarrow$ click `m` $\rightarrow$ returns `0: string: hello world`.

### 3B: Arithmetic Operators (`ari.sol`)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;

contract ari {
    function add(int a, int b) public pure returns (int) { return a + b; }
    function sub(int a, int b) public pure returns (int) { return a - b; }
    function multi(int a, int b) public pure returns (int) { return a * b; }
    function divide(int a, int b) public pure returns (int) { return a / b; }
}
```
* **Test in Remix:** Call `add(24, 42)` $\rightarrow$ returns `0: int256: 66`.

### 3C: Student Structure (`studentC.sol`)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;

contract studentC {
    struct StudentD { uint sid; string name; string add; }
    StudentD s;
    function addStudent(uint i, string memory n, string memory a) public {
        s = StudentD(i, n, a);
    }
    function getS() public view returns (uint, string memory, string memory) {
        return (s.sid, s.name, s.add);
    }
}
```
* **Test in Remix:** Call `addStudent(301, "abc", "thane")` $\rightarrow$ click `getS` $\rightarrow$ returns `301, "abc", "thane"`.

### 3D: Employee Structure via Mapping (`employee.sol`)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;

contract employee {
    struct empD { uint eid; string ename; uint esal; string edes; }
    mapping(uint => empD) public e;

    function addempD(uint i, string memory n, uint s, string memory a) public {
        e[i] = empD(i, n, s, a);
    }
    function getE(uint i) public view returns (uint, string memory, uint, string memory) {
        empD memory e1 = e[i];
        return (e1.eid, e1.ename, e1.esal, e1.edes);
    }
    function updatesal(uint i, uint ns) public {
        e[i].esal = ns;
    }
}
```
* **Test in Remix:** Call `addempD(301, "abc", 65000, "thane")` $\rightarrow$ call `getE(301)` $\rightarrow$ returns `301, "abc", 65000, "thane"`.

### 3E: Palindrome Number Checker (`Palindrome.sol`)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;

contract Palindrome {
    function check(uint n) public pure returns (string memory) {
        uint rev = 0;
        uint n1 = n;
        while (n != 0) {
            rev = rev * 10 + (n % 10);
            n /= 10;
        }
        return rev == n1 ? "Pallindrome" : "not Pallindrome";
    }
}
```
* **Test in Remix:** Call `check(9876789)` $\rightarrow$ returns `0: string: Pallindrome`.

---

## P4: Solidity Contracts (Bank Account, Perfect Number, Voting System)

### 4A (Q1): Bank Account Management (`Bank.sol`)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Bank {
    struct User { uint accno; string name; uint bal; }
    mapping(uint => User) public users;

    function createAccount(uint accno, string memory name) public returns (uint) {
        require(accno != 0 && users[accno].accno == 0, "Invalid or existing account");
        users[accno] = User(accno, name, 0);
        return accno;
    }
    function deposit(uint accno) public payable {
        require(users[accno].accno != 0 && msg.value > 0, "Invalid deposit");
        users[accno].bal += msg.value;
    }
    function withdraw(uint accno, uint amt) public {
        require(users[accno].bal >= amt, "Insufficient balance");
        users[accno].bal -= amt;
        payable(msg.sender).transfer(amt);
    }
    function getBalance(uint accno) public view returns (uint) {
        return users[accno].bal;
    }
}
```
* **Test in Remix:**
  1. `createAccount(201, "CC")`
  2. Set **VALUE** box in Remix to `10000` wei $\rightarrow$ call `deposit(201)`
  3. Set **VALUE** to `0` $\rightarrow$ call `withdraw(201, 50)`
  4. Call `getBalance(201)` $\rightarrow$ returns `9950`.

### 4B (Q2): Perfect Number (`per.sol`)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;

contract per {
    function pn(uint n) public pure returns (string memory) {
        if (n <= 1) return "Not perfect";
        uint sum = 1;
        for (uint i = 2; i <= n / 2; i++) if (n % i == 0) sum += i;
        return sum == n ? "Perfect number" : "Not perfect";
    }
}
```
* **Test in Remix:** Call `pn(6)` $\rightarrow$ returns `0: string: Perfect number`.

### 4C (Q3): Voting Contract (`Voting.sol`)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Voting {
    string[] public candidates;
    mapping(address => bool) public hasVoted;
    mapping(string => uint) public voteCount;

    constructor(string[] memory candidateNames) {
        candidates = candidateNames;
    }
    function vote(string memory candidateName) public {
        require(!hasVoted[msg.sender], "Already voted");
        bool exists = false;
        for (uint i = 0; i < candidates.length; i++) {
            if (keccak256(bytes(candidates[i])) == keccak256(bytes(candidateName))) exists = true;
        }
        require(exists, "Candidate does not exist");
        voteCount[candidateName]++;
        hasVoted[msg.sender] = true;
    }
    function Winner() public view returns (string memory) {
        string memory w = candidates[0];
        for (uint i = 1; i < candidates.length; i++) {
            if (voteCount[candidates[i]] > voteCount[w]) w = candidates[i];
        }
        return w;
    }
}
```
* **Test in Remix:** Pass `["AB","BC","CB"]` next to **Deploy** $\rightarrow$ call `vote("BC")` $\rightarrow$ call `Winner()` $\rightarrow$ returns `"BC"`.

---

## P5: Solidity Contracts (Transferring Ether & Student Management System)

### 5A: Transferring Ether (`BankC.sol`)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;

contract BankC {
    uint public balance;
    function deposit() public payable {
        balance += msg.value;
    }
    function transferE(address payable r) public payable {
        require(address(this).balance >= msg.value, "Insufficient balance");
        r.transfer(msg.value);
    }
}
```
* **Test in Remix:** Copy Account 2's address (`0xAb8...35cb2`), enter a Wei amount in the **VALUE** field (or deposit first), paste the address into `transferE`, and click **Transact**.

### 5B: Student Management System (`Student.sol`)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Student {
    uint256[] public studentIds;
    string[] public studentNames;
    string[] public studentLocations;
    uint256[5][] public studentMarks;

    function addStud(uint256 _id, string memory _name, string memory _loc, uint256[5] memory _marks) public {
        studentIds.push(_id); studentNames.push(_name);
        studentLocations.push(_loc); studentMarks.push(_marks);
    }
    function totalStudent() public view returns (uint256) {
        return studentIds.length;
    }
    function displayAllStudents() public view returns (uint256[] memory, string[] memory, string[] memory, uint256[5][] memory) {
        return (studentIds, studentNames, studentLocations, studentMarks);
    }
    function getAllGrades() public view returns (string[] memory) {
        string[] memory grades = new string[](studentIds.length);
        for (uint i = 0; i < studentIds.length; i++) {
            uint total = 0;
            for (uint j = 0; j < 5; j++) total += studentMarks[i][j];
            uint avg = total / 5;
            grades[i] = avg >= 75 ? "A" : (avg >= 60 ? "B" : "C");
        }
        return grades;
    }
    function getStudentById(uint256 _id) public view returns (uint256, string memory, string memory, uint256[5] memory) {
        for (uint i = 0; i < studentIds.length; i++) {
            if (studentIds[i] == _id) return (studentIds[i], studentNames[i], studentLocations[i], studentMarks[i]);
        }
        revert("Student ID not found");
    }
}
```
* **Test in Remix:** Call `addStud(101, "abc", "thane", [24,48,67,87,70])` $\rightarrow$ call `displayAllStudents()`, `totalStudent()`, `getAllGrades()`, and `getStudentById(101)`.

---

## P6: Solidity Contracts (Quiz Application & Inheritance)

### 6A: Quiz Application (`quiz.sol`)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;

contract quiz {
    struct Question { string Qtest; string CA; }
    Question[] public q;
    uint public tc;
    uint public tw;
    event Answer(string qu, string uans, bool isc);

    function add(string memory qt, string memory cans) public {
        q.push(Question(qt, cans));
    }
    function ansQ(uint i, string memory uans) public {
        require(i < q.length, "Invalid question index");
        bool ok = keccak256(bytes(uans)) == keccak256(bytes(q[i].CA));
        if (ok) tc++; else tw++;
        emit Answer(q[i].Qtest, uans, ok);
    }
}
```
* **Test in Remix:** Call `add("What is 2 + 2?", "4")` $\rightarrow$ call `ansQ(0, "4")` $\rightarrow$ check `tc` (`1`) and `tw` (`0`).

### 6B: Single Inheritance — Vehicle & Bike (`Vehicle.sol`)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;

contract Vehicle {
    string vehType; string nt;
    constructor(string memory t, string memory n) { vehType = t; nt = n; }
    function disp() public view returns (string memory, string memory) { return (vehType, nt); }
}

contract Bike is Vehicle {
    string brandname; string color;
    constructor(string memory t, string memory n, string memory bn, string memory c) Vehicle(t, n) {
        brandname = bn; color = c;
    }
    function dispB() public view returns (string memory, string memory) {
        return (brandname, color);
    }
}
```
* **Test in Remix:** Select **`Bike`** from the Contract dropdown $\rightarrow$ deploy with `"TWO WHEELER", "BIKE", "HONDA", "BLACK"` $\rightarrow$ call `disp()` and `dispB()`.

---

## P7: Addition DApp in VS Code using Truffle, Ganache & Web3.js

**Aim:** Create and deploy an Addition Smart Contract DApp in VS Code using Truffle and Ganache.

### Step-by-Step Setup & Execution

**Step 1: Install Node.js & Truffle globally and start Ganache GUI (or Ganache CLI on port 7545)**
```powershell
npm install -g truffle
mkdir P7_Truffle_Addition -Force
cd P7_Truffle_Addition
truffle init
```
*(Open **Ganache** and click **Quickstart** so RPC server runs at `http://127.0.0.1:7545` with Network ID `5777`.)*

**Step 2: Create `contracts/Addition.sol`**
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.21;

contract Addition {
    function add(uint a, uint b) public pure returns (uint) {
        return a + b;
    }
}
```

**Step 3: Create `migrations/2_deploy_contracts.js`**
```javascript
const Addition = artifacts.require("Addition");
module.exports = function (deployer) {
    deployer.deploy(Addition);
};
```

**Step 4: Replace `truffle-config.js`**
```javascript
module.exports = {
  networks: {
    development: { host: "127.0.0.1", port: 7545, network_id: "*" }
  },
  compilers: {
    solc: {
      version: "0.8.21",
      settings: { optimizer: { enabled: false, runs: 200 }, evmVersion: "paris" }
    }
  }
};
```

**Step 5: Compile and migrate contract to Ganache**
```powershell
truffle compile
truffle migrate --reset
```
*(Copy the deployed `contract address` printed in the terminal and paste it into `app/app.js` below.)*

**Step 6: Create `app/index.html` and `app/app.js`**

`app/index.html`
```html
<!DOCTYPE html>
<html>
<head><title>Addition DApp</title></head>
<body>
    <h1>Blockchain Addition</h1>
    <input type="number" id="num1" placeholder="Enter first number">
    <input type="number" id="num2" placeholder="Enter second number">
    <button onclick="addNumbers()">Add</button>
    <h2>Result: <span id="result">-</span></h2>
    <script src="https://cdn.jsdelivr.net/npm/web3@1.10.0/dist/web3.min.js"></script>
    <script src="app.js"></script>
</body>
</html>
```

`app/app.js`
```javascript
const contractAddress = "PASTE_DEPLOYED_CONTRACT_ADDRESS_HERE";
const contractABI = [{
    "inputs": [{"name": "a", "type": "uint256"}, {"name": "b", "type": "uint256"}],
    "name": "add",
    "outputs": [{"name": "", "type": "uint256"}],
    "stateMutability": "pure",
    "type": "function"
}];

const web3 = new Web3("http://127.0.0.1:7545");
const contract = new web3.eth.Contract(contractABI, contractAddress);

async function addNumbers() {
    const n1 = document.getElementById("num1").value;
    const n2 = document.getElementById("num2").value;
    document.getElementById("result").innerText = await contract.methods.add(n1, n2).call();
}
```

**Step 7: Open `app/index.html` with VS Code Live Server (or `python -m http.server 8000`)**

---

## P8: Connecting MetaMask with Ganache & Prime Number DApp using Truffle

**Aim:** Connect MetaMask to a local Ganache blockchain and build a Prime Number Checker DApp with Truffle.

### Step-by-Step Setup & Execution

**Step 1: Connect MetaMask to Ganache**
1. Open **Ganache** (`http://127.0.0.1:7545`).
2. In **MetaMask** $\rightarrow$ **Settings $\rightarrow$ Networks $\rightarrow$ Add a network manually**:
   - **Network Name:** `Ganache Local`
   - **RPC URL:** `http://127.0.0.1:7545`
   - **Chain ID:** `1337`
   - **Currency Symbol:** `ETH`
3. In Ganache, click the **Key icon** next to Account 1 $\rightarrow$ copy the **Private Key** $\rightarrow$ in MetaMask click **Add account / Import account** and paste the Private Key.

**Step 2: Initialize Truffle project**
```powershell
mkdir P8_Truffle_Prime -Force
cd P8_Truffle_Prime
truffle init
```

**Step 3: Create `contracts/Prime.sol`**
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Prime {
    function isPrime(uint256 number) public pure returns (bool) {
        if (number < 2) return false;
        for (uint256 i = 2; i * i <= number; i++) {
            if (number % i == 0) return false;
        }
        return true;
    }
}
```

**Step 4: Create `migrations/1_deploy.js` and use the same `truffle-config.js` from P7**
```javascript
const Prime = artifacts.require("Prime");
module.exports = function (deployer) {
    deployer.deploy(Prime);
};
```

**Step 5: Compile and deploy to Ganache**
```powershell
truffle compile
truffle migrate --reset
```
*(Copy the deployed `Prime` contract address into `src/app.js` below.)*

**Step 6: Create `src/index.html` and `src/app.js`**

`src/index.html`
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Prime Number DApp</title>
    <script src="https://cdn.jsdelivr.net/npm/web3@1.10.0/dist/web3.min.js"></script>
</head>
<body>
    <h1>Prime Number DApp</h1>
    <label>Enter Number:</label>
    <input type="number" id="number">
    <button onclick="checkPrime()">Check Prime</button>
    <p>Result: <span id="result"></span></p>
    <script src="app.js"></script>
</body>
</html>
```

`src/app.js`
```javascript
const contractAddress = "PASTE_DEPLOYED_PRIME_ADDRESS_HERE";
const contractABI = [{
    "inputs": [{"internalType": "uint256", "name": "number", "type": "uint256"}],
    "name": "isPrime",
    "outputs": [{"internalType": "bool", "name": "", "type": "bool"}],
    "stateMutability": "pure",
    "type": "function"
}];

async function checkPrime() {
    if (!window.ethereum) return alert("Please install MetaMask");
    const web3 = new Web3(window.ethereum);
    await window.ethereum.request({ method: "eth_requestAccounts" });
    const contract = new web3.eth.Contract(contractABI, contractAddress);
    const num = document.getElementById("number").value;
    const res = await contract.methods.isPrime(num).call();
    document.getElementById("result").innerText = res ? "Prime Number" : "Not a Prime Number";
}
```

---

## P9: Voting System DApp using Solidity & Java Web3j

**Aim:** Create a Decentralized Application (DApp) for a Voting System using Solidity, Ganache, and Java Web3j.

### Step-by-Step Setup & Execution

**Step 1: Deploy `Voting.sol` to Ganache via Remix IDE**
1. Open Ganache (`http://127.0.0.1:7545`).
2. In Remix IDE, compile `Voting.sol` below, switch Environment to **Dev - Ganache Provider (`http://127.0.0.1:7545`)**, click **Deploy**, and copy the deployed **Contract Address** + a Ganache account **Private Key**.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

contract Voting {
    struct Candidate { uint id; string name; uint voteCount; }
    mapping(uint => Candidate) public candidates;
    mapping(address => bool) public hasVoted;
    uint public candidatesCount;
    event Voted(address indexed voter, uint indexed candidateId);

    constructor() {
        addCandidate("Alice"); addCandidate("Bob"); addCandidate("Charlie");
    }
    function addCandidate(string memory _name) private {
        candidatesCount++;
        candidates[candidatesCount] = Candidate(candidatesCount, _name, 0);
    }
    function vote(uint _candidateId) public {
        require(!hasVoted[msg.sender], "Already voted");
        require(_candidateId > 0 && _candidateId <= candidatesCount, "Invalid candidate");
        hasVoted[msg.sender] = true;
        candidates[_candidateId].voteCount++;
        emit Voted(msg.sender, _candidateId);
    }
}
```

**Step 2: Create Maven project folder and `pom.xml`**
```powershell
mkdir P9_Web3j_Voting\src\main\java\com\example -Force
cd P9_Web3j_Voting
```

`pom.xml`
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>voting-app</artifactId>
    <version>1.0</version>
    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>
    <dependencies>
        <dependency><groupId>org.web3j</groupId><artifactId>core</artifactId><version>4.9.8</version></dependency>
        <dependency><groupId>org.slf4j</groupId><artifactId>slf4j-simple</artifactId><version>1.7.36</version></dependency>
    </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>3.1.0</version>
                <configuration><mainClass>com.example.VotingApp</mainClass></configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

**Step 3: Create `src/main/java/com/example/VotingApp.java`**
```java
package com.example;

import org.web3j.abi.*;
import org.web3j.abi.datatypes.*;
import org.web3j.abi.datatypes.generated.Uint256;
import org.web3j.crypto.Credentials;
import org.web3j.protocol.Web3j;
import org.web3j.protocol.core.DefaultBlockParameterName;
import org.web3j.protocol.core.methods.request.Transaction;
import org.web3j.protocol.http.HttpService;
import org.web3j.tx.RawTransactionManager;
import org.web3j.tx.response.PollingTransactionReceiptProcessor;
import java.math.BigInteger;
import java.util.*;

public class VotingApp {
    static Web3j web3 = Web3j.build(new HttpService("http://127.0.0.1:7545"));
    static Credentials creds;
    static RawTransactionManager txMgr;
    static String contractAddr;

    public static void main(String[] args) throws Exception {
        Scanner sc = new Scanner(System.in);
        long chainId = web3.ethChainId().send().getChainId().longValue();
        System.out.print("Enter Ganache Private Key: ");
        creds = Credentials.create(sc.nextLine().trim());
        txMgr = new RawTransactionManager(web3, creds, chainId);
        System.out.print("Enter Deployed Voting Contract Address: ");
        contractAddr = sc.nextLine().trim();

        while (true) {
            System.out.println("\n1. Show Candidates  2. Cast Vote  3. Check Voted  4. Exit");
            String ch = sc.nextLine().trim();
            if (ch.equals("1")) showCandidates();
            else if (ch.equals("2")) {
                showCandidates();
                System.out.print("Candidate ID: ");
                castVote(Integer.parseInt(sc.nextLine().trim()));
            } else if (ch.equals("3")) {
                System.out.println(hasVoted() ? "You have already voted." : "Not voted yet.");
            } else break;
        }
    }

    static List<Type> call(Function f) throws Exception {
        String res = web3.ethCall(Transaction.createEthCallTransaction(creds.getAddress(), contractAddr, FunctionEncoder.encode(f)), DefaultBlockParameterName.LATEST).send().getValue();
        return FunctionReturnDecoder.decode(res, f.getOutputParameters());
    }

    static void showCandidates() throws Exception {
        Function cntF = new Function("candidatesCount", Collections.emptyList(), List.of(new TypeReference<Uint256>() {}));
        int count = ((BigInteger) call(cntF).get(0).getValue()).intValue();
        System.out.println("ID | Name | Votes");
        for (int i = 1; i <= count; i++) {
            Function f = new Function("candidates", List.of(new Uint256(i)), Arrays.asList(new TypeReference<Uint256>() {}, new TypeReference<Utf8String>() {}, new TypeReference<Uint256>() {}));
            List<Type> r = call(f);
            System.out.println(r.get(0).getValue() + "  | " + r.get(1).getValue() + " | " + r.get(2).getValue());
        }
    }

    static boolean hasVoted() throws Exception {
        Function f = new Function("hasVoted", List.of(new Address(creds.getAddress())), List.of(new TypeReference<Bool>() {}));
        return (Boolean) call(f).get(0).getValue();
    }

    static void castVote(int id) throws Exception {
        Function f = new Function("vote", List.of(new Uint256(id)), Collections.emptyList());
        String hash = txMgr.sendTransaction(BigInteger.valueOf(20_000_000_000L), BigInteger.valueOf(300_000L), contractAddr, FunctionEncoder.encode(f), BigInteger.ZERO).getTransactionHash();
        var rec = new PollingTransactionReceiptProcessor(web3, 1000, 20).waitForTransactionReceipt(hash);
        System.out.println("Vote recorded in block " + rec.getBlockNumber());
    }
}
```

**Step 4: Compile and run with Maven**
```powershell
mvn compile exec:java
```

---

## P10: Solidity Inheritance (Online Shopping System & Student Course Registration)

### 10A (1): Online Shopping System (`OnlineStore.sol`)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Product {
    uint public pro_id; string public pro_name; uint public price; uint public stock;
    constructor(uint _id, string memory _name, uint _price, uint _stock) {
        pro_id = _id; pro_name = _name; price = _price; stock = _stock;
    }
    function display() public view returns (uint, string memory, uint, uint) {
        return (pro_id, pro_name, stock, price);
    }
}

contract OnlineStore is Product {
    address public owner;
    constructor(uint _id, string memory _name, uint _price, uint _stock) Product(_id, _name, _price, _stock) {
        owner = msg.sender;
    }
    function purchase(uint _quantity) public payable {
        require(_quantity > 0 && stock >= _quantity, "Invalid quantity or stock");
        require(msg.value == price * _quantity, "Incorrect payment");
        stock -= _quantity;
    }
    function getBalance() public view returns (uint) {
        return address(this).balance;
    }
}
```
* **Test in Remix:** Deploy **`OnlineStore`** with `201, "IPHONE", 600, 6` $\rightarrow$ set **VALUE** to `2400` wei $\rightarrow$ call `purchase(4)` $\rightarrow$ call `display()` (stock becomes `2`) and `getBalance()` (`2400`).

### 10B (2): Student Course Registration (`StudentCourse.sol`)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Person {
    string public stu_name; uint public stu_id;
    constructor(string memory _name, uint _id) { stu_name = _name; stu_id = _id; }
}

contract Student is Person {
    string public course_name; uint public course_fees; uint public max_capacity; uint public registeredCount;
    mapping(uint => bool) public isRegistered;

    constructor(string memory _name, uint _id, string memory _course, uint _fees, uint _cap) Person(_name, _id) {
        course_name = _course; course_fees = _fees; max_capacity = _cap;
    }
    function register(uint _studentId) public payable {
        require(registeredCount < max_capacity, "Seats are full");
        require(!isRegistered[_studentId], "Already registered");
        require(msg.value == course_fees, "Pay exact course fee");
        isRegistered[_studentId] = true;
        registeredCount++;
    }
}
```
* **Test in Remix:** Deploy **`Student`** with `"ABC", 201, "CS", 600, 20` $\rightarrow$ set **VALUE** to `600` wei $\rightarrow$ call `register(201)` $\rightarrow$ check `isRegistered(201)` (`true`).

---

## P11: Solidity Function Overriding & Crowdfunding

### 11A (Q1): Vehicle Rental System with `virtual` & `override` (`Rental.sol`)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Rental {
    string public agencyName;
    event VehicleRented(address indexed renter, string vehicleType, uint daysRented, uint totalRent);

    constructor(string memory _agencyName) { agencyName = _agencyName; }
    function calculateRent(uint daysRented) public pure virtual returns (uint) {
        return daysRented * 100;
    }
}

contract CarRental is Rental {
    constructor(string memory _agencyName) Rental(_agencyName) {}
    function calculateRent(uint daysRented) public pure override returns (uint) {
        return daysRented * 1000;
    }
    function rentCar(uint daysRented) public {
        emit VehicleRented(msg.sender, "Car", daysRented, calculateRent(daysRented));
    }
}

contract BikeRental is Rental {
    constructor(string memory _agencyName) Rental(_agencyName) {}
    function calculateRent(uint daysRented) public pure override returns (uint) {
        return daysRented * 300;
    }
    function rentBike(uint daysRented) public {
        emit VehicleRented(msg.sender, "Bike", daysRented, calculateRent(daysRented));
    }
}
```
* **Test in Remix:** Deploy **`CarRental`** with `"bmw"` $\rightarrow$ call `rentCar(4)` (emits `VehicleRented` with `totalRent: 4000`). Deploy **`BikeRental`** with `"triumph"` $\rightarrow$ call `rentBike(5)` (emits `totalRent: 1500`).

### 11B (Q2): Crowdfunding Contract (`Crowdfunding.sol`)
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract CampaignDetails {
    address public owner; uint public fundingGoal; uint public deadline;
    constructor(uint _fundingGoal, uint _duration) {
        owner = msg.sender; fundingGoal = _fundingGoal; deadline = block.timestamp + _duration;
    }
}

contract Crowdfunding is CampaignDetails {
    uint public totalRaised; bool public goalReached;
    mapping(address => uint) public contributions;
    event ContributionReceived(address indexed contributor, uint amount);
    event GoalReached(uint totalAmount);

    constructor(uint _fundingGoal, uint _duration) CampaignDetails(_fundingGoal, _duration) {}

    function contribute() public payable {
        require(block.timestamp < deadline && !goalReached && msg.value > 0, "Cannot contribute");
        contributions[msg.sender] += msg.value;
        totalRaised += msg.value;
        emit ContributionReceived(msg.sender, msg.value);
        if (totalRaised >= fundingGoal) {
            goalReached = true;
            emit GoalReached(totalRaised);
        }
    }
    function refund() public {
        require(block.timestamp >= deadline && !goalReached, "Refund not available");
        uint amt = contributions[msg.sender];
        require(amt > 0, "No contribution");
        contributions[msg.sender] = 0; totalRaised -= amt;
        (bool ok, ) = payable(msg.sender).call{value: amt}("");
        require(ok, "Refund failed");
    }
}
```
* **Test in Remix:** Deploy **`Crowdfunding`** with `_fundingGoal: 6`, `_duration: 60` $\rightarrow$ set **VALUE** to `6` wei $\rightarrow$ call `contribute()` $\rightarrow$ check `goalReached` (`true`).

---

## P12: Addition DApp using Hardhat, Ethers.js & MetaMask

**Aim:** Develop and deploy a Solidity Smart Contract DApp for Addition using Hardhat.

### Step-by-Step Setup & Execution

**Step 1: Initialize project and install Hardhat (v2 with Toolbox for easiest CommonJS setup)**
```powershell
mkdir P12_Hardhat_Addition\contracts -Force
mkdir P12_Hardhat_Addition\scripts -Force
mkdir P12_Hardhat_Addition\frontend -Force
cd P12_Hardhat_Addition
npm init -y
npm install --save-dev hardhat@2.22.17 @nomicfoundation/hardhat-toolbox
```

**Step 2: Create `hardhat.config.js`**
```javascript
require("@nomicfoundation/hardhat-toolbox");
module.exports = { solidity: "0.8.24" };
```

**Step 3: Create `contracts/Addition.sol`**
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Addition {
    function add(uint256 a, uint256 b) public pure returns (uint256) {
        return a + b;
    }
}
```

**Step 4: Create `scripts/deploy.js`**
```javascript
const { ethers } = require("hardhat");
async function main() {
    const Addition = await ethers.getContractFactory("Addition");
    const addition = await Addition.deploy();
    await addition.waitForDeployment();
    console.log("Addition contract deployed to:", await addition.getAddress());
}
main().catch((e) => { console.error(e); process.exitCode = 1; });
```

**Step 5: Create `frontend/index.html` (Single file with embedded Ethers.js v6)**
```html
<!DOCTYPE html>
<html lang="en">
<head><meta charset="UTF-8"><title>Addition DApp</title></head>
<body>
    <h1>Addition DApp</h1>
    <p>Enter two numbers and calculate their sum using Solidity.</p>
    <button id="connectButton">Connect MetaMask</button>
    <br><br>
    <input type="number" id="num1" placeholder="Enter first number">
    <input type="number" id="num2" placeholder="Enter second number">
    <button id="addButton">ADD</button>
    <h3 id="result">Result: --</h3>
    <p id="status"></p>

    <script src="https://cdn.jsdelivr.net/npm/ethers@6.13.5/dist/ethers.umd.min.js"></script>
    <script>
        const contractAddress = "0x5FbDB2315678afecb367f032d93F642f64180aa3";
        const contractABI = ["function add(uint256 a, uint256 b) public pure returns (uint256)"];
        let contract;

        document.getElementById("connectButton").onclick = async () => {
            if (!window.ethereum) return alert("Install MetaMask!");
            await window.ethereum.request({ method: "eth_requestAccounts" });
            try {
                await window.ethereum.request({ method: "wallet_switchEthereumChain", params: [{ chainId: "0x7a69" }] });
            } catch (e) {
                if (e.code === 4902) {
                    await window.ethereum.request({
                        method: "wallet_addEthereumChain",
                        params: [{ chainId: "0x7a69", chainName: "Hardhat Local", nativeCurrency: { name: "ETH", symbol: "ETH", decimals: 18 }, rpcUrls: ["http://127.0.0.1:8545"] }]
                    });
                }
            }
            const provider = new ethers.BrowserProvider(window.ethereum);
            const signer = await provider.getSigner();
            contract = new ethers.Contract(contractAddress, contractABI, signer);
            document.getElementById("status").innerText = "Connected: " + await signer.getAddress();
        };

        document.getElementById("addButton").onclick = async () => {
            const n1 = BigInt(document.getElementById("num1").value);
            const n2 = BigInt(document.getElementById("num2").value);
            const ans = await contract.add(n1, n2);
            document.getElementById("result").innerText = "Result: " + ans.toString();
        };
    </script>
</body>
</html>
```

**Step 6: Compile, start Hardhat node, deploy, and serve frontend**
```powershell
# Terminal 1 (Keep running):
npx hardhat node

# Terminal 2 (Compile & deploy to localhost:8545):
npx hardhat compile
npx hardhat run scripts/deploy.js --network localhost
# Copy deployed address into frontend/index.html if different from 0x5FbDB2315678afecb367f032d93F642f64180aa3, then serve:
python -m http.server 8000
```
*(Open `http://localhost:8000/frontend/` in your browser, click **Connect MetaMask**, enter `1` and `2`, and click **ADD** $\rightarrow$ `Result: 3`.)*

---

## P13: Decentralized Web Application for Perfect Number using Hardhat

**Aim:** Make a decentralized web application for checking Perfect Numbers using Hardhat configuration and automated unit tests.

### Step-by-Step Setup & Execution

**Step 1: Create project folders and install Hardhat Toolbox**
```powershell
mkdir P13_Hardhat_PerfectNumber\contracts -Force
mkdir P13_Hardhat_PerfectNumber\scripts -Force
mkdir P13_Hardhat_PerfectNumber\test -Force
cd P13_Hardhat_PerfectNumber
npm init -y
npm install --save-dev hardhat@2.22.17 @nomicfoundation/hardhat-toolbox
```

**Step 2: Create `hardhat.config.js`**
```javascript
require("@nomicfoundation/hardhat-toolbox");
module.exports = { solidity: "0.8.28" };
```

**Step 3: Create `contracts/PerfectNumber.sol`**
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.28;

contract PerfectNumber {
    function isPerfect(uint256 n) public pure returns (bool) {
        if (n < 2) return false;
        uint256 sum = 1;
        for (uint256 i = 2; i <= n / i; i++) {
            if (n % i == 0) {
                sum += i;
                if (n / i != i) sum += n / i;
            }
        }
        return sum == n;
    }
}
```

**Step 4: Create `scripts/deploy.js`**
```javascript
const { ethers } = require("hardhat");
async function main() {
    const PerfectNumber = await ethers.getContractFactory("PerfectNumber");
    const pn = await PerfectNumber.deploy();
    await pn.waitForDeployment();
    console.log("PerfectNumber deployed to:", await pn.getAddress());
}
main().catch((e) => { console.error(e); process.exitCode = 1; });
```

**Step 5: Create `test/PerfectNumber.js`**
```javascript
const { expect } = require("chai");
const { ethers } = require("hardhat");

describe("PerfectNumber", function () {
    let pn;
    beforeEach(async function () {
        pn = await (await ethers.getContractFactory("PerfectNumber")).deploy();
        await pn.waitForDeployment();
    });
    it("identifies 6 and 28 as perfect", async function () {
        expect(await pn.isPerfect(6)).to.equal(true);
        expect(await pn.isPerfect(28)).to.equal(true);
    });
    it("rejects 10 and 1", async function () {
        expect(await pn.isPerfect(10)).to.equal(false);
        expect(await pn.isPerfect(1)).to.equal(false);
    });
});
```

**Step 6: Create `index.html`**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8"><title>Perfect Number Checker</title>
    <style>
        body { font-family: Arial, sans-serif; background: #f1f5f9; display: grid; place-items: center; min-height: 100vh; margin: 0; }
        .app { background: white; padding: 30px; border-radius: 16px; width: 340px; text-align: center; box-shadow: 0 8px 30px #0001; }
        input, button { width: 100%; padding: 12px; margin-top: 12px; border-radius: 8px; box-sizing: border-box; font-size: 16px; }
        button { border: none; background: #4f46e5; color: white; cursor: pointer; }
    </style>
</head>
<body>
    <main class="app">
        <h1>Perfect Number Checker</h1>
        <p>Decentralized application using Ethereum</p>
        <button id="connect">Connect MetaMask</button>
        <p id="wallet">Wallet not connected</p>
        <form id="checker">
            <input id="number" type="number" min="1" placeholder="Enter a positive integer" required>
            <button type="submit">Check Number</button>
        </form>
        <h3 id="result">Result will appear here</h3>
    </main>
    <script type="module">
        import { BrowserProvider, Contract } from "https://cdn.jsdelivr.net/npm/ethers@6/+esm";
        const address = "0x5FbDB2315678afecb367f032d93F642f64180aa3";
        const abi = ["function isPerfect(uint256 n) pure returns (bool)"];
        let contract;

        document.getElementById("connect").onclick = async () => {
            const provider = new BrowserProvider(window.ethereum);
            await provider.send("eth_requestAccounts", []);
            const signer = await provider.getSigner();
            contract = new Contract(address, abi, signer);
            document.getElementById("wallet").textContent = "Connected: " + await signer.getAddress();
        };

        document.getElementById("checker").onsubmit = async (e) => {
            e.preventDefault();
            const n = BigInt(document.getElementById("number").value);
            const ok = await contract.isPerfect(n);
            document.getElementById("result").textContent = ok ? `${n} is a Perfect Number!` : `${n} is NOT a Perfect Number.`;
        };
    </script>
</body>
</html>
```

**Step 7: Run tests, start node, deploy, and launch web server**
```powershell
# Run automated unit tests:
npx hardhat compile
npx hardhat test

# Terminal 1 (Start local Hardhat blockchain):
npx hardhat node

# Terminal 2 (Deploy contract & serve index.html):
npx hardhat run scripts/deploy.js --network localhost
python -m http.server 8000
```
*(Open `http://localhost:8000` in your browser, connect MetaMask on Chain ID `31337`, enter `6`, and click **Check Number** $\rightarrow$ `6 is a Perfect Number!`.)*
