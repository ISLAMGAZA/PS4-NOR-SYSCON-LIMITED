# SECURITY & ANTI-THEFT PLAN
# خطة الحماية ومنع السرقة

**Project / المشروع:** PS4 NOR-SYSCON EASY TOOL
**Owner / المالك:** ISLAM JA - github.com/ISLAMGAZA

---

## Reality check - الحقيقة أولًا

**EN:** No software protection is unbreakable. If you publish code or binaries,
a determined attacker can copy it. The goal is therefore NOT impossibility, but:
1. Make casual theft inconvenient.
2. Make ownership provable.
3. Make removal fast and cheap when theft happens.

**AR:** لا توجد حماية برمجية غير قابلة للكسر. إن نشرتَ كودًا أو ملفات تنفيذية،
يمكن للمخترق المصمّم نسخها. لذلك الهدف **ليس الاستحالة** بل:
1. جعل السرقة العابرة أمرًا مزعجًا.
2. جعل الملكية قابلة للإثبات.
3. جعل الإزالة سريعة ورخيصة عند حدوث السرقة.

---

## Layer 1 - Legal (STRONGEST, free) / الطبقة القانونية (الأقوى ومجانية)

| Action | Why |
|---|---|
| Proprietary LICENSE (no GPL for the app) | GPL/permissive licenses ALLOW resale. A proprietary license does not. |
| Copyright headers in every source file | Proves authorship, visible in any copy |
| Copyright + HWID in the UI and in generated HTML reports | Evidence travels with every output the thief produces |
| GitHub DMCA takedown | Free, fast, and GitHub responds to valid DMCA notices |

**AR:** الترخيص الاحتكاري + حقوق النشر في كل ملف + إشعار DMCA على GitHub هي
أقوى طبقة، ومجانية بالكامل. ترخيص GPL **يسمح** بإعادة البيع — لذلك لا يُستخدم هنا.

---

## Layer 2 - What NOT to publish (protects the real asset)
## الطبقة 2 - ما لا يُنشر (تحمي الأصل الحقيقي)

**EN:** The most valuable asset is NOT the code - it is the donor dataset
(thousands of real dumps). Never commit it.

**AR:** الأصل الأثمن ليس الكود بل **قاعدة الدمبات** (آلاف الدمبات الحقيقية).
لا تُرفع إلى Git أبدًا.

Must stay OUT of the repository / يجب أن تبقى خارج المستودع:
- `DONORS/` (all dumps) / كل الدمبات
- `*.nhd`, `*.bin` dumps
- `.lic_secret` (HMAC signing secret)
- `blob_calibration*.json` (derived from donors - keep local)
- `hash_db.json` (local; regenerate or distribute a minimal copy)

Verify with / تحقّق عبر: `.gitignore`

---

## Layer 3 - Runtime licensing (already implemented)
## الطبقة 3 - الترخيص أثناء التشغيل (مُنفَّذ بالفعل)

`ps4_nsrepair/license.py`:

| Feature | Detail |
|---|---|
| HWID lock | Stable per-machine ID (MachineGuid / machine-id / IOPlatformUUID) |
| Attempt-based trial | **5 repair attempts** (fairer than a time limit) |
| Signed keys | `v1:HWID:EXP:FEAT:DONOR:SIG` - HMAC-SHA256, secret in `.lic_secret` |
| Fail-closed counter | Missing/corrupt/tampered usage file => 0 attempts left |
| Expiry support | `EXP=YYYYMMDD` or `0` permanent |
| Feature tiers | `LTD` (limited) / `FULL` |

**AR:** القفل بـ HWID + تجربة **5 محاولات** + مفاتيح موقّعة + عدّاد يُغلق عند
العبث. هذه ردع للمستخدم العادي، لا حماية ضد من يملك المصدر.

---

## Layer 4 - Watermarking (deterrent + evidence)
## الطبقة 4 - العلامة المائية (ردع + دليل)

**EN:** Every generated HTML report and output filename can carry:
- Owner name / brand
- Tool name + build number
- Target console serial (already present) and HWID of the producing PC

If a thief resells outputs, the evidence points back to the source.

**AR:** كل تقرير HTML واسم ملف يمكن أن يحمل: اسم المالك + اسم الأداة + رقم
البناء + سريال الجهاز + HWID الجهاز المُنتِج. إن باع السارق النواتج، فالدليل
يقود إلى المصدر.

---

## Layer 5 - Distribution policy
## الطبقة 5 - سياسة التوزيع

| Option | Risk | Benefit |
|---|---|---|
| **Publish binaries only (recommended)** | Low - source stays private | Users can still use it; code not reusable |
| Publish source (open) | High - can be rebranded/resold | Community trust, contributions |
| Publish source + proprietary license | Medium - license deters honest actors | Trust + legal ground |

**Recommendation / التوصية:** publish **binaries + documentation** (README,
OPTIONS_GUIDE, FAULTS). Keep `ps4_nsrepair/` source private, or publish only with
a clear proprietary license and DMCA readiness.

---

## Layer 6 - Monitoring
## الطبقة 6 - المراقبة

- Search GitHub periodically for the project name / ابحث دوريًا عن اسم المشروع
- Set a Google Alert for "PS4 NOR-SYSCON EASY TOOL"
- Keep dated evidence: commit history, release MD5s (already in `RELEASE/*.md5`)
- The EXE integrity check (sidecar `.md5`) also detects tampered copies

---

## What we will NOT do
## ما لن نفعله

- No telemetry / no phone-home / no user dump uploading (privacy + trust)
- No destructive "anti-piracy" behavior that could damage a customer's console
- No hidden actions; every protection is documented here

**AR:** لا تتبّع، لا إرسال بيانات، لا رفع دمبات المستخدمين (خصوصية وثقة).
ولا سلوك تدميري قد يضرّ جهاز العميل. كل الحماية موثّقة بشفافية.

---

## If theft happens - إن حدثت سرقة

1. Collect evidence (URL, screenshots, dates)
2. File a GitHub DMCA takedown: https://github.com/legal/dmca
3. Contact the host if hosted elsewhere
4. Publish your proof of ownership (repo history, this file, LICENSE)

---

Copyright (c) ISLAM JA. All rights reserved.
جميع الحقوق محفوظة لـ ISLAM JA.

---

## Official contact / التواصل الرسمي
- Email / البريد: **islamabuaker83@gmail.com** (send HWID for licensing / أرسل HWID للترخيص)
- TikTok: https://tiktok.com/@ps4easytool
- GitHub: https://github.com/ISLAMGAZA/PS4-NOR-SYSCON-LIMITED

---

## UPDATE - 2026-09-12: RSA license keys (works on EVERY machine)

license.key is now signed with RSA-2048 (owner PRIVATE key). The tool embeds
ONLY the PUBLIC key, so any copy verifies licensed keys with NO secret present,
yet nobody can forge one. Keys remain HWID-locked (one device).
The trial usage counter trusts its file when no secret exists (fresh install =
5 attempts); tampering only resets the trial.
Never share: .lic_secret, .lic_rsa, DONORS donor dumps.
