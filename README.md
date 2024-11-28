# Component models for the framework [`EnergyModelsX`](https://github.com/EnergyModelsX)

This README provides an overview of the different component models of [`EnergyModelsX`](https://github.com/EnergyModelsX) within iDesignRES.

It is handled by SINTEF Energy Research and part of Work package 1, Tasks 1.2 and 1.3.

## Purpose of the model

[`EnergyModelsX`](https://github.com/EnergyModelsX) is an energy system modelling framework developed using the language [Julia](https://julialang.org/).
It provides the user with flexibility with respect to technology descriptions and design of individual model instances.
Within *iDEsignRES*, it will be utilized with a receding horizon framework in stress-testing the energy systems received from capacity expansion models.
This stress testing includes a more detailed description of the different technologies to account for limitations in the operation of the energy system using a NUTS-2 geographical resolution.

## Model design philosophy

One key aim in the development of `EnergyModelsX` was to create an energy system model that

1. offers maximum flexibility with respect to the description of technologies,
2. is simple to extend with additional features without requiring changes in the core structure, and
3. is designed in a way such that the thought process for understanding the model is straight forward.

Julia as a programming language offers the flexibility required in points 1 and 2 through the concept of multiple dispatch.
`EnergyModelsX` hence focuses on only creating variables and constraints that are used, instead of creating all potential constraints and variables and constrain a large fraction of these variables to a value of 0.
In that respect, `EnergyModelsX` moves away from a parameter driven flexibility to a type driven flexibility.
Point 3 is achieved through a one direction flow in function calls, that is that we limit the number of required files and function calls for the individual technology constraint creations, and meaningful names of the individual functions.

The general concept of `EnergyModelsX` is based on a graph structure.
The technologies within the modeled energy system are represented by `Node`s.
These `Node`s correspond to, *eg*, a hydropower plant, a gas turbine, or the Haber-Bosch process.
The individual nodes are then connected *via* `Link`s/edges representing the transport of mass or energy between the technologies.

## Input to and output from the model

The input to the model varies depending on the chosen technology options.
In general, `EnergyModelsX` requires as input the topology of the energy system, that is the available technologies and the links between them.
The individual technologies may require different input parameters while investment options allow for even more combinations.
The demand for energy carriers (or mass) can be incorporated using different approaches.
In general, a time dependent demand for each individual energy carrier can be implemented through soft constraints.
It is hence not necessary to satisfy the demand if the costs are prohibitive or if it is infeasible.

The output from the model differs as well, depending on the chosen packages.
It provides the user in general with the optimal dispatch of the included technologies.
If investments are included, it furthermore provides the user with optimal capacities for each technology.

In the context of *iDesignRES*, `EnergyModelsX` model instances obtain the system topology and some technology parameters directly from a capacity expansion model.
The energy system is then stress-tested using a receding horizon framework.
The output is then given by the ability of the energy system to satisfy varying energy demand and renewable power generation profiles.

## Implemented features

[`EnergyModelsBase`](https://github.com/EnergyModelsX/EnergyModelsBase.jl) is by default an operational model without geographic representation.
It hence describes a local energy system.
SINTEF has developed multiple extension packages to incorporate additional features.
These extensions packages provide additional features like a capacity expansion model, a geographical model, or detailed descriptions of different technologies.
All packages are either already registered in the [Julia Registry](https://github.com/JuliaRegistries/General) or will be registered in the near future.
This simplifies the usage of `EnergyModelsX`.

The following extension packages are available for `EnergyModelsX` and will be applied in the *iDesignRES* project:

- [`EnergyModelsRenewableProducers`](https://github.com/EnergyModelsX/EnergyModelsRenewableProducers.jl) adds technology descriptions for modelling non-dispatchable renewable energy sources, simple and detailed hydropower, as well as battery storage,
- [`EnergyModelsHydrogen`](https://github.com/EnergyModelsX/EnergyModelsHydrogen.jl) adds technology descriptions for electrolysis, natural gas reforming and hydrogen storage,
- [`EnergyModelsCO2`](https://github.com/EnergyModelsX/EnergyModelsCO2.jl) adds technology descriptions for CO₂ retrofit and CO₂ storage,
- [`EnergyModelsHeat`](https://github.com/EnergyModelsX/EnergyModelsHeat.jl) adds technology descriptions for heat exchange between streams at different temperature levels, combined heat and power plants, district heating pipelines, heat pumps, and thermal energy storage, and
- [`EnergyModelsGeography`](https://github.com/EnergyModelsX/EnergyModelsGeography.jl) adds features to model different areas with local energy systems as well as the transmission of energy or mass between these regions.

The different packages can be combined with each other or only a subset of the different packages.
Each individual repository includes the module and the documentation, including examples of the application of the implemented features.
The documentation furthermore describes each developed technology in more detail.

In addition, the following extensions are available, but not directly used within the *iDesignRES* project:

- [`EnergyModelsInvestments`](https://github.com/EnergyModelsX/EnergyModelsInvestments.jl) adds the potential for using `EnergyModelsX` as capacity expansion model and
- [`EnergyModelsGUI`](https://github.com/EnergyModelsX/EnergyModelsGUI.jl) to illustrate the energy system and its results graphically.

> [!CAUTION]
> Not all technology descriptions support investment decisions. In this case, it is clearly stated on the individual description in the documentation if you have to be careful when including investment decisions.

## Core assumption

The general design of `EnergyModelsX` has as core assumption *perfect foresight*.
Although the time structure allows for inclusion of strategic uncertainty, it is not yet tested within the different packages.

The individual packages may have additional core assumptions.
These assumptions are stated on the individual nodal fields.
