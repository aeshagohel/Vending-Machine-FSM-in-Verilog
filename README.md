# Vending Machine Verilog Module

This repository contains a Verilog implementation of a simple vending machine that accepts coins and dispenses products accordingly.

### Description

* The module vending_machine simulates a vending machine that:
  
* Accepts two types of coins: ₹5 and ₹10
* Dispenses a product when the required amount is reached
* Returns coins if the inserted amount exceeds the product price
* Uses a finite state machine (FSM) to track inserted coins and manage outputs

### Inputs and Outputs
Inputs
| Name  | Width | Description                              |
| ----- | ----- | ---------------------------------------- |
| clk   | 1     | Clock signal                             |
| reset | 1     | Reset signal (active high)               |
| coin  | 2     | Coin inserted: 00=no coin, 01=₹5, 10=₹10 |

### Outputs
| Name        | Width | Description                        |
| ----------- | ----- | ---------------------------------- |
| dispense    | 1     | High when the product is dispensed |
| return_coin | 1     | High when a coin is returned       |

### State Encoding
| State | Encoding | Description                      |
| ----- | -------- | -------------------------------- |
| S0    | 00       | Initial state, no coins inserted |
| S5    | 01       | ₹5 inserted                      |
| S10   | 10       | ₹10 inserted                     |
| S15   | 11       | ₹15 inserted (overpayment)       |

### Functionality

S0 (No Coin)

* ₹5 inserted → move to S5
* ₹10 inserted → move to S10 and dispense product

S5 (₹5 inserted)

* ₹5 inserted → move to S10 and dispense product
* ₹10 inserted → move to S15, dispense product, and return ₹5

S10 (₹10 inserted)

* Dispense product and reset to S0

S15 (₹15 inserted)

* Dispense product, return ₹5, and reset to S0

### Usage

1.Instantiate the module in your testbench:
```verilog 
// Instantiate the vending machine module
vending_machine vm (
    .clk(clk),            // Clock input
    .reset(reset),        // Reset input (active high)
    .coin(coin),          // Coin input: 00 = no coin, 01 = ₹5, 10 = ₹10
    .dispense(dispense),  // Output: goes high when product is dispensed
    .return_coin(return_coin) // Output: goes high when excess coin is returned
);
```

2.Apply clock, reset, and coin inputs according to your simulation requirements.
3.Observe dispense and return_coin outputs to verify functionality

### Notes

* The module uses synchronous reset.
* State transitions and outputs are defined using a finite state machine (FSM).
* Overpayments are handled by returning excess coins automatically.
