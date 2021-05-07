# Home Pi Projects

## Dev

Replace reserved URL Firebase config with CDN:

```
  <script src="https://www.gstatic.com/firebasejs/8.5.0/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/8.5.0/firebase-firestore.js"></script>
  <script>
    var firebaseConfig = {
      apiKey: "",
      authDomain: "",
      projectId: "",
      storageBucket: "",
      messagingSenderId: "",
      appId: ""
    };
    firebase.initializeApp(firebaseConfig);
  </script>
```

Start server:

```
npx http-server public/
```

## Raspberry Pi

- `pip install firebase-admin`
- Download serviceAccountKey.json from firebase project settings page
- `export GOOGLE_APPLICATION_CREDENTIALS="serviceAccountKey.json"`
- `python <file>.py`

```
import firebase_admin
from firebase_admin import firestore
import threading
from signal import pause
from gpiozero import PWMLED

led17 = PWMLED(17)
led27 = PWMLED(27)

default_app = firebase_admin.initialize_app()
db = firestore.Client()

# Create an Event for notifying main thread.
callback_done = threading.Event()

# Create a callback on_snapshot function to capture changes
def on_snapshot(doc_snapshot, changes, read_time):
    for doc in doc_snapshot:
        led17.value = doc.to_dict()['led17']
        led27.value = doc.to_dict()['led27']
        print(f'Received document snapshot: {doc.id}')
    callback_done.set()

doc_ref = db.collection(u'devices').document(u'pi-no-1')

# Watch the document
doc_watch = doc_ref.on_snapshot(on_snapshot)

pause()
```

## Deployment

Revert Firebase CDN config with reserved URL config, then deploy:

```
firebase deploy
```
