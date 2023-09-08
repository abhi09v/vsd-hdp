
# VSD-Hardware Design 
- Author - Abhishek Verma



This repo contains everything from architectural design to routing. It covers critical steps such as logic synthesis, post-synthesis STA analysis, floor-planning, placement, clock tree synthesis (CTS), and routing.
# Table of Content
   - [Tool Installation and setup](#Tool-Installation-and-setup)
   - [Yosys](#Yoysys)
   - [OpenSTA](#OpenSTA)
   - [ngspice](#ngspice)
   - [iverilog](#iverilog)
   - [gtkwave](#gtkwave)
   - [magic](#magic)
          
## Tool Installation and setup
 <details>
<summary> Yosys </summary>

Instatllation of OpenSource RTL synthesis tool- Yosys
```bash
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys-master 
$ sudo apt install make (If make is not installed please install it) 
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make 
$ sudo make install
```

![yosys](https://github.com/abhi09v/vsd-hdp/assets/120673607/bac5eb3b-3991-4261-9352-56c7bb536d32)

 Installed and built OpenSTA (including the needed packages) using the following commands:

 ```bash
sudo apt-get install cmake clang gcctcl swig bison flex
git clone https://github.com/The-OpenROAD-Project/OpenSTA.git
cd OpenSTA
mkdir build
cd build
cmake ..
make
```

![opensta](https://github.com/abhi09v/vsd-hdp/assets/120673607/09834f78-5b63-4424-a818-2e765d12c4c8)

</details>
 <details>
 <summary> ngspice </summary>


 Downloading the tarball from https://sourceforge.net/projects/ngspice/files/ to a local directory and unpacked it using the following commands:
 ```bash
tar -zxvf ngspice-37.tar.gz
cd ngspice-37
mkdir release
cd release
../configure  --with-x --with-readline=yes --disable-debug
make
sudo make install
 ```

</details>
 <details>
 <summary> iverilog </summary>


 Installed iverilog using the following command:
  ```bash
sudo apt-get install iverilog
 ```

![iverilog](https://github.com/abhi09v/vsd-hdp/assets/120673607/f4c09643-dd2b-4385-aa1f-e7a56e103582)


 </details>
 <details>
 <summary>gtkwave </summary>


Installed gtkwave using the following command:
  ```bash
sudo apt-get install gtkwave
 ```

![gtkwave](https://github.com/abhi09v/vsd-hdp/assets/120673607/1f7ebafd-77a5-443c-9a45-4d58bed7115d)

</details>
 <details>
 <summary>magic  </summary>


 Installed magic using the following commands:
  ```bash
sudo apt-get install m4
sudo apt-get install tcsh
sudo apt-get install csh
sudo apt-get install libx11-dev
sudo apt-get install tcl-dev tk-dev
sudo apt-get install libcairo2-dev
sudo apt-get install mesa-common-dev libglu1-mesa-dev
sudo apt-get install libncurses-dev
 ```

 ![magic](https://github.com/abhi09v/vsd-hdp/assets/120673607/efd26ac4-ad03-4f08-8c0c-ae0077ff6c0f)


</details>
 <details>
 <summary> ngspice </summary>

 Installed ngspice using the following commands:
  ```bash
sudo apt-get install ngspice
 ```
![ngspice](https://github.com/abhi09v/vsd-hdp/assets/120673607/79b75e3a-1b85-4f7b-acfe-e5ed6459e1ab)
</details>

## Day 1

<details>
 <summary> Introduction </summary>

Simulated and synthesized a 2x1 mux using iverilog and yosys respectively.

- iverilog generates from the RTL design and its testbench a value changing dump file (vcd)
- gtkwave is the tool used to plot the simulation results of the design. 
- Yosys is a tool which synthesizes RTL designs into a netlist. It is also used to test the synthesized netlist when we provide it with a testbench tp check its functionality.

</details>	
	
<details>
 <summary>  2x1 mux (good_mux.v) </summary>

The verilog codes of the 2x1 mux (good_mux.v) 

![Screenshot from 2023-06-24 17-18-32](https://github.com/abhi09v/vsd-hdp/assets/120673607/e547667c-3c10-445b-9529-5a52594b12ed)

 Its testbench (tb_good_mux.v)

![Screenshot from 2023-06-24 17-22-35](https://github.com/abhi09v/vsd-hdp/assets/120673607/988cd52a-1405-48f2-9ccd-8ee930577cfb)

</details>

 <details>
 <summary> Simulation: iverilog and gtkwave </summary>
 
 I used the following commands to simulate and view the plots of the RTL design:
	
 ```bash
 cd verilog_filles
 iverilog <name verilog: good_mux.v> <name testbench: tb_good_mux.v>
 ./a.out
 gtkwave tb_good_mux.vcd
 ```

![Screenshot from 2023-06-24 17-11-50](https://github.com/abhi09v/vsd-hdp/assets/120673607/44b8c239-4e3f-48e3-9ee5-47af0c9b0155)
	
 ![Screenshot from 2023-06-24 17-10-31](https://github.com/abhi09v/vsd-hdp/assets/120673607/ed271be4-5c13-4352-a70e-736e64a4412a)

 </details>

<details>
 <summary> Synthesis: Yosys </summary>
	
 In the directory of the verilog files, I used the following commands to synthesize and view the synthesized deisgn:
	
 ```bash
yosys> read_liberty -lib <path to lib file>
yosys> read_verilog <path to verilog file>
yosys> synth -top <top_module_name>
yosys> abc -liberty <path to lib file>
yosys> show
 ```
 ![Screenshot from 2023-06-24 18-34-45](https://github.com/abhi09v/vsd-hdp/assets/120673607/dcb0a0f6-bcd2-4780-979d-9824ae7b422b)

	
 I used the following commands to generate the netlist:
 ```bash
 yosys> write_verilog <file_name_netlist.v>
 yosys> write_verilog -noattr <file_name_netlist.v>
 ```
 

 ![Screenshot from 2023-06-24 18-54-38](https://github.com/abhi09v/vsd-hdp/assets/120673607/35d4b6a3-ae67-49f3-a9ff-835dd0e87526)
 
 </details>

 ## Day 2

<details>
 <summary> Introduction </summary>

I first synthesized a multiple module (made of two submodules) at the multiple module level (both in hierarchical and flattened forms) then at the submodule level. 

Synthesis at the submodule level is important for two reasons: 

1-) when we have multiple instances of same module (we synthesize once and replicate this netlist multiple times and stitch together the replicas to get the multiple module netlist, and 

2-) when we want to divide and conquer (in massive designs) so that the tool can generate a portion by portion of the overall netlist and then we can stitch together the netlist portions to get the multiple module netlist.
After that, I sumulated the different flop designs using iverilog and gtkwave, then synthesized the designs.
Finally, I synthesized 2 designs that were special; their synthesis used optimizations.

</details>	
	
<details>
 <summary> multiple module  </summary>

- multiple module (multiple_modules.v)
- 
![Screenshot from 2023-06-25 11-57-41](https://github.com/abhi09v/vsd-hdp/assets/120673607/32071988-8c52-4f49-992d-9998f85e7e5b)

- the D-flipflop with asynchronous reset (dff_asyncres.v)
- the D-flipflop with asynchronous set (dff_async_set.v) 
- the D-flipflop with synchronous reset (dff_syncres.v)
  ![Screenshot from 2023-06-26 16-42-29](https://github.com/abhi09v/vsd-hdp/assets/120673607/8d299cc7-9467-41f0-a993-66b628b2e144)
- mult_2.v and 
- mult_8.v
  

</details>
	
<details>
 <summary> multiple_modules level </summary>
		
```bash		
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: multiple_modules.v>
yosys> synth -top <name: multiple_modules>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show <name: multiple_modules>
yosys> write_verilog -noattr <name: multiple_modules_hier.v>
```
Below is the screenshot of the generated hierarchical design:
![Screenshot from 2023-06-25 12-02-52](https://github.com/abhi09v/vsd-hdp/assets/120673607/c553e389-b9e8-46e3-b9ca-ba075dc29c43)		

	
Below is the screenshot of the generated hierarchical netlist:
		
![Screenshot from 2023-06-25 12-06-55](https://github.com/abhi09v/vsd-hdp/assets/120673607/79b87fe9-281e-4695-8265-d7e5ab7b8163)

I used the following additional commands to synthesize and view the design of the flattened multiple module:
		
```bash
yosys> flatten
yosys> write_verilog -noattr <name: multiple_modules_flat.v>
```
	
Below is the screenshot of the generated flattened design:
		
![Screenshot from 2023-06-25 13-17-45](https://github.com/abhi09v/vsd-hdp/assets/120673607/e5aa3469-5a6d-4776-a6ad-689ad784aa61)

Below is the screenshot of the generated flattened netlist:
		
![Screenshot from 2023-06-25 12-19-15](https://github.com/abhi09v/vsd-hdp/assets/120673607/c8b5d3d2-d232-46f0-8636-affd43c07adc)

Below is the screenshot of Sythesized Design:

![Screenshot from 2023-06-26 10-00-21](https://github.com/abhi09v/vsd-hdp/assets/120673607/f1484766-346b-40d4-9381-01786f101f21)

Below is the screenshot of Netlist:

![Screenshot from 2023-06-25 12-00-04](https://github.com/abhi09v/vsd-hdp/assets/120673607/4192f723-b1ad-48ff-a77c-c40b808786e5)

</details>
<details>
 <summary> sub_module1 </summary>
		
I used the following commands to view the synthesized design of the submodule:
		
```bash		
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: multiple_modules.v>
yosys> synth -top <name: sub_module1>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show <name: sub_module1>
```
	
Below is the screenshot of the generated design:
		
![Screenshot from 2023-06-26 10-50-40](https://github.com/abhi09v/vsd-hdp/assets/120673607/ab251297-f464-4756-a521-864b2d7f5ae9)
	
</details>
<details>
<summary> dff with asynchronous reset </summary>

I used the following commands to simulate the RTL design of the dff with asynchronous reset:
	
```bash	
iverilog <name verilog: dff_asyncres.v> <name testbench: tb_dff_asyncres.v>
./a.out
gtkwave <name vcd file: tb_dff_asyncres.vcd>
```	
	
Below is the screenshot of the simulation:
![Screenshot from 2023-06-26 16-28-00](https://github.com/abhi09v/vsd-hdp/assets/120673607/9de791eb-d894-4468-9731-4245dd6ef8ad)

I used the following commands to synthesize the design:
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: dff_asyncres.v>
yosys> synth -top <name: dff_asyncres>
yosys> dfflibmap -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show <name: dff_asyncres>
```	
Below is the screenshot of Syntesized Design :
![Screenshot from 2023-06-26 16-58-59](https://github.com/abhi09v/vsd-hdp/assets/120673607/7150c2a6-f9ad-42f5-b975-04c92ac1aedb)		
</details>

<details>
<summary> dff with asynchronous set </summary>
I used the following commands to simulate the RTL design of the dff with asynchronous set:
	
```bash	
iverilog <name verilog: dff_async_set.v> <name testbench: tb_dff_async_set.v>
./a.out
gtkwave <name vcd file: tb_dff_async_set.vcd>
```
	
Below is the screenshot of the simulation:
![Screenshot from 2023-06-26 16-32-52](https://github.com/abhi09v/vsd-hdp/assets/120673607/8b34351e-55b8-423e-bb47-406721284888)
I used the following commands to synthesize the design:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: dff_async_set.v>
yosys> synth -top <name: dff_async_set>
yosys> dfflibmap -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show <name: dff_async_set>
```	
Below is the screenshot of Design:
![Screenshot from 2023-06-26 17-12-30](https://github.com/abhi09v/vsd-hdp/assets/120673607/25d5b27e-8f44-42cb-a2c6-97457587ddf2)
</details>
<details>
	
<summary> dff with synchronous reset </summary>
	
I used the following commands to simulate the RTL design of the dff with synchronous reset:
	
```bash	
iverilog <name verilog: dff_syncres.v> <name testbench: tb_dff_syncres.v>
./a.out
gtkwave <name vcd file: tb_dff_syncres.vcd>
```	
	
Below is the screenshot of the simulation:
![Screenshot from 2023-06-26 16-19-28](https://github.com/abhi09v/vsd-hdp/assets/120673607/f0e3dc1a-a68b-4449-be90-e3239a73c6b8)
I used the following commands to synthesize the design:
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: dff_syncres.v>
yosys> synth -top <name: dff_syncres>
yosys> dfflibmap -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show <name: dff_syncres>
```
Below is the screenshot of Syntsized Design:	
![Screenshot from 2023-06-26 16-58-59](https://github.com/abhi09v/vsd-hdp/assets/120673607/3541c70d-b33a-4ce4-9f15-a58ee26d988c)


</details>

<details>
 <summary> mult_2.v </summary>
	
I used the following commands to synthesize and view the design:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: mult_2.v>
yosys> synth -top <name: mul2>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show <name: mul2>
yosys> write_verilog -noattr <name: mul2_net.v>
```
	
Below is the screenshot of the synthesized design, note that no hardware was used (no cells are synthesised) as multiplying a 3-bit input by a power of two is equivalent to shifting for output:
![Screenshot from 2023-06-26 20-30-55](https://github.com/abhi09v/vsd-hdp/assets/120673607/3a944269-1e6a-4347-b376-b9c8500ba346)
	
Below is the screenshot of the netlist:
	
![Screenshot from 2023-06-26 20-38-41](https://github.com/abhi09v/vsd-hdp/assets/120673607/f0ce176b-1d25-424f-8bd8-aec416b879dd)


</details>
<details>
 <summary> Synthesis: mult_8.v </summary>
	
I used the following commands to synthesize and view the design:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: mult_8.v>
yosys> synth -top <name: mult8>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show <name: mult8>
yosys> write_verilog -noattr <name: mult8_net.v>
```
	
Below is the screenshot of the synthesized design, note that no hardware was used (no cells are synthesised) as multiplying a 3-bit input (special case) by a nine is equivalent to replicating the input twice for output:
	
![Screenshot from 2023-06-26 20-46-48](https://github.com/abhi09v/vsd-hdp/assets/120673607/3557ef14-9c95-48af-a914-dfb1a0a5de02)

	
Below is the screenshot of the netlist:
	
![Screenshot from 2023-06-26 20-48-08](https://github.com/abhi09v/vsd-hdp/assets/120673607/bde7970a-6718-4f3b-a54b-fb1a90ef6ce7)
</details>


## Day 3
	
<details>
 <summary> Summary </summary>

I have synthesized designs with optimizations. Combinational logic optimizations include 

1-) constant propagation (when the combination is just propagating a constant) 

2-) boolean logic optimization (when boolean rules are used to simplify the expression). Sequential logic optimizations include   

                  a) sequential constant propagation (when constant is propagated with clock involved), 
		  
                  b) state optimization (when unused states are optimized), 
		  
                  c) retiming (when logic is split to decrease timing of the different logic portions and increase frequency), 
		  
                  d) sequential logic cloning (when physical aware synthesis is done to optimize the floop plan)

</details>	
	
<details>
 <summary> Combinational logic optimizations: opt_check.v </summary>
I used the below commands to view the synthesized design of opt_check.v with optimizations with additional command :
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: opt_check.v>
yosys> synth -top <name: opt_check>
yosys> opt_clean -purge
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
Belog is verilog code of opt_ckeck.v 


![Screenshot from 2023-07-01 15-48-54](https://github.com/abhi09v/vsd-hdp/assets/120673607/cb89f3e4-9692-4e30-a000-c99dd810b049)

Below is the screenshot of the obtained optimized design, as we can see a 2-input and gate is realized as was expected when optimizations are applied:

![Screenshot from 2023-07-01 15-19-57](https://github.com/abhi09v/vsd-hdp/assets/120673607/16d8e34b-9e1a-4372-8fca-ce55b473d4c1)

</details>
	
<details>
 <summary> Combinational logic optimizations: opt_check2.v </summary>
	I used the below commands to view the synthesized design of opt_check2.v with optimizations:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: opt_check2.v>
yosys> synth -top <name: opt_check2>
yosys> opt_clean -purge
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
below is screentshot of the opt_check2.v

![Screenshot from 2023-07-01 15-49-15](https://github.com/abhi09v/vsd-hdp/assets/120673607/19112db0-d73c-43f9-8650-fc411f38ca0c)


Below is the screenshot of the obtained optimized design, as we can see a 2-input or gate is realized as was expected when optimizations are applied:
	
![Screenshot from 2023-07-01 15-36-37](https://github.com/abhi09v/vsd-hdp/assets/120673607/23ce991c-3056-49a1-a3f0-5752cd4a0886)



</details>
	
<details>
 <summary> Combinational logic optimizations: opt_check3.v </summary>
	
I used the below commands to view the synthesized design of opt_check3.v with optimizations:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: opt_check3.v>
yosys> synth -top <name: opt_check3>
yosys> opt_clean -purge
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
below is screentshot of the opt_check3.v
![Screenshot from 2023-07-01 15-49-42](https://github.com/abhi09v/vsd-hdp/assets/120673607/3da9ed2a-66b8-425f-9dd7-daf506be25ee)

Below is the screenshot of the obtained optimized design, as we can see a 3-input and gate is realized as was expected when optimizations are applied:
	

![Screenshot from 2023-07-01 15-43-58](https://github.com/abhi09v/vsd-hdp/assets/120673607/5f1406f2-1593-4cf4-b012-2a6b0bb36ecb)



</details>
	
<details>
 <summary> Combinational logic optimizations: opt_check4.v </summary>
	
I used the below commands to view the synthesized design of opt_check4.v with optimizations:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: opt_check4.v>
yosys> synth -top <name: opt_check4>
yosys> opt_clean -purge
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```

below is screentshot of the opt_check4.v
![Screenshot from 2023-07-01 15-50-56](https://github.com/abhi09v/vsd-hdp/assets/120673607/b6efeb20-3dd4-4676-b1e9-0033e0a5c6de)

Below is the screenshot of the obtained optimized design, as we can see a 2-input xnor gate is realized as was expected when optimizations are applied:
	
![Screenshot from 2023-07-01 16-01-33](https://github.com/abhi09v/vsd-hdp/assets/120673607/3ea26e1f-ba14-47fe-b189-2e4ba1f5b2a5)



</details>
		
<details>
 <summary> Combinational logic optimizations: multiple_module_opt.v </summary>
	
I used the below commands to view the synthesized design of multiple_module_opt.v with optimizations:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: multiple_module_opt.v>
yosys> synth -top <name: multiple_module_opt>
yosys> flatten 
yosys> opt_clean -purge
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```

Below is the screenshot of the obtained optimized design, as we can see 2 and gates and 1 or gate are realized as was expected when optimizations are applied:
	
![Screenshot from 2023-07-01 16-04-20](https://github.com/abhi09v/vsd-hdp/assets/120673607/937d7799-b40d-4d23-8d25-99577d2a582d)


</details>
	
<details>
 <summary> Combinational logic optimizations: multiple_module_opt2.v </summary>
	
I used the below commands to view the synthesized design of multiple_module_opt2.v with optimizations:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: multiple_module_opt2.v>
yosys> synth -top <name: multiple_module_opt2>
yosys> flatten 
yosys> opt_clean -purge
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
	
Below is the screenshot of the obtained optimized design, as we can see no standard cells are realized as was expected when optimizations are applied:
	
 ![Screenshot from 2023-07-01 16-08-14](https://github.com/abhi09v/vsd-hdp/assets/120673607/85cc53cc-579b-4e7b-b3b3-5e2545923b27)
![Screenshot from 2023-07-01 16-05-30](https://github.com/abhi09v/vsd-hdp/assets/120673607/eeccf597-e897-425e-8155-032a660a4f8a)

</details>
	
<details>
 <summary> Sequential logic optimizations: dff_const1.v </summary>
	
I used the below commands to simulate the design of dff_const1.v:
	
```bash
iverilog <name verilog: dff_const1.v> <name testbench: tb_dff_const1.v>
./a.out
gtkwave tb_dff_const1.vdc
```	

Below is the screenshot of the obtained simulation, a we can see even when reset is zero, Q waits for next rising edge of clock:
	
![Screenshot from 2023-07-01 16-19-12](https://github.com/abhi09v/vsd-hdp/assets/120673607/20d6399b-6917-4036-bd9b-dea20c81cd19)


	
I used the below commands to view the synthesized design of dff_const1.v with optimizations:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: dff_const1.v>
yosys> synth -top <name: dff_const1>
yosys> dfflibmap -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
	
Below is the screenshot of the obtained optimized design:
	

![Screenshot from 2023-07-01 16-25-12](https://github.com/abhi09v/vsd-hdp/assets/120673607/18c52372-acc7-4e46-95cb-953dde191307)




</details>
	
<details>
 <summary> Sequential logic optimizations: dff_const2.v </summary>
	
I used the below commands to simulate the design of dff_const2.v:
	
```bash
iverilog <name verilog: dff_const2.v> <name testbench: tb_dff_const2.v>
./a.out
gtkwave tb_dff_const2.vdc
```	

Below is the screenshot of the obtained simulation, as we can see Q is one regardless of the value of reset and clock:
	
![Screenshot from 2023-07-01 16-21-43](https://github.com/abhi09v/vsd-hdp/assets/120673607/d029fc0f-cdb9-44bd-9fec-5d3fd67887c1)


I used the below commands to view the synthesized design of dff_const2.v with optimizations:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: dff_const2.v>
yosys> synth -top <name: dff_const2>
yosys> dfflibmap -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
	
Below is the screenshot of the obtained optimized design:
	

![Screenshot from 2023-07-01 16-37-25](https://github.com/abhi09v/vsd-hdp/assets/120673607/7106bf31-ccd0-4a92-aca9-d9c57fcba557)


</details>

	
<details>
 <summary> Sequential logic optimizations: dff_const3.v </summary>
	
I used the below commands to simulate the design of dff_const3.v:
	
```bash
iverilog <name verilog: dff_const3.v> <name testbench: tb_dff_const3.v>
./a.out
gtkwave tb_dff_const3.vdc
```	

Below is the screenshot of the obtained simulation, as we can see Q does not follow Q1 immediately:
	
![Screenshot from 2023-07-01 17-22-57](https://github.com/abhi09v/vsd-hdp/assets/120673607/44c7e890-0dff-4c68-a537-74164e0a9457)


I used the below commands to view the synthesized design of dff_const3.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: dff_const3.v>
yosys> synth -top <name: dff_const3>
yosys> dfflibmap -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
	
Below is the screenshot of the obtained design, the 2 flipflops are retained and optimization could not remove any of them:


![Screenshot from 2023-07-01 16-40-04](https://github.com/abhi09v/vsd-hdp/assets/120673607/8958568d-f9ef-4a7f-ae6d-1a05ce81d48b)



</details>
	
<details>
 <summary> Sequential logic optimizations: dff_const4.v </summary>
	
I used the below commands to simulate the design of dff_const4.v:
	
```bash
iverilog <name verilog: dff_const4.v> <name testbench: tb_dff_const4.v>
./a.out
gtkwave tb_dff_const4.vdc
```	

Below is the screenshot of the obtained simulation, as we can see Q and Q1 are one regardless of clk and reset:


![Screenshot from 2023-07-01 17-26-50](https://github.com/abhi09v/vsd-hdp/assets/120673607/d705bc32-6e7e-4e46-8312-5bfc0360688e)


	
I used the below commands to view the synthesized design of dff_const4.v with optimizations:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: dff_const4.v>
yosys> synth -top <name: dff_const4>
yosys> dfflibmap -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
	
Below is the screenshot of the obtained optimized design, and no hardware was used as expected:
	

![Screenshot from 2023-07-01 16-42-08](https://github.com/abhi09v/vsd-hdp/assets/120673607/d3177004-b87b-4e88-bfcc-35c3a8075614)


</details>
	
<details>
 <summary> Sequential logic optimizations: dff_const5.v </summary>
	
I used the below commands to simulate the design of dff_const5.v:
	
```bash
iverilog <name verilog: dff_const5.v> <name testbench: tb_dff_const5.v>
./a.out
gtkwave tb_dff_const5.vdc
```	

Below is the screenshot of the obtained simulation, as we can see when reset is zero, Q1 becomes one on the next rising edge of clk, and Q follows Q1 on the next rising edge of clk:

![Screenshot from 2023-07-01 17-29-02](https://github.com/abhi09v/vsd-hdp/assets/120673607/a665687f-4771-4196-90b0-33b93da5fc9f)

	
I used the below commands to view the synthesized design of dff_const5.v with optimizations:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: dff_const5.v>
yosys> synth -top <name: dff_const5>
yosys> dfflibmap -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
	
Below is the screenshot of the obtained optimized design, and the 2 flipflops are retained:
	

![Screenshot from 2023-07-01 16-50-35](https://github.com/abhi09v/vsd-hdp/assets/120673607/3a83ddc1-095e-4c3b-9047-0d466d2f00b6)


</details>
	
<details>
 <summary> Sequential logic optimizations for unused outputs: counter_opt.v </summary>
	
I used the below commands to view the synthesized design of counter_opt.v with optimizations:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: counter_opt.v>
yosys> synth -top <name: counter_opt>
yosys> dfflibmap -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
	
Below is the screenshot of the obtained optimized design, and the only used output (count[0]) is present and 1 flipflop is used:
	

![Screenshot from 2023-07-01 16-53-07](https://github.com/abhi09v/vsd-hdp/assets/120673607/8bf2059c-ae20-4e39-84d1-db10fc7cc77a)

	
</details>
	
<details>
 <summary> Sequential logic optimizations for 3 used outputs: counter_opt2.v </summary>
	
I used the below commands to view the synthesized design of counter_opt2.v with optimizations:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: counter_opt2.v>
yosys> synth -top <name: counter_opt2>
yosys> dfflibmap -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
	
Below is the screenshot of the obtained optimized design, and 3 flipflops are used in addition to the counting logic of all bits:

![image](https://github.com/abhi09v/vsd-hdp/assets/120673607/60fc2e37-be77-4edc-87bf-939a3cb1f43e)
	
</details>



## Day 4

<details>
 <summary> Summary </summary>

 GLS is when the testbench is run with the netlist as design under test to ensure there are no synthesis and simulation mismatches, and it is important as it 

1-) verifies the logical correctness of the post-synthesis design  

2-) ensures the timing of design is met. Synthesis and simulation mismatches can happen due to a lot of reasons including 
- missing sensitivity list (some signal changes are not captured by the circuit because they are missing from the sensitivity list), 
- blocking vs non-blocking assignments (inside an always block, 
- "=" statements inside it are blocking meaning they are executed in order they are written, 
- assignments (<=) on the other hand are non-blocking so they are executed in parallel => non-blocking should be used with sequential circuits. 

-Note that the synthesis will yield same circuit with blocking and non-blockin; it will yield what would be obtained as if the statements where written in non-blocking format, so in case they weren't written as such a mismatch will occur with the simulation), and non-standard verilog coding.
	
</details>	
<details>
 <summary> Simulation, synthesis, and GLS: ternary_operator_mux.v </summary>
	
Below is Verilog Code and testbench for ternary_mux.v
![Screenshot from 2023-07-08 12-57-41](https://github.com/abhi09v/vsd-hdp/assets/120673607/07aa2d18-080d-4a7c-90f4-8e0ae7bd82ea)

Used the below commands to simulate the design of ternary_operator_mux.v:
	
```bash
iverilog <name verilog: ternary_operator_mux.v> <name testbench: tb_ternary_operator_mux.v>
./a.out
gtkwave tb_ternary_operator_mux.vdc
```	

Below is the screenshot of the obtained simulation, we can see that when sel is high y follows i1, and when sel is low y follows i0:

![ternary](https://github.com/abhi09v/vsd-hdp/assets/120673607/88f27d11-cd49-48d8-a3c9-0ad5f2205b85)


I used the below commands to synthesize the design into a netlist and view the synthesized design of ternary_operator_mux.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: ternary_operator_mux.v>
yosys> synth -top <name: ternary_operator_mux>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> write_verilog -noattr <name of netlist: ternary_operator_mux_net.v>
yosys> show
```
	
Below is the screenshot of the obtained design:

![Screenshot from 2023-07-08 13-05-11](https://github.com/abhi09v/vsd-hdp/assets/120673607/31742b73-6b5e-4c6d-9587-f6c4d278deaa)


I used the below commands to carry out GLS of ternary_operator_mux.v:
	
```bash
iverilog <path to verilog model: ../mylib/verilog_model/primitives.v> <path to sky130_fd_sc_hd__tt_025C_1v80.lib: ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib> <name netlist: ternary_operator_mux_net.v> <name testbench: tb_ternary_operator_mux.v>
./a.out
gtkwave tb_ternary_operator_mux.vdc
```	
	
Below is the screenshot of the obtained simulation, and this matches with pre-synthesis simulation:
	
![Screenshot from 2023-07-08 13-55-20](https://github.com/abhi09v/vsd-hdp/assets/120673607/9ffc371f-63fa-4996-adbb-2723d0c1d962)
	
</details>

<details>
 <summary> Simulation, synthesis, and GLS: bad_mux.v </summary>

I used the below commands to simulate the design of bad_mux.v:
	
```bash
iverilog <name verilog: bad_mux.v> <name testbench: tb_bad_mux.v>
./a.out
gtkwave tb_bad_mux.vdc
```	

Below is the screenshot of the obtained simulation, we can see that when inputs change, y is not evaluated which is wrong behavior:

![Screenshot from 2023-07-08 14-00-35](https://github.com/abhi09v/vsd-hdp/assets/120673607/a8b265de-af24-439b-acfd-2cb9f7e2c8e3)



I used the below commands to synthesize the design into a netlist and view the synthesized design of bad_mux.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: bad_mux.v>
yosys> synth -top <name: bad_mux>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> write_verilog -noattr <name of netlist: bad_mux_net.v>
yosys> show
```
	
Below is the screenshot of the obtained design:

![Screenshot from 2023-07-08 14-04-13](https://github.com/abhi09v/vsd-hdp/assets/120673607/002f3031-9c6d-43aa-ab49-4e0611c5e17d)


	
Below is the screenshot of the obtained netlist:

![Screenshot from 2023-07-08 14-07-12](https://github.com/abhi09v/vsd-hdp/assets/120673607/c3bdd33e-e839-4bf3-b467-1d3d876428f7)

	
I used the below commands to carry out GLS of bad_mux.v:
	
```bash
iverilog <path to verilog model: ../mylib/verilog_model/primitives.v> <path to sky130_fd_sc_hd__tt_025C_1v80.lib: ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib> <name netlist: bad_mux_net.v> <name testbench: tb_bad_mux.v>
./a.out
gtkwave tb_bad_mux.vdc
```	
	
Below is the screenshot of the obtained simulation, and this mismatches with pre-synthesis simulation:
	
![Screenshot from 2023-07-08 14-22-54](https://github.com/abhi09v/vsd-hdp/assets/120673607/21577d1e-e01a-408a-9cb1-73e9abcd35f6)

	
</details>

<details>
 <summary> Simulation, synthesis, and GLS: blocking_caveat.v </summary>
Below is verilog code for blocking_caveat.v 
![Screenshot from 2023-07-08 14-38-16](https://github.com/abhi09v/vsd-hdp/assets/120673607/c7df3fb4-161d-47cf-b648-c444e15af992)

I used the below commands to simulate the design of blocking_caveat.v:
	
```bash
iverilog <name verilog: blocking_caveat.v> <name testbench: tb_blocking_caveat.v>
./a.out
gtkwave tb_blocking_caveat.vdc
```	

Below is the screenshot of the obtained simulation, and as we can see d is seeing the precious values, and hence it is acting as if there was a flop in the circuit which is not the case (incorrect behavior):

![Screenshot from 2023-07-08 14-38-42](https://github.com/abhi09v/vsd-hdp/assets/120673607/905380a2-7a13-4d6a-b6a8-854e978d7e98)


I used the below commands to synthesize the design into a netlist and view the synthesized design of blocking_caveat.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: blocking_caveat.v>
yosys> synth -top <name: blocking_caveat>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> write_verilog -noattr <name of netlist: blocking_caveat_net.v>
yosys> show
```
	
Below is the screenshot of the obtained design:

![Screenshot from 2023-07-08 14-42-29](https://github.com/abhi09v/vsd-hdp/assets/120673607/13f451ea-224d-4d99-b462-fc77e8959a83)

	
Below is the screenshot of the obtained netlist:

![Screenshot from 2023-07-08 14-44-54](https://github.com/abhi09v/vsd-hdp/assets/120673607/c47912d2-c346-4ba8-b588-4fc55de1f686)


I used the below commands to carry out GLS of blocking_caveat.v:
	
```bash
iverilog <path to verilog model: ../mylib/verilog_model/primitives.v> <path to verilog model: ../mylib/verilog_model/sky130_fd_sc_hd.v> <name netlist: blocking_caveat_net.v> <name testbench: tb_blocking_caveat.v>
./a.out
gtkwave tb_blocking_caveat.vdc
```	
	
Below is the screenshot of the obtained simulation, and this mismatches with pre-synthesis simulation due to blocking statement:
	
![Screenshot from 2023-07-08 14-47-18](https://github.com/abhi09v/vsd-hdp/assets/120673607/b86b1719-dd19-4d68-8b4a-921b12ead186)

	
</details>


