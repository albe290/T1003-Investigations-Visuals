🕵️ Threat Hunt – PowerShell Post-Credential Access Validation
Objective

Validate whether a credential dumping detection (MITRE T1003) was followed by attacker post-exploitation activity.

Hypothesis

If credential access was malicious, attackers would likely execute obfuscated PowerShell commands.

Hunt Query (CrowdStrike)

Event Type: ProcessRollup2
Host: VSDWSDLCTXSPT01
Indicators:

powershell.exe

powershell_ise.exe

CommandLine contains -enc

Investigation Steps

Scoped hunt to affected endpoint

Queried PowerShell execution telemetry

Searched for encoded or obfuscated commands

Validated execution context

Findings

PowerShell execution observed

No encoded payloads identified

Activity aligned with authorized administrative usage

Conclusion

No evidence of post-exploitation activity. Detection validated as non-malicious administrative behavior.

