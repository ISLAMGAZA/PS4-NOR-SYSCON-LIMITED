# PS4 NOR-SYSCON EASY TOOL - النسخة المحدودة v2.0-beta (البناء 0098)
**أداة احترافية لإصلاح NOR (sflash) + Syscon لأجهزة بلايستيشن 4 - النسخة المحدودة**
Professional PS4 NOR (sflash) + Syscon repair toolkit - Limited Edition.

**المالك / Owner:** ISLAM JA - github.com/ISLAMGAZA

> **التجربة: 5 محاولات تصليح** (وليس وقتًا). كل إصلاح ناجح يستهلك محاولة واحدة.
> A trial of **5 repair attempts** (not time-based). Each successful repair consumes one attempt.
> مفتاح `license.key` (مقفل بـHWID) يزيل الحد - A HWID-locked `license.key` removes the limit.

---

## تحديث 2026-09-12 (البناء 0098)
- **[Y] إعادة توليد NVS (CID/UNK) - 3 مخرجات**: ترميم NVS من متبرع (M1 بايتات دقيقة / M2 نسخ أعمى للنصف الأخير / M3 الاثنان) بحمايات صارمة (مفاتيح EAP والهارد وFW_VER وcore_swch). قائمة المتبرعين **الأقرب ← الأبعد** وأنت من تختار (Enter = الأقرب).
- **قاعدة الثقة بالفيرموير**: FW_VER موجود صراحةً وداخل الحيّز المعقول للشريحة يُعتمد فورًا؛ الفارغ أو خارج الحيّز يمر عبر الإجماع وبوابة تأكيد بقائمة مرتبة (الخيار 4).
- **انتقاء موحّد** (X / Z / Y / 4): قوائم مرتبة الأقرب-أولاً بدليل Board + FW + SKU - بلا قرارات صامتة.
- **الترخيص**: مفاتيح license.key بتوقيع RSA-2048 تتحقق على أي جهاز بلا أسرار (المفتاح العام مدمج).
- **التواصل عند انتهاء المحاولات**: واتساب +201097714567 | البريد islamabuaker83@gmail.com | تيك توك ps4easytool

## القائمة الرئيسية
```text
Diagnostics (read-only) / التشخيص
  [1] NOR Analyser       / تحليل NOR
  [2] Syscon Analyser    / تحليل Syscon
  [3] UART Log Diagnosis / تشخيص UART

Repair / التصليح
  [4] Full Auto-Repair (BLOD)
  [D] Downgrade (specialised - no other repairs) / داونجريد متخصص
  [R] Restore from donor (severe damage)          / استعادة من مانح
  [X] EAP Rescue All Models - 3 Outputs (Fat/Slim/Pro)
  [Z] Fat Aeolia EAP Rescue (3 outputs)
  [Y] Regenerate NVS (CID/UNK) - 3 outputs

Tools / الأدوات
  [8]  Browse Donor Library   / تصفح مكتبة الدونرز
  [9]  Rebuild Donor Database / إعادة بناء قاعدة الدونرز
  [11] Hash Database Manager  / مدير قاعدة الهاش
  [0]  Exit / خروج
```

## الضمان الأساسي
الأداة **لا تعدّل ملفك الأصلي أبدًا** - كل ناتج يُكتب في ملف جديد داخل `OUTPUT/`.
Never modifies your original dump - every result is a new file in `OUTPUT/`.

مفتاح EAP من متبرع لا يفك تشفير قرصك أبدًا - only a same-console backup can (Serial/MAC).
Donor EAP key can never decrypt your HDD - only a same-console backup can.

## الدونرز - الباقات الكاملة (جوجل درايف)
جميع باقات الدونرز (رؤوس NOR وnordonors وSyscon وبلوبات EAP/EMC/Torus - كل الأنواع) على جوجل درايف:

**https://drive.google.com/drive/folders/1nw79XTzTtsucSJt-p0gZZDlYZowBEdHS?usp=drive_link**

باسورد الأرشيف / Archive password: **`ISLAMJAMEL`**

## التشغيل
```
PS4_NOR_SYSCON_EASY_TOOL_Limited.exe
PS4_NOR_SYSCON_EASY_TOOL_Limited.exe cli
```

## الترخيص والتواصل
عند التشغيل يظهر: `HWID: ... | trial - N repair attempt(s) left`.
أرسل **HWID** عبر واتساب/البريد/تيك توك أعلاه لتحصل على `license.key`، ثم ضعه في `DONORS/license.key` وأعد التشغيل.

التواصل / Contact:
- واتساب: +201097714567
- البريد: islamabuaker83@gmail.com
- تيك توك: ps4easytool

**© ISLAM JA. الملكية خاصة - يُمنع إعادة التوزيع أو البيع دون إذن كتابي.**
Proprietary - redistribution or commercial resale without written permission is prohibited.