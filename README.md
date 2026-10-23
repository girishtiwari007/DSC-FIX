# DSC Signer GitHub package

Upload `install.ps1` to the `main` branch of a public GitHub repository. Replace OWNER and REPOSITORY in the following command, then run it in PowerShell on Windows:

```powershell
irm 'https://raw.githubusercontent.com/OWNER/REPOSITORY/main/install.ps1' | iex
```

The URL becomes usable after upload. Replace `main` with your branch name if different, or a full commit SHA for a fixed version.

Check-only mode (diagnostic logs/reports are written, no repairs applied):

```powershell
& ([scriptblock]::Create((irm 'https://raw.githubusercontent.com/OWNER/REPOSITORY/main/install.ps1'))) -Mode check
```

For interactive console mode, use the same command with `-Mode console`.

Local launch:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\install.ps1
```

This is a self-contained PowerShell launcher, not an MSI or a rewrite of the application. It embeds the original batch file, verifies its SHA-256, saves it under `%LOCALAPPDATA%\DSC-Signer-Launcher\<unique-id>`, and launches it. The separate batch file is included for inspection and direct use; the hosted launcher requires only install.ps1. Windows PowerShell must be installed as required by the original application.

The original GUI, console, repair selection, pack mode, and administrator restart behavior are preserved. Default mode is GUI. Repairs still require the original application's user selection and UAC approval when applicable. Selected repairs can affect Java, certificates, hosts entries, browser policies, environment variables, processes, and scheduled tasks. The Digital Signer Service installer is not embedded in the supplied source; the application may ask you to select it.

The payload hash detects corruption; it does not authenticate the publisher. Review the repository before running remote code.

Extracted folders remain available for administrator restarts and pack mode. You may delete individual folders under `%LOCALAPPDATA%\DSC-Signer-Launcher` after all application windows close. The application keeps its own logs and backups separately, generally under `%ProgramData%\DSC-Signer-Fixer`.

Validation: launcher and embedded PowerShell syntax checked, and extract-only execution through a scriptblock verified against the original batch SHA-256. Repairs, GUI, UAC, and installation were not executed during packaging.
