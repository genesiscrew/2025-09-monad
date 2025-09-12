# Monad audit details
- Total Prize Pool: $504,000 in USDC
  - Warden pool: $500,000 in USDC
    - HM awards: up to $480,000 in USDC 
      - If no valid Highs or Mediums are found, the HM pool is $0 
    - QA awards: $20,000 in USDC
  - Judge awards: $3,500 in USDC
  - Scout awards: $500 in USDC
- [Read our guidelines for more details](https://docs.code4rena.com/competitions)
- Starts September 15, 2025 20:00 UTC 
- Ends October 12, 2025 20:00 UTC

**❗ Important notes for wardens** 

1. While this audit's code is not yet deployed, a variation of the ["live criticals" exception](https://docs.code4rena.com/awarding#the-live-criticals-exception) will apply:
   - Sponsors may choose to fix High-severity findings during the submission phase of the audit. 
   - Wardens are encouraged to submit High-severity submissions promptly, to guarantee payout in the case where [a sponsor patches a finding during the audit](https://docs.code4rena.com/awarding#the-live-criticals-exception).
   - If a fix is planned, C4 staff will post a notification for wardens in the audit channel, to provide as much advance notice as possible and encourage wardens to submit any in-progress work.
   - Once the fix has been applied, it will be added to the `Publicly Known Issues` section.
2. Prior to receiving payment for this audit, you MUST become a [Certified Contributor](https://docs.code4rena.com/roles/sr-wardens#certified-contributors) (successfully complete KYC).
  - You do not have to be certified prior to submitting bugs, but you must successfully complete the certification process within 30 days of the award announcement in order to receive awards. 
  - This applies to all audit participants including wardens, teams, judges and scouts.
  - C4 staff will contact all relevant participants after awards are announced, to assist with this process.
3. Judging phase risk adjustments (upgrades/downgrades):
  - High- or Medium-risk submissions downgraded by the judge to Low-risk (QA) will be ineligible for awards.
  - Upgrading a Low-risk finding from a QA report to a Medium- or High-risk finding is not supported.
  - As such, wardens are encouraged to select the appropriate risk level carefully during the submission phase.

## Automated Findings / Publicly Known Issues

The 4naly3er report can be found [here](https://github.com/code-423n4/2025-09-monad/blob/main/4naly3er-report.md).

_Note for C4 wardens: Anything included in this `Automated Findings / Publicly Known Issues` section is considered a publicly known issue and is ineligible for awards._

The following issues and risks are considered **known** or **by design** and will not be considered valid findings in this contest.

### General Risks

- **Attacks on the Consensus Trust Model:** The security of the Monad protocol relies on a fundamental assumption of a supermajority of stake weight being non-faulty. Therefore, any attack scenario requiring more than 1/3 of the network's stake to be malicious is considered a fundamental, out-of-scope attack on the protocol's trust model and will not be considered a valid finding. This includes, but is not limited to, scenarios resulting in network finality failures, double-spending, or long-range attacks.
- **Validator/Proposer Malicious Behavior:** Reports related to attack scenarios by a malicious validator or proposer that are not economically rational - i.e. where the attacker cannot profit or the attack would cost more than any potential profit - are out of scope.
- **Naive Attacks on Cryptographic Primitives:** The utilized cryptographic primitives were selected due to their widely accepted hardness. Attacks requiring their breakage are out of scope unless they provide clear indices how the utilized algorithm outperforms existing attacks.
- **Validator/Stake Centralization:** This is an inherent social and economic dynamic of a PoS system, not a protocol bug.
- **Transaction Censorship:** The theoretical collusion of a small number of malicious validators to censor transactions is an acceptable risk and is addressed by social and economic countermeasures, not as a protocol vulnerability.
- **Economic Attacks:** Any attack that relies on an attacker spending a prohibitively high amount of gas or network resources (e.g., gas price manipulation or DoS via gas griefing) is considered an acceptable economic attack and is out of scope.
- **Insecure and/or Inappropriate Node Configuration:** Findings related to improperly configured nodes are considered out of scope, as it is the node operator's responsibility to ensure correct setup.
- **Open Issues:** Any open issues in our public trackers at the start of the contest are considered known risks and will not constitute a valid finding.
- **Reporting and Performance Metrics:** Telemetry and performance reporting failures are out of scope for bug reports, unless they enable a severe vulnerability like remote code execution (RCE) or a consensus node crash on a default configuration.
  - These reports will only be considered in scope for QA submissions.
- **General DDoS attacks:** such as amplification, reflection, or flooding, that impact node or network availability but do not exploit protocol- or implementation-specific weaknesses.
- **UI/UX Security:** Security-related bugs in the user interface or user experience, such as sensitive information in logs or password brute force vulnerabilities, are out of scope as this contest is focused on the core protocol's security.
- **Insecure Dependencies:** Findings concerning insecure dependencies with known vulnerabilities are out of scope. The contest is focused on the custom Monad protocol code, not third-party libraries.
- **Missing Best Practices:** Findings related to missing general security best practices for API endpoints, such as the lack of security flags for HTTP-based APIs or rate limiting, are out of scope.
- **Social Engineering:** Any attack that relies on social engineering is out of scope and will not be considered a valid finding.
- **Physical Attacks:** Any attack that requires physical access to a node or its hardware is out of scope. This contest is limited to the security of the Monad protocol's software implementation.
- **Local attacks against nodes:** Any attack against the software running on a node that requires the attacker to have prior access to that node. This contest focuses on attacks that are possible against the components exposed over the network.
- **Missing Optimisations:** Findings that present a potential performance improvement in client code are out of scope, unless that improvement mitigates a viable attack scenario.
- **C/C++ Undefined Behavior:** Findings related to C/C++ undefined behavior are considered low severity unless a practical, demonstrable impact can be shown in a node’s default configuration. The mere existence of undefined behavior, without a clear and practical exploit, will not be considered a higher than low severity finding.

### Monad-Specific 

- **EVM/EIP Deviation**:
  - **Gas Limit & Gas Pricing:** Monad's gas limit and gas pricing model do not strictly follow Ethereum's EIPs (such as EIP-1559). Any finding related to these intended deviations is out of scope, unless the issue can be used to achieve a form of DoS.
  - **Max Contract Size:** The maximum contract size is 128 KB, which is larger than Ethereum's 24.5 KB limit. This is by design and is not considered a bug.
  - **Mempool:** There is no global mempool. Transactions are forwarded directly to a subset of future leaders. Any findings based on the assumption of a global mempool or the assumption of private mempool access are considered out of scope.
  - **Historical Data:** Full nodes do not store arbitrary historical state due to high throughput and storage requirements. Reports related to the inability to access full historical state from a standard full node are out of scope.
  - **VM Execution Trace & Error Codes:** Due to natural constraints of JIT execution, the execution trace and, therefore, EVM error codes of the VM (while executing via JIT compilation) are not guaranteed to be the exact same as major interpreted EVM implementations. Reports related to this are out of scope.
- **RPC Compliance:** The RPC is designed to be generally Ethereum-compliant but may have minor deviations due to Monad's unique adaptations. Reports on these deviations will not be considered valid findings.
  - See intentional differences:
    - https://docs.monad.xyz/reference/
- **Statesync Peer Trust Model:** Statesync peers are assumed to be trusted. Findings demonstrating a malicious statesync peer are therefore out of scope.
- **Hardware and Operating System Performance:** Monad is a high-performance system designed to run on specific hardware and operating system requirements. Any performance degradation, synchronization issues, or consensus failures that result from a node operating on hardware or an operating system below the recommended specifications are considered out of scope. The security and performance model assumes that node operators will meet or exceed the minimum hardware and operating system requirements outlined in the official documentation.
- **RaptorCast protocol DoS:** findings that rely solely on exhausting cryptographic computation in the RaptorCast transport (e.g., forcing excessive handshakes or signature verifications) and do not exploit a protocol- or implementation-specific flaw are also out of scope.
- **Attacks requiring an infeasible amount of MON:** MON (Monad’s native token) issuance will be limited and therefore reports requiring large amounts of MON (in the ranges of `type(uint256).max`) are not in scope.
- **Slashing:** findings related to malicious validator behavior that would be mitigated by slashing is not in scope.

### Existing issues

Any GitHub issues raised prior to the start of the audit contest found in the following two repos are considered out of scope. This also applies to any newly discovered vulnerabilities that are a direct result of the same underlying root cause as a known, open issue.

- https://github.com/category-labs/monad-bft/issues
- https://github.com/category-labs/monad/issues

# Overview

[ ⭐️ SPONSORS: add info here ]

## Links

- **Previous audits:**  
  - ✅ SCOUTS: If there are multiple report links, please format them in a list.
- **Documentation:** https://docs.monad.xyz
- **Website:** https://www.monad.xyz
- **X/Twitter:** https://x.com/monad

---

# Scope

[ ✅ SCOUTS: add scoping and technical details here ]

### Files in scope
- ✅ This should be completed using the `metrics.md` file
- ✅ Last row of the table should be Total: SLOC
- ✅ SCOUTS: Have the sponsor review and and confirm in text the details in the section titled "Scoping Q amp; A"

*For sponsors that don't use the scoping tool: list all files in scope in the table below (along with hyperlinks) -- and feel free to add notes to emphasize areas of focus.*

| Contract | SLOC | Purpose | Libraries used |  
| ----------- | ----------- | ----------- | ----------- |
| [contracts/folder/sample.sol](https://github.com/code-423n4/repo-name/blob/contracts/folder/sample.sol) | 123 | This contract does XYZ | [`@openzeppelin/*`](https://openzeppelin.com/contracts/) |

### Files out of scope
✅ SCOUTS: List files/directories out of scope

Any code not reachable via a running node under default configuration (e.g., test, mock or fuzzing code) is out of scope. Any project, development, build configuration or build, scripting or other miscellaneous development or testing files are out of scope. 

Findings concerning known vulnerabilities within third-party dependencies (‘**/third_party/*’) themselves are out of scope. However, a bug is considered a valid finding if it leads to a protocol-level vulnerability.

Any proof of concepts must demonstrate how the report is triggerable by an attacker, outlining the full attack path on a default node configuration, which exploits in-scope code.

# Additional context

## Areas of concern (where to focus for bugs)

The team's primary concerns for the Monad protocol are focused on the following components:

- **Consensus Safety:** The stability of the MonadBFT consensus mechanism is paramount.
    -	Malicious validator activity and its potential to disrupt the consensus process.
    -	General safety or liveness issues and disruption of the consensus process that can arise from a byzantine validator activity (within protocol assumptions of < 1/3 stake)
    -	The possibility of a faulty node causing a chain split or incorrect finality.
    -	Exploitation of the leader election process for unfair advantage.
-	**Networking Layer (RaptorCast):** The custom message delivery protocol is a potential attack surface.
    -	Abuse of the broadcast tree by a malicious node to delay block propagation.
    -	Denial-of-Service (DoS) attacks via spamming with invalid or malformed RaptorCast chunks.
    -	Vulnerabilities in the signature and Merkle tree verification logic.
    -	Incorrect encoding or decoding of messages, or tampering of messages
    -	Exploitation of the local mempool and transaction forwarding to create race conditions.
-	**Parallel Execution and State Integrity:** Parallel execution is a core innovation and a critical area of concern.
    -	Potential for non-deterministic outcomes or state mismatches between nodes.
    -	Accurate and robust read/write set analysis.
    -	Risk of non-deterministic outcomes from asynchronous execution.
-	**Transaction and Fee Model:** The custom gas model requires scrutiny.
    -	Manipulation of the gas limit for DoS attacks.
    -	Correctness and resistance to overflow/underflow in fee calculations.
    -	Correct handling of unexpected state reversions.
-	**MonadDB Integrity:** The custom database (`MonadDB`) is essential for state management.
    -	Corruption or bypassing of database integrity checks.
    -	Race conditions in parallel state access.
-	**JIT VM and Interpreter:** The Just-In-Time (JIT) compiler is a highly sensitive component.
    -	Differences in execution output between interpreted and JIT-compiled EVM bytecode.
    -	Potential for a malicious contract to achieve **remote code execution (RCE)**.
    -	Data persistence or corruption in the cache after a transaction rollback.

## Main invariants

-	**Consensus:** The network must achieve consensus on a single, canonical block order if a supermajority of stake weight is non-faulty. All honest nodes must arrive at the exact same committed final state.
-	**Chain finality:** Speculative execution results of a previous block will be finalized within the designated system parameters if a supermajority of nodes are participating in consensus
-	**Execution:** The outcome of any transaction, including failure, is deterministic regardless of the execution path (interpreter or JIT). The final state of a block must be identical across all nodes.
-	**State Integrity:** The blockchain state must be consistent and verifiable at all block heights. The `merkle root` from a delayed block D must be agreed upon by all nodes to prevent state divergence. The total native token supply shall not exceed the maximum configured supply.
-	**Transactions:** The total token deduction from a sender's balance must be equal to the sum of `value` and the product of `gas_price` and `gas_limit`. A transaction's `max_expenditure` shall not exceed the sender's available balance at the time of consensus.
-	**Networking:** All RaptorCast chunks must have a valid signature from the originator. The network must guarantee reliable message delivery to all non-faulty nodes if a supermajority of stake weight is non-faulty.
-	**EVM Compatibility:** Aside from the known deviations mentioned, execution of a transaction should conform to existing EVM specifications.

## All trusted roles in the protocol

- **StateSync Peer**	
  -	**Purpose:** Trusted nodes that provide state synchronization data during initial sync or catch-up 
  -	**Who can be a StateSync Peer:** Nodes explicitly configured in the `state_sync_peers` list, which defaults to bootstrap nodes if none specified

✅ SCOUTS: Please format the response above 👆 using the template below👇

| Role                                | Description                       |
| --------------------------------------- | ---------------------------- |
| Owner                          | Has superpowers                |
| Administrator                             | Can change fees                       |

## Running tests

`monad-bft`:

```
git clone https://github.com/category-labs/monad-bft.git && cd monad-bft
git submodule update --init --recursive

cd monad-<insert crate name>
cargo test
```
Note: Individual crate tests can be run from each crate as standard

To run a consensus client + execution client + JsonRPC:
1.	cd docker/single-node
2.	nets/run.sh


`monad`:

See this README.md for more detailed information.
```
git clone https://github.com/category-labs/monad.git && cd monad
git submodule update --init --recursive

# Whole project tests
./scripts/configure.sh
./scripts/build.sh
./scripts/test.sh

# VM Specific tests (after running the project tests above)
./build/test/vm/unit/vm-unit-tests
```

**Additional Comments**

The codebase uses features of the Linux kernel and makes use of instructions specific to x86 such as AVX. It further requires Rust 1.87 and Clang 19 or newer.

For more information on build environment configuration, you can refer to the docker build configurations: https://github.com/category-labs/monad/blob/8cd73a6186366fe52529ab1050b2e6a388af4893/docker/release.Dockerfile#L1-L87 

✅ SCOUTS: Please format the response above 👆 using the template below👇

```bash
git clone https://github.com/code-423n4/2023-08-arbitrum
git submodule update --init --recursive
cd governance
foundryup
make install
make build
make sc-election-test
```
To run code coverage
```bash
make coverage
```

✅ SCOUTS: Add a screenshot of your terminal showing the test coverage

## Miscellaneous
Employees of Category Labs or Asymmetric Research, and employees' family members are ineligible to participate in this audit.

Code4rena's rules cannot be overridden by the contents of this README. In case of doubt, please check with C4 staff.
