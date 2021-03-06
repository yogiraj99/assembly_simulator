[![CircleCI](https://circleci.com/gh/step/assembly_simulator.svg?style=shield)](https://circleci.com/gh/step/assembly_simulator)
[![npm version](https://badge.fury.io/js/%40craftybones%2Fassembly_simulator.svg)](https://badge.fury.io/js/%40craftybones%2Fassembly_simulator)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

# Assembly Simulator

This project is a small simulator that combines BASIC like line numbers and assembly like instructions to help teach the basics of computing.


1. [Installation](#installation)
2. [Usage](#usage)
3. [Architecture](#architecture)
4. [Instruction Set](#instruction-set)
5. [Documentation](#documentation)
6. [Error Handling](#error-handling)

### Installation

`npm install https://github.com/step/assembly_simulator.git`


### Usage

```javascript
const Machine = require('assembly_simulator');
machine = new Machine();
machine.load('10 START\n20 PRN "HELLO"\n30 STOP');
machine.execute();
console.log(machine.getPrn().join("\n"));
```

For detailed instructions, see [documentation](#documentation)

### Architecture

The "Machine" has 4 registers `A, B, C, D`.

There are 4 flags that are set on the compare instruction.

1. EQ - this is set when two values are equal
2. NE - this is set when two values are not equal
3. GT - this is set when the value of the first argument is greater than the second one.
4. LT - this is set when the value of the first argument is lesser than the second one.

The "Machine" only recognises positive integers for now.

There is also a stack that you can `PUSH` and `POP` from.

### Instruction Set

0. **Comments**

   Comments can be entered by starting a line with `;`. The first non-space character has to be a `;` for it to be considered a comment.

   Valid uses
   ```
   ; This is a comment
      ; this is also a comment
   ```

   Invalid uses
   ```
   10 START ; comments cannot be placed after an instruction
   this is also not a comment
   ```
1. **START**

   Starts execution and resets flags. Used at the start of a program, often as `10 START`.


2. **MOV**

   Copies a value into a register. First argument has to be a register. Second argument can either be a register or a positive integer.

   Valid uses
   ```
   10 MOV A,2   ;; Copies 2 into A
   20 MOV B,A   ;; Copies A's value into B.
   ```

   Invalid uses
   ```
   10 MOV 2,2
   20 MOV 2,A
   ```

3. **ADD**

   Adds a value with a register and copies the new value into the register. First argument has to be a register. Second argument can either be a register or a positive integer.

   Valid uses
   ```
   10 ADD A,2   ;; Adds 2 to A and puts it back in A
   20 ADD B,A   ;; Adds A to B and puts it back in B
   ```
   Invalid uses
   ```
   10 ADD 2,2
   20 ADD 2,A
   ```
4. **SUB**

   Subtracts a value from a register and copies the new value into the register. First argument has to be a register. Second argument can either be a register or a positive integer.

   Valid uses
   ```
   10 SUB A,2   ;; Subtracts 2 from A and puts it back in A
   20 SUB B,A   ;; Subtracts A from B and puts it back in B
   ```
   Invalid uses
   ```
   10 SUB 2,2
   20 SUB 2,A
   ```

   Note that one can subtract a greater value from a lesser value and get a negative number even though one can't directly enter negative numbers.

5. **CMP**

   The compare instruction sets flags. Based on the arguments it sets one or more flags. The first argument has to be a register. The second argument can be either a positive integer or a register.

   `CMP A,10` will set the `EQ` flag if A is 10.

   `CMP A,10` will set the `NE` and `GT` flag if A is 12.

   `CMP A,10` will set the `NE` and `LT` flag if A is 8.

   Valid uses
   ```
   10 CMP A, 2  ;; will set LT if A<2 or GT if A>2
   20 CMP A, B  ;; will set LT if A<B or GT if A>B
   ```

   Invalid uses
   ```
   10 CMP 2,2
   20 CMP 2,A
   ```
6. **JMP**

   The `JMP` instruction unconditionally continues execution from the line specified. `JMP` and all other jump instructions leave the registers and flags unmodified. `JMP` takes one argument and that argument has to be a line number.

   Valid uses
   ```
   70 JMP 80
   70 JMP 60
   ```

   Invalid uses
   ```
   10 JMP A
   20 JMP "10"
   ```

7. **JE**

   `JE` is a conditional jump instruction that only jumps to a specified line if the `EQ` flag is set. If `EQ` is not set, then the execution continues normally.

   Valid uses
   ```
   10 JE 50
   20 JE 10
   ```

   Invalid uses
   ```
   10 JE A
   20 JE "10"
   ```

8. **JNE**

   `JNE` is a conditional jump instruction that only jumps to a specified line if the `NE` flag is set. If `NE` is not set, then the execution continues normally.

   Valid uses
   ```
   10 JNE 50
   20 JNE 10
   ```

   Invalid uses
   ```
   10 JNE A
   20 JNE "10"
   ```

9. **JGT**

   `JGT` is a conditional jump instruction that only jumps to a specified line if the `GT` flag is set. If `GT` is not set, then the execution continues normally.

   Valid uses
   ```
   10 JGT 50
   20 JGT 10
   ```

   Invalid uses
   ```
   10 JGT A
   20 JGT "10"
   ```

10. **JGE**

   `JGE` is a conditional jump instruction that only jumps to a specified line if the `GT` or `EQ` flags are set. If `GT` or `EQ` are not set, then the execution continues normally.

   Valid uses
   ```
   10 JGE 50
   20 JGE 10
   ```

   Invalid uses
   ```
   10 JGE A
   20 JGE "10"
   ```

11. **JLT**

   `JLT` is a conditional jump instruction that only jumps to a specified line if the `LT` flag is set. If `LT` is not set, then the execution continues normally.

   Valid uses
   ```
   10 JLT 50
   20 JLT 10
   ```

   Invalid uses
   ```
   10 JLT A
   20 JLT "10"
   ```

12. **JLE**

   `JLE` is a conditional jump instruction that only jumps to a specified line if the `LT` or `EQ` flags are set. If `LT` or `EQ` are not set, then the execution continues normally.

   Valid uses
   ```
   10 JLE 50
   20 JLE 10
   ```

   Invalid uses
   ```
   10 JLE A
   20 JLE "10"
   ```

13. **STOP**

   The `STOP` instruction halts execution entirely. It does not take an argument.

   Valid uses
   ```
   100 STOP
   ```

   Invalid uses
   ```
   100 STOP 10
   100 STOP A
   ```

14. **PUSH**
   
   The `PUSH` instruction pushes the value of a register onto the stack. It takes exactly one argument that has to be a register.

   Valid uses
   ```
   10 PUSH A
   20 PUSH B
   ```

   Invalid uses
   ```
   10 PUSH
   20 PUSH 20
   30 PUSH A,B
   ```

15. **POP**
   
   The `POP` instruction pops the stack into the register specified. It takes exactly one argument that has to be a register.

   Valid uses
   ```
   10 POP A
   20 POP B
   ```

   Invalid uses
   ```
   10 POP
   20 POP 20
   30 POP A,B
   ```

16. **FUNC**

   The `FUNC` instruction is a special instruction. It has no effect on execution and leaves flags and registers unchanged. It is used to indicate the beginning of a function. It takes one argument that has to be a valid function name.

   A valid function name begins with an alphabet but can consequently contain alphabets or numbers.

   Function names are case insensitive. So, `mul` is the same as `MUL`. 

   The `RET` instruction is used to return to the point of execution from where the previous call was invoked.

   Valid uses

   ```
   10 FUNC MUL
   20 ADD A,A
   30 RET
   ```

   Invalid uses

   ```
   10 FUNC 1MUL
   20 FUNC "ABCD"
   30 FUNC A,B
   ```

17. **CALL**

   The `CALL` instruction calls the given function. It takes exactly one argument which has to be a valid function name.

   A valid function name begins with an alphabet but can consequently contain alphabets or numbers.

   Valid uses
   ```
   70 CALL MUL
   80 CALL GREATEST
   ```

   Invalid uses
   ```
   70 CALL 
   80 CALL 1MUL
   ```

18. **RET**

   The `RET` instruction is used to return to the point of execution from where `CALL` was last called from. `RET` is usually the last instruction executed in a function. The instruction accepts no arguments.

   Valid uses
   ```
   30 RET
   ```

   Invalid uses
   ```
   30 RET A
   40 RET 20
   50 RET "ABC"
   ```

### Documentation

   The simulator can be accessed and controlled using the `Machine` object. The machine object is instantiated as

   ```javascript
   const machine = new Machine();
   ```

   To load a program into the machine, use the `load` method.

   ```javascript
   let program = fs.readFileSync("add.asm","utf8");
   machine.load(program);
   ```

   To execute a program in the machine, use the `execute` method.
   ```javascript
   let program = fs.readFileSync("add.asm","utf8");
   machine.load(program);
   machine.execute();
   ```

   Once the machine has finished execution, you can get all the output that has been generated using the `PRN`  instruction by using the method `getPrn`.

   ```javascript
   // code to load and execute here
   let lines = machine.getPrn();
   console.log(lines.join("\n"));
   ```

   `getPrn` returns an array of lines. This is preferred instead of giving a string as one may wish to use this library either on the command line or on the frontend where having separate lines available makes it easy to format/render


   You can also get the trace table of the execution by using the `getTable` method
   ```javascript
   // code to load and execute here
   let table = machine.getTable();
   table.forEach(({CL,NL,A,B,C,D,EQ,NE,GT,LT,SL,INST,PRN})=>{
     console.log([CL,NL,SL,INST,A,B,C,D,EQ,NE,GT,LT,PRN],join(", "));
   })
   ```

   This table is very useful to help debug and trace execution. Each row of the table consists of:

   1. The registers `A`, `B`, `C` and `D`
   2. The flags `EQ`,`NE`,`GT` and `LT`
   3. Print output from the `PRN` instruction.
   4. The line number from the source file or program that was executed in `SL`
   5. The actual instruction being executed in `INST`
   6. The stack as an array contained in `STK`

   
#### Stepwise Execution

The Machine has the capability of executing stepwise. This feature allows one to 'step through' the program. 

```javascript
const machine = new Machine();
let program = fs.readFileSync("add.asm","utf8");
let callBack = (state) => {
  let { A, B, C, D } = state;
  let { EQ, NE, GT, LT } = state;
  let { PRN } = state;
  let { CL, NL } = state;
  let { SL, INST } = state;
  let { STK } = state;
  console.log(`A : ${A}, B : ${B}, C : ${C}, D : ${D}`);
  console.log(`EQ : ${EQ}, NE : ${NE}, GT : ${GT}, LT : ${LT}`);
  console.log(`CL : ${CL}, NL : ${NL}`);
  console.log(`SL : ${SL}, INST : ${INST}`);
  console.log(`INST : ${INST}`);
  console.log(`STK : ${STK.join(" ")}`);
} 
let executor;
try {
  machine.load(program);
  executor = machine.executeStepWise();
} catch(e) {
  console.log("Error on ", e.lineNumber, e.instruction);
}

try {
  machine.nextStep();
  machine.nextStep();
} catch(e) {
  // do whatever with the error
}
```

As shown above, in order to use it, you are required to pass a callback. The callback will be called with the same fields that `getTable` fetches. The important thing to note here is that once the machine executes the program completely, the callback will no longer be called no matter how many times you call `nextStep`.

This manner of execution is very useful to debug things like infinite loops.

### Error Handling

All errors currently are encapsulated by the `InvalidInstructionException` class. All exceptions have two pieces of information currently.

1. Source line number where the exception occurred in `lineNumber`
2. The offending instruction in `instruction`

Always surround your machine's `load` and `execute` in try catch blocks.

```javascript
try {
  machine.load(program);
  machine.execute();
} catch(e) {
  console.log("Error on ", e.lineNumber, e.instruction);
}
```
