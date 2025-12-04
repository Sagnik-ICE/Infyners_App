# Quick Setup Guide - 100% Free Tier

## 🎉 All Features FREE - No Storage Costs!

### What's New:
1. ✅ **PDF uploads work** - Stored as base64 in Firestore (FREE!)
2. ✅ **Profile pictures work** - Stored as base64 in Firestore (FREE!)
3. ✅ **No Firebase Storage needed** - Everything in Firestore!
4. ✅ **Updated profile fields** - Registration ID, Department, Blood Group, Mobile
5. ✅ **Comments show profile info** - Name and picture
6. ✅ **Attachments open in browser** - Using url_launcher

**100% Free Tier - No Premium Features!**

---

## 📋 Firebase Console Setup (3 Minutes)

### Step 1: Update Firestore Rules

1. Open Firebase Console: https://console.firebase.google.com
2. Select your project
3. Go to **Firestore Database** → **Rules** tab
4. Copy the entire content from `firestore.rules` file
5. Paste and **Publish**

**Key Points:**
- Profile pictures stored as base64 data URLs in Firestore
- PDF files stored as base64 in Firestore (small files only)
- No Firebase Storage needed - saves costs!

### Step 2: Create Admin User (If Not Already Done)

1. Go to **Firestore Database** → **Data** tab
2. Click **Start collection**
3. Collection ID: `admins`
4. Document ID: Copy user UID from **Authentication** tab
5. Add field: `isAdmin` = `true` (boolean)
6. Save

**That's it! No Storage setup needed!**

---

## 🧪 Testing the New Features

### Test PDF Upload:
```
1. Login as admin
2. Go to Notices tab
3. Click "Upload PDF/DOC"
4. Select a PDF file
5. Add notice text
6. Click "Post Notice"
7. ✅ Should upload successfully
8. Click the PDF link to open in browser
```

### Test Profile Features:
```
1. Go to Settings → My Profile
2. Click camera icon or "Change Photo"
3. Select image from gallery
4. ✅ Photo should upload and display
5. Fill in:
   - Name
   - Registration ID (e.g., 242-50-000)
   - Department (e.g., Computer Science)
   - Blood Group (e.g., O+)
   - Mobile (e.g., +8801XXXXXXXXX)
6. Click Save
7. ✅ Profile saved successfully
```

### Test Comments with Profile:
```
1. Go to any notice
2. Click "Comments" to expand
3. Type a comment and send
4. ✅ Comment should show your profile picture and name
```

---

## 📱 Run the App

```powershell
# Install dependencies (already done if you ran pub get)
flutter pub get

# Run on connected device or emulator
flutter run

# Or run on web
flutter run -d chrome
```

---

## 🆓 Free Tier Limits (You're Safe!)

### Firestore (Free Tier) - Your ONLY Storage:
- ✅ **1 GB total storage**
- ✅ 50K reads/day
- ✅ 20K writes/day

**Your app uses:**
- Profile pictures: ~50-70 KB each (base64, resized to 512x512)
- Small PDF attachments: ~100-500 KB each (base64)
- User profiles: ~5-10 KB each
- Notices with comments: ~5 KB each

**Estimate:** 
- 100 users with photos: ~7 MB
- 100 small PDFs: ~50 MB  
- 1000 notices: ~5 MB
- **Total: ~62 MB out of 1 GB = Only 6% used!**

### ⚠️ Important Notes:
1. **PDFs should be small** (<500 KB) - base64 increases size by ~33%
2. **For large files** - use URL links to Google Drive/Dropbox
3. **Profile pictures auto-resized** to 512x512 (keeps them small)
4. **JPEG quality 75%** for optimal compression

### No Premium Services:
- ❌ Firebase Storage NOT used
- ❌ Cloud Functions NOT required (optional for FCM)
- ✅ Everything stored in Firestore (FREE tier)

---

## 🔧 Troubleshooting

### PDF Upload Fails:
```
Error: "File bytes not available"
Fix: Ensure PDF is small (<500 KB recommended)
     Large files should use URL links instead
```

### Profile Picture Not Showing:
```
Error: "Failed to upload photo"
Fix: Check internet connection
     Images stored as base64 in Firestore (no Storage needed)
```

### Comments Show Email Instead of Name:
```
Issue: Old comments before update
Fix: Add new comment - it will show profile name
     Old comments will still show email (one-time migration needed)
```

### Attachment Links Don't Open:
```
Error: "Cannot open attachment"
Fix: Ensure url_launcher is installed (already done)
     Check URL is valid and accessible
```

---

## 📊 What Changed in Code

### Removed Packages:
```yaml
# REMOVED - No longer needed!
# firebase_storage: ^12.3.4  # Premium service removed
```

### Storage Method:
- **Old:** Firebase Storage (premium)
- **New:** Base64 encoding in Firestore (100% free!)

### New Profile Fields:
```dart
registrationId: String    // e.g., 242-50-000
department: String        // e.g., Computer Science
bloodGroup: String        // e.g., O+, A-
mobile: String           // e.g., +8801XXXXXXXXX
profilePicUrl: String    // Base64 data URL (no Storage!)
```

### PDF/File Storage:
```dart
// Files stored as base64 data URLs in Firestore
attachmentUrl: String    // data:application/pdf;base64,...
attachmentName: String   // Original filename
```

### Comment Structure:
```dart
// Old:
userEmail: String

// New:
userName: String         // From user profile
profilePicUrl: String   // From user profile
userId: String          // For reference
```

---

## ✅ Quick Verification

After setup, verify these work:

- [ ] Firebase Firestore rules updated (no permission errors)
- [ ] **No Storage setup needed** (we use base64!)
- [ ] Admin user created (can post notices)
- [ ] Small PDF upload works (<500 KB)
- [ ] Profile picture upload works (auto-resized)
- [ ] Comments show profile name and picture
- [ ] Attachments open in external browser

---

## 🎉 You're All Set!

All features are **100% FREE TIER**! 

- ✅ No Firebase Storage costs
- ✅ No premium services
- ✅ Everything in Firestore (1 GB free)

**Tips:**
- Keep PDFs small (<500 KB) or use URL links
- Profile pictures auto-optimized
- Base64 adds ~33% to file size (built into estimates)

For detailed documentation, see:
- `FEATURES_SUMMARY.md` - Complete feature list
- `DEPLOYMENT_GUIDE.md` - Platform-specific deployment
- `firestore.rules` - Security rules (copy to Firebase Console)
