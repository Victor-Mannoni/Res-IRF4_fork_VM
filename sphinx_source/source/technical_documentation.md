
# Technical documentation

## Context

### Overview of the model

> The Res-IRF model[^model] is a tool for simulating energy consumption and energy efficiency improvements in the 
> French residential building sector. It currently focuses on space heating as the main usage. 
> The rationale for its development is to integrate a detailed description of the energy performance of the dwelling 
> stock with a rich description of household behaviour. Res-IRF has been developed to improve the behavioural realism 
> that is typically lacking in integrated models of energy demand.

[^model]: The acronym Res-IRF stands for the Residential module of IMACLIM-R France. Also developed at CIRED, IMACLIM-R
France is a general equilibrium model of the French economy {cite:ps}`sassiIMACLIMRModellingFramework2010`. The two
models are linked in {cite:ps}`giraudetExploringPotentialEnergy2012` and 
{cite:ps}`mathyRethinkingRoleScenarios2015`. The linkage in these papers is ‘soft’ in that it only concerns energy
markets:
Res-IRF sends energy demand to IMACLIM-R France which in turn sends back updated energy prices.

Fed by population growth, household income growth and energy prices as exogenous inputs, the model returns construction
and renovation costs and flows, energy consumption and heating comfort as endogenous outputs ({numref}`model_figure`). 
This process is iterated on an annual time-step basis. The energy efficiency of a dwelling is characterized by its Energy
Performance Certificate (EPC, diagnostic de performance énergétique ) and energy efficiency improvements correspond to
upgrade to one a more efficient label. Energy consumption results from household decisions along three margins: the
extensive margin of investment – the decision of whether or not to renovate –, the intensive margin of investment – the
magnitude of the energy efficiency upgrade – and the intensity with which the heating infrastructure is used after
renovation. Investment decisions are based on a net present value (NPV) calculation that incorporates a number of
barriers at the source of the so-called ‘energy-efficiency gap’ – the discrepancy between observed energy-efficiency
levels and those predicted by engineering studies{cite:ps}`jaffeEnergyefficiencyGapWhat1994`,
{cite:ps}`gillinghamEnergyEfficiencyEconomics2009`, 
{cite:ps}`allcottThereEnergyEfficiency2012a`. These include: myopic expectation of energy prices, hidden costs of
renovation (e.g., inconvenience associated with insulation works), barriers to collective decision-making within
homeowner associations, split incentives between landlords and tenants, credit constraints, and the so-called rebound
effect.

```{figure} img/elementary_model_structure.png
:name: model_figure

Elementary structure model
```

### Previous developments

The development of Res-IRF has produced six peer-reviewed articles to this day, of which an overview is provided in
{numref}`previous_development`.

**Overview of achievements with Res-IRF**
```{eval-rst}
.. csv-table:: Previous Developments
   :name: previous_development
   :file: table/previous_developments.csv
   :header-rows: 1
```
*Note : The symbol * points to the reference that contains the most comprehensive description of the associated version
of the model. The most comprehensive description of version 3.0 is to be found in the present document.*

The documentation reflects the latest version of the model. The model is intended to be input-agnostic and the
documentation is structured to reflect this paradigm. For the sake of clarity, however, some processes are
illustrated with numerical examples from version 3.0, which was used in several recent publications.

## Energy use

The model uses two metrics for energy use: the conventional consumption predicted by the EPC label of the dwelling; and
the actual consumption that determines energy expenditure. The two are linked by the intensity of heating of the heating
infrastructure, which is an endogenous function of the model.

### Conventional energy use

Integrating new data on specific consumption (kWh/m²/year) and housing surface area (m²) allowed us to improve the
accuracy of total consumption parameters (kWh/year).

#### Specific consumption

The conventional specfic consumption of an existing dwelling is directly given by its EPC label[^label]. By including a
precise measurement of the conventional energy consumption of each dwelling, the Phébus-DPE database made it possible to
estimate for each band an average consumption.

[^label]: Final energy consumption is deducted from primary energy consumption by a coefficient of 1 for natural gas,
heating oil and wood energy and by the conventional coefficient of 1/2.58 that applies to electricity in France.

Since the EPC covers energy consumption for heating, hot water and air conditioning, adjustments are needed to isolate
the part specifically dedicated to heating. Here again, the Phébus-DPE database, by distinguishing energy between
heating use, hot water use and photovoltaic production, makes it possible to estimate an average share dedicated to
heating for each EPC band.

New dwellings fall into two categories of energy performance: 

