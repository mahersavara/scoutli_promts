# Prompt: Sync, Compare & Pull Code

Use this prompt to instruct an AI agent or automate the process of synchronizing your local microservices with their remote GitHub repositories.

---

**Goal:** Check all relevant local Git repositories (including the root, microservices subfolders, and other project repositories), compare them with their remote `origin/master` (or `main`), fetch updates, and pull changes if any divergence is found.

**Context:**
- This script is designed to be run from the root of your multi-repo workspace (e.g., `C:\Users\EKHUATL5J\Downloads\Work\ericodessy\scoutli_java`).
- It will check repositories directly under this root, and also dive into the `microservices/` folder if it's a Git repository itself or contains other Git repositories.

**Instructions for AI:**

1.  **Identify Repositories:** Locate all directories within the current working directory that are Git repositories (contain a `.git` folder). This includes the current directory itself and any immediate subdirectories.
2.  **Fetch Updates:** For each identified repository, run `git fetch origin` to get the latest metadata from GitHub.
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
$githubUser = "mahersavara" # Thay đổi thành GitHub username của bạn

# Tìm kiếm tất cả các thư mục con là git repo, và cả thư mục hiện tại
$repos = @()
# Add the current directory if it's a git repo
if (Test-Path ".git") {
    $repos += Get-Item "."
}
# Add direct subdirectories that are git repos
$repos += Get-ChildItem -Directory -Exclude node_modules,target,build,dist | Where-Object { Test-Path "$($_.FullName)\.git" }
# Add subdirectories within 'microservices' if 'microservices' exists and contains git repos
if (Test-Path "microservices" -PathType Container) {
    $repos += Get-ChildItem -Path "microservices" -Directory | Where-Object { Test-Path "$($_.FullName)\.git" }
}

foreach ($repo in $repos) {
    Write-Host "Checking $($repo.Name)..." -ForegroundColor Cyan
    Push-Location $repo.FullName
    
    # Đảm bảo remote origin tồn tại và chính xác
    try {
        $remoteUrl = git config --get remote.origin.url
        if (-not ($remoteUrl -eq "https://github.com/$githubUser/$($repo.Name).git")) {
            git remote remove origin | Out-Null
            git remote add origin "https://github.com/$githubUser/$($repo.Name).git"
            Write-Host "  -> Updated remote origin for $($repo.Name)." -ForegroundColor Yellow
        }
    } catch {
        git remote add origin "https://github.com/$githubUser/$($repo.Name).git"
        Write-Host "  -> Added remote origin for $($repo.Name)." -ForegroundColor Yellow
    }

    # Fetch
    git fetch origin
    
    # Detect default branch (master or main)
    $localBranch = (git rev-parse --abbrev-ref HEAD)
    $remoteBranch = "origin/master"
    if (git ls-remote --heads origin main) {
        $remoteBranch = "origin/main"
    }

    # Compare Status
    $status = git status -uno
    if ($status -match "Your branch is behind '$($remoteBranch)'") {
        Write-Host "  -> Updates found. Pulling..." -ForegroundColor Yellow
        git pull
        Write-Host "  -> Updated successfully." -ForegroundColor Green
    } elseif ($status -match "Your branch is up to date with '$($remoteBranch)'") {
        Write-Host "  -> Up to date." -ForegroundColor Gray
    } elseif ($status -match "Your branch is ahead of '$($remoteBranch)'") {
        Write-Host "  -> Local is ahead. Please push your changes." -ForegroundColor Magenta
    } elseif ($status -match "have diverged,") {
        Write-Host "  -> Branches have diverged. Please resolve manually (e.g., git pull --rebase)." -ForegroundColor Red
    } else {
        Write-Host "  -> Status: $($status.Split("`n")[0])" -ForegroundColor Magenta # Chỉ lấy dòng đầu tiên của status
    }
    
    Pop-Location
}
```
*Lưu ý: Script này giả định thư mục làm việc hiện tại của bạn (`scoutli_java`) KHÔNG PHẢI là một git repository. Nếu nó là một git repo, cần điều chỉnh lại để nó cũng được kiểm tra.*
```