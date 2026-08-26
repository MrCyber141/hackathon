# SIH CyberAudit

SIH CyberAudit is a Windows command-line security assessment tool. It collects
basic host, network, Windows Firewall, and Microsoft Defender information,
then reports security findings and a score from 0 to 100.

## Requirements

- Windows 10 or Windows 11
- Python 3.9 or newer
- PowerShell with the Windows Defender and NetFirewall cmdlets available
- Administrator privileges for the most complete results

The application uses only Python's standard library. `requirements.txt` is
kept for future optional dependencies.

## Run

From the repository root:

```powershell
python -m cyberaudit version
python -m cyberaudit scan
```

Write a machine-readable report:

```powershell
python -m cyberaudit scan --json reports\scan.json
```

Use `--host-only` to stop rather than scan when the machine appears to be a
virtual machine:

```powershell
python -m cyberaudit scan --host-only
```

Aggregate existing JSON reports:

```powershell
python -m cyberaudit aggregate reports
python -m cyberaudit aggregate reports --avg
```

## Remediation helper

The repository includes `remediate_smb_defender.ps1`. It attempts to enable
Microsoft Defender real-time protection and adds a Public-profile inbound
firewall rule for SMB ports 139 and 445. Review the script and its impact
before running it in an elevated PowerShell session:

```powershell
PowerShell -ExecutionPolicy Bypass -File .\remediate_smb_defender.ps1
```

The script does not disable the Server service by default.

## Project layout

```text
cyberaudit/
  analyzers/     Finding checks and risk scoring
  collectors/    Windows and network data collection
  models/        Finding and scan result models
  output/        Terminal and JSON output
  utils/         Privilege and virtual-machine helpers
tests/           Test location
```

## Limitations

- This is an early prototype and currently focuses on Defender, Firewall,
  listening ports, and basic host information.
- Results depend on the Windows version, available PowerShell cmdlets, and
  the privileges of the account running the scan.
- A scan is informational; findings should be reviewed before remediation.

## Development

Run the module help to inspect all commands:

```powershell
python -m cyberaudit --help
```

Before publishing reports, remove or redact host-specific data. Generated
reports and local environments are excluded by `.gitignore`.
