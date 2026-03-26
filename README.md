## **General Information and Purpose**

**TE ID & Name:** TE-15 - Proactive maintenance services

**Description and purpose:** TE-15 provides **predictive and proactive maintenance services** aiming to ensure **safe operation**, **continuity of service** and **reduced maintenance-related operational costs** for electrified buildings. The service focuses on **early fault detection**, **fault diagnosis**, and, where feasible, **prognosis / Remaining Useful Life (RUL) estimation**, supporting the reduction of unexpected downtime and improving maintenance planning.

The approach follows **Reliability Centered Maintenance (RCM)** principles and **Failure Mode Effects and Criticality Analysis (FMECA)** logic for identifying critical faults, selecting suitable detection methods and prioritising maintenance actions.

At the current stage (**D4.2**), TE-15 progress is mainly demonstrated through **methodological development** and **validation using available datasets**. Availability of final pilot operational data and real fault events remains a known constraint. Therefore, part of the work relies on **historical datasets**, **simulated / pseudo-fault scenarios**, and/or **non-final datasets** to validate the workflow and algorithms. This does not affect the overall service concept, data needs and integration design, which remain applicable as pilots mature.

**Lead partner/Contact Information:**

- Lead (TE-15): **Not explicitly specified in the provided text**
- Main stakeholders / pilot relevance: asset-oriented maintenance analytics across relevant project partners and pilots

**Target Front Runners/Pilots:**

- **FR2 - Croatia** — heat pump-related assets (subject to final availability / installation schedule)
- **FR6 - Spain** — heat pump-related assets and **PVT** technologies

**Additional asset note:**

- **Batteries:** Battery data is expected to be available, but not necessarily from **FR2 / FR6**. Data may originate from other locations, in line with the project’s evolving deployment plan and possible amendment context.

**Deployment note:** The exact deployment locations for some assets, especially **batteries**, may change due to installation planning and data-sharing constraints. TE-15 remains applicable because the services are **asset-class driven** (e.g. batteries, heat pumps, PV/PVT) and require standard operational variables. Therefore, the maintenance analytics workflow and integration design remain valid even if the physical location changes.

**Assets in scope (indicative):**

- **Heat pumps (HP)**
- **Batteries** (with focus on cycling / charging / discharging behaviour)
- **PVT systems** (as data becomes available)
- **Auxiliary equipment** where operational telemetry is available (e.g. inverters, BMS / EMS related signals)

TE-15 operates as an **analytics service** consuming **operational time-series data** from pilots and/or repositories and producing **health indicators** and **maintenance-related outputs**.

The service can be understood as a layered predictive-maintenance workflow composed of:

1. **Data Sources / Producers**
   - Building / pilot monitoring systems
   - Sensors and metering infrastructure
   - Building Management Systems (**BMS**) / Energy Management Systems (**EMS**)
   - Equipment logs and alarms, where available
   - Cloud storage / data repositories (currently under feasibility study)

2. **Data Preparation Layer**
   - Data preprocessing and validation
   - Handling of missing values, outliers and inconsistent measurements
   - Dataset quality checks and reproducible gap-handling strategies

3. **Predictive Maintenance Analytics Core**
   - Condition monitoring and feature extraction
   - Fault detection / anomaly detection
   - Diagnosis of likely fault type or affected component
   - Health indicator computation (e.g. degradation indices)
   - Prognosis / RUL estimation where feasible

4. **Output / Service Layer**
   - Fault and anomaly alerts
   - Health indicators and degradation trends
   - Maintenance recommendations prioritised by severity / criticality
   - Visual analytics and dashboards

**Example outputs already used (battery analytics):**

- Evolution of **internal resistance**
- **State of Health (SoH)** estimate over cycles
- **Damage index** ranging from 0 to 1 indicating damage state

## **Functional Requirements**

Describe the core capabilities of the TE and the functions it provides.  
Focus on what the TE does, not how it is implemented.

- **Data Acquisition & Ingestion:** Consume time-series operational telemetry such as voltage, current and, where available, temperatures, power and logs.
- **Data Preprocessing & Quality Handling:** Detect and handle missing values, gaps and outliers, and support documented, reproducible gap-handling strategies.
- **Condition Monitoring:** Extract relevant features and build condition indicators for targeted assets.
- **Fault Detection:** Detect abnormal behaviour and early degradation signals.
- **Fault Diagnosis:** Identify the likely fault type or affected component where supported by available data.
- **Health Indicators Computation:** Compute health metrics such as **SoH**, resistance-based indicators and derived degradation indexes.
- **Prognosis / RUL Estimation:** Estimate **Remaining Useful Life** or degradation trajectory when data conditions permit.
- **Alerting & Reporting:** Provide alerts and/or periodic reports for operators.
- **Advisory Maintenance Planning:** Produce recommendations prioritised by **severity** and **criticality**, aligned with **RCM / FMECA** logic.

