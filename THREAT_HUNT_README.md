# 🕵️ Threat Hunt: PowerShell Post-Credential Access Validation

## Objective
Validate whether a credential dumping detection (**MITRE T1003**) was followed by attacker post-exploitation activity or lateral movement.

## Hypothesis
If the identified credential access was malicious, the actor would likely follow up by executing obfuscated PowerShell commands to maintain persistence or harvest further environment data.

---

## 🛠 Hunt Query (CrowdStrike Falcon)

**Event Type:** `ProcessRollup2`  
**Target Host:** `VSDWSDLCTXSPT01`  

**Indicators of Interest:**
* `powershell.exe`
* `powershell_ise.exe`
* `CommandLine` contains `-enc` or `-EncodedCommand`

---

## 🔍 Investigation Steps

1.  **Scope:** Isolated the hunt specifically to the endpoint where the T1003 alert originated.
2.  **Querying:** Extracted all `ProcessRollup2` telemetry for PowerShell execution within a 4-hour window of the initial alert.
3.  **De-obfuscation Check:** Specifically searched for encoded command lines or non-standard execution flags.
4.  **Contextual Validation:** Compared the `ParentProcess` and `User` context against known administrative baseline behavior.

---

## 📊 Visual Evidence: ProcessRollup2 Analysis

To validate the post-alert activity, I reviewed the process execution metadata to identify any deviations from the baseline.

### PowerShell Execution Review
![ProcessRollup2 Analysis 1](https://github.com/albe290/T1003-Investigations-Visuals/blob/main/Threat%20Hunt%20ProcessRollup%202-1.png?raw=true)

### Command Line Correlation
![ProcessRollup2 Analysis 2](https://github.com/albe290/T1003-Investigations-Visuals/blob/main/Threat%20Hunt%20ProcessRollup%202.png?raw=true)

---

## 📝 Findings & Conclusion

### Findings:
* **PowerShell Activity:** Execution was observed on the target host following the registry access.
* **Payload Analysis:** No encoded payloads or suspicious download strings (`IWR`, `Net.WebClient`) were identified.
* **Administrative Alignment:** The command-line arguments and parent process tree aligned with authorized administrative scripts used for environment auditing.

### Conclusion:
**No evidence of post-exploitation activity.** The detection was validated as non-malicious administrative behavior. The hunt confirms that while the T1003 trigger was technically accurate (behavioral match), the intent was benign.

---

## 🛠 Skills Applied
* **Threat Hunting (Hypothesis-based)**
* **EDR Telemetry Analysis (CrowdStrike)**
* **PowerShell Forensics**
* **Post-Exploitation Validation**
