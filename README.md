# Python Beta Water Unavailability Methodology for the Delta Watershed 

The Water Unavailability Methodology was used to analyze water unavailability in the Sacramento-San Joaquin Delta Watershed and determine when water right curtailments were appropriate under drought emergency curtailment and reporting regulations effective between August 2021 and August 2023. All curtailment orders issued pursuant to the emergency curtailment regulations were rescinded on April 3, 2023, the regulations expired by operation of law on August 14, 2023, and the Methodology is not presently being used to evaluate water unavailability in the Delta watershed. More information on the drought emergency regulations can be found on the Delta Drought webpage. The Water Unavailability Methodology website has detailed documentation on the Methodology’s inputs, assumptions, calculations, and products. 

While the State Water Board previously published the Methodology in the format of an Excel workbook (available at the Methodology website linked above), this repository contains a beta version implemented in Python programming language via Jupyter Notebook. This approach aims to provide the same results as the spreadsheet-based analysis while harnessing the efficiency and adaptability of Python. It includes flexible standardized inputs, automated ingestion of external datasets, systematic function-based water unavailability calculations, and a streamlined analytical workflow for quicker computations and scenario analysis. **This code is a beta version of the Methodology that is provided for informational purposes only. It is not currently being used to issue water right curtailments or for any other regulatory purposes.** 

The beta Methodology is structured as four scripts in the code folder, each with in-line comments describing function purposes and operations. The scripts must be run sequentially in their numbered order: 

* **1.Supply.ipynb** processes external water supply datasets and user inputs to develop water supply values that feed into the analysis. 

* **2.Demand.ipynb** processes the input datasets of water rights, points of diversion (PODs), and demands in the watershed to develop water demand values that feed into the analysis. 

* **3.Analysis.ipynb** performs water unavailability analyses for the Delta Watershed at both the headwater subwatershed and watershed scales. 

* **4.Curtailments.ipynb** processes the water rights input dataset and the results of the Analysis script to generate a curtailment status for each water right in the Delta watershed, along with some data summary tables. 

A sequential run of the 4 scripts represents a water unavailability analysis for a single timestep. The scripts prompt some user inputs which determine the parameters of the analysis and are saved for reference in the code\output-data\Scenario_Inputs.csv table: 

Start Date and End Date, prompted in the 1.Supply script, delineate the period (inclusive of both days) over which water unavailability will be analyzed. Any period between October 1, 2012, and one year from the current date can be entered, though curtailment analyses generally used periods of one month. 

Forecast Exceedance, prompted in the 1.Supply script, is the exceedance probability of water supply forecast that will be used for the entire watershed. A 50% value would be used to represent median conditions; if the specified period includes only dates in the past, the value entered will have no effect. 

Demand Year, prompted in the 2.Demand script, is the calendar year of quality-controlled reported diversions that will be used to represent demands in the watershed. Currently calendar year 2018 or 2019 can be input, and curtailment analyses generally used 2018 demands. 

Use of Enhanced Reporting, prompted in the 2.Demand script, determines if Enhanced Reporting of Projected Demand data will be used to represent demands for the largest water rights and claims in the watershed. Curtailment analyses between November 2022 and March 2023 generally used Enhanced Reporting values, but they may not be appropriate for analyses outside this period. 

Standardized input datasets for the beta Methodology scripts are provided in the user-inputs folder. They can be modified to affect the analysis but must keep the same headers and structure to be processed by the scripts: 

AnalysisPreparationRiparian.csv is a table used by the 3.Analysis script to aggregate senior demands for claims of riparian rights. 

ConsumptiveUseFactors.csv is a table of multipliers for each subwatershed and month which are applied to reduce demands for direct diversion to account for abandoned return flows which increase available supply. 

Demands.csv is a list of all water rights and claims in the Delta Watershed and their quality-controlled monthly demands for direct diversion and diversion to storage. Currently this dataset contains demands for calendar years 2018 and 2019. Values are in acre-feet (AF). The water rights and claims in this dataset correspond with those in the PODs and WaterRights datasets. 

GapFillingFactors.csv is a table of multipliers for specific subwatersheds each month which are used to estimate supply for small tributaries for which supply data are not available based on correlations with other subwatersheds. 

InstreamFlowsAbandoned.csv is a table of instream flow requirements for each subwatershed and month which apply for the lower reach of each subwatershed but are assumed to be abandoned below their confluence with the valley floor. Values are in cubic feet per second (cfs) and currently correspond with flows required in dry or critical years. 

PODs.csv is a list of all points of diversion in the Delta Watershed and their locations (in NAD83 coordinates). Key attributes for each POD include subwatershed and watershed, location (or not) within the Legal Delta, and weight values between zero and one which are used to apportion a water right’s demands for direct diversions and storage to individual PODs. The water rights and claims in this dataset correspond with those in the Demands and WaterRights datasets. 

WaterRights.csv is a list of all water rights and claims in the watershed, along with the Water Right Type, Primary Owner, and Priority Date of each. The water rights and claims in this dataset correspond with those in the Demands and PODs datasets. 

The user-inputs\Enhanced Reporting folder contains files for each month during which Enhanced Reporting of Projected Demand data was received and quality controlled for use in the Methodology. Each water right subject to Enhanced Reporting was required to submit a report, either indicating that the 2018 reported diversions accurately represented their projected diversions for the month (No Changes), or reporting their projected demand for direct diversions and diversions to storage for the month. 

Each script generates intermediate output datasets that are used sequentially by later scripts. These datasets will be generated each time the script is run and saved in the code\intermediate-outputs folder: 

Instream_Flow_Period.csv, output from the 1.Supply script, provides the volume of abandoned instream flow from each subwatershed for the user-specified analysis period. 

Supply_Period_Total.csv, output from the 1.Supply script, provides the supply from each subwatershed for the user-specified analysis period. 

Demands_Period_POD.csv, output from the 2.Demand script, provides total demands from individual PODs for the user-specified analysis period. 

Analysis_Dataset_Pre.csv, output from the 2.Demand script, is an aggregated list of demands by subwatershed that is used at the start of the 3.Analysis script. 

Headwater_Calculations.csv, output from the 3.Analysis script, contains the final water supplies for each subwatershed that feed into the watershed-scale analysis, incorporating temporarily disconnected subwatersheds and abandoned instream flows. 

Analysis_Results.csv, output from the 3.Analysis script, contains the results of the water unavailability analysis which are processed to determine curtailments in the 4.Curtailments script. 

The 4.Curtailments script generates the final Curtailment_List.csv dataset into the code\output-data folder, which represents the curtailment status of each water right and claim in the watershed based on the unavailability analyses. Absent any effective curtailment regulation, these statuses serve informational purposes only and have no regulatory effect. The script also outputs the Curtailment_Summary.csv dataset, which summarizes curtailments by subwatershed. 

Validation and debugging have been performed to help ensure that the results of these scripts match the outputs of the Excel version of the Methodology, but some bugs may remain. Questions regarding the scripts or bug reports should be posted as Issues in this repository. Other general questions about the Methodology or drought emergency regulations in the Delta watershed should be sent to Bay-Delta@waterboards.ca.gov. 
