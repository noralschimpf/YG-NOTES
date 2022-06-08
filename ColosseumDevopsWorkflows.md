# Colosseum Devops Workflows (as designed by Dilenium for DARPA SC)
*Dr. Bob Baxley, long history w/ CRN/SDR (Zylenium)*

##DARPA SCs:
* first on Winlab (Rice?)
* SC2 grand challenge: Colosseum designed for this
* initially, zero documentation, auoomation, etc......
* Each team designed a radio (Link/MAC/PHY), assigned as 10 nodes for variety of scenarios,
**Need to organize infrastructure in order to access / work with colosseum, effectively design algorithms**


## Zylenium radio design
 * TDMA/FDMA: 50 time slots dynamic, 589.5kHz channels
 * OFDM
 * Codes: Polar FEC (computational efficnet, capacity achieving) (affect.github.io)
 *  rateless codes
 * *stuck with GNURadio, maintain modular / easily-defined paradigm for standardiation*
 
## Repeatable experimentation
**NEED TO DEFINE & AUTOMATE WORKFLOW**
* Ansible to build, share with GW & run batch. 
  * configure for RF and Traffic scenarios, ansible would automate batch emulation
* Python to automate log parsing, generate visuals, dashboards, ......

**track the gitsha's for all repos in uild**

**Dashboards, plots**
* Developing in tableu. no-code, extremely powerful & easy to build large-scale dashboards


# DevOps for Colosseum
* Robust development flow. philosophies, practices, and tools to improve development pace
* "aws for RF": protected, reliable infrastructure that requires high control.
* Process
  1. Build
    a. code commit
    b. automate build & test deploy
  2. Test
    a. **RF-IN-Loop**: lxc containers
    b. compile metrics
    c. Analyze 
    a. modify algos
  3. Release
  
  
# Lessons Learned
1. **Do not invent conventions**: doesn't matter if another approach is inferior, it is easier and more sustainable
  a. always check for the object before trying to make it
 2. if you've done a multi-step process 3+ times, automate it
   a. don't jus write local scripts. will get out-of-hand. *Use Ansible or other infra*
 3. just print. running an experiment is expensive, no shame in adding them
 4. run your tests locally, as much as possible, before RF-in-loop (pytest)
 5. scripting applies to capture log data as well
 
# Tools

## Ansible - baseline for script automating
Declarative, scripting language. infrastructure as code
* conventions already exist
* gitlab.com/zylinium-pub/colosseum-cm 

## Jinja - Templating
* (llink)[palletsprojects.com/[/jinja]
* hard to parse somehwat, easy to configure & integrate with ansible

## Airflow
* manages database building, dashboard building, slack messages, .....
* Visual interface for workflow, jobs status, etc.....

## ElasticSearch - Data sotre (or PostGres)
- not schema-based. push experiment data to, directly build simple dashboards

## Kibana - web-based visualization (Free!)

## Tableu - visualization (~1k license)
* needs rigid, SQL database

Automation: time saves v requires
* simple rf experiment may not require
* ansible is the baseline. it will help with a lot
* other things (airflow, jinja, elastic...) moreso helpful for large teams
