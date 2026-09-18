# WalkerAI-Independent-

Walker helps Deaf and Hard-of-Hearing users communicate through sign language.

**Mission:** move the user from **Gesture → Meaning**.

This repository intentionally stays lean while the local end-to-end pipeline
hardens.

## Current working structure

- `index.html` - browser prototype layout and pinned MediaPipe CDN includes
- `style.css` - browser prototype styling
- `app.js` - browser prototype face tracking + hand landmark detection
- `scripts/collect_sign_data.py` - Phase 1 local capture
- `scripts/analyze_sign_dataset.py` - dataset quality checks
- `scripts/train_sign_model.py` - Phase 2 local training
- `scripts/evaluate_sign_model.py` - model evaluation and threshold review
- `scripts/live_sign_inference.py` - Phase 3 live local inference bridge
- `config/sign_labels.json` - shared sign labels and capture defaults
- `config/training_config.json` - shared training and inference defaults
- `config/responses.json` - local response mapping for Moon Cake

This project uses only free, open-source, local resources.

## Browser prototype (LEGACY / EXPERIMENTAL / NOT PRODUCT PROOF)

A browser prototype of the core loop's INPUT and OUTPUT stages.

This section is legacy/experimental only. Camera access, hand detection,
landmarks, generic contour gestures, mouse-drawn gestures, a static or
animated avatar, status panels, and canned responses are not product proof and
must not be presented as Saudi Sign Language understanding.

- **Camera → hand landmarks** — the webcam feed is processed with MediaPipe
  Hands to detect 21 hand landmarks in real time, drawn as an overlay.
- **Avatar makes eye contact** — a simple avatar face uses MediaPipe FaceMesh
  to track your face and moves its eyes toward you, so Walker makes natural
  eye contact with the user.

### Run it

No build step. Because the app uses the webcam, browsers require a secure
context, so serve the folder over `http://localhost` (not `file://`):

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000 and click **Start camera**. Allow camera
access and look at the camera; the avatar's eyes follow your face.

The page itself runs locally, but the current prototype still fetches pinned
MediaPipe assets from jsDelivr, so the first load requires a network
connection.

## Phase 1 - Local Sign Capture (LEGACY / EXPERIMENTAL / NOT PRODUCT PROOF)

This repository includes legacy/experimental local capture instructions.
These instructions are not approved Saudi Sign Language implementation or
product proof.

### Setup

1. Create a local Python environment.
2. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

3. Review or edit the shared label config if needed:

   - `/home/runner/work/WalkerAI-Independent-/WalkerAI-Independent-/config/sign_labels.json`

### Run

```bash
python scripts/collect_sign_data.py
```

Optional custom labels:

```bash
python scripts/collect_sign_data.py --labels "1:Hello" "2:Meeting" "3:Price" "4:Yes" "5:No"
```

Optional longer samples:

```bash
python scripts/collect_sign_data.py --sequence-length 45
```

Require stronger samples:

```bash
python scripts/collect_sign_data.py --sequence-length 45 --min-valid-frames 30 --countdown-seconds 3
```

### Controls

- Press `1`, `2`, `3`, `4`, or `5` to arm a recording for the matching word.
- Wait for the countdown, then perform the sign.
- Press `c` to cancel a bad recording before it is saved.
- Press `u` to undo the last saved sample and remove it from the manifest.
- Press `q` to quit.

### Phase 1 improvements built in

- Countdown before capture for more consistent starts
- Automatic rejection of weak samples with too few valid hand-detection frames
- Stable left/right hand ordering for cleaner future training
- Shared label config as a single source of truth
- Per-frame handedness metadata in both JSON and CSV
- Portable manifest entries using paths relative to the dataset folder
- Local `collector_config.json` snapshot so Phase 2 knows how the data was
  captured
- Dataset outputs are kept local and ignored by git

### Check dataset quality before training

```bash
python scripts/analyze_sign_dataset.py
```