## **Non-Functional Requirements**

- **Performance:**
  - Primarily designed for **batch / periodic analytics** over historical windows, such as hourly or daily updates
  - Not intended for strict real-time control
  - Preferred data refresh granularity: **5-15 minutes** where available; event-based logs as available
  - Processing latency: typically **minutes to hours**, depending on dataset size and analytics pipeline
- **Reliability and Availability:**
  - Supports **offline analysis** when streaming data is not continuously available
  - Can operate with **delayed data** (“nearline”), as it relies on trends rather than instantaneous control loops
  - Supports **modular pipeline evolution** as new assets and variables become available
- **Maintainability:**
  - Modular analytics pipeline that can be extended to new assets, variables and maintenance use cases as pilot data evolves
- **Security (authentication, authorisation, data encryption, data privacy):**
  - Processes mainly **operational technical data**, not personal data
  - Access should be **read-only** where possible
  - Uses **pseudonymised asset identifiers** where applicable
  - GDPR-friendly by design through **data minimisation** and **purpose limitation**

## **Service Interfaces**

#### **Logical Interfaces**

At **D4.2** stage, interfaces are described at a **logical level** and implementation details may evolve.

#### **Input Interfaces**

- **Time-series operational data feed** in preferred formats such as **CSV**, **JSON**, database exports or APIs where available
- **Repository / cloud storage access** (currently under feasibility study)
- **Optional log / event export**, including alarms or fault flags where available

#### **Output Interfaces**

- **Health indicators** (JSON / CSV export; dashboard views)
- **Alerts / notifications** (event-based, severity-labelled)
- **Reports** (periodic summaries, maintenance recommendations)

#### **Interaction Notes**

- TE-15 does **not require strict real-time streaming**
- Historical data access is sufficient for most current analytics
- Integration with the **WP4 interoperability layer (TE-10)** is desirable for consistent data access and format harmonisation

#### **API Endpoints**

At the current stage, **API endpoints have not been fully specified yet**.

For this TE, the interfaces are currently described at **logical level**:

- **REST / integration APIs:** To be defined as implementation matures
- **Primary integration mode:** Batch or periodic ingestion of telemetry, logs and operational datasets
- **Output artefacts:** Health indicators, alerts, dashboards and maintenance recommendation reports
- **Data exchange:** Preferably harmonised through **TE-10** where applicable

#### **UI Mockups (if applicable)**

- Under study: **Veolia** has a storage-based service for data sharing
- The facilitation potential is being studied for TEs involved in **remote maintenance**, including **TE-6** and **TE-7** from **Tecnalia**
- Indicative UI concepts include:
  - **Asset health dashboard:** SoH, degradation trends, resistance evolution and damage index
  - **Maintenance alert view:** Severity-labelled warnings and recommended actions
  - **Fleet analytics view:** Cross-asset monitoring, anomaly comparison and maintenance prioritisation

## **Data Model**

- **Core entities:**
  - **Asset** (HeatPump, Battery, PV/PVT, Inverter, AuxiliaryEquipment)
  - **Measurement** (timestamped signals such as voltage, current, power, temperature)
  - **OperationalState** (charging / discharging modes, operating states)
  - **FaultEvent** (if logs / events exist)
  - **HealthIndicator** (SoH, resistance-based indicators, degradation index)
  - **MaintenanceRecommendation** (actions, severity, priority)

- **Key relationships:**
  - **Asset** → has **Measurements**
  - **Asset** → has **HealthIndicators**
  - **HealthIndicator** → supports **MaintenanceRecommendation**
  - **FaultEvent** → associated with **Asset** where available

- **Logical schema / artefacts:**
  - assets: {asset_id, asset_type, pilot_id, metadata, pseudonymised_identifier}
  - measurements: {asset_id, ts, variable_name, value, unit, quality_flag}
  - operational_states: {asset_id, ts, mode, status, derived_state}
  - fault_events: {event_id, asset_id, ts, fault_type, severity, source}
  - health_indicators: {indicator_id, asset_id, ts, soh, internal_resistance, damage_index, degradation_score}
  - maintenance_recommendations: {recommendation_id, asset_id, related_indicator_id, severity, priority, action, generated_at}
  - analytics_runs: {run_id, dataset_id, asset_scope, method_version, execution_ts, status}

## **Data Requirements**

Below values reflect **preferred / expected ranges**. Actual availability depends on **pilot readiness** and **data-sharing constraints**.

