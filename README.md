# APB_RAM_Design_UVM_Verification

## Overview
This project is a complete **APB RAM peripheral design and verification** done using **SystemVerilog and UVM**. The goal was to build a realistic memory-mapped slave that follows the **AMBA APB protocol** and then verify it using a proper UVM testbench, just like it would be done in an SoC environment.

The RAM supports normal read and write operations, checks for invalid addresses, and reports errors using `PSLVERR`. The verification environment uses both **directed** and **constrained-random tests** to make sure the design behaves correctly under different scenarios.

---

## What this project includes

### APB RAM Design
The RTL implements an **APB3-compliant RAM slave** with:
- Read and write support
- Address decoding and range checking
- Error reporting through `PSLVERR`
- A simple FSM that handles the **IDLE → SETUP → ACCESS** flow of the APB protocol  
- Parameterized memory depth and data width so it is easy to scale

---

### UVM Verification Environment
A full **UVM testbench** was written to verify the RAM. It includes:
- APB transactions and sequences
- Driver and monitor to interact with the bus
- A scoreboard to check data correctness
- Functional and code coverage
- Protocol checking for proper APB handshaking

The tests cover:
- Back-to-back reads and writes  
- Random address and data accesses  
- Invalid address cases  
- Reset behavior  
- Error (`PSLVERR`) generation  

---

## Project Structure

APB_RAM_UVM/
│
├── DUT.sv          # APB RAM RTL (Design Under Test)
├── testbench.sv   # UVM-based verification environment
├── README.md

---

## Design Overview
The APB RAM is built around a small **finite state machine (FSM)** that tracks the APB phases.  
`PSEL` and `PENABLE` are used to detect when a transfer starts and when it is valid, and based on `PWRITE`, the RAM either stores `PWDATA` or returns data for a read.

If an address is outside the valid memory range, the design asserts `PSLVERR` to indicate an illegal access.

---

## Verification Strategy
The UVM testbench checks both **functionality and protocol correctness**.  
Every transaction sent by the driver is monitored and compared in the scoreboard, so any data mismatch or protocol error is caught immediately.

Functional coverage is used to make sure:
- All address ranges are exercised  
- Both reads and writes occur  
- Error cases are tested  
- Different APB timing situations are covered  

This helps ensure that the design is not just working for a few test cases, but is actually robust.

---
