# Firebase Setup Instructions

## 1. Firebase Storage Rules (for file uploads)

Go to Firebase Console → Storage → Rules and update with:

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /notice_attachments/{userId}/{fileName} {
      // Only authenticated users can upload
      allow create: if request.auth != null && request.auth.uid == userId;
      // Anyone can read attachments
      allow read: if true;
      // Only owner can delete
      allow delete: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

## 2. FCM Push Notifications Setup

### Option A: Cloud Functions (Recommended)

1. Install Firebase CLI: `npm install -g firebase-tools`
2. Initialize functions: `firebase init functions`
3. Create `functions/index.js`:

```javascript
const functions = require('firebase-functions');
const admin = require('firebase-admin');
admin.initializeApp();

// Trigger when a new notice is created
exports.sendNoticeNotification = functions.firestore
  .document('notices/{noticeId}')
  .onCreate(async (snap, context) => {
    const notice = snap.data();
    const message = {
      notification: {
        title: 'New Notice Posted',
        body: notice.text.substring(0, 100),
      },
      topic: 'all_users', // Send to all users subscribed to this topic
    };

    try {
      await admin.messaging().send(message);
      console.log('Notification sent successfully');
    } catch (error) {
      console.error('Error sending notification:', error);
    }
  });

// Trigger when a new exam/event is posted
exports.sendExamNotification = functions.firestore
  .document('exams/{examId}')
  .onCreate(async (snap, context) => {
    const exam = snap.data();
    const message = {
      notification: {
        title: 'New Event Posted',
        body: `${exam.type}: ${exam.title}`,
      },
      topic: 'all_users',
    };

    try {
      await admin.messaging().send(message);
      console.log('Notification sent successfully');
    } catch (error) {
      console.error('Error sending notification:', error);
    }
  });
```

4. Deploy: `firebase deploy --only functions`

### Option B: Manual API Call (for testing)

Use Postman or curl to send FCM messages:

```bash
curl -X POST https://fcm.googleapis.com/v1/projects/YOUR_PROJECT_ID/messages:send \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "message": {
      "topic": "all_users",
      "notification": {
        "title": "Test Notice",
        "body": "This is a test notification"
      }
    }
  }'
```

### Subscribe Users to Topic

Add to `lib/main.dart` in `initializeNotifications()`:

```dart
if (!kIsWeb) {
  // Subscribe all users to 'all_users' topic
  await FirebaseMessaging.instance.subscribeToTopic('all_users');
}
```

## 3. Android Permissions for File Picker

Already configured in `android/app/src/main/AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"
    android:maxSdkVersion="32" />
```

## 4. Testing File Upload

1. Run the app
2. Login as admin
3. Go to Notices tab
4. Click "Upload PDF/DOC" button
5. Select a PDF or DOC file
6. Add notice text
7. Click "Post Notice"
8. File will be uploaded to Firebase Storage and URL stored in Firestore

## 5. Optional: URL Launcher (for opening attachments in browser)

Add to `pubspec.yaml`:
```yaml
dependencies:
  url_launcher: ^6.2.2
```

Then in `NoticeCard`, replace the toast with:
```dart
import 'package:url_launcher/url_launcher.dart';

onTap: () async {
  final uri = Uri.parse(widget.attachmentUrl!);
  if (await canLaunchUrl(uri)) {
    await launchUrl(uri, mode: LaunchMode.externalApplication);
  } else {
    Fluttertoast.showToast(msg: 'Cannot open attachment');
  }
},
```
