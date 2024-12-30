# Schneider Electric Script Programming

📚 **Voorbeelden en documentatie voor Script Programming in Schneider Electric WorkStation**

Dit repository bevat praktische voorbeelden en uitgebreide documentatie voor het programmeren van scripts in Schneider Electric's EcoStruxure Building Operation WorkStation.

## 📁 Bestanden

### 📖 Documentatie
- **[Script_Editor_Reference_Guide.md](Script_Editor_Reference_Guide.md)** ⭐ **NIEUW!** - Complete referentie gids met:
  - Alle Program Components (Constants, Operators, Statements, Expressions, Variables)
  - Uitgebreide operator documentatie met precedence
  - Alle Program Statements (Action, Declaration, Program Control)
  - System Variables en System Functions
  - Platform-specifieke informatie
  - Quick Reference Card
  - Volledige syntax documentatie van begin tot eind

- **[Script_Editor_Syntax_Rules.txt](Script_Editor_Syntax_Rules.txt)** ⭐ **NIEUW!** - Syntax regels en validatie:
  - Alle syntax regels voor statements
  - Validatie checklists
  - Veelvoorkomende fouten en oplossingen
  - Platform-specifieke restricties
  - Best practices checklist

- **[Script_Programming_Documentatie.md](Script_Programming_Documentatie.md)** - Basis documentatie met uitleg over:
  - Basis structuur van Script Programming
  - Inputs, Outputs en Variabelen
  - Data types (BOOL, INT, REAL, STRING)
  - Operatoren en Control Structures
  - Best practices en veelvoorkomende fouten

### 💡 Voorbeelden
- **[Eenvoudig_Voorbeeld.txt](Eenvoudig_Voorbeeld.txt)** - Simpel beginnersvoorbeeld met stap-voor-stap uitleg
- **[Fan_Control_Script.txt](Fan_Control_Script.txt)** - Compleet voorbeeldscript met:
  - Meerdere inputs (temperatuursensor, schakelaars, knoppen)
  - Meerdere outputs (ventilator, statuslamp, alarm)
  - Automatische temperatuurregeling met hysterese
  - Handmatige bediening
  - Veiligheidscontroles

## 🚀 Quick Start

### Basis Structuur (Schneider Electric Script Editor Syntax)

```script
; 1. VARIABELE DECLARATIES
Numeric Input TemperatuurSensor
Numeric Output Ventilator
Numeric Drempelwaarde

; 2. INITIALISATIE
Drempelwaarde = 25.0

; 3. PROGRAMMA LOGICA
If TemperatuurSensor > Drempelwaarde then
    Set Ventilator to On
Else
    Set Ventilator to Off
Endif
```

### Belangrijkste Concepten

- **`Numeric Input`** - Input variabelen (signalen die binnenkomen, zoals sensoren)
- **`Numeric Output`** - Output variabelen (signalen die je verstuurt, zoals actuatoren)
- **`Numeric` / `String` / `Datetime`** - Lokale variabelen (waarden die je opslaat)
- **`If...Then...Else...Endif`** - Voorwaardelijke logica
- **`Set ... to On/Off`** - Output aan/uit zetten

**📚 Voor volledige syntax en alle mogelijkheden, zie [Script_Editor_Reference_Guide.md](Script_Editor_Reference_Guide.md)**

## 📋 Data Types

| Type | Beschrijving | Voorbeeld |
|------|--------------|-----------|
| **Numeric** | Floating-point nummer (IEEE 754 single precision) | `Numeric Temp = 25.5` |
| **String** | Tekst string (default 16 karakters, max 1MB) | `String Status = "Actief"` |
| **Datetime** | Datum en tijd | `Datetime StartTime = Date` |

**Variable Types (in Variables Pane):**
- **Float** - Floating-point (default voor Numeric)
- **Int** - Integer (geheel getal)
- **Bool** - Boolean (0/1, False/True)
- **String** - Character string
- **DateTime** - Date and time

## 🔧 Operatoren

