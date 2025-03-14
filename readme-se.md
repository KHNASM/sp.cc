# Solcellskalkylator Dokumentation

Det här dokumentet innehåller detaljerad information om solcellskalkylatorsystemet, inklusive beskrivningar av inparametrar, beräkningsmetoder och implementationsdetaljer.

## Innehållsförteckning

1. [Översikt](#översikt)
2. [Nyckelparametrar](#nyckelparametrar)
3. [Beräkningsmetodik](#beräkningsmetodik)
4. [Systemspecifikationer](#systemspecifikationer)
5. [Energidistribution](#energidistribution)
6. [Kostnadsanalys](#kostnadsanalys)
7. [Finansiella nyckeltal](#finansiella-nyckeltal)
8. [Investeringsjämförelse](#investeringsjämförelse)
9. [Implementationsdetaljer](#implementationsdetaljer)

## Översikt

Solcellskalkylatorn är ett omfattande verktyg för att uppskatta solcellssystemkrav, kostnader och finansiell avkastning baserat på ett hushålls årliga elförbrukning. Kalkylatorn ger detaljerad analys av systemspecifikationer, energidistribution, kostnader, finansiella nyckeltal och investeringsjämförelser.

### Kärnfunktioner

- **Enkel input**: Beräkna fullständiga solcellssystemdetaljer från endast årlig elförbrukning
- **Omfattande output**: Detaljerad nedbrytning av systemspecifikationer, kostnader och finansiell avkastning
- **Investeringsanalys**: Jämför solinvestering med aktiemarknadsalternativ
- **Anpassningsbara parametrar**: Avancerade användare kan justera nyckelparametrar för mer precisa beräkningar
- **Databasintegration**: Marknadsparametrar kan laddas från en databas för uppdaterade beräkningar

## Nyckelparametrar

### Marknadsparametrar

Dessa representerar aktuella marknadsförhållanden och kan uppdateras från en databas:

| Parameter | Standardvärde | Beskrivning |
|-----------|---------------|-------------|
| SolarPanelPriceSEK | 4 500 | Pris per solcellspanel i SEK |
| InverterBasePriceSEK | 25 000 | Grundpris för växelriktare i SEK |
| Battery5kWPriceSEK | 13 000 | Pris för 5kW batteripaket i SEK |
| InstallationCostPercentage | 20% | Installationskostnad som procent av hårdvara |
| StandardPanelRatedPowerWp | 440 | Standard paneleffekt i Wp |
| PanelAreaSquareMeters | 1,7 | Yta per panel i kvadratmeter |
| InverterEfficiencyPercentage | 90% | Växelriktarens effektivitet som procent av märkeffekt |
| StandardInverterSizeKW | 15 | Standard växelriktarstorlek i kW |
| SolarSystemPercentage | 88% | Målsystem procent av årlig förbrukning |
| EnergyCalculationPercentage | 97% | Energiberäkningsprocent för omvandling från märkeffekt till faktisk produktion |
| DefaultSelfConsumedPercentage | 70% | Typisk procent av producerad el som förbrukas av hushållet |
| DefaultSoldElectricityPercentage | 15% | Typisk procent av producerad el som säljs tillbaka till nätet |
| ElectricityPurchasePriceSEK | 1,95 | Elköpspris per kWh i SEK |
| ElectricitySellingPriceSEK | 0,60 | Ersättning vid försäljning av el per kWh i SEK |
| DefaultBankInterestRatePercentage | 4,1% | Standard bankränta |
| StockMarketReturnRatePercentage | 10,4% | Förväntad aktiemarknadsavkastning |
| AnnualDegradationRatePercentage | 0,3% | Årlig prestationsförsämring för paneler |
| AnnualMaintenanceCostPercentage | 1,2% | Årlig underhållskostnad som procent av systemkostnad |
| GreenTechDeductionPercentage | 19,4% | Procent för grönt teknikavdrag |
| MaxDeductionPerPersonSEK | 50 000 | Maximalt avdrag per person i SEK |
| DefaultNumPersonsForDeduction | 2 | Standard antal personer som är berättigade till avdrag |
| SolarPanelLifetimeYears | 25 | Förväntad livslängd på solcellspaneler i år |
| DefaultLoanPeriodYears | 10 | Standard låneperiod i år |

### Systemparametrar

Dessa härleds från marknadsparametrar men kan anpassas:

| Parameter | Beskrivning |
|-----------|-------------|
| AnnualElectricityConsumption | Årlig elanvändning i kWh (primärt inmatningsvärde) |
| SolarSystemPercentage | Procent av förbrukning som täcks av solceller (påverkar systemstorlek) |
| EnergyCalculationPercentage | Omvandlingsfaktor från märkeffekt till faktisk produktion |
| PanelRatedPower | Individuell panels märkeffekt i Wp |
| SelfConsumedPercentage | Procent av genererad el som används av hushållet |
| SoldElectricityPercentage | Procent av genererad el som säljs till nätet |
| LoanPeriodYears | Finansieringens längd i år |
| BankInterestRate | Årlig ränta på lån |
| AlternativeInvestmentRate | Avkastning för alternativa investeringar |
| DegradationRate | Årlig prestationsförlust i procent |
| MaintenanceCostPercentage | Årlig underhållskostnad som procent av systemkostnad |
| ElectricityPurchasePrice | Kostnad per kWh köpt från nätet i SEK |
| ElectricitySellingPrice | Ersättning per kWh såld till nätet i SEK |

## Beräkningsmetodik

Detta avsnitt beskriver de matematiska formler som används i kalkylatorn.

### Systemspecifikationer

#### Beräkning av märkeffekt

Systemets märkeffekt beräknas baserat på årlig förbrukning:

```
MärkEffekt (Wp) = ÅrligFörbrukning (kWh) × (SolcellsSystemProcent / 100)
```

#### Antal paneler

Antalet solcellspaneler beräknas baserat på märkeffekt:

```
AntalPaneler = golv(MärkEffekt (Wp) / PanelMärkEffekt (Wp))
```

#### Faktisk systemmärkeffekt

Den faktiska systemmärkeffekten beräknas baserat på antalet paneler:

```
TotalMärkEffekt (Wp) = AntalPaneler × PanelMärkEffekt (Wp)
```

#### Årlig energiproduktion

Den årliga energiproduktionen beräknas baserat på systemets märkeffekt:

```
ÅrligEnergiProduktion (kWh) = TotalMärkEffekt (Wp) × (EnergiBeräkningsProcent / 100)
```

#### Växelriktarstorlek

Den nödvändiga växelriktarstorleken beräknas baserat på systemets märkeffekt:

```
VäxelriktarStorlek (W) = TotalMärkEffekt (Wp) × (VäxelriktarEffektivitetsProcent / 100)
```

#### Antal växelriktare

Antalet växelriktare som behövs beräknas baserat på den nödvändiga växelriktarstorleken:

```
AntalVäxelriktare = max(1, tak(VäxelriktarStorlek (W) / StandardVäxelriktarStorlek (W)))
```

#### Nödvändig takyta

Den nödvändiga takytan beräknas baserat på antalet paneler:

```
NödvändigTakyta (m²) = AntalPaneler × PanelYta (m²)
```

### Energidistribution

#### Egenförbrukad el

Mängden el som förbrukas av hushållet:

```
EgenförbrukadEl (kWh) = ÅrligEnergiProduktion (kWh) × (EgenförbrukningsProcent / 100)
```

#### Såld el

Mängden el som säljs tillbaka till nätet:

```
SåldEl (kWh) = ÅrligEnergiProduktion (kWh) × (SåldElProcent / 100)
```

#### Köpt el

Mängden el som fortfarande behöver köpas från nätet:

```
KöptEl (kWh) = ÅrligFörbrukning (kWh) - EgenförbrukadEl (kWh)
```

### Kostnadsanalys

#### Panelkostnader

Den totala kostnaden för solcellspaneler:

```
PanelKostnad (SEK) = AntalPaneler × PanelPris (SEK)
```

#### Växelriktarkostnader

Den totala kostnaden för växelriktare:

```
VäxelriktarKostnad (SEK) = AntalVäxelriktare × VäxelriktarPris (SEK)
```

#### Total hårdvarukostnad

Den totala hårdvarukostnaden:

```
HårdvaruKostnad (SEK) = PanelKostnad (SEK) + VäxelriktarKostnad (SEK) + BatteriKostnad (SEK)
```

#### Installationskostnad

Kostnaden för installation:

```
InstallationsKostnad (SEK) = HårdvaruKostnad (SEK) × (InstallationsKostnadsProcent / 100)
```

#### Total systemkostnad före incitament

Den totala systemkostnaden före statliga incitament:

```
TotalKostnadFöreIncitament (SEK) = HårdvaruKostnad (SEK) + InstallationsKostnad (SEK)
```

#### Grönt teknikavdrag

Staten subventionsbelopp:

```
MaxAvdrag (SEK) = MaxAvdragPerPerson (SEK) × AntalPersonerFörAvdrag
PotentiellAvdrag (SEK) = TotalKostnadFöreIncitament (SEK) × (GröntTeknikAvdragProcent / 100)
GröntTeknikAvdrag (SEK) = min(PotentiellAvdrag (SEK), MaxAvdrag (SEK))
```

#### Total systemkostnad efter incitament

Den slutliga kostnaden efter statliga incitament:

```
TotalKostnadEfterIncitament (SEK) = TotalKostnadFöreIncitament (SEK) - GröntTeknikAvdrag (SEK)
```

### Finansiella nyckeltal

#### Årlig besparing från egenförbrukning

De årliga besparingarna från egenförbrukad el:

```
ÅrligBesparingEgenförbrukning (SEK) = EgenförbrukadEl (kWh) × ElköpsPris (SEK/kWh)
```

#### Årlig inkomst från såld el

Den årliga inkomsten från el såld till nätet:

```
ÅrligInkomstSåldEl (SEK) = SåldEl (kWh) × ElförsäljningsPris (SEK/kWh)
```

#### Total årlig vinst

Den totala årliga finansiella vinsten:

```
TotalÅrligVinst (SEK) = ÅrligBesparingEgenförbrukning (SEK) + ÅrligInkomstSåldEl (SEK)
```

#### Årlig underhållskostnad

De årliga underhållsutgifterna:

```
ÅrligUnderhållsKostnad (SEK) = TotalKostnadFöreIncitament (SEK) × (UnderhållsKostnadsProcent / 100)
```

#### Månatlig lånebetalning

Den månatliga lånebetalningen (med hjälp av annuitetsformel):

För ränta > 0:
```
MånatligLåneBetalning (SEK) = LåneBelopp (SEK) × (r(1+r)^n / ((1+r)^n - 1))
```

där:
- r är den månatliga räntesatsen (årlig ränta delat med 12)
- n är det totala antalet betalningar (lånetid i år multiplicerat med 12)

För ränta = 0:
```
MånatligLåneBetalning (SEK) = LåneBelopp (SEK) / n
```

#### Netto årligt kassaflöde

Det netto årliga kassaflödet:

```
NettoÅrligtKassaflöde (SEK) = TotalÅrligVinst (SEK) - ÅrligUnderhållsKostnad (SEK) - ÅrligLåneBetalning (SEK)
```

#### Enkel återbetalningstid

Den enkla återbetalningstiden i år:

```
EnkelÅterbetalningsTid = TotalKostnadEfterIncitament (SEK) / (TotalÅrligVinst (SEK) - ÅrligUnderhållsKostnad (SEK))
```

#### Återbetalningstid med alternativkostnad

Återbetalningstiden med hänsyn till alternativkostnad:

```
AlternativKostnad (SEK) = TotalKostnadEfterIncitament (SEK) × (AlternativInvesteringsRänta / 100)
ÅterbetalningsTidMedAlternativKostnad = TotalKostnadEfterIncitament (SEK) / (TotalÅrligVinst (SEK) - AlternativKostnad (SEK))
```

#### Total livstidsvinst

Den totala vinsten över systemets livstid:

```
ÅrligDegraderingsFaktor = 1 - (DegraderingsGrad / 100)
```

För varje år från 1 till SystemLivslängd:
```
ÅrligVinst (SEK) = TotalÅrligVinst (SEK) × ÅrligDegraderingsFaktor^(år-1)
TotalLivstidsVinst (SEK) = Summan av alla ÅrligVinst värden
```

```
TotalaLåneBetalningar (SEK) = ÅrligLåneBetalning (SEK) × LånePeriodÅr
TotaltUnderhåll (SEK) = ÅrligUnderhållsKostnad (SEK) × SystemLivslängd
TotalLivstidsVinst (SEK) = TotalLivstidsVinst (SEK) - TotalaLåneBetalningar (SEK) - TotaltUnderhåll (SEK)
```

### Investeringsjämförelse

#### Aktiemarknadsavkastning

Den förväntade avkastningen på aktiemarknaden:

```
AktiemarknadsAvkastning (SEK) = TotalKostnadEfterIncitament (SEK) × (1 + (AlternativInvesteringsRänta / 100))^SystemLivslängdÅr
```

#### Solcellssystemavkastning

Den förväntade avkastningen från solcellssystemet:

```
SolcellssystemAvkastning (SEK) = TotalKostnadEfterIncitament (SEK) + TotalLivstidsVinst (SEK)
```

#### Investeringsskillnad

Skillnaden mellan de två investeringsalternativen:

```
InvesteringsSkillnad (SEK) = |SolcellssystemAvkastning (SEK) - AktiemarknadsAvkastning (SEK)|
```

## Systemspecifikationer

Det här avsnittet beskriver systemspecifikationsberäkningarna och resultaten i detalj.

### Input

- **Årlig elförbrukning**: Hushållets årliga elanvändning i kWh.

### Output

- **Märkeffekt (Wp)**: Den teoretiska märkeffekten för solcellssystemet i Watt-peak.
- **Antal paneler**: Det totala antalet solcellspaneler som krävs.
- **Total märkeffekt (Wp)**: Den faktiska systemmärkeffekten baserad på antalet paneler.
- **Årlig energiproduktion (kWh)**: Den förväntade årliga energiproduktionen.
- **Växelriktarstorlek (W)**: Den nödvändiga växelriktarkapaciteten.
- **Antal växelriktare**: Antalet växelriktare som behövs.
- **Nödvändig takyta (m²)**: Takytan som behövs för solcellspanelerna.

## Energidistribution

Det här avsnittet beskriver energiflödesberäkningarna i detalj.

### Input

- **Egenförbrukningsprocent**: Procent av producerad el som förbrukas av hushållet.
- **Såld el procent**: Procent av producerad el som säljs till nätet.

### Output

- **Egenförbrukad el (kWh)**: Mängden el som produceras och förbrukas på plats.
- **Såld el (kWh)**: Mängden el som säljs tillbaka till nätet.
- **Köpt el (kWh)**: Mängden el som fortfarande behövs från nätet.

## Kostnadsanalys

Det här avsnittet beskriver kostnadsberäkningsdetaljerna.

### Input

- **Panelpris (SEK)**: Priset per solcellspanel.
- **Växelriktarpris (SEK)**: Priset per växelriktare.
- **Installationskostnadsprocent**: Installationskostnaden som procent av hårdvarukostnaden.
- **Grönt teknikavdragsprocent**: Den statliga subventionsprocenten.
- **Max avdrag per person (SEK)**: Maximalt subvention per person.
- **Antal personer för avdrag**: Antalet personer som är berättigade till avdraget.

### Output

- **Panelkostnad (SEK)**: Den totala kostnaden för solcellspaneler.
- **Växelriktarkostnad (SEK)**: Den totala kostnaden för växelriktare.
- **Installationskostnad (SEK)**: Kostnaden för installation.
- **Total kostnad före incitament (SEK)**: Den totala systemkostnaden före statliga incitament.
- **Grönt teknikavdrag (SEK)**: Det statliga subventionsbeloppet.
- **Total kostnad efter incitament (SEK)**: Den slutliga kostnaden efter statliga incitament.

## Finansiella nyckeltal

Det här avsnittet beskriver beräkningarna för finansiell avkastning i detalj.

### Input

- **Elköpspris (SEK/kWh)**: Kostnaden per kWh köpt från nätet.
- **Elförsäljningspris (SEK/kWh)**: Ersättningen per kWh såld till nätet.
- **Bankränta (%)**: Den årliga räntan på lånet.
- **Låneperiod (år)**: Finansieringens längd.
- **Underhållskostnadsprocent (%)**: Den årliga underhållskostnaden som procent av systemkostnaden.
- **Degraderingsgrad (%)**: Den årliga prestationsförlustprocenten.

### Output

- **Årlig besparing från egenförbrukning (SEK)**: De årliga besparingarna från egenförbrukad el.
- **Årlig inkomst från såld el (SEK)**: Den årliga inkomsten från el såld till nätet.
- **Total årlig vinst (SEK)**: Den totala årliga finansiella vinsten.
- **Årlig underhållskostnad (SEK)**: De årliga underhållsutgifterna.
- **Månatlig lånebetalning (SEK)**: Den månatliga låneavbetalningen.
- **Netto årligt kassaflöde (SEK)**: Det netto årliga kassaflödet.
- **Enkel återbetalningstid (år)**: Den enkla återbetalningstiden.
- **Återbetalningstid med lån (år)**: Återbetalningstiden med hänsyn till lånebetalningar.
- **Total livstidsvinst (SEK)**: Den totala vinsten över systemets livstid.

## Investeringsjämförelse

Det här avsnittet beskriver investeringsjämförelseberäkningarna i detalj.

### Input

- **Alternativ investeringsränta (%)**: Den förväntade årliga avkastningsräntan för alternativa investeringar.
- **Systemlivslängd (år)**: Den förväntade operativa livslängden för systemet.

### Output

- **Aktiemarknadsavkastning (SEK)**: Den förväntade avkastningen om samma belopp investerades på aktiemarknaden.
- **Solcellssystemavkastning (SEK)**: Den förväntade avkastningen från solcellssysteminvesteringen.
- **Investeringsskillnad (SEK)**: Den absoluta skillnaden mellan de två investeringsalternativen.
- **Bättre investering**: Indikerar vilken investering som presterar bättre.

## Implementationsdetaljer

Kalkylatorn är implementerad i C# med följande klassstruktur:

- **MarketParameters**: Innehåller aktuella marknadsförhållanden (kan laddas från en databas).
- **SolarSystemParameters**: Innehåller systemparametrar härledda från marknadsparametrar (kan anpassas).
- **SolarCalculationResults**: Innehåller beräkningsresultaten organiserade efter kategori.
- **SystemSpecifications**: Innehåller systemspecifikationsresultat.
- **EnergyDistribution**: Innehåller energidistributionsresultat.
- **CostBreakdown**: Innehåller kostnadsuppdelningsresultat.
- **FinancialMetrics**: Innehåller finansiella nyckeltalsresultat.
- **InvestmentComparison**: Innehåller investeringsjämförelseresultat.
- **SolarCalculator**: Huvudkalkylatorklassen som utför beräkningarna.

### Användningsexempel

```csharp
// Ladda marknadsparametrar (i en riktig app skulle detta komma från en databas)
var marknadsParams = await MarketParameters.LoadFromDatabaseAsync();

// Skapa kalkylator med användarens förbrukning
var kalkylator = new SolarCalculator(årligFörbrukning, marknadsParams);

// Beräkna resultat
var resultat = kalkylator.CalculateResults();

// Visa resultat
Console.WriteLine(resultat.GetFormattedSummary());
```

### Anpassningsexempel

```csharp
// Skapa anpassade parametrar baserade på standardvärden
var anpassadeParams = new SolarSystemParameters(årligFörbrukning, marknadsParams);

// Anpassa parametrar
anpassadeParams.SelfConsumedPercentage = 80;
anpassadeParams.ElectricityPurchasePrice = 2.1;
anpassadeParams.LoanPeriodYears = 15;

// Skapa kalkylator med anpassade parametrar
var anpassadKalkylator = new SolarCalculator(anpassadeParams, marknadsParams);

// Beräkna anpassade resultat
var anpassadeResultat = anpassadKalkylator.CalculateResults();
```
