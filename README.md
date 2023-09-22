
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

![image](https://github.com/abhi09v/vsd-hdp/assets/120673607/7ff257df-93f7-461d-b062-74ad285747fa)

I used the below commands to carry out GLS of rca.v:
	
```bash
iverilog <path to verilog model: ../mylib/verilog_model/primitives.v> <path to verilog model: ../mylib/verilog_model/sky130_fd_sc_hd.v> <name netlist: rca_net.v> <name testbench: tb_rca.v>
./a.out
gtkwave tb_rca.vdc
```	
	
Below is the screenshot of the obtained simulation, and this matches with pre-synthesis simulation:

![Screenshot from 2023-07-10 19-55-02](https://github.com/abhi09v/vsd-hdp/assets/120673607/b9576a10-628f-4582-8e8d-6d06df03fcc0)

</details>
	
## Day 6
	
<details>
 <summary> Summary </summary>
	
My design  is 4 stage pipelined desgin where:
- Register Bank containing 16 16-bit Register
- 256*16 Memory
 using 2 phase clock for alternate pipeline stages act as master-slave clock

![Screenshot from 2023-09-15 22-40-12](https://github.com/abhi09v/vsd-hdp/assets/120673607/199be16e-9818-416a-9b58-5f7bf14b9a7a)

</details>	
	
<details>
 <summary> Verilog codes  </summary>	
	### pipe_mem.v
	
![Screenshot from 2023-09-15 22-47-37](https://github.com/abhi09v/vsd-hdp/assets/120673607/996d9dda-26c4-4ee2-a582-ba61d01fe342)
      ### pipe.mem_tb.v
      
 ![Screenshot from 2023-09-15 22-48-00](https://github.com/abhi09v/vsd-hdp/assets/120673607/4f5ae3ce-6070-463d-a6e3-34da40c3a7a4)     
	
	
</details>

<details>
 <summary> Simulation, synthesis, and GLS: pipe_mem.v </summary>

I used the below commands to simulate the design of pipe_mem.v:
	
```bash
iverilog <name verilog: pipe_mem.v> <name testbench: pipe_mem_tb.v>
./a.out
gtkwave pipe_mem_tb.vdc
```	

Below is the screenshot of the obtained simulation, we can see that n 
mem[125] = 8
mem[126] = 24
mem[127] = 14
mem[128] = 3
mem[129] = 3
mem[130] = 38
![Screenshot from 2023-09-15 22-02-13](https://github.com/abhi09v/vsd-hdp/assets/120673607/9eb76033-e6f8-4759-89fa-704e311bdcf3)


	
I used the below commands to synthesize and view the synthesized design of mariam_updown_counter.v:
	
```bash
yosys> read_liberty -lib <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> read_verilog <name of verilog file: pipe_mem.v>
yosys> synth -top <name: pipe_mem>
yosys> dfflibmap -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> abc -liberty <path to sky130_fd_sc_hd__tt_025C_1v80.lib>
yosys> write_verilog -noattr pipe_mem_flat.v
yosys> show
```
	
Below is the screenshot of the obtained design:
	
![Screenshot from 2023-09-15 22-32-07](https://github.com/abhi09v/vsd-hdp/assets/120673607/f328ffd1-77d3-4e5d-957e-569b8dba8098)
![Screenshot from 2023-09-15 22-32-26](https://github.com/abhi09v/vsd-hdp/assets/120673607/3949a930-f9cd-46bb-9633-03c130a558e3)

	

	
```bash
iverilog <path to verilog model: ../mylib/verilog_model/primitives.v> <path to verilog model: ../mylib/verilog_model/sky130_fd_sc_hd.v> <name netlist: pipe_mem_heir.v> <name testbench: pipe_mem_tb.v>
./a.out
gtkwave pipe_mem_tb.vdc
```	
Below is screenshot of Lib cell 
![Screenshot from 2023-09-15 22-32-45](https://github.com/abhi09v/vsd-hdp/assets/120673607/74fbba9f-8e9c-4931-b581-82b99cf05c9d)


Below is the part of code of the generated netlist:

![Screenshot from 2023-09-15 23-07-04](https://github.com/abhi09v/vsd-hdp/assets/120673607/b8e51bbb-4aea-479a-b918-8d4d3a4c582e)
	
</details>

## Day 7
	
<details>
 <summary> Summary </summary>
	
### About Static Timing Analysis (STA).

-  setup time is the time the input data signals must be stable (either high or low) before the active clock edge occurs while hold time is the time the input data signals must be stable (either high or low) after the active clock edge occurs.

-  It is important to note that min delay Thold<=Tcq_inflop+Tcomb) and max delay are two sides of the same coin: if there was no min delay we could have met any max delay requirement by pushing the clock. But when we push the clock we interfere with the time of the "capture" flop.

-  Delay is a function of inflow of current (i.e. input transition): if there is fast current sourcing, we will get less delay (fast-rise). Delay is also a function of load capacitance. The timing arcs in combinational cells consist the delay information from every input pin to every outp-ut pin it can control.

-  The timing arcs in a sequantial cell consist of delay from clock to Q for D flipflop or of delay from clock to Q and that from D to Q in case of a D latch. In both cases, timing arcs in a sequential cell also consist of setup/hold delays from clock to D. Note that setup/hold are around the sampling. Below is a picture noting the timing arcs in the sequential case.
	


Timing paths have start points (input ports or/and clock pins of registers) and end points (output ports or/and D pin of DFF/DLAT). TIming paths always start at one start point and end at one end point: clock to D timing paths are the register-register timing paths, clock to output and input to D timing paths are IO timing paths, input to output timing paths are indesirable to have in designs. A critical path is that which decides or limits the clock frequency because it has the most delay incurred. If I reduce the delay in the critical path, I can increase the freq (fclk=1/Tclk, where Tclk>= Tcq+Tcomb+Tsetup). It is Tclk that decides Tcomb and not vice versa. That is why we need the constraints, 

- 1-) I set the clock period and my synthesis tool will work to optimize the register-register time paths (by choosing what cells to connect in combinational logic part) to meet that constraint (Tcq and Tsetup are picked up from .lib) => register-register timing paths are constraints by clock.
- 2-) I need to contstraint the input delay in order to have synchronous paths (from external registers to internal registers) working on the same clock => input-register timing paths are constrained by the input external delay and clock.
- 3-) I have to constraint the output delay as to have synchronous paths (from internal registers to external registers) working on the same clock => register-output timing paths are constrained by the output external delay and clock. Note that register-output and input-register timing paths are collectively the IO timing paths (delay associated to them is called IO delay modeling). IO pathes also need to be constrained for max delay (setup) and min delay (hold).

READ THIS- https://drive.google.com/file/d/1I4HeX4hvjyyLUUMmaqOJCj9G2gaZaMYS/view?usp=drive_link
	
Rule of thumb: The Tclk is 70% for external delay and 30% internal delay. The synthesis tools infer the cell delay numbers from the .lib file.
	
Some .lib file terminologies: 
	
- 1-) max capacitance limit: determines the maximum loading capacitance, beyond which the tool should break out the circuit and add buffers.
	
- 2-) delay model lookup table: delay = f(input transition+output load), so for every cell there is a table that has the delay values corresponding to a pair of input transition value (index_2) and output load value (index_1). Numbers not in table are interpolated.
	
- 3-) _and2_2 is wider, faster and has more area than _and2_0
		
- 4-) unateness: positive unateness is when the input rises (one pin), the output either does not change or it also rises (true for AND, OR). Negative unateness is when the input rises (one pin), the output either does not change or it falls (true for NOT, NAND, NOR). XOR is non-unate as input rise in one pin can cause either rise or fall in output. In D fliflop, with respect to clock, Q has no unateness, but with respect to  => This information is used by the tool to propagate the signal. 
	
- 5-) timing_type: combinational or falling_edge/rising_edge conveys combinatioal or sequential logic respectively. setup_falling and setup_rising indicate that the setup time is measured at the falling and rising edge respectively. 
	
	
</details>
	
<details>
 <summary> Useful dc shell commands </summary>	
	
In order to show the library name, the file name, and the path, use the following command:
	
```bash
list_lib
```	
	
In order to list the library cells of name containing "and" (this includes nand) one by one, use the following commands:
	
```bash
foreach_in_collection <looping variable: my_lib_cell> [get_lib_cells */*and] {
set <another variable: my_lib_cell_name [get_object_name $my_lib_cell];
echo $my_lib_cell_name; 
}
```

In order to view pins of a library, display direction (this is the attribute we are querying, when =2 it means pin is output) of all pins we use the following commands:
	
```bash
get_lib_pins <path/librarycellnamefromprevcommand/*> 
foreach_in_collection <variable name: my_pins> [get_lib_pins <path/librarycellnamefromprevcommand/*>] {
set <another variable: my_pin_name [get_object_name $my_pins];
set <another variable: pin_dir> [get_lib_attribute $my_pin_name <attribute name:direction>];
echo $my_pin_name $pin_dir;
}
```
	
In order to select a pin (output) and view its functionality and area, then view the capacitance of a certain pin, check whether it is a clock pin or not, use the following commands:

```bash
get_lib_attribute <nameoflibpinfromprevcommands> function
get_lib_attribute <nameofcellfromprevcommands> area
get_lib_attribute <nameoflibpinfromprevcommands> capacitance
get_lib_attribute <nameoflibpinfromprevcommands> clock
```

In order to find output pin and its functionality for each cell in a list, write a script (my_script.tcl, shown below), then source it:
	
```bash
set <variable name: my_list> [list <path/librarycellnamefromprevcommand \
path/librarycellnamefromprevcommand\
path/librarycellnamefromprevcommand ]
foreach <variable name: my_cell> $my_list {
	foreach_in_collection <variable name: my_lib_pin> [get_lib_pins $(my_cell)/*] {
		set <variable name: my_lib_pin_name> [get_object_name $my_lib_pin];
		set <variable name: a> [get_lib_attribute $my_lib_pin_name direction];
		if { $a > 1 } {
			set <variable name: fn> [get_lib_attribute $my_lib_pin_name function];
			echo $my_lib_pin_name $a $fn;
		}
	}
```

In order to find out all cells in library that are sequential, use the following command:

```bash
get_lib_cells */* -filter "is_sequential == true"
```

In order to write all available attributes that can be used to do queries to a file, use the following command:
	
```bash
list_attributes -app > <name: a>
```
READ THIS -https://drive.google.com/file/d/1ljPXmtvoDVkRASEjanTJkIf4UajWXaR5/view?usp=drive_link	
</details>
	
## Day 8
	
<details>
 <summary> Summary </summary>
	
learn advanced constraints. There is a delay in routing the clock which is not seen by the synthesis too due to the below ASIC flow, where clock routing is done post synthesis in the Clock Tree Synthesis (CTS) step shown below:
	
use THIS DOCUMENTATION - https://drive.google.com/file/d/1t8iz_HaQh2ekT3UXeDIiqNJ72Y_M1rd3/view?usp=drive_link
	
- The clock generation happens through aan osillator, PLL, or external clock source. All these sources have inherent varitions in the clock period due to stochastic effects, so a practical clock has a non-zero rise time and there is also gitter (capture edge of clock doesn't come exactly where it is expected and hence capture window is reduced). In a practical clock network, all flops may not see the clock edge at the same instance, this is known as clock skew => These delays should be considered by the sythesis through constraints.
	
- Clock modeling should take into consideration the latency of source (time taken by source to generate the clock), period, clock network latency (time taken by the clock distribution network), clock skew (clock path delay mismatches which causes difference in the arrival of the clock), and jitter. After CTS however, the skew and clock network constraints must be removed. These sources are shown below:
	
	
Important terminology associted with constraints:
	
1-) ports: primary IOs of the design 
	
2-) nets: interconnections between pins (associated with gates) or between pin and port. A net can have only one driver (multidriven nets are corrupted, but within one standard cell this is fine as the sizing decides what drives what etc like in a latch), but it can drive multiple pins.
	
