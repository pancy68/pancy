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
feat: implement emergency stop in Counter

Added an emergencyStop function that freezes all operations. Extra safety layer for contracts deployed on Base.
feat: allow Counter to be incremented by anyone or only owner

Added a toggle so the owner can decide if public increments are allowed. More flexible control for Base deployments.
feat: add event for ownership transfer in Counter

Emitted an OwnershipTransferred event whenever the owner changes. Improves transparency on Base explorers.
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract AdvancedCounter {
    address public owner;
    int256 public count;
    int256 public maxCount;
    int256 public step = 1;
    bool public stopped;

    event CountChanged(int256 newCount, address indexed caller);
    event EmergencyStop(address indexed caller);

    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }

    modifier notStopped() {
        require(!stopped, "Contract stopped");
        _;
    }

    constructor(int256 _maxCount) {
        owner = msg.sender;
        maxCount = _maxCount;
    }

    function increment() external notStopped {
        require(count + step <= maxCount, "Max reached");
        count += step;
        emit CountChanged(count, msg.sender);
    }

    function decrement() external notStopped {
        count -= step;
        emit CountChanged(count, msg.sender);
    }

    function setStep(int256 _step) external onlyOwner {
        require(_step > 0, "Step must be positive");
        step = _step;
    }

    function emergencyStop() external onlyOwner {
        stopped = true;
        emit EmergencyStop(msg.sender);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BasicCounter {
    address public owner;
    uint256 public count;

    constructor() {
        owner = msg.sender;
    }

    function increment() external {
        count++;
    }

    function reset() external {
        require(msg.sender == owner, "Not owner");
        count = 0;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleTodo {
    string public task;
    bool public completed;

    function setTask(string calldata _task) external {
        task = _task;
        completed = false;
    }

    function complete() external {
        completed = true;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract TimestampLocker {
    uint256 public value;
    uint256 public lockedAt;

    function lock(uint256 _value) external {
        value = _value;
        lockedAt = block.timestamp;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MinNumber {
    uint256 public number;

    function setIfHigher(uint256 _number) external {
        require(_number > number, "Too low");
        number = _number;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract TipJar {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    receive() external payable {}

    function withdraw() external {
        require(msg.sender == owner, "Not owner");
        payable(owner).transfer(address(this).balance);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Lockbox {
    address public owner;
    bool public unlocked;

    constructor() {
        owner = msg.sender;
    }

    function unlock() external {
        require(msg.sender == owner, "Not owner");
        unlocked = true;
    }

    receive() external payable {}
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DepositTracker {
    address public lastDepositor;
    uint256 public lastAmount;

    receive() external payable {
        lastDepositor = msg.sender;
        lastAmount = msg.value;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract LuckyNumber {
    uint256 public number;

    function choose(uint256 _number) external {
        number = _number;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract TimeLogger {
    uint256 public lastTime;

    function log() external {
        lastTime = block.timestamp;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract NumberList {
    uint256[] public numbers;

    function add(uint256 num) external {
        numbers.push(num);
    }

    function count() external view returns (uint256) {
        return numbers.length;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SenderTracker {
    address public lastCaller;

    function callMe() external {
        lastCaller = msg.sender;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract StringLength {
    string public text;

    function setText(string calldata _text) external {
        text = _text;
    }

    function getLength() external view returns (uint256) {
        return bytes(text).length;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract LastCaller {
    address public last;

    function update() external {
        last = msg.sender;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ActiveFlag {
    bool public active = true;

    function deactivate() external {
        active = false;
    }

    function activate() external {
        active = true;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PublicCounter {
    uint256 public count;

    function increment() external {
        count += 1;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract EmptyChecker {
    function isEmpty(string calldata text) external pure returns (bool) {
        return bytes(text).length == 0;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Presence {
    mapping(address => bool) public hasInteracted;

    function interact() external {
        hasInteracted[msg.sender] = true;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract InteractionLogger {
    uint256 public lastInteraction;

    function interact() external {
        lastInteraction = block.timestamp;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract MinOfTwo {
    function min(uint256 a, uint256 b) external pure returns (uint256) {
        return a < b ? a : b;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ChainId {
    function getChainId() external view returns (uint256) {
        return block.chainid;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BlockSaver {
    uint256 public savedBlock;

    function save() external {
        savedBlock = block.number;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract TimeDiff {
    function diff(uint256 a, uint256 b) external pure returns (uint256) {
        return a > b ? a - b : b - a;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract LessOrEqual {
    function check(uint256 a, uint256 b) external pure returns (bool) {
        return a <= b;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract IsContract {
    function check(address addr) external view returns (bool) {
        return addr.code.length > 0;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract StepCounter {
    uint256 public count;
    uint256 public step = 1;

    function setStep(uint256 _step) external {
        step = _step;
    }

    function increment() external {
        count += step;
    }
}
feat: basic Value swapper

Swaps two stored numbers.
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract OnceLocker {
    uint256 public value;
    bool private locked;

    function lockValue(uint256 _value) external {
        require(!locked, "Already locked");
        value = _value;
        locked = true;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract NumberCloner {
    uint256 public original;
    uint256 public copy;

    function set(uint256 value) external {
        original = value;
    }

    function clone() external {
        copy = original;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract IncrementTwice {
    uint256 public count;

    function inc() external {
        count += 2;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ValueToggler {
    uint256 public value;

    function toggle() external {
        value = value == 0 ? 1 : 0;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Decreaser {
    uint256 public count = 10;

    function decrement() external {
        require(count > 0, "Already zero");
        count--;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Accumulator {
    uint256 public total;

    function add(uint256 amount) external {
        total += amount;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleCounter {
    uint256 public count;

    function inc() external {
        count++;
    }

    function get() external view returns (uint256) {
        return count;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ViewCounter {
    uint256 private count;

    function increment() external {
        count++;
    }

    function current() external view returns (uint256) {
        return count;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Renounceable {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    function renounce() external {
        require(msg.sender == owner, "Not owner");
        owner = address(0);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DualCounter {
    uint256 public countA;
    uint256 public countB;

    function incA() external {
        countA++;
    }

    function incB() external {
        countB++;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract LastBlockHash {
    bytes32 public lastHash;

    function update() external {
        lastHash = blockhash(block.number - 1);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract LeftShift {
    function shift(uint256 value, uint256 positions) external pure returns (uint256) {
        return value << positions;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract UintToBytes32 {
    function convert(uint256 value) external pure returns (bytes32) {
        return bytes32(value);
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract NumberHash {
    function hash(uint256 number) external pure returns (bytes32) {
        return keccak256(abi.encodePacked(number));
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BlockNumberAdd {
    function add(uint256 number) external view returns (uint256) {
        return block.number + number;
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract CustomError {
    error InvalidValue();

    function check(uint256 value) external pure {
        if (value == 0) revert InvalidValue();
    }
}
