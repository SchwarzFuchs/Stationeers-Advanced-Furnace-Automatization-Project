# Furnace automatization for the Stationeers game.
## Features:
* Full alloy smelting automatization. You only have to provide gas and resourses.
* System automatically checks if your gases satisfy conditions for smelting and tells you if there are any problems.
* Gas mixing accuracy is >99.5% in favorable conditions.
* System has some settings for using in different conditions without any code changes.
## Setting up
### Basic scheme:
![image](https://github.com/SchwarzFuchs/Stationeers-Advanced-Furnace-Automatization-Project/assets/36659425/8603c3c0-318a-4496-b47d-12640b530e1a)
### Name-dependent devices
ICs are adressing some devices by their names. So you should name them the same as in the table, or if you want to name them your own, replace the hashes in the code.
| Device       |  Name        | Name HASH |
| ------------ | ------------ |---------- |
| Button | Button Gas Check | -1443172783 |
| Advanced Furnace | Advanced Furnace | -1131993479 |
| Pipe Analyzer | Hot Pipe Analyzer | 993965803 |
| Pipe Analyzer | Cold Pipe Analyzer | -1504034795 |
| Display (Small) | Hot Gas Satisfaction | -2015441916 |
| Display (Small) | Cold Gas Satisfaction | 817721657 |
| Display (Small) | Pressurize Status | -206058721 |
| Logic Memory | Logic Memory Target Pressure | -823947099 |
| Logic Memory | Logic Memory Target Temperature | -1941351525 |
| Logic Memory | Logic Memory Correction Mean | -679739121 |
| Logic Memory | Logic Memory Target Hash | -74186795 |
| Small Insulated Tank* | Hot Tank Small | -382368696 |
| Small Insulated Tank | Cold Tank Small | 765508678 |
| Big Insulated Tank | Hot Tank Big | 1739686464 |
| Big Insulated Tank | Cold Tank Big | -1045288362 |
| Active Vent | Active Vent Furnace Env. Pressurize | -217226766 |
| Gas Sensor | Gas Sensor Furnace Env. | -881915129 |

\* System supports any amount of insulated tanks of any size
### ICs and their connections
#### Recipe Chip
Allows you to select a recipe and write its parameters to memory.
Connections: 
| dN      |  Device        |
| ------------ | ------------ |
| d0 | Dial |
| d1 | TemperaturePrioritySwitch |
| d2 | HotTank* |

\* Any one of them
#### Main Chip
Monitors button presses and orchestrates all other chips, except for the Recipe IC.

Connections: 
| dN      |  Device        |
| ------------ | ------------ |
| d0 | StartButton |
| d1 | GasCheckIC |
| d2 | PreProccesIC |
| d3 | FurnaceIC |
| d4 | GasCompositionCheckIC |
| d5 | PrePressurizeIC |  
#### Gas Composition Checker IC
Computes the composition of the gases in the tanks to determine the average number of joules per mole for more accurate calculations.

Connections: 
| dN      |  Device        |
| ------------ | ------------ |
| d0 | HotTubeAnalyzer |
| d1 | ColdTubeAnalyzer |
| d2 | TemperaturePrioritySwitch |
| d3 | HotTank* |

\* Any one of them
#### Gas Checker IC
Calculates whether the gases meet the conditions of the selected recipe. Not 100% accurate, so it is recommended to have a reserve of 130-150%.

Connections: 
| dN      |  Device        |
| ------------ | ------------ |
| d0 | ColdGasCheckSwitch |
| d1 | HotTubeAnalyzer |
| d2 | TemperaturePrioritySwitch |
| d3 | ColdTubeAnalyzer |
| d4 | LED |
| d5 | Klaxon |
#### PreProcessing IC
Processes ore and ingots.

Connections:  
| dN      |  Device        |
| ------------ | ------------ |
| d0 | Valve |
| d1 | Pump |
| d2 | Furnace |
| d3 | LED |
| d4 | Klaxon |
| d5 | HotTank* |

\* Any one of them
#### PrePressurizing IC
Mixes gases and starts pummping mix to the furnace.

Connections: 
| dN      |  Device        |
| ------------ | ------------ |
| d0 | Klaxon |
| d1 | TurboPump |
| d2 | LED |
| d3 | HotColdMixer |
| d4 | HotColdMixAnalyzer |
| d5 | Furnace |    
#### Furnace IC
Ends pumping, releases alloy when it's ready, сlears remaining gases from the system.

Connections: 
| dN      |  Device        |
| ------------ | ------------ |
| d0 | Klaxon |
| d1 | TurboPump |
| d2 | LED |
| d3 | HotColdMixAnalyzer |
| d4 | Furnace |    

## Important
### Setting switches
* Cold Gas Check Switch - When set to 1, makes system think that you always have enough cold gas even if tanks are currently empty. Use it on planets where you're not limited in cold gas, like Mars and Europe.
* Temperature Priority Switch  - When set to 1, the system will use as little cold gas as possible. Use it when you are short on cold gas.
### Advices
* Very high pressure on mixer inputs may lead to worse mixing accuracy.
* The system continuously adjusts the mixer settings, but changes in gas composition and temperature during the smelting process may lead to worse mixing accuracy.
* Don't copy scripts from sample world. They may not contain all the latest improvements over the code in the repository.
* You don't need to turn on all ICs, only the Main and Recipe ones.
* You can connect PreProcessing pump to the cold pipe if your "cold" gas hotter than 430K. Don't forget to change the tank on the IC dials in that case.
### Error messages from klaxon
* Fire - you have volatiles in your pipes/tanks. Remove or burn it.
* Temperature low - your hot gas isn't hot enough.
* One Pressure low - you don't have enough hot gas.
* Temperature high - your cold gas isn't cold enough.
* Two Pressure low - you don't have enough cold gas.
* System failure - for some reason the set temperature and pressure have not been reached or you put the wrong ingridients into the furnace.
