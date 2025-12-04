# 🎓 INFYNERS - Batch Notice & Communication App

[![Flutter](https://img.shields.io/badge/Flutter-3.24.0+-02569B?style=for-the-badge&logo=flutter)](https://flutter.dev/)
[![Firebase](https://img.shields.io/badge/Firebase-Platform-FFCA28?style=for-the-badge&logo=firebase)](https://firebase.google.com/)
[![Dart](https://img.shields.io/badge/Dart-3.5.0+-0175C2?style=for-the-badge&logo=dart)](https://dart.dev/)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

> A modern, real-time communication platform for educational institutions built with Flutter and Firebase. Features a stunning glassmorphism dark theme with neon accents.

---

## 📱 Overview

INFYNERS is a comprehensive batch notice and communication application designed for educational institutions. It provides real-time updates, interactive features, and seamless communication between administrators and students.

### ✨ Key Highlights

- 🎨 **Modern UI/UX** - Glassmorphism design with neon cyan and electric purple theme
- ⚡ **Real-time Updates** - Powered by Firebase Firestore
- 📱 **Cross-platform** - Works on Android, iOS, Web, Windows, macOS, and Linux
- 🔔 **Push Notifications** - FCM integration for instant alerts
- 👥 **Role-based Access** - Admin and user roles with different permissions
- 🌙 **Dark Mode** - Beautiful dark theme with persistent settings
- 💯 **100% Free** - Built entirely on Firebase free tier

---

## 🚀 Features

### 👨‍🎓 Student Features

#### 📢 Notices & Announcements
- View all notices in real-time
- Search and filter notices
- Pin important notices to top
- Add comments with profile picture
- Vote on polls
- Attachment support (PDF/URL links)

#### 📅 Events & Exams
- Calendar view and list view toggle
- Event reminders (1 day before)
- Search and filter by type (Quiz/Mid/Final)
- Date-wise grouping in calendar

#### 📚 Class Routine
- Weekly class schedule
- Time slots with course, teacher, and room info
- Clean and organized view

#### 👤 User Profile
- Upload profile picture (stored as base64 in Firestore)
- Personal information management
  - Full Name
  - Registration ID (e.g., 242-50-000)
  - Department (e.g., ICE)
  - Blood Group
  - Mobile Number
- Profile picture appears in comments

#### ⚙️ Settings
- Toggle dark mode (persists across sessions)
- Notification preferences
- Change password
- Logout

---

### 👨‍💼 Admin Features

#### 📝 Content Management
- Create, edit, and delete notices
- Create events with automatic reminders
- Manage class routines
- Pin/unpin important notices
- Upload attachments (URLs)

#### 📊 Analytics Dashboard
- Content statistics (notices, events, polls)
- Engagement metrics (comments, votes)
- Recent activity feed
- Visual overview of platform usage

#### 👥 Member Management
- Complete member directory
- View all registered users with details
- Advanced PDF export features:
  - Export all members with customizable fields
  - Export individual member profiles
  - Professional PDF formatting
- Member statistics (active/inactive)
- User management capabilities

#### 🎯 Interactive Features
- Create polls with multiple options
- Real-time vote counting
- Comment moderation (delete inappropriate comments)
- Notice editing capabilities

---

## 🛠️ Technology Stack

### Frontend
- **Flutter** - Cross-platform UI framework
- **Material 3** - Modern design system
- **Google Fonts** (Outfit, Poppins)
- **Glassmorphism UI** - Custom components

### Backend
- **Firebase Authentication** - Email/password auth
- **Cloud Firestore** - NoSQL real-time database
- **Firebase Cloud Messaging** - Push notifications

### Key Packages
```yaml
- firebase_core & firebase_auth
- cloud_firestore
- firebase_messaging
- flutter_local_notifications
- google_fonts
- intl & timezone
- image_picker
- pdf & printing
- url_launcher
- fluttertoast
```

---

## 📋 Prerequisites

Before you begin, ensure you have:

- Flutter SDK (3.24.0 or higher)
- Dart SDK (3.5.0 or higher)
- Firebase account
- Android Studio / Xcode (for mobile development)
- Git

---

## ⚙️ Installation & Setup

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/Sagnik-ICE/Infyners_App.git
cd Infyners_App
```

### 2️⃣ Install Dependencies

```bash
flutter pub get
```

### 3️⃣ Firebase Setup

#### Option A: Use Existing Configuration
The project includes pre-configured Firebase setup. Just add your `google-services.json`:

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Select your project or create a new one
3. Navigate to Project Settings → Your apps
4. Download `google-services.json`
5. Place it in `android/app/`

#### Option B: Full Setup
Follow the comprehensive guide in **[FIREBASE_SETUP.md](FIREBASE_SETUP.md)**

### 4️⃣ Configure Admin Access

Add admin email in Firestore:

1. Open Firebase Console → Firestore Database
2. Create collection: `admins`
3. Add document with your admin email:
   ```
   Document ID: (auto)
   Field: email
   Value: your-admin@email.com
   ```

### 5️⃣ Run the App

```bash
# For Android
flutter run

# For Web
flutter run -d chrome

# For Windows
flutter run -d windows

# For specific device
flutter devices
flutter run -d <device-id>
```

---

## 📁 Project Structure

```
lib/
└── main.dart                 # Single-file architecture (~8000 lines)
    ├── Theme Configuration
    ├── Firebase Initialization
    ├── Authentication Pages
    │   ├── GetStartedPage
    │   ├── LoginPage
    │   └── SignupPage
    ├── Main App Pages
    │   ├── HomePage (Dashboard)
    │   ├── NoticePage
    │   ├── ExamPage
    │   ├── RoutinePage
    │   └── ProfilePage
    ├── Admin Pages
    │   ├── AnalyticsPage
    │   ├── MembersPage
    │   └── ChangePasswordPage
    └── Settings & About Pages

android/                      # Android configuration
ios/                         # iOS configuration
web/                         # Web configuration
windows/                     # Windows configuration
assets/                      # App assets
firestore.rules             # Firestore security rules
```

---

## 🔒 Security & Firestore Rules

Comprehensive security rules are implemented:

```javascript
- Admin-only write access for notices, exams, routines
- User-specific read/write for personal settings
- Authenticated users can create comments
- Admins can delete any comment
- Vote validation and user-specific updates
- Profile picture size limits (base64)
```

Full rules available in `firestore.rules`

---

## 📖 Documentation

Additional setup guides are available:

- 🚀 **[DEPLOYMENT_GUIDE.md](DEPLOYMENT_GUIDE.md)** - Complete deployment instructions for all platforms
- 🔥 **[FIREBASE_SETUP.md](FIREBASE_SETUP.md)** - Step-by-step Firebase configuration
- ⚡ **[QUICK_SETUP.md](QUICK_SETUP.md)** - Quick start guide for developers
- 🎨 **[DESIGN_GUIDE.md](DESIGN_GUIDE.md)** - UI/UX design system guidelines

---

## 🎨 Design System

### Color Palette
```dart
Neon Cyan:       #00E5FF
Electric Purple: #7C4DFF
Accent Green:    #00D084
Accent Pink:     #FF006E
Dark Background: #0A0E27
Dark Card:       #1A1F3A
```

### Typography
- **Primary**: Outfit (Google Fonts)
- **Secondary**: Poppins (Google Fonts)

### UI Components
- Glassmorphism containers
- Neon gradient buttons
- Smooth animations
- Material 3 design

---

## 🧪 Testing

### Manual Testing Checklist

- [ ] Authentication (Login/Signup/Logout)
- [ ] Create and view notices
- [ ] Upload and view attachments
- [ ] Comment on notices
- [ ] Create and vote on polls
- [ ] Pin/unpin notices
- [ ] Create events with reminders
- [ ] Calendar view toggle
- [ ] Profile picture upload/removal
- [ ] Edit profile information
- [ ] Dark mode toggle
- [ ] Change password
- [ ] Admin analytics dashboard
- [ ] Member management
- [ ] PDF export functionality

---

## 🚀 Deployment

### Android
```bash
flutter build apk --release
flutter build appbundle --release
```

### iOS
```bash
flutter build ios --release
```

### Web
```bash
flutter build web --release
```

### Windows
```bash
flutter build windows --release
```

Detailed deployment instructions in **[DEPLOYMENT_GUIDE.md](DEPLOYMENT_GUIDE.md)**

---

## 💡 Key Achievements

- ✅ **100% Firebase Free Tier** - No paid services required
- ✅ **Single-file Architecture** - Easy to maintain and understand
- ✅ **Modern UI/UX** - Glassmorphism with neon accents
- ✅ **Cross-platform** - 6 platforms from single codebase
- ✅ **Real-time Sync** - Instant updates across all devices
- ✅ **Offline Support** - Firestore caching
- ✅ **Professional PDF Export** - Styled member reports
- ✅ **Base64 Image Storage** - No Firebase Storage needed

---

## 🐛 Known Issues & Solutions

All common issues and their solutions are documented in the deployment guide. If you encounter any problems:

1. Check **[DEPLOYMENT_GUIDE.md](DEPLOYMENT_GUIDE.md)** - Troubleshooting section
2. Verify Firebase configuration
3. Ensure all dependencies are installed
4. Check Flutter doctor: `flutter doctor -v`

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 👨‍💻 Developer

**Sagnik Saha Dibbay**
- Institution: Daffodil International University
- Department: Information and Communication Engineering
- GitHub: [@Sagnik-ICE](https://github.com/Sagnik-ICE)
- Email: dibbaysaha17@gmail.com
- Email: dibbay242-50-014@diu.edu.bd

---

## 📞 Support

For support, email dibbay242-50-014@diu.edu.bd or open an issue in the GitHub repository.

---

## 🙏 Acknowledgments

- Flutter team for the amazing framework
- Firebase team for the powerful backend services
- Google Fonts for beautiful typography
- All contributors and testers

---

## 📊 Project Status

✅ **Production Ready** - Fully tested and deployed

**Last Updated**: December 2025  
**Version**: 1.0.0  
**Status**: Active Development

---

<div align="center">

### ⭐ Star this repository if you find it helpful!

Made with using Flutter and Firebase

</div>
