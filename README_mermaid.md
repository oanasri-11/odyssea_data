# Riverload Prediction Model

## Overview
This project predicts the minimum river depth (Dmin) a vessel will encounter on future route legs using historical gauge and discharge data.

## Mermaid Diagram

```mermaid
flowchart TD
    %% Past data flow
    subgraph Past["Past"]
        WaterLevels[Water levels<br/>21 gauges] -->| | Past
        Discharge[Discharge<br/>16 series] -->| | Past
        Past --> issue_time[issue_time]
        issue_time --> FEATURES_X[FEATURES X]
        FEATURES_X --> ML_MODEL[ML MODEL]
        ML_MODEL --> FUTURE_Dmin_DIST[FUTURE Dmin DISTRIBUTION]
        FUTURE_Dmin_DIST --> q05[q05]
        FUTURE_Dmin_DIST --> q10[q10]
        FUTURE_Dmin_DIST --> q25[q25]
        FUTURE_Dmin_DIST --> q50[q50]
    end

    %% River load journey
    subgraph Journey["Future Journey"]
        Riverload_C1[RIVERLOAD C1] --> Barge[BARGE IN ROTTERDAM]
        Barge --> Junction{WAAL / LEK}
        Junction -->|WAAL| Waal[WAAL]
        Junction -->|LEK| Lek[LEK]
        Waal --> DepartureSlot[Departure Slot<br/>0...24]
        Lek --> DepartureSlot
        DepartureSlot --> Leg[Leg]
        Leg --> FutureJourney[FUTURE JOURNEY]
        FutureJourney --> RiverSegments[River segments<br/>Left & Right]
        RiverSegments --> Depths[Depth layers<br/>Depth 1, Depth 2, ...]
        Depths --> Dmin[Dmin<br/>= minimum depth]
        Dmin --> Q05[q05]
        Dmin --> Q10[q10]
        Dmin --> Q25[q25]
        Dmin --> Q50[q50]
    end

    %% Style definitions
    classDef past fill:#f9f,stroke:#333,stroke-width:2px;
    classDef journey fill:#bbf,stroke:#333,stroke-width:2px;
    classDef process fill:#cfc,stroke:#333,stroke-width:2px;
    classDef data fill:#ffc,stroke:#333,stroke-width:2px;
    class Past,FEATURES_X,ML_MODEL,FUTURE_Dmin_DIST process;
    class WaterLevels,Discharge data;
    class Riverload_C1,Barge,Waal,Lek,DepartureSlot,Leg,FutureJourney,RiverSegments,Depths,Dmin journey;
    class q05,q10,q25,q50,data;
```

## Problem Statement
Predict the minimum depth (Dmin) a vessel will encounter for each route, departure slot, and leg using available river information up to issue_time.

## Input
Historical hourly data for 21 days before issue_time, including 21 gauge series and 16 discharge series, plus scenario, route, departure, and leg information.

## Output / Labels
Four quantiles for Dmin:
- q05, q10, q25, q50

## Type of ML
Supervised learning + time-series forecasting + quantile regression

## Metric
Mean Pinball Loss

## Goal
Reduce Pinball Loss, focusing on accurate prediction of the lower tail of Dmin distribution.

## Main Challenge
The future is relatively distant, water waves move through the river, and Dmin is determined by the weakest point along the journey.