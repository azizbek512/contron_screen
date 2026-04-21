# Android Telefonni Linux dan ADB va scrcpy Orqali Boshqarish

## Kirish

Bu maqolada Android telefonni Linux kompyuterdan ADB (Android Debug Bridge) va scrcpy yordamida qanday boshqarish mumkinligi ko'rsatiladi. Amaliyot davomida yo'l qo'yilgan xatolar va ularning to'g'ri yechimlari ham keltirilgan.

---

## Kerakli dasturlar

```bash
sudo apt install adb scrcpy
```

---

## 1-bosqich: Telefonda Developer Mode yoqish

```
Sozlamalar → Telefon haqida → "Build number" ga 7 marta bosing
```

"You are now a developer!" xabari chiqadi.

---

## 2-bosqich: USB Debugging yoqish

```
Sozlamalar → Developer options → USB Debugging → ON
```

---

## 3-bosqich: Telefonni USB orqali ulash

Type-C yoki Micro-USB kabel bilan telefonni kompyuterga ulang.

Terminalda tekshiring:

```bash
adb devices
```

Telefon ekranida **"Allow USB Debugging?"** so'rovi chiqadi → **Allow** bosing.

To'g'ri natija:
```
List of devices attached
RF8R91A5L0X     device
```

Agar `unauthorized` chiqsa — telefonda Allow bosilmagan. Agar bo'sh bo'lsa — USB Debugging yoqilmagan.

---

## 4-bosqich: scrcpy ishga tushirish

```bash
scrcpy
```

Telefon ekrani kompyuterda ko'rinadi va sichqoncha/klaviatura bilan boshqarish mumkin bo'ladi.

---

## 5-bosqich: WiFi orqali ulash (USB siz ishlash)

USB ni olib tashlamasdan avval quyidagilarni bajaring:

```bash
# ADB ni WiFi rejimiga o'tkazish
adb tcpip 5555

# Telefon IP sini toping:
# Sozlamalar → WiFi → ulanган tarmoq → IP address

# WiFi orqali ulash
adb connect 192.168.1.109:5555
```

Endi USB ni olib tashlang va scrcpy ni qayta ishga tushiring:

```bash
scrcpy -e
```

`-e` parametri — WiFi qurilmasini tanlaydi.

---

## Yo'l qo'yilgan xatolar va yechimlari

### Xato 1: ADB qurilma topilmadi

```
ERROR: Could not find any ADB device
```

**Sabab:** Telefon ulanmagan yoki USB Debugging yoqilmagan.

**Yechim:** USB Debugging ni yoqing va Allow bosing.

---

### Xato 2: Bir nechta qurilma topildi

```
ERROR: Multiple (2) ADB devices:
    --> (usb)   RF8R91A5L0X   device  SM_A127F
    --> (tcpip) 192.168.1.109:5555  device  SM_A127F
```

**Sabab:** USB ham, WiFi ham bir vaqtda ulangan.

**Yechim:** USB ni olib tashlang yoki `-e` parametridan foydalaning:

```bash
scrcpy -e   # WiFi qurilmasini tanlaydi
scrcpy -d   # USB qurilmasini tanlaydi
```

---

## Xulosa

| Vazifa | Buyruq |
|--------|--------|
| Qurilmani tekshirish | `adb devices` |
| WiFi rejimga o'tish | `adb tcpip 5555` |
| WiFi orqali ulash | `adb connect IP:5555` |
| Ekranni ko'rsatish | `scrcpy` |
| WiFi qurilmasini tanlash | `scrcpy -e` |

scrcpy o'zi to'liq boshqaruvni ta'minlaydi — sichqoncha, klaviatura, scroll, klik — hech qanday qo'shimcha dastur shart emas.
