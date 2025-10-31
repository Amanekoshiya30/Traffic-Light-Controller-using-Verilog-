# Traffic-Light-Controller-using-Verilog-
This project implements a real-world Traffic Light Controller based on Indian road conditions using Verilog HDL and Vivado 2019.1. The design uses a finite state machine (FSM) to control North–South, East–West, and pedestrian lights in a realistic timing sequence.

## Features
- FSM-based light transitions (Green → Yellow → Red)
- Pedestrian signal active only when both directions are red
- Clock division from 100 MHz → 8 MHz → 1 Hz using Clock Wizard and custom divider
- Behavioral simulation in Vivado

## Verilog Code for Traffic Light Controller 
```
module Traffic_light(
    input clk_1hz,
    input reset,
    output reg [2:0] NS,
    output reg [2:0] EW ,
    output reg  Ped_light
   );
   reg [2:0] state;
   reg [2:0] nextstate;
   reg [22:0] counter;
   localparam S0 = 3'd0;
   localparam S1 = 3'd1;
   localparam S2 = 3'd2;
   localparam S3 = 3'd3;
   localparam S4 = 3'd4;
   
   parameter Green = 23'd10;
   parameter Yellow = 23'd5;
   parameter Red = 23'd7;
   
   always @(posedge clk_1hz or posedge reset)
    begin 
        if (reset) begin 
            counter <= 0;
            state <= S0; end 
        else begin 
            counter <= counter +1;
            case (state)
            S0: if (counter >= Green) begin // for NS
                    state<= S1;
                    counter <=0;end 
            S1: if (counter >= Yellow) begin 
                    state <= S2;
                    counter <= 0;end
            S2: if (counter >=  Red) begin //at this time all lights are Red which means pedestrians can cross the roads 
                    state <= S3;
                    counter <= 0;end
            S3: if (counter >= Green) begin   //for EW
                    state <= S4;
                    counter <=0;end
            S4: if (counter >= Yellow)begin 
                    state <=S2;
                    counter <= 0; end
            default : begin  state <= S0;
                          counter <=0 ;end
            endcase 
            end 
      end 
      //001 -> green
      //010 -> yellow
      //011 -> red 
always @(*) begin 
    NS = 3'b000;
    EW = 3'b000;
    Ped_light = 1'b0;
    
    case (state)
    S0: begin 
        NS = 3'b001;
        EW = 3'b011;
        Ped_light = 1'b0;end 
    S1: begin 
        NS = 3'b010;
        EW = 3'b011;
        Ped_light = 1'b0;end
    S2: begin 
        NS = 3'b011;
        EW = 3'b011;
        Ped_light = 1'b1;end
    S3: begin 
        NS = 3'b011;
        EW = 3'b001;
        Ped_light = 1'b0;end
    S4: begin 
        NS = 3'b011;
        EW = 3'b010;
        Ped_light = 1'b0;end        
    default : begin NS= 3'b011;
              EW = 3'b011;
              Ped_light = 1'b0;end
    endcase 
    end          
      
endmodule
```
## TestBench 
```
module tb_top_wrapper;

    reg clk_100Mhz;
    reg reset;
    wire [2:0] NS;
    wire [2:0] EW;
    wire Ped_light;

    top_wrapper uut (
        .clk_100Mhz(clk_100Mhz),
        .reset(reset),
        .NS(NS),
        .EW(EW),
        .Ped_light(Ped_light)
    );

    initial begin
        clk_100Mhz = 0;
        forever #5 clk_100Mhz = ~clk_100Mhz; 
    end

    initial begin
        reset = 1;
        #20;
        reset = 0;
    end

    initial begin
        #20000000;  
        $finish;
    end

endmodule
```
## Output Waveform 
<img width="1551" height="870" alt="image" src="https://github.com/user-attachments/assets/cbd6f53c-37dd-469f-a42d-ca6ab9c7096d" />

## Conclusion 
This project was a great way to understand how digital systems work in real-time using Verilog. I designed and simulated a Traffic Light Controller that follows a proper sequence of signals for smooth traffic flow. Through this, I learned how to write and test HDL code, create timing-based logic, and use simulation tools like Vivado to visualize outputs. This project strengthened my basics of digital logic design and gave me practical experience in implementing state-based systems.

