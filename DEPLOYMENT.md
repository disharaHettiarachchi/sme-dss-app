# Deployment Guide

Step-by-step instructions for deploying the SME DSS app to Streamlit Community Cloud via GitHub.

## ✅ Pre-flight checklist

Before you start, confirm you have:

- [ ] A GitHub account
- [ ] Git installed locally (`git --version` should work)
- [ ] All these files in your project folder:

```
sme-dss-app/
├── app.py                       ✓ Streamlit dashboard
├── requirements.txt             ✓ Pinned dependencies (sklearn 1.6.1, TF 2.18)
├── runtime.txt                  ✓ python-3.11
├── README.md                    ✓ Project documentation
├── LICENSE                      ✓ MIT
├── .gitignore                   ✓ Excludes dataset and caches
├── .streamlit/config.toml       ✓ Theme + server config
└── outputs/                     ✓ Pre-trained models (8.8 MB total)
    ├── models/                  ✓ 7 .joblib + .keras files
    ├── figures/                 ✓ 7 PNG charts
    ├── rfm_segments.csv         ✓ 5,819 customer segments
    ├── cluster_profile.csv      ✓ Segment summary
    ├── segmentation_metrics.json
    ├── forecasting_metrics.json
    └── anomaly_metrics.json
```

**Total deployment size:** ~9 MB — well within GitHub's free-tier limits.

---

## 🚀 Step 1 — Test locally first

Always test before deploying:

```bash
cd path/to/sme-dss-app

# Create a clean virtual environment
python3.11 -m venv venv
source venv/bin/activate         # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run the app
streamlit run app.py
```

Browser opens at `http://localhost:8501`. Click through all 6 pages. Verify:
- Overview page shows KPI cards
- Customer Segments page shows the 4 clusters
- Sales Forecast page shows the model comparison
- Anomaly Alerts page shows metrics
- Upload Your Data page accepts a CSV (test with a small subset of the original dataset)

If anything fails locally, **fix it now** — debugging on Streamlit Cloud is slower.

---

## 🐙 Step 2 — Create the GitHub repo

### 2a. Initialise git locally

```bash
cd path/to/sme-dss-app
git init
git add .
git status     # ← verify outputs/models/*.joblib appear in the staged list
git commit -m "Initial commit: SME DSS Streamlit app with trained models"
git branch -M main
```

### 2b. Create the empty repo on GitHub

1. Go to https://github.com/new
2. Repository name: `sme-dss-app` (or any name you prefer)
3. Set to **Public** ⚠️ Streamlit Cloud free tier requires public repos
4. **Do NOT** initialize with README, .gitignore, or licence (you have them)
5. Click **Create repository**

### 2c. Push your local repo to GitHub

GitHub shows the commands on the new repo page. Run them:

```bash
git remote add origin https://github.com/YOUR_USERNAME/sme-dss-app.git
git push -u origin main
```

If prompted for credentials and you use 2FA, generate a Personal Access Token:
1. https://github.com/settings/tokens → **Generate new token (classic)**
2. Tick the `repo` scope
3. Copy the token and paste it as your "password" when prompted

### 2d. Verify on GitHub

Open `https://github.com/YOUR_USERNAME/sme-dss-app` in a browser. You should see:
- All your files visible
- README rendering with badges and the live demo placeholder

---

## ☁️ Step 3 — Deploy to Streamlit Community Cloud

### 3a. Connect your GitHub

1. Go to https://share.streamlit.io
2. Click **Sign in with GitHub**
3. Authorise Streamlit to read your repos

### 3b. Create the app

1. Click **New app** (top-right)
2. Fill in:
   - **Repository:** `YOUR_USERNAME/sme-dss-app`
   - **Branch:** `main`
   - **Main file path:** `app.py`
   - **App URL:** pick a custom subdomain like `sme-dss-demo`
3. Expand **Advanced settings**
   - **Python version:** `3.11`
   - (no secrets needed for this app)
4. Click **Deploy!**

### 3c. Watch the build

The right-hand pane shows live logs:

```
[XX:XX:XX] 🐍 Python 3.11 set up
[XX:XX:XX] 📦 Installing dependencies (this takes 3-5 min)
[XX:XX:XX]    Downloading tensorflow-cpu-2.18.0...
[XX:XX:XX]    Downloading scikit-learn-1.6.1...
[XX:XX:XX] ✅ Dependencies installed
[XX:XX:XX] 🚀 Starting app...
[XX:XX:XX] App is running!
```

