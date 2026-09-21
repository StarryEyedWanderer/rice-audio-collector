Rice Audio Collector

What it does
- Records one continuous microphone stream.
- Runs a frozen bootstrap rice-mass model locally in the browser.
- Auto-detects sound events and shows a live/final gram prediction.
- Does NOT add an event to the dataset until you choose a tolerance label, Perfect, exact grams, Completely wrong, or Skip.
- Every saved row includes start/end offsets into the continuous recording.
- Exact gram values are kept as exact labels; tolerance feedback is kept as an interval label.

Recommended Android setup
1. Host this folder on any HTTPS static host (GitHub Pages works well).
2. Open the HTTPS URL in Chrome on the phone.
3. Allow microphone access.
4. Use Chrome's menu -> Add to Home screen / Install app.
5. After the first load, the service worker caches the app for offline use.

Important
- Microphone APIs normally require HTTPS or localhost. Opening index.html from an insecure http:// LAN URL may not work.
- The app requests echo cancellation, noise suppression, and automatic gain control OFF, but the phone/browser may ignore some of those requests. The actual device settings are captured in the session internally for future extension.
- The bootstrap model is deliberately frozen. Your newly labeled data are exported for later offline retraining; the model does not update during collection.

Session workflow
1. Start continuous recording.
2. Pour normally. When an acoustic event ends, the app freezes a prediction and waits.
3. Grade it with Right within 10/5/2 g, Perfect, exact grams, Completely wrong, or Skip.
4. Change distance/container/direction metadata whenever you move the phone or setup.
5. At the end, Stop session, then download the CSV and continuous audio file.
6. Keep the CSV and audio file together; the CSV start/end timestamps refer to that recording.

Bootstrap model
- Two frozen random-forest models selected by the Direction control.
- cup->bowl model: 162 examples; masses 5, 10, 20, 25, 50, 52, 66, 100, 108, 150 g.
- bowl->cup model: 61 return-pour examples; masses 5, 20, 52, 66, 108 g.
- This is a data-collection bootstrap, not a validated kitchen-scale replacement.

Feedback semantics
- Completely wrong = false-positive event/prediction. It is saved with feedback_type=false_positive and no gram target, so it can train a future event detector as negative audio.
- Skip = do not judge this candidate; keep the timestamp for audit, but do not treat it as positive or negative supervision.