Optional stricter quality gate:

```bash
python scripts/analyze_sign_dataset.py --min-samples-per-label 20 --min-valid-ratio 0.9
```

The analyzer now also warns about:

- configured labels that are missing from the dataset
- unexpected labels that are not in `config/sign_labels.json`

### Output

Saved files are written locally under:

- `/home/runner/work/WalkerAI-Independent-/WalkerAI-Independent-/data/sign_language/manifest.csv`
- `/home/runner/work/WalkerAI-Independent-/WalkerAI-Independent-/data/sign_language/samples/<Label>/*.json`
- `/home/runner/work/WalkerAI-Independent-/WalkerAI-Independent-/data/sign_language/samples/<Label>/*.csv`

Each sample includes:

- JSON with the full frame sequence
- CSV with one row per frame, handedness metadata, and flattened landmark
  coordinates
- A manifest entry for later training in Phase 2
- `collector_config.json` with the Phase 1 capture settings

## Phase 2 - Local LSTM Training (LEGACY / EXPERIMENTAL / NOT PRODUCT PROOF)

This section is legacy/experimental only and is not evidence that Walker
understands Saudi Sign Language.

### Setup

Install the Phase 2 dependency set:

```bash
pip install -r requirements-phase2.txt
```

Shared training defaults live in:

- `/home/runner/work/WalkerAI-Independent-/WalkerAI-Independent-/config/training_config.json`

### Train

```bash
python scripts/train_sign_model.py
```

Recommended flow:

1. Record balanced samples for each sign.
2. Run `python scripts/analyze_sign_dataset.py`.
3. Fix any low-sample or low-quality warnings.
4. Train the model.

Example training command:

```bash
python scripts/train_sign_model.py --epochs 50 --batch-size 8 --validation-split 0.2 --export-tflite
```

### Phase 2 outputs

Saved locally under `models/sign_lstm/`:

- `best_model.keras`
- `final_model.keras`
- `final_model.tflite` if `--export-tflite` is used
- `labels.json`
- `training_summary.json`

### Evaluate the trained model

```bash
python scripts/evaluate_sign_model.py
```

This saves an evaluation summary with:

- overall accuracy
- accepted accuracy after confidence thresholding
- per-label precision, recall, and F1 score
- confusion matrix
- rejected low-confidence samples

## Phase 3 - Local Live Inference Bridge (LEGACY / EXPERIMENTAL / NOT PRODUCT PROOF)

This section is legacy/experimental only and is not product proof.

Run the trained model live from the webcam:

```bash
python scripts/live_sign_inference.py
```

What it does:

- reads webcam landmarks locally
- runs the trained TensorFlow CPU model locally
- applies confidence thresholding and smoothing
- maps recognized signs to local responses
- writes live state to `/home/runner/work/WalkerAI-Independent-/WalkerAI-Independent-/runtime/live_state.json`

This JSON file is the bridge for the future Moon Cake local web character
layer.

## Focus

The repo stays intentionally lightweight:

- no cloud APIs
- no paid subscriptions
- no Docker or Kubernetes at this stage
- no heavy platform refactor before the local pipeline proves itself

## Durable repository handoff (Phase 0.6)

VS Code/Copilot chat history is not treated as durable project memory.

- Chats are helpful context only.
- Repository files, Git commits, pull requests, issues, and tests are the
  durable source of truth.
- Checkpoint status in this clone:
  - `d2852d1` - UNVERIFIED / unavailable in this clone; do not use it as a
    baseline or verified checkpoint
  - `4e997f0` - verified reachable checkpoint in this clone

Use these tracked files before continuing work in a new session:

- `docs/agent-operating-rules.md`
- `docs/current-handoff.md`

## Roadmap

1. Saudi Sign Language recognition (not started; requires Isharah permission and trained model)
2. Intent recognition
3. Avatar response
4. Memory
5. Website deployment
6. Desktop companion
