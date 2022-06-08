# Introduction
Workflow
 1. reserve resources
 2. SSH to nodes
 3. run experiments on nodes
 4. save logs/oiutputs

## LXC Containers
Image-based software / containerziation for OS instances. access to hardware
* LXC Images are immutable
* Enforces reproducibility
* limited storage space (most space for containers)


## File-proxy
* locations /share/nas/[common | your-groupname]/images
* connected to your reservation: store configs, save data location

## Data Collection on colosseum
* Reservations & file-proxy connected between each-other.
  * /share/ common to all nodes
  * /share/[teamname]/reservation/[reservation-id] to access image for reservation


# Assignment
1. download base image
2. Customie locally (addGNURadio)
3. Upload image
4. Run experiment

## GNURadio
SDK, compose DSP locks. Interface for USRP X310
`./flash_fpga_x310.sh`
`gnuradio-companion`
Design the RX/TX

## Local container creation & upload

## Scneario & Experiment run
