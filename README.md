# Furnace automatization for the Stationeers game.
## Features:
* Full alloy smelting automatization. You only have to provide gas and resourses.
* System automatically checks if your gases satisfy conditions for smelting and tells you if there are any problems.
* Gas mixing accuracy is >99.5%.
* System has some setting switches for using in different conditions without any code changes.
## Setting up
### Basic scheme:
<img width="5210" height="3510" alt="StationeersAdvFurnaceScheme drawio (1)" src="https://github.com/user-attachments/assets/a153947c-8577-4277-86d9-9304a704326e" />

### Name-dependent devices
ICs are adressing some devices by their names. So you should name them the same as in the table, or if you want to name them your own, replace the hashes in the code.
| Device       |  Name        | Name HASH |
| ------------ | ------------ |---------- |
| Advanced Furnace | Advanced Furnace | -1131993479 |
| Pipe Analyzer | Hot Pipe Analyzer | 993965803 |
| Pipe Analyzer | Cold Pipe Analyzer | -1504034795 |
| Pipe Analyzer | Waste Pipe Analyzer | -114736392 |
| Pipe Analyzer | Hot/Cold Mix Pipe Analyzer | -2109432728 |
| Pipe Analyzer | Waste Buffer Pipe Analyzer | -887401742 |
| Turbo Volume Pump | Hot/Cold Mix Pump | 454545836 |
| Turbo Volume Pump | Hot Gas Pump | -1471488033 |
| Turbo Volume Pump | Cold Gas Pump | 239695817 |
| Turbo Volume Pump | Waste Pump | 1228632777 |
| Volume Pump | Preserve Waste Pump | -982582202 |
| Volume Pump | Discard Waste Pump | -1383860730 |
| Display (Small) | Hot Gas Satisfaction | -2015441916 |
| Display (Small) | Cold Gas Satisfaction | 817721657 |
| Display (Small) | Pressurize Status | -206058721 |
| Logic Memory | Logic Memory Target Pressure | -823947099 |
| Logic Memory | Logic Memory Target Temperature | -1941351525 |
| Logic Memory | Logic Memory Correction Mean | -679739121 |
| Logic Memory | Logic Memory Target Hash | -74186795 |
| Logic Memory | Logic Memory Waste Preserve Threshold | -1125432018 |
| Button | Gas Check Button | -803738805 |
| Switch | Cold Gas Check Switch | -1113167255 |
| Switch | Temperature Priority Switch | 889038674 |
| Switch | No Waste Usage Switch | 418789413 |
| Small Insulated Tank* | Hot Tank Small | -382368696 |
| Small Insulated Tank | Cold Tank Small | 765508678 |
| Small Insulated Tank | Waste Tank Small | 986570611 |
| Big Insulated Tank | Hot Tank Big | 1739686464 |
| Big Insulated Tank | Cold Tank Big | -1045288362 |
| Big Insulated Tank | Waste Tank Big | 2088526909 |
| Active Vent | Active Vent Furnace Env. Pressurize | -217226766 |
| Gas Sensor | Gas Sensor Furnace Env. | -881915129 |
| IC Housing | Furnace Orchestrator IC | 125439440 |
| IC Housing | Recipe IC | -1274372589 |
| IC Housing | Gas Calculator IC | -2037625998 |
| IC Housing | Gas Check IC | -1306530900 |
| IC Housing | PreProcessing IC | 180461171 |
| IC Housing | PrePressurizing | 2730726 |
| IC Housing | Furnace Finalizer IC | 1332447006 |

\* System supports any amount of insulated tanks of any size
### List of building supplies
| Item      |  Quantity        |
| ------------ | ------------ |
| Advanced Furnace | 1 |
| Kit (Logic switch) | 6 |
| Kit (Consoles) | 4 |
| Logic Memory | 5 |
| IC Housing | 7 |
| IC 10 Circuit | 7 |
| Kit (Powered Chutes) | 1 |
| Basic Chutes | >10* |
| Circuitboard (Hash Display) | 1 |
| Kit (Lights) | 1 |
| Klaxon Speaker | 1 |
| Pipe Analyzer | 5 |
| Turbo Volume Pump (Gas) | 4-5** |
| Volume Pump (Gas) | 2-3** |
| Active Vent | 1 |
| Gas Sensor | 1 |
| Kit (Tank Insulated) | >=3 |
| Pipe Utility (insulated) | >=6 |
| Pipes (insulated) | >30 |
| Cables | Yes |

\* Depends on how much capacity of buffer chutes you want to have

