# Panduan Setup Environment Variables

Template ini menggunakan Next.js + Firebase dengan berbagai layanan tambahan. Ikuti panduan berikut untuk mengisi semua environment variables yang diperlukan.

## 📋 Daftar Environment Variables

### Firebase Configuration
- `NEXT_PUBLIC_FIREBASE_PUBLIC_API_KEY`
- `NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN`
- `NEXT_PUBLIC_FIREBASE_DATABASE_URL`
- `NEXT_PUBLIC_FIREBASE_PROJECT_ID`
- `NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET`
- `NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID`
- `NEXT_PUBLIC_FIREBASE_APP_ID`
- `NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID`
- `PRIVATE_FIREBASE_CLIENT_EMAIL`
- `PRIVATE_FIREBASE_PRIVATE_KEY_ID`
- `PRIVATE_FIREBASE_PRIVATE_KEY`

### Security & Authentication
- `NEXT_PUBLIC_RECAPTCHA_KEY`
- `COOKIE_SECRET_CURRENT`
- `COOKIE_SECRET_PREVIOUS`
- `NEXT_PUBLIC_COOKIE_SECURE`

### Third-party Services
- `RESEND_API_KEY`
- `NEXT_PUBLIC_SENTRY_DSN`
- `NEXT_PUBLIC_SENTRY_ORG`
- `NEXT_PUBLIC_SENTRY_PROJECT`

### Local Development
- `HOST`
- `GOOGLE_APPLICATION_CREDENTIALS`

---

## 🔥 Setup Firebase Project

