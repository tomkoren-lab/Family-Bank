# הבנק המשפחתי – סיכום פרויקט להעברה ל-Claude Code

## מה זה
אפליקציית ניהול דמי כיס משפחתית בעברית. כרגע PWA (Progressive Web App) – קובץ HTML יחיד שרץ על GitHub Pages עם Firebase כמסד נתונים.

**URL נוכחי:** https://tomkoren-lab.github.io/Family-Bank  
**GitHub Repo:** https://github.com/tomkoren-lab/Family-Bank  
**קובץ:** index.html (קובץ יחיד, ~880 שורות)

---

## Firebase Config
```javascript
{
  apiKey: "AIzaSyBKK8MZotCmhDp_86kLv1G42RVUWsNiVRY",
  authDomain: "family-bank-9401c.firebaseapp.com",
  databaseURL: "https://family-bank-9401c-default-rtdb.europe-west1.firebasedatabase.app",
  projectId: "family-bank-9401c",
  storageBucket: "family-bank-9401c.firebasestorage.app",
  messagingSenderId: "322322946024",
  appId: "1:322322946024:web:9817d4e817029ba7dd765d"
}
```
- סוג: **Realtime Database** (לא Firestore)
- Region: europe-west1
- SDK: firebase-compat v9.23.0

---

## Stack טכני
| שכבה | טכנולוגיה |
|------|-----------|
| Frontend | HTML + CSS + Vanilla JS (קובץ אחד) |
| DB | Firebase Realtime Database |
| Hosting | GitHub Pages (סטטי) |
| גרפים | Chart.js 4.4.0 |
| פונטים | Heebo (Google Fonts) |
| אייקון | PWA apple-touch-icon מוטמע כ-base64 |

---

## מבנה נתונים ב-Firebase

```
/
├── settings/
│   ├── familyName: string          // שם המשפחה לתצוגה
│   ├── pin: string (4 ספרות)       // קוד הורים
│   ├── autoAllowance: boolean      // הפקדה אוטומטית
│   ├── allowanceDay: string        // sunday/friday/saturday
│   ├── allowanceHour: string       // "08" וכו'
│   ├── securityQuestion: string    // שאלת אבטחה לשחזור
│   ├── securityAnswer: string      // תשובה לשחזור קוד
│   ├── savingsRate: number         // ריבית חיסכון % (ברירת מחדל 5)
│   └── savingsReleaseDay: number   // יום שחרור חיסכון (ברירת מחדל 28)
│
└── kids/
    └── {kidId}/
        ├── name: string
        ├── color: string           // teal/blue/purple/amber/pink
        ├── allowance: number       // דמי כיס שבועיים/חודשיים
        ├── photo: string           // base64 (אופציונלי)
        ├── linkPin: boolean        // האם הלינק מוגן
        ├── linkPinCode: string     // קוד 4 ספרות ללינק
        ├── txs/
        │   └── {txId}/
        │       ├── type: 'income' | 'expense'
        │       ├── amount: number
        │       ├── desc: string
        │       ├── date: string    // תאריך מקומי
        │       └── ts: timestamp
        └── savings/
            └── {savId}/
                ├── amount: number          // סכום מופקד
                ├── months: number          // תקופה בחודשים
                ├── startTs: timestamp
                ├── released: boolean       // האם שוחרר
                ├── interestPaid: number    // סה"כ ריבית ששולמה
                └── lastInterestMonth: string // מפתח חודש אחרון
```

---

## עמודים (Routing ב-Hash)

| Hash | עמוד | תיאור |
|------|------|-------|
| `#home` / `` | דף בית | Landing page תדמיתי |
| `#pin-admin` | PIN | מסך הזנת קוד הורים |
| `#kids` | בחירת ילד | גריד של כל הילדים |
| `#kid/{id}` | דשבורד ילד | ממשק הילד הספציפי |
| `#admin` | ממשק הורים | ניהול ילדים + הגדרות |

---

## פיצ'רים קיימים

### ממשק הורים (Admin)
- [x] PIN 4 ספרות לכניסה
- [x] שחזור קוד עם שאלת אבטחה
- [x] תמיכה במקלדת בהזנת PIN
- [x] הוספת/מחיקת ילדים
- [x] צבע + תמונה לכל ילד (base64)
- [x] הפקדה/משיכה ידנית לכל ילד
- [x] מחיקת תנועות
- [x] קישור ייעודי לכל ילד + הגנת PIN אופציונלית
- [x] דמי כיס אוטומטיים (יום + שעה)
- [x] "שלם לכולם עכשיו"
- [x] הגדרות ריבית חיסכון ויום שחרור

