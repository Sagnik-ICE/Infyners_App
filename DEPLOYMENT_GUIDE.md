# Deployment & Testing Guide

## 🏃 Quick Start

### 1. Install Dependencies
```powershell
flutter pub get
```

### 2. Run the App
```powershell
# Android emulator or device
flutter run

# Web (development)
flutter run -d chrome

# iOS (Mac only)
flutter run -d ios
```

---

## 🔥 Firebase Configuration

### Required Steps

1. **Firestore Security Rules**
   
   Go to Firebase Console → Firestore → Rules and copy/paste from `firestore.rules`:
   
   ```javascript
   // Complete rules in firestore.rules file
   // Essential points:
   // ✅ Admin-only writes for notices/exams/routines
   // ✅ Users can update their own profiles with specific fields
   // ✅ Comments include userName and profilePicUrl
   // ✅ Profile pictures stored in users collection
   // ✅ Vote documents restricted to owner
   ```

2. **Firebase Storage Rules**
   
   Go to Firebase Console → Storage → Rules and copy/paste from `storage.rules`:
   
   ```javascript
   // Complete rules in storage.rules file
   // Essential points:
   // ✅ notice_attachments/{userId}/ - for PDF/DOC uploads
   // ✅ profile_pictures/{userId}/ - for user photos
   // ✅ Public read access for all files
   // ✅ Users can only upload to their own folders
   ```

3. **Create Admin User**
   
   Firebase Console → Firestore → Add Document:
   ```
   Collection: admins
   Document ID: [user_uid_from_auth]
   Field: isAdmin = true
   ```

---

## 📱 Platform-Specific Setup

### Android

1. **Minimum SDK**: Already set to 21 in `android/app/build.gradle.kts`

2. **Permissions**: Already configured in `AndroidManifest.xml`
   - Internet
   - Notifications (POST_NOTIFICATIONS for Android 13+)
   - Read external storage (for file picker)

3. **Google Services**: Ensure `google-services.json` is in `android/app/`

4. **Build APK**:
   ```powershell
   flutter build apk --release
   ```
   Output: `build/app/outputs/flutter-apk/app-release.apk`

### iOS (Mac only)

1. **Pod Install**:
   ```bash
   cd ios
   pod install
   cd ..
   ```

2. **Firebase Configuration**: Ensure `GoogleService-Info.plist` is in `ios/Runner/`

3. **Build IPA**:
   ```bash
   flutter build ios --release
   ```

### Web

1. **Firebase Config**: Already in `lib/firebase_options.dart`

2. **Build**:
   ```powershell
   flutter build web --release
   ```
   Output: `build/web/`

3. **Deploy**: Upload `build/web/` to hosting (Firebase Hosting, Netlify, etc.)

---

## 🔔 Cloud Functions Setup (Optional but Recommended)

### Install Firebase CLI
```powershell
npm install -g firebase-tools
firebase login
```

### Initialize Functions
```powershell
firebase init functions
# Choose JavaScript or TypeScript
# Install dependencies: Yes
```

### Create Notification Triggers

Edit `functions/index.js`:
```javascript
const functions = require('firebase-functions');
const admin = require('firebase-admin');
admin.initializeApp();

// Notice notification
exports.sendNoticeNotification = functions.firestore
  .document('notices/{noticeId}')
  .onCreate(async (snap, context) => {
    const notice = snap.data();
    
    const message = {
      notification: {
        title: notice.isPinned ? '📌 Pinned Notice' : 'New Notice',
        body: notice.text.substring(0, 100),
      },
      topic: 'all_users',
    };

    try {
      await admin.messaging().send(message);
      console.log('Notice notification sent');
    } catch (error) {
      console.error('Error:', error);
    }
  });

// Event notification
exports.sendExamNotification = functions.firestore
  .document('exams/{examId}')
  .onCreate(async (snap, context) => {
    const exam = snap.data();
    
    const message = {
      notification: {
        title: '📅 New Event',
        body: `${exam.type}: ${exam.title}`,
      },
      topic: 'all_users',
    };

    try {
      await admin.messaging().send(message);
      console.log('Event notification sent');
    } catch (error) {
      console.error('Error:', error);
    }
  });
```

