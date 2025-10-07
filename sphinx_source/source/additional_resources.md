# Additional Resources 

This section provides complementary materials that help to better understand some specificities of Res-IRF modelisation. 
You can find here some explanatory notes on various topics. All the notes are written in French. For each note we display an abstract in English. 

- [Heating Systems Market Share Determination](explanatory_notes/20250822_market_share_heater.pdf)

**Abstract:**  
In **Res-IRF**, households can replace their heating system **only when it reaches the end of its life**. When this is the case, the choice of the next heating system is determined by the **heater replacement** function, which uses a **market share matrix** computed by the **endogenous market share heater** function in the `building.py` file.
This matrix plays a **central role** in understanding households’ heating system replacement choices. This note therefore aims to **explain how the market share matrix is constructed** within the Res-IRF model. The objective is also to **clarify the role of the reference matrix** in determining the matrix computed by the model for a given year *t*.
The analysis highlights the **influence of the reference matrix** on the evolution of simulated market shares through the **calibration mechanism**. This influence depends on the coefficients derived from the calibration: the more these coefficients modify the calculated utilities, the more the weight of the reference matrix can be considered **structuring** in the model’s dynamics.


- [Heating intensity](explanatory_notes/20250612_heating_intensity.pdf)

**Abstract:**
Energy consumption calculations at the dwelling scale are performed for so-called **“conventional” building usage behaviors**, so that consumption data can be compared between dwellings and depend **only on the characteristics of the building envelope and the installed energy systems**. This is the case, for example, in the method for calculating the Energy Performance Certificate (DPE) (3CL-DPE 2021) (Ministère de la Transition Écologique 2021), as well as in the analogous method implemented in **Res-IRF**, which is based on the TABULA/EPISCOPE methodology (Loga 2013).
However, this conventional consumption can be quite different from the actual consumption (Aydin, Kok & Brounen 2017; Christensen et al. 2023; Astier et al. 2024), notably due to adjustments in the heating setpoint temperature or the proportion of the dwelling’s floor area that is actually heated. Denoting **$C_{obs}$** as the observed (actual) consumption and **$C_{conv}$** as the conventional consumption, the **usage intensity (UI)** can be defined as:
$
\text{UI} = \frac{C_{\text{obs}}}{C_{\text{conv}}}
$

- [Building stock](explanatory_notes/20250620_building_stock.pdf)

**Abstract:**
National residential energy consumption is proportional to the number of dwellings in the national housing stock; it is therefore a crucial aspect of Res-IRF. This study aims to analyze the building stock trajectories implementing in Res-IRF. It also explains how the new inputs for constructions and demolition have been constructed, according the projection of the SDES. The scope of the study is limited to primary residences in metropolitan France. 

- [Importing a calibration](explanatory_notes/20250624_importer_une_calibration.pdf)

**Abstract:**
The **Res-IRF model** relies on **essential calibration steps** from the very first year of simulation, particularly regarding building energy consumption and renovation barriers. These calibrations must be **strictly identical across scenarios** to allow for a rigorous comparison of results.
This document presents **two complementary solutions** to ensure the consistency of these calibrations:
1. **Neutralizing parameter variations before calibration**, notably by annualizing insulation costs.
2. **Exporting calibrations from a reference scenario** and then **importing them into other scenarios**, offering a more flexible approach.

An implementation of this functionality via **keys added to the configuration files** is presented. Tests show that this method introduces **negligible discrepancies**, validating its robustness and usefulness for **reliable comparative analyses** in Res-IRF.

- [Rebound effect (branch SNBC_run3_DGEC_new)](explanatory_notes/20250922_Effet_rebond_branche_DGEC.pdf)

**Abstract:**

Improvements in **residential building energy efficiency** through renovation works do not always translate into **energy savings proportional to the expected technical gains**. This discrepancy is explained by the so-called **rebound effect**: households, benefiting from a better-insulated dwelling that is cheaper to heat, may be tempted to **increase their comfort level** (e.g., heating certain rooms more, maintaining a higher indoor temperature, or extending the heating period). 
As a result, a portion of the theoretical energy savings is effectively **“recovered”** through increased actual consumption. Empirical studies indicate that this rebound effect leads to **energy savings that are 20% lower than predicted under constant behavior**, and the effect is more pronounced in **households with higher income** (Fack and Giraudet, 2024).  
This note presents how the rebound effect is implementing in Res-IRF, in the SNBC_run3_DGEC_new branch.

- [Analysis of subsidies amount - MPR Performance](explanatory_notes/20250925_analyse_montants_aides.pdf)

**Abstract:**

The **Res-IRF model** is used for the **evaluation of public renovation policies** as well as for simulations of the **SNBC³**. In this context, it is important that the model accurately reproduces **public expenditures** and the **financial incentives** implemented for energy renovation.  
It has been observed that there are **significant discrepancies** between the average subsidy amounts granted by **Res-IRF’s MPR Performance**¹ and the average subsidy amounts provided under the **MaPrimeRénov’ Major Renovation program**², for which figures are supplied by **ANAH**.  
This note therefore aims to **identify the potential sources of these discrepancies** in order to explain them and **adjust the necessary parameters** so that Res-IRF can evaluate public policies with **more realistic budget estimates**.


- [Electric performance boiler in TREMI 2020](explanatory_notes/Analyse_chauffage_electrique_TREMI_2020.pdf)

**Abstract:**

This analysis is based on the **TREMI 2020 survey data**, with a focus on **electric heating systems** and **transitions from electric systems to air-to-water heat pumps (AWHPs)**.  
The objective is to **assess the justification** for not distinguishing between **resistive (Joule effect) heaters** and **electric boilers with hot water loops** in the Res-IRF model. This choice may pose an issue because the transition from a **resistive heater** to an **AWHP** incurs **additional installation costs for a hot water loop**, which are currently **not accounted for in the model**.


Don't hesitate to contact us if you need further explanations.