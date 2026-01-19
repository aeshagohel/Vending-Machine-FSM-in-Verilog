# Vending Machine Using Verilog HDL
## 📘 Project Overview

This project implements a Vending Machine Controller using Verilog HDL based on the concept of a Finite State Machine (FSM).
The system accepts different coin inputs and performs actions such as dispensing the product and returning extra change when required.
This project helps in understanding digital design, FSM modeling, and basic VLSI concepts through practical implementation.

## ❗ Problem Statement

A vending machine must correctly handle multiple coin inputs and make decisions based on the total inserted amount.
The challenge is to design a hardware-based controller that can:
* Detect coin inputs
* Track the inserted amount
* Dispense the product at the correct time
* Return extra money if excess amount is inserted
* The goal is to design this control logic using Verilog HDL.

## 🎯 Objective

* To design a vending machine using FSM approach
* To implement the design using Verilog HDL
* To accept ₹5 and ₹10 coins
* To dispense product once required amount is reached
* To return change if extra amount is inserted
* To analyze the design using RTL and simulation waveform

## 🧠 FSM Description

The vending machine is modeled using four states, each representing the total inserted amount.
| State | Description        |
| ----- | ------------------ |
| S0    | Initial state (₹0) |
| S5    | ₹5 inserted        |
| S10   | ₹10 inserted       |
| S15   | ₹15 inserted       |

Product cost = ₹10

## ⚙️ Working Description

1. System starts in S0 (₹0)
2. When ₹5 is inserted, it moves to S5
3. When ₹10 is inserted:
*  Product is dispensed
 4. If ₹15 is inserted:
   * Product is dispensed
* Extra ₹5 is returned
5. After dispensing, the system resets back to S0

## 🧪 Inputs and Outputs
### Inputs
* clk – Clock signal
* reset – Reset signal
* coin[1:0]
* 00 → No coin
* 01 → ₹5
* 10 → ₹10

### Outputs
* dispense – Dispenses product
* return_coin – Returns extra coin

## 💻 Verilog Code
```verilog 
module vending_machine(
    input clk,
    input reset,
    input [1:0] coin,     // 00: no coin, 01: ₹5, 10: ₹10
    output reg dispense,
    output reg return_coin
);

    // State encoding
    parameter S0  = 2'b00;   // ₹0
    parameter S5  = 2'b01;   // ₹5
    parameter S10 = 2'b10;   // ₹10
    parameter S15 = 2'b11;   // ₹15

    reg [1:0] state, next_state;

    // State register
    always @(posedge clk or posedge reset) begin
        if (reset)
            state <= S0;
        else
            state <= next_state;
    end

    // Next state and output logic
    always @(*) begin
        dispense = 0;
        return_coin = 0;

        case (state)

            S0: begin
                case (coin)
                    2'b01: next_state = S5;     // ₹5 inserted
                    2'b10: begin
                        next_state = S10;       // ₹10 inserted
                        dispense = 1;
                    end
                    default: next_state = S0;
                endcase
            end

            S5: begin
                case (coin)
                    2'b01: begin
                        next_state = S10;       // ₹5 + ₹5
                        dispense = 1;
                    end
                    2'b10: begin
                        next_state = S15;       // ₹5 + ₹10
                        dispense = 1;
                        return_coin = 1;        // Return ₹5
                    end
                    default: next_state = S5;
                endcase
            end

            S10: begin
                dispense = 1;
                next_state = S0;
            end

            S15: begin
                dispense = 1;
                return_coin = 1;
                next_state = S0;
            end

            default: next_state = S0;
        endcase
    end

endmodule
```

## 🧩 RTL View

<img width="959" height="489" alt="image" src="https://github.com/user-attachments/assets/00bcdd7a-0300-4717-98bf-2104b28b0c6f" />

* The RTL view shows the internal structure of the vending machine controller designed using Verilog HDL.
* It consists of a Finite State Machine, logic blocks, and multiplexers that control product dispensing and coin return based on the inserted amount.
* The design operates using clock and reset signals and ensures correct state transitions.

## 📈 Simulation Waveform

<img width="904" height="420" alt="image" src="https://github.com/user-attachments/assets/b85f6280-591d-41c6-ae27-bde6b341511f" />

* The waveform verifies the functional operation of the vending machine.
* When the required amount is inserted, the dispense signal becomes HIGH.
* If extra amount is inserted, the return_coin signal is also activated.
* After each transaction, the system returns to the initial state, confirming correct operation.

## ✅ Conclusion

* This project successfully demonstrates the design of a Vending Machine Controller using Verilog HDL.
* It provides practical understanding of FSM-based digital systems and strengthens fundamentals of VLSI and hardware design.
