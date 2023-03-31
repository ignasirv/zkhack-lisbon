# zkHack Lisbon

Bounty: pool of 5k Matic for best rom and smart contracts optimization proposals.

### zk-ROM
Setup:
Download zk-rom
```
git clone https://github.com/0xPolygonHermez/zkevm-rom
cd zkevm-rom
npm i
```
Build rom

```
npm run build
```
At the repository, there are different tests for each EVM opcode implementation. They can be tested with the following commands:
```
node counters/counters-executor.js --test opAdd
```
The list of tests can be found at path counters/tests.
If you change the counters consumption, you will have to comment the following function of the test using `;` in order to see the consumption at the console:
```
checkCounters:
; %OPADD_STEP - STEP:JMPN(failedCounters)
; %OPADD_CNT_BINARY - CNT_BINARY :JMPNZ(failedCounters)
; %OPADD_CNT_ARITH - CNT_ARITH :JMPNZ(failedCounters)
; %OPADD_CNT_KECCAK_F - CNT_KECCAK_F :JMPNZ(failedCounters)
; %OPADD_CNT_MEM_ALIGN - CNT_MEM_ALIGN :JMPNZ(failedCounters)
; %OPADD_CNT_PADDING_PG - CNT_PADDING_PG :JMPNZ(failedCounters)
; %OPADD_CNT_POSEIDON_G - CNT_POSEIDON_G :JMPNZ(failedCounters)
```
Example of optimization:
```
 A               :MSTORE(bytesToStore)
                 :CALL(MSTORE32)
```
In the case above, we are using two steps to store value of registry A to `bytesToStore` and then, call function `MSTORE32`. This can optimize to only use one step:
```
 A               :MSTORE(bytesToStore),CALL(MSTORE32)
```
There are optimization as simple as this one, but there are other optimization far more complex that require more knowledge of the assembly language and of the EVM.

A way to parameterize the consumption of the rom is using the counters. Each rom operation is consuming different counters:
**Steps**: consumed for every execution line of the rom.
**Binary**: consumed every time a binary operation is executed:
- ADD
- SUB
- LT & SLT
- EQ
- AND
- OR 
- XOR

**Arithmetic**: 256 bits arithmetic operations.
- A*B + C = D*2^256 + E
- Range check of inputs and outputs.
- 32 CLOCKs per operation.
- Includes EC addition formulas for ECDSA multiplication.

**Keccak**: Consumed for every HASHKDIGEST operation
**Mem align**: Consumed for every memory alignment operation
**Poseidon**: Consumed for every HASHKDIGEST operation

CheatSheet zkEVM operations:
https://hackmd.io/VPA-DbmoTfyu1EqfjMHdNA

### Want to move to the next step? Apply for a critical bug and earn up to 1M USD!!
https://immunefi.com/bounty/polygonzkevm/

Useful links:
Audit reports:
https://github.com/0xPolygonHermez/zkevm-rom/tree/main/audits
https://github.com/0xPolygonHermez/zkevm-contracts/tree/main/audits
Smart contracts:
https://github.com/0xPolygonHermez/zkevm-contracts
