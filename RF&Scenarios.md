# RF & TRAFFIC SCENARIOS OVERVIEW
*Lenoardo Bonati*

## What are scenarios
 * Emulation at-scale: 256 trxs, > 65k Channels (many SDRs allocated toward)
 * diverse conditions: fading, mobility, topologies. MANDATORY
 * Data traffic: dl/ul, bandwidth, bitrate, udp/tcp... OPTIONAL. CAN CREATE TRAFFIC BY OTHER MEANS.
 
Scenarios are deterministic, extended w/ stochastic distribtions via the filter taps on FPGAs
enables:
 * full control of channel. nonstationarity, 
 * totally reproducible, maintain only conditions of interest..
   * easy to compare algoirhtms as a result
   
## High-level overview

Main components
 * Standard Radio Nodes (SRNs) - radio front-end. container  emulates server and RF. USRPX310.
 * Massive Channel Emulator (MCHEM) - specify scenario.
   * Utilizes X310s for simulations. 
     * Wired to precent undesired simulation interference. 
     * 1:1 for each SRN.
     * *benefit*: using licensed spectrum without worry. 
     * *benefit*: isolate experiments for reproducibility.
   * channel degredation & inter-node interference
   * all SRNs connect to MCHEM for RF TX/RX
 * Traffic Generator (TGEN) - multiple enginer generator (MGEN, USNAavy)
   * easily select traffic scenarios (i.e. video traffic). define traffic flows (bitrate, packetsize/distr, ...)
   * All nodes connect to TGEN, which allocates packets to each SRN
 * RXs sum all signals... from MCHEM


### Wireless Channel Modeeling
 * Via FIR Filter taps (512 in FPGas) 
 * stored in scenario server
 * **channel is emulated by MCHEM**
   1. SRN generates signals
   2. MCHEM applies taps (FPGA, realtime)
   3. Signals forwarded to SRNs
 
**Why FIR Filters**
* convenient for channel impulse response
* each channel indvidually simulated. 255 SRNs x 255 SRNs

**Northeastern's approaches to generate filter taps scenarios**
* *mathematical models*: determinisic + stochastic. no ground truth, but low-complexity
* *on-site measruements*: collected with actual radio nodes prior. realistic, but site-time specific.
* *sofware-based (ray traced)*: via matlab or WirelessInsight, other tools. paramterize the building/layout & tracing complexity. accurate, reproducible, complex, time-consuming to define.
* *reccomended: initially define ray tracer, callibrate with on-site measurements.*
* on-site and ray tracing approxiate taps. 512->4 via KMeans to select  reduce?*

**Complexity v accuracy**
* 512-tap filters, sparse (only 4 used)
* high-complexity of taps, balance storage & realtime performance
* i.e.: 50 nodes, single-tap, 10 minute duration. need 100 GB for FIRs, 2 hours to generate on server/workstation.







# Scenario Components
## RF
**Can specify**
* Bandwidth (<80 MHz), SNR, Frequency (Check 310 specs), # of nodes
## Traffic
Based on Multi-Generator (MGEN) [github: USNAvalResearchLaboratory/mgne]
* generate TCP/UDP traffic, open-source.
* specifiy: source, 
* Other options: iPerf2/3, Netperf, MTR, bypass TGEN


### Example: Alleys o fAustin Scenario
* Platoon of National Guard at Camp Mabry, practicing urban maneuvers. 
* walking in straight lines, 9 soldiers + 1 UAV
* used for DARPA SC2
* Stages: 
  1. squads progress, voice comm
  2. + video, images
  3. + extra traffic
**Path loss**
* 5 teams/networks
10 nodes: leader, uav, soldiers


### Example: ray-trace of Krentzman Quadrant
* Model of 3D scenario via raytracing
* applied material properties from ITU model
* get channel taps from ray tracer
* Significant time to compute, design, save, simulate

### Example: Cellular from OpenCellID
* for bsae stations in Boston Public Garden
* 6 interfereing base stations, 24 users
* srsLTE manages BSS SRNs
* Pedestrian mobility causing external interference

No K8S clusters, can deploy to nodes and develop 
docker containers/components okay?
Mobility restrictions: must be set prior to scenario, cannot be on-demand. 
  * ray tracer / simulation  /calculation done to define taps through time
  * would need to run operational simulation of scenario (i.e. route defining) prior to scenario
  * not possible ot on-the-fly simulate / adjust mobility due to complexity