**Typical first deploy: 4–7 minutes** (TensorFlow install is the slow part).

When it finishes, your app is live at `https://sme-dss-demo.streamlit.app`.

---

## 📸 Step 4 — Capture screenshots for the research paper

Open your deployed app and take screenshots of each page:

| Page | Filename | Replaces Figure |
|---|---|---|
| Overview | `screenshot_overview.png` | Figure 11 |
| Customer Segments | `screenshot_segments.png` | Figure 12 |
| Sales Forecast | `screenshot_forecast.png` | Figure 13 |
| Anomaly Alerts | `screenshot_anomaly.png` | Figure 14 |
| Upload Your Data | `screenshot_upload.png` | Figure 15 |

**Tips for clean screenshots:**
- Use full browser width (1400px+ ideal)
- Hide bookmarks bar
- Use a clean, light-themed browser window
- On macOS: `Cmd+Shift+4` then drag · On Windows: Snipping Tool
- Save as PNG, not JPEG

### Replace placeholders in the research paper

1. Open `Full_Research_Paper.docx` in Microsoft Word
2. Navigate to Chapter 4, Section 4.6
3. **Right-click** on each placeholder image → **Change Picture** → **From a File...**
4. Select the corresponding screenshot
5. The caption stays unchanged

### Add the live URL to the paper

In Chapter 4 Section 4.6, find and replace:
- `[INSERT YOUR STREAMLIT APP URL HERE]` → `https://sme-dss-demo.streamlit.app`
- `[INSERT YOUR GITHUB REPOSITORY URL HERE]` → `https://github.com/YOUR_USERNAME/sme-dss-app`

### Update the README too

In `README.md`, replace `YOUR-APP-NAME.streamlit.app` with your actual subdomain (4 occurrences):

```bash
sed -i 's/YOUR-APP-NAME.streamlit.app/sme-dss-demo.streamlit.app/g' README.md
git add README.md
git commit -m "Add live demo URL to README"
git push
```

(On Windows / without `sed`, just edit the file manually.)

---

## 🔄 Step 5 — Iterate

Any change you make locally:

```bash
git add .
git commit -m "Describe your change"
git push
```

Streamlit Cloud auto-redeploys within ~60 seconds. Subsequent deploys are faster (~1 min) because dependencies are cached.

---

## 🚨 Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| Build hangs at "Installing dependencies" | TF download timing out | Wait — first build is genuinely slow. If >15 min, redeploy. |
| `ModuleNotFoundError: No module named 'X'` | Missing package | Add `X==1.2.3` to `requirements.txt`, commit, push |
| `InconsistentVersionWarning: ... sklearn 1.6.1 ... 1.5.x` | sklearn version mismatch | Already pinned to 1.6.1 in requirements — should not happen with provided config |
| `OSError: SavedModel file does not exist` | LSTM model didn't get committed | Check `git ls-files outputs/models/` — if `lstm.keras` missing, your `.gitignore` is wrong |
| App boots but says "Trained models not found" | `outputs/` folder excluded by gitignore | Check `.gitignore` doesn't contain `outputs/` — only `online_retail_II.xlsx` should be excluded |
| `ResourceExhaustedError` from TensorFlow | Memory limit hit | Confirm you're using `tensorflow-cpu` not `tensorflow` |
| App URL returns 404 | App was deleted or sleeping | Streamlit Cloud sleeps inactive apps; visit URL to wake it up (takes ~30s) |
| `streamlit: command not found` (local) | Virtual env not activated | `source venv/bin/activate` |

---

## 🎓 Optional polish

After deployment is working:

1. **Add Open Graph metadata** to make link previews look good when sharing
2. **Pin the app** in Streamlit Cloud dashboard so it never sleeps (paid tier)
3. **Add Google Analytics** via `.streamlit/config.toml` to track usage
4. **Set up GitHub Actions** to run `pytest` on every push
5. **Add a `CITATION.cff` file** so GitHub auto-generates citation snippets

---

## 📞 Where to get help

- Streamlit Forum: https://discuss.streamlit.io (most issues already answered)
- Streamlit Cloud Status: https://www.streamlitstatus.com
- GitHub Issues: open one in your own repo for project-specific questions

Good luck with the deployment! 🚀
