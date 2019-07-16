# pktr_preserved
********************
     #README
********************  
Author: msfrigau@iastate.edu  chenye@iastate.edu
Institute: Iowa State University  
Lab: Dependable Networking and Computing  

# MAC scheduler pktR for oaisim

pktR is a joint interference and power control MAC scheduler for cellular networks.  
pktR objective is to guarantee per-packet reliability in the presence of co-channel interference.  
It leverages the PRK (physical-ratio-K) interference model: [PRKS paper]{www.ece.iastate.edu/~hongwei/group/publications/PRKS-TWC.pdf}  
This version of the code has been implemented under OpenAirInterface5G v0.5.2 - oaisim - tag: large_scale_simulations.

## Run automated overwrite on edited files 
1. cp ~/pktr-UCS/files-overwrite $YOUR_OPENAIR/files-overwrite
2. source oaienv
3. chmod +x files-overwrite
4. ./files-overwrite
5. sudo mkdir /opt/pktr/
6. sudo touch /opt/pktr/txpower-tx
7. sudo touch /opt/pktr/context-cell

## Getting Started
1. Clone the repo:
```git clone https://github.com/chenye-iastate/pktr-UCS.git```
2. Build oaisim:
```cd cmake_targets```
```./build_oai --oaisim -t ETHERNET --noS1```
3. Run oaisim with a template file:
```sudo -E ./oaisim_nos1 -O ~/pktr/targets/PROJECTS/GENERIC-LTE-EPC/CONF/enb.band7.tm1.generic.oaisim.local_no_mme.conf -H -a -c template_34.xml -F -R 25 2>&1 | tee tmplog.log```

### Overview of pktR code structure

*** List of files modified in targets/SIMU/USER:  
- oaisim.c  
- oaisim_functions.c
- channel_sim.c  

*** List of files modified in openair1/PHY/:
- impl_defs_top.h

*** List of files modified in openair2/LAYER2/MAC:
- defs.h: structure definitions and constants such as SINR and reliability thresholds
- pre_processor.c: pktR functions
- proto.h : functions declared for pre_processor.c
 
*** List of files modified in openair2/LAYER2/PDPC:
- pdcp.c: track sequence numbers at PDPC layer to compute link reliability

*** Main functions of pktR ***
+ void sched_controller_pktR(int UE_id, module_id_t Mod_id, uint8_t CC_id, frame_t frameP, sub_frame_t subframeP): Triggered every ITTI and creates 3 different threads that run in parallel
+ void *get_meas_pktR (void *params): Periodically collects in-situ measurements such as NI values of current UE at different time instants
+ void *process_tpc_pktR (void *params): Compute TX power level that will be used by the transmitter in the next subframe
+ void *update_value_K_pktR (void *params): Update K value of each UE


## Authors

* **Matthias Sander-Frigau** - *git repo* - [github](https://github.com/hongmat/pktr.git)






# pktr-preserved