\** You can replace PreProcessing pump with turbo version for faster preprocessing in some cases
### ICs and their connections
#### Recipe IC
Allows you to select a recipe and write its parameters to memory. There're 3 versions of code for it, one is universal and the two other ones are optimised for Venus and Vulcan accordingly.

Connections: 
| dN      |  Device        |
| ------------ | ------------ |
| d0 | Dial |
| d1 | Temperature Priority Switch |
| d2 | HotTank* |
| d3 | Start Button |

\* Any one of them
#### Furnace Orchestrator IC
Monitors button presses and orchestrates all other chips, except for the Recipe IC.

Connections: 
| dN      |  Device        |
| ------------ | ------------ |
| d0 | Start Button |
| d1 | Gas Check IC |
| d2 | PreProcessing IC |
| d3 | Furnace Finalizer IC |
| d4 | Gas Calculator IC |
| d5 | PrePressurizing IC |  
#### Gas Calculator IC
Computes the composition of the gases in the tanks to determine the average number of joules per mole for the more accurate calculations. Also determines the possibility of waste gas usage.

Connections: 
| dN      |  Device        |
| ------------ | ------------ |
| d0 | Hot Pipe Analyzer |
| d1 | Cold Pipe Analyzer |
| d2 | Waste Pipe Analyzer |

#### Gas Check IC
Calculates whether the gases meet the conditions of the selected recipe. 

Connections: 
| dN      |  Device        |
| ------------ | ------------ |
| d0 | Cold Gas Check Switch |
| d1 | Hot Pipe Analyzer |
| d2 | Waste Pipe Analyzer |
| d3 | Cold Pipe Analyzer |
| d4 | LED |
| d5 | Klaxon |
#### PreProcessing IC
Processes ore and ingots to load them into the furnace chamber. Imporant: system won't accept additional resourses after the end of PreProcessing stage but during it you can throw as much as you want up to Advanced Furnace Limit.

Connections:  
| dN      |  Device        |
| ------------ | ------------ |
| d0 | Valve |
| d1 | PreProcessing Pump |
| d2 | Furnace |
| d3 | LED |
| d4 | Klaxon |
| d5 | HotTank* |

\* Any one of them
#### PrePressurizing IC
Mixes gases and starts pumping mix into the furnace.

Connections: 
| dN      |  Device        |
| ------------ | ------------ |
| d0 | Klaxon |
| d1 | Hot/Cold Mix Pump |
| d2 | LED |
| d3 | Hot/Cold Mix Pipe Analyzer |
| d4 | Furnace |

#### Furnace Finalizer IC
Ends pumping, releases alloy when it's ready, сlears remaining gases from the system.

Connections: 
| dN      |  Device        |
| ------------ | ------------ |
| d0 | Klaxon |
| d1 | Hot/Cold Mix Pump |
| d2 | LED |
| d3 | Hot/Cold Mix Pipe Analyzer |
| d4 | Advanced Furnace |    

## Important
### Setting switches
* Cold Gas Check Switch — When set to 1, makes system think that you always have enough cold gas even if tanks are currently empty. Use it on planets where you're not limited in cold gas, like Mars and Europe.
* Temperature Priority Switch — When set to 1, the system will use as little cold gas as possible. Use it when you are short on cold gas.
* No Waste Usage Switch — name speaks for itself.
### Advices
* Volume of pipes between tanks and pumps should be several hundred liters, at least 300. Small volume leads to bad mixing accuracy.
* Mix buffer pipe volume shouldn't be big, otherwise system will work slow.
* If you feel that system overcorrects itself too much, try lowering the number on line 69 of PrePressurizingIC, default value is 48.
* The system continuously adjusts pump settings, but changes in gas composition and temperature during the smelting process may lead to worse mixing accuracy.
* Don't copy code from sample world. They may not contain all the latest improvements over the code in the repository.
* You don't need to turn on all ICs, only the Main and Recipe ones.
* You can connect PreProcessing pump to the cold pipe if your "cold" gas is hotter than 450K. Or to the any other pipe that is hot enough, the furnace waste one for example. Don't forget to change the tank on the IC dials in this case.
### Error messages from klaxon
* Fire — you have volatiles in your pipes/tanks. Remove or burn it.
* Temperature low — your hot gas isn't hot enough. It could be about gas you use for smelting (should be hotter than target temperature) or about gas you use for preprocessing (should be hotter than 450K).
* One Pressure low — you don't have enough hot gas.
* Temperature high — your cold gas isn't cold enough.
* Two Pressure low — you don't have enough cold gas.
* Three Pressure high — you have water in your pipes/tanks. Remove it.
* System failure — for some reason designated temperature and pressure haven't been reached or you put the wrong ingridients into the furnace.
