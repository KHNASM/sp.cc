# Solar Panel Calculator Documentation

This document provides detailed information about the solar panel calculator system, including descriptions of input parameters, calculation methodologies, and implementation details.

## Table of Contents

1. [Overview](#overview)
2. [Key Parameters](#key-parameters)
3. [Calculation Methodology](#calculation-methodology)
4. [System Specifications](#system-specifications)
5. [Energy Distribution](#energy-distribution)
6. [Cost Analysis](#cost-analysis)
7. [Financial Metrics](#financial-metrics)
8. [Investment Comparison](#investment-comparison)
9. [Implementation Details](#implementation-details)

## Overview

The Solar Panel Calculator is a comprehensive tool for estimating solar panel system requirements, costs, and financial returns based on a household's annual electricity consumption. The calculator provides detailed analysis of system specifications, energy distribution, costs, financial metrics, and investment comparisons.

### Core Features

- **Simple Input**: Calculate complete solar system details from just annual electricity consumption
- **Comprehensive Output**: Detailed breakdown of system specifications, costs, and financial returns
- **Investment Analysis**: Compare solar investment with stock market alternatives
- **Customizable Parameters**: Advanced users can adjust key parameters for more precise calculations
- **Database Integration**: Market parameters can be loaded from a database for up-to-date calculations

## Key Parameters

### Market Parameters

These represent current market conditions and can be updated from a database:

| Parameter | Default Value | Description |
|-----------|---------------|-------------|
| SolarPanelPriceSEK | 4,500 | Price per solar panel in SEK |
| InverterBasePriceSEK | 25,000 | Base price for inverter in SEK |
| Battery5kWPriceSEK | 13,000 | Price for 5kW battery unit in SEK |
| InstallationCostPercentage | 20% | Installation cost as percentage of hardware |
| StandardPanelRatedPowerWp | 440 | Standard panel power in Wp |
| PanelAreaSquareMeters | 1.7 | Area per panel in square meters |
| InverterEfficiencyPercentage | 90% | Inverter efficiency as percentage of rated power |
| StandardInverterSizeKW | 15 | Standard inverter size in kW |
| SolarSystemPercentage | 88% | Target system percentage of annual consumption |
| EnergyCalculationPercentage | 97% | Energy calculation percentage for conversion from rated power to actual production |
| DefaultSelfConsumedPercentage | 70% | Typical percentage of produced electricity consumed by household |
| DefaultSoldElectricityPercentage | 15% | Typical percentage of produced electricity sold back to grid |
| ElectricityPurchasePriceSEK | 1.95 | Electricity purchase price per kWh in SEK |
| ElectricitySellingPriceSEK | 0.60 | Electricity selling price per kWh in SEK |
| DefaultBankInterestRatePercentage | 4.1% | Default bank loan interest rate |
| StockMarketReturnRatePercentage | 10.4% | Expected stock market return rate |
| AnnualDegradationRatePercentage | 0.3% | Annual panel performance degradation rate |
| AnnualMaintenanceCostPercentage | 1.2% | Annual maintenance cost as percentage of system cost |
| GreenTechDeductionPercentage | 19.4% | Green technology tax deduction percentage |
| MaxDeductionPerPersonSEK | 50,000 | Maximum deduction per person in SEK |
| DefaultNumPersonsForDeduction | 2 | Default number of persons eligible for deduction |
| SolarPanelLifetimeYears | 25 | Expected lifetime of solar panels in years |
| DefaultLoanPeriodYears | 10 | Default loan period in years |

### System Parameters

These are derived from market parameters but can be customized:

| Parameter | Description |
|-----------|-------------|
| AnnualElectricityConsumption | Annual electricity usage in kWh (primary input value) |
| SolarSystemPercentage | Percentage of consumption to be covered by solar (affects system size) |
| EnergyCalculationPercentage | Conversion factor from rated power to actual production |
| PanelRatedPower | Individual panel power rating in Wp |
| SelfConsumedPercentage | Percentage of generated electricity used by household |
| SoldElectricityPercentage | Percentage of generated electricity sold to grid |
| LoanPeriodYears | Duration of financing in years |
| BankInterestRate | Annual interest rate on loan |
| AlternativeInvestmentRate | Return rate for alternative investments |
| DegradationRate | Annual performance loss percentage |
| MaintenanceCostPercentage | Annual maintenance cost as percentage of system cost |
| ElectricityPurchasePrice | Cost per kWh bought from grid in SEK |
| ElectricitySellingPrice | Compensation per kWh sold to grid in SEK |

## Calculation Methodology

This section outlines the mathematical formulas used in the calculator.

### System Specifications

#### Rated Power Calculation

The rated power of the system is calculated based on annual consumption:

```
RatedPower (Wp) = AnnualConsumption (kWh) × (SolarSystemPercentage / 100)
```

#### Number of Panels

The number of solar panels is calculated based on rated power:

```
NumberOfPanels = floor(RatedPower (Wp) / PanelRatedPower (Wp))
```

#### Actual System Rated Power

The actual system rated power is calculated based on the number of panels:

```
TotalRatedPower (Wp) = NumberOfPanels × PanelRatedPower (Wp)
```

#### Annual Energy Production

The annual energy production is calculated based on the system's rated power:

```
AnnualEnergyProduction (kWh) = TotalRatedPower (Wp) × (EnergyCalculationPercentage / 100)
```

#### Inverter Size

The required inverter size is calculated based on the system's rated power:

```
InverterSize (W) = TotalRatedPower (Wp) × (InverterEfficiencyPercentage / 100)
```

#### Number of Inverters

The number of inverters needed is calculated based on the required inverter size:

```
NumberOfInverters = max(1, ceiling(InverterSize (W) / StandardInverterSize (W)))
```

#### Required Roof Area

The required roof area is calculated based on the number of panels:

```
RequiredRoofArea (m²) = NumberOfPanels × PanelArea (m²)
```

### Energy Distribution

#### Self-Consumed Electricity

The amount of electricity self-consumed by the household:

```
SelfConsumedElectricity (kWh) = AnnualEnergyProduction (kWh) × (SelfConsumedPercentage / 100)
```

#### Sold Electricity

The amount of electricity sold back to the grid:

```
SoldElectricity (kWh) = AnnualEnergyProduction (kWh) × (SoldElectricityPercentage / 100)
```

#### Purchased Electricity

The amount of electricity still needed to be purchased from the grid:

```
PurchasedElectricity (kWh) = AnnualConsumption (kWh) - SelfConsumedElectricity (kWh)
```

### Cost Analysis

#### Panel Costs

The total cost of solar panels:

```
PanelsCost (SEK) = NumberOfPanels × PanelPrice (SEK)
```

#### Inverter Costs

The total cost of inverters:

```
InvertersCost (SEK) = NumberOfInverters × InverterPrice (SEK)
```

#### Total Hardware Cost

The total hardware cost:

```
HardwareCost (SEK) = PanelsCost (SEK) + InvertersCost (SEK) + BatteriesCost (SEK)
```

#### Installation Cost

The cost of installation:

```
InstallationCost (SEK) = HardwareCost (SEK) × (InstallationCostPercentage / 100)
```

#### Total System Cost Before Incentives

The total system cost before government incentives:

```
TotalCostBeforeIncentives (SEK) = HardwareCost (SEK) + InstallationCost (SEK)
```

#### Green Technology Deduction

The government subsidy amount:

```
MaxDeduction (SEK) = MaxDeductionPerPerson (SEK) × NumPersonsForDeduction
PotentialDeduction (SEK) = TotalCostBeforeIncentives (SEK) × (GreenTechDeductionPercentage / 100)
GreenTechDeduction (SEK) = min(PotentialDeduction (SEK), MaxDeduction (SEK))
```

#### Total System Cost After Incentives

The final cost after government incentives:

```
TotalCostAfterIncentives (SEK) = TotalCostBeforeIncentives (SEK) - GreenTechDeduction (SEK)
```

### Financial Metrics

#### Annual Saving from Self-Consumption

The yearly savings from self-consumed electricity:

```
AnnualSavingSelfConsumption (SEK) = SelfConsumedElectricity (kWh) × ElectricityPurchasePrice (SEK/kWh)
```

#### Annual Income from Sold Electricity

The yearly income from electricity sold to the grid:

```
AnnualIncomeSoldElectricity (SEK) = SoldElectricity (kWh) × ElectricitySellingPrice (SEK/kWh)
```

#### Total Annual Benefit

The total yearly financial benefit:

```
TotalAnnualBenefit (SEK) = AnnualSavingSelfConsumption (SEK) + AnnualIncomeSoldElectricity (SEK)
```

#### Annual Maintenance Cost

The yearly maintenance expenses:

```
AnnualMaintenanceCost (SEK) = TotalCostBeforeIncentives (SEK) × (MaintenanceCostPercentage / 100)
```

#### Monthly Loan Payment

The monthly loan payment (using the annuity formula):

For interest rate > 0:
```
MonthlyLoanPayment (SEK) = LoanAmount (SEK) × (r(1+r)^n / ((1+r)^n - 1))
```

where:
- r is the monthly interest rate (annual rate divided by 12)
- n is the total number of payments (loan term in years multiplied by 12)

For interest rate = 0:
```
MonthlyLoanPayment (SEK) = LoanAmount (SEK) / n
```

#### Net Annual Cash Flow

The net yearly cash flow:

```
NetAnnualCashFlow (SEK) = TotalAnnualBenefit (SEK) - AnnualMaintenanceCost (SEK) - YearlyLoanPayment (SEK)
```

#### Simple Payback Period

The simple payback period in years:

```
SimplePaybackYears = TotalCostAfterIncentives (SEK) / (TotalAnnualBenefit (SEK) - AnnualMaintenanceCost (SEK))
```

#### Payback with Opportunity Cost

The payback period considering opportunity cost:

```
OpportunityCost (SEK) = TotalCostAfterIncentives (SEK) × (AlternativeInvestmentRate / 100)
PaybackWithOpportunityCostYears = TotalCostAfterIncentives (SEK) / (TotalAnnualBenefit (SEK) - OpportunityCost (SEK))
```

#### Total Lifetime Profit

The total profit over the system's lifetime:

```
YearlyDegradationFactor = 1 - (DegradationRate / 100)
```

For each year from 1 to SystemLifetime:
```
YearlyBenefit (SEK) = TotalAnnualBenefit (SEK) × YearlyDegradationFactor^(year-1)
TotalLifetimeBenefit (SEK) = Sum of all YearlyBenefit values
```

```
TotalLoanPayments (SEK) = YearlyLoanPayment (SEK) × LoanPeriodYears
TotalMaintenance (SEK) = AnnualMaintenanceCost (SEK) × SystemLifetime
TotalLifetimeProfit (SEK) = TotalLifetimeBenefit (SEK) - TotalLoanPayments (SEK) - TotalMaintenance (SEK)
```

### Investment Comparison

#### Stock Market Return

The projected stock market return:

```
StockMarketReturn (SEK) = TotalCostAfterIncentives (SEK) × (1 + (AlternativeInvestmentRate / 100))^SystemLifetimeYears
```

#### Solar System Return

The projected solar system return:

```
SolarSystemReturn (SEK) = TotalCostAfterIncentives (SEK) + TotalLifetimeProfit (SEK)
```

#### Investment Difference

The difference between the two investment options:

```
InvestmentDifference (SEK) = |SolarSystemReturn (SEK) - StockMarketReturn (SEK)|
```

## System Specifications

This section describes the system specification calculations and outputs in detail.

### Input

- **Annual Electricity Consumption**: The household's annual electricity usage in kWh.

### Output

- **Rated Power (Wp)**: The theoretical rated power of the solar system in Watt-peak.
- **Number of Panels**: The total number of solar panels required.
- **Total Rated Power (Wp)**: The actual system rated power based on the number of panels.
- **Annual Energy Production (kWh)**: The expected yearly energy production.
- **Inverter Size (W)**: The required inverter capacity.
- **Number of Inverters**: The number of inverters needed.
- **Required Roof Area (m²)**: The roof space needed for the solar panels.

## Energy Distribution

This section describes the energy flow calculations in detail.

### Input

- **Self-Consumed Percentage**: The percentage of produced electricity consumed by the household.
- **Sold Electricity Percentage**: The percentage of produced electricity sold to the grid.

### Output

- **Self-Consumed Electricity (kWh)**: The amount of electricity produced and consumed on-site.
- **Sold Electricity (kWh)**: The amount of electricity sold back to the grid.
- **Purchased Electricity (kWh)**: The amount of electricity still needed from the grid.

## Cost Analysis

This section describes the cost calculation details.

### Input

- **Panel Price (SEK)**: The price per solar panel.
- **Inverter Price (SEK)**: The price per inverter.
- **Installation Cost Percentage**: The installation cost as a percentage of hardware cost.
- **Green Tech Deduction Percentage**: The government subsidy percentage.
- **Max Deduction Per Person (SEK)**: The maximum subsidy per person.
- **Number of Persons for Deduction**: The number of people eligible for the subsidy.

### Output

- **Panels Cost (SEK)**: The total cost of solar panels.
- **Inverters Cost (SEK)**: The total cost of inverters.
- **Installation Cost (SEK)**: The cost of installation.
- **Total Cost Before Incentives (SEK)**: The total system cost before government incentives.
- **Green Tech Deduction (SEK)**: The government subsidy amount.
- **Total Cost After Incentives (SEK)**: The final cost after government incentives.

## Financial Metrics

This section describes the financial return calculations in detail.

### Input

- **Electricity Purchase Price (SEK/kWh)**: The cost per kWh bought from the grid.
- **Electricity Selling Price (SEK/kWh)**: The compensation per kWh sold to the grid.
- **Bank Interest Rate (%)**: The annual interest rate on the loan.
- **Loan Period (years)**: The duration of financing.
- **Maintenance Cost Percentage (%)**: The annual maintenance cost as a percentage of system cost.
- **Degradation Rate (%)**: The annual performance loss percentage.

### Output

- **Annual Saving from Self-Consumption (SEK)**: The yearly savings from self-consumed electricity.
- **Annual Income from Sold Electricity (SEK)**: The yearly income from electricity sold to the grid.
- **Total Annual Benefit (SEK)**: The total yearly financial benefit.
- **Annual Maintenance Cost (SEK)**: The yearly maintenance expenses.
- **Monthly Loan Payment (SEK)**: The monthly loan installment.
- **Net Annual Cash Flow (SEK)**: The net yearly cash flow.
- **Simple Payback Period (years)**: The simple payback period.
- **Payback with Loan (years)**: The payback period considering loan payments.
- **Total Lifetime Profit (SEK)**: The total profit over the system's lifetime.

## Investment Comparison

This section describes the investment comparison calculations in detail.

### Input

- **Alternative Investment Rate (%)**: The expected annual return rate for alternative investments.
- **System Lifetime (years)**: The expected operational life of the system.

### Output

- **Stock Market Return (SEK)**: The projected return if the same amount was invested in the stock market.
- **Solar System Return (SEK)**: The projected return from the solar system investment.
- **Investment Difference (SEK)**: The absolute difference between the two investment options.
- **Better Investment**: Indicates which investment performs better.

## Implementation Details

The calculator is implemented in C# with the following class structure:

- **MarketParameters**: Contains current market conditions (can be loaded from a database).
- **SolarSystemParameters**: Contains system parameters derived from market parameters (can be customized).
- **SolarCalculationResults**: Contains the calculation results organized by category.
- **SystemSpecifications**: Contains system specification results.
- **EnergyDistribution**: Contains energy distribution results.
- **CostBreakdown**: Contains cost breakdown results.
- **FinancialMetrics**: Contains financial metrics results.
- **InvestmentComparison**: Contains investment comparison results.
- **SolarCalculator**: The main calculator class that performs the calculations.

### Usage Example

```csharp
// Load market parameters (in a real app, this would come from a database)
var marketParams = await MarketParameters.LoadFromDatabaseAsync();

// Create calculator with user's consumption
var calculator = new SolarCalculator(annualConsumption, marketParams);

// Calculate results
var results = calculator.CalculateResults();

// Display results
Console.WriteLine(results.GetFormattedSummary());
```

### Customization Example

```csharp
// Create custom parameters based on default ones
var customParams = new SolarSystemParameters(annualConsumption, marketParams);

// Customize parameters
customParams.SelfConsumedPercentage = 80;
customParams.ElectricityPurchasePrice = 2.1;
customParams.LoanPeriodYears = 15;

// Create calculator with custom parameters
var customCalculator = new SolarCalculator(customParams, marketParams);

// Calculate custom results
var customResults = customCalculator.CalculateResults();
```
