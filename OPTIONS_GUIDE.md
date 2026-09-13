# PS4 NOR-SYSCON EASY TOOL - Main Options Guide
# دليل الخيارات الرئيسية - PS4 NOR-SYSCON EASY TOOL

Owner / المالك: **ISLAM JA** - All rights reserved / جميع الحقوق محفوظة.
Version / الإصدار: build 0052 (beta).
License / الترخيص: see `LICENSE` - commercial redistribution prohibited /
انظر `LICENSE` - يُمنع إعادة التوزيع التجاري.

---

## Core guarantee (applies to EVERY option)
## الضمان الأساسي (ينطبق على كل خيار)

**EN:** The tool NEVER modifies your original dump. Every result is written to a
NEW file in `OUTPUT/`. The source file is only ever read.

**AR:** الأداة لا تعدّل ملفك الأصلي أبدًا. كل ناتج يُكتب في ملف جديد داخل
`OUTPUT/`. الملف المصدر يُقرأ فقط.

Additional absolute rules / قواعد مطلقة إضافية:

| EN | AR |
|---|---|
| A donor EAP key can never decrypt your HDD | مفتاح EAP من مانح لا يمكنه فك تشفير قرصك أبدًا |
| CoreOS slot-switch MUST be paired with the Syscon SNVS patch | تبديل خانة CoreOS يجب أن يُقرن بترقيع SNVS للسيسكون |
| Nothing is written on a guessed firmware | لا يُكتب أي شيء بناءً على إصدار مُخمّن |

---

## 1 - NOR Analyser

**EN:** Read-only inspection of a PS4 NOR dump: full validation, console info
(SKU / serial / MAC / motherboard / firmware), A/B redundancy, anomaly
discovery, diagnosis, and EAP key check. Produces an HTML report.

**AR:** فحص للقراءة فقط لدمب النور: تحقق كامل، معلومات الجهاز (الطراز/السريال/
المـاك/اللوحة/الإصدار)، التكرار A/B، اكتشاف الشذوذ، التشخيص، وفحص مفتاح EAP.
يُنتج تقرير HTML.

**Output / الناتج:** `<name>.report.html` (next to the dump / بجانب الدمب).

---

## 2 - Syscon Analyser

**EN:** Read-only inspection of a 512KB Syscon dump: validation, firmware info,
DEBUG state, SNVS/NVS viewers, patchability check. Also hosts the Syscon
repair operations - including **`F` = FULL Syscon Repair (2 outputs)**.

**AR:** فحص للقراءة فقط لدمب السيسكون (512KB): تحقق، معلومات البرمجية، حالة
الديباج، عارضا SNVS/NVS، فحص قابلية الترقيع. ويحتوي أيضًا عمليات إصلاح
السيسكون - ومنها **`F` = إصلاح السيسكون الكامل (مخرجان)**.

