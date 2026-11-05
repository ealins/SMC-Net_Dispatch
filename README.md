# SMC-Net Stuttgart Dispatch Dashboard

Single-file web app (`index.html`). No build step.

Host on GitHub Pages: Settings > Pages > Deploy from branch > main / root.

The HERE API key is not in the code; the app asks for it once and keeps it in the browser (localStorage). Without it, routing falls back to OSRM.
