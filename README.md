# Design-of-3-bit-Flash-ADC-using-Inverter-Threshold-Comparator-and-Transmission-Gate-Switch
This repository is created as a design submission for **Mixed Signal SoC design Marathon using eSim & SKY130**
- [Abstract](#abstract)
- [Reference Circuit Diagram](#reference-circuit-diagram)
- [Reference Waveform](#reference-waveform)
- [Circuit Details](#circuit-details)
- [Truth Table](#truth-table)
- [Software Used](#software-used)
  * [eSim](#esim)
  * [NgSpice](#ngspice)
  * [Makerchip](#makerchip)
  * [Verilator](#verilator)
- [Circuit Diagram in eSim](#circuit-diagram-in-esim)
- [Verilog Code](#verilog-code)
- [Makerchip](#makerchip-1)
- [Makerchip Plots](#makerchip-plots)
- [Netlists](#netlists)
- [NgSpice Plots](#ngspice-plots)
- [Steps to run generate NgVeri Model](#steps-to-run-generate-ngveri-model)
- [Steps to run this project](#steps-to-run-this-project)
- [Acknowlegdements](#acknowlegdements)
- [References](#references)



## Abstract
Analog to Digital Converter (ADC) converts time continuous physical signal to digital number. In simple words, ADC converts analog signals in to digital signals that can be used by electronic circuits. In this paper a 3-bit Flash ADC is designed and implemented using comparator circuit called Inverter Threshold Comparator (ITC). The Sample & Hold circuit for ADC is implemented using Transmission Gate Switch (TGS). The Priority encoder is implemented as a Digital Circuit and coded using Verilog. The design is implemented using e-sim tool and sky130pdk.
## Reference Circuit Diagram

**Block Diagram**
![image](https://github.com/DarshanDattaNaik/Design-of-3-bit-Flash-ADC-using-Inverter-Threshold-Comparator-and-Transmission-Gate-Switch/blob/main/project_images/block_diagram.jpeg)

**Inverter Threshold Comparator and Transmission Gate Switch**
![image](https://github.com/DarshanDattaNaik/Design-of-3-bit-Flash-ADC-using-Inverter-Threshold-Comparator-and-Transmission-Gate-Switch/blob/main/project_images/ITC%20and%20TGS.png)

## Reference Waveform

![image](https://github.com/DarshanDattaNaik/Design-of-3-bit-Flash-ADC-using-Inverter-Threshold-Comparator-and-Transmission-Gate-Switch/blob/main/project_images/Waveform.jpeg)

## Circuit Details
ADC has its applications in high-speed communication and signal processing systems. Therefore there is a need of low power, low area and low cost ADC’s. Comparator and encoder are the basic building blocks of Flash ADC.Sampling circuit is very much necessary for the efficient functioning of ADC.The following ADC and Sampling circuit is designed for an operating frequency of 20 KHz and sampling frequency of 800 KHz. A supply voltage of 1V is given to ADC and sample and hold circuit. 

The Sample and Hold circuit is designed using Transmission Gate Switch(TGS) by varying the W and L of PMOS and NMOS and Capacitor value in TGS to get required sampling and operating frequency. 

The sampled input is given to Seven Comparators with desired reference voltages. Seven different voltage values between 0 V and 1 V are used as reference voltage for each comparator. The L and W values of MOS transistors used in the inverter are varied to obtain different threshold voltages for different ITCs. These modified threshold voltages are considered as the comparator reference voltages. 

When input is greater than the reference voltage(threshold voltage) an inverted output (active low) is obtained from the comparator.
The output of the comparators is fed as input to the priority encoder. The priority encoder is designed to convert the output of seven comparators into 3-bit digital output.The output of encoder is given to latch and the final output is enabled only during the hold period of Sample and Hold circuit.The truth table of encoder is given in [Truth Table](#truth-table) section.

** NOTE : In the literature survey report reference waveforms are mentioned for Ramp Wave input just to show the behaviour of ADC but in implementation  Sine waveform is used to make frequency analysis as it is difficult to use ramp wave for frequency analysis. But the required output is obtained **


### 1. Transmission Gate Switch
Transmission Gate switch is designed for the following specifications
- Input frequency <= 20 KHz
- Input Voltage range = 0 to 1V
- Vdd = 1V
- Sampling frequency = 800 KHz


Components Used:
- sky130_fd_pr__nfet_01v8_lvt
- sky130_fd_pr__pfet_01v8_lvt
- sky130_fd_pr__cap_mim_m3_1

Design Table
|  | PMOS | NMOS  | Capacitor = 5.0412 pF |
| ------------- | ------------- | ------------- | ------------- |
| W | 15u | 15u  | 100 |
| L  | 0.45u | 0.45u| 25 |
| MF |---|---| 1 |


### 2. Inverter Threshold Comparators (ITC)
Seven Inverter Threshold Comparators are designed for following specifications 
- Vdd = 1V
- Reference Voltages(Threshold Voltages) : 0.2V , 0.3V , 0.4V , 0.5V , 0.6V , 0.7V , 0.8V

Components Used:
- sky130_fd_pr__nfet_01v8
- sky130_fd_pr__pfet_01v8


Design Table
| ITC    | Vth(mV) | PMOS  |  | NMOS |  |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
|        |         |    W(u)  | L(u) |   W(u)   | L(u) |
|  1st   |    202.542     |    1   |16.5  |   50   | 0.45 |
|  2nd   |    303.39     |     1  | 3 |    4  |  0.45|
|  3rd   |   400.39      |  1     | 0.3 |    5  | 0.3 |
|  4th   |    500.8     |   1    |0.15  |   9   | 0.3 |
|  5th   |   602.5      |  3     | 0.15 |    1  | 0.15 |
|  6th   |   700      |    50   | 0.15 |    1  | 3 |
|  7th   |  804.23       |  30     | 0.15 |  1    | 60 |


## Truth Table

### Priority encoder with Latch
| A7 | A6 | A5  | A4 | A3 | A2 | A1 | Y3 | Y2 | Y1 |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| 1 | 1 | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 0 |
| 1 | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 1 | 0 | 0 | 0 | 1 | 0 |
| 1 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 1 | 1 |
| 1 | 1 | 1 | 0 | 0 | 0 | 0 | 1 | 0 | 0 |
| 1 | 1 | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 1 |
| 1 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 0 |
| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 1 |

### Latch
| en | D3 | D2 | D1 |
| ------------- | ------------- | ------------- | ------------- |
| 0 | D3  | D2 | D1|
| 1 | Y3 | Y2 | Y1 |
## Software Used
### eSim
It is an Open Source EDA developed by FOSSEE, IIT Bombay. It is used for electronic circuit simulation. It is made by the combination of two software namely NgSpice and KiCAD.
</br>
For more details refer:
</br>
https://esim.fossee.in/home
### NgSpice
It is an Open Source Software for Spice Simulations. For more details refer:
</br>
http://ngspice.sourceforge.net/docs.html
### Makerchip
It is an Online Web Browser IDE for Verilog/System-verilog/TL-Verilog Simulation. Refer
</br> https://www.makerchip.com/
### Verilator
It is a tool which converts Verilog code to C++ objects. Refer:
https://www.veripool.org/verilator/

## Circuit Diagram in eSim
![image](https://github.com/DarshanDattaNaik/Design-of-3-bit-Flash-ADC-using-Inverter-Threshold-Comparator-and-Transmission-Gate-Switch/blob/main/project_images/esim_implemented_circuit.png)

## Verilog Code
    module darshan_naik_priority_encoder(A,D,en);
    input [7:1] A;
    input en;
    output reg [3:1] D;
    reg [3:1] Y;
    always@(A)
    begin
    if(~A[7]) Y=3'd7;
    else if (~A[6]) Y=3'd6;
    else if (~A[5]) Y=3'd5;
    else if (~A[4]) Y=3'd4;
    else if (~A[3]) Y=3'd3;
    else if (~A[2]) Y=3'd2;
    else if (~A[1]) Y=3'd1;
    else Y=3'd0;
    end
    always@(*)
    if(en) D<=Y;
    else D<=D;
    endmodule


## Makerchip
      \TLV_version 1d: tl-x.org
      \SV
      /* verilator lint_off UNUSED*/  /* verilator lint_off DECLFILENAME*/  /* verilator lint_off BLKSEQ*/  /* verilator lint_off WIDTH*/  /* verilator lint_off SELRANGE*/  /* verilator lint_off PINCONNECTEMPTY*/  /* verilator lint_off DEFPARAM*/  /* verilator lint_off IMPLICIT*/  /* verilator lint_off COMBDLY*/  /* verilator lint_off SYNCASYNCNET*/  /* verilator lint_off UNOPTFLAT */  /* verilator lint_off UNSIGNED*/  /* verilator lint_off CASEINCOMPLETE*/  /* verilator lint_off UNDRIVEN*/  /* verilator lint_off VARHIDDEN*/  /* verilator lint_off CASEX*/  /* verilator lint_off CASEOVERLAP*/  /* verilator lint_off PINMISSING*/  /* verilator lint_off LATCH*/  /* verilator lint_off BLKANDNBLK*/  /* verilator lint_off MULTIDRIVEN*/  /* verilator lint_off NULLPORT*/  /* verilator lint_off EOFNEWLINE*/  /* verilator lint_off WIDTHCONCAT*/  /* verilator lint_off ASSIGNDLY*/  /* verilator lint_off MODDUP*/  /* verilator lint_off STMTDLY*/  /* verilator lint_off LITENDIAN*/  /* verilator lint_off INITIALDLY*/  /* verilator lint_off */  

      //Your Verilog/System Verilog Code Starts Here:
      module darshan_naik_priority_encoder(A,D,en);
      input [7:1] A;
      input en;
      output reg [3:1] D;
      reg [3:1] Y;
      always@(A)
      begin
      if(~A[7]) Y=3'd7;
      else if (~A[6]) Y=3'd6;
      else if (~A[5]) Y=3'd5;
      else if (~A[4]) Y=3'd4;
      else if (~A[3]) Y=3'd3;
      else if (~A[2]) Y=3'd2;
      else if (~A[1]) Y=3'd1;
      else Y=3'd0;
      end
      always@(*)
      if(en) D<=Y;
      else D<=D;
      endmodule



     //Top Module Code Starts here:
	module top(input logic clk, input logic reset, input logic [31:0] cyc_cnt, output logic passed, output logic failed);
		logic  [7:1] A;//input
		logic  en;//input
		logic  [3:1] D;//output
     //The $random() can be replaced if user wants to assign values
		assign A = $random();
		assign en = $random();
		darshan_naik_priority_encoder darshan_naik_priority_encoder(.A(A), .en(en), .D(D));
	
    \TLV
    //Add \TLV here if desired                                     
    \SV
    endmodule
    
## Makerchip Plots


## Netlists
```
* c:\users\darsh\esim-workspace\flash_adc_3bit\flash_adc_3bit.cir

.include "C:\FOSSEE\eSim\library\sky130_fd_pr\models\sky130_fd_pr__model__diode_pw2nd_11v0.model.spice"
.include "C:\FOSSEE\eSim\library\sky130_fd_pr\models\sky130_fd_pr__model__diode_pd2nw_11v0.model.spice"
.include "C:\FOSSEE\eSim\library\sky130_fd_pr\models\sky130_fd_pr__model__pnp.model.spice"
.include "C:\FOSSEE\eSim\library\sky130_fd_pr\models\sky130_fd_pr__model__linear.model.spice"
.lib "C:\FOSSEE\eSim\library\sky130_fd_pr\models\sky130.lib.spice" tt
.include "C:\FOSSEE\eSim\library\sky130_fd_pr\models\sky130_fd_pr__model__inductors.model.spice"
.include "C:\FOSSEE\eSim\library\sky130_fd_pr\models\sky130_fd_pr__model__r+c.model.spice"
* s c m o d e
xsc16 v7 vsample net-_sc1-pad4_ net-_sc1-pad4_ sky130_fd_pr__pfet_01v8 w=30 l=0.15
xsc14 v6 vsample net-_sc1-pad4_ net-_sc1-pad4_ sky130_fd_pr__pfet_01v8 w=50 l=0.15
xsc13 v6 vsample gnd gnd sky130_fd_pr__nfet_01v8 w=1 l=3
xsc12 v5 vsample net-_sc1-pad4_ net-_sc1-pad4_ sky130_fd_pr__pfet_01v8 w=3 l=0.15
xsc11 v5 vsample gnd gnd sky130_fd_pr__nfet_01v8 w=1 l=0.15
xsc10 v4 vsample net-_sc1-pad4_ net-_sc1-pad4_ sky130_fd_pr__pfet_01v8 w=1 l=0.15
xsc9 v4 vsample gnd gnd sky130_fd_pr__nfet_01v8 w=9 l=0.3
xsc8 v3 vsample net-_sc1-pad4_ net-_sc1-pad4_ sky130_fd_pr__pfet_01v8 w=1 l=0.3
xsc7 v3 vsample gnd gnd sky130_fd_pr__nfet_01v8 w=5 l=0.3
xsc4 v1 vsample net-_sc1-pad4_ net-_sc1-pad4_ sky130_fd_pr__pfet_01v8 w=1 l=16.5
xsc3 v1 vsample gnd gnd sky130_fd_pr__nfet_01v8 w=50 l=0.45
xsc6 v2 vsample net-_sc1-pad4_ net-_sc1-pad4_ sky130_fd_pr__pfet_01v8 w=1 l=3
xsc5 v2 vsample gnd gnd sky130_fd_pr__nfet_01v8 w=4 l=0.45
v1 net-_sc1-pad4_ gnd  dc 1
xsc15 v7 vsample gnd gnd sky130_fd_pr__nfet_01v8 w=1  l=60
* u9  v7 v6 v5 v4 v3 v2 v1 net-_u10-pad1_ net-_u10-pad2_ net-_u10-pad3_ net-_u10-pad4_ net-_u10-pad5_ net-_u10-pad6_ net-_u10-pad7_ adc_bridge_7
* u10  net-_u10-pad1_ net-_u10-pad2_ net-_u10-pad3_ net-_u10-pad4_ net-_u10-pad5_ net-_u10-pad6_ net-_u10-pad7_ net-_u10-pad8_ d3 d2 d1 darshan_naik_priority_encoder
* u14  net-_sc1-pad2_ net-_u10-pad8_ adc_bridge_1
* u11  d3 plot_v1
* u13  d2 plot_v1
* u12  d1 plot_v1
xsc1 vin net-_sc1-pad2_ vsample net-_sc1-pad4_ sky130_fd_pr__pfet_01v8_lvt w=15 l=0.45
xsc2 vin net-_sc2-pad2_ vsample gnd sky130_fd_pr__nfet_01v8_lvt w=15 l=0.45
xsc17 vsample gnd sky130_fd_pr__cap_mim_m3_1 W=100  L=25  MF=1
* u15  vsample plot_v1
v2  vin gnd sine(0.5 0.5 20000 0 0)
v5  net-_sc2-pad2_ gnd pulse(0 1 0 1p 1p 0.625u 1.25u)
v4  net-_sc1-pad2_ gnd pulse(1 0 0 1p 1p 0.625u 1.25u)
* u8  vin plot_v1
* u1  v7 plot_v1
* u2  v6 plot_v1
* u3  v5 plot_v1
* u4  v4 plot_v1
* u5  v3 plot_v1
* u6  v2 plot_v1
* u7  v1 plot_v1
a1 [v7 v6 v5 v4 v3 v2 v1 ] [net-_u10-pad1_ net-_u10-pad2_ net-_u10-pad3_ net-_u10-pad4_ net-_u10-pad5_ net-_u10-pad6_ net-_u10-pad7_ ] u9
a2 [net-_u10-pad1_ net-_u10-pad2_ net-_u10-pad3_ net-_u10-pad4_ net-_u10-pad5_ net-_u10-pad6_ net-_u10-pad7_ ] [net-_u10-pad8_ ] [d3 d2 d1 ] u10
a3 [net-_sc1-pad2_ ] [net-_u10-pad8_ ] u14
* Schematic Name:                             adc_bridge_7, NgSpice Name: adc_bridge
.model u9 adc_bridge(in_low=0.015 in_high=0.015 rise_delay=1.0e-9 fall_delay=1.0e-9 ) 
* Schematic Name:                             darshan_naik_priority_encoder, NgSpice Name: darshan_naik_priority_encoder
.model u10 darshan_naik_priority_encoder(rise_delay=1.0e-9 fall_delay=1.0e-9 input_load=1.0e-12 instance_id=1 ) 
* Schematic Name:                             adc_bridge_1, NgSpice Name: adc_bridge
.model u14 adc_bridge(in_low=0.015 in_high=0.015 rise_delay=1.0e-9 fall_delay=1.0e-9 ) 
.tran 1e-09 0.05e-03 0e-00

* Control Statements 
.control
run
print allv > plot_data_v.txt
print alli > plot_data_i.txt
plot v(d3) 
plot v(d2) 
plot v(d1)
plot v(vsample) v(vin)
plot v(v7) v(v6) v(v5) v(v4) v(v3) v(v2) v(v1)
.endc
.end
```

## NgSpice Plots
### Input and Sampled Waveform for Sine wave of 20 Khz

![image](https://github.com/DarshanDattaNaik/Design-of-3-bit-Flash-ADC-using-Inverter-Threshold-Comparator-and-Transmission-Gate-Switch/blob/main/project_images/Input%20and%20sampled%20waveform.png)

### Comparator waveforms for Sine wave of 20 Khz
Note: 'V' is used to denote output instead of 'A'
![image](https://github.com/DarshanDattaNaik/Design-of-3-bit-Flash-ADC-using-Inverter-Threshold-Comparator-and-Transmission-Gate-Switch/blob/main/project_images/Comparator%20outputs.png)

### Output Waveforms
![image](https://github.com/DarshanDattaNaik/Design-of-3-bit-Flash-ADC-using-Inverter-Threshold-Comparator-and-Transmission-Gate-Switch/blob/main/project_images/output%20waveform.png)

## Steps to run generate NgVeri Model
1. Open eSim
2. Run NgVeri-Makerchip 
3. Add top level verilog file in Makerchip Tab
4. Click on NgVeri tab
5. Add dependency files
6. Click on Run Verilog to NgSpice Converter
7. Debug if any errors
8. Model created successfully
## Steps to run this project
1. Open a new terminal
2. Clone this project using the following command:</br>
```git clone https://github.com/Eyantra698Sumanto/XOR-XNOR-Gate.git ```</br>
3. Change directory:</br>
```cd eSim_project_files/xor_xnor```</br>
4. Run ngspice:</br>
```ngspice xor_xnor.cir.out```</br>
5. To run the project in eSim:

  - Run eSim</br>
  - Load the project</br>
  - Open eeSchema</br>
## Acknowlegdements
1. FOSSEE, IIT Bombay
2. Steve Hoover, Founder, Redwood EDA
3. Kunal Ghosh, Co-founder, VSD Corp. Pvt. Ltd. - kunalpghosh@gmail.com
4. Sumanto Kar, eSim Team, FOSSEE

## References
1. Ahmad, Nabihah & Hasan, Rezaul. (2011). A new design of XOR-XNOR gates for low power application. 10.1109/ICEDSA.2011.5959039. 
2. K. Ravali, N. R. Vijay, S. Jaggavarapu and R. Sakthivel, "Low power XOR gate design and its applications," 2017 Fourth International Conference on Signal Processing, Communication and Networking (ICSCN), 2017, pp. 1-4, doi: 10.1109/ICSCN.2017.8085699.
3. https://github.com/Eyantra698Sumanto/Two-in-One-Low-power-XOR-XNOR-Gate.git
 











