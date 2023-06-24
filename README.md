# VSD-Hardware Design 
This repo contains everything from architectural design to routing. It covers critical steps such as logic synthesis, post-synthesis STA analysis, floor-planning, placement, clock tree synthesis (CTS), and routing.
# Table of Content
[Tool Installation and setup](#Tool-Installation-and-setup)

##Tool-Installation-and-setup

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
 </details>
