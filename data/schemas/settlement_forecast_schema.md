# Settlement Forecasting Model Schema

## Overview
This schema defines the data structure required for the Power BI settlement forecasting dashboard.

## Data Sources
1. **Filevine:** `Projects`, `Settlements`, `MedicalLiens`, `Costs`.
2. **HubSpot:** `Deals` (Historical conversion data).

## Tables & Fields

### Fact_Settlements
| Field Name | Description | Source |
|---|---|---|
| `MatterID` | Unique project ID | Filevine |
| `SettlementDate` | Actual or Projected date | Filevine |
| `GrossAmount` | Total settlement value | Filevine |
| `AttorneyFees` | Calculated fee (e.g., 33.3%) | Derived |
| `NetToClient` | Final payout | Derived |

### Fact_CaseTimelines
| Field Name | Description | Source |
|---|---|---|
| `MatterID` | | |
| `PhaseName` | Current phase (Discovery, Trial, etc.) | Filevine |
| `DaysInPhase` | Duration for bottleneck analysis | Derived |

## Key DAX Measures (Plan)
*   `Total Projected Fees = SUM(Fact_Settlements[AttorneyFees])`
*   `Avg Cycle Time = AVERAGE(Fact_CaseTimelines[DaysInPhase])`
*   `Liquidity Forecast = CALCULATE([Total Projected Fees], NEXTMONTH())`