- Low Energy (LE) level, aligned with the prevailing building code at 50 kWh/m²/year of primary energy,
- Net Zero Energy (NZ) level, mandating zero consumption, net of production from renewable sources. Since we focus
  on gross energy consumption, we assign a consumption of 40 kWhEP/m²/year to NZ dwellings.

The same coefficient of 0.4 is applied to BBC and BEPOS consumption in order to isolate
heating from the five usages covered by building code prescriptions (instead of three usages in the case of the EPC in
existing dwellings). Note that the energy requirements of EPC band A are also net of production from renewable sources.
Our focus on gross consumption leads us to apply a coefficient that is greater than 1. These calculations are detailed
in {numref}`conventional_energy`.

##### Surface area
The same approach was used to set surface area parameters bases on average values estimated on Phébus data, by
category of dwelling.

#### Actual energy use
A growing number of academic studies point to a gap between the conventional energy consumption predicted by energy
performance certificates and actual energy consumption. The most common explanation is a more intense heating of the
heating infrastructure after an energy efficiency improvement – a phenomenon commonly referred to as the “rebound
effect.” [^rebound_effect]

[^rebound_effect]: See for example {cite:ps}`aydinEnergyEfficiencyHousehold2017`. Another explanation sometimes put forward is the pre-bound effect,
according to which consumption before renovation, from which energy savings are predicted, is overestimated (
Sunikka-Blank et al., 2012).

In version 4.0, we add a new functional form to calculate the heating intensity. It is an isoelastic function that links the heating intensity to the energy bill of the household. 
Heating intensity in Res-IRF follows the equation: 

$$
\text{Heating Intensity} = (C_{\text{conventional}} \cdot p)^{-1/\rho}
$$

where:  

- $C_{\text{conventionnal}}$ = Conventionel energy consumption for space heating  
- $p$ = Energy price 
- $\rho$ = 5  

$\rho$ has been fix to 5 so as to generate a short-term price elasticity of -0.2 as estimated in France by `Douenne_Fabre_2022`.

Heating intensity is also defined as:

$$\text{Heating Intensity} = \frac{\text{Actual energy use}}{\text{Conventional energy use}}$$


#### Total energy use

The total final actual energy consumption generated by Res-IRF from data for the initial year differs from the values
produced by CEREN. These differences are due to differing scopes between the initial building stock and CEREN databases
and to the adjustments needed to select the main heating fuel.

To ensure consistency with the CEREN data, which is the reference commonly used in modelling exercises, Res-IRF is
calibrated to reproduce the final energy consumption given by CEREN for each fuel in the initial year. The resulting
conversion coefficients applied to the Phebus Building Stock are listed in {numref}`calibration_energy`. They indicate
that Res-IRF reproduces natural gas and heating oil consumption fairly accurately, with an error of around 5%. On the
other hand, electricity consumption is clearly overestimated and fuel-wood consumption is greatly underestimated. The
succinct documentation of the CEREN database did not allow us to clearly identify the reasons for these biases. However,
it can reasonably be assumed that they are attributable to the procedure for selecting a main heating fuel in Res-IRF,
which probably implies some substitution of electricity for wood in dwellings that are mainly heated with electricity
but use wood as auxiliary heating. The difficulties inherent in converting different forms of wood (logs, pellets, etc.)
into TWh can also explain the differences observed in wood consumption.

```{eval-rst}
.. csv-table:: Calibration of total final actual energy consumption
   :name: calibration_energy
   :file: table/input_2012/ceren_energy_consumption_2012.csv
   :header-rows: 1
   :stub-columns: 1
```

## Energy efficiency improvements

### Stock dynamics

#### Initial stock 
The initial stock is used to calibrate the model and is a base for the simulation period. So it is an important input 

- **Number of dwellings in the initial stock for 2023**: This initial stock amounts to 30,225,456, which is the sum of the number of dwellings across the segments of the `buildingstock_sdes2023` file, adjusted. 

#### Evolution of the Number of Dwellings
The trajectory of the number of primary residences is provided exogenously in Res-IRF through several parameters:
The evolution of the housign stock is driven by 2 main inputs that determine the number of new construction and the demolition rate of the existing stock. 
the existing stock is the iniital stock all the new dwellings constructed throughout the simulation are considering as new stock. 
 
- **Number of annual constructions**: The reference input file is `flow_construction_sdes_central.csv`. These figures are based on SDES projections and have been adjusted.  
- **Building demolition rate**: The reference input file is `demolition_rate_sdes_central.csv`, also derived from SDES projections. Demolitions are assumed to primarily affect buildings with the lowest energy performance ratings.

#### Evolution of the housing stock composition 

