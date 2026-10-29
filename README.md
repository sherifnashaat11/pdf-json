# لوحة الأدمن — بيانات المشتركين (PWA)

## الملفات
index.html
manifest.json
sw.js
make-icons.html
icons/
icon-192.png
icon-512.png
icon-maskable-512.png
data/ ← أنشئه (للملفات المشفّرة)
master.enc.gz
settings.json
history.enc.gz

text

## خطوات النشر الأولى
1. أنشئ مستودعًا **عامًا** على GitHub.
2. افتح `make-icons.html` في المتصفح → نزّل 3 أيقونات → ضعها في `icons/`.
3. ارفع `index.html`, `manifest.json`, `sw.js`, `README.md`, ومجلد `icons/`.
4. أنشئ مجلد `data/` (فارغًا مؤقتًا).
5. Settings → Pages → Deploy from branch → `main` / root → Save.
6. افتح `https://USERNAME.github.io/REPO/` على الهاتف → ثبّت التطبيق.

## أول مرة — بناء Master
1. ارفع ملفات PDF/Excel → اضبط الأعمدة → حفظ في Master (يُحفظ محليًا في IndexedDB).
2. اضغط **"🔒 تصدير Master مشفّر"** → أدخل كلمة السر مرتين → يُنزَّل `master.enc.gz`.
3. ارفعه إلى `data/master.enc.gz` في المستودع.

## كل شهر
1. رفع الملف الشهري → استيراد → مراجعة المطابقات → تطبيق.
2. تصدير **History مشفّر** → ارفعه إلى `data/history.enc.gz`.
3. لو تغيّرت بيانات العملاء → حدّث Master وأعد تصديره مشفّرًا.

## من جهاز آخر
1. افتح التطبيق → أدخل `owner/repo/branch` في قسم GitHub → حفظ.
2. اضغط **"🔒 تحميل Master من GitHub"** → أدخل كلمة السر.

## ملاحظات أمان
- `master.enc.gz` مشفّر بـ AES-GCM 256 + PBKDF2 (100,000 دورة).
- **لا يمكن استرجاع كلمة السر إن نسيت** — احفظها في مكان آمن.
- خيار "تذكرني" يحفظها مشفّرة محليًا — استخدمه فقط على جهازك الشخصي.
- النسخة غير المشفّرة (`master_PLAIN`) للاستخدام المحلي فقط — لا ترفعها.

## هيكل البيانات المشفّرة
كل ملف `.enc.gz` = `gzip( salt(16) + iv(12) + AES-GCM-ciphertext )`