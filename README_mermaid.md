# Riverload C1 — Minimum Depth (Dmin) Prediction Model

## Overview
This project addresses the **RIVERLOAD C1** challenge: forecasting the minimum river depth ($D_{min}$) a container barge will encounter on its journey along the Rhine. By training on 21 days of historical water gauge levels and discharge rates up to an `issue_time`, the model predicts the lower tail quantiles of depth ($D_{min}$) for specific routes, departure slots, and journey legs 2 to 11 days into the future.

---

## Workflow & System Architecture

```mermaid
flowchart TD
    %% Past Historical Data & Feature Engineering
    subgraph PastData["Past Data Inputs (up to issue_time)"]
        WL["Water Levels<br/>(21 Gauges)"]
        DC["Discharge Data<br/>(16 Flow Series)"]
    end

    subgraph Pipeline["Machine Learning Pipeline"]
        FE["Feature Engineering<br/>(Lags, Upstream Diffs, Trends)"]
        Model["Quantile ML Model<br/>(LightGBM / CatBoost / TFT)"]
        Dist["Predicted Depth Distribution<br/>(Future Dmin)"]
    end

    WL --> FE
    DC --> FE
    FE --> Model
    Model --> Dist

    Dist --> Q05_Out["dmin_q05_cm<br/>(95% safe)"]
    Dist --> Q10_Out["dmin_q10_cm<br/>(90% safe)"]
    Dist --> Q25_Out["dmin_q25_cm<br/>(75% safe)"]
    Dist --> Q50_Out["dmin_q50_cm<br/>(Median)"]

    %% Journey Logic & Ground Truth
    subgraph Journey["Future Journey Logic (2-11 Days Ahead)"]
        Barge["Barge in Rotterdam"] --> RouteChoice{"Route Selection"}
        RouteChoice -->|Via Waal| WaalRoute["Waal Route"]
        RouteChoice -->|Via Lek| LekRoute["Lek Route"]

        WaalRoute --> DepSlot["Departure Slot<br/>(0 to 24 | +2 to +8 days)"]
        LekRoute --> DepSlot

        DepSlot --> LegSeg["Leg Segmentation<br/>(Rotterdam → Duisburg → etc.)"]
        LegSeg --> Timetable["Hourly River Segments<br/>(En-route Timetable)"]
        Timetable --> TargetCalc["Dmin Computation<br/>min(Segment Depths)"]
    end

    TargetCalc -. Evaluated Against .-> Dist

    %% Styling
    classDef inputs fill:#e1f5fe,stroke:#0288d1,stroke-width:1.5px,color:#01579b;
    classDef mlProcess fill:#e8f5e9,stroke:#388e3c,stroke-width:1.5px,color:#1b5e20;
    classDef outputs fill:#fff3e0,stroke:#f57c00,stroke-width:1.5px,color:#e65100;
    classDef domain fill:#f3e5f5,stroke:#7b1fa2,stroke-width:1.5px,color:#4a148c;

    class WL,DC inputs;
    class FE,Model,Dist mlProcess;
    class Q05_Out,Q10_Out,Q25_Out,Q50_Out outputs;
    class Barge,RouteChoice,WaalRoute,LekRoute,DepSlot,LegSeg,Timetable,TargetCalc domain;