### F - Full Syscon Repair (safe, 2 outputs) / إصلاح السيسكون الكامل
| Output | What it does | When to use |
|---|---|---|
| `<orig>_<rand>_syscon_revert` | Drops ONE FW step (the newest FW-update group is FF'd **in place**, layout preserved) + `DEBUG=0x85` | With a NOR slot switch / downgrade |
| `<orig>_<rand>_syscon_repair` | Keeps every FW step, removes only SAMU's **pending-update intent** + `DEBUG=0x85` | Boot repair without changing FW |

- The newest group is chosen by **write counter** (not by list position), so it
  is correct even on a wrapped SNVS ring buffer.
- The original dump is **never** modified; both outputs are new files.
- Flash order: **NOR first, then the matching Syscon output**.

**Note / ملاحظة:** Syscon must be paired with the CoreOS patch - never use it
separately (BwE rule). / السيسكون يجب أن يُقرن بترقيع CoreOS - لا يُستخدم
منفردًا أبدًا (قاعدة BwE).

**Hidden (not shown in the menu):** 6 (SNVS patch A-E), 8 (factory reset),
12 (surgical patch), 13 (strip SYS2), 14 (clean SNVS for downgrade) - they use
the block-model writer or destroy identity, so they are no longer offered.

---

## 3 - UART Log Diagnosis

**EN:** Interprets UART boot-log errors using the built-in error map
(`loadBios -8`, `checkUpdVersion`, `Panic EAP Key`, `BlStorageHeader`,
`SAMU Enter/Leave`, `IDPS`, `CE-35888-2`, `SU-30631-3` ...). Tells you the likely
cause and the matching repair action.

**AR:** يفسّر أخطاء سجل إقلاع UART باستخدام خريطة الأخطاء المدمجة
(`loadBios -8`، `checkUpdVersion`، `Panic EAP Key`، `BlStorageHeader`،
`SAMU Enter/Leave`، `IDPS`، `CE-35888-2`، `SU-30631-3` ...). يخبرك بالسبب
المحتمل والإجراء المناسب.

---

## 4 - Full Auto-Repair (BLOD)

**EN:** The main repair pipeline. Fixes blob corruption (EMC / EAP / Torus),
recovers A/B redundancy, repairs NVS, repairs the BIOS area, enables UART,
cleans system flags, and (optionally) switches the CoreOS slot. Produces two NOR
variants plus one HTML report each.

**AR:** مسار الإصلاح الرئيسي. يصلح تلف الـblobs (EMC / EAP / Torus)، يسترجع
التكرار A/B، يرمّم NVS، يصلح منطقة BIOS، يفعّل UART، ينظّف الأعلام، ويبدّل خانة
CoreOS (اختياريًا). يُنتج نسختين من النور + تقرير HTML لكل منهما.

**Output / الناتج:**
- `<name>_<rand>_...-slotas-is.BIN` - original slot kept / الخانة الأصلية محفوظة
- `<name>_<rand>_...-slotsw.BIN` - slot switched / الخانة مبدَّلة
- one `*.report.html` per output / تقرير HTML لكل ناتج

**EN:** Nothing is injected from a donor on a guess. If the detected firmware is
uncertain, blob replacement is refused.

**AR:** لا يُحقن شيء من مانح بناءً على تخمين. إذا كان الإصدار المكتشف غير مؤكد،
يُرفض استبدال الـblob.

---

## D - Downgrade (specialised - no other repairs)

**EN:** Downgrade only. Switches the CoreOS slot to the inactive (older) slot,
enables UART for verification, rebuilds the Syscon SNVS with the PREVIOUS
firmware-update group, and forces DEBUG=0x85 (required for downgrade to boot).

**AR:** داونجريد فقط. يبدّل خانة CoreOS إلى الخانة غير النشطة (الأقدم)، يفعّل
UART للتحقق، يعيد بناء SNVS بمجموعة التحديث السابقة، ويجبر `DEBUG=0x85`
(ضروري لإقلاع الداونجريد).

**Skipped / لا يفعله:** EMC, EAP, Torus, NVS, CoreOS header patches, flags,
donors. / لا يمسّ EAP و Torus و NVS وترقيعات CoreOS والأعلام والمانحين.

**Requires / يتطلب:** a paired Syscon (mandatory) / سيسكون مقترن (إلزامي).

---

## R - Restore from donor (severe damage)

**EN:** Structured restore for dumps too damaged for blob-level repair (e.g.
whole SAM_IPL/CoreOS regions wiped). Copies NON-identity regions from a matching
healthy donor, then re-applies THIS console's identity (serial, MAC, motherboard,
EAP keys, HDD info, NVS). Donor must match SKU + SouthBridge. CoreOS is restored
ONLY from a same-console backup.

**AR:** استعادة هيكلية للدمبات المتضررة بشدة (مثل مَسح مناطق SAM_IPL/CoreOS
بالكامل). ينسخ المناطق غير المرتبطة بالهوية من مانح سليم مطابق، ثم يُعيد هوية
هذا الجهاز (السريال، الماك، اللوحة، مفاتيح EAP، معلومات القرص، NVS). يجب أن
يُطابق المانح الطراز + الساوث بريدج. الـCoreOS يُستعاد فقط من نسخة لنفس الجهاز.

**Never / ممنوع:** taking identity from a donor. / أخذ الهوية من مانح.

---

## 8 - Browse Donor Library

**EN:** Browses the donor library (full NOR dumps, raw-heads, and component
blobs: EMC / EAP / Torus) and shows what is indexed.

**AR:** يستعرض مكتبة المانحين (دمبات نور كاملة، raw-head، و blobs المكوّنات:
EMC / EAP / Torus) ويعرض ما تمّت فهرسته.

---

## 9 - Rebuild Donor Database

**EN:** Rescans the donor folders and rebuilds the donor index and the empirical
blob-calibration database (which blob belongs to which console/firmware,
according to healthy dumps).

**AR:** يعيد مسح مجلدات المانحين ويبني فهرس المانحين وقاعدة المعايرة التجريبية
(أي blob ينتمي لأي جهاز/إصدار، بناءً على الدمبات السليمة).

---

## 11 - Hash Database Manager

**EN:** View / add / verify / clear known-good blob hashes used for validation.

**AR:** عرض / إضافة / تحقق / مسح هاشات الـblobs الموثوقة المستخدمة في التحقق.

---

## X - EAP Rescue All Models (3 outputs)

**EN:** LAST RESORT for a wiped EAP key on ANY model. Produces three variants to
try (blank-HDD variant, donor-HDD variant that needs a format, and a clean
variant with Torus fixed). On dual-key consoles the tool restores the key
LOCALLY from Key B and refuses donor injection.

**AR:** الملاذ الأخير لمفتاح EAP ممسوح على أي طراز. يُنتج ثلاث نسخ للتجربة
(نسخة بقرص فارغ، نسخة بقرص مانح تحتاج فورمات، ونسخة نظيفة مع إصلاح Torus).
على الأجهزة ذات المفتاحين تستعيد الأداة المفتاح محليًا من Key B وترفض حقن مانح.