### Rekenkundig
- `+` `PLUS` - Optellen
- `-` `MINUS` - Aftrekken  
- `*` `TIMES` - Vermenigvuldigen
- `/` `DIVIDED BY` - Delen
- `MOD` `REMAINDER` - Modulo (rest)
- `^` `^^` `EXP` - Machtsverheffen

### Vergelijking
- `=` `EQUALS` `IS EQUAL TO` - Gelijk aan
- `<>` `IS NOT EQUAL TO` - Niet gelijk aan
- `>` `IS GREATER THAN` - Groter dan
- `<` `IS LESS THAN` - Kleiner dan
- `>=` `IS GREATER THAN OR EQUAL TO` - Groter dan of gelijk aan
- `<=` `IS LESS THAN OR EQUAL TO` - Kleiner dan of gelijk aan
- `IS IN` `IS EITHER` - In lijst
- `IS NOT IN` `IS NEITHER` - Niet in lijst
- `IS BETWEEN` `IS THRU` - In bereik

### Logisch
- `AND` `&` - Logische EN
- `OR` `!` - Logische OF
- `NOT` - Logische NIET

**📚 Zie [Script_Editor_Reference_Guide.md](Script_Editor_Reference_Guide.md) voor volledige operator precedence en alle aliases**

## 📝 Voorbeeld: Fan Control

Het `Fan_Control_Script.txt` bestand bevat een compleet voorbeeld met:

✅ **Inputs:**
- Temperatuursensor
- Handmatige schakelaar
- Start/Stop knoppen

✅ **Outputs:**
- Ventilator
- Statuslamp
- Alarm

✅ **Features:**
- Automatische temperatuurregeling
- Hysterese om flikkeren te voorkomen
- Handmatige bediening
- Veiligheidscontroles (stop knop heeft voorrang)

## 📚 Gebruik in WorkStation

1. Open **EcoStruxure Building Operation WorkStation**
2. Navigeer naar de locatie waar je het script wilt toevoegen
3. Klik met rechts → **Nieuw** > **Programma** > **Script Programma**
4. Open de Script Editor
5. Kopieer en plak het gewenste script
6. Bind de inputs/outputs aan je fysieke apparaten
7. Sla op en activeer het script

## ⚠️ Best Practices

1. **Duidelijke namen gebruiken** - `TemperatuurSensor` in plaats van `T1`
2. **Commentaar toevoegen** - Maak je code begrijpelijk
3. **Veiligheid eerst** - Stop knoppen moeten altijd werken
4. **Hysterese gebruiken** - Voorkomt constant aan/uit schakelen
5. **Variabelen initialiseren** - Geef beginwaarden op

## 🔗 Bronnen

- [EcoStruxure Building Operation - WorkStation User Guide](https://www.se.com/us/en/download/document/51226669/)
- Schneider Electric Support Portal
- Lokale Schneider Electric vertegenwoordiger

## 📅 Laatste Update

**2024-12-30** - Complete referentie gids en syntax rules toegevoegd:
- ✅ Complete Script Editor Reference Guide met alle componenten
- ✅ Syntax Rules en Validatie documentatie
- ✅ Platform-specifieke informatie
- ✅ Quick Reference Card
- ✅ Best practices en veelvoorkomende fouten

**2024-12-30** - Eerste versie met voorbeelden en basis documentatie

## 📄 Licentie

Deze voorbeelden zijn bedoeld voor educatieve doeleinden. Gebruik op eigen risico.

---

💡 **Tips voor gebruik:**

1. **Beginners:** Start met `Eenvoudig_Voorbeeld.txt` en `Script_Programming_Documentatie.md`
2. **Geavanceerd:** Gebruik `Script_Editor_Reference_Guide.md` als complete referentie
3. **Syntax Check:** Raadpleeg `Script_Editor_Syntax_Rules.txt` voor validatie regels
4. **Quick Lookup:** Gebruik de Quick Reference Card in de Reference Guide

📖 **Voor complete syntax van begin tot eind, zie [Script_Editor_Reference_Guide.md](Script_Editor_Reference_Guide.md)**


