# ZOSING TOPUP — Starter Website

គម្រោង HTML/CSS/JavaScript + Node.js/Express ដែលមាន UI សម្រាប់ទូរសព្ទ និង Desktop។

## មានអ្វីខ្លះ?
- Homepage dark gaming UI ពណ៌ខ្មៅ/ទឹកក្រូច
- Search និង Filter ហ្គេម
- Game cards និង Order form
- Backend API សម្រាប់បង្កើត/មើល Order
- ABA PayWay integration endpoints ជាគ្រោងសុវត្ថិភាព (QR ត្រូវបានបិទរហូតដល់បញ្ជាក់ Hash formula ផ្លូវការ)
- `.env.example` សម្រាប់ environment variables

> សំខាន់៖ នេះជា starter project មិនមែន production-ready commerce system ទេ។ Order storage នៅក្នុង memory ហើយបាត់ពេល server restart។ មុនបើកទទួលប្រាក់ពិត ត្រូវបន្ថែម PostgreSQL, Admin authentication, rate limiting, logging, callback signature validation, និងការផ្ទៀងផ្ទាត់ Check Transaction API។

## 1. ដំណើរការក្នុងកុំព្យូទ័រ
ត្រូវមាន Node.js 20 ឬថ្មីជាងនេះ។

```bash
npm install
cp .env.example .env
npm start
```

API ដំណើរការតាម `http://localhost:3000`។ ដើម្បីបើក Frontend សាកល្បង អ្នកអាចបើក `public/index.html` ក្នុង browser។ បើប្រើ local file ហើយ API មិនភ្ជាប់បាន សូម host `public/` ដោយ static server ហើយកំណត់ `LYHENG_API_BASE` ក្នុង `index.html` ឬដាក់ Frontend និង API នៅ domain ដែលបានរៀបចំ CORS ត្រឹមត្រូវ។

## 2. GitHub Pages (Frontend)
1. បង្កើត GitHub repository ឧ. `zosing-topup`។
2. Upload files ក្នុងថត `public/` ទៅ root នៃ repository ឬកែ GitHub Pages source ឱ្យចង្អុលទៅថត `public/` ប្រសិនបើប្រើ branch-based deployment។
3. Settings → Pages → Deploy from branch → ជ្រើស `main` និង folder។
4. បន្ទាប់ពីបាន URL Frontend សូមកំណត់ `FRONTEND_ORIGIN` នៅ Render ជា URL នោះ។

Frontend ត្រូវការកំណត់ API URL ពេលប្រើ Render។ វិធីងាយ៖ កែបន្ទាត់ដំបូងនៃ `public/app.js` ដោយកំណត់ `const API_BASE = "https://YOUR-RENDER-SERVICE.onrender.com";` ជំនួស `""`។ កុំដាក់ Secret Key នៅទីនេះ។

## 3. Render (Backend)
1. Push project ទៅ GitHub។
2. Render → New → Web Service → ភ្ជាប់ repository។
3. Build Command: `npm install`
4. Start Command: `npm start`
5. Add Environment Variables:
   - `FRONTEND_ORIGIN` = GitHub Pages URL របស់អ្នក
   - `PAYWAY_ENV` = `sandbox`
   - `PAYWAY_MERCHANT_ID` និង `PAYWAY_API_KEY` ទុកទទេរហូតទទួលបានពី ABA
6. Deploy ហើយសាកល្បង `https://YOUR-RENDER-SERVICE.onrender.com/api/health`

## 4. ABA PayWay — មុនពេល Production
ឯកសារផ្លូវការ:
- QR API: https://developer.payway.com.kh/qr-api-14530840e0
- ABA QR API integration guide: https://developer.payway.com.kh/aba-qr-api-3158158f0
- Check Transaction API: https://developer.payway.com.kh/check-transaction-14530826e0

ត្រូវស្នើ Sandbox Merchant ID និង API Key ពី ABA មុន។ សម្រាប់ Production ត្រូវប្រើ Production credentials ដែល ABA អនុម័ត។

**មិនបានបញ្ចូល Hash formula ដោយការស្មានទេ។** ឯកសារ QR API ដែលអាចមើលឃើញពី public search បង្ហាញ fields និង endpoint ប៉ុន្តែមិនបានបង្ហាញលំដាប់ field ទាំងអស់សម្រាប់ការបង្កើត `hash` នៅក្នុងផ្នែកដែលអាចផ្ទៀងផ្ទាត់បាន។ សូមយក hash-string sequence និង callback verification rules ពី Merchant integration guide របស់ ABA។ បន្ទាប់មកបំពេញ `buildPaywayQrHash()` និង Check Transaction hash តាមឯកសារនោះ ហើយសាកល្បង Sandbox មុន Production។

Endpoint ផ្លូវការ QR API:
- Sandbox: `https://checkout-sandbox.payway.com.kh/api/payment-gateway/v1/payments/generate-qr`
- Production: `https://checkout.payway.com.kh/api/payment-gateway/v1/payments/generate-qr`

QR response របស់ PayWay អាចមាន `qrImage`, `qrString`, `abapay_deeplink` និង `status`។ ត្រូវបង្ហាញ QR និងត្រួតពិនិត្យការទូទាត់នៅ Backend។ ABA ណែនាំឱ្យប្រើ Callback និង Check Transaction API រួមគ្នា។

## 5. មុនពេលដាក់ប្រើពិត
- ប្រើ PostgreSQL ឬ database persistent
- Admin login + role-based access
- Validate និង rate-limit API
- បង្កើត hash តាមឯកសារ ABA ផ្លូវការពិតប្រាកដ
- Validate callback signature/fields តាម ABA guide
- Check Transaction API មុនសម្គាល់ថាបានបង់ប្រាក់
- បង្កើត idempotency ដើម្បីកុំឱ្យ Order/fulfillment ដំណើរការច្រើនដង
- កុំស្នើឱ្យអតិថិជនផ្តល់ password ឬ OTP របស់គណនីហ្គេម
- កុំបញ្ចូល `.env` ទៅ GitHub

## API ដែលមានក្នុង starter
- `GET /api/health`
- `POST /api/orders`
- `GET /api/orders/:id`
- `POST /api/payments/qr` (បិទដោយសុវត្ថិភាពរហូតដល់ផ្ទៀងផ្ទាត់ hash formula)
- `POST /api/payments/payway-callback` (ទទួល callback ប៉ុន្តែមិនសម្គាល់ថាបង់ប្រាក់រួចទេ)

## តម្លៃ Game Packages
តម្លៃក្នុងកូដគឺ Demo សម្រាប់ UI ប៉ុណ្ណោះ។ កែ `games` នៅ `public/app.js` និង `catalog` នៅ `src/server.js` ឱ្យត្រូវគ្នា។ ត្រូវផ្ទៀងផ្ទាត់តម្លៃ និងតំបន់ Game មុនដាក់លក់ពិត។