**Warning / تحذير:** a donor EAP key cannot decrypt your original HDD. /
مفتاح EAP من مانح لا يمكنه فك تشفير قرصك الأصلي.

---

## Z - Fat Aeolia EAP Rescue (3 outputs)

**EN:** LAST RESORT for single-key Fat consoles (CUH-10xx/11xx) with a wiped
EAP key. Produces three variants (as above). The console will boot, but the
original HDD must be replaced or formatted.

**AR:** الملاذ الأخير لأجهزة Fat وحيدة المفتاح (CUH-10xx/11xx) بمفتاح EAP
ممسوح. يُنتج ثلاث نسخ (كما أعلاه). الجهاز سيقلع، لكن القرص الأصلي يجب استبداله
أو فورماته.

**EN:** The tool never fabricates a Key B on these models - that would
misreport a single-key board as dual-key.
**AR:** الأداة لا تصنع Key B على هذه الطرازات - لأن ذلك يُظهر لوحة وحيدة
المفتاح كأنها ذات مفتاحين.

---

## Repair coverage by console type
## تغطية الإصلاح حسب نوع الجهاز

| Console / الجهاز | NOR repair / إصلاح النور | NVS / CID | Notes / ملاحظات |
|---|---|---|---|
| Fat 10xx / 11xx | Good / جيد | Limited / محدود | BwE confirms weaker on 10xx-11xx |
| Fat 12xx | Good / جيد | Good / جيد | |
| Slim 20xx / 21xx | Good / جيد | Good / جيد | |
| Slim 22xx | Good / جيد | Good / جيد | |
| Pro 70xx / 71xx / 72xx | Good / جيد | Good / جيد | |

## Known unfixable / معروف أنه غير قابل للإصلاح

| EN | AR |
|---|---|
| IDPS error (identity is cryptographically bound) | خطأ IDPS (الهوية مرتبطة تشفيريًا) |
| SAMU Enter/Leave (APU hardware fault, not flash) | SAMU Enter/Leave (عطب عتادي في APU، ليس فلاش) |
| DCT / RAM training failure | فشل تدريب الذاكرة |

---

## Deferred experiment (documented, NOT enabled)
## تجربة مؤجلة (موثّقة، غير مفعّلة)

**EN:** IDATA / VTRM identity transplant. Concept approved; awaiting a fully
restorable console with complete NOR + Syscon dumps. See `AGENTS.md`.

**AR:** زراعة هوية IDATA / VTRM. الفكرة معتمدة؛ بانتظار جهاز قابل للاسترجاع
الكامل مع دمبات نور وسيسكون كاملة. انظر `AGENTS.md`.

---

Copyright (c) ISLAM JA. Unauthorized commercial use or redistribution is
prohibited. / جميع الحقوق محفوظة لـ ISLAM JA. يُمنع الاستخدام التجاري أو
إعادة التوزيع دون إذن.

---

## Contact / التواصل
- Email / البريد: **islamabuaker83@gmail.com**
- TikTok: https://tiktok.com/@ps4easytool
- GitHub: https://github.com/ISLAMGAZA/PS4-NOR-SYSCON-LIMITED

---

## UPDATE - 2026-09-12 (build 0098)

**New in this build:**
- **Y. Regenerate NVS (CID/UNK) - 3 outputs**: donor-based NVS regeneration in 3 variants
  (M1 accurate bytes / M2 blind last-half / M3 both) exactly in the spirit of the
  WeeTools PRO feature, with per-console protections (EAP keys, HDD, FW_VER, core_swch).
  The donor list is shown CLOSEST -> FARTHEST and YOU pick (Enter = closest).
- **Firmware trust rule**: an explicitly present FW_VER that lies inside the
  plausible band of the SouthBridge is TRUSTED (nothing overrides it); blank or
  out-of-band values trigger evidence-based determination + a confirm gate with
  the ranked donor list (option 4).
- **Unified selection** (X / Z / Y / 4): ranked closest-first donor lists based on
  Board ID + FW + SKU evidence - no silent decisions.
- **Licensing**: license.key is now RSA-2048 signed; it verifies on ANY machine
  with no secret present (public key embedded). Keys are HWID-locked.
- **Contact when the trial is exhausted**:
  WhatsApp +201097714567 | Email islamabuaker83@gmail.com | TikTok ps4easytool

## Donors - FULL packages (Google Drive)

All donor packs (NOR raw, nordonors, Syscon, EAP/EMC/Torus blobs) are hosted on
Google Drive:

**https://drive.google.com/drive/folders/1nw79XTzTtsucSJt-p0gZZDlYZowBEdHS?usp=drive_link**

Archive password: **`ISLAMJAMEL`**

Three small convenience packs are attached to this release too
(DONORS-minimal, DONORS-NOR-own-full, DONORS-Syscon-minimal).
