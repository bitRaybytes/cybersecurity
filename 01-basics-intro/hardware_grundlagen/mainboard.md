# 💻 Mainboard und ACPI: Das Fundament der System-Sicherheit

Das Mainboard, auch Haupt- oder Motherboard genannt, ist die zentrale Steuerplatine eines Computers. Es verbindet alle Komponenten und ist die kritische Hardware-Ebene, die für die physische und Firmware-Sicherheit verantwortlich ist.

## Inhaltsverzeichnis
- [Einführung](#einführung)
- [Das Mainboard - warum es sicherheitsrelevant ist](#das-mainboard---warum-es-sicherheitsrelevant-ist)
    - [Stark vereinfachte Mainboard-Topologie](#stark-vereinfachte-mainboard-topologie)
- [Firmware: BIOS, UEFI — Angriffsfläche & Schutzmaßnahmen](#firmware-bios-uefi--angriffsfläche--schutzmaßnahmen)
    - [Warum Firmware kritisch ist](#warum-firmware-kritisch-ist)
    - [Typische Firmware-Angriffe](#typische-firmware-angriffe)
    - [Schutzmaßnahmen (Firmware-Hardening)](#schutzmaßnahmen-firmware-hardening)
- [Physische Komponenten & Risiken (Jumper, TPM, Steckplätze)](#physische-komponenten--risiken-jumper-tpm-steckplätze)
- [Anschlüsse & Schnittstellen — Einfallstore](#anschlüsse--schnittstellen--einfallstore)
- [Empfehlungen](#empfehlungen)
- [ACPI: Funktionsweise, S-States/C-States und Sicherheitsprobleme](#acpi-funktionsweise-s-statesc-states-und-sicherheitsprobleme)
- [Angriffe & Exploits — Beispiele und Erkennungsmerkmale](#angriffe--exploits--beispiele-und-erkennungsmerkmale)
- [Hardening-Checklist — Sofortmaßnahmen & Best Practices](#hardening-checklist--sofortmaßnahmen--best-practices)
- [Forensik & Erkennung - was prüfen?](#forensik--erkennung---was-prüfen)
- [Haftungsausschluss](#haftungsausschluss)



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Einführung

Das Mainboard (Motherboard) ist die zentrale Hardware-Plattform eines Rechners: CPU-Sockel, RAM-Steckplätze, Firmware-Chip, Interconnects (PCIe) und I/O-Anschlüsse. Viele Sicherheitsentscheidungen und -mechanismen sitzen hier entweder physisch (TPM, Jumper, Hardware-Switches) oder firmwareseitig (BIOS/UEFI, ACPI). Ein kompromittiertes Mainboard/UEFI kann ein System vollständig unter Kontrolle bringen — vor dem OS-Start.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Das Mainboard - warum es sicherheitsrelevant ist

- Alle Komponenten verbinden sich über das Mainboard **->** physische Ebene der Vertrauens-Kette.

- **Firmware läuft vor dem OS:** Manipulation ermöglicht persistente, schwer entdeckbare Malware (Firmware-Rootkits).

- Physischer Zugriff (z. B. auf JTAG, SPI-Flash, M.2/PCIe) ermöglicht Auslesen/Verändern sensitiver Daten.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Stark vereinfachte Mainboard-Topologie
```text
+-------------------------------------------------------------+
| CPU Socket  [CPU]    RAM Slots [DIMM1][DIMM2]   M.2 Slot    |
|       |                 |                 |                 |
|    Chipset (SATA, PCIe lanes, ACPI controller)              |
|       |         +----------------+    +----------------+    |
|       +-------- | PCIe Slots     |    | SATA Ports     |    |
|                 | [GPU][NIC][etc]|    | [SATA1] [SATA2]|    |
|  USB Headers -> |                |    +----------------+    |
|  TPM Header ->  +----------------+                          |
|  BIOS/UEFI (SPI Flash)                                      |
|  Front I/O (USB, Audio, Power)                              |
+-------------------------------------------------------------+

```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Firmware: BIOS, UEFI — Angriffsfläche & Schutzmaßnahmen
### Warum Firmware kritisch ist

**Firmware** (BIOS/UEFI) startet zuerst, initialisiert Hardware und lädt das Betriebssystem. Eine kompromittierte Firmware läuft vor Anti-Malware, Kernel-TLS und Host-Monitoring.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Typische Firmware-Angriffe

- **Firmware-Rootkits:** persistente Malware, bleibt nach OS-Neuinstallation bestehen.

- **SPI/Flash-Manipulation:** direktes Schreiben in den Flash-Chip (z. B. via physischem SPI-Programmer).

- **Bootkit / Measured Boot-Bypass:** Manipulieren des UEFI, um den gemessenen Start/sicheren Start zu untergraben.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



### Schutzmaßnahmen (Firmware-Hardening)

- **Secure Boot aktivieren (UEFI):** signierte Bootloader erzwingen.
- **Measured Boot & TPM-Attestation:** Boot-Messwerte im TPM speichern, Remote Attestation nutzen.
- **UEFI-Secure Firmware Update:** nur signierte Firmware-Updates zulassen.
- **Write-Protect für SPI-Flash:** Hardware-jumper oder Firmware Lock Bits nutzen.
- **Firmware-Integrity-Monitoring:** regelmäßige Checksums / Signed Image Verification.
- **Firmware-Rollback-Protection:** verhindert Downgrade auf verwundbare Versionen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Physische Komponenten & Risiken (Jumper, TPM, Steckplätze)
### Jumper & CMOS/Battery-Reset
Jumper zum Zurücksetzen des BIOS/UEFI oder CMOS können Sicherheitskonfigurationen (passwortgeschütztes BIOS) umgehen. Physischer Zugriff erlaubt häufig sofortiges Umgehen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Trusted Platform Module (TPM)
TPM speichert kryptografische Schlüssel und ermöglicht Measured Boot, Full Disk Encryption Schutz (z. B. BitLocker). TPM erhöht Sicherheit erheblich, ist aber nur wirksam bei korrekt konfigurierter Integritätskette (Secure Boot + Measured Boot).



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Steckplätze (PCIe, M.2, SATA)
Interne Steckplätze können missbraucht werden:

- M.2/PCIe-NVMe SSDs auslesen oder manipulieren.
- bösartige PCIe-Peripherie (z. B. rogue NIC mit DMA) kann direkten RAM-Zugriff (DMA) ermöglichen → **IOMMU/VT-d** Schutz nötig.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Anschlüsse & Schnittstellen — Einfallstore

- **USB (BadUSB):** Manipulierte Firmware auf USB-Geräten kann beim Einstecken Payload ausführen (Tastatur, Netzwerkkarte etc.).

- **Thunderbolt / PCIe-over-Cable:** direkter DMA-Zugriff möglich, Schutz per IOMMU/Kernel-Konfiguration wichtig.

- **UART / JTAG / SPI Headers (intern):** ermöglichen Debug/Flash Zugriff; sollten bei Produktionsgeräten abgedeckt/gesichert sein.

- **Network Interfaces:** kompromittierte Netzwerkkarten (Firmware) können Man-in-the-Firmware-Angriffe ausführen.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Empfehlungen

- Physische Ports absichern (abdecken, abschalten im BIOS).
- USB-Policy / Device-Control (Whitelist) in Unternehmensumgebungen.
- IOMMU (Intel VT-d / AMD-VI) aktivieren, Kernel-DMA-Filters konfigurieren.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## ACPI: Funktionsweise, S-States/C-States und Sicherheitsprobleme
### Was ist ACPI?
ACPI (Advanced Configuration and Power Interface) ist ein Standard, der dem OS erlaubt, Energie- und Konfigurations-Policies zu steuern (z. B. Sleep, Hibernate, CPU-C-States). ACPI enthält eine Firmware-kompilierte Ausgabesprache (AML/ASL), die das Betriebssystem interpretiert.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Wichtige Konzepte

- **S-States (System):** **S0** (running), **S1**/**S2** (low power), **S3** (suspend to RAM), **S4** (hibernate — suspend to disk), **S5** (soft off).
- **C-States (CPU):** **C0**..**Cn** — CPU Idle-States mit unterschiedlichem Stromverbrauch.
- **AML/ASL:** Code im UEFI/BIOS, den OS interpretiert — *Turing-vollständig genug*, um Logik auszuführen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Sicherheitsrelevanz von ACPI

- **Manipulation der ACPI-Tabellen (AML):** Ein Angreifer kann gefährliches Verhalten in AML einbetten (z. B. Tasten-/IO-Umleitungen), das das OS beim Interpretieren ausführt.
- **Memory Disclosure in S4/S3:** Bei S3 (RAM bleibt geladen), S4 (Hibernation schreibt RAM auf Disk) können sensitive Daten verbleiben; Fehlerhafte Verschlüsselung der Hiberfile führt zu Disclosure.
- **ACPI-Treiberbugs:** Kernel treiber, die ACPI-Tabelle interpretieren, können zu Privilege-Escalation führen (Parsing-Bugs).
- **Wake-on-LAN / wake events:** unerwartete Wake-Events können Ausnutzungspfade eröffnen.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### ACPI S-States (vereinfachte Darstellung)
```text
+-------------------------------------------+
| S0 (Running)   <- Normalbetrieb           |
+-------------------------------------------+
          |
          v
+-------------------------------------------+
| S1/S2 (Low power, CPU idle)               |
+-------------------------------------------+
          |
          v
+-------------------------------------------+
| S3 (Suspend to RAM)  -- RAM bleibt geladen|
+-------------------------------------------+
          |
          v
+-------------------------------------------+
| S4 (Hibernate)  -- RAM => Disk (hiberfil)|
+-------------------------------------------+
          |
          v
+-------------------------------------------+
| S5 (Soft Off)  -- Gerät ist aus           |
+-------------------------------------------+
```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


## Angriffe & Exploits — Beispiele und Erkennungsmerkmale
### Firmware-Rootkit

- **Symptom:** Persistente Malware, die trotz OS-Neuinstallation bleibt.

- **Erkennung:** Abweichende Firmware-Checksums, Nichtübereinstimmung von UEFI-Signaturen, verdächtige Boot-Messwerte im TPM.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### BadUSB / USB-Firmware Malware

- **Symptom:** Gerät verhält sich plötzlich als Tastatur/Network Adapter.

- **Erkennung:** Unerwartete HID-Events, neue Netzwerkschnittstellen nach USB-Plug.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### DMA-Angriffe (Thunderbolt/PCIe)

- **Symptom:** Unauthorisierte Memory Reads/Writes, Instabile Prozesse.

- **Schutz:** IOMMU aktivieren; Thunderbolt security level / Kernel config prüfen.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### ACPI-/AML-Exploits

- **Symptom:** Kernel-Panics beim ACPI-Parsing, ungewöhnliches Verhalten beim Sleep/Wake.

- **Erkennung:** Kernel-Logs (dmesg) auf ACPI-Parsing-Fehler prüfen.

<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>



## Hardening-Checklist — Sofortmaßnahmen & Best Practices

### Firmware & Boot

- UEFI + Secure Boot aktivieren.

- Firmware auf aktuelle, signierte Version updaten (Vendor-Signed).
- Write-Protect für SPI/BIOS-Flash aktivieren (sofern Hardware unterstützt).
- Measured Boot / TPM-Attestation einrichten.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### OS & Kernel

- IOMMU (VT-d/AMD-VI) aktivieren, DMA-Protection konfigurieren.

- Kernel- & ACPI-Patches aktuell halten.
- Hibernation (S4) sicher konfigurieren: Hiberfile mit Full Disk Encryption oder deaktivieren.
- USB Device Control: Only allow known device classes / use endpoint whitelisting.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>


### Physisch

- Gehäuse mit Chassis-Intrusion detection / verschließbare Gehäuse.

- SPI / JTAG / UART Header nach Produktion abdecken.
- TPM-Modul verwenden und korrekt provisionieren.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Netzwerk & Peripherie

- Netzwerk-Boot/Remote-Management (iLO, iDRAC) absichern (starke Passwörter, 2FA, Management-VLAN).

- Keine unkontrollierten Thunderbolt/USB-Ports in sensiblen Umgebungen.


<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

### Monitoring & Detection

- Periodische Firmware-Integritätschecks (Hashes, signed images).

- TPM-Based attestation logs überwachen.
- EDR/Host-Monitoring auf ungewöhnliche Boot-Sequenzen oder persistente Rootkit-Indikatoren.



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Forensik & Erkennung - was prüfen?

- **Firmware Checksums / Signaturen:** Vergleiche SPI-Flash Inhalt mit Vendor-Image.

- **TPM Logs (PCRs)**: Prüfe Boot-Messwerte (PCRs) auf Remote Attestation.
- **dmesg / Kernel Logs:** ACPI-Parsing-Fehler, DMA-Anomalien, neue/unerwartete Bus-Geräte.
- **USB Audit:** Welche USB-Geräte wurden wann verbunden (Auditd/USBGuard).
- **Memory Forensics:** Bei S3/S4 prüfen, ob sensible Daten unverschlüsselt sind.
- **Bootloader/Bootsektor:** Verifikation von GRUB/Bootloader-Integrität.




<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Nützliche Kommando-Snippets (Linux, Prüfung & Hardening)

Prüfe TPM / PCRs (nur Beispiel, erfordert Tools & Berechtigungen):
```bash
# tpm2_pcrread
tpm2_pcrread

# prüfe Secure Boot status
mokutil --sb-state

# IOMMU aktiv (Kernel cmdline)
# GRUB: add intel_iommu=on oder amd_iommu=on

```



<div align=right>

[↑ Inhaltsverzeichnis](#inhaltsverzeichnis)

</div>

## Haftungsausschluss

Dieses Repository dient ausschließlich zu Ausbildungs-, Forschungs- und Demonstrationszwecken im Bereich der IT-Sicherheit.

Alle hier dokumentierten Techniken und Tools dürfen nur in legalen und autorisierten Testumgebungen verwendet werden – z. B. in Labors, CTFs oder mit ausdrücklicher Genehmigung des Eigentümers der Zielsysteme.

Wir distanzieren uns ausdrücklich von jeglicher illegalen Nutzung.
Dieses Projekt richtet sich an White-Hat-Sicherheitsforscher, Ethical Hacker und Auszubildende, die ethisch und rechtlich korrekt handeln.

[Disclaimer](/00-disclaimer/disclaimer.md)

--- 


Stay curious – stay secure. 🔐

🗓️ **Letzte Aktualisierung:** September 2025  
🤝 **Pull Requests willkommen** – Vorschläge für neue Kurse oder Kategorien gerne einreichen!

---