> **⚠️ DISCLAIMER**: This repository is **NOT** intended to bypass, abuse, or violate GitHub Actions Terms of Service. This workflow is designed **SOLELY** for legitimate development purposes including:
>
> - Interactive debugging and troubleshooting of Windows-specific build failures
> - Compiling projects that require GUI interaction or manual intervention
> - Testing complex Windows applications that cannot be fully automated
> - Validating build scripts and configurations when remote desktop access is needed for diagnosis
> - Quick prototyping and experimentation on Windows environments
>
> **You should stop the VM at any time when your work is finished using the `Cancel Workflow` button in the running Actions, or else there is a risky chance of your account suspension due to abuse of free GitHub compute.**
>
> This tool is meant to be used **responsibly** for development workflows where automated CI/CD pipelines alone are insufficient. The author does not condone or support any misuse of GitHub Actions resources. 
---

## What is this tool?

A GitHub Actions workflow that provisions a temporary Windows environment with remote access capabilities. It's for those moments when you've spent **hours** trying to automate some bullshit that just won't cooperate, and you need to get hands-on with the actual Windows environment to figure out what tf is going on and finally find the issue after HOURS of working for absolutely no reason.

**Think of it as:** "I've tried 47 different YAML configurations, none of them work, and I just want to compile my source code that takes a lot of time on my own home PC and find out what's wrong with my code compiling."

---

## How to use?
Fork this repository, configure repository secrets (mentioned below), and run `CI-RDP` workflow.

## Features

| Feature | Why |
|---------|-----|
| **Chrome + VS Code + Utilities** | Full development environment because you need actual tools |
| **Sysinternals Suite** | Why not? |
| **Tailscale** | Secure remote access without exposing ports to the entire internet |
| **RDP + SSH** | Multiple ways to connect because sometimes one (or sometimes your internet) just decides to shit itself |
| **Performance Optimizations** | Disables unnecessary bloat for a snappy experience |
| **Extended Runtime** | 5h 50m - enough time to debug without GitHub killing the job |
| **Dark Theme** | Because MY EYES ARE BURNING |

---

## Requirements

- GitHub repository with Actions enabled
- Two GitHub Secrets configured (see below)
- A legitimate need to access a Windows / Linux runner (don't be *that* guy)

---

## Setup

### 1. Add Repository Secrets

Go to your repository → Settings → Secrets and variables → Actions → Add new repository secret:

| Secret Name | Description |
|-------------|-------------|
| `TAILSCALE_AUTH_KEY` | Your Tailscale auth key (get from Tailscale admin console) |
| `RDP_PASSWORD` | Password for the Windows / Linux user (8+ characters, make it something good) |
|`GITHUB_TOKEN`| Your GitHub Classic token access |

Uh, your RDP password doesn't matter at all lol because GitHub Actions **do not have internet-routable IP addresses**, yeah just put something good for it.

### 2. Run the Workflow

1. Go to Actions tab
2. Select "CI-RDP-Windows" or "CI-RDP-Linux" workflow
3. Click "Run workflow"
4. Wait **~4 minutes** for setup (go grab a coffee or some shi)
5. Check workflow logs for connection details

### 3. Connect

The workflow logs will display something like:
```
✅ RDP Ready - 100.64.0.1:3389 (THE REAL IP IS DIFFERENT)
Username: runneradmin (or runner if Linux)
Password is set in Repository Secrets (hope you haven't forgotten it bro)
Connect via SSH: ssh runneradmin@100.64.0.1
```

Connect via:
- **RDP**: Use Windows Remote Desktop Client to `100.64.0.1:3389`
- **SSH**: `ssh runneradmin@100.64.0.1` (or runner if Linux) [password from `RDP_PASSWORD`]
- **Tailscale**: The machine will appear in your Tailscale network

> The real IP you use to connect is **dynamic** and is **not 100.64.0.1**

---

## When To Use This

✅ **Appropriate use cases:**
- Debugging a Windows-specific compiler error you can't replicate locally
- Testing GUI applications in a clean Windows environment
- Validating complex build scripts interactively
- Investigating cryptic errors in automated pipelines
- Teaching yourself Windows administration/debugging
- When you've tried automating something for 6 hours and want to throw your laptop out the window

❌ **Inappropriate use cases:**
- Running crypto miners or resource-intensive workloads (don't ever think of crypto miners or scanners, GitHub loves banning accounts for this)
- Hosting public services or websites
- Using as a permanent VM replacement
- Sharing access with unauthorized users
- Any activity that violates [GitHub Actions ToS](https://docs.github.com/site-policy/github-terms/github-terms-of-service)

---

## Important Notes

- **Temporary**: The runner exists for **~6 hours MAXIMUM** and then nukes itself
- **Cancellable**: You can stop it at any time using `Cancel Workflow` button.
- **Self-destructing**: All data is lost when the job completes (poof, gone)
- **Not a free VM**: This is a debugging tool, not a free Windows instance
- **Use responsibly**: Don't be the person that ruins GitHub Actions for everyone else

---

## Known Problems

| Issue | Status |
|-------|--------|
| WSL requires reboot for full kernel features | Works without reboot for most shit and stuff |
| Ghostery extension manual install | Still figuring this out, don't ask me why it's not installed |
| ~5 minute setup time | Acceptable for debugging purposes |

> Install **Ghostery** yourself below 👇

[![Install on Chrome Web Store](https://img.shields.io/badge/Install%20on-Chrome%20Web%20Store-4285F4?style=for-the-badge&logo=googlechrome&logoColor=white)](https://chromewebstore.google.com/detail/ghostery-adblocker-for-pr/mlomiejdfkolichcflejclcbmpeaniij)

---

## GitHub's Perspective

We understand this tool operates in a sensitive area. Here's how we address concerns:

| Concern | Our Response |
|---------|--------------|
| **Resource abuse** | Sessions are limited to 6 hours and require explicit use case selection |
| **ToS violation** | We only use runners for their intended purpose: debugging and testing software |
| **Account risk** | We strongly advise against overuse and recommend using sparingly |


---

## Contributing

Found an issue? Have a suggestion to make this more useful for legitimate development? Open an issue or PR.

---

## License

MIT - Use responsibly, don't be stupid.

---

## FAQ

**Q: Isn't this just RDP farming?**  
A: No. RDP farming implies malicious intent - using resources for unauthorized bullshit. This is a development tool for legitimate debugging. The difference is intent and usage.

**Q: Will GitHub ban me?**  
A: If you use this responsibly (sparingly, for actual debugging), probably not. If you run this 24/7 with multiple concurrent jobs, yes, you're going to have a bad time.

**Q: Why not just use a local VM?**  
A: Some issues only reproduce on GitHub's specific runner environment. Hardware variations, OS versions, and pre-installed software all differ from local setups. Sometimes you need the exact environment.

**Q: Why specifically 5h 50m??**  
A: GitHub's hard timeout is 6 hours. This exits cleanly before that to avoid showing as "failed" and cluttering your Actions dashboard with red marks. It's not a conspiracy, it's just not wanting to look at failed workflows.

**Q: Why disable Windows Search if you installed Everything?**  
A: Because Windows Search is actual garbage and Everything is 1000x better. If you've ever used Windows Search you already know this.

**Q: WinRAR AND 7-Zip? Really?**  
A: Yes. Both are good. I'm not changing my mind, also someone might not like WinRAR, someone might not like 7-Zip, we gotta respect everyone's choice ✌️

---

**Remember:** With great power comes great responsibility. Use this shit respectfully and only when you genuinely need it. Don't be the reason GitHub nerfs Actions for everyone.
