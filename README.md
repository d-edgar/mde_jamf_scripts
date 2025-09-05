# Microsoft Defender for Endpoint (MDE) – Jamf Extension Attributes

This repository provides **Jamf Pro Extension Attributes (EAs)** and supporting scripts to monitor and report on the health of **Microsoft Defender for Endpoint (MDE)** on macOS devices.  

By leveraging these EAs, you can create **Smart Groups** and **Advanced Searches** in Jamf Pro to dynamically identify Macs that are out of compliance — for example, those with network protection stopped, real-time protection disabled, or outdated engine versions.

---

## 📋 Overview

The scripts use the [`mdatp`](https://learn.microsoft.com/microsoft-365/security/defender-endpoint/mac-install-manually) command-line tool (installed with Defender for Endpoint) to query health fields such as:

- **Network Protection**
- **Real-Time Protection**
- **Tamper Protection**
- **Healthy flag**
- **Onboarding/EDR status**
- **App and Engine versions**
- **Signature/Definition updates** (optional)

Each script is formatted for Jamf Extension Attributes, returning values inside `<result> … </result>`.

---

## 🛠️ Extension Attributes Included

### Network Protection
- `network_protection_status`  
- `network_protection_enforcement_level`  

### Real-Time Protection
- `real_time_protection_enabled`  
- `real_time_protection_available`  
- `real_time_protection_subsystem`  

### Tamper Protection
- `tamper_protection` (block | audit | disabled)  

### General Health
- `healthy` (overall status)  

### App / Engine Versions
- `app_version`  
- `engine_version`  

### EDR
- `is_onboarded` or `org_id` (depending on Defender version)  

---

## 🚀 Usage

1. **Upload to Jamf Pro**
   - In **Jamf Pro → Settings → Computer Management → Extension Attributes**, create a new EA.
   - Paste the script into the “Script” section.
   - Set the **Data Type**:
     - *String* for text values (subsystems, versions, tamper protection).
     - *Boolean* for true/false flags.

2. **Inventory Update**
   - Run `sudo jamf recon` on a test Mac to populate values.
   - Verify EA results appear in the computer record.

3. **Smart Groups**
   - Examples:
     - Macs with **Network Protection stopped/disabled**  
       → EA `MDE: Network Protection Status` contains `stopped`  
     - Macs with **RTP disabled**  
       → EA `MDE: RTP Enabled` equals `false`  
     - Macs **not onboarded**  
       → EA `MDE: EDR Onboarded` equals `false` or `Unknown`  

---

## 📂 Repository Structure
mde_jamf_scripts/
│
├── network_protection_status.sh
├── real_time_protection_enabled.sh
├── real_time_protection_available.sh
├── real_time_protection_subsystem.sh
├── tamper_protection.sh
├── healthy.sh
├── app_version.sh
├── engine_version.sh
├── edr_onboarded.sh
└── README.md

---

## 🔍 Requirements

- macOS with **Microsoft Defender for Endpoint** installed.  
- `mdatp` binary available at `/usr/local/bin/mdatp` (or in `$PATH`).  
- Jamf Pro with permission to create and assign Extension Attributes.

---

## 📖 References

- [Microsoft Docs – Defender for Endpoint on macOS](https://learn.microsoft.com/microsoft-365/security/defender-endpoint/mac-whatsnew)  
- [Jamf Pro – Extension Attributes](https://learn.jamf.com/bundle/jamf-pro-documentation-current/page/Computer_Extension_Attributes.html)  

---

## 🤝 Contributing

Pull requests are welcome! If you’d like to add more fields, fix scripts, or extend functionality, please open an issue or submit a PR.

---

## 📜 License

MIT License. See [LICENSE](LICENSE) for details.
