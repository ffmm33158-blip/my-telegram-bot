# my-telegram-bot
بوت الجامعة

**إعداد n8n وتشغيل الـ EduBot**

- **الهدف:** استيراد workflow الخاص بـ `EduBot Telegram Teaching Assistant and Group Manager` إلى n8n وتشغيله للتواصل مع Telegram واستخدام نموذج Google Gemini.

**الملفات المضافة:**
- `n8n/workflows/EduBot Telegram Teaching Assistant and Group Manager.json` : ملف الـ workflow (تصدير من n8n).
- `docker-compose.yml` : تكوين لتشغيل `n8n` مع `Postgres` محلياً.
- `.env.example` : أمثلة المتغيّرات البيئة (ضع القيم الحقيقية هنا أو في `.env`).

**خطوات سريعة للتشغيل (محلي عبر Docker Compose):**
1. انسخ `.env.example` إلى `.env` واملأ المتغيرات:

```
cp .env.example .env
# ثم حرّر .env وأدخل قيمك:
#   POSTGRES_PASSWORD=قيمة
#   TELEGRAM_BOT_TOKEN=توكن_البوت
#   GOOGLE_PALM_API_KEY=مفتاح_الـAI
```

2. شغّل الخدمات:

```bash
docker compose up -d
```

3. افتح المتصفح على `http://localhost:5678` للدخول إلى واجهة n8n.

4. استورد الـ workflow:
	- في n8n UI: اضغط على زر القائمة → `Import` → احفظ ملف `n8n/workflows/EduBot Telegram Teaching Assistant and Group Manager.json` أو الصق المحتوى.

5. أنشئ Credentials المطلوبة في n8n:
	- اذهب إلى `Credentials` → `Create`.
	- أنشئ اعتماد `Telegram` (اسم افتراضي في الـ workflow: **Telegram account**) وضع فيه `Bot Token` الخاص بك (`TELEGRAM_BOT_TOKEN`).
	- أنشئ اعتماد `Google Palm API` (أو موفر Gemini المناسب) باسم **Google Gemini(PaLM) Api account** وضع فيه `API Key` من `GOOGLE_PALM_API_KEY`.

مهم: ملف الـ workflow يحتوي مراجع للاعتمادات (`name` و`id`) فقط — القيم السرّية (التوكن والمفاتيح) غير مضمنة داخل ملف JSON، لذا يجب إدخالها في واجهة n8n يدوياً أو استيراد اعتمادات منفصلة.

**اختبار سريع:**
- بعد استيراد workflow وربطه بالـ credentials، فعّل الـ workflow (شغّله) في واجهة n8n.
- افتح المحادثة في Telegram وجرّب إرسال رسالة أو قم بإرسال ملف PDF لاختبار مسار التحميل والمعالجة.

**هل تريد أن أعمل commit & push للتغييرات؟**
- أستطيع عمل commit للتغييرات في الفرع `main` ودفعها إلى الريبو إذا سمحت. 

**ملاحظات أمان:**
- لا تضع التوكن أو مفاتيح الـ AI في الريبو العام. استخدم ملفات `.env` محلية أو سرّية في CI.

