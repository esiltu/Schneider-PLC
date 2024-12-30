# Schneider Electric Script Editor - Complete Referentie Gids

## Inhoudsopgave

1. [Inleiding](#inleiding)
2. [Program Componenten](#program-componenten)
3. [Programma Constants](#programma-constants)
4. [Operators](#operators)
5. [Program Statements](#program-statements)
6. [Expressions](#expressions)
7. [Variabelen](#variabelen)
8. [System Variables](#system-variables)
9. [Functions](#functions)
10. [Syntax Regels en Best Practices](#syntax-regels-en-best-practices)

---

## Inleiding

Script Editor is een krachtige programmeertaal voor Schneider Electric's EcoStruxure Building Operation systemen. Met Script Editor kun je geavanceerde logica en automatisering creëren voor gebouwbeheersystemen.

**Belangrijk:** Script gebruikt alleen floating-point nummers (IEEE 754 single precision). Alle numerieke waarden worden behandeld als decimale getallen, zelfs als ze geen decimaal punt hebben.

---

## Program Componenten

Script programma's bestaan uit de volgende componenten:

### 1. Program Constants
- Numerieke constants (bijv. `12`, `74.5`, `4E+32`)
- String constants (bijv. `"Hello world"`)
- Script constants (vooraf gedefinieerde waarden)

### 2. Program Operators
- Rekenkundige operators (`+`, `-`, `*`, `/`)
- Vergelijkingsoperators (`<`, `>`, `=`, `<>`)
- Logische operators (`AND`, `OR`, `NOT`)
- Bit operators (`BITAND`, `BITOR`, `BITXOR`)

### 3. Program Statements
- Action statements (`SET`, `TURN`, `PRINT`, `P`)
- Declaration statements (`NUMERIC`, `STRING`, `DATETIME`, `LINE`)
- Program Control statements (`IF...THEN...ELSE`, `FOR...NEXT`, `GOTO`, `WHILE`)

### 4. Expressions
- Numerieke expressies
- String expressies
- Datetime expressies

### 5. Variables
- Lokale program variabelen
- Input/Output variabelen
- Public variabelen
- System variabelen

---

## Programma Constants

### Numeric Constants

**Formaat:** Floating-point nummers (IEEE 754 single precision)

**Bereik:**
- Positieve nummers: `1.40129E-45` tot `3.402823E+38`
- Negatieve nummers: `-3.402823E+38` tot `-1.40129E-45`
- Resolutie: 23 bits (ongeveer 7 significante cijfers)

**Voorbeelden:**
```script
12          ; Geheel getal (wordt als float behandeld)
74.5        ; Decimaal getal
-.543       ; Negatief decimaal
4E+32       ; Wetenschappelijke notatie
```

**Belangrijke regels:**
- Gebruik GEEN komma in nummers (`1,000` is ongeldig)
- Nummers groter dan `3.402823E+38` worden geïnterpreteerd als oneindig
- Nummers kleiner dan `-3.402823E+38` worden geïnterpreteerd als negatief oneindig
- Nummers tussen `1.40129E-45` en `-.40129E-45` worden geïnterpreteerd als nul

### String Constants

**Formaat:** Tekst omringd door aanhalingstekens

**Voorbeelden:**
```script
"Hello world"
"Access to the building is restricted"
"123"                    ; String, niet een nummer!
```

**Speciale tekens in strings:**
- `|"` - Print een aanhalingsteken
- `|7` - Beep (ASCII code 7)
- `|12` - Form feed
- `|27` - ESCAPE
- `|numeric_constant` - ASCII code

**Voorbeeld:**
```script
Print "The string |" abc|" is included"
; Output: The string "abc" is included

Print "|7" to Printer1    ; Beep op printer
```

### Script Constants

**Vooraf gedefinieerde constanten (alphabetisch):**

**Status en Commands:**
- `ON`, `OFF`, `-ON` (tri-state)
- `Success`, `Failure`
- `True`, `False`
- `Active`, `Inactive`
- `Enabled`, `Disabled`
- `Online`, `Offline`
- `Running`, `Opened`, `Closed`
- `Overridden`, `OverRange`

**Maanden:**
- `Jan`, `January`, `Feb`, `February`, `Mar`, `March`, `Apr`, `April`
- `May`, `Jun`, `June`, `Jul`, `July`, `Aug`, `August`
- `Sep`, `September`, `Oct`, `October`, `Nov`, `November`, `Dec`, `December`

**Dagen van de week:**
- `Sun`, `Sunday`, `Mon`, `Monday`, `Tue`, `Tuesday`, `Wed`, `Wednesday`
- `Thu`, `Thursday`, `Fri`, `Friday`, `Sat`, `Saturday`

**Licht Commands:**
- `LightCommandNoCommand`, `LightCommandOn`, `LightCommandOff`
- `LightCommandUp`, `LightCommandDown`
- `LightCommandColorUp`, `LightCommandColorDown`, `LightCommandStop`

**Blind Commands:**
- `BlindCommandNoCommand`, `BlindCommandUp`, `BlindCommandDown`
- `BlindCommandStop`, `BlindCommandUpStep`, `BlindCommandDownStep`
- `BlindCommandResynchronize`

**Object gerelateerd:**
- `Object`, `ObjectClass`, `ObjectID`, `ObjectIdentifierAlreadyExists`
- `ObjectReference`, `ObjectDeletionNotPermitted`
- `NoVTSessionsAvailable`

**Andere:**
- `Binary`, `Bitstring`, `Constant`, `CurrentValue`
- `Average`, `Averaged`, `Singular`
- `ColorDown`, `ColorUp`, `CommandDown`
- `Days`, `Hours`, `Minutes`, `Months`, `Seconds`

**Belangrijk:** Script constants kunnen NIET worden gebruikt als line labels of program variabelen.

### Constant Keywords

**Failure**
- Numerieke waarde: `1`
- Gebruikt om aan te geven dat een functie is mislukt

```script
If MyFunction() = Failure then
    Print "Function failed"
Endif
```

**OFF**
- Numerieke waarde: `0` (of bottom of scale)
- Aanduiding dat een item OFF is

```script
If Lobby_Light is Off then
    Turn Off the Heat
Endif
```

**ON**
- Numerieke waarde: `1` (of top of scale)
- Aanduiding dat een item ON is

```script
If timeofday is between 9 and 17 then
    Set the Fan to ON
Else
    Set the Fan to OFF
Endif
```

**-ON**
- Numerieke waarde voor tri-state point
- Aanduiding dat een tri-state point op -ON staat

```script
If Flow is greater than SetPoint then
    Set Damper to -On
Endif
```

**Success**
- Numerieke waarde: `0`
- Aanduiding dat een proces of functie succesvol is voltooid

```script
If MyUpdateFunction("Zone1", "Temperature", TempValue) = Success then
    Print "Update Successful"
Else
    Goto RecordFailure
Endif
```

---

## Operators

### Operator Precedence (volgorde van bewerking)

Operators worden uitgevoerd in deze volgorde (van hoog naar laag):

1. `()` `[]` - Parentheses, Array Element (links naar rechts)
2. `\` of spatie - Path Name Connector (rechts naar links)
3. `+` `-` `NOT` `BITNOT` `%` - Unary operators (rechts naar links)
4. `^^` - Exponentiation (links naar rechts)
5. `*` `/` `MOD` - Vermenigvuldigen, Delen, Modulo (links naar rechts)
6. `+` `-` - Optellen, Aftrekken (links naar rechts)
7. `<` `<=` `>` `>=` - Vergelijkingen (links naar rechts)
8. `=` `<>` `IS IN` `IS NOT IN` `IS BETWEEN` `IS THRU` - Gelijkheidsoperators (links naar rechts)
9. `BITAND` `BITOR` `BITXOR` - Bit operators (links naar rechts)
10. `&` `!` - Logische AND/OR (links naar rechts)
11. `;` - String joining (links naar rechts)

### Rekenkundige Operators (Fundamental Operators)

| Operator | Alias | Beschrijving | Voorbeeld |
|----------|-------|--------------|-----------|
| `+` | `PLUS` | Optellen | `number + number` |
| `-` | `MINUS` | Aftrekken | `number - number` |
| `*` | `TIMES`, `MULT`, `MULTIPLIED BY` | Vermenigvuldigen | `number * number` |
| `/` | `DIVIDED BY`, `DIV` | Delen | `number / number` |
| `MOD` | `REMAINDER` | Modulo (rest) | `9 MOD 7` = `2` |
| `^` of `^^` | `EXP` | Machtsverheffen | `2 ^ 3` = `8` |

**Voorbeelden:**
```script
Usage = kwh / 24
Kwh_day = Kwh_PM - Kwh_AM
Extra = 5 MOD 2                    ; Result: 1
Energycost = Building1_KW * 1.78
```

### Vergelijkingsoperators (Comparative Operators)

| Operator | Alias | Beschrijving | Voorbeeld |
|----------|-------|--------------|-----------|
| `<` | `IS LESS THAN`, `IS BELOW` | Kleiner dan | `Temp < 70` |
| `<=` | `IS LESS THAN OR EQUAL TO` | Kleiner dan of gelijk | `Temp <= 70` |
| `>` | `IS GREATER THAN`, `IS ABOVE` | Groter dan | `Temp > 70` |
| `>=` | `IS GREATER THAN OR EQUAL TO` | Groter dan of gelijk | `Temp >= 70` |
| `=` | `EQUALS`, `IS EQUAL TO`, `IS EQUAL` | Gelijk aan | `Temp = 70` |
| `<>` | `DOES NOT EQUAL`, `IS NOT EQUAL TO`, `IS NOT EQUAL` | Niet gelijk | `Temp <> 70` |

**String vergelijkingen:**
Strings worden vergeleken op basis van ASCII codes:
- `"Z" > "A"` (90 > 65)
- `"9" > "0"`
- `"A" > "9"`
- `"ABC" > "A"`

**Voorbeelden:**
```script
If Temp is less than 70 then
    Turn On the Heat
Endif

If Zone_Temp is greater than 80 then
    Turn On the Cool
Endif

If Zone_Temp is equal to 72 then
    Print "Temperature is perfect"
Endif
```

### List en Range Operators

**IS IN / IS EITHER**
```script
If Zone_Temp is in Occupied, Warmup then
    ; Zone is in Occupied of Warmup state
Endif

If Zone_Temp is either Occupied, Warmup, or Reset then
    ; Zone is in één van deze states
Endif
```

**IS NOT IN / IS NEITHER**
```script
If Zone_Temp is not in Occupied, Warmup then
    ; Zone is NIET in Occupied of Warmup
Endif

If Zone_Temp is neither Occupied nor Warmup then
    ; Zone is in geen van beide
Endif
```

**IS THRU / IS BETWEEN**
```script
If Zone_Temp is 70 thru 80 then
    ; Temperature is tussen 70 en 80 (inclusief)
Endif

If Zone_Temp is between 70 and 80 then
    ; Zelfde als bovenstaande
Endif

If Zone_Temp is not between 70 and 80 then
    ; Temperature is NIET tussen 70 en 80
Endif
```

**Belangrijk:**
- `THRU` en `BETWEEN` zijn altijd inclusief (70 en 80 tellen mee)
- Lagere waarde moet eerst komen (`70 thru 80`, niet `80 thru 70`)
- Gebruik GEEN parentheses rond de range: `(70 thru 80)` is ongeldig

### Bit Operators

**BITAND** - Logische AND op bits
```script
Set result = Total1 bitand Total2
; Total1 = 13 (binary: 0000000000001101)
; Total2 = 11 (binary: 0000000000001011)
; Result = 9  (binary: 0000000000001001)
```

**BITOR** - Logische OR op bits
```script
Set result = Total1 bitor Total2
; Result = 15 (binary: 0000000000001111)
```

**BITXOR** - Exclusieve OR op bits
```script
Set result = Total1 bitxor Total2
; Result = 6  (binary: 0000000000000110)
```

**BITNOT** - Eéns complement
```script
Set result = bitnot AMOUNT
; AMOUNT = 13  (binary: 0000000000001101)
; Result = 65522 (binary: 1111111111110010)
```

**Belangrijk:** Bit operators werken alleen met integers 0-65535. Decimalen worden afgekapt.

**Notitie:** Bit operators zijn NIET geldig voor Infinet device Script programma's.

### Logische Operators

**AND** (alias: `&`)
```script
If Temp < 70 and TOD > 7:00am then
    Turn On the Heat
Endif

; Met ampersand:
If Temp < 70 & TOD > 7:00am then
    Turn On the Heat
Endif
```

**OR** (alias: `!`)
```script
If Heat is On or Cool is On then
    Print "HVAC is active"
Endif

; Met uitroepteken:
If Heat is On ! Cool is On then
    Print "HVAC is active"
Endif
```

**NOT**
```script
If not OCCUPIED then
    Turn Off the Lights
Endif
```

**Operator Precedence met AND/OR:**
- OR wordt vóór AND uitgevoerd als beide aanwezig zijn
- Gebruik parentheses om volgorde te controleren:
```script
; Zonder parentheses:
If Wkd = Sat or Wkd = Sun and TOD > 9:00 then
    ; Geïnterpreteerd als: (Wkd = Sat or Wkd = Sun) and TOD > 9:00

; Met parentheses voor controle:
If (TOD > 9:00) and (Wkd = Sat or Wkd = Sun) then
    ; Expliciete volgorde
Endif
```

### String Joining Operator

**`;` (semicolon)** - String concatenatie
```script
Print WeekDay; " "; Month; " "; DayOfMonth
; Output: Tuesday January 2
```

---

## Program Statements

### Action Statements

#### SET (alias: `ADJUST`, `CHANGE`, `LET`, `MODIFY`)

**Syntax 1:**
```script
Set namelist to number
Adjust Valve to Open
Change Space_SP to 72
Modify Space_SP to 72
```

**Syntax 2:**
```script
Set namelist = number
Let KwAvg = Average(KW)
KwAvg = Average(KW)        ; Zonder SET/LET
```

**Meerdere variabelen:**
```script
Set KwAvg, OATAvg, HWAvg, CHWAvg = 0
Set D1, D2 = On
Set D1, D2, D3, D4 = On
```

**String variabelen:**
```script
Set Stat_Message to "Heat active"
```

#### TURN

**Syntax:**
```script
Turn On point_list
Turn Off point_list
Turn point_list On
Turn point_list OFF
```

**Voorbeelden:**
```script
Turn on the Fan and the Pump
Turn the Pump off
Turn off the Pump
```

**Belangrijk:** Gebruik `ON`/`OFF` keywords voor Infinet device Script programma's. `Inactive`/`Active` en `True`/`False` werken niet voor digital outputs in Infinet devices.

#### PRINT

**Format 1: Print naar string point/variable**
```script
Print "HEAT_SETPT" to PointName
```

**Format 2: Print string**
```script
Print "WARNING - Trouble on the 4th Floor!"
```

**Format 3: Print lijst**
```script
Print TEMP1, TEMP2, TEMP3, TEMP4
; Output: 67 72 77 75
```

**Format 4: Print met format string**
```script
Print "The temperature is |###.# at |##:##:##", ~Temp4, Hour, Minute, Second
; Output: The Temperature is 75.0 at 4:32:05
```

**Formatting karakters:**

| Karakter | Beschrijving | Voorbeeld |
|----------|--------------|-----------|
| `|"` | Print aanhalingsteken | `"|" abc|""` → `"abc"` |
| `|7` | Beep (ASCII 7) | `Print "|7"` |
| `|12` | Form feed | `Print "|12"` |
| `|###` | Numeriek, rechts uitgelijnd | `"|#######", 1340` → `   1340` |
| `$###` | ON/-ON/OFF voor tristate | `"|$###", FanStatus` → `ON` |
| `%` | Percentage (×100 + %) | `"|%###", 0.45` → `45%` |
| `*` | Elke lengte string | `"| *", FullName` |
| `@` | Alphanumeriek, links uitgelijnd | `"|@@@@@@@@", Name` |
| `,` | Duizendtallen scheiding | `"|##,###", 1340` → `1,340` |
| `}` | Verwijder trailing zeros | `"|##.##}", 21.70` → `21.7` |
| `>` | Rechts uitlijnen | `"|>####", 88.8` → `  89` |
| `<` | Links uitlijnen | `"|<####", 88.8` → `88.8` |
| `-` | Leading/trailing min/plus | `"|-##", -88.8` → `-89` |
| `^^` | Wetenschappelijke notatie | `"|###^##", 97000` → `9.7e+04` |

**Semicolon voor zelfde regel:**
```script
Print WeekDay;          ; Geen nieuwe regel
Print " ";
Print Month;            ; Zelfde regel als vorige
Print " ";
Print DayOfMonth
```

#### P (alias: `PR`)

Print waarden van variabelen/points, elk op een aparte regel:
```script
P SupplyAir, ReturnAir, BurnerStat, CoilStatus
; Output:
; SupplyAir = 46 degrees F
; ReturnAir = 68 degrees F
; BurnerStat = on
; CoilStatus = off
```

### Declaration Statements

#### NUMERIC (alias: `NUMBER`)

```script
Numeric Avg_Temp
Numeric Ave_OAT, AVE_Temp, Fan_SP
Numeric Fan_SP[20]                    ; Array met 20 elementen
Numeric Fan_SP[20], Pump_SP, AHU_SP[10], Heat_SP
```

**Met binding:**
```script
Numeric Input Temp_DegF              ; Input variable
Numeric Output Thermostat_Temp_DegF  ; Output variable
Numeric Public ReceivedValue         ; Public variable
```

#### STRING

```script
String VarName                        ; Default lengte: 16 karakters
String 20 LNAME, LOGON, PWORD        ; Lengte: 20 karakters
String 8 Pump[20]                    ; Array: 20 strings van 8 karakters
String Pump[6], Blower[8], HeaterNM, FanName
```

**Belangrijk:**
- Default lengte: 16 karakters
- Maximum lengte (EcoStruxure BMS servers): 1,048,576 karakters (1 MB)
- Maximum lengte (b3 BACnet devices): 32 karakters
- Vermijd grote strings vanwege performance impact

**Met binding:**
```script
String Input S1
String Output outstr1
```

#### DATETIME

```script
Datetime TempTime
Datetime TempTime, FirstTime, LastTime
Datetime TempTime[40], Timer[30], Watch[15]
Datetime TempTime[40], FirstTime, Timer[30], LastTime
```

**Gebruik:**
```script
Datetime TempTime
TempTime = Date                      ; Huidige datum/tijd
Print TempTime                       ; Format: MONTH DD YYYY hh:mm:ss
```

**Met binding:**
```script
Datetime Input DT1
Datetime Output outdt1
```

#### LINE

Label een regel voor `GOTO`:
```script
Line Startup
Line 1
Line Shutdown

; Of met colon:
Startup:
1:
Shutdown:
```

**Reserved line labels:**
- `LINE 0` - Predefined voor stoppen van programma (gebruik `GOTO line 0`)
- `LINE C` - Gereserveerd voor toekomstig gebruik
- `LINE E` - Automatisch uitgevoerd bij errors

**Voorbeeld met error handling:**
```script
Line OpenPort3
Result = Open(Comm3)
Line TestingOpen
If result = success then goto PrintMenus
...
Line E
Result = Close(Comm3)
Print "EMERGENCY EXIT - MENUPROGRAM FAILED."
```

#### ARG (alias: `PARAM`)

Gebruikt in functies om argumenten te declareren:
```script
ARG 5 RptStatus                      ; Declare argument 5 met naam RptStatus
If RptStatus is Success then...      ; Gebruik naam
If ARG[5] is Success then...         ; Of gebruik nummer
```

**ARG oproepen:**
```script
ARG[integer_expression]              ; Oproep met nummer
```

#### INPUT / OUTPUT / PUBLIC

**INPUT:**
```script
Numeric Input Temp_DegF
String Input S1
Datetime Input DT1
```

- Read-only variabele
- Neemt waarde over van bound object property
- Kan niet worden gezet in programma

**OUTPUT:**
```script
Numeric Output Thermostat_Temp_DegF
String Output outstr1
```

- Write-only (kunnen worden gezet, zetten bound property)
- Wanneer variabele wordt gezet, wordt bound property ook gezet

**PUBLIC:**
```script
Numeric Public ReceivedValue
```

- Zelfde als OUTPUT, maar kan worden aangepast vanuit buiten het programma
- Behoudt waarde na power failure, warm start, cold start, reboot
- **Belangrijk:** Nieuwe PUBLIC variabelen moeten NA bestaande PUBLIC keywords worden geplaatst, anders verliezen ze waarde na save

#### FUNCTION

```script
Function MyFunction                  ; Declare functie variabele
Result = MyFunction(Arg1, Arg2)     ; Roep functie aan
```

#### WEBSERVICE

```script
Webservice wsCalculator              ; Declare Web Service
```

**Notitie:** Alleen ondersteund in automation server Script programma's, NIET in functies, event programma's, of MP/RP controllers.

### Program Control Statements

#### IF...THEN...ELSE

**Syntax 1: Single line**
```script
If expression then statement
If TOD >= 1200 then Goto Noon
```

**Syntax 2: Multi-line zonder ELSE**
```script
If expression then
    statement
    statement
    ...
Endif

If Wkd = Mon and TOD > 800 and TOD < 1600 then
    Run the HeaterProg
    Run the FanCheckProg
    Stop the PumpProg
Endif
```

**Syntax 3: Single line met ELSE**
```script
If expression then statement else statement
If TOD > 800 & TOD < 1700 then Run DayPrg else Run NiteProg
```

**Syntax 4: Multi-line met ELSE**
```script
If expression then
    statement
    statement
    ...
Else
    statement
    statement
    ...
Endif

If Temp < 72 and Pump.Stat is off then
    Turn on the Fan
    Close the Damper
Else
    Turn off the Fan
    Open the Damper
Endif
```

**Geneste IF statements:**
```script
If the Wkd is Either Saturday OR Sunday then
    Set Occupancy to Off
    Stop the DailyProgram
    If the Temp is Greater Than 70 then
        Turn On the Fan
        Open the Damper
    Else
        Turn OFF the Fan
        Open the Damper
    Endif
Else
    Set Occupancy to On
    Start the DailyProgram
Endif
```

**Belangrijk:**
- `IF` en `THEN` moeten op dezelfde regel staan
- `ELSE` moet op een eigen regel staan (bij multi-line)
- Elke `IF` moet een `ENDIF` hebben
- Non-zero waarden worden geïnterpreteerd als `TRUE`

#### FOR...NEXT

```script
For numeric_name = begin to end
    statement
    statement
    ...
Next numeric_name

For numeric_name = begin to end step number
    statement
    statement
    ...
Next numeric_name
```

**Voorbeelden:**
```script
For count = 2 to 10 step 2
    Print count, " ", (10 + count)
Next count
; Output: 2 12, 4 14, 6 16, 8 18, 10 20

For count = 1 to 15
    Set ARG[count] = 0
Next count

For count = 3 to 15 step 3
    Set Pump[count] = 0
Next count

; Negatieve step:
For count = 10 to 1 step -1
    Print count
Next count
```

#### WHILE

```script
While number
    statement
    statement
    ...
Endwhile

Numeric Counter
Set Counter to 1
While Counter <= 10
    ManualArray[Counter] = Counter * 10
    Counter = Counter + 1
Endwhile
```

#### REPEAT...UNTIL

```script
Repeat
    statement
    statement
    ...
Until number

Numeric Count
Count = 1
Repeat
    Print OutsideAir[Count]
    Count = Count + 1
Until Count = OutsideAir_Size
```

**Belangrijk:** Statements worden altijd minstens één keer uitgevoerd voordat de conditie wordt geëvalueerd.

#### GOTO

```script
Goto linename
Goto Line linename
Go linename
Go To linename
Go To Line linename
```

**Voorbeelden:**
```script
Line Beginning
If Temp is less than 68 then Goto Heating
If Temp is greater than 76 then Goto Cooling
Goto Beginning

Line Heating
Turn on Heater1
Goto Beginning

Line Cooling
Turn on Cool1
Goto Beginning
```

#### BASEDON...GOTO

```script
Basedon (number) Goto linelist
Basedon (number) Go to linelist
Basedon (number) Goto Line linelist

Basedon Wkd Goto Sun_1, Mon_1, Tue_1, Wed_1, Thu_1, Fri_1, Sat_1
; Sunday (1) gaat naar Sun_1, Monday (2) naar Mon_1, etc.
```

#### SELECT CASE

```script
Select Case test_expression
    Case expression_list
        statement_list
    Case expression_list
        statement_list
    ...
    Case Else
        else_statement_list
EndSelect

Select Case Weekday
    Case Monday, Tuesday
        Print "Run Mon_Tue Report"
    Case Wednesday thru Friday
        Print "Run Wed_Thu_Fri Report"
    Case Else
        Print "Run Weekend Report"
EndSelect

Select Case ReportId
    Case 1
        Print "Run First Report"
    Case 2, 3, 5 thru 10, 15
        Print "Run Special Report"
    Case 20
        Print "Run Final Report"
    Case Else
        Print "Invalid Report Id"
EndSelect
```

**Belangrijk:**
- Lege CASE statements zijn toegestaan
- `Case Else` is optioneel maar aanbevolen
- Expressions kunnen numeric, string, of datetime zijn

#### BREAK

Stopt de kleinste omringende loop:
```script
For count = 1 to 20
    If Temp[count] > 75 then
        Break                       ; Exit FOR loop
    Endif
    Print Temp[count]
Next count
```

#### CONTINUE

Slaat huidige iteratie over en gaat naar volgende:
```script
For count = 1 to 20
    If Temp[count] <= 75 then
        Continue                   ; Sla deze over, ga naar volgende
    Endif
    Print "Temperature over 75: ", Temp[count]
Next count
```

#### RETURN

In functies, retourneert waarde:
```script
Return                              ; Return zonder waarde (returns 0)
Return number                       ; Return met waarde

ARG 1 radius
Return (3.14159 * (radius ^ 2))    ; Return berekende waarde
```

#### WAIT (alias: `Delay`)

Wacht aantal seconden (alleen EcoStruxure BMS servers):
```script
Wait 5                              ; Wacht 5 seconden
Wait (3)                            ; Wacht 3 seconden
Delay 10                            ; Wacht 10 seconden
```

**Restricties:**
- Alleen in automation server Script programma's
- NIET in MP/RP controllers
- NIET in Script functies
- NIET in Script event programma's
- NIET binnen For loops

#### STOP (alias: `CLOSE`, `SHUT`)

Stopt equipment of zet point naar bottom of scale:
```script
Stop point_list
If TOD > 2000 then stop the HeatProg
Stop                              ; Stop huidige programma
```

#### MOVE (alias: `MODULATE`)

Converteert engineering units naar electrical units:
```script
MOVE output_point_list TO number
MOVE output_point_list TO number %

MOVE VALVE2 TO 45                  ; 45 degrees (engineering) → 10 mA (electrical)
MODULATE VALVE2 TO 50%             ; 50% = 0.5 = 10 mA
```

**Ondersteund:** CX series, BACnet series, CMX series, SCX series, TCX series, LCX series, ACX series, DCX 250, EcoStruxure BMS servers

---

## Expressions

### Numeric Expressions

Expressions die altijd een nummer teruggeven:
```script
2                                   ; Constant
SQRT(9)                             ; Function result
900 / 8                             ; Calculation
WKD > Monday                        ; Comparison (returns 0 or 1)
```

### String Expressions

Expressions die altijd een string teruggeven:
```script
"WARNING- HIGH TEMPERATURE"        ; Constant
LEFT("TEST", 1)                     ; Function result
"The number is "; TOTAL1            ; Concatenation
```

---

## Variabelen

### Lokale Program Variabelen

**Declaratie:**
```script
Type variable_name
Type variable_name[array_size]
Type Binding_qualifier variable_name
```

**Voorbeelden:**
```script
Numeric A
Datetime D
String S
Numeric Input A1
Numeric Output B1
Numeric Public C1
Numeric Fan_SP[20], Temp[20]
```

### Arrays

```script
Numeric Temperature[50]             ; Array van 50 elementen
Temperature[1] = 68.5               ; Eerste element
Temperature[50] = 72.0              ; Laatste element

For count = 1 to 50
    Print Temperature[count]
Next count
```

**Belangrijk:**
- Arrays beginnen bij index 1 (niet 0)
- Maximum array size: 32,767 elementen
- Array index kan een variabele zijn: `Array[IndexVar]`

### Variable Types in Variables Pane

| Type | Beschrijving | Gebruik |
|------|--------------|---------|
| **Float** | IEEE 754 single precision floating point | Voor analog values, floating point properties |
| **Int** | Whole numbers | Voor integer properties, multistate values |
| **Bool** | 0 of 1 (False/True) | Voor boolean properties, digital values |
| **String** | Character string | Voor string values |
| **DateTime** | Date and time | Voor datetime properties |

**Voorbeeld variabele resolutie:**
```script
Numeric Variable1                    ; Float (default)
Variable1 = 55.7

Numeric Variable2                    ; Int
Variable2 = Variable1                ; Becomes 56 (rounded)

Numeric Variable3                    ; Float
Variable3 = Variable2                ; Becomes 56.0 (lost resolution)

Numeric Variable4                    ; Bool
Variable4 = Variable1                ; Becomes True (non-zero)

Numeric Variable5                    ; Float
Variable5 = Variable4                ; Becomes 1.0
```

---

## System Variables

### Date and Time System Variables

| Variabele | Alias | Beschrijving | Bereik/Format |
|-----------|-------|--------------|---------------|
| `DATE` | `TIME` | Huidige systeem datum en tijd | Datetime |
| `DAYOFMONTH` | `DOM` | Dag van de maand | 1-31 |
| `DAYOFYEAR` | `DOY` | Dag van het jaar | 1-366 |
| `HOD` | `HOUROFDAY` | Tijd in decimale vorm | 0.0-23.99 |
| `HOUR` | `HR` | Huidig uur | 0-23 |
| `MINUTE` | `MIN` | Huidige minuut | 0-59 |
| `MONTH` | `MTH` | Huidige maand | Jan-Dec (of 1-12) |
| `SECOND` | `SEC` | Huidige seconde | 0-59 |
| `TOD` | `TIMEOFDAY` | Tijd van de dag | 0-2359 (HHMM) |
| `UTCOffset` | - | UTC offset in seconden | Seconden |
| `WEEKDAY` | `WKD` | Dag van de week | Sun-Sat (of 1-7) |
| `YEAR` | `YR` | Huidig jaar | 4-cijferig (bijv. 2024) |
| `DST` | - | Daylight savings time offset | Seconden |

**Voorbeelden:**
```script
Datetime Temp_Date
Temp_Date = Date                    ; Huidige datum/tijd
Print Temp_Date                     ; Output: MONTH DD YYYY hh:mm:ss

If Hour is equal to 5 then
    Goto Startup
Endif

If Weekday is Saturday then
    Print Weekday                   ; Output: Saturday
Endif

If TOD is greater than 500 then     ; Na 5:00 AM
    Print "Morning routine"
Endif

If Month is December then
    Print "Holiday season"
Endif

If DayofMonth is between 7 and 14 then
    Print "Mid-month period"
Endif
```

**MONTH waarden:**
```script
Month = 1        ; of JAN, January
Month = 2        ; of FEB, February
...
Month = 12       ; of DEC, December
```

**WEEKDAY waarden:**
```script
Weekday = 1      ; of SUN, Sunday
Weekday = 2      ; of MON, Monday
...
Weekday = 7      ; of SAT, Saturday
```

**Belangrijk:** Script gebruikt Sunday als eerste dag van de week (1), terwijl Function Block Monday gebruikt (1).

### Runtime System Variables

| Variabele | Beschrijving |
|-----------|--------------|
| `ERRORS` | Aantal pending system errors |
| `FREEMEM` | Aantal bytes vrije memory in grootste block |
| `SCAN` | `SC` | Lengte van laatste interpreter scan in seconden |
| `IsBound(variable)` | Controleert of variabele is bound (returns True/False) |

**Voorbeelden:**
```script
If Errors > 10 then
    Goto Report_Error
Endif

Print Freemem                        ; Print vrije memory

Numeric Tot_Scan_SCS, Scan_Count, Scan_Avg
Set Tot_Scan_SCS, Scan_Count, Scan_Avg = 0
Line Totaling
Tot_Scan_SCS = Tot_Scan_SCS + Scan
Scan_Count = Scan_Count + 1
If TOD = 2359 then
    Scan_Avg = Tot_Scan_SCS / Scan_Count
    Print "The average scan for", WKD " is", Scan_Avg, "sec"
    Set Scan_Avg, Scan_Count, Tot_Scan_SCS = 0
Endif

If IsBound(AV_1) then
    Message1 = "AV_1 IS BOUND"
Else
    Message1 = "AV_1 IS NOT BOUND"
Endif
```

### Runtime Variables (Program-level)

| Variabele | Beschrijving |
|-----------|--------------|
| `TS` | Time in Seconds - tijd sinds programma op huidige regel |
| `TM` | Time in Minutes - tijd sinds programma op huidige regel |
| `TH` | Time in Hours - tijd sinds programma op huidige regel |
| `TD` | Time in Days - tijd sinds programma op huidige regel |

**Voorbeelden:**
```script
Line OpenValve
Turn On Valve1
If TS >= 90 then                    ; 90 seconden verstreken
    Turn Off Valve1
    Goto NextStep
Endif
Goto OpenValve

Line StartFan
Turn On Fan1
If TM >= 5 then                     ; 5 minuten verstreken
    Turn On Pump1
Endif
```

---

## Functions

### Script Functions

**Declaratie en gebruik:**
```script
Function MyFunction                  ; Declare in programma
Result = MyFunction(Arg1, Arg2)     ; Roep aan
```

**Functie bestand:**
```script
ARG 1 radius                         ; Declare argument 1
ARG 2 height                         ; Declare argument 2
Numeric Volume
Volume = 3.14159 * (radius ^ 2) * height
Return Volume                        ; Return waarde
```

**Functie zonder argumenten:**
```script
; Functie bestand:
Turn off Heat
Turn off Fan
Run Pump, Cooling
Return

; Roep aan:
Function Fanstop
If TOD > 1800 and TOD < 800 then
    FanStop()
Endif
```

**Functie met argumenten:**
```script
; Functie bestand:
ARG 1 Program1
ARG 2 Program2
Start Program1
Start Program2
Return

; Roep aan:
Function StartPrograms
StartPrograms("ProgramA", "ProgramB")
```

**Maximum 15 argumenten ondersteund.**

### System Functions

#### Mathematical Functions

| Functie | Beschrijving | Voorbeeld |
|---------|--------------|-----------|
| `ABS(number)` | Absolute waarde | `ABS(-3)` → `3` |
| `EXPONENTIAL(number)` | e tot de macht | `EXP(0)` → `1` |
| `FACTORIAL(int)` | Faculteit | `FACT(3)` → `6` (max 34) |
| `LN(number)` | Natuurlijke logaritme | `LN(3.2)` |
| `LOG(number)` | Logaritme base 10 | `LOG(10)` → `1` |
| `RANDOM(number)` | Random 0-32767 | `RANDOM(8)` |
| `SQRT(number)` | Vierkantswortel | `SQRT(4)` → `2` |
| `SUM(list/array/log)` | Som | `SUM(1,2,3)` → `6` |

#### Rounding Functions

| Functie | Beschrijving | Voorbeeld |
|---------|--------------|-----------|
| `CEILING(number)` | Rondt altijd omhoog | `CEILING(4.3)` → `5`, `CEILING(-2.7)` → `-2` |
| `FLOOR(number)` | Rondt altijd omlaag | `FLOOR(4.7)` → `4`, `FLOOR(-3.8)` → `-4` |
| `ROUND(number)` | Rondt naar dichtstbijzijnde | `ROUND(4.3)` → `4`, `ROUND(4.7)` → `5` |
| `TRUNCATE(number)` | Verwijdert decimaal deel | `TRUNCATE(4.7)` → `4`, `TRUNCATE(-3.8)` → `-3` |

#### Statistical Functions

| Functie | Beschrijving | Voorbeeld |
|---------|--------------|-----------|
| `AVERAGE(list/array/log)` | Gemiddelde | `AVG(70,72,74)` → `72` |
| `MAXIMUM(list/array/log)` | Maximum waarde | `MAX(70,72,74)` → `74` |
| `MINIMUM(list/array/log)` | Minimum waarde | `MIN(70,72,74)` → `70` |
| `MAXITEM(list/array/log)` | Index van maximum | `MAXITEM(60,65,70,67)` → `3` |
| `MINITEM(list/array/log)` | Index van minimum | `MINITEM(60,65,70,67)` → `1` |
| `StandardDeviation(list/array/log)` | Standaarddeviatie | `SD(70,72,74)` → `2` |

**Voorbeelden:**
```script
Flr8_AVG = Average(TMP801, TMP802, TMP803, TMP804)
OAT_AVG = Average(OAT)                      ; Array
HourlyAVG = Average(TEMPLOG)                ; Log

TOPNUMBER = Maximum(Zone1, Zone2, Zone3, Zone4)
BotNumber = Minimum(Zone1, Zone2, Zone3, Zone4)

MaxIndex = Maxitem(Temp1, Temp2, Temp3, Temp4)
MinIndex = Minitem(Temp1, Temp2, Temp3, Temp4)

TempDev = StandardDeviation(70, 72, 74)     ; Result: 2
```

#### String Functions

| Functie | Beschrijving | Voorbeeld |
|---------|--------------|-----------|
| `ASC(string)` | ASCII waarde van eerste karakter | `ASC("S")` → `83` |
| `CHR(number)` | Karakter van ASCII code | `CHR(65)` → `"A"` |
| `LEFT(string, int)` | Linker deel string | `LEFT("ABCDE", 2)` → `"AB"` |
| `LENGTH(string)` | Lengte van string | `LENGTH("ABCDE")` → `5` |
| `MID(string, offset, number)` | Middelste deel | `MID("ABCDE", 2, 3)` → `"BCD"` |
| `RIGHT(string, int)` | Rechter deel string | `RIGHT("ABCDE", 3)` → `"CDE"` |
| `SEARCH(string, search_string)` | Positie van substring (case sensitive) | `SEARCH("ABCDE", "BC")` → `2` |
| `FIND(string, search_string, [0/1])` | Positie (0=case sensitive, 1=insensitive) | `FIND("ABCDE", "bc", 1)` → `2` |
| `STRINGFILL(number, charcode)` | String gevuld met karakter | `STRINGFILL(60, 42)` → `60 asterisks` |
| `TAB(number)` | String met aantal spaties | `TAB(10)` → `10 spaces` |

**Voorbeelden:**
```script
LVAL = Left("ABCDEF", 2)                    ; "AB"
RGTVAL = Right("ABCDE", 3)                  ; "CDE"
MIDSTR = MID("ABCDE", 2, 3)                 ; "BCD"
Result = Length("ABCDE")                    ; 5

NSTR = Search("ABCDE", "BC")                ; 2
NSTR = Find("ABCDE", "bc", 1)               ; 2 (case insensitive)
NSTR = Find("ABCDE", "bc", 0)               ; 0 (case sensitive, niet gevonden)

Print CHR(12); "Weekly Report"              ; Form feed + title
Print Stringfill(60, 42)                    ; 60 asterisks
Print Tab(10); "Title"                      ; 10 spaces + "Title"
```

#### Conversion Functions

| Functie | Beschrijving | Voorbeeld |
|---------|--------------|-----------|
| `NUMTOSTR(number)` | Converteer nummer naar string | `NUMTOSTR(123.5)` → `"123.5"` |
| `STRTONUM(string)` | Converteer string naar nummer | `STRTONUM("78.5")` → `78.5` |
| `STRTODATE(date_time)` | Converteer string naar datetime | `STRTODATE("SEPTEMBER-21-2010 11:00 pm")` |

**NUMTOSTR limieten:**
- Nummers < 1,000,000: decimale notatie
- Nummers ≥ 1,000,000: exponentiële notatie
- 8 cijfers resolutie

**Voorbeelden:**
```script
Trans = StrToNum("78.5") + 92.8            ; 171.3
Result = NumToStr(123.5)                   ; "123.5"

CONV_DATE = StrToDate("SEPTEMBER-21-2010 11:00 pm")
```

#### Time Functions

| Functie | Beschrijving |
|---------|--------------|
| `DIFFTIME(SECOND/MINUTE/HOUR/WKD, date1, date2)` | Verschil tussen twee datums |
| `TIMEPIECE(sys_var, datetime)` | Haal tijd/datum component op |
| `GetDST(datetime)` | Daylight savings time offset |

**Voorbeelden:**
```script
Datetime Timer[2]
Result = Difftime(SECOND, Timer[1], Timer[2])      ; Verschil in seconden
Result = Difftime(MINUTE, Timer[1], Timer[2])      ; Verschil in minuten
Result = Difftime(HOUR, Timer[1], Timer[2])        ; Verschil in uren
Result = Difftime(WKD, OLDTIME, DATE)              ; Verschil in dagen

CurrentMin = Timepiece(Minute, Date1)              ; Minuut van Date1
TodayMonth = Timepiece(Month, Date)                ; Maand van vandaag

dtOutUTC1 = dtNow - UTCOffset - GetDST(dtOutUTC1)
```

#### Trigonometric Functions

| Functie | Beschrijving | Opmerking |
|---------|--------------|-----------|
| `ACOS(number)` | Arccosine | Input: -1 tot 1, Output: 0 tot π |
| `ASIN(number)` | Arcsinus | Input: -1 tot 1, Output: -π/2 tot π/2 |
| `ATAN(number)` | Arctangens | Output: -π/2 tot π/2 |
| `ATAN2(sin, cos)` | Arctangens met 2 args | Output: -π tot π |
| `COS(number)` | Cosinus | Input in radialen: -65536 tot 65536 |
| `SIN(number)` | Sinus | Input in radialen: -65536 tot 65536 |
| `TAN(number)` | Tangens | Input in radialen |

**Radialen ↔ Graden:**
- Radialen = graden × (π/180)
- Graden = radialen × (180/π)
- π ≈ 3.14159

**Voorbeelden:**
```script
Result = COS(3.14159/2)                           ; Cosinus van 90 graden
Result = SIN(1)                                   ; Sinus van 1 radiaal
Position = TAN(3.14159/180)                       ; Tangens van 1 graad
```

#### Object Functions

| Functie | Beschrijving |
|---------|--------------|
| `ReadProperty(object_property)` | Lees BACnet object property |
| `WriteProperty(object_property, value, [priority], [index])` | Schrijf BACnet object property |
| `Relinquish(object_property, [priority])` | Geef command vrij |

**Voorbeelden:**
```script
Numeric Input AV1
Numeric Temp
Temp = ReadProperty(AV1)

Numeric Output AV1
WriteProperty(AV1, 100)                          ; Priority 16 (default)
WriteProperty(AV1, 100, 5)                       ; Priority 5
WriteProperty(AV1, , 5)                          ; Relinquish priority 5

Relinquish(AV1, 5)                               ; Relinquish priority 5
```

**Belangrijk voor EcoStruxure BMS servers:**
- `WriteProperty` schrijft altijd naar priority 16, tenzij direct bound naar specifieke priority
- Voor specifieke priority: bind output variabele direct naar die priority

#### Dynamic Array Functions

| Functie | Beschrijving |
|---------|--------------|
| `GetArraySize(ArrayName)` | Haal huidige array size op |
| `SetArraySize(ArrayName, Number)` | Zet array size |

**Voorbeelden:**
```script
Numeric myArray[10]
ArraySize = GetArraySize(myArray)                ; Returns 10
SetArraySize(myArray, 60)                        ; Resize to 60
SetArraySize(myArray, DayofMonth)                ; Resize to day of month
```

#### Buffered Variable Functions

| Functie | Beschrijving |
|---------|--------------|
| `GetBufferSize(variable_name)` | Aantal buffered waarden |
| `GetBufferedValue(variable_name)` | Haal volgende waarde uit buffer (FIFO) |

**Alleen in EcoStruxure BMS server Script programma's:**
```script
Numeric Buffered Input BIn1
Numeric Buffered Input BIn2
Numeric Output Out1

Out1 = BIn1 + BIn2                               ; Gebruik huidige waarde

While GetBufferSize(BIn2) > 0
    If GetBufferedValue(BIn2) = Success then
        Out2 = Out2 + BIn2                       ; Tel alle buffered waarden
    Endif
EndWhile
```

#### Event Program Functions

| Functie | Beschrijving |
|---------|--------------|
| `GetTickCount()` | Milliseconden sinds system start (max 16777215, ~4.66 uur) |
| `GetElapsedTime(tick_count)` | Verstreken tijd sinds tick_count |
| `StartTimer(variable)` | Start timer met variabele waarde (ms) |
| `StartTimer(variable, milliseconds)` | Start timer met specifieke waarde |
| `StopTimer(timer_interval)` | Stop timer |
| `GetTriggeredVariableName()` | Naam van variabele die programma triggerde |
| `GetTriggeredVariableId()` | ID van variabele die programma triggerde |

**Alleen in Script event programma's:**
```script
Numeric Triggered Input input1
Numeric TickCount, ElapsedTime
Numeric TimerVariable

TickCount = GetTickCount()
ElapsedTime = GetElapsedTime(TickCount)

TimerVariable = 1000                              ; 1 seconde
StartTimer(TimerVariable)

If Second mod 30 = 0 then
    StopTimer(TimerVariable)
Endif

TriggeredVar = GetTriggeredVariableName()
TriggeredId = GetTriggeredVariableId()
```

#### Other Functions

| Functie | Beschrijving |
|---------|--------------|
| `PASSED(arg_number)` | Controleert of argument is doorgegeven (in functies) |

**Voorbeeld:**
```script
Function MaxItem
Numeric Count, Lastmax
Lastmax = 1
For Count = 1 TO 15
    If not(Passed(Count)) then return (Lastmax)
    If ARG[Count] > Lastmax then Lastmax = Count
Next Count
```

---

## Syntax Regels en Best Practices

### Basis Syntax Regels

1. **Case Sensitivity:**
   - Line labels zijn case-insensitive (`STARTUP` = `Startup` = `startup`)
   - Variabelen zijn case-sensitive
   - Keywords zijn case-insensitive

2. **Comments:**
   ```script
   ; Single line comment
   ' Alternative comment (single quote)
   ```

3. **Line Continuation:**
   - Statements kunnen meerdere regels beslaan
   - `IF` en `THEN` moeten opzelfde regel
   - `ELSE` moet op eigen regel (bij multi-line)

4. **Whitespace:**
   - Spaties en tabs worden genegeerd (behalve in strings)
   - Meerdere statements per regel zijn mogelijk met `;`

5. **Naming Conventions:**
   - Variabele namen moeten beginnen met letter
   - Kan letters, cijfers, underscores, periods bevatten
   - Kan geen keywords zijn
   - Maximaal 32 karakters (aanbevolen)

### Best Practices

1. **Declareer alle variabelen bovenaan:**
   ```script
   ; Variables
   Numeric TempAvg
   String StatusMsg
   Datetime StartTime
   
   ; Program logic
   ...
   ```

2. **Gebruik duidelijke namen:**
   ```script
   ; GOED:
   Numeric Input RoomTemperature
   Numeric Output FanSpeed
   
   ; SLECHT:
   Numeric Input T1
   Numeric Output O1
   ```

3. **Voeg commentaar toe:**
   ```script
   ; Check if temperature exceeds threshold
   If RoomTemperature > 75 then
       ; Turn on fan at maximum speed
       Set FanSpeed to 100
   Endif
   ```

4. **Veiligheid eerst:**
   ```script
   ; Safety check - always stop if emergency button pressed
   If EmergencyStop = On then
       Stop Fan1
       Stop Pump1
       Goto Shutdown
   Endif
   ```

5. **Gebruik hysterese voor stabiele werking:**
   ```script
   Numeric Hysteresis = 2.0
   If Temperature > (SetPoint + Hysteresis) then
       Turn On Cooling
   ElseIf Temperature < (SetPoint - Hysteresis) then
       Turn Off Cooling
   Endif
   ```

6. **Initialiseer variabelen:**
   ```script
   Numeric Counter = 0
   String Status = "Initializing"
   ```

7. **Gebruik parentheses voor duidelijkheid:**
   ```script
   ; Onduidelijk:
   Result = A + B * C / D
   
   ; Duidelijk:
   Result = A + (B * C / D)
   ```

8. **Error handling:**
   ```script
   Line E
   Print "ERROR: Program failed at line ", CurrentLine
   Result = Close(CommPort)
   Stop
   ```

9. **Avoid GOTO wanneer mogelijk:**
   ```script
   ; Beter: gebruik IF-THEN-ELSE of SELECT CASE
   ; Alleen GOTO voor loops of error handling
   ```

10. **Test edge cases:**
    - Test met extreme waarden
    - Test met NULL/leeg
    - Test boundary conditions

### Veelvoorkomende Fouten

1. **Vergeten ENDIF:**
   ```script
   ; FOUT:
   If Condition then
       Statement
   
   ; GOED:
   If Condition then
       Statement
   Endif
   ```

2. **Verkeerde operator precedence:**
   ```script
   ; FOUT (mogelijk onverwacht resultaat):
   Result = A + B * C
   
   ; GOED (expliciet):
   Result = A + (B * C)
   ```

3. **Input/Output niet gedeclareerd:**
   ```script
   ; FOUT:
   Temp = InputValue
   
   ; GOED:
   Numeric Input InputValue
   Numeric Temp
   Temp = InputValue
   ```

4. **String vs Numeric:**
   ```script
   ; FOUT:
   String Value = 123              ; Moet "123" zijn
   
   ; GOED:
   String Value = "123"
   Numeric Value = 123
   ```

5. **Array index out of bounds:**
   ```script
   Numeric Array[10]
   ; FOUT:
   Value = Array[11]               ; Index 11 bestaat niet
   
   ; GOED:
   If Index >= 1 and Index <= 10 then
       Value = Array[Index]
   Endif
   ```

6. **PUBLIC variabelen volgorde:**
   ```script
   ; FOUT (verliest waarde na save):
   Numeric Public NewVar
   Numeric Public OldVar
   
   ; GOED (behoudt waarde):
   Numeric Public OldVar
   Numeric Public NewVar           ; Nieuwe na bestaande
   ```

### Performance Tips

1. **Vermijd grote strings:**
   - Max 1 MB is toegestaan, maar vermijd > 1000 karakters
   - Monitor CPU usage bij grote strings

2. **Gebruik arrays efficiënt:**
   - Declareer arrays met juiste size
   - Gebruik `GetArraySize()` en `SetArraySize()` voor dynamische arrays

3. **Minimaliseer loops:**
   - Vermijd nested loops waar mogelijk
   - Gebruik `BREAK` en `CONTINUE` voor efficiëntie

4. **Cache vaak gebruikte waarden:**
   ```script
   Numeric CurrentTemp
   CurrentTemp = RoomTemperature   ; Cache voor meerdere vergelijkingen
   If CurrentTemp > 75 then...
   If CurrentTemp < 60 then...
   ```

---

## Platform Specifieke Informatie

### EcoStruxure BMS Servers
- Volledige Script Editor functionaliteit
- WebService ondersteuning
- Buffered inputs
- WAIT keyword
- Dynamic arrays
- Event programs

### MP en RP Controllers
- Script programma's en functies
- Event programs
- Lighting/Blind control keywords
- GEEN WebService
- GEEN WAIT in loops
- GEEN buffered inputs in functies/event programs

### b3 BACnet Controllers
- Beperkte functionaliteit
- Max string lengte: 32 karakters
- GEEN buffered inputs
- GEEN WebService
- GEEN event programs

### Infinet Controllers
- Gebruik `ON`/`OFF` voor digital outputs (niet `Active`/`Inactive`)
- GEEN bit operators
- Beperkte functies

---

## Appendices

### ASCII Codes (Belangrijkste)

| Code | Karakter | Beschrijving |
|------|----------|--------------|
| 7 | BEL | Beep |
| 8 | BS | Backspace |
| 9 | TAB | Tab |
| 10 | LF | Line feed |
| 12 | FF | Form feed |
| 13 | CR | Carriage return |
| 27 | ESC | Escape |
| 42 | * | Asterisk |
| 65-90 | A-Z | Hoofdletters |
| 97-122 | a-z | Kleine letters |
| 127 | DEL | Delete |

### Script Constants Overzicht

**Status:** `On`, `Off`, `-On`, `Success`, `Failure`, `True`, `False`, `Active`, `Inactive`, `Enabled`, `Disabled`, `Online`, `Offline`, `Running`, `Opened`, `Closed`, `Overridden`, `OverRange`

**Maanden:** `Jan`, `January`, `Feb`, `February`, `Mar`, `March`, `Apr`, `April`, `May`, `Jun`, `June`, `Jul`, `July`, `Aug`, `August`, `Sep`, `September`, `Oct`, `October`, `Nov`, `November`, `Dec`, `December`

**Weekdays:** `Sun`, `Sunday`, `Mon`, `Monday`, `Tue`, `Tuesday`, `Wed`, `Wednesday`, `Thu`, `Thursday`, `Fri`, `Friday`, `Sat`, `Saturday`

**Light Commands:** `LightCommandNoCommand`, `LightCommandOn`, `LightCommandOff`, `LightCommandUp`, `LightCommandDown`, `LightCommandColorUp`, `LightCommandColorDown`, `LightCommandStop`

**Blind Commands:** `BlindCommandNoCommand`, `BlindCommandUp`, `BlindCommandDown`, `BlindCommandStop`, `BlindCommandUpStep`, `BlindCommandDownStep`, `BlindCommandResynchronize`

---

## Quick Reference Card

### Declaraties
```script
Numeric [Input/Output/Public] VarName [ArraySize]
String [Input/Output/Public] [Length] VarName [ArraySize]
Datetime [Input/Output/Public] VarName [ArraySize]
Function FuncName
Webservice ServiceName
Line LabelName
ARG number ArgName
```

### Control Flow
```script
If condition then [statement | statements... Endif] [else statement | statements... Endif]
For var = start to end [step increment] ... Next var
While condition ... Endwhile
Repeat ... Until condition
Select Case expr ... Case ... EndSelect
Goto label
Break
Continue
Return [value]
```

### Operators
```script
+ - * / MOD ^^          ; Rekenkundig
< <= > >= = <>          ; Vergelijking
AND OR NOT              ; Logisch
BITAND BITOR BITXOR BITNOT  ; Bitwise
IS IN, IS NOT IN        ; List
IS BETWEEN, IS THRU     ; Range
;                       ; String join
```

### Functies (Selectie)
```script
ABS, SQRT, LN, LOG, EXP, RANDOM, SUM
AVERAGE, MAXIMUM, MINIMUM, MAXITEM, MINITEM, StandardDeviation
CEILING, FLOOR, ROUND, TRUNCATE
LEFT, RIGHT, MID, LENGTH, SEARCH, FIND, STRINGFILL, TAB
NUMTOSTR, STRTONUM, STRTODATE
DIFFTIME, TIMEPIECE, GetDST
COS, SIN, TAN, ACOS, ASIN, ATAN, ATAN2
ReadProperty, WriteProperty, Relinquish
GetArraySize, SetArraySize
GetBufferSize, GetBufferedValue
GetTickCount, GetElapsedTime, StartTimer, StopTimer
```

---

**Document Versie:** 1.0  
**Laatste Update:** 2024  
**Gebaseerd op:** Schneider Electric Script Programming Documentation (04-50006-01-en, December 2022)

