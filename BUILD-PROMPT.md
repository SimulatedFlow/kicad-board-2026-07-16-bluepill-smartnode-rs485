```json
{
  "board_name": "BluePill-SmartNode-RS485",
  "one_liner": "Handlötbares Träger- und Automationsboard für die STM32 Bluepill mit RS485-Bus, 2 robusten Relais-Ausgängen und Sensor-Schnittstellen.",
  "market_gap": "Bastler verdrahten Bluepills für Haus- und Gartensteuerung meist fliegend über instabile Jumper-Kabel mit externen Modulen. Dieses Board vereint alle Kernfunktionen (Schalten, Bus, Sensorik, Stromversorgung) auf einer rein handlötbaren, störsicheren THT-Platine in Eurocard-Größe.",
  "confidence": "high",
  "price_eur": 24,
  "target_enclosure": "100x80mm Eurocard-Gehäuse / DIN-Hutschienengehäuse (z. B. CamdenBoss CNMB/4)",
  "injection_notes": "keine"
}
```

## BUILD-PROMPT

### 1. Spezifikation & Harte DFM-Vorgaben
- **Zielgruppe & Montage:** Reine Handlötbarkeit (THT-Fokus). KEINE SMD-Bauteile kleiner als 0805, bevorzugt werden DIP-Sockel, TO-220, Standard-Axialwiderstände und Standard-THT-Kondensatoren/Dioden.
- **Leiterbahnen & Abstände (Clearance):** 
  - Standard-Signal-Clearance: $\ge 0.3\,\text{mm}$
  - Standard-Signal-Leiterbahnbreite: $0.4\,\text{mm}$
  - Lastpfade (Relaiskontakte bis Schraubklemmen): $\ge 1.5\,\text{mm}$ Leiterbahnbreite (4-lagig (F.Cu/B.Cu Signal, In1.Cu GND-Plane, In2.Cu Power-Plane) verstärkt).
- **Bluepill-Sockel:** 2× 20-Pin-Buchsenleiste mit Standard-Rastermaß (RM 2.54 mm) und einem exakten Reihenabstand von **0.9″ (22.86 mm)**.
- **Anschlüsse:** Alle Schraubklemmen im RM 5.08 mm ausgeführt und direkt an den Platinenrändern platziert (Kabelauslass nach außen).
- **Massefläche:** 4-Lagen-Design (F.Cu/B.Cu Signal, In1.Cu durchgehende GND-Plane, In2.Cu durchgehende Versorgungs-Plane) mit einer durchgehenden GND-Kupferfläche auf dem Bottom-Layer (`B.Cu`).

---

### 2. Mechanik (Abmessungen & Bohrungen)
- **Board-Größe:** Exakt $100.0\,\text{mm} \times 80.0\,\text{mm}$ (Ecken abgerundet).
- **Befestigungslöcher:** 4× M3-Befestigungsbohrungen (Durchmesser $3.2\,\text{mm}$) an den exakten Koordinaten (Ursprung unten links bei (0,0)):
  - Loch 1: $(5.0\,\text{mm}, 5.0\,\text{mm})$
  - Loch 2: $(95.0\,\text{mm}, 5.0\,\text{mm})$
  - Loch 3: $(5.0\,\text{mm}, 75.0\,\text{mm})$
  - Loch 4: $(95.0\,\text{mm}, 75.0\,\text{mm})$
- **Keepout:** $5.0\,\text{mm}$ kreisförmiger Sperrbereich (Keepout) um alle vier Bohrlöcher für Schraubenköpfe/Unterlegscheiben.

---

### 3. Bestückungs- & Bauteilliste (BOM)

#### Stromversorgung (9-24V DC Input auf 5V und 3.3V):
- **J_POWER:** 2-polige Schraubklemme (RM 5.08 mm) – *Footprint: TerminalBlock_Phoenix:TerminalBlock_Phoenix_MKDS-1.5-2-5.08_1x02_P5.08mm_Horizontal*
- **D_REVERSE:** 1N4007 Verpolschutzdiode (THT DO-41) – *Footprint: Diode_THT:D_DO-41_SOD81_P10.16mm_Horizontal*
- **C_BULK1:** 100 µF / 25 V Elektrolytkondensator (THT) – *Footprint: Capacitor_THT:CP_Radial_D8.0mm_P3.50mm*
- **U_REG5V:** L7805CV linearer Spannungsregler (TO-220, stehend, liefert 5V für Relais/RS485) – *Footprint: Package_TO_SOT_THT:TO-220-3_Vertical*
- **U_REG3V3:** LD1117V33 oder LF33CV linearer Spannungsregler (TO-220, stehend, liefert stabile 3.3V für Bluepill/Sensoren) – *Footprint: Package_TO_SOT_THT:TO-220-3_Vertical*
- **C_BULK2, C_BULK3:** 2× 47 µF / 10 V Elkos (THT) – *Footprint: Capacitor_THT:CP_Radial_D5.0mm_P2.00mm*
- **C_DEC1, C_DEC2, C_DEC3:** 3× 100 nF Keramikkondensatoren (Stützkondensatoren, THT) – *Footprint: Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm*