| Data Category | Data Description | Source Type | Temporal Granularity | Spatial Scope | Historical Depth | Access Mode |
| --- | --- | --- | --- | --- | --- | --- |
| Battery Voltage | Voltage during charge/discharge cycles | Battery monitoring / dataset provider | 5-15 min or per cycle | Asset | As available | Read-only |
| Battery Current | Current during charge/discharge cycles | Battery monitoring / dataset provider | 5-15 min or per cycle | Asset | As available | Read-only |
| Battery Internal Resistance | Derived or provided signal | Dataset / computed | Per cycle / periodic | Asset | As available | Read-only |
| Battery SoH | State of Health indicator | Dataset + computed | Per cycle / periodic | Asset | As available | Read-only |
| HP Operational Telemetry | Temperatures, power, status flags | BMS / EMS / HP monitoring | 5-15 min | Asset / Building | Partial / to be confirmed | Read-only |
| PV / PVT Telemetry | Generation, temperatures, status | Monitoring system | 5-15 min | Asset / Building | To be confirmed | Read-only |
| Event Logs / Alarms | Fault flags, alarms, maintenance logs | BMS / EMS logs | Event-based | Asset / Building | Likely limited initially | Read-only |

**Handling of gaps / outliers (example already observed):**

- Battery internal resistance data may present **gaps** and **outliers**
- Two strategies are supported:
  - **Remove problematic segments** (risk: data loss)
  - **Complete missing values using trend-based interpolation** (risk: pseudo values, but preserves cycles)
- The selected strategy should be **documented per dataset**

## **Integration and Dependencies**

- **External dependencies:** Availability of operational datasets and basic asset metadata; pilot readiness; feasibility of repository / cloud storage access
- **System dependencies:** Analytics pipeline for preprocessing, feature extraction, fault detection, diagnosis and health indicator computation
- **Third-party integrations:** Potential data-sharing and storage mechanisms under study, including Veolia-related infrastructure; possible future links to dashboards and digital twin services
- **Edge/Cloud integration:** TE-15 is primarily an **analytics service**, consuming telemetry and logs from repositories, APIs or platform exports rather than operating a closed-loop controller
- **Data space integration:** Integration with **TE-10** is beneficial to standardise data access, formatting and metadata where implemented

**Integration opportunities:**

- **Digital Twin (TECNALIA / T4.6):** Provide health indicators and maintenance alerts to enrich digital twin monitoring views
- **Flexibility / optimisation services (e.g. TE-14 / T4.4):** Maintenance outputs can inform operational constraints, such as degraded asset capacity
- **WP2 / WP3 data repositories:** Reuse established data access mechanisms where available

**Data exchange patterns:**

- Primarily **read-only** consumption of telemetry and logs by TE-15
- Outputs published as **indicators**, **alerts** and **reports** for dashboards / digital twin consumption
- No direct control loop is required

## **Security and Privacy**

- **Data Sensitivity:** TE-15 uses technical operational data only and does not require personal data.
- **Access Control:** Access should be limited to authorised project actors according to the **principle of least privilege**.
- **Privacy:** Asset identifiers can be **pseudonymised**.
- **Data governance:** Where pseudo / simulated data is used for fault emulation, it should be clearly documented and separated from real pilot data.
- **Compliance:** GDPR-friendly by design through limited-scope operational analytics.

## **Current Status**

This section summarises the current maturity of TE-15 at the **D4.2** stage.

**Implementation status (summary):**

- Methodology for predictive maintenance is defined through an **RCM / FMECA-driven workflow**
- A **battery analytics pipeline** has been implemented and demonstrated with available dataset(s), including:
  - **internal resistance evolution analysis**
  - **SoH computation** and validation through comparison to available SoH values
  - **damage index (0-1)** calculation

**Validation evidence:**

- Validation is currently performed mainly **offline**, using available datasets
- Dataset quality issues such as **missing values** and **gaps** have been analysed
- Mitigation strategies have been proposed, including **removal** versus **trend-based completion**

**Main limitations / risks:**

- Limited access to final pilot operational data and real fault event data at **D4.2**
- Changes in asset installation locations, especially for **batteries**, may still occur
- Despite these constraints, the analytics workflow remains applicable across equivalent asset classes

## **Next Steps**

Concrete actions toward further maturation:

1. **Confirm definitive data sources and data availability** per pilot and asset class.
2. **Finalise the expected variable list and granularity** with technology providers and pilot owners.
3. **Extend validation using additional datasets**, including public datasets if needed, and/or pseudo-fault scenarios when real faults are not available.
4. **Progress integration pathways with TE-10** and digital twin interfaces as the WP4 architecture evolves.
5. **Expand beyond battery analytics** as sufficient pilot data becomes available for heat pumps, PV/PVT and auxiliary equipment.
