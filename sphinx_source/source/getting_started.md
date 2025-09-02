## Installation

**Step 1**: Git **clone Res-IRF folder** in your computer.
   - Use your terminal and go to a location where you want to store the Res-IRF project.
   - `https://github.com/CIRED/Res-IRF4.git`

**Step 2**: **Create a conda environment** from the environment.yml file:
   - The requirements.txt file is in the Res-IRF folder.
   - Use the **terminal** and go to the Res-IRF folder stored on your computer.
   - Type: `conda env create -f environment.yml`

**Step 3**: **Activate the new environment**.
   - The first line of the yml file sets the new environment's name.
   - Type: `conda activate envResIRF`

**Step 4**: **Launch Res-IRF**
   - Launch from Res-IRF root folder (not from `/project`):
   - `python -m project.main -c project/input/config.json`
   - `project/input/config.json` is the path to the configuration file

## Getting started

Project includes libraries, scripts and notebooks.  
`/project` is the folder containing scripts, notebooks, inputs and outputs.  

The standard way to run Res-IRF:  

**Launch Res-IRF main script.**  
The model creates results in a folder in project/output.  
Folder name is by default `ddmmyyyy_hhmm` (launching date and hour).
By default, only a  selection of the most important results are available and graphs.

A configuration file must be declared.
An example of configuration file is in the `config` folder under the name of `config.json`.
The Res-IRF script use Multiprocessing tool to launch multiple scenarios in the same time. 

In the `output/ddmmyyyy_hhmm` folder:
- One folder for each scenario declared in the configuration file with detailed outputs:
    - `output.csv` detailed output readable directly with an Excel-like tool
- `.png` graphs comparing scenarios launch in the same config file.

## API

It is also possible to get data and Python object directly (useful to create its own scripts).  
`config = get_config()` allows to get the Reference configuration file.  
`inputs = get_inputs(building_stock=path)` allow to get data.
`inputs, stock, year, policies_heater, policies_insulation, taxes = config2inputs(config)`: create Python objects from raw data.  
Finally:  
`buildings, energy_prices, taxes, post_inputs, cost_heater, ms_heater, cost_insulation, ms_intensive, renovation_rate_ini, policies_heater, policies_insulation, flow_built = initialize(inputs, stock, year, policies_heater, policies_insulation, taxes, config, path)`
parse and create Python objects used by Res-IRF.  
Moreover, the user can use all the methods of AgentBuildings object `buildings` defined in buildings.py.  


