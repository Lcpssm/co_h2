# co_h2
Semiconductor sensor measurements of CO and H<sub>2</sub>

All data available [here](https://cloud.mail.ru/public/4DbR/3tVK6mWZ7)

Read and explore data with [prepared jupyter notebook](https://github.com/Lcpssm/co_h2/blob/main/data_cut.ipynb)


# 0. Experiment

A laboratory setup for metal oxide gas sensor operation in the simulated conditions of industrial safety purpose detection of hydrogen and propane:
![experiment_schema](https://raw.githubusercontent.com/Lcpssm/co_h2/main/CO-H2_experiment_scheme.png)

More details about experiment at [Sensors and Actuators B paper](https://www.sciencedirect.com/science/article/abs/pii/S0925400517313096?via%3Dihub)

# 1. Raw data: config & data files

Each observation is represented by of pair of files: 

* **measurement file** `.csv` - e.g.:  `session_5-6_data.csv`

|  | Time | T_set | Phase | Stage | ObservId_local | ObservationId_global | T1 | T2 | T3 | T4 | T5 | T6 | T7 | T8 | T9 | T10 | T11 | T12 | R1 | R2 | R3 | R4 | R5 | R6 | R7 | R8 | R9 | R10 | R11 | R12 |
| :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- |
| 0 | 0.016 | 150.2 | 1 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |1 | 0.235 | 152.2 | 1 | 1 | 1 | 1 | 0 | 0 | 445 | 0 | 0 | 0 | 446 | 453 | 0 | 449 | 0 | 0 | 88818135.6 | 94071580.3 | 4454.7 | 100000000000 | 100000000000 | 100000000000 | 59.6 | 30560.3 | 100000000000 | 1372.6 | 21361358 | 25004112.4 |
| 2 | 0.344 | 153.3 | 1 | 1 | 1 | 1 | 0 | 0 | 451 | 0 | 0 | 0 | 450 | 453 | 0 | 449 | 0 | 0 | 86310345.3 | 81837604.9 | 28483.1 | 100000000000 | 100000000000 | 100000000000 | 219.6 | 22228564.9 | 100000000000 | 31966575.5 | 20991584.5 | 22925579.1 |
| 3 | 0.454 | 154.8 | 1 | 1 | 1 | 1 | 0 | 0 | 96 | 0 | 0 | 0 | 96 | 94 | 0 | 93 | 0 | 0 | 86728556.2 | 81766978.7 | 26971.8 | 100000000000 | 100000000000 | 100000000000 | 213.9 | 19291637.3 | 100000000000 | 24561252.4 | 21028015.1 | 23087606 |

- `Time`: time in seconds from start of the experiment (sampling rate ~0.1sec);
- `T_set`: temperature (Celsius) set by the heater control program;
- `Stage`: the number of session Stage (each record in `*_config.json` file with constant gas concentrations during long time period)
- `Phase`: heater operating modes:
    - 0 - stable temperature; 
    - 1 - heating;
    - -1 - cooling;
- `ObservId_local`: the local number of measurement period within Stage (each cycle ~50sec long; ~550 records; consists of phases `-1 0 1`);
- `ObservationId_global`: the global number of measurement period through all stages;
- `T*`: observed temperature (Celsius) `*` - sensor number (1-12);
- `R*`: observed sensor resistance `*` - sensor number (1-12);<br><br>



* **config file** `.json` - e.g.: `session_5-6_config.json`

`[{'Stage Num': 1,
  'Gas List': ['CO', 'H2', 'NO2'],
  'Gas Concentrations, ppm': [0.0, 0.0, 0.0],
  'Stage Duration, sec': 3600},
 {'Stage Num': 2,
  'Gas List': ['CO', 'H2', 'NO2'],
  'Gas Concentrations, ppm': [0.0, 666.667, 1.426],
  'Stage Duration, sec': 120}, 
  ...]`


- `Stage Num`: number of session stage (each stage has constant values of gas concentrations)
- `Gas List`: set gases labels;
- `Gas Concentrations, ppm`: list of gas concentrations (corresponding to gas list) parts per million;
- `Stage Duration, sec`: time in seconds from start of the experiment;
<br><br>
The detailed explanation of config file example `session_5-6_config.json`:<br><br>
record 1: 
`{`<br>
`'Stage Num': 1,` - 1st session stage;<br>
`'Gas List': ['CO', 'H2', 'NO2'],` - observed gases (*air*, because all gases concentrations are zeros);<br>
`'Gas Concentrations, ppm': [0.0, 0.0, 0.0],` - gases concentrations are zeros (parts per million);<br>
`'Stage Duration, sec': 3600` -  was oberved during 0-3600 sec <br>
`}`<br><br>
       record 2: 
`{`<br>
`'Stage Num': 2,` - 2nd session stage;<br>
`'Gas List': ['CO', 'H2', 'NO2'],` - observed gases (*H<sub>2</sub>* & *NO<sub>2</sub>* only, because CO concentration is zero);<br>
`'Gas Concentrations, ppm': [0.0, 666.667, 1.426],` - CO - 0ppm, H<sub>2</sub> - 666.667ppm, NO<sub>2</sub> - 1.426ppm;<br>
`'Stage Duration, sec': 120` - 2nd stage was oberved during next 120sec (3600-3720 sec from session start) <br>
`}`<br>
<br> 

# 2. Prepared data: cut observations

Goal - create dataset of full 50sec cycles\periods with corresponding values of gas concentrations.

You could find examples of such files in root folder `./cut_experiment2` for sensor **R3**.

There are 2 files for sensor R3:

#### file with gas concentrations: *cut_R3_dataY.csv* :

|  | file_num | Stage Num | ObservId_local | CO | H2 |
| :- | :- | :- | :- | :- | :- |
| 3200 | 6.0 | 5.0 | 11.0 | 21.333333 | 0.0 |
| 3201 | 6.0 | 5.0 | 12.0 | 21.333333 | 0.0 |
| 3202 | 6.0 | 5.0 | 13.0 | 21.333333 | 0.0 |
| 3203 | 6.0 | 5.0 | 14.0 | 21.333333 | 0.0 |

<br>

> Columns description:
- ` ` - empty column with row numbers of this file;
- `file_num` - id of data file (the sequence number of the data file in the sorted list of filenames);
- `Stage Num` - the same as in raw data (see part 1);
- `ObservId_local` - the same as in raw data (see part 1);
- `CO` & `H2` - columns with concentration of gases (ppm).

<br>

#### file with sensor responses: *cut_R3_dataX.csv* :
    
|  | 0 | 1 | 2 | 3 | 4 | ... | 500 | 501 | 502 | 503 |
| :- | :- | :- | :- | :- | :- | :- | :- | :- | :- | :- |
| 3200 | 334558.3 | 338764.9 | 339763.3 | 346024.1 | 317497.5 | ... | 356527.5 | 323535.2 | 295557.8 | 346714.8 |
| 3201 | 334885.1 | 348876.9 | 338944.7 | 339065.6 | 333527.1 | ... | 346309.7 | 306458.4 | 294092.5 | 185230.1 |
| 3202 | 331846.0 | 334781.5 | 339560.2 | 331811.2 | 316959.0 | ... | 312375.3 | 339492.9 | 340384.0 | 392030.5 |
| 3203 | 333107.7 | 336391.6 | 354144.1 | 334263.2 | 273902.3 | ... | 283107.3 | 340617.3 | 376603.9 | 384202.3 |

> Columns description:
- ` ` - empty column with a row numbers of this file;
- `0`, `1`, `2` and etc. - just a column numbers of this file.


# 3. References
>  V. Krivetskiy, A. Efitorov, A. Arkhipenko, S. Vladimirova, M. Rumyantseva, S. Dolenko, A. Gaskov,
Selective detection of individual gases and CO/H2 mixture at low concentrations in air by single semiconductor metal oxide sensors working in dynamic temperature mode. Sensors and Actuators B: Chemical,
V254 pp.502-513, 2018. https://doi.org/10.1016/j.snb.2017.07.100.