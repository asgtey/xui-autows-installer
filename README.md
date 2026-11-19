# xui-autows-installer

نسخه اصلاحشدهٔ `install.sh` مربوط به [3x-ui](https://github.com/MHSanaei/3x-ui) (تستشده با `v2.9.4`):

- **همه سوالات بهصورت خودکار تایید میشوند** (حالت `NONINTERACTIVE` اجباری)، پس نصب با `bash <(curl ...)` متوقف نمیشود.
- **بعد از نصب** یک اینباند **VLESS + WebSocket** ساخته میشود (پورت، UUID و مسیر تصادفی).
- **خروجی**: لینک `vless://...` چاپ میشود و کانفیگ کامل در `/etc/x-ui/ws-config.txt` (مد 600) ذخیره میشود.

## نصب تکدستوره

روی سرور لینوکسی (با کاربر root):

```bash
bash <(curl -Ls https://raw.githubusercontent.com/asgtey/xui-autows-installer/main/install.sh) v2.9.4 && cat /etc/x-ui/ws-config.txt
```

همان دستور، به شکل فشردهتر:

```bash
curl -Ls https://raw.githubusercontent.com/asgtey/xui-autows-installer/main/install.sh | bash -s v2.9.4; cat /etc/x-ui/ws-config.txt
```

## تنظیمات اختیاری

قبل از دستور میتوانید متغیرها را تنظیم کنید:

| متغیر | پیشفرض | توضیح |
|---|---|---|
| `XUI_WS_PORT` | پورت آزاد تصادفی (10000–60000) | پورت اینباند WS |
| `XUI_WS_PATH` | `/<رشته تصادفی>` | مسیر WebSocket |
| `XUI_WS_UUID` | UUID تصادفی | UUID کلاینت |
| `XUI_WS_HOST` | تشخیص خودکار از `XUI_ACCESS_URL` | آدرسی که داخل لینک مینشیند |
| `XUI_WS_EMAIL` | `auto@ws` | ایمیل کلاینت |

مثال:

```bash
XUI_WS_PORT=8443 XUI_WS_HOST=example.com bash <(curl -Ls https://raw.githubusercontent.com/asgtey/xui-autows-installer/main/install.sh) v2.9.4 && cat /etc/x-ui/ws-config.txt
```

## نکات

- اینباند **WS ساده** است (بدون TLS). برای TLS از CDN / ریورس پرکسی جلوی پورت استفاده کنید.
- پورت اینباند را حتماً در فایروال سرور / پنل ارائهدهنده باز کنید.
- فایل کانفیگ فقط توسط root قابل خواندن است (`chmod 600`).

## منبع

اسکریپت اصلی از [MHSanaei/3x-ui](https://github.com/MHSanaei/3x-ui) گرفته شده و فقط سه پچ کوچک روی آن اعمال شده است (جستجوی `AUTOWS PATCH` در فایل).
