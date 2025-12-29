Termux setup for ALPARSLAN (Android)

This guide describes how to run ALPARSLAN from Termux on an Android device without removing any features.

Prerequisites (Termux):
- Termux app installed
- Internet connection

1) Install essential packages

```bash
pkg update
pkg upgrade -y
pkg install -y git nodejs python ffmpeg
```

2) Install yt-dlp (recommended via pip)

```bash
pip install --upgrade pip
pip install yt-dlp
```

3) Grant Termux access to storage (so downloads can be saved to shared storage)

```bash
termux-setup-storage
# This will create ~/storage and a symlink to /storage/emulated/0
```

4) Clone and install the project

```bash
# If you haven't cloned already:
git clone https://github.com/<your-repo>/ALPARSLAN.git
cd ALPARSLAN/ALPARSLAN-main
npm install
```

5) Recommended: configure the app to use a folder on shared storage
- Open the app settings in the web UI and set **Output Folder** to one of these paths:
  - `/storage/emulated/0/ALP/`
  - `~/storage/shared/ALP/`

6) Start the backend server or both servers

```bash
# Run backend only (serves API + frontend dist if built)
npm run server

# Or run frontend dev + server together for development
npm run dev:all
# For Vite dev server to be reachable on the device browser, run:
npm run dev -- --host
```

7) Open the UI in the Android browser
- If running locally on Termux on the same device: open http://localhost:8080
- If you started Vite dev server on port 5173 and used `--host`: open http://localhost:5173

8) Verify system info endpoint (useful for debugging)
- Visit: http://localhost:8080/api/sysinfo
- The response will show Node, Python, yt-dlp, and ffmpeg availability.

Notes & Troubleshooting
- If "Failed to fetch metadata" appears, check the Termux server logs to see whether Python/yt-dlp is present or whether yt-dlp produced an error.
- Ensure `yt-dlp` is executable and visible in PATH. You can run `yt-dlp --version` or `python3 -m yt_dlp --version`.
- If downloads require writing to shared storage, ensure you used `termux-setup-storage` and set the output folder to a path under `/storage/emulated/0/`.

Optional: If you want to make the server report errors more verbosely, view the terminal running `npm run server` for stack traces.
