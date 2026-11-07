# نظام مفاضلة تغيير الاختصاص 2025-2026 (v3.3)

الهيئة السورية للاختصاصات الطبية - البورد السوري

## 🎉 المميزات

- ✅ قاعدة بيانات PostgreSQL (البيانات محفوظة دائماً)
- ✅ Backend كامل (API Routes)
- ✅ مظهر حديث ومطور (Cairo + shadcn/ui + Framer Motion)
- ✅ أمان قوي (bcrypt + JWT)
- ✅ متوافق مع عدة استضافات: **Movestax, Vercel, Render, Railway, VPS**

## 🚀 النشر على الاستضافات (اختر واحدة)

### ⭐ 1. Movestax (يحتوي PostgreSQL مدمج)
اقرأ ملف **MOVESTAX-DEPLOY.md** للتعليمات الكاملة.

**ملخص سريع:**
1. أنشئ حساباً على https://app.movestax.com
2. أنشئ مشروعاً جديداً + قاعدة بيانات PostgreSQL
3. انسخ **Transaction Pooler URL** وأضف:
   ```
   ?pgbouncer=true&connection_limit=1
   ```
4. ارفع المشروع إلى GitHub
5. اربط GitHub مع Movestax App Deployment
6. أضف متغيرات البيئة الثلاثة
7. Deploy! 🚀

### ▲ 2. Vercel + Neon PostgreSQL (مجاني للأبد)
اقرأ ملف **VERCEL-DEPLOY.md** للتعليمات الكاملة.

### 🚂 3. Railway ($5 رصيد مجاني)
1. اذهب إلى https://railway.app
2. أنشئ مشروعاً + PostgreSQL
3. اربط مستودع GitHub
4. أضف متغيرات البيئة

### 🎨 4. Render (Web Service)
اقرأ ملف **DEPLOYMENT-GUIDE.md** للتعليمات الكاملة.

### 🐳 5. VPS بـ Docker
```bash
docker build -t syrian-board .
docker run -p 3000:3000 \
  -e DATABASE_URL="postgresql://..." \
  -e ADMIN_PASSWORD_HASH="..." \
  -e JWT_SECRET="..." \
  syrian-board
```

## 🔧 التشغيل المحلي للتطوير

```bash
# 1. تثبيت الحزم
npm install --legacy-peer-deps

# 2. إعداد البيئة
cp .env.example .env
# عدّل DATABASE_URL بقاعدة بياناتك

# 3. توليد Prisma Client
npx prisma generate

# 4. إنشاء الجداول في قاعدة البيانات
npx prisma db push --accept-data-loss

# 5. تشغيل الموقع
npm run dev
```

افتح: http://localhost:3000

## 🔐 الروابط

- الاستمارة: `/`
- لوحة الإدارة: `/syrian-commission-for-medical-specialties`
- كلمة المرور الافتراضية: `board-admin-2026`

## 🔄 تغيير كلمة المرور

```bash
node scripts/generate-hash.js "your-new-password"
```

ثم حدّث `ADMIN_PASSWORD_HASH` في متغيرات البيئة على الاستضافة.

## 📊 متغيرات البيئة المطلوبة

| الاسم | الوصف | القيمة الافتراضية |
|------|------|------------------|
| `DATABASE_URL` | رابط PostgreSQL | مطلوب |
| `ADMIN_PASSWORD_HASH` | bcrypt hash لكلمة المرور | `$2b$10$M7C51jnRh6HyPEf8Tc1FiuXzfzT760N4DRbvSZY4kjbv3/k89JFCi` |
| `JWT_SECRET` | مفتاح JWT السري (32+ حرف) | `SyrianBoard_2026_Production_Secret_Min_32_Chars_Long_String_DoNotShare` |

## 🛠️ التقنيات

- **Framework**: Next.js 16 (App Router + API Routes)
- **Database**: PostgreSQL + Prisma ORM 6
- **Auth**: bcryptjs (cost 10) + jose (JWT HS256)
- **Styling**: Tailwind CSS 4 + shadcn/ui + خط Cairo
- **Animations**: Framer Motion
- **Icons**: Lucide React

## 📁 بنية المشروع

```
├── prisma/
│   └── schema.prisma          # مخطط قاعدة البيانات (PostgreSQL)
├── public/
│   └── logo.jpg               # شعار الهيئة الرسمي
├── scripts/
│   └── generate-hash.js       # توليد كلمة مرور مشفرة
├── src/
│   ├── app/
│   │   ├── api/               # API Routes
│   │   ├── globals.css
│   │   ├── layout.tsx
│   │   ├── page.tsx           # الاستمارة
│   │   └── syrian-commission-for-medical-specialties/
│   │       └── page.tsx       # لوحة الإدارة
│   ├── components/
│   │   ├── sbboard/          # مكونات النظام
│   │   └── ui/               # shadcn/ui
│   └── lib/
│       ├── auth.ts           # bcrypt + JWT
│       ├── constants.ts      # بيانات المفاضلة
│       ├── db.ts             # Prisma client
│       └── forms/            # إدارة الحالة
├── .env.example              # قالب متغيرات البيئة
├── app.json                  # إعدادات Heroku/Movestax
├── vercel.json               # إعدادات Vercel
├── package.json
├── VERCEL-DEPLOY.md          # دليل Vercel
├── MOVESTAX-DEPLOY.md        # دليل Movestax
└── DEPLOYMENT-GUIDE.md       # دليل Render/VPS
```

## 🆘 الدعم

إذا واجهت أي مشكلة، اقرأ ملفات النشر المناسبة:
- **MOVESTAX-DEPLOY.md** لـ Movestax
- **VERCEL-DEPLOY.md** لـ Vercel
- **DEPLOYMENT-GUIDE.md** لـ Render و VPS