### ממשק ילד
- [x] יתרה נוכחית
- [x] כפתור רישום הוצאה (ללא קוד)
- [x] כפתור הכנסה (דורש PIN הורה)
- [x] גרף הפרש נטו שבועי/חודשי (Chart.js)
- [x] 10 תנועות אחרונות + "ראה עוד"
- [x] קופת חיסכון 🐷 (כרטיס כתום-זהב)
- [x] כרטיס עידוד חיסכון סגול
- [x] גישה דרך לינק ייעודי (עם/בלי PIN)

### חיסכון
- [x] הפקדה לחיסכון עם תחזית ריבית אינטראקטיבית
- [x] בחירת תקופה: 1/2/3/6/12 חודשים
- [x] ריבית אוטומטית בכניסה לאפליקציה (אם הגיע יום השחרור)
- [x] שחרור קרן בסוף תקופה
- [x] לא ניתן לשחרר מוקדם

### PWA
- [x] apple-touch-icon מוטמע (base64)
- [x] ניתן להוסיף למסך הבית ב-iPhone
- [x] שם: "בנק משפחתי"

---

## מה חסר / רוצים לבנות (Roadmap)

### עדיפות גבוהה (MVP לסאס)
- [ ] **Firebase Authentication** – כל משפחה חשבון נפרד
- [ ] **Multi-tenant** – הפרדת דאטה בין משפחות (כרגע הכל באותו DB)
- [ ] **דף הרשמה / כניסה** לאדמין
- [ ] **Stripe** – תשלום מנוי (freemium: עד 2 ילדים חינם)
- [ ] **Landing page שיווקי** נפרד

### עדיפות בינונית
- [ ] **משימות** – ילד מבצע משימה → מקבל כסף אוטומטית
- [ ] **יעדי חיסכון** – ילד מגדיר מטרה (אופניים = ₪500)
- [ ] **התראות** – WhatsApp/SMS בהפקדה/שחרור חיסכון
- [ ] **דוח PDF** חודשי להורים

### עדיפות נמוכה
- [ ] אנגלית / מולטי-לינגואל
- [ ] Dark mode
- [ ] אנימציות בעת הפקדה/שחרור

---

## בעיות ידועות / מגבלות

1. **אין multi-tenant** – כרגע כל המשפחות באותו Firebase. זה הבעיה הגדולה ביותר לפני launch.
2. **הפקדה אוטומטית** – לא רצה ב-background. מופעלת רק כשמישהו פותח את האתר (localStorage לוודא שלא משלמים פעמיים).
3. **תמונות כ-base64** – עלולות לנפח את ה-DB. בפיתוח עתידי – Firebase Storage.
4. **GitHub Pages = סטטי** – לא ניתן להריץ backend. כל לוגיקה בצד client.
5. **אין email/SMS** – שחזור קוד רק דרך שאלת אבטחה בממשק.

---

## הוראות פריסה נוכחיות

1. ערוך `index.html` בכל עדכון
2. ב-GitHub: לחץ על הקובץ ← עיפרון ← Ctrl+A ← Delete ← הדבק ← Commit
3. GitHub Pages מתעדכן אוטומטית תוך ~דקה

**לעתיד:** לעבור ל-Vercel + Next.js כשיש multi-tenant.

---

## הקשר עסקי

- **שוק יעד:** הורים ישראלים, עברית בלבד בשלב ראשון
- **מתחרים:** PayKid (paykid.co.il) – דומה אבל חלש יותר בUX
- **מודל עסקי מתוכנן:** Freemium – עד 2 ילדים חינם, 3+ ב-₪19.90/חודש
- **סטטוס:** פרוטוטייפ עובד, עדיין לא multi-tenant

---

## הוראות לפיתוח עתידי עם Claude Code

```bash
# Clone the repo
git clone https://github.com/tomkoren-lab/Family-Bank
cd Family-Bank

# הקובץ הראשי
ls index.html
```

**המשך מומלץ:**
1. להמיר לפרויקט Next.js
2. להוסיף Firebase Auth
3. לשנות את מבנה ה-DB ל-`/families/{familyId}/kids/...`
4. לחבר Stripe Checkout
