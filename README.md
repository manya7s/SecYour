# SecYour SDK Demo (Windows + Android Studio Setup)

This repository demonstrates how to run the SecYour SDK demo app on **Windows OS** using **Flutter** and **Android Studio**.
 

SecYour is a cross-platform **behavioral fraud detection and trust scoring system** for banking applications. It continuously monitors user interaction patterns and device signals to detect anomalies, compute a dynamic **trust score**, and trigger **real-time security actions**.  

<img src="architecture.jpg">



## Features  

- **Dynamic Trust Score (0–100)** updated in real-time  
- **TFLite NN Model** (`model_lite.tflite`) predicts expected session trust score  
- **Behavioral Penalties** applied for risky actions (failed PINs, OTP skips, large transactions, etc.)  
- **User Baseline Profiles** created statistically from user sessions for personalized thresholds  
- **Popups when risk is detected**:  
  - ≤20 → **Force Logout**  
  - ≤40 → **OTP Verification**  
  - ≤70 → **Security Question**  
- **Session Log Export** to JSON (secure local storage)  
- **Device & Location Tracking** for contextual analysis  

---

## How It Works  

1. **Session Tracking**
   - Onboarding session logs first usage pattern.
   - SDK logs taps, swipes, navigation, screen visits, transactions, and timing data.  
   - Device info, location, and session metadata are collected.  

3. **ML Regression Model**  
   - Features (e.g., session duration, penalties) are fed into a TensorFlow Lite regression model (`model_lite.tflite`).  
   - The model outputs an **expected trust score baseline**.  

4. **Deviation Detection**  
   - The **Behaviour Manager** compares the model’s prediction with the behavior-adjusted trust score.  
   - If the **deviation is significant**, the system assumes suspicious behavior.  

5. **Risk-Based Actions (Popups)**  
   - Large deviation or low trust score → popup is triggered:  
     - Logout  
     - OTP Verification  
     - Security Question  
   - Trust can be restored after successful re-authentication.  

6. **Baseline Learning**  
   - Every user session, SecYour rebuilds the **user profile baseline** (average tap/swipe speeds).  
   - Future sessions are compared against this baseline for more personalized anomaly detection.  

7. **Log Export & Analytics**  
   - All session data is exported as JSON to `/sdcard/Download/SecYour/` for secure local storage.



## Components  

- **Flutter SDK** → Tracks behaviors, manages trust score, triggers popups  
- **TFLite NN Model (`model_lite.tflite`)** → Predicts baseline trust score  
- **Behaviour Manager** → Applies penalties, compares with ML, enforces actions   



## Tech Stack  

- **Frontend (SDK)**: Flutter (cross-platform: Android, iOS, Web, Desktop)  
- **ML Model**: TensorFlow → TensorFlow Lite (TFLite) NN model  

---

## How to Run

### 1. Clone the Repository

Use Git to clone the project:

```bash
git clone https://github.com/manya7s/SudoCode-PhishSafe.git
```


---
### 2. Link the SDK to the Demo App

In your demo app's `pubspec.yaml` file, add a path reference to the local SDK folder:

```yaml
dependencies:
  phishsafe_sdk:
    path: ../<path-to-your-SecYour-SDK-folder>
```

### 3. Install Dependencies for the SecYour SDK

Navigate to the SDK folder and run in terminal:

```bash
flutter pub get
```

Navigate to the Demo app folder and run in terminal:

```bash
flutter pub get
```

This will fetch all required packages for the SecYour SDK and Demo application.


Make sure the relative path is correct based on your folder structure.



### 4. Fix TFLite Tensor Error (Important)

Manually modify a file in the pub cache to fix a TensorFlow Lite compatibility issue.

**Navigate to:**

```
C:\Users\<your-username>\AppData\Local\Pub\Cache\hosted\pub.dev\tflite_flutter-0.10.4\lib\src\tensor.dart
```

**Steps:**

1. Open the file in any text or code editor.
2. Press `Ctrl + F` and search for:

   ```
   UnmodifiableUint8ListView
   ```

3. Replace it with:

   ```dart
   Uint8List.fromList
   ```

4. Save the file.

This fixes runtime issues when loading models using TFLite.



## Requirements to run prototype

- Flutter SDK installed
- Dart SDK installed
- Android Studio with Flutter and Dart plugins
- Working Android emulator or connected device

You can verify your setup by running:

```bash
flutter doctor
```

---

## Support

If you face issues:

- Double-check the SDK path in `pubspec.yaml`
- Make sure you've run `flutter pub get` inside the SDK
- Verify that you’ve fixed the `tensor.dart` issue correctly

---

## License

This project is intended for demo and academic use. License terms may apply depending on your use case.
