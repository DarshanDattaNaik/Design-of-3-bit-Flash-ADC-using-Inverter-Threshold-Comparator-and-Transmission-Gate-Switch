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
- [GAW Plots](#gaw-plots)
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
ADC has its applications in high-speed communication and signal processing systems. Therefore there is a need of low power, low area and low cost ADC’s. Comparator and encoder are the basic building blocks of Flash ADC.Sampling circuit is very much necessary for the efficient functioning of ADC. 


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










