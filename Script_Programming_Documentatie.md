# Schneider Electric Script Programming - Documentatie

## Inhoudsopgave
1. [Basis Structuur](#basis-structuur)
2. [Inputs](#inputs)
3. [Outputs](#outputs)
4. [Variabelen](#variabelen)
5. [Data Types](#data-types)
6. [Operatoren](#operatoren)
7. [Control Structures](#control-structures)
8. [Best Practices](#best-practices)

---

## Basis Structuur

Een Script Programming bestand bestaat uit drie hoofdonderdelen:

```plaintext
; 1. INPUT DECLARATIES
IN NaamVanInput

; 2. OUTPUT DECLARATIES  
OUT NaamVanOutput

; 3. VARIABELEN (optioneel)
VAR NaamVanVariabele = Waarde

; 4. PROGRAMMA LOGICA
; Hier komt je code
```

**Opmerking:** Regels die beginnen met `;` zijn commentaar en worden genegeerd door de compiler.

---

## Inputs

**Inputs** zijn signalen die het script binnenkomen van sensoren, schakelaars, of andere apparaten.

### Syntax:
```plaintext
IN NaamVanInput
```

### Voorbeelden:
```plaintext
IN TemperatuurSensor      ; Numerieke waarde (REAL)
IN StartKnop              ; Boolean waarde (TRUE/FALSE)
IN Druksensor             ; Numerieke waarde
IN Noodstop               ; Boolean waarde
```

### Gebruik in programma:
```plaintext
IF StartKnop = TRUE THEN
    ; Doe iets
ENDIF

Temperatuur = TemperatuurSensor  ; Kopieer input naar variabele
```

---

## Outputs

**Outputs** zijn signalen die het script verstuurt naar actuatoren, lampen, of andere apparaten.

### Syntax:
```plaintext
OUT NaamVanOutput
```

### Voorbeelden:
```plaintext
OUT Ventilator            ; Boolean output (ON/OFF)
OUT StatusLamp            ; Boolean output (ON/OFF)
OUT Motor                 ; Boolean output
OUT Alarm                 ; Boolean output
```

### Gebruik in programma:
```plaintext
Ventilator = ON           ; Zet output aan
Ventilator = OFF          ; Zet output uit
StatusLamp = TRUE         ; Alternatieve syntax
StatusLamp = FALSE        ; Alternatieve syntax
```

---

## Variabelen

**Variabelen** worden gebruikt om waarden op te slaan en berekeningen uit te voeren.

### Syntax:
```plaintext
VAR NaamVanVariabele = Beginwaarde
```

### Voorbeelden:
```plaintext
VAR Drempelwaarde = 25.0         ; REAL (decimaal getal)
VAR Hysterese = 2.0               ; REAL
VAR VentilatorAan = FALSE         ; BOOL (Boolean)
VAR Teller = 0                    ; INT (geheel getal)
VAR Status = "Actief"            ; STRING (tekst)
```

### Variabelen gebruiken:
```plaintext
; Waarde toekennen
Temperatuur = TemperatuurSensor
VentilatorAan = TRUE

; Berekeningen
NieuweWaarde = OudeWaarde + 10
Gemiddelde = (Waarde1 + Waarde2) / 2
```

---

## Data Types

### BOOL (Boolean)
- Waarden: `TRUE`, `FALSE`, `ON`, `OFF`
- Gebruik: Schakelaars, status indicatoren

### INT (Integer)
- Waarden: Gehele getallen (bijv. -100, 0, 100)
- Gebruik: Tellers, gehele getallen

### REAL (Real/Decimal)
- Waarden: Decimale getallen (bijv. 25.5, -10.3)
- Gebruik: Temperatuur, druk, metingen

### STRING
- Waarden: Tekst (bijv. "Actief", "Fout")
- Gebruik: Status berichten, labels

---

## Operatoren

### Rekenkundige Operatoren
```plaintext
+   Optellen
-   Aftrekken
*   Vermenigvuldigen
/   Delen
```

### Vergelijkingsoperatoren
```plaintext
=   Gelijk aan
<>  Niet gelijk aan
>   Groter dan
<   Kleiner dan
>=  Groter dan of gelijk aan
<=  Kleiner dan of gelijk aan
```

### Logische Operatoren
```plaintext
AND     Logische EN
OR      Logische OF
NOT     Logische NIET
```

### Voorbeelden:
```plaintext
IF Temperatuur > 25.0 THEN
    ; Temperatuur is hoger dan 25
ENDIF

IF (StartKnop = TRUE) AND (StopKnop = FALSE) THEN
    ; Start knop is ingedrukt EN stop knop is niet ingedrukt
ENDIF

IF NOT Alarm THEN
    ; Alarm is NIET actief
ENDIF
```

---

## Control Structures

### IF-THEN-ELSE
```plaintext
IF Voorwaarde THEN
    ; Code als voorwaarde waar is
ELSE
    ; Code als voorwaarde niet waar is
ENDIF
```

### IF-THEN-ELSEIF
```plaintext
IF Voorwaarde1 THEN
    ; Code 1
ELSEIF Voorwaarde2 THEN
    ; Code 2
ELSE
    ; Code 3
ENDIF
```

### GOTO (Springen)
```plaintext
GOTO LabelNaam

LabelNaam:
; Code hier
```

**Let op:** Gebruik GOTO spaarzaam, alleen wanneer echt nodig.

---

## Best Practices

### 1. Duidelijke Namen
```plaintext
; GOED
IN TemperatuurSensor
OUT Ventilator

; SLECHT
IN T1
OUT O1
```

### 2. Commentaar Toevoegen
```plaintext
; Lees temperatuur van sensor
Temperatuur = TemperatuurSensor

; Controleer of temperatuur te hoog is
IF Temperatuur > Drempelwaarde THEN
    Ventilator = ON
ENDIF
```

### 3. Veiligheid Eerst
```plaintext
; Stop knop heeft altijd voorrang
IF StopKnop = TRUE THEN
    Ventilator = OFF
    GOTO Einde
ENDIF
```

### 4. Hysterese voor Stabiele Werking
```plaintext
; Voorkomt constant aan/uit schakelen
IF Temperatuur > (Drempelwaarde + Hysterese) THEN
    Ventilator = ON
ELSEIF Temperatuur < (Drempelwaarde - Hysterese) THEN
    Ventilator = OFF
ENDIF
```

### 5. Variabelen Initialiseren
```plaintext
VAR VentilatorAan = FALSE    ; Begin met uitgeschakeld
VAR Teller = 0                ; Begin met 0
```

### 6. Logische Structuur
```plaintext
; 1. Veiligheidscontroles eerst
; 2. Handmatige bediening
; 3. Automatische logica
; 4. Status updates
```

---

## Voorbeeld: Compleet Fan Control Script

Zie `Fan_Control_Script.txt` voor een volledig werkend voorbeeld met:
- Meerdere inputs (temperatuur, schakelaars, knoppen)
- Meerdere outputs (ventilator, lamp, alarm)
- Variabelen met drempelwaarden
- Hysterese logica
- Veiligheidscontroles
- Handmatige en automatische modus

---

## Veelvoorkomende Fouten

### 1. Vergeten ENDIF
```plaintext
; FOUT
IF Voorwaarde THEN
    Ventilator = ON

; GOED
IF Voorwaarde THEN
    Ventilator = ON
ENDIF
```

### 2. Verkeerde Data Type
```plaintext
; FOUT
VAR Drempelwaarde = TRUE    ; Moet REAL zijn, niet BOOL

; GOED
VAR Drempelwaarde = 25.0
```

### 3. Input/Output niet gedeclareerd
```plaintext
; FOUT - gebruikt zonder declaratie
Ventilator = ON

; GOED - eerst declareren
OUT Ventilator
Ventilator = ON
```

---

## Tips

1. **Test stap voor stap:** Begin met eenvoudige logica en bouw uit
2. **Gebruik commentaar:** Maak je code begrijpelijk voor anderen
3. **Documenteer inputs/outputs:** Noteer welke fysieke apparaten gekoppeld zijn
4. **Test edge cases:** Wat gebeurt er bij extreme waarden?
5. **Veiligheid:** Stop knoppen moeten altijd werken, ongeacht andere logica

---

## Extra Bronnen

- [EcoStruxure Building Operation - WorkStation User Guide](https://www.se.com/us/en/download/document/51226669/)
- Schneider Electric Support Portal
- Lokale Schneider Electric vertegenwoordiger

