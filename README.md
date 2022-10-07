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
Analog to Digital Converter (ADC) converts time continuous physical signal to digital number. In simple words, ADC converts analog signals in to digital signals that can be used by electronic circuits. In this paper a 3-bit Flash ADC is designed and implemented using comparator circuit called Inverter Threshold Comparator (ITC). The Sample & Hold circuit for ADC is implemented using Transmission Gate Switch (TGS). The Priority encoder is implemented as a Digital Circuit and coded using Verilog. The design is implemented using e-sim tool.