The housign stock composition is driven by 3 mechanisms: 
- the new constructions that have specific characteristics
- the demolition that affect in priority least efficient buildings
- the renovation of existing buildings

##### Characteristics of new constructions 

The new constructions have specific characteristics that come from exogenous data listed there: 

- Surface of new dwellings: determined by Fidéli 2018 
The surface of new dwellings is lower that the one of existing dwellings, this represent the densification of the housing stock, and also maybe some sufficiency 
Fidéli c'est une enquête de l'INSEE**aller voir article de fidéli**

- Insulation performance, no sources find for this input: trouver d'où viennent ces valeurs, en tout cas elles traduisent une bonne isolation **peut etre rajouter un tableau avec les valeurs** 
- Market share for heater for new construction to determine the heating system of new constructions **trouevr la source, un ms qui favorise les PAC**
- Share of single family dwellings in new constructions from S2 ADEME scenario, 

All these inouts combined give us the characteristics of new dwellings that are supposed to be well performed

##### Characteristics of demolished buildings 

##### Building renovations 

The big part and endogenous one of the energy efficiency improvement 
Include investments barriers 

 






The number of dwellings is determined each year by exogenous projections of
new constructions and demolition from the SDES **ajouter source**. The total housing
stock is divided into two components:

- The stock of “existing dwellings” corresponds to the total stock of the initial year. It is eroded at a rate of
  0.1%/year due to destruction, based on `SDES central scenario projection`. Destructions are
  assumed to affect in priority the lowest energy performance labels, based on based on {cite:ps}`traisnelHabitatDeveloppementDurable2001`. The surface of existing dwellings is dtermined according to INSEE data **trouver la source exacte** 
- New constructions are determined by SDES projections, it is an exogenous data. The cumulative sum of new constructions since the initial year constitutes the stock of “new
  dwellings.” Their surface are set according to Fidéli 2018. + the part of multi family building in building stock is set according to s2 scenarios 



The share of owner-occupied and rented dwellings is held constant.

### Renovation dynamics 

Renovation and investment decisions are deetermined by Res-IRF. The model gives a detailed description of behaviour and 
modelizes barriers to investment 

### Investment decisions – general case

The energy performance of the housing stock in Res-IRF is affected by both the construction of new dwellings and the
renovation of existing ones. Both effects are modelled by discrete choice functions. Generally speaking, the owner of a
dwelling of initial performance i∈{1…n} chooses to upgrade it to an option of final performance f∈{i+1,…,n} by comparing
its life-cycle cost to that of other options. The life-cycle cost $LCC_{i,f}$ of an option is the sum of three terms:

$$LCC_{i,f}= INV_{i,f}+ γ * ENER_f + IC_{i,f}$$

where INV is the investment cost; ENER is the life-cycle discounted cost of conventional energy use, calculated using
the energy price for the year under consideration; IC are some “intangible costs,” representing non-energy attributes of
the investment, such as aesthetic or acoustic benefits, inconvenience generated by insulation works, etc..

The assumption of myopic expectation, which materializes by applying the discount factor to the contemporaneous energy
price, is justified by a number of econometric studies {cite:ps}`andersonWhatConsumersBelieve2011` The discount factor γ
depends on the discount rate r and the investment horizon l according to the following relationship:

$$γ(r,l)=∑_{t=0} (1+r)^{-t} = \frac{1-(1+r)^{-l)}}{r}$$

The two parameters are set in Res-IRF to capture various barriers to home renovation:
- The discount rate captures both the tighter credit constraints facing lower-income households and the barriers to
  decision-making within homeowner associations.
- The investment horizon reflects the intensity with which real estate and rental markets capitalize the “green value”
  of the housing, i.e., magnitude of the rental or resale premium for a property that has just undergone energy
  efficiency improvements.

The market share $MS_{i,f}$ of upgrades from labels i to f, resulting from the aggregation of individual choices, is
determined by their life-cycle cost from the following equation:

$$MS_{i,f} = \frac{LCC_{i,f}^{-v}}{∑_{k=i+1} LCC_{i,k} ^{-v}}$$

Parameter v characterizing the heterogeneity of preferences is set to 8 in the model.[^2model]  Intangible costs are
calibrated so that the observed market shares are reproduced in the initial year.

[^2model]: In the absence of data allowing a more precise estimate, this value is set in an ad hoc manner. Sensitivity
analysis of the model has shown that this parameter only had a small influence on the simulated energy consumption {cite:ps}`brangerGlobalSensitivityAnalysis2015`.

The paragraphs thereafter describe in detail the specification of energy efficiency improvements, which are based on two
types of technical data – the market shares of the different options in the initial year and their investment cost – and
two types of behavioural data – the discount rate and the investment horizon.