#### Mikrocontroller & Logik:
- **U1, U2 (Bluepill-Sockel):** 2× PinSocket 1x20 (RM 2.54 mm, Reihenabstand 22.86 mm) – *Footprint: Connector_PinSocket:PinSocket_1x20_P2.54mm_Vertical*

#### RS485-Bus (MAX485):
- **U_MAX485:** MAX485 Transceiver im DIP-8-Gehäuse (gesockelt) – *Footprint: Package_DIP:DIP-8_W7.62mm*
- **C_DEC4:** 100 nF Stützkondensator für MAX485 – *Footprint: Capacitor_THT:C_Disc_D5.0mm_W2.5mm_P5.00mm*
- **R_TERM:** Optionale 120 Ohm Abschlusswiderstand (THT, zuschaltbar über Lötbrücke oder direkt bestückt) – *Footprint: Resistor_THT:R_Axial_DIN0207_L6.3mm_D2.5mm_P10.16mm_Horizontal*
- **J_RS485:** 3-polige Schraubklemme (RM 5.08 mm, Anschlüsse: A, B, GND) – *Footprint: TerminalBlock_Phoenix:TerminalBlock_Phoenix_MKDS-1.5-3-5.08_1x03_P5.08mm_Horizontal*

#### Relais-Ausgänge (2-Kanal):
- **U_ULN2003:** ULN2003A Darlington-Transistor-Array im DIP-16-Gehäuse (gesockelt; nur 2 der 7 Kanäle genutzt, integrierte Freilaufdioden/Basiswiderstände vereinfachen das Layout) – *Footprint: Package_DIP:DIP-16_W7.62mm*
- **K1, K2:** 2× Printrelais 5VDC (Typ Songle SRD-05VDC-SL-C) – *Footprint: Relay_THT:Relay_SPDT_Sanyou_SRD_Series_Form_C*
- **LED1, LED2:** 2× LED 5 mm (Statusanzeige rot/grün) – *Footprint: LED_THT:LED_D5.0mm*
- **R1, R2:** 2× 330 Ohm Vorwiderstände für LEDs – *Footprint: Resistor_THT:R_Axial_DIN0207_L6.3mm_D2.5mm_P10.16mm_Horizontal*
- **J_REL1, J_REL2:** 2× 3-polige Schraubklemmen (RM 5.08 mm, Belegung: COM, NO, NC) – *Footprint: TerminalBlock_Phoenix:TerminalBlock_Phoenix_MKDS-1.5-3-5.08_1x03_P5.08mm_Horizontal*

#### Sensor-Schnittstellen:
- **J_I2C:** 4-Pin Stiftleiste für OLED-Display (GND, 3.3V, SDA, SCL) – *Footprint: Connector_PinHeader_2.54mm:PinHeader_1x04_P2.54mm_Vertical*
- **J_DS18B20:** 3-Pin Stiftleiste für Dallas One-Wire Sensoren (GND, DQ, 3.3V) – *Footprint: Connector_PinHeader_2.54mm:PinHeader_1x03_P2.54mm_Vertical*
- **R_PULL1:** 4.7k Ohm Pull-up-Widerstand (THT) für DS18B20 – *Footprint: Resistor_THT:R_Axial_DIN0207_L6.3mm_D2.5mm_P10.16mm_Horizontal*

---

### 4. Verdrahtungsplan (Netz-Labels)

