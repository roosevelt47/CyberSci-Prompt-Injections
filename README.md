# CyberSci Crash Course on Prompt Injections

A **deliberately vulnerable** FastAPI service that fronts a
[llama.cpp](https://github.com/ggml-org/llama.cpp) server, built for a hands-on
workshop on prompt injection attacks.

**Slides:** [CyberSci-Prompt-Injections.pdf](CyberSci-Prompt-Injections.pdf)
(the exercises are on the "Exercise Time" slides near the end).

**This fork** only fixes the setup so it works out of the box on Windows and Mac.
None of the exercise content is changed. Original repo:
[Simple-Networks/CyberSci-Prompt-Injections](https://github.com/Simple-Networks/CyberSci-Prompt-Injections).

## Quick start

```bash
git clone https://github.com/roosevelt47/CyberSci-Prompt-Injections.git
cd CyberSci-Prompt-Injections
docker compose up --build
```

Then open **http://localhost:8000**. The first run takes 5 to 10 minutes because it
downloads a ~700MB model. See the FAQ below if it looks stuck.

Works the same way on **Windows**, **macOS**, and **Linux**. No admin rights and no
special setup needed on any of them.

## What you need

- **Docker Desktop** installed and running. Install it *before* the workshop: the
  installer needs admin rights and may ask you to restart. Get it at
  [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/).
- **~1.5GB free disk** (one-time model download plus built images).
- **A few GB of free RAM.** 8GB total system RAM works. 16GB is comfortable if you're
  also on a call or screen-sharing.
- **No GPU.** Runs on CPU, on basically any laptop from the last ~10 years.

## FAQ

**Does this actually work on a MacBook?**
Yes, both Intel and Apple Silicon (M1/M2/M3/M4). The vendored AI engine binary is
x86-64 only, so on Apple Silicon Docker runs it through its built-in Rosetta
translation layer instead of natively. It is a little slower to start, but correct.
This fork pins that explicitly so it can't silently break. *(Not yet verified on real
Mac hardware. If `docker compose up --build` gives you trouble, tell your organizer.)*

**Do I need admin rights?**
Only to install Docker Desktop (one time, do it before the session). Cloning and
running the lab doesn't need admin.

**Do I need to turn anything on in Windows settings first?**
No. The original setup needed Windows "Developer Mode" turned on. This fork doesn't.
Just clone and run.

**It's stuck on `llama Building` or downloading forever. Is it broken?**
No, that's normal. The download's progress bar doesn't render properly without a
real terminal, so it looks frozen. Give it 5 to 10 minutes on a first run.

**How much does this download, and how much RAM/CPU does it use?**
- ~700MB model download, one time only (cached, so later runs skip it).
- ~420MB of Docker images.
- Around 1.6GB RAM while idle, and roughly one CPU core for a couple of seconds per
  question.

**I get an error like `file too short`, or the `llama` container keeps restarting.**
You're probably on the *original* repo, not this fork. The original has a
Windows-specific problem where some files get broken during `git clone`. Use this
fork's URL above instead.

**My Docker Desktop / Windows RAM usage keeps climbing the longer it runs.**
Docker's background VM (shows as `Vmmem` in Task Manager) doesn't always give memory
back on its own. Close Docker Desktop, then run this in a normal (non-admin)
PowerShell or Command Prompt window: `wsl --shutdown`. Reopen Docker Desktop after.

**How do I stop it when I'm done?**
Press `Ctrl+C` in the terminal running it, then run `docker compose down`. To also
delete the downloaded model and free the disk space, run `docker compose down -v`.

**Can I edit the app's code to solve the exercises?**
No. The exercises are about interacting with the app as it is (see the slides), not
modifying it.

**Where are the answers / walkthrough?**
Not here on purpose. Your organizer covers those live in the session.

## Credits

Workshop content, slides, and challenge design by Julian Eckhardt / CyberSci:
[Simple-Networks/CyberSci-Prompt-Injections](https://github.com/Simple-Networks/CyberSci-Prompt-Injections).
This fork adds cross-platform build fixes only.