</details>
	

<details>
 <summary> Useful dc shell commands </summary>	
	
To query the pins, check whether a pin is a clock, check all clocks reaching a pin (here clocks created by commands further down will be shown), and query ports in dc shell, use the following commands (note that port, pin, clock, etc are all names which are case sensitive):
	
```bash
get_pins *;
get_attribute [get_pins <pin_name>] clock;
get_attribute [get_pins <pin_name>] clocks;
get_ports clk;
get_ports *clk*;
get_ports *;
get_ports * -filter "direction == in";
get_ports * -filter "direction == out";	
```

To query the clocks in dc shell (if is_generated is false this means the clock is a master clock), use the following commands:
	
```bash
get_clocks *;
get_clocks *clk*;
get_clocks * -filter "period > 10";
get_attribute [get_clocks my_clk] is_generated;	
report_clocks my_clk;
```
	
To query the physical and/or hierarchical cells (and check whether they are hierarchical or physical) in dc shell, use the following commands:
	
```bash
get_cells * -hier;
get_attribute [get_cells u_combo_logic] is_hierarchical; 	
get_attribute [get_cells u_combo_logic/U1] is_hierarchical;
```
	
To create a clock in dc (note: clocks must be created on the clock generators like PLL or oscillators or primary IO pins for external clocks. Clocks must not be created on hierarchical pins because they are not clock generators) (note2: creating a clock by default assumes a 50% duty cycle and starting phase is high, to change the phase/duty cycle use -wave {1st_rise_edge_time 2nd_rise_edge_time} with the first command), set the network latency and uncertainty constraints (remember to reduce the uncertatinty delay post CTS to reflect only jitter, and by default the comand below sets the setup uncertainty value), use the following commands:

