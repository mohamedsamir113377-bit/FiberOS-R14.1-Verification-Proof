# FiberOS R14.1 — Sales Core: Verification Proof

> **هذه الصفحة للإثبات فقط — Proof-only page.**
> هذا المستودع **لا يحتوي أي كود مصدري ولا أي أرشيف تسليم**. المنتج نفسه مستودع خاص (private) ولا يمكن تحميله من هنا إطلاقاً.
> هذا المستودع موجود لغرض واحد: أن يطمئن المشتري إلى أن جميع بوابات التحقق والاختبارات نجحت على الشجرة النهائية المُسلَّمة، ثم يعود إلى موقع البيع ليدفع ويحمل الحزمة.

---

## 1) الشجرة المُوثَّقة (Verified tree)

| البند | القيمة |
|---|---|
| الحزمة | FiberOS R14.1 — Sales Core |
| الإصدار | `1.0.0-rc.1+commercial.2026-09-25` |
| الالتزام المرجعي | `dac01a2ef20b37f5262af151bb200479e2e32e75` |
| تاريخ الالتزام | 2026-09-30 |
| سلسلة الترحيلات الإنتاجية | `001–116` (متجاورة، مسنودة بـ SHA-256 ledger) |

---

## 2) نتائج الاختبار الحقيقية (من جلسة التحقق 2026-09-30)

بيئة الجلسة: **Node.js `v24.21.0`** (الإصدار المُعلن في `package.json` و`.nvmrc`)، **Redis `7.0.15`** حي، تثبيت من الـ lockfile عبر `npm ci`.

مخرج `npm test` الحرفي:

```
ℹ tests 316
ℹ suites 1
ℹ pass 316
ℹ fail 0
ℹ cancelled 0
ℹ skipped 0
ℹ todo 0
ℹ duration_ms 5690.246808
```

**316/316 اختباراً نجح — صفر فشل، صفر تخطّي.**

مخرج بوابة `npm run verify` الكاملة (exit code = 0):

```
Release integrity:                PASS
Migration sequence + SHA ledger:  PASS — 116/116 contiguous
Production-console boundary:      PASS
Architecture boundary/map:        PASS — 41 packages
JavaScript syntax:                PASS
Root-cause contracts:             PASS
Formal verification:              PASS_CORE — 259 checks
Capability parity:                PASS — 101 capabilities
Domain neutrality:                PASS — multi-buyer
i18n quality gate:                PASS — 114 locales, 101 keys, 2 verified
Manifest parity gate:             PASS — 1155 files
Commercial release gate:          ok: true
```

سجل CI الحقيقي على المستودع الخاص (run IDs للتوثيق):

| Run ID | Workflow | الالتزام | النتيجة |
|---|---|---|---|
| `36676646280` | verify-gate | `dac01a2` | **success** |
| `36676646302` | sales-security | `dac01a2` | **success** |

---

## 3) البصمة القابلة لإعادة الإنتاج (Reproducible build fingerprint)

الحزمة تُبنى الآن ببنّاء حتمي مُرفق داخلها (`scripts/build-delivery-archive.py`): ترتيب إدخالات ثابت، أزمنة مثبّتة من `SOURCE_DATE_EPOCH`، أذونات و`uid/gid` ثابتة، مستوى ضغط ثابت، وبدون أي حقول إضافية.

بنى الحزمة مرتين متتاليتين من الشجرة نفسها:

```
$ python3 scripts/build-delivery-archive.py --output final_A.zip \
    --commit dac01a2ef20b37f5262af151bb200479e2e32e75
sha256: 6abd1fa870dbf5ddbe7369da08e3b901764a42e55f3a5c98ce42c08d95eb87ff

$ python3 scripts/build-delivery-archive.py --output final_B.zip \
    --commit dac01a2ef20b37f5262af151bb200479e2e32e75
sha256: 6abd1fa870dbf5ddbe7369da08e3b901764a42e55f3a5c98ce42c08d95eb87ff
```

**الهاشان متطابقان تماماً:**

```
6abd1fa870dbf5ddbe7369da08e3b901764a42e55f3a5c98ce42c08d95eb87ff  final_A.zip
6abd1fa870dbf5ddbe7369da08e3b901764a42e55f3a5c98ce42c08d95eb87ff  final_B.zip
```

أي أن إعادة بناء المشتري للحزمة المُسلَّمة ستنتج **نفس البصمة بالبايت**. داخل الأرشيف ملف `MANIFEST_BUILD_STAMP.txt` يوثّق الطابع:

```
package=FiberOS R14.1 Sales Core
build.epoch=1790746233
build.stamp=2026-09-30T05:30:33Z
tree.commit=dac01a2ef20b37f5262af151bb200479e2e32e75
```

---

## 4) حدود الصدق (Honesty boundaries)

- «صفر أخطاء» تعني هنا: **جميع الاختبارات التزامنية (316/316) وجميع بوابات التحقق الساكنة نجحت** في بيئة التحقق المرجعية — وليست ضماناً خالياً من أي عطل في كل بيئة تشغيل ممكنة.
- التحقق الساكن لا يُعوّض قبول البيئة الخاصة بالمشتري: PostgreSQL/PostGIS، OIDC/JWKS، TLS/ingress، Redis/HA، التخزين، والنسخ الاحتياطي — كلها مسؤولية المشتري قبل التشغيل الحقيقي (`FINAL_VERIFICATION_STATUS.md` داخل الحزمة).
- مُقيّد المعدل المدمج هو `PROCESS_LOCAL` وليس مُقيّداً مشتركاً بين مثيلات متعددة.
- عزل المستأجرين مُفروض على مستوى قاعدة البيانات (`tenant_id` + RLS)؛ ادعاءات JWT التنظيمية استشارية فقط.

---

## 5) ما لا يوجد هنا

- ❌ لا كود مصدري.
- ❌ لا أرشيف تسليم (ZIP).
- ❌ لا Releases ولا Artifacts.
- ✅ فقط: سجل الإثبات أعلاه + تعليمات إعادة البناء.

المصدر الكامل موجود في مستودع خاص. بعد الشراء من موقع البيع تستلم حزمة `FiberOS-R14_1-SALES-CORE-rc1` الموقوطة بهذه البصمة، ويمكنك إعادة بنائها بأمر واحد والتحقق من الهاش بنفسك.
