# 🚀 الدليل الكامل للنشر على Vercel

دليل خطوة بخطوة لنشر موقع الهيئة السورية للاختصاصات الطبية على Vercel مع قاعدة بيانات PostgreSQL مجانية.

## 📋 المتطلبات

1. حساب GitHub (مجاني): https://github.com/signup
2. حساب Vercel (مجاني): https://vercel.com/signup
3. حساب Neon PostgreSQL (مجاني): https://neon.com/signup

---

## 🗄️ الخطوة 1: إنشاء قاعدة بيانات PostgreSQL مجانية

### 1.1 التسجيل في Neon
1. اذهب إلى https://neon.com/signup
2. اضغط **Sign up with GitHub** (أو Google)
3. أكد التسجيل

### 1.2 إنشاء مشروع جديد
1. اضغط **New Project**
2. املأ البيانات:
   - **Project name**: `syrian-board`
   - **Database name**: `board`
   - **Region**: اختر الأقرب (Frankfurt لأوروبا، Singapore لآسيا)
3. اضغط **Create Project**

### 1.3 نسخ رابط الاتصال
1. في صفحة المشروع، ابحث عن **Connection String**
2. انسخ الرابط الذي يبدأ بـ `postgresql://`
3. سيكون بالشكل:
   ```
   postgresql://neondb_owner:npg_xxxxxxxx@ep-xxx-xxx.eu-central-1.aws.neon.tech/board?sslmode=require
   ```
4. احتفظ به — ستحتاجه في الخطوة 3

---

## 📦 الخطوة 2: رفع المشروع إلى GitHub

### 2.1 إنشاء مستودع
1. اذهب إلى https://github.com/new
2. **Repository name**: `syrian-board`
3. اختر **Private** (مستودع خاص)
4. اضغط **Create repository**

### 2.2 رفع الملفات
على جهازك، فك ضغط `syrian-board-deploy.zip`:

```bash
# فك الضغط
unzip syrian-board-deploy.zip -d syrian-board
cd syrian-board

# مهم: لا ترفع ملف .env — احذفه
rm -f .env

# إنشاء المستودع
git init
git add .
git commit -m "Initial commit: Syrian Board Change Specialty 2025-2026"

# اربطه بـ GitHub (استبدل USERNAME باسمك)
git branch -M main
git remote add origin https://github.com/USERNAME/syrian-board.git
git push -u origin main
```

### 2.3 (بديل) استخدام GitHub CLI
لو عندك GitHub CLI:
```bash
gh repo create syrian-board --private --source=. --push
```

---

## ▲ الخطوة 3: نشر التطبيق على Vercel

### 3.1 التسجيل في Vercel
1. اذهب إلى https://vercel.com/signup
2. اضغط **Continue with GitHub**
3. اسمح لـ Vercel بالوصول لمستودعاتك

### 3.2 استيراد المشروع
1. في لوحة Vercel، اضغط **Add New...** → **Project**
2. ابحث عن `syrian-board` واضغط **Import**

### 3.3 إضافة متغيرات البيئة (مهم جداً!)

قبل الضغط على Deploy، انزل لقسم **Environment Variables** وأضف:

| الاسم | القيمة |
|------|-------|
| `DATABASE_URL` | الصق رابط Neon الذي نسخته من الخطوة 1.3 |
| `ADMIN_PASSWORD_HASH` | `$2b$10$M7C51jnRh6HyPEf8Tc1FiuXzfzT760N4DRbvSZY4kjbv3/k89JFCi` |
| `JWT_SECRET` | `SyrianBoard_2026_Production_Secret_Min_32_Chars_Long_String_DoNotShare` |

### 3.4 النشر
1. اضغط **Deploy**
2. انتظر 2-3 دقائق
3. ستحصل على رابط مثل: `https://syrian-board-xxx.vercel.app`

---

## ✅ الخطوة 4: التحقق من النشر

### 4.1 اختبار الصفحة الرئيسية
افتح الرابط:
```
https://syrian-board-xxx.vercel.app
```
يجب أن ترى استمارة الهيئة السورية.

### 4.2 اختبار لوحة الإدارة
افتح:
```
https://syrian-board-xxx.vercel.app/syrian-commission-for-medical-specialties
```
- كلمة المرور: `board-admin-2026`

### 4.3 اختبار قاعدة البيانات
أرسل طلباً تجريبياً من الاستمارة، ثم تحقق من ظهوره في لوحة الإدارة.

---

## 🎉 النتيجة

- ✅ موقع يعمل بـ HTTPS
- ✅ قاعدة بيانات PostgreSQL دائمة
- ✅ CDN عالمي سريع
- ✅ نشر تلقائي عند كل `git push`
- ✅ معاينات فورية لكل فرع

---

## 🔒 تغيير كلمة مرور الإدارة (مهم للإنتاج)

### 1. توليد كلمة مرور جديدة
```bash
node scripts/generate-hash.js "your-new-strong-password"
```

### 2. تحديث Vercel
1. Vercel → Project → Settings → Environment Variables
2. عدّل `ADMIN_PASSWORD_HASH` بالقيمة الجديدة
3. اضغط **Save**
4. Deployments → آخر deployment → **Redeploy**

---

## 🆘 استكشاف الأخطاء

### "Database connection failed"
- تأكد أن `DATABASE_URL` يبدأ بـ `postgresql://` (ليس `file:`)
- تأكد من إضافة `?sslmode=require` لنهاية الرابط
- تأكد أن قاعدة بيانات Neon نشطة

### "Prisma Client not generated"
- في Vercel → Project → Settings → Build Command، يجب أن يكون:
  ```
  npx prisma generate && next build
  ```

### "Migration failed"
- احذف كل الجداول في Neon SQL Editor:
  ```sql
  DROP TABLE IF EXISTS "Application" CASCADE;
  ```
- أعد النشر على Vercel

### "صفحة فارغة بعد النشر"
- راجع سجلات Vercel:
  1. Vercel → Project → Deployments
  2. اضغط على آخر deployment
  3. اضغط **Build Logs** أو **Runtime Logs**

### "Neon connection limit reached"
- استخدم Pooled connection string من Neon (يحتوي على `-pooler`)

---

## 📊 حدود الخطة المجانية

### Vercel (Hobby Plan)
- ✅ 100GB نقل بيانات شهرياً
- ✅ 100 ساعة بناء شهرياً
- ✅ HTTPS مجاني
- ✅ معاينة فورية لكل commit

### Neon (Free Plan)
- ✅ 100 مشروع
- ✅ 1GB تخزين لكل مشروع
- ✅ 100 ساعة حساب شهرياً
- ⚠️ يتوقف بعد 5 دقائق من عدم النشاط (يستيقظ تلقائياً)

هذه الحدود كافية جداً لموقع الهيئة السورية.

---

## 💡 نصائح مهمة

1. **لا ترفع ملف `.env` إلى GitHub أبداً** — احذفه قبل `git add`
2. **غيّر كلمة المرور الافتراضية** قبل النشر الرسمي
3. **اعمل backup لقاعدة البيانات** شهرياً من لوحة Neon
4. **راقب الاستخدام** في لوحة Vercel و Neon

بالتوفيق! 🎉