```bash
create_clock -name <clock name> -per <period> [get_ports <clock definition point i.e. pin to define clock at>];
set_clock_latency <latency> [get_clocks <name of clock>];
set_clock_uncertainty <value of jitter and skew delay> [get_clocks <name of clock>];
```
	
To set the IO path constraints (the values are with respect to the clock edge, i.e. after it), use the following commands: 

```bash
set_input_delay -max <value> -clock [get_clocks <name clock>][get_ports <name of all input ports, use *>];
set_input_delay -min <value> -clock [get_clocks <name clock>][get_ports <name of all input ports, use *>];
set_input_transition -max <value> [get_ports <name of all input ports, use *>];
set_input_transition -min <value> [get_ports <name of all input ports, use *>];
set_output_delay -max <value> -clock [get_clocks <name clock>][get_ports <name of all output ports, use *>];
set_output_delay -min <value> -clock [get_clocks <name clock>][get_ports <name of all output ports, use *>];
set_output_load -max <value> [get_ports <name of all output ports, use *>];
set_output_load -min <value> [get_ports <name of all output ports, use *>];
```
	
</details>
	
<details>
 <summary> Inserting constraints: lab8_circuit.v </summary>	
	
To create a clock, use the following command:
	
```bash
create_clock -name <clock name: MYCLK> -per <period: 10> [get_ports <name: clk>];
```
	
