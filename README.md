# Microblaze MCU FPGA Design Example

Build the application software for RTL simulation flow. Folow the steps to launch the RTL simualtion with application software running on Microblaze MCU

| # | Training Module | Page | Description |
|:-:|:-|:-|:-|
| 1 | Vivado HW Development Flow | [Page](https://github.com/heslabs/haps_mcu_v22p2/tree/main/01_Create_HW_Design) | Create RTL hardware design using Vivado |
| 2 | Vitis SW Development Flow | [Page](https://github.com/heslabs/haps_mcu_v22p2/tree/main/02_Create_Application_SW) | Build MCU baremetal software using Vitis |
| 3 | Vivado Simulation Flow | [Page](https://github.com/heslabs/haps_mcu_v22p2/tree/main/03_Run_RTL_Simulation) | Running Verilog simulation using Vivado |
| 4 | Application SW Test Flow | [Page](https://github.com/heslabs/haps_mcu_v22p2/tree/main/04_Run_Software_Application) | Running application software in RTL simulation |
| 5 | Program FPGA Bitstream | [Page](https://github.com/heslabs/haps_mcu_v22p2/tree/main/05_Program_FPGA) | Running application software in FPGA emulation |
| 6 | Running FPGA Emulation | [Page](https://github.com/heslabs/haps_mcu_v22p2/tree/main/06_Running_FPGA_Emulation) | Running application software in FPGA emulation |


#### Prerequisites
* Verilog design and simluation 
* FPGA design and implementaiton methodology
* AMBA-based SoC architecture 


---  
### 1. Vivado HW Development Flow

* Create design based on prebuit reference (top.tcl) \
  ```$ make new```
* Add your custom IP into the referecne and validate it
* Export the hardware in Vivado (top_wrapper.xsa) \
  ```$ make exp```

---  
### 2. Vitis SW Development Flow

* Create new application project in Vitis \
  ```$ make app-new```
* Select the xsa file from hardware export for platform   
* Enter the platform name: "main", this will be used in tcl command later  
* Create C/C++ program from the template
* Modify the application C/C++ codes
* Build the Vitis project and geenrate the exectuable binary (main.elf)

--- 
### 3. Vivado Simulation Flow
* Association the elf files in Vivado
* Export the simulation for Vivado XSIM simualtor
* Lunch the generated testbench script "tb.sh"
* Open the dumped waveform file (*.wdb) with selected signals (tb_behav.wcfg)

---
### 4. Application SW Test Flow

* Copy the application source from workspace
  ```
  $ cd ./appsw/memorytests
  $ cp -rf ../workspace/main/src .
  ```
  
* Modify the application code in command mode \
  ```$ vim ./appsw/memorytests/src/memorytest.c```
  
* Build the Vitis project and generate elf file in command mode \
  ```$ make app-cmd```
  
* Launch the RTL simulaiton and view the dumped waveform \
  ```$ make sim```
  
* Copy the simulation waveform (*.wdb) into SW test case
  ```
  $ cd ./vivado && ln -s ../appsw/$(APP)/tb.wdb .
  $ cd ./vivado && cp -f ../appsw/$(APP)/tb_behav.wcfg .
  ```
  
* Launch waveform viewer for SW test case
  ```
  $ cd ./vivado && vivado -mode gui -project ./$(PRJ).xpr -source ../appsw/$(APP)/xsim_wave.tcl &
  ### xsim_wave.tcl
  set_property target_simulator XSim [current_project]
  open_wave_database {./tb.wdb}
  open_wave_config  -quiet ./tb_behav.wcfg
  ```

---
### 5. Run FPGA Emulation Flow

* Download embedded software and launch emulation
* Download your application software for Microblaze MCU and launch the FPGA emulaiton at runtime. Note that the prerequisites are the FPGA bitstream was successfully programmed and the Microblaze MCU in the HW design works funcitonal.
* Prepare you application software "apps.elf"
* Launch Xilinx system command line tool with tcl script
* Check the results in "uart.log"

```
$ xsct ../scripts/xsct_uartcmd.tcl $(HAPS_IP)
```