#### Strom & Masse:
- `GND`: Zentraler Massebus. Verbindet J_POWER Pin 2, alle Elko-/Kerko-GNDs, Regler-GNDs, Bluepill GND, ULN2003 Pin 8, MAX485 Pin 5, Sensor-GNDs und J_RS485 Pin 3.
- `12V_IN`: Ausgang von D_REVERSE, führt zu C_BULK1, C_DEC1 und U_REG5V Input.
- `5V`: Ausgang von U_REG5V, führt zu C_BULK2, C_DEC2, U_REG3V3 Input, MAX485 Pin 8, ULN2003 Pin 9 (COM) und den Spulen der Relais K1-K2. Führt auch an den 5V-Pin der Bluepill (Einspeisung).
- `3.3V`: Ausgang von U_REG3V3, führt zu C_BULK3, C_DEC3, dem Pull-up-Widerstand R_PULL1 und den Sensor-Headern J_I2C, J_DS18B20.

#### Mikrocontroller-Steuerleitungen:
- **Relais-Kanäle (2):**
  - `PA0` $\rightarrow$ U_ULN2003 Pin 1 (IN1) $\rightarrow$ Pin 16 (OUT1) $\rightarrow$ Spule K1 (und LED1 mit R1)
  - `PA1` $\rightarrow$ U_ULN2003 Pin 2 (IN2) $\rightarrow$ Pin 15 (OUT2) $\rightarrow$ Spule K2 (und LED2 mit R2)
- **RS485-Interface:**
  - `PA2` (USART2_TX) $\rightarrow$ U_MAX485 Pin 4 (DI)
  - `PA3` (USART2_RX) $\rightarrow$ U_MAX485 Pin 1 (RO)
  - `PA4` (TX_EN) $\rightarrow$ Verbunden mit U_MAX485 Pin 2 (~RE) und Pin 3 (DE) zur Richtungssteuerung.
- **I2C-Sensor-Bus:**
  - `PB6` (I2C1_SCL) $\rightarrow$ J_I2C Pin 4 (SCL)
  - `PB7` (I2C1_SDA) $\rightarrow$ J_I2C Pin 3 (SDA)
- **Sensor-Eingänge:**
  - `PB0` $\rightarrow$ J_DS18B20 Pin 2 (DQ)

---

### 5. Arbeitsanweisung für den Bau-Agenten
1. **Projekt-Setup:** Erstelle ein neues KiCad-Projekt mit dem Namen `BluePill-SmartNode-RS485`.
2. **Schaltplan erstellen:** Platziere alle oben genannten Symbole. Weise jedem Symbol den exakt spezifizierten Footprint zu. Verbinde alle Netze sauber mit globalen Labels.
3. **Leiterplatten-Layout (PCB):**
   - Zeichne den Board-Umriss (`Edge.Cuts`) auf ein exaktes Rechteck von $100.0\,\text{mm} \times 80.0\,\text{mm}$.
   - Setze die 4× M3-Befestigungslöcher auf die definierten Positionen und sperre die Keepout-Zonen.
   - Platziere den Bluepill-Sockel mittig.
   - Ordne alle Schraubklemmen (J_POWER, J_RS485, J_REL1 - J_REL4) entlang der Ränder an, so dass die Kabeleinführung nach außen zeigt.
   - Platziere die Entkopplungskondensatoren so nah wie möglich an den jeweiligen IC-Power-Pins (insb. MAX485 und Regler).
   - Platziere die Status-LEDs gut sichtbar neben den jeweiligen Relais.
4. **Routing (4 Lagen, dedizierte Planes):**
   - Setze das Board auf 4 Kupferlagen: F.Cu (Signal) / In1.Cu (durchgehende GND-Plane) / In2.Cu (durchgehende Versorgungs-Plane, Netz `5V`) / B.Cu (Signal). Lege GND-Zone auf In1.Cu und 5V-Zone auf In2.Cu über die ganze Fläche an.
   - Route Signalleitungen AUSSCHLIESSLICH auf F.Cu und B.Cu; GND und 5V kommen über die Planes + Vias (dadurch viel weniger Routing-Dichte). Keine Signale auf In1/In2.
   - Nutze für Relais-Lastpfade eine Leiterbahnbreite von mindestens $1.5\,\text{mm}$.
5. **Prüfung (DRC):** Führe den Design Rule Check (DRC) durch. Korrigiere alle Clearance- und Überlappungsfehler.
6. **Export:** Exportiere die Gerber- und Excellon-Bohrdaten strukturiert in das Verzeichnis `./gerbers`.

*Zusammenfassung:* Bitte führe das Design autonom durch und erstelle am Ende einen ehrlichen Bericht, welche Schritte erfolgreich abgeschlossen wurden.