The design of the circuit after creating the master clock (which will propagate to all clock pins because it is created at clk pin which is connected to all clock pins in the circuit) is shown below:
	
![image](https://github.com/abhi09v/vsd-hdp/assets/120673607/1e18af5a-1e5f-4d94-a983-1bdf78155902)
	
To remove the already created clock then create another clock with a modified waveform, then view it, use the following command:
	
```bash
remove_clock <name: MYCLK>
create_clock -name <clock name: MYCLK> -per <period: 10> [get_ports <pin name: clk>] -wave {<1st rising edge:5> <second rising edge: 10>};
report_clocks *;
```
	
To add constraints to the clock created above (model source clock latency and clock network attributes) and print the timing report for setup and hold, use the following commands:
	
```bash
set_clock_latency -source <latency: 1> [get_clocks <name of clock: MYCLK>];
set_clock_latency <latency: 1> [get_clocks <name of clock: MYCLK>];
set_clock_uncertainty -setup <value of jitter and skew delay: 0.5> [get_clocks <name of clock: MYCLK>];
set_clock_uncertainty -hold <min value of jitter and skew delay: 0.1> [get_clocks <name of clock: MYCLK>];
report_timing -to <destination pin name: REGC_reg/D> -delay max;
report_timing -to <destination pin name: REGC_reg/D> -delay min;
```


	
In order to report information about all ports (for now we will not see transition delays on input ports and no delays due to loads on output ports), use the command below:
	
```bash
report_port -verbose;
```

To add IO constraints (for external input delay, input transitions, external output delay, and output loads) and print the timing report, use the commands below:
	
```bash
set_input_delay -max <value: 5> -clock [get_clocks <name: MYCLK>] [get_ports <name: IN_A>];
set_input_delay -max <value: 5> -clock [get_clocks <name: MYCLK>] [get_ports <name: IN_B>];
set_input_delay -min <value: 1> -clock [get_clocks <name: MYCLK>] [get_ports <name: IN_A>];
set_input_delay -min <value: 1> -clock [get_clocks <name: MYCLK>] [get_ports <name: IN_B>];
set_input_transition -max <value: 0.3> [get_ports <name: IN_A>];
set_input_transition -max <value: 0.3> [get_ports <name: IN_B>];
set_input_transition -min <value: 0.1> [get_ports <name: IN_A>];
set_input_transition -min <value: 0.1> [get_ports <name: IN_B>];
report_timing -from <source pin name: IN_A> -trans -cap -nosplit > <name output file: a_trans>;
set_output_delay -max <value: 5> -clock [get_clocks <name: MYCLK>] [get_ports <name: OUT_Y>];
set_output_delay -min <value: 1> -clock [get_clocks <name: MYCLK>] [get_ports <name: OUT_Y>];
set_load -max <value: 0.4> [get_ports <name: OUT_Y>];
report_timing -to <destination pin name: OUT_Y> -trans -cap -nosplit > <name output file: out_load>;
report_port -verbose;
```
	
Below is the timing report reported from IN_A (slack is 4.08 now):
	
![image](https://github.com/abhi09v/vsd-hdp/assets/120673607/e5a95864-e12e-450e-817e-11f932cb85a9)

Below is the timing report reported from OUT_Y (slack is 1.88 now):

![image](https://github.com/abhi09v/vsd-hdp/assets/120673607/c8fd8af3-7d1f-4205-b89c-178632c2e335)


	
A generated clock is used to later on model the propagation delay (flight delay) of the clock seen at the output port. To create a generated clock and constraint the above design taking into consideration the generated clock (modeling the propagation delay), use the following commands:
	
```bash
create_generated_clock -name <name: MYGEN_CLK> -master [get_clocks <name master clock: MYCLK>] -div <division value usefor for clock division: 1> [get_ports OUT_CLK];
set_clock_latency -max <value: 1> [get_clocks MYGEN_CLK];
set_output_delay -max <value: 5> -clock [get_clocks <name: MYGEN_CLK>] [get_ports <name: OUT_Y>];
set_output_delay -min <value: 1> -clock [get_clocks <name: MYGEN_CLK>] [get_ports <name: OUT_Y>];
report_timing -to <destination pin name: OUT_Y>;
```
	
Below is a screenshot of the reported timing:
	
![image](https://github.com/abhi09v/vsd-hdp/assets/120673607/b898ea0e-26a4-450b-b251-4fcc4604513a)
	




</details>
	
<details>
 <summary> Inserting constraints: lab14_circuit.v </summary>
	
The design of this circuit is similar to that of lab18_circuit.v, but has additional inputs, gate and output in the module as shown below:
	
![image](https://github.com/abhi09v/vsd-hdp/assets/120673607/ea5f1094-4bd5-4219-a3b3-4c22eefb6611)

	
We constraint the model above by using all constraints we discussed in lab8_circuit.v and in addition we add either the constraints in the first set of commands below then recompile to get an optimization as our constraint added will lead to a violation (because previous compilation did not take into account this newly added path) that will be fixed post compilation, or we add the constraints that follow in second set of commands which use a virtual clock (either ways work, but note that virtual clock does not need a source name and we need to be careful to have the latency in between equivalent to 0.1ns, and we have that as out of 10ns from virtual clock, 5ns is to input delay and 4.9ns is to output delay hence what is left is 0.1ns for the logic so this is correct):
	
```bash
set_max_latency <value:0.1> -from [all_inputs] -to [get_port OUT_Z];
compile_ultra;
report_timing -to <name: OUT_Z>;
```

```bash
create_clock -name MYVCLK -per <value: 10>;
set_input_delay -max <value: 5> [get_ports <name: IN_C>] -clock [get_clocks MYVCLK];
set_input_delay -max <value: 5> [get_ports <name: IN_D>] -clock [get_clocks MYVCLK];
set_output_delay -max <value: 4.9> [get_ports <name: OUT_Z>] -clock [get_clocks MYVCLK];
compile_ultra;
report_timing -to <name: OUT_Z>;
```
	
Below is the reported timing from the both approaches (one screenshot because both apply same constraints and optimizations and hence we get same timing report):
	

![image](https://github.com/abhi09v/vsd-hdp/assets/120673607/dd5a10bd-547d-490e-a614-202c2d5621db)

	
</details>
