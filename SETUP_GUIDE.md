# Setup Guide: R, RStudio, Quarto, Git, GitHub and Netlify

Follow these steps in order. Every tool is free. Budget about one hour the first time.
When a step says "in the Console," it means the Console pane in RStudio (bottom left by default).

## What you are setting up, in plain words

- **R** is the programming language.
- **RStudio** is the program you write R in. It also includes **Quarto**, the tool that turns your `.qmd` pages into a website.
- **Git** is a program on your computer that records the history of your files (each saved snapshot is a **commit**).
- **GitHub** is a website that stores a copy of that history online (your **repository**, or repo). Sending your commits there is called a **push**.
- **Netlify** is a website host. It watches your GitHub repository and publishes your rendered site every time you push.

The chain is: edit in RStudio, render with Quarto, commit and push with Git, and Netlify publishes. Nothing is published that you did not push.

---

## Step 1: Install R

1. Go to https://cran.r-project.org
2. **Windows:** click "Download R for Windows", then "base", then the download link. Run the installer with the defaults.
3. **Mac:** click "Download R for macOS". Pick `arm64` if your Mac has an Apple chip (M1 to M4) or `x86_64` for an older Intel Mac (Apple menu, About This Mac, tells you). Run the installer.

## Step 2: Install RStudio Desktop

1. Go to https://posit.co/download/rstudio-desktop/ and download RStudio Desktop (free) for your system. Install with the defaults.
2. Open RStudio. In the Console type `1 + 1` and press Enter. If you see `[1] 2`, R and RStudio are talking to each other.
3. Quarto is included in RStudio; you do not install it separately.

## Step 3: Install the R packages the course uses

In the Console, run this line and wait for it to finish (it can take several minutes the first time):

```r
install.packages(c("tidyverse", "tidymodels", "usethis", "gitcreds", "rmarkdown"))
```

Check it worked:

```r
library(tidyverse)
library(tidymodels)
```

Both should load with no error (messages listing packages are fine).

## Step 4: Install Git

Git is separate from GitHub and must be installed on your computer.

1. **Windows:** go to https://git-scm.com/download/win, download "Git for Windows", install with the defaults.
2. **Mac:** open the Terminal app (Applications, Utilities, Terminal), type `git --version` and press Enter. If Git is missing, macOS offers to install the command line tools; accept and wait.
3. Restart RStudio after installing Git. Then in the Console:

```r
usethis::use_git_config(user.name = "Your Name", user.email = "you@students.niu.edu")
```

Use the same email you will use for GitHub in the next step.

## Step 5: Create your GitHub account

1. Go to https://github.com and sign up with your university email.
2. Pick a username you would be comfortable putting on a resume: this repository is part of your portfolio.
3. Optional but worth it: apply for the GitHub Student Developer Pack at https://education.github.com/pack (free tools with a student email).

## Step 6: Create your project repository from the template

1. Open the template link posted in the Dr. B Learning Arena.
2. Click **Use this template**, then **Create a new repository**.
3. Name it something a recruiter can read (for example `qsr-demand-analysis`), keep it **Public**, click **Create repository**.
4. On your new repository page click the green **Code** button and copy the HTTPS address (it ends in `.git`). You need it in Step 7.

If the template was given to you as a zip instead: unzip it, then in Step 7 use File, New Project, Existing Directory, pick the folder, and afterwards run `usethis::use_github()` in the Console to create the GitHub repository from it.

## Step 7: Open the repository in RStudio

1. In RStudio: File, New Project, **Version Control**, **Git**.
2. Paste the HTTPS address from Step 6 into "Repository URL". Choose where on your computer the folder should live. Click **Create Project**.
3. RStudio downloads (clones) the repository and opens it as a Project. From now on, always open your work by double-clicking the `.Rproj` file in that folder, or through File, Open Project.

## Step 8: Connect RStudio to GitHub with a personal access token

GitHub does not accept your account password from RStudio. It accepts a **personal access token** (PAT), a long code that acts as a password for tools.

