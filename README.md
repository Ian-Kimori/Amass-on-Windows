# Amass-on-Windows

Amass ships prebuilt Windows binaries, so you have three practical routes. The current major version is v5, and it can also be installed from source with `go install -v github.com/owasp-amass/amass/v5/...@latest`.

**Option 1: Prebuilt binary (simplest)**

Grab `amass_windows_amd64.zip` from the latest release at `github.com/owasp-amass/amass/releases`, extract it, and put `amass.exe` somewhere on your PATH:

```powershell
Expand-Archive .\amass_windows_amd64.zip -DestinationPath C:\Tools\amass
[Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\Tools\amass\amass_windows_amd64", "User")
amass -version
```

**Option 2: Go install**

With Go installed (`winget install GoLang.Go`):

```powershell
go install -v github.com/owasp-amass/amass/v5/...@latest
```

The binary lands in `%USERPROFILE%\go\bin\amass.exe`, which the Go installer usually adds to PATH.

**Option 3: WSL**

Since you already work on Linux, running it inside WSL (`sudo apt install amass` on Kali WSL, or the Go install) sidesteps Windows DNS resolver quirks and tends to behave more predictably for brute-forcing.

**Config location**

On Windows, the config folder is `%AppData%\amass\`. Drop `config.yaml` and `datasources.yaml` (your API keys for Shodan, SecurityTrails, Netlas, etc.) there.

**Basic usage**

```powershell
amass enum -passive -d example.com
amass enum -active -brute -d example.com -o C:\recon\example-subs.txt
amass subs -names -d example.com
```

In v5, `enum` writes findings into the local asset database, and `amass subs` is how you pull the discovered names back out afterwards, so if `enum` seems to print little, query with `subs`.

A couple of Windows-specific notes: run it from PowerShell or Windows Terminal rather than old cmd for cleaner output, and if active enumeration is slow or returns oddly few results, pass a resolver list explicitly with `-rf C:\recon\resolvers.txt` since the default Windows resolver can throttle heavy query volume.