### New constructions

Construction costs at the LE and NZ levels have been updated in Res-IRF 3.0 based on estimates recently made available
by CGDD (2015) [^constructions]

```{eval-rst}
.. csv-table:: Construction costs (€/m2)
   :name: cost_construction
   :file: table/input_2012/cost_construction_2012.csv
   :header-rows: 2
   :stub-columns: 1
```

[^constructions]: The study does not include information on housing heated with fuel oil or on multi-family homes heated with
wood. The former are therefore assigned the costs of new buildings heated with natural gas, and the latter the costs of
single-family homes heated with wood, to which we add the average additional cost of multi-family homes.

The market shares used to calibrate intangible costs were also updated in 3.0, based on trends provided by CEREN.

The massive penetration of natural gas at the expense of electric heating observed over the past ten years in
multi-family housing is mainly due to the anticipation and subsequent application of the 2012 building code. In order to
abstract from short-term variations, the 2012 market shares are set in Res-IRF on the average of the years 2012-2015 {numref}`heating_fuel_construction`.

```{eval-rst}
.. csv-table:: Distribution of heating fuels in new constructions in Res-IRF for year 2012
   :name: heating_fuel_construction
   :file: table/input_2012/heating_fuel_construction_2012.csv
   :header-rows: 1
   :stub-columns: 1
```

Considering that the quality of new constructions results from decisions made by building and real estate professionals
rather than by future owners, we subject these decisions in the model to private investment criteria, reflected by a
discount rate of 7% and a time horizon of 25 years.

### Renovation of existing dwellings

The model simultaneously determines the number of renovations and their performance. The process is therefore more
complex than in new construction, where the two margins are distinct. For the sake of clarity, we hereafter describe
them sequentially.

#### Intensive margin

Renovation costs $INV_{i,f}$ are described by an upper diagonal matrix linking the initial EPC label i of the dwelling
to its final label f {numref}`renovation_cost`. Parameterization of the matrix is based on piecemeal data supplemented
with values interpolated according to the following principles:

* Decreasing returns, i.e., increasing incremental cost of renovation:
$INV_{i,f+2}-INV_{i,f+1} > INV_{i,f+1} - INV_{i,f}$

* Economies of scale, i.e., deep retrofits costing less than a succession of incremental renovations:
$INV_{i,f} < INV_{i,i+k} + INV_{i+k,f}$ for all k such that $1≤k<f-i$

```{eval-rst}
.. csv-table:: Renovation costs used in Res-IRF 3.0 (€/m2). Source: Expert opinion
   :name: renovation_cost
   :file: table/input_2012/cost_renovation_2012.csv
   :header-rows: 1
   :stub-columns: 1
```

The matrix equally applies to single- and multi-family dwellings, in both private and social housing. 

The same is true for the matrix of market shares used to calibrate intangible costs of renovation, which was based on
{cite:ps}`pucaHabitatExistantDans2015` (Table 11).

```{eval-rst}
.. csv-table:: Market shares of energy efficiency upgrades in 2012. Source: PUCA (2015)
   :name: market_share
   :file: table/input_2012/ms_renovation_ini_2012.csv
   :header-rows: 1
   :stub-columns: 1
```

Intangible costs are calibrated so that the life-cycle cost model, fed with the investment costs reported in
{numref}`renovation_cost`, matches the market shares reported in {numref}`market_share`. The resulting intangible
costs for Res-IRF 3.0 are reported in {numref}`intangible_cost`.

```{eval-rst}
.. csv-table:: Averaged intangible costs in 2012 (€/m²)
   :name: intangible_cost
   :file: table/input_2012/intangible_cost_2012.csv
   :header-rows: 1
   :stub-columns: 1
```

The costs reported in {numref}`renovation_cost`, weighted by the market shares reported in {numref}`market_share`
and {numref}`renovation_share_ep`, result in an average renovation cost of 112 €/m², very close to the 110 €/m² value
given by OPEN (9 978 € of average expenditure compared to 91 m²). Compared to the cumulative energy savings they
generate (assuming an average lifetime of 26 years), they correspond to an average “negawatt-hour cost” of 83 €/MWh,
with extreme values of 25 and 446. These values are in line with those recently produced by {cite:ps}`dgtresorBarrieresInvestissementDans2017`.
 
#### Extensive margins

An upgrade an initial label i is determined by its net present value (NPV), calculated as the sum of the life-cycle cost
of the different options f∈{i+1,…,n}, weighted by their market share:

$$NPV_i=∑_{f>i}^n MS_{i,f} * LCC_{i,f}$$

