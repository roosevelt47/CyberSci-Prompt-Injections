# CyberSci Crash Course on Prompt Injections

A **deliberately vulnerable** FastAPI service that fronts a
[llama.cpp](https://github.com/ggml-org/llama.cpp) server, built for a hands-on
workshop on prompt injection attacks.

**Slides:** [CyberSci-Prompt-Injections.pdf](CyberSci-Prompt-Injections.pdf)
(the exercises are on the "Exercise Time" slides near the end).

**This fork** only fixes the setup so it works out of the box on Windows and Mac.
None of the exercise content is changed. Original repo:
[Simple-Networks/CyberSci-Prompt-Injections](https://github.com/Simple-Networks/CyberSci-Prompt-Injections).

## Prerequisites (install before the workshop)

1. **Docker Desktop** (required). Get it at
   [docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop/).
   The installer needs admin rights and may ask you to restart, so don't leave it
   for the day of. Open it once after installing and make sure it says it's running.
2. **Python 3** (for the exercise, you'll run a small web server with it).
   Check with `python --version` (Windows) or `python3 --version` (Mac).
   Get it at [python.org/downloads](https://www.python.org/downloads/) if missing.
3. **Git** (optional). Without git, use the green **Code > Download ZIP** button on
   this page and unzip it instead of `git clone`. Both work.

Your machine needs **~1.5GB free disk** and **8GB+ RAM** (16GB is comfortable if
you're also on a call). No GPU needed.

## Quick start

```bash
git clone https://github.com/roosevelt47/CyberSci-Prompt-Injections.git
cd CyberSci-Prompt-Injections
docker compose up --build
```

Then open **http://localhost:8000**. The first run takes 5 to 10 minutes because it
downloads a ~700MB model. Do this before the workshop so you're not downloading on
shared wifi. See the FAQ below if it looks stuck.

Works the same way on **Windows** and **macOS**. No admin rights and no special
settings needed to run it.

## Running a web server for the exercise

The slides hint that you'll need a web server the app can reach. Run it from any
folder containing the files you want to serve:

```bash
# Windows
python -m http.server 8080 --bind 127.0.0.1

# Mac
python3 -m http.server 8080 --bind 127.0.0.1
```

Then give the app URLs like **`http://host.docker.internal:8080/yourfile.txt`**,
not `http://localhost:8080/...`. The app runs inside a Docker container, and inside
it `localhost` means the container itself, not your laptop. `host.docker.internal`
is the name Docker Desktop gives your laptop from inside a container.

`--bind 127.0.0.1` keeps the server private to your machine and avoids the Windows
Firewall popup.

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

**`docker` command not found, or "cannot connect to the Docker daemon".**
Docker Desktop isn't installed or isn't open. Start Docker Desktop, wait until it
says it's running, then try again.

**It's stuck on `llama Building` or downloading forever. Is it broken?**
No, that's normal. The download's progress bar doesn't render properly without a
real terminal, so it looks frozen. Give it 5 to 10 minutes on a first run.

**The app says `could not fetch URL: ... Connection refused`.**
You probably used `localhost` in the URL. Use `host.docker.internal` instead (see
"Running a web server for the exercise" above), and check your Python web server is
still running.

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
