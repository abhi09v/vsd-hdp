
# VSD-Hardware Design 
- Author - Abhishek Verma



This repo contains everything from architectural design to routing. It covers critical steps such as logic synthesis, post-synthesis STA analysis, floor-planning, placement, clock tree synthesis (CTS), and routing.

          
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

![Screenshot from 2023-06-25 11-57-41](https://github.com/abhi09v/vsd-hdp/assets/120673607/32071988-8c52-4f49-992d-9998f85e7e5b)

- the D-flipflop with asynchronous reset (dff_asyncres.v)
- the D-flipflop with asynchronous set (dff_async_set.v) 
- the D-flipflop with synchronous reset (dff_syncres.v)

  ![Screenshot from 2023-06-26 16-42-29](https://github.com/abhi09v/vsd-hdp/assets/120673607/ac6f7506-e170-490a-937b-3bc411a30e46)

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

![Screenshot from 2023-06-26 16-28-00](https://github.com/abhi09v/vsd-hdp/assets/120673607/bca4193f-a005-4bbf-96f4-94172d57761e)

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

![Screenshot from 2023-06-26 16-58-59](https://github.com/abhi09v/vsd-hdp/assets/120673607/d10334b6-1fe6-4bc9-9536-9e329f9c7024)		
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

![Screenshot from 2023-06-26 16-32-52](https://github.com/abhi09v/vsd-hdp/assets/120673607/60c924d5-d9b3-4838-860d-1446e353840c)

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

![Screenshot from 2023-06-26 17-12-30](https://github.com/abhi09v/vsd-hdp/assets/120673607/68f3fdfc-b88b-4d43-a618-3cd622e2320b)

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

![Screenshot from 2023-06-26 16-19-28](https://github.com/abhi09v/vsd-hdp/assets/120673607/5ce0f631-d45f-4898-90a2-52d2b2fee2af)


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

![Screenshot from 2023-06-26 16-58-59](https://github.com/abhi09v/vsd-hdp/assets/120673607/9f92c020-48e7-41a1-aac9-6bf00084bbac)


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

![Screenshot from 2023-06-26 20-30-55](https://github.com/abhi09v/vsd-hdp/assets/120673607/6a2f005d-8e13-448e-a733-4a6766a6b442)
	
Below is the screenshot of the netlist:
	
![Screenshot from 2023-06-26 20-38-41](https://github.com/abhi09v/vsd-hdp/assets/120673607/1846639e-fcbc-4e22-80e6-980bfb2e238a)


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
	
![Screenshot from 2023-06-26 20-46-48](https://github.com/abhi09v/vsd-hdp/assets/120673607/22563382-e894-4999-8de8-a2f8742b89e5)

	
Below is the screenshot of the netlist:
	
![Screenshot from 2023-06-26 20-48-08](https://github.com/abhi09v/vsd-hdp/assets/120673607/4c651954-182b-41a6-8e67-c1d4804ca56a)

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


![Screenshot from 2023-07-01 15-48-54](https://github.com/abhi09v/vsd-hdp/assets/120673607/8389acc1-e872-417c-b768-72084cbf7b2a)

Below is the screenshot of the obtained optimized design, as we can see a 2-input and gate is realized as was expected when optimizations are applied:

![Screenshot from 2023-07-01 15-19-57](https://github.com/abhi09v/vsd-hdp/assets/120673607/6fb53252-1ddd-4b65-885d-83df40ad552b)

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

![Screenshot from 2023-07-01 15-49-15](https://github.com/abhi09v/vsd-hdp/assets/120673607/40ed1964-0e4e-4d29-93a2-5b8c032ed44c)

Below is the screenshot of the obtained optimized design, as we can see a 2-input or gate is realized as was expected when optimizations are applied:
	
![Screenshot from 2023-07-01 15-36-37](https://github.com/abhi09v/vsd-hdp/assets/120673607/e8139182-4cdb-413d-8211-dfacbe161c3d)



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
![Screenshot from 2023-07-01 15-49-42](https://github.com/abhi09v/vsd-hdp/assets/120673607/ee36b77e-1e86-4504-b9b9-03bf5674750c)

Below is the screenshot of the obtained optimized design, as we can see a 3-input and gate is realized as was expected when optimizations are applied:
	

![Screenshot from 2023-07-01 15-43-58](https://github.com/abhi09v/vsd-hdp/assets/120673607/e17bd9f5-fee0-4666-ab20-4ae906d71b90)



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
![Screenshot from 2023-07-01 15-50-56](https://github.com/abhi09v/vsd-hdp/assets/120673607/e8abfb6b-d03d-4268-9ec0-c8c3187cd112)

Below is the screenshot of the obtained optimized design, as we can see a 2-input xnor gate is realized as was expected when optimizations are applied:
	
![Screenshot from 2023-07-01 16-01-33](https://github.com/abhi09v/vsd-hdp/assets/120673607/e362e75c-7b4f-45dc-a36e-5b894ba19206)


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
	
![Screenshot from 2023-07-01 16-04-20](https://github.com/abhi09v/vsd-hdp/assets/120673607/035f03c3-06cd-4e6a-81f3-aca12cfeefe3)

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
	
![Screenshot from 2023-07-01 16-08-14](https://github.com/abhi09v/vsd-hdp/assets/120673607/5033030b-4e0f-4f6d-9d9d-5cd91a14b92b)
![Screenshot from 2023-07-01 16-05-30](https://github.com/abhi09v/vsd-hdp/assets/120673607/4852beaf-665d-4fe2-a467-cb72f0131c63)

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
	

![Screenshot from 2023-07-01 16-19-12](https://github.com/abhi09v/vsd-hdp/assets/120673607/a4a27abb-5e1a-4ea9-8e5d-342dc1f33667)
	
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
	

![Screenshot from 2023-07-01 16-25-12](https://github.com/abhi09v/vsd-hdp/assets/120673607/6b246625-4aef-4bf4-85af-8d5fed4de462)




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
	
![Screenshot from 2023-07-01 16-21-43](https://github.com/abhi09v/vsd-hdp/assets/120673607/72f5e638-a6cb-4b6f-a4f6-dbab1db8ce5b)


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

![Screenshot from 2023-07-01 16-37-25](https://github.com/abhi09v/vsd-hdp/assets/120673607/efcaf488-a7b3-4d27-b910-3673ab564acf)



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
	
![Screenshot from 2023-07-01 17-22-57](https://github.com/abhi09v/vsd-hdp/assets/120673607/f3a2a557-7cd0-4e4b-9806-ae464d81bc4a)

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


![Screenshot from 2023-07-01 16-40-04](https://github.com/abhi09v/vsd-hdp/assets/120673607/9e6b8cba-9de6-4572-a2c4-bbba8a6419a0)



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


![Screenshot from 2023-07-01 17-26-50](https://github.com/abhi09v/vsd-hdp/assets/120673607/37838a9e-56d0-41a7-961b-7645b2d23659)

	
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
	


![Screenshot from 2023-07-01 16-42-08](https://github.com/abhi09v/vsd-hdp/assets/120673607/4ebf745b-c6e6-4c12-a7df-095ef3c84a87)


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

![Screenshot from 2023-07-01 17-29-02](https://github.com/abhi09v/vsd-hdp/assets/120673607/26784e6e-7323-42cb-808a-2df13b4b4079)
	
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
	

![Screenshot from 2023-07-01 16-50-35](https://github.com/abhi09v/vsd-hdp/assets/120673607/898db664-4d27-4fbe-88bb-7c52f3bf93ef)


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
	

![Screenshot from 2023-07-01 16-53-07](https://github.com/abhi09v/vsd-hdp/assets/120673607/e9580706-9cc0-4858-92bf-d51f765ee306)
	
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

![image](https://github.com/abhi09v/vsd-hdp/assets/120673607/69b98b8e-14b4-44fa-8646-17b614c1f837)
	
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
![Screenshot from 2023-07-08 12-57-41](https://github.com/abhi09v/vsd-hdp/assets/120673607/ad0d22f4-bc49-47be-87e2-291b503b8e30)

Used the below commands to simulate the design of ternary_operator_mux.v:
	
```bash
iverilog <name verilog: ternary_operator_mux.v> <name testbench: tb_ternary_operator_mux.v>
./a.out
gtkwave tb_ternary_operator_mux.vdc
```	

Below is the screenshot of the obtained simulation, we can see that when sel is high y follows i1, and when sel is low y follows i0:


![ternary](https://github.com/abhi09v/vsd-hdp/assets/120673607/11bb8fed-3774-465c-ae2d-abf6f045ad2d)

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

![Screenshot from 2023-07-08 13-05-11](https://github.com/abhi09v/vsd-hdp/assets/120673607/c35dfa85-09c8-49e5-b7d8-7b2997cb11fc)

I used the below commands to carry out GLS of ternary_operator_mux.v:
	
```bash
iverilog <path to verilog model: ../mylib/verilog_model/primitives.v> <path to sky130_fd_sc_hd__tt_025C_1v80.lib: ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib> <name netlist: ternary_operator_mux_net.v> <name testbench: tb_ternary_operator_mux.v>
./a.out
gtkwave tb_ternary_operator_mux.vdc
```	
	
Below is the screenshot of the obtained simulation, and this matches with pre-synthesis simulation:
	
![Screenshot from 2023-07-08 13-55-20](https://github.com/abhi09v/vsd-hdp/assets/120673607/01d2fd5c-4704-459d-878d-d2be25cdc30c)	
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

![Screenshot from 2023-07-08 14-00-35](https://github.com/abhi09v/vsd-hdp/assets/120673607/b075f56f-93df-4104-8379-a96d853e4b69)


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

![Screenshot from 2023-07-08 14-04-13](https://github.com/abhi09v/vsd-hdp/assets/120673607/1796fa73-ef26-4e9a-a2b5-f03c187cd550)

	
Below is the screenshot of the obtained netlist:

![Screenshot from 2023-07-08 14-07-12](https://github.com/abhi09v/vsd-hdp/assets/120673607/76c79bb6-82a8-4399-8099-3f6b9e641ec7)
	
I used the below commands to carry out GLS of bad_mux.v:
	
```bash
iverilog <path to verilog model: ../mylib/verilog_model/primitives.v> <path to sky130_fd_sc_hd__tt_025C_1v80.lib: ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib> <name netlist: bad_mux_net.v> <name testbench: tb_bad_mux.v>
./a.out
gtkwave tb_bad_mux.vdc
```	
	
Below is the screenshot of the obtained simulation, and this mismatches with pre-synthesis simulation:
	
![Screenshot from 2023-07-08 14-22-54](https://github.com/abhi09v/vsd-hdp/assets/120673607/2589ded1-e1b6-4962-8684-0669b1d1e77d)
	
</details>

<details>
 <summary> Simulation, synthesis, and GLS: blocking_caveat.v </summary>
Below is verilog code for blocking_caveat.v
	
![Screenshot from 2023-07-08 14-38-16](https://github.com/abhi09v/vsd-hdp/assets/120673607/0613687a-2e5b-47cb-8051-d471b8b7f012)
I used the below commands to simulate the design of blocking_caveat.v:
	
```bash
iverilog <name verilog: blocking_caveat.v> <name testbench: tb_blocking_caveat.v>
./a.out
gtkwave tb_blocking_caveat.vdc
```	

Below is the screenshot of the obtained simulation, and as we can see d is seeing the precious values, and hence it is acting as if there was a flop in the circuit which is not the case (incorrect behavior):

![Screenshot from 2023-07-08 14-38-42](https://github.com/abhi09v/vsd-hdp/assets/120673607/879fcef7-5b00-4939-ab99-488be704f742)

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

![Screenshot from 2023-07-08 14-42-29](https://github.com/abhi09v/vsd-hdp/assets/120673607/4ced99de-6542-406c-96b4-444f62230452)

	
Below is the screenshot of the obtained netlist:

![Screenshot from 2023-07-08 14-44-54](https://github.com/abhi09v/vsd-hdp/assets/120673607/015f3a49-dbcc-4ba6-83a1-33adce21108e)


I used the below commands to carry out GLS of blocking_caveat.v:
	
```bash
iverilog <path to verilog model: ../mylib/verilog_model/primitives.v> <path to verilog model: ../mylib/verilog_model/sky130_fd_sc_hd.v> <name netlist: blocking_caveat_net.v> <name testbench: tb_blocking_caveat.v>
./a.out
gtkwave tb_blocking_caveat.vdc
```	
	
Below is the screenshot of the obtained simulation, and this mismatches with pre-synthesis simulation due to blocking statement:
	
![Screenshot from 2023-07-08 14-47-18](https://github.com/abhi09v/vsd-hdp/assets/120673607/44343f97-61c1-4e33-bbac-2a7b6fe95e9a)

	
</details>


## Day 5
	
<details>
 <summary> Summary </summary>

- "if" statements are used to convey priority logic (ony one portion can be executed), and the hardware will look like a series of muxes in hardware, but in "case" statements there is no inferred priotity (sequential execution can mean multiple portions can be executed) 

- Also the hardware would be a series of muxes. Inferred latches can occur if there is an incomplete "if" statement (no else), in this case the hardware will have a latch storing a previous output value. This is bad coding example unless the latch is intended (like in case of a counter). Incomplete "case" can lead to inferred latches too, and to avoid that code the "case" with a default. 

- Another caveat of "case" statements is partial assignments which also creates inferred latches, and to avoid that we should assign all the outputs in all the segments of the case. In "case" statements, one must be careful that portions should not be overlapping otherwise they could be executed due to the sequential non-prioritized execution of those statement.

Then I have learned about looping constructs: for loop (inside always block) and generate for loop (cannot be used inside always block). The for loop is used to evaluate expressions in blocking format (provides code efficiency as complexity of circuits increases) while the generate for loop is used to instantiate hardware (provides code efficiency when hardware instantiation increases in complexity). 
	
</details>

	
<details>
 <summary> Simulation and synthesis: incomp_if.v </summary>
Below is Verilog Code incomp_if.v:
![Screenshot from 2023-07-08 19-09-43](https://github.com/abhi09v/vsd-hdp/assets/120673607/41ca1d22-e328-481a-9336-12167cef00b0)

I used the below commands to simulate the design of incomp_if.v:
	
```bash
iverilog <name verilog: incomp_if.v> <name testbench: tb_incomp_if.v>
./a.out
gtkwave tb_incomp_if.vdc
```	

Below is the screenshot of the obtained simulation, we can see that there is an inferred latch as output is latching to a constant value when select is not high:

![Screenshot from 2023-07-08 19-01-59](https://github.com/abhi09v/vsd-hdp/assets/120673607/28b8f474-3330-4054-be7f-3568c2dc2812)


I used the below commands to view the synthesized design of incomp_if.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: incomp_if.v>
yosys> synth -top <name: incomp_if>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
	
Below is the screenshot of the obtained design, and a latch is seen as was expected:

![Screenshot from 2023-07-08 19-06-43](https://github.com/abhi09v/vsd-hdp/assets/120673607/6b6a0464-909e-4692-9b8d-c678bf0cf352)

</details>
	
<details>
 <summary> Simulation and synthesis: incomp_if2.v </summary>
	
Below is Verilog Code incomp_if.v
![Screenshot from 2023-07-08 19-11-00](https://github.com/abhi09v/vsd-hdp/assets/120673607/99984bd1-11a8-44f1-bd1d-5e3d7de928d2)
I used the below commands to simulate the design of incomp_if2.v:
	
```bash
iverilog <name verilog: incomp_if2.v> <name testbench: tb_incomp_if2.v>
./a.out
gtkwave tb_incomp_if2.vdc
```	

Below is the screenshot of the obtained simulation, we can see that the output latches a constant value when i0 and i2 are zero:

![Screenshot from 2023-07-08 19-21-00](https://github.com/abhi09v/vsd-hdp/assets/120673607/4ce2cdef-5191-4bd1-83a4-6238d2c677f9)


I used the below commands to view the synthesized design of incomp_if2.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: incomp_if2.v>
yosys> synth -top <name: incomp_if2>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
	
Below is the screenshot of the obtained design, and we can see a latch as was expected:

![Screenshot from 2023-07-08 19-22-33](https://github.com/abhi09v/vsd-hdp/assets/120673607/e1e86a1a-e536-4f20-b3db-c156afd059c4)

</details>

<details>
 <summary> Simulation and synthesis: incomp_case.v </summary>
Below is Verilog code for incomp_case.v, comp_case.v ,partial_case_assign.v ,bad_case.v
![Screenshot from 2023-07-08 19-29-08](https://github.com/abhi09v/vsd-hdp/assets/120673607/be11aacc-2b6c-4a0f-8fc2-6d9d49541a32)

I used the below commands to simulate the design of incomp_case.v:
	
```bash
iverilog <name verilog: incomp_case.v> <name testbench: tb_incomp_case.v>
./a.out
gtkwave tb_incomp_case.vdc
```	

Below is the screenshot of the obtained simulation, we can see that the output latches a constant value when select has a vlaue of 2 or 3 (when sel[1] is 1):

![Screenshot from 2023-07-08 19-42-33](https://github.com/abhi09v/vsd-hdp/assets/120673607/a866aca7-0e7b-431a-b7c0-6c11d84b077b)


I used the below commands to view the synthesized design of incomp_case.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: incomp_case.v>
yosys> synth -top <name: incomp_case>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
	
Below is the screenshot of the obtained design, and we can see a latch as was expected:
	

![Screenshot from 2023-07-08 19-44-33](https://github.com/abhi09v/vsd-hdp/assets/120673607/2b5a8b1f-72bd-4dbe-b888-4be337d6a226)

</details>

<details>
 <summary> Simulation and synthesis: comp_case.v </summary>

I used the below commands to simulate the design of comp_case.v:
	
```bash
iverilog <name verilog: comp_case.v> <name testbench: tb_comp_case.v>
./a.out
gtkwave tb_comp_case.vdc
```	

Below is the screenshot of the obtained simulation, we can see that the output follows i2 when select has a value of 2 or 3 (when sel[1] is 1):

![Screenshot from 2023-07-09 21-07-49](https://github.com/abhi09v/vsd-hdp/assets/120673607/caa6c34b-7b59-4ec2-b2c2-51d25b6a52d9)

I used the below commands to view the synthesized design of comp_case.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: comp_case.v>
yosys> synth -top <name: comp_case>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
	
Below is the screenshot of the obtained design, and we do not see a latch as was expected:
	
![Screenshot from 2023-07-09 21-10-33](https://github.com/abhi09v/vsd-hdp/assets/120673607/eaad1d46-f1cb-4c02-93ee-65d073f78f0c)

</details>
	
<details>
 <summary> Synthesis: partial_case_assign.v </summary>

I used the below commands to view the synthesized design of partial_case_assign.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: partial_case_assign.v>
yosys> synth -top <name: partial_case_assign>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> show
```
	
Below is the screenshot of the obtained design, and we see one latch for x output as was expected, and the boolean expressions of x and y that were expected are also inferred by the design obtained:

![Screenshot from 2023-07-10 12-02-42](https://github.com/abhi09v/vsd-hdp/assets/120673607/733cdff9-9c21-4c28-9edf-644044fad616)

</details>

<details>
 <summary> Simulation, synthesis, and GLS: bad_case.v </summary>

I used the below commands to simulate the design of bad_case.v:
	
```bash
iverilog <name verilog: bad_case.v> <name testbench: tb_bad_case.v>
./a.out
gtkwave tb_bad_case.vdc
```	

Below is the screenshot of the obtained simulation, we can see that when sel is "11", the simulator is getting confused and output y is taking a constant "1" value:
	
![Screenshot from 2023-07-10 12-05-11](https://github.com/abhi09v/vsd-hdp/assets/120673607/b524cd47-6759-4d18-bda0-2b2c69b169c1)


I used the below commands to synthesize and view the synthesized design of bad_case.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: bad_case.v>
yosys> synth -top <name: bad_case>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> write_verilog -noattr bad_case_net.v
yosys> show
```
	
Below is the screenshot of the obtained design, and there is no inferred latch:
	
![Screenshot from 2023-07-10 12-08-08](https://github.com/abhi09v/vsd-hdp/assets/120673607/a1dfe5f1-1543-4a7e-812d-492d597c8d9a)

	
I used the below commands to carry out GLS of bad_case.v:
	
```bash
iverilog <path to verilog model: ../mylib/verilog_model/primitives.v> <path to verilog model: ../mylib/verilog_model/sky130_fd_sc_hd.v> <name netlist: bad_case_net.v> <name testbench: tb_bad_case.v>
./a.out
gtkwave tb_bad_case.vdc
```	
	
Below is the screenshot of the obtained simulation, and this mismatches with pre-synthesis simulation. When sel is "11", y takes value of i3 and no latching happens here:
	
![Screenshot from 2023-07-10 12-05-11](https://github.com/abhi09v/vsd-hdp/assets/120673607/7caa45a0-58c7-49cf-a3c3-0622a0d4dbb6)


</details>

<details>
 <summary> Simulation, synthesis, and GLS: mux_generate.v </summary>

I used the below commands to simulate the design of mux_generate.v:
	
```bash
iverilog <name verilog: mux_generate.v> <name testbench: tb_mux_generate.v>
./a.out
gtkwave tb_mux_generate.vdc
```	

Below is the screenshot of the obtained simulation, we can see it's a 4:1 mux functionality:
	
![Screenshot from 2023-07-10 19-20-58](https://github.com/abhi09v/vsd-hdp/assets/120673607/741e0e51-c441-4eaa-b62e-1770a74e962f)


I used the below commands to synthesize and view the synthesized design of mux_generate.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: mux_generate.v>
yosys> synth -top <name: mux_generate>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> write_verilog -noattr mux_generate_net.v
yosys> show
```
	
Below is the screenshot of the obtained design, and it is a 4:1 mux:
	

![Screenshot from 2023-07-10 19-22-42](https://github.com/abhi09v/vsd-hdp/assets/120673607/85579aa6-9690-4058-ae4f-2b284f25a88b)

I used the below commands to carry out GLS of mux_generate.v:
	
```bash
iverilog <path to verilog model: ../mylib/verilog_model/primitives.v> <path to verilog model: ../mylib/verilog_model/sky130_fd_sc_hd.v> <name netlist: mux_generate_net.v> <name testbench: tb_mux_generate.v>
./a.out
gtkwave tb_mux_generate.vdc
```	
	
Below is the screenshot of the obtained simulation, and this matches with pre-synthesis simulation:

![Screenshot from 2023-07-10 19-27-28](https://github.com/abhi09v/vsd-hdp/assets/120673607/6f240db6-f89d-4c7e-be0a-5d46722290e3)


</details>
	
<details>
 <summary> Simulation: demux_case.v </summary>

I used the below commands to simulate the design of demux_case.v:
	
```bash
iverilog <name verilog: demux_case.v> <name testbench: tb_demux_case.v>
./a.out
gtkwave tb_demux_case.vdc
```	

Below is the screenshot of the obtained simulation, we can see it's a 1:8 demux functionality:
	
![Screenshot from 2023-07-10 19-30-43](https://github.com/abhi09v/vsd-hdp/assets/120673607/7438c7fe-37a1-43a5-8e4e-e58cf8accfbd)



</details>
	
<details>
 <summary> Simulation, synthesis, and GLS: demux_generate.v </summary>

I used the below commands to simulate the design of demux_generate.v:
	
```bash
iverilog <name verilog: demux_generate.v> <name testbench: tb_demux_generate.v>
./a.out
gtkwave tb_demux_generate.vdc
```	

Below is the screenshot of the obtained simulation, we can see it's a 1:8 demux functionality (same as demux_case.v):
	
![Screenshot from 2023-07-10 19-33-03](https://github.com/abhi09v/vsd-hdp/assets/120673607/d78ccfd3-adcf-4f9c-be8b-3e62a306a954)



I used the below commands to synthesize and view the synthesized design of demux_generate.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: demux_generate.v>
yosys> synth -top <name: demux_generate>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> write_verilog -noattr demux_generate_net.v
yosys> show
```
	
Below is the screenshot of the obtained design, and it is a 1:8 demux:
	
![Screenshot from 2023-07-10 19-34-55](https://github.com/abhi09v/vsd-hdp/assets/120673607/44415ce5-2230-4a68-b44b-01c51116abfa)


I used the below commands to carry out GLS of demux_generate.v:
	
```bash
iverilog <path to verilog model: ../mylib/verilog_model/primitives.v> <path to verilog model: ../mylib/verilog_model/sky130_fd_sc_hd.v> <name netlist: demux_generate_net.v> <name testbench: tb_demux_generate.v>
./a.out
gtkwave tb_demux_generate.vdc
```	
	
Below is the screenshot of the obtained simulation, and this matches with pre-synthesis simulation:

![Screenshot from 2023-07-10 19-36-57](https://github.com/abhi09v/vsd-hdp/assets/120673607/ba94d517-9e97-4d82-afdf-9d354c3109af)



</details>
	
<details>
 <summary> Simulation, synthesis, and GLS: rca.v </summary>

I used the below commands to simulate the design of rca.v:
	
```bash
iverilog <name verilog: rca.v> <name verilog: fa.v> <name testbench: tb_rca.v>
./a.out
gtkwave tb_rca.vdc
```	

Below is the screenshot of the obtained simulation, we can see it's an 8-bit RCA functionality:
	
![Screenshot from 2023-07-10 19-44-58](https://github.com/abhi09v/vsd-hdp/assets/120673607/871776e9-b360-443b-a945-8cdd81fc34ba)


I used the below commands to synthesize and view the synthesized design of rca.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: rca.v>
yosys> read_verilog <name of verilog file: fa.v>
yosys> synth -top <name: rca>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> write_verilog -noattr rca_net.v
yosys> show
```
	
Below is the screenshot of the obtained design, and it is an 8-bit RCA:

![Screenshot from 2023-07-10 19-34-55](https://github.com/abhi09v/vsd-hdp/assets/120673607/d2246f3d-5e20-4483-93e2-168d3d3e75d9)

I used the below commands to carry out GLS of rca.v:
	
```bash
iverilog <path to verilog model: ../mylib/verilog_model/primitives.v> <path to verilog model: ../mylib/verilog_model/sky130_fd_sc_hd.v> <name netlist: rca_net.v> <name testbench: tb_rca.v>
./a.out
gtkwave tb_rca.vdc
```	
	
Below is the screenshot of the obtained simulation, and this matches with pre-synthesis simulation:

![Screenshot from 2023-07-10 19-55-02](https://github.com/abhi09v/vsd-hdp/assets/120673607/b9576a10-628f-4582-8e8d-6d06df03fcc0)

</details>
	