The renovation rate $τ_i$ of dwellings labelled i is then calculated as a logistic function of the NPV:

$$τ_i=\frac{τ_{max}}{1+(τ_{max}/τ_{min} -1) e^{-ρ(NPV_i- NPV_{min})}}$$

with $τ_{min}=0,001%$, $NPV_{min}=-1 000€$ and $τ_{max}=20%$. The logistic form captures heterogeneity in heating
preference and habits, assuming they are normally distributed[^distributed]. Parameter ρ is calibrated, for each type of
decision-maker and each initial label (i.e., 6x6=36 values), so that the NPVs calculated with the subsidies in effect in
2012 {cite:ps}`giraudetExploringPotentialEnergy2012` reproduce the renovation rates described in
{numref}`renovation_share_ep` and {numref}`renovation_rate_dm` and their aggregation represents 3% (686,757
units) of the housing stock of the initial year.

[^distributed]: For a micro-founded justification of the logistic form, see Giraudet et al. (2018, Online Appendix, Figure A3).

```{eval-rst}
.. csv-table:: Renovation share by energy performance label. Source: PUCA (2015)
   :name: renovation_share_ep
   :file: table/input_2012/renovation_share_ep_2012.csv
   :header-rows: 1
   :stub-columns: 1
```

```{eval-rst}
.. csv-table:: Renovation rate by type of dwelling. Source : OPEN (2016) and USH (2017)
   :name: renovation_rate_dm
   :file: table/input_2012/renovation_rate_dm_2012.csv
   :header-rows: 1
   :stub-columns: 1
```

#### Behavioural parameters

##### Discount rate

In private housing, discount rates are differentiated by housing type in order to capture the heterogeneous constraints
faced by investors {numref}`discount_rate_existing`. Specifically, the discount rates decrease with the owner’s income to reflect
tighter credit constraints faced by lower-income households. Discount rates are also higher in multi-family housing than
in single-family homes to capture the difficulties associated with decision-making within homeowner associations. In
social housing, on the other hand, the discount rate is set at 4%, the value commonly used in public decision-making.

```{eval-rst}
.. csv-table:: Discount rates. Source: Expert opinion
   :name: discount_rate_existing
   :file: table/input_2012/discount_rate_existing_2012.csv
   :header-rows: 1
   :stub-columns: 1
```

##### Investment horizon

The investment horizon is subject to different scenario variants, reflecting different intensities of market
capitalization of energy savings {numref}`investment_horizon`:

- In the ‘full capitalization’ scenario, the investment horizon corresponds to the entire lifetime of energy retrofits,
  i.e., 30 years for improvements on the envelope and 16 years for the improvements on heating systems. Investors enjoy
  the benefits of the investment as long as they own the property (possibly in the form of higher rents); upon
  reselling, they receive a premium equal to the discounted sum of the residual monetary savings generated by the
  investment.
- The reference scenario corresponds to a situation where the horizon of landlords is reduced to three years, the
  average term of a lease. This assumption reflects an inability to increase rents in an attempt to recoup investment.
  This situation, often referred to as the “landlord-tenant dilemma,” is the most common in practice {cite:ps}`giraudetMoralHazardEnergy2018`.
- In the ‘no sale capitalization,’ the investment horizon is limited to seven years, equivalent to the average length of
  ownership of a property. This assumption totally ignores the residual benefits of the investment at the time of
  resale.
- In the ‘no sale nor rent capitalization’ scenario, the owner-tenant dilemma adds to the lack of capitalization of the
  resale premium.

```{eval-rst}
.. csv-table:: Investment horizon for improvements on the envelope (on heating systems)
   :name: investment_horizon
   :file: table/input_2012/investment_horizon_2012.csv
   :header-rows: 1
   :stub-columns: 1
```

#### Endogenous technical change

In both new construction and renovation, the life-cycle costs of the various energy efficiency options decrease
endogenously with their cumulative production. These mechanisms are calibrated as in the previous version of the model
as follows:

- Investment costs decrease exponentially with the cumulative sum of operations to capture the classical
  “learning-by-doing” process. The rate of cost reduction is set at 15% in new construction and 10% in renovation for a
  doubling of production. The lower value in the former case is motivated by the fact the renovation technologies tend
  to be more mature.
- Intangible renovation costs decrease according to a logistic curve with the same cumulative production to
  capture peer effects and knowledge diffusion. The rate of decrease is set at 25% for a doubling of cumulative
  production.

In both cases, reductions in the life-cycle cost of an option increase its market share compared to that of alternative
options.

```{bibliography}
:style: unsrt
```