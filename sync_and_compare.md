# Prompt: Sync, Compare & Pull Code

Use this prompt to instruct an AI agent or automate the process of synchronizing your local microservices with their remote GitHub repositories.

---

**Goal:** Check all local microservices directories, compare them with the remote `origin/master` (or `main`), fetch updates, and pull changes if any divergence is found.

**Context:**
- Root Directory: `scoutli_java/`
- Microservices Directories: `scoutli-auth-service`, `scoutli-discovery-service`, `scoutli-interaction-service`, `scoutli-frontend`, `scoutli-infra`.

**Instructions for AI:**

1.  **List Repositories:** Identify all subdirectories in the current workspace that are valid Git repositories.
2.  **Fetch Updates:** For each repository, run `git fetch origin` to get the latest metadata from GitHub.
3.  **Compare Status:**
    - Run `git status -uno` to check local status.
    - Compare local `HEAD` with `origin/master` (or `origin/main`).
4.  **Action - Pull if Needed:**
    - If `HEAD` is behind `origin`, execute `git pull`.
    - If `HEAD` is ahead, notify the user (do not push automatically).
    - If there are merge conflicts, stop and notify the user.
5.  **Report:** Generate a summary report of what was updated.

**Example PowerShell Script to execute this workflow:**

```powershell
$repos = Get-ChildItem -Directory | Where-Object { Test-Path "$($_.FullName)\.git" }

foreach ($repo in $repos) {
    Write-Host "Checking $($repo.Name)..." -ForegroundColor Cyan
    Push-Location $repo.FullName
    
    # Fetch
    git fetch origin
    
    # Check status
    $status = git status -uno
    if ($status -match "Your branch is behind") {
        Write-Host "  -> Updates found. Pulling..." -ForegroundColor Yellow
        git pull
        Write-Host "  -> Updated successfully." -ForegroundColor Green
    } elseif ($status -match "Your branch is up to date") {
        Write-Host "  -> Up to date." -ForegroundColor Gray
    } else {
        Write-Host "  -> Status: $status" -ForegroundColor Magenta
    }
    
    Pop-Location
}
```
