# Batch jobs
*Leonardi Bonati*
Batch jobs are the preferred way of using colosseum. instead of operating interactive sessions with each resource, they are set up *a priori* with containers, images, scripts... that automatically executes and exports results to NAS.
 * batch reservations are provided at discount because of scheduling benefits.
 
 **Batch mode**
 * Automatiically controlled by Colosseum
 * Containers must be precondifuredd to use Radio API.
 * Containers **do not have access to the teams' storage**
 * Cannot debug program.
 * Setup via config files
   * Batch COnfiguration: JSON
     * Tells colosseum configuration, scenario, node information...
     * **must be saved to file proxy:** `/share/nas/teamname/batch/`
     * includes: BatchName, Duration (seconds), RFScenario, TrafficScenario, NodeData (mapping of SRNs)
       * NodeData:
         * RFNode_ID: 1-N. NOT SRN ID
         * ImageName: container image name to load (i.e. "modem-image-v1", "")
         * ModemConfig: name of modem config file. can be separate for each node
         * isGateway:  true/false, connects node to hard-wired / interconnection between session nodes
         * TrafficNode_ID: 
         * node_type (legacy SC2): competitor (standard), ot (can SSH into during batch job runtime, for debugging)
   * Modem configuration file: optional
     * pass additional parameters: i.e. altering frequency / parameters of config - things that can change at runtime / during an interactive session.
       * must write your own program to parse thse configs and run
     * **save to:** `/share/nas/teamname/config`
     
## Batch Job Workflow
* wait for resources available
* continers loaded to SRNs
* `colosseum.ini` copied to container (batch job info, i.e. scenario, freq, etc. SC2 legacy)
* if specified, radio config is loaded to container
* Startup scripts executed (`upstart` of sysvinit scripts) (i.e. flash FPGA, specify start of srsLTE, not only way though)
* SRN periodically calls status.sh, check if container is ready for operation
  * must specify a "READY" parameter to indicate node preparation.
  * if not specified, 5-minute timeout, SRN controller calls `start.sh` regardless.
* after `start.sh` starts, RF & Traffic scenarios start. radios communicate via MCHEM
  * Containers should resturn "ACTIVE" during this time via their `status.sh`
* runs experiments according to your config files.
* Radio API calls `stop.sh`
  * cleanup environment
  * Users should specify to save datta to `/logs`: automatically moved to team NAS folder
  * ensure that your `status.sh` returns STOPPING when completed
  * teardown prep
* 2 minutes after STOPPING, SRNs deallocated 

## Batch Mode Example
* 00:00-13:00 - Batch job starts
  * USRP flash, container allocation/initiate, startup scripts (`startup`)
  * NOT wihtin experiment duration
* 13:00 - Internal readiness check
  * all containers return READY? RF & traffic subsystems return READY?
* 13:00-16:00 - scenario prep
* 16:00 - scenario starts (`start.sh`)
16:00-26:00 - experiment runtime
* 26:00 - experiment stops
  * all continers call to `stop.sh`
* 26:00-28:00 - Container teardown
  * copy data to `/logs`
  * colosseum moves `/logs` to `/nas/teamname/...`
  * colosseum teardown
* **more info [https://tinyurl.com/5y4cens8]**

## Validation Pipeline
* Containers to testbeds
* Colosseum as at-scale emulation, then on to real-world environment or large-scale scenario i.e. PAWR




Token System
* Batch jobs are 80% discount of interactive allocation
* tokens are updated/refreshed each week
  * can request ticket for more
  
User must specify all output / log data processes
