# Prompt: Setup Tool Paths

Use this prompt to instruct an AI agent or automate the setup of tool paths (`kubectl`, `helm`, `argocd`) based on the current working directory.

---

**Goal:** Dynamically locate the `tools` directory within the current project and configure variables or environment paths so that commands can be executed easily without typing full absolute paths.

**Instructions for AI:**

1.  **Identify CWD:** Get the current working directory.
2.  **Locate Tools:** Check for a `tools` subdirectory.
3.  **Verify Executables:** Confirm the existence of `kubectl.exe`, `helm.exe`, and `argocd.exe` within that directory.
4.  **Output Configuration:** Generate PowerShell commands to set these paths as variables or add them to the system PATH for the current session.

**Example PowerShell Script:**

```powershell
# Get current directory
$projectRoot = Get-Location

# Define tools directory
$toolsDir = Join-Path $projectRoot "tools"

if (Test-Path $toolsDir) {
    Write-Host "Tools directory found at: $toolsDir" -ForegroundColor Cyan
    
    # Set variables for easy access
    $global:kubectl = Join-Path $toolsDir "kubectl.exe"
    $global:helm = Join-Path $toolsDir "helm.exe"
    $global:argocd = Join-Path $toolsDir "argocd.exe"
    
    # Verify and Output
    if (Test-Path $global:kubectl) { Write-Host "  kubectl: $global:kubectl" }
    if (Test-Path $global:helm) { Write-Host "  helm:    $global:helm" }
    if (Test-Path $global:argocd) { Write-Host "  argocd:  $global:argocd" }
    
    # Optional: Add to PATH for current session
    $env:Path += ";$toolsDir"
    Write-Host "Added tools to PATH for this session." -ForegroundColor Green
} else {
    Write-Host "Tools directory not found in $projectRoot" -ForegroundColor Red
}
```

**Usage:**
Copy and paste the script above into your PowerShell terminal. Then you can simply use `kubectl`, `helm`, or `argocd` directly.