### 1. Buat Firebase Project
1. Buka [Firebase Console](https://console.firebase.google.com/)
2. Klik "Add project"
3. Beri nama project (contoh: `my-app-2024`)
4. Pilih atau buat Google Analytics account
5. Tunggu hingga project selesai dibuat

### 2. Dapatkan Firebase Configuration
1. Di Firebase Console, klik icon Web (`</>`) untuk add web app
2. Beri nama app (contoh: `My App Web`)
3. Klik "Register app"
4. Copy Firebase configuration yang muncul:

```javascript
const firebaseConfig = {
  apiKey: "AIzaXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "your-project-id.firebaseapp.com",
  databaseURL: "https://your-project-id-default-rtdb.firebaseio.com",
  projectId: "your-project-id",
  storageBucket: "your-project-id.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef123456789012345678"
};
```

**Isi di .env.local:**
```
NEXT_PUBLIC_FIREBASE_PUBLIC_API_KEY=AIzaXXXXXXXXXXXXXXXXXXXXXXX
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your-project-id.firebaseapp.com
NEXT_PUBLIC_FIREBASE_DATABASE_URL=https://your-project-id-default-rtdb.firebaseio.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your-project-id
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your-project-id.appspot.com
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=123456789012
NEXT_PUBLIC_FIREBASE_APP_ID=1:123456789012:web:abcdef123456789012345678
```

### 3. Buat Service Account Key
1. Di Firebase Console, pergi ke **Project Settings** → **Service accounts**
2. Klik "Generate new private key"
3. Pilih "JSON" dan klik "Generate"
4. Download file JSON tersebut
5. Buka file JSON, cari nilai-nilai berikut:

```json
{
  "type": "service_account",
  "project_id": "your-project-id",
  "private_key_id": "key-id-here",
  "private_key": "-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n",
  "client_email": "firebase-adminsdk-xxxxx@your-project-id.iam.gserviceaccount.com",
  "client_id": "123456789012345678901"
}
```

**Isi di .env.local:**
```
PRIVATE_FIREBASE_CLIENT_EMAIL=firebase-adminsdk-xxxxx@your-project-id.iam.gserviceaccount.com
PRIVATE_FIREBASE_PRIVATE_KEY_ID=key-id-here
PRIVATE_FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"
```

### 4. Setup Google Analytics ID
1. Di Firebase Console, pergi ke **Project Settings** → **Integrations**
2. Klik Google Analytics, kamu akan melihat Measurement ID
3. Format: `G-XXXXXXXXXX`

**Isi di .env.local:**
```
NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID=G-XXXXXXXXXX
```

---

## 🔐 Setup reCAPTCHA

1. Di Firebase Console, pergi ke **Authentication** → **Sign-in method**
2. Scroll ke bawah, temukan **reCAPTCHA**
3. Klik "Add site"
4. Isi form:
   - **Site name**: Nama website kamu (contoh: "My App")
   - **Site domain**: `localhost` untuk development, atau domain production kamu
5. Klik "Create"
6. Copy **Site Key** yang muncul

**Isi di .env.local:**
```
NEXT_PUBLIC_RECAPTCHA_KEY=6LeIxAcTAAAAAJcZVRqyHh71UMIEsBg1RLkMYTqe
```

> **Note**: Ini adalah test key untuk development, gunakan production key dari Google reCAPTCHA untuk production.

---

## 📧 Setup Resend untuk Email

1. Buka [Resend Dashboard](https://resend.com/dashboard)
2. Sign up atau login
3. Di dashboard, klik **API Keys** → **Create API Key**
4. Beri nama (contoh: "Development Key")
5. Copy API key yang dihasilkan

**Isi di .env.local:**
```
RESEND_API_KEY=re_your_api_key_here
```

---

## 🐛 Setup Sentry untuk Error Monitoring

1. Buka [Sentry.io](https://sentry.io)
2. Sign up atau login
3. Create new project:
   - Pilih platform: **Next.js**
   - Beri nama project (contoh: "my-app-frontend")
4. Setelah project dibuat, copy DSN dari setup instructions
5. Di dashboard Sentry, kamu bisa melihat:
   - **Organization Slug**: di URL atau di settings
   - **Project Slug**: di URL atau di settings

**Isi di .env.local:**
```
NEXT_PUBLIC_SENTRY_DSN=https://your-dsn@sentry.io/project-id
NEXT_PUBLIC_SENTRY_ORG=your-organization-slug
NEXT_PUBLIC_SENTRY_PROJECT=your-project-slug
```

---

## 🔑 Generate Cookie Secrets

Buka terminal dan jalankan:

```bash
# Generate random string untuk cookie secrets
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

Jalankan 2 kali untuk mendapatkan 2 random string yang berbeda.

**Isi di .env.local:**
```
COOKIE_SECRET_CURRENT=your-first-random-string-here
COOKIE_SECRET_PREVIOUS=your-second-random-string-here
```

---

## 📁 Setup Google Application Credentials

1. Buat folder `keys` di root project
2. Pindahkan file JSON service account ke sana (contoh: `firebase-service-account.json`)
3. Update .env.local:

```
GOOGLE_APPLICATION_CREDENTIALS=E:\RProject\nextjs-firebase\keys\firebase-service-account.json
```

---

## 📝 Complete .env.local Template

```bash
# Firebase Configuration
PRIVATE_FIREBASE_CLIENT_EMAIL=firebase-adminsdk-xxxxx@your-project-id.iam.gserviceaccount.com
NEXT_PUBLIC_FIREBASE_PUBLIC_API_KEY=AIzaXXXXXXXXXXXXXXXXXXXXXXX
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=your-project-id.firebaseapp.com
NEXT_PUBLIC_FIREBASE_DATABASE_URL=https://your-project-id-default-rtdb.firebaseio.com
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=your-project-id.appspot.com
NEXT_PUBLIC_FIREBASE_PROJECT_ID=your-project-id
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=123456789012
NEXT_PUBLIC_FIREBASE_APP_ID=1:123456789012:web:abcdef123456789012345678
NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID=G-XXXXXXXXXX
NEXT_PUBLIC_RECAPTCHA_KEY=6LeIxAcTAAAAAJcZVRqyHh71UMIEsBg1RLkMYTqe

# Firebase Service Account
PRIVATE_FIREBASE_PRIVATE_KEY_ID=key-id-here
PRIVATE_FIREBASE_PRIVATE_KEY='"-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"'

# Cookie Secrets (generate dengan: node -e "console.log(require('crypto').randomBytes(32).toString('hex'))")
COOKIE_SECRET_CURRENT=generate-random-string-32-characters-here
COOKIE_SECRET_PREVIOUS=generate-another-random-string-32-characters-here
NEXT_PUBLIC_COOKIE_SECURE=false # set ke true untuk HTTPS environment

# Firebase Functions
RESEND_API_KEY=re_your_api_key_here

# Sentry (error reporting dan monitoring)
NEXT_PUBLIC_SENTRY_DSN=https://your-dsn@sentry.io/project-id
NEXT_PUBLIC_SENTRY_ORG=your-organization-slug
NEXT_PUBLIC_SENTRY_PROJECT=your-project-slug

# Local development
HOST=local
GOOGLE_APPLICATION_CREDENTIALS=E:\RProject\nextjs-firebase\keys\firebase-service-account.json
```

---

## 🚀 Menjalankan Project

Setelah semua environment variables terisi, jalankan:

```bash
# Install dependencies
yarn install

# Jalankan development server
yarn dev

# Atau jalankan Firebase emulator untuk development lokal
yarn emulate
```

---

## ⚠️ Security Notes

- **Jangan commit** file `.env.local` ke version control!
- Tambahkan `.env.local` ke `.gitignore`
- Gunakan production keys untuk production environment
- Set `NEXT_PUBLIC_COOKIE_SECURE=true` untuk HTTPS environment
- Keep semua API keys dan secrets aman

---

## 📚 Useful Links

- [Firebase Console](https://console.firebase.google.com/)
- [Resend Dashboard](https://resend.com/dashboard)
- [Sentry.io](https://sentry.io)
- [Next.js Documentation](https://nextjs.org/docs)
- [Firebase Documentation](https://firebase.google.com/docs)