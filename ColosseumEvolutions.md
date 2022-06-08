# Evolution of Colosseum
New and upcoming features

**What capabilities do you think are missing? what would you like to use in research?**
6GHz limit for mmWave - remember, upgrade to SRNs && MCHEM
slicing and newer cellular (srsRAN, OAI, 
Channel sonuding technique (generate RF data for external training. limited but can be done one-on-one)
logging doesn't support... latency (PHY?)? layer-dependent, tool-dependent. i.e. srsLTE buffer information for E2E latency

NS3 integration?
* independent approaches to problem
* NS3 **has** integrations for sockets to SDRs
* NS3 presentation on integrating with POWDER

**reminder: YOU define the experiment, stack, parameters. They have prebuilt scenarios, but it's your responsibility otherwise**



## OpenRAN Gym
[openrangym.com]
**need testing of closed-loop control without compromising network performance**
* PRAN enables feedback, optimization, control of RF environemtn
* OpenRAN GYm:  can use to collect datasets on RF scenarios, develop ML solutions for O-RAN, test & refine experimental platforms

Components
* O-RAN compliant near-realtime RIC on Colosseum. Docker cluster & control architecture
  * RIC: simplified from O-RAN RIC for colosseum containers
* RAN Framework for data collection and control of base stations (srsRAN based, an use OAI somewhat)
  * Python scripts / APIs for automatically running large-scale experiments
  * Collect data in RF Scenarios (path loss, position/distnance of BS/UEs, mobility/speed, ...)
  * Traffic flows & types among nodes
  * Cellular scenarios (as discussed yesterday) (ray-traced / high simulation)
* Programmable protocol stacks (srsRAN)
* Public, experimental platforms (colosseum, arena, POWDER, ....)

Worflow
!. Data collection (SCOPE, batch jobs)
2. Training (Colosseum DGX (nt yet), SRN or local GPU)
3. Deploy model as xApp (CoIO-RAN, colosseum)
...

**xApp is key**
* service-model connector: gives simulation data, takes control actions. Users develop AI logic (inference, prediction ,control...)
* the rest is handles by ORG

**RIC developed as base container available for Colosseum!!!!**

EX experiment: 7 EnBs, 42 UEs, 3 slices....




## AI JumpStart Rack
Hardware additions for colosseum, reservations!
* 2x NVIDIA DGXA100: 8 GPUs
* 1 large-memory node (~3 TB RAM)
**Goal: bring code into colosseum, process remotely for IQ samples, etc**

Fully-integrated with colosseum, soon may reserve SRNs and GPUs
orchestration via NOMAD
* GPU Images via docker (not LXC)

**Currenlty onboarding Beta Users. email villa.d@northeastern.edu and m.polese@northeaster with subject [Colosseum AI] Test account request**
* general availability in late-end of summer





## National Radio Dynamic Zones (NRDZ)
Spectrum is a limited resource with many uses -> improving coexistence.
**create playground for spectrum experiments that are not meeting current regulations**
* Colosseum is an emulation platform, controlled, develop scenarios for this simulation without risk of actual interference in repeatedable technique
* Need to extend capabiliites of colosseum.
** Current limitations: 1km^2, sub-6GHz, omnidirectional**
New cpabilities: 100km^2, 16x16, mmwave & directional signals
* **Scenarios being developed FOR long-range communications with SDRs. Sometime in Fall?**

NRDZ&Colosseum
* Coexistence with generic waveforms: colosseum is SD, able to safely study interference of WiFi/5G/Radar/etc on one oanohter
* near-far interference issues: out-of-band emissions affecting e.g. airplane signal due to adjacent cellular
* Spectrum-sharing

Next steps: **looking for input with spectrum shairng et al**
* mmwave scenario modelling: start to support dynamic (RF) modelling for directional scenarios. While maintaining realtime requirements
* mmwave MCHEM architecture. identify FPGA design * RF frontends for bandwidth needs......

sensitivt y of USRP v scientific equivalent: amplification to match gain/snr, not necessarily P_rx
