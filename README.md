## Traffic-Light-Controller-Using-Verilog-HDL
## Aim
```
To design and simulate a traffic light controller using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 simulation environment. The objective is to control the traffic lights for a junction with a specific time-based sequence for Red, Yellow, and Green lights.
```
## Apparatus Required
```
Vivado 2023.1 or equivalent Verilog simulation tool.
Computer system with a suitable operating system.
FPGA board (optional for hardware verification).
```
## Procedure
```
Launch Vivado 2023.1:

Open Vivado and create a new project.
Design the Traffic Light Controller Verilog Code:

Write the Verilog code for the traffic light controller, using an FSM (Finite State Machine) to transition between Green, Yellow, and Red lights based on timing intervals.
Create the Testbench:

Write a testbench to simulate the traffic light controller. The testbench will check the sequence of light transitions based on time.
Add the Verilog Files:

Add the traffic light controller Verilog code and the testbench file to the project.
Run Simulation:

Run the behavioral simulation in Vivado to verify the correct sequence of the traffic lights.
Observe the Waveforms:

Examine the waveform output to verify that the traffic light transitions through the Green, Yellow, and Red lights in the correct sequence.
Save and Document Results:

Capture screenshots of the waveform and save the simulation logs to include in your report.
```
## Verilog Code for Traffic Light Controller
```
state_t current_state, next_state;
reg [3:0] counter;  // Timer counter

// State transition based on counter
always @(posedge clk or posedge reset) begin
    if (reset) begin
        current_state <= GREEN;
        counter <= 0;
    end else begin
        if (counter == 4'd9) begin
            current_state <= next_state;
            counter <= 0;
        end else begin
            counter <= counter + 1;
        end
    end
end

// Next state logic and output control
always @(*) begin
    case (current_state)
        GREEN: begin
            lights = 3'b001;  // Green light on
            next_state = YELLOW;
        end
        YELLOW: begin
            lights = 3'b010;  // Yellow light on
            next_state = RED;
        end
        RED: begin
            lights = 3'b100;  // Red light on
            next_state = GREEN;
        end
        default: begin
            lights = 3'b000;  // All lights off
            next_state = GREEN;
        end
    endcase
end
```
## Output
![WhatsApp Image 2024-10-17 at 22 32 51_d22eaa66](https://github.com/user-attachments/assets/5ee0c287-532a-44a4-a22d-8619c3882dc1)

## Testbench for Traffic Light Controller
```
traffic_light_controller_tb.v `timescale 1ns / 1ps
module traffic_light_controller_tb;
// Inputs
reg clk;
reg reset;

// Outputs
wire [2:0] lights;

// Instantiate the Unit Under Test (UUT)
traffic_light_controller uut (
    .clk(clk),
    .reset(reset),
    .lights(lights)
);

// Clock generation
always #5 clk = ~clk;  // Toggle clock every 5 ns

// Test procedure
initial begin
    // Initialize inputs
    clk = 0;
    reset = 1;

    // Release reset after some time
    #10 reset = 0;

    // Run simulation for 100 ns to observe light transitions
    #100 $stop;
end

// Monitor outputs
initial begin
    $monitor("Time=%0t | Lights (R Y G) = %b", $time, lights);
end
module traffic_light_controller_tb;
endmodule
```
## Diagram
![WhatsApp Image 2024-10-17 at 22 33 12_c4f71360](https://github.com/user-attachments/assets/ba4662db-af38-4115-a426-06bb3c20aaf8)
## Conclusion
```
In this experiment, a traffic light controller was successfully designed and simulated using Verilog HDL. The design controlled the traffic lights to switch between Green, Yellow, and Red in a cyclic manner based on timing intervals. The testbench verified that the traffic lights followed the correct sequence and timing. The simulation results confirm the correct functionality of the traffic light controller, demonstrating the effectiveness of Verilog HDL in designing FSM-based controllers for real-world applications.
```
