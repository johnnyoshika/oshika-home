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

## Deployment

Revert Firebase CDN config with reserved URL config, then deploy:

```
firebase deploy
```
