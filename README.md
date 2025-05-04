
# مشروع Eat-Me-Pi

## معلومات المشروع

**URL**: https://lovable.dev/projects/a4068971-3b7c-465e-9229-a5ec131e33e4

هذا المشروع عبارة عن تطبيق لطلب الطعام ودفع ثمنه باستخدام عملة Pi الرقمية.

## خريطة المشروع

```
├── public/                 # الملفات العامة والصور
│   ├── manifest.json       # ملف التطبيق للأجهزة المحمولة
│   ├── lovable-uploads/    # الصور المخزنة
│   └── robots.txt          # إعدادات محركات البحث
│
├── src/
│   ├── backend/            # الخدمات الخلفية والتكاملات
│   │   ├── integrations/   # تكاملات مع خدمات خارجية (مثل Supabase)
│   │   └── services/       # خدمات API
│   │
│   ├── components/         # مكونات واجهة المستخدم القابلة لإعادة الاستخدام
│   │   ├── food-provider/  # مكونات خاصة بمزودي الطعام
│   │   ├── header/         # مكونات رأس الصفحة
│   │   ├── home/           # مكونات الصفحة الرئيسية
│   │   └── ui/             # مكونات واجهة المستخدم الأساسية (shadcn-ui)
│   │
│   ├── config/             # ملفات الإعدادات
│   │
│   ├── contexts/           # سياقات React للحالة العامة
│   │   ├── homefood/       # سياقات خاصة بـ HomeFood
│   │   ├── AppProvider.tsx # مزود السياق الرئيسي
│   │   ├── LanguageContext.tsx # سياق اللغة
│   │   └── PiAuthContext.tsx # سياق مصادقة Pi
│   │
│   ├── frontend/           # مكونات واجهة المستخدم للأجهزة المحمولة
│   │
│   ├── hooks/              # hooks مخصصة
│   │
│   ├── lib/                # وظائف وأدوات مساعدة
│   │
│   ├── pages/              # صفحات التطبيق
│   │   ├── Cart.tsx        # صفحة سلة المشتريات
│   │   ├── HomeFood.tsx    # صفحة الطعام المنزلي
│   │   ├── Index.tsx       # الصفحة الرئيسية
│   │   ├── Mining.tsx      # صفحة التعدين
│   │   ├── Restaurants.tsx # صفحة المطاعم
│   │   └── Wallet.tsx      # صفحة المحفظة
│   │
│   ├── types/              # تعريفات TypeScript
│   │
│   ├── App.tsx             # مكون التطبيق الرئيسي
│   └── main.tsx            # نقطة الدخول للتطبيق
│
├── index.html              # ملف HTML الرئيسي
├── tailwind.config.ts      # إعدادات Tailwind CSS
├── capacitor.config.ts     # إعدادات Capacitor للأجهزة المحمولة
└── package.json            # تبعيات المشروع وأوامر التشغيل
```

## كيفية تشغيل المشروع

### المتطلبات الأساسية
- Node.js (النسخة 18 أو أحدث)
- npm (مدير حزم Node.js)

### خطوات التشغيل

1. استنساخ المشروع:
```sh
git clone https://lovable.dev/projects/a4068971-3b7c-465e-9229-a5ec131e33e4
cd eat-me-pi
```

2. تثبيت التبعيات:
```sh
npm install
```

3. تشغيل المشروع في وضع التطوير:
```sh
npm run dev
```

4. بناء المشروع للإنتاج:
```sh
npm run build
```

5. معاينة نسخة الإنتاج محلياً:
```sh
npm run preview
```

## ربط المشروع بشبكة Pi

المشروع يستخدم Pi Network SDK للتفاعل مع شبكة Pi. تم تضمين SDK في ملف `index.html`:
```html
<script src="https://sdk.minepi.com/pi-sdk.js"></script>
```

### تكوين SDK

لربط التطبيق بشبكة Pi، يجب إعداد Pi SDK في ملف `config/piNetwork.ts`:

```typescript
// تهيئة SDK
Pi.init({ version: "2.0", sandbox: true });

// المصادقة
const authenticateUser = async () => {
  try {
    const authResponse = await Pi.authenticate(['username', 'payments'], onIncompletePaymentFound);
    return authResponse;
  } catch (error) {
    console.error("Authentication error:", error);
    return null;
  }
};

// إنشاء عملية دفع
const createPayment = async (amount, memo) => {
  try {
    const payment = await Pi.createPayment({
      amount,
      memo,
      metadata: { orderId: generateOrderId() }
    });
    return payment;
  } catch (error) {
    console.error("Payment creation error:", error);
    return null;
  }
};

// إكمال عملية الدفع
const completePayment = async (paymentId) => {
  try {
    const payment = await Pi.payments.complete(paymentId);
    return payment;
  } catch (error) {
    console.error("Payment completion error:", error);
    return null;
  }
};
```

### متغيرات البيئة

قد تحتاج إلى إعداد ملف `.env` للإعدادات الحساسة مثل مفاتيح API:

```
PI_API_KEY=your_pi_api_key
SUPABASE_URL=your_supabase_url
SUPABASE_ANON_KEY=your_supabase_anon_key
```

## ربط المشروع بـ Supabase

المشروع يستخدم Supabase كقاعدة بيانات وخدمة تخزين. التكوين موجود في `backend/integrations/supabase/client.ts`.

للاتصال بـ Supabase، أضف مفاتيح API الخاصة بك في ملف `.env`:

```
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-anon-key
```

## نشر التطبيق

لنشر المشروع، يمكنك استخدام Netlify أو Vercel:

### باستخدام Netlify
1. قم بالتسجيل في [Netlify](https://netlify.com)
2. اربط حسابك بـ GitHub
3. اختر المشروع واضبط أوامر البناء:
   - Build command: `npm run build`
   - Publish directory: `dist`

### باستخدام Lovable
ببساطة افتح [Lovable](https://lovable.dev/projects/a4068971-3b7c-465e-9229-a5ec131e33e4) واضغط على Share -> Publish.

## التقنيات المستخدمة

- **إطار العمل الأساسي**: Vite, React, TypeScript
- **واجهة المستخدم**: shadcn-ui, Tailwind CSS
- **إدارة الحالة**: React Context, TanStack Query
- **البناء للأجهزة المحمولة**: Capacitor
- **قاعدة البيانات**: Supabase
- **الاتصالات**: Pi Network SDK

## المميزات الرئيسية

- استعراض المطاعم والطعام المنزلي
- سلة مشتريات وعملية دفع
- دعم الدفع باستخدام عملة Pi
- محفظة Pi ومعلومات الرصيد
- نظام المكافآت والتعدين
- دعم متعدد اللغات (الإنجليزية والعربية)
- وضع الظلام والنور
- واجهة مستخدم متوافقة مع الأجهزة المحمولة

## حقوق الملكية

© 2025 Eat-Me-Pi. جميع الحقوق محفوظة.
