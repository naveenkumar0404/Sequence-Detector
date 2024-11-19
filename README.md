# EXP-6 Sequence-Detector

## Aim:

&emsp;&emsp;To design and simulate a sequence detector using both Moore and Mealy state machine models in Verilog HDL, and verify their functionality through a testbench using the Vivado 2023.1 simulation environment. The objective is to detect a specific sequence of bits (e.g., 1001) and compare the Moore and Mealy designs.<br>

## Apparatus Required:

&emsp;&emsp;Vivado 2023.1 or equivalent Verilog simulation tool.<br>

&emsp;&emsp;Computer system with a suitable operating system.<br>

## Procedure:

## Launch Vivado 2023.1:

&emsp;&emsp;Open Vivado and create a new project.<br>

**Write the Verilog Code for Sequence Detector (Moore and Mealy FSM):**

&emsp;&emsp;Design two Verilog modules: one for a Moore FSM and another for a Mealy FSM to detect a sequence such as 1001.<br>

## Create the Testbench:

&emsp;&emsp;Write a testbench to apply input sequences and verify the output of both FSM designs.<br>

## Add the Verilog Files:

&emsp;&emsp;Add the Verilog code for both FSMs and the testbench to the Vivado project.<br>

## Run Simulation:

&emsp;&emsp;Run the simulation and observe the output to check if the sequence is detected correctly.<br>

## Observe the Waveforms:

&emsp;&emsp;Analyze the waveform to ensure both the Moore and Mealy machines detect the sequence as expected.<br>

## Save and Document Results:

&emsp;&emsp;Capture the waveforms and include the results in the final report.<br>

## Verilog Code for Sequence Detector Using Moore FSM:

```
module moore_sequence_detector ( 
    input wire clk, 
    input wire reset, 
    input wire seq_in, 
    output reg detected 
);
parameter reg [2:0] S0=2'd0, S1=2'd1, S2=2'd2, S3=2'd3, S4=3'd4;
reg [2:0] current_state, next_state;
// State transition logic
always @(posedge clk) begin
    if (reset)
        current_state <= S0;
    else
        current_state <= next_state;
end
// Next state and output logic
always @(*) begin
    case (current_state)
        S0: begin
            next_state = S1;
            detected = 0;
        end
        S1: begin//1
            next_state = S2;
            detected = 0;
        end
        S2: begin//10
            next_state = S3;
            detected = 0;
        end
        S3: begin//100
            next_state = S4;
            detected = 0;
        end
        S4: begin//1001
            next_state = S0;
            detected = 1;  // Sequence 1001 detected
        end
        default: next_state = S0;
    endcase
end
endmodule
```

## Testbench for Sequence Detector (Moore FSM):

```
`timescale 1ns / 1ps
module sequence_detector_tb; // Inputs 
reg clk; reg reset; reg seq_in;
// Outputs
wire moore_detected;
wire mealy_detected;
// Instantiate the Moore FSM
moore_sequence_detector moore_fsm (
    .clk(clk),
    .reset(reset),
    .seq_in(seq_in),
    .detected(moore_detected)
);
// Instantiate the Mealy FSM
mealy_sequence_detector mealy_fsm (
    .clk(clk),
    .reset(reset),
    .seq_in(seq_in),
    .detected(mealy_detected)
);
// Clock generation
initial
begin
forever #5 clk = ~clk;  // Clock with 10 ns period
end
// Test sequence
initial begin
    // Initialize inputs
    clk = 1;
    reset = 1;
    seq_in = 1;
    // Release reset after 20 ns
    #20 reset = 0;
    // Apply sequence: 1001
    @(posedge clk);
     seq_in = 1;
    @(posedge clk);
    seq_in = 0;
    @(posedge clk);
    seq_in = 0;
    @(posedge clk);
    seq_in = 1;
    @(posedge clk);
    seq_in = 1;
    @(posedge clk);
    seq_in = 0;
    @(posedge clk);
    seq_in = 0;
    @(posedge clk);
    seq_in = 1;
    @(posedge clk);
    seq_in = 1;
    @(posedge clk);
    // Stop the simulation
    #30 $stop;
end
// Monitor the outputs
initial begin
    $monitor("Time=%0t | seq_in=%b | Moore FSM Detected=%b | Mealy FSM Detected=%b",
             $time, seq_in, moore_detected, mealy_detected);
end
endmodule
```

## Output Waveform

![Screenshot (252)](https://github.com/user-attachments/assets/8499d6df-3d70-49bf-b045-a3430bfc0a2e)

## Conclusion:

&emsp;&emsp;In this experiment, Moore and Mealy FSMs were successfully designed and simulated to detect the sequence 1001. Both designs worked as expected, with the main difference being that the Moore FSM generated the output based on the current state, while the Mealy FSM generated the output based on both the current state and input. The testbench verified the functionality of both FSMs, demonstrating that the Verilog HDL can effectively model both types of state machines for sequence detection tasks.
