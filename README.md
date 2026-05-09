# 🏠 הבית שלי — מדריך הקמה

## מה צריך?

1. חשבון Google (להתחברות)
2. פרויקט Firebase (חינמי)
3. שרת אחסון סטטי (GitHub Pages / Netlify / Vercel / כל שרת)

## שלב 1: הקמת Firebase

1. לך ל-[Firebase Console](https://console.firebase.google.com)
2. לחץ **Add Project** → תן שם (למשל `habait`) → צור
3. בדף הפרויקט לחץ **Web** (אייקון `</>`) → רשום את האפליקציה
4. **העתק את ה-firebaseConfig** שתקבל ושים במקום הערכים ב-`index.html`:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "habait-xxxxx.firebaseapp.com",
  projectId: "habait-xxxxx",
  storageBucket: "habait-xxxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

## שלב 2: הפעלת Authentication

1. ב-Firebase Console → **Authentication** → **Sign-in method**
2. הפעל **Google** → בחר support email → שמור

## שלב 3: הקמת Firestore

1. ב-Firebase Console → **Firestore Database** → **Create Database**
2. בחר **Start in test mode** (נשנה אח"כ)
3. בחר region (למשל `europe-west3` לישראל)

### חוקי אבטחה (Security Rules)

החלף את חוקי ברירת המחדל ב:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // User preferences - only the user
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    // Home data - any authenticated user with access
    match /homes/{homeId}/{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

## שלב 4: העלאה לשרת

### אפשרות א: GitHub Pages
```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/USERNAME/habait.git
git push -u origin main
```
ב-GitHub: Settings → Pages → Source: main → Save

### אפשרות ב: Firebase Hosting
```bash
npm install -g firebase-tools
firebase login
firebase init hosting  # בחר public directory = .
firebase deploy
```

### אפשרות ג: Netlify
גרור את התיקייה ל-[Netlify Drop](https://app.netlify.com/drop)

## שלב 5: PWA — אייקון במסך הבית

אחרי שהאתר באוויר:
- **אייפון**: Safari → Share → Add to Home Screen
- **אנדרואיד**: Chrome → תפריט ⋮ → Add to Home Screen / Install App

## שיתוף עם המשפחה

1. תתחבר לאפליקציה
2. הגדרות → צור בית משותף חדש
3. העתק את **קוד הבית** ושלח לבני המשפחה
4. הם מתחברים → הגדרות → מכניסים את הקוד → לוחצים "הצטרף"

## מבנה הקבצים

```
habait/
├── index.html      ← האפליקציה (הכל בקובץ אחד)
├── manifest.json   ← PWA manifest (אייקון, שם, צבעים)
├── sw.js           ← Service Worker (offline + push)
└── README.md       ← המדריך הזה
```

## בעיות נפוצות

| בעיה | פתרון |
|------|-------|
| שגיאת התחברות | בדוק שהפעלת Google Auth ב-Firebase |
| הנתונים לא נשמרים | בדוק שהגדרת Firestore + Security Rules |
| האייקון לא מופיע | האתר חייב להיות ב-HTTPS |
| התראות לא עובדות | בדוק הרשאות דפדפן + HTTPS נדרש |