### Deploy
```powershell
firebase deploy --only functions
```

---

## 🧪 Testing Checklist

### Authentication
- [ ] Register new user
- [ ] Login with existing credentials
- [ ] Logout (theme resets to light)
- [ ] Change password
- [ ] Invalid credentials error handling

### Notices (Admin)
- [ ] Post text notice
- [ ] Post notice with URL attachment
- [ ] Upload PDF file attachment
- [ ] Upload DOC/DOCX file
- [ ] Create poll (2-6 options)
- [ ] Pin notice to top
- [ ] Delete notice

### Notices (User)
- [ ] View all notices
- [ ] Search notices
- [ ] Vote on poll
- [ ] Change vote
- [ ] Add comment
- [ ] Click attachment link/file
- [ ] See pinned notices at top

### Events (Admin)
- [ ] Post Quiz event with date
- [ ] Post Mid-term event
- [ ] Post Final event
- [ ] Toggle admin panel collapse
- [ ] Delete event

### Events (User)
- [ ] Search events
- [ ] Filter by type (All/Quiz/Mid/Final)
- [ ] Toggle calendar view
- [ ] See grouped months in calendar
- [ ] Receive notification 1 day before event

### Settings
- [ ] Toggle dark mode
- [ ] Settings persist on restart
- [ ] Toggle notice notifications
- [ ] Toggle event notifications
- [ ] Navigate to profile
- [ ] Navigate to change password

### Profile
- [ ] View current profile
- [ ] Edit name, batch, semester
- [ ] Save profile
- [ ] See avatar with first letter

### Analytics (Admin Only)
- [ ] View content statistics
- [ ] View engagement metrics
- [ ] See recent activity feed

---

## 🎨 Customization

### App Name & Icon
Located in:
- `assets/icon/icon.png`
- `pubspec.yaml` → `flutter_launcher_icons`
- Run: `flutter pub run flutter_launcher_icons`

### Theme Colors
Edit `lib/main.dart`:
```dart
MaterialApp(
  theme: ThemeData(
    colorScheme: ColorScheme.fromSeed(
      seedColor: Colors.indigo, // Change this
    ),
  ),
)
```

### App Title
Change in `lib/main.dart`:
```dart
AppBar(title: const Text('Infyners')) // Your app name
```

---

## 🐛 Troubleshooting

### Build Errors

**Android Gradle Issues:**
```powershell
cd android
./gradlew clean
cd ..
flutter clean
flutter pub get
flutter run
```

**iOS Pod Issues:**
```bash
cd ios
pod deintegrate
pod install
cd ..
flutter clean
flutter run
```

### Firebase Errors

**Permission Denied:**
- Check Firestore rules are updated
- Verify user is authenticated
- For admin features, check user in `admins` collection

**File Upload Fails:**
- Check Storage rules are updated
- Verify internet connection
- Check file size (Firebase free tier: 5GB total)

**Notifications Not Working:**
- Ensure Google Services JSON/plist files present
- Check AndroidManifest.xml permissions
- Verify FCM is enabled in Firebase Console
- For Cloud Functions: check function logs in Firebase Console

---

## 📊 Performance Tips

1. **Firestore Indexes**: Firebase will prompt for these when needed
2. **Image Compression**: If adding image uploads, use `flutter_image_compress`
3. **Pagination**: For large notice lists, implement pagination with `startAfter()`
4. **Caching**: Enable offline persistence:
   ```dart
   FirebaseFirestore.instance.settings = const Settings(
     persistenceEnabled: true,
   );
   ```

---

## 🚀 Production Checklist

- [ ] Update `pubspec.yaml` version
- [ ] Test on real devices (Android & iOS)
- [ ] Update Firestore rules
- [ ] Update Storage rules
- [ ] Deploy Cloud Functions
- [ ] Set up Firebase Hosting (web)
- [ ] Generate release builds
- [ ] Test with multiple users
- [ ] Monitor Firebase usage/quota
- [ ] Set up Firebase Analytics (optional)
- [ ] Configure budget alerts in Firebase