1. In the Console run:

```r
usethis::create_github_token()
```

   A browser page opens on GitHub with the right settings pre-selected. In the "Note" field write something like `OMIS 681 laptop`. Leave the expiration and the pre-checked boxes as they are, scroll down, click **Generate token**.
2. Copy the token shown (it starts with `ghp_`). You will never see it again, so copy it now.
3. Back in the Console run:

```r
gitcreds::gitcreds_set()
```

   When asked for the password or token, paste the token and press Enter.
4. Confirm everything is connected:

```r
usethis::git_sitrep()
```

   Look for your name, your email, and "Personal access token: <discovered>". If it says <unset>, repeat this step.

Tokens expire (the default is 30 days). When RStudio starts asking for a password again, create a new token with the same two commands.

## Step 9: Add Dr. B as a collaborator (required for grading)

1. On your repository page on GitHub: **Settings**, then **Collaborators** (left menu), then **Add people**.
2. Enter `GITHUB_USERNAME` and confirm.

## Step 10: Render your site for the first time

1. In RStudio with your Project open, find the **Build** pane (top right; if you do not see it, click Build in the menu bar) and click **Render Website**.
2. RStudio creates a folder called `_site` containing the finished web pages and shows the home page in the Viewer.
3. If Render stops with "there is no package called ...", install that package (Step 3 pattern) and render again.
4. In the template, the code chunks that load packages start switched off (`eval: false`). Once Step 3 is done, change `false` to `true` in those chunks so your code runs.

## Step 11: Commit and push

1. Click the **Git** tab (top right pane). Every changed file is listed.
2. Tick the **Staged** box next to each file (including the `_site` folder: Netlify publishes exactly what is in there).
3. Click **Commit**, write one short sentence describing the change (for example "first render of the site"), click **Commit**, close the window.
4. Click **Push** (green arrow). If a login appears, paste your token (Step 8).
5. Refresh your repository page on GitHub: your files are there.

The `data` folder and any `.csv` file never appear in the Git tab: the template's `.gitignore` file keeps them out on purpose. Leave that as it is.

## Step 12: Create your Netlify site from the repository

1. Go to https://app.netlify.com and choose **Sign up with GitHub**. This links the two accounts in one step.
2. Click **Add new project**, then **Import an existing project**, then **GitHub**. Authorize Netlify when asked and pick your repository.
3. In the settings screen: leave **Build command** empty, set **Publish directory** to `_site`. Click **Deploy**.
4. Netlify gives your site a random name. Change it under Project configuration (also called Site configuration), General, Site details, **Change site name**.
5. Open the address Netlify shows. Your site is live.

Because the site is imported from GitHub, every push republishes it automatically. If you ever see "Page not found" after a push, the usual reason is that `_site` was not committed: render, stage `_site`, commit, push.

## Step 13: The rhythm from here

Every working session: open the `.Rproj`, edit, **Render Website**, **Commit**, **Push**. Small pushes often beat one giant push before a deadline, and the commit history is part of how your work is graded.

## Step 14: Screenshots for the Software Pre-Flight ticket

1. RStudio with a successfully rendered page in the Viewer.
2. Your GitHub profile page.
3. Your repository's Collaborators page showing Dr. B added.
4. Your live Netlify site in a browser, address visible.

## Troubleshooting

- **"there is no package called X"**: run `install.packages("X")` and try again.
- **RStudio keeps asking for a GitHub password**: your token expired or was not stored. Repeat Step 8.
- **Push rejected**: someone (usually you, on another computer) pushed changes your copy does not have. Click **Pull** first, then Push.
- **Render fails at a code chunk**: read the first line of the error; it names the chunk and the problem. Fix that chunk; do not delete it.
- **Netlify shows an old version**: check that the last commit reached GitHub (refresh the repository page) and that `_site` is in that commit.
- **Everything is broken and you do not know why**: post in the Dr. B Learning Arena with the exact error text. Screenshots of red text help.
