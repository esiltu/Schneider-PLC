# Schneider Electric Script Programming

📚 **Voorbeelden en documentatie voor Script Programming in Schneider Electric WorkStation**

Dit repository bevat praktische voorbeelden en uitgebreide documentatie voor het programmeren van scripts in Schneider Electric's EcoStruxure Building Operation WorkStation.

## 📁 Bestanden

### 📖 Documentatie
- **[Script_Programming_Documentatie.md](Script_Programming_Documentatie.md)** - Complete documentatie met uitleg over:
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

### Basis Structuur

```plaintext
; 1. INPUT DECLARATIES
IN TemperatuurSensor
IN StartKnop

; 2. OUTPUT DECLARATIES
OUT Ventilator

; 3. VARIABELEN
VAR Drempelwaarde = 25.0

; 4. PROGRAMMA LOGICA
IF TemperatuurSensor > Drempelwaarde THEN
    Ventilator = ON
ELSE
    Ventilator = OFF
ENDIF
```

### Belangrijkste Concepten

- **`IN`** - Inputs (signalen die binnenkomen, zoals sensoren)
- **`OUT`** - Outputs (signalen die je verstuurt, zoals actuatoren)
- **`VAR`** - Variabelen (waarden die je opslaat)
- **`IF-THEN-ELSE`** - Voorwaardelijke logica
- **`= ON/OFF`** - Output aan/uit zetten

## 📋 Data Types

| Type | Beschrijving | Voorbeeld |
|------|--------------|-----------|
| **BOOL** | Boolean (TRUE/FALSE, ON/OFF) | `VAR Status = TRUE` |
| **INT** | Geheel getal | `VAR Teller = 100` |
| **REAL** | Decimaal getal | `VAR Temperatuur = 25.5` |
| **STRING** | Tekst | `VAR Status = "Actief"` |

## 🔧 Operatoren

### Rekenkundig
- `+` Optellen
- `-` Aftrekken
- `*` Vermenigvuldigen
- `/` Delen

### Vergelijking
- `=` Gelijk aan
- `<>` Niet gelijk aan
- `>` Groter dan
- `<` Kleiner dan
- `>=` Groter dan of gelijk aan
- `<=` Kleiner dan of gelijk aan

### Logisch
- `AND` Logische EN
- `OR` Logische OF
- `NOT` Logische NIET

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

**2025-12-30** - Eerste versie met voorbeelden en documentatie

## 📄 Licentie

Deze voorbeelden zijn bedoeld voor educatieve doeleinden. Gebruik op eigen risico.

---

💡 **Tip:** Begin met `Eenvoudig_Voorbeeld.txt` en gebruik de documentatie als naslagwerk!

