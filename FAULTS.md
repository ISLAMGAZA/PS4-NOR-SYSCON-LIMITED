![Logo Main](logo_main.jpg)

# Faults Report - PS4 NOR Dumps (bad/ 9 dumps)

**English (Top)**
| Dump | Fault | Repair |
|------|-------|--------|
| 21.BIN (CUH-2216B 13.52) | EMC 78bcc7e6 suspicious | Fix blobs to 07f41f9a |
| 4.BIN (CUH-2216A 10.50) | Almost healthy 10.50 | Check only |
| aleezzzz.BIN (CUH-2216A 6.20) | EMC faff old | Update Southbridge |
| FAT1XXX.BIN (CUH-1001A 6.72) | EAP wiped, HDD blank - No Key B | Z - 3 outputs (Fat Aeolia) |
| FAT1XXX2.bin (CUH-1003A 6.72) | EAP wiped, HDD blank | Z - 3 outputs |
| MBRERROR.BIN (CUH-7016B 13.02) | MBR corrupt | Repair MBR |
| NOPOWER1.BIN (CUH-2016B 9.00) | EAP DANGER | Z or Full Auto |
| SLIM PR.BIN (CUH-2216A 9.00) | EMC 3f19 wrong | Patch Southbridge |
| W25Q256JV@WSON8.BIN (CUH-2216A 10.50) | EAP wiped | Z |

![Logo Secondary](logo_secondary.jpg)

---
**العربية (الأسفل)**
| الدمب | العطل | الإصلاح |
|------|-------|--------|
| 21.BIN | EMC مشبوه | تصحيح Blobs |
| 4.BIN | سليم تقريباً | فحص فقط |
| aleezzzz | EMC قديم | تحديث Southbridge |
| FAT1XXX | EAP تالف، HDD فارغ - لا يوجد Key B | Z - 3 ملفات |
| FAT1XXX2 | EAP تالف | Z - 3 ملفات |
| MBRERROR | MBR تالف | إصلاح MBR |
| NOPOWER1 | EAP خطر | Z أو Full Auto |
| SLIM PR | EMC خطأ | Patch Southbridge |
| W25Q256JV | EAP تالف | Z |

Generated: 2026-09-04 - Tool v2.0-beta

---

## UPDATE - 2026-09-12 (build 0098) - NEW diagnosed cases

| Case | Diagnosis | Handling |
|------|-----------|----------|
| FW: FF.FF (blank FW_VER) | Never-booted / wiped NVS1 state; FW must be DERIVED (CoreOS exact-hit, EAP/EMC tables) or picked by the user from the ranked list | Option 4 confirm gate / Y donor list |
| FW_VER explicit but OUT of band (e.g. 14.04) | Impossible stamp - the field is not trusted | Consensus + gate |
| EAP_KBL unknown MD5 but family-consistent (e.g. FC16C143 = 97.66% similar to 12.50) | Genuine region variant NOT in the tables - replacement with a different-gen blob would BRICK | KEEP, report family similarity |
| CoreOS slot shows a version with NO exact DB hit | Possible self-indexed circular evidence | Trust-rule ignores it when the explicit FW is in-band |
| Donor with blank FW_VER | Unsafe field donor - EXCLUDED from the library (moved to _EXCLUDED_blankFW) | Not offered |
| HDD/EAP-Key transfer by Regenerate NVS | WeeTools PRO does NOT transfer them either (verified) | Protected by design |

Full donor packages (all types) on Google Drive:
**https://drive.google.com/drive/folders/1nw79XTzTtsucSJt-p0gZZDlYZowBEdHS?usp=drive_link**
Archive password: **`ISLAMJAMEL`**
