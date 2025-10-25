# 🛡️ معرفی و بررسی سیستم‌عامل Qubes OS

> **پروژه آزمایشگاه سیستم‌عامل ۱**  
> موضوع: آشنایی با توزیع امنیت‌محور **Qubes OS**  
> ارائه‌دهنده: *محمدامین داودیان*  
> دانشگاه: *دانشگاه ملی مهارت استان مازندران*  
> ترم: ۱۴۰۴-۱۴۰۵  

---

## 🧩 معرفی کلی Qubes OS

**Qubes OS** یک سیستم‌عامل **امنیت‌محور** است که از فناوری **مجازی‌سازی (Xen Hypervisor)** برای جداسازی فعالیت‌های کاربر استفاده می‌کند.  
به‌جای اجرای تمام برنامه‌ها در یک محیط مشترک، Qubes OS هر فعالیت (مثلاً مرور وب، ایمیل، توسعه کد و...) را در یک **ماشین مجازی (VM)** جداگانه اجرا می‌کند.

این روش باعث می‌شود اگر یک برنامه آلوده شود، سایر بخش‌های سیستم در امان بمانند — به این مفهوم در Qubes گفته می‌شود:  
> 🧠 **Security by Compartmentalization** (امنیت از طریق جداسازی)

---

## 🧱 ساختار سیستم در Qubes OS

Qubes OS از چند نوع ماشین مجازی تشکیل شده است:

| نوع ماشین | توضیح | نمونه |
|------------|--------|--------|
| **Dom0** | کنترل‌کننده اصلی و لایه مدیریتی سیستم | رابط کاربری، مدیریت پنجره‌ها |
| **TemplateVM** | قالب پایه برای ساخت سایر VMها | قالب Fedora یا Debian |
| **AppVM** | برای اجرای برنامه‌ها (مرورگر، فایل‌ها، ترمینال و...) | work, personal, dev |
| **NetVM** | ماشین مجازی مخصوص مدیریت شبکه | sys-net |
| **FirewallVM** | کنترل و فیلتر ترافیک بین AppVMها و NetVM | sys-firewall |

---

## 🧠 ۱۰ دستور خاص و کاربردی در Qubes OS

> 💡 این دستورات مخصوص محیط Qubes OS هستند و در سایر توزیع‌های لینوکس وجود ندارند یا کاربرد متفاوتی دارند.

---

### 1️⃣ `qvm-ls`
نمایش فهرست تمام ماشین‌های مجازی (VM) فعال و غیرفعال در Qubes OS.

```bash
qvm-ls
```

**خروجی نمونه:**
```
NAME        STATE   CLASS        LABEL
dom0        Running Admin        black
work        Running AppVM        blue
personal    Halted  AppVM        green
sys-net     Running NetVM        red
```

---

### 2️⃣ `qvm-start <vm-name>`
برای **روشن‌کردن** یک ماشین مجازی خاص.

```bash
qvm-start work
```

💬 مثال: اجرای محیط کاری (VM با نام work).

---

### 3️⃣ `qvm-shutdown <vm-name>`
خاموش‌کردن امن یک VM خاص.

```bash
qvm-shutdown personal
```

⚠️ در صورتی که برنامه‌ای در حال اجرا باشد، تا زمان بسته‌شدن کامل VM صبر می‌کند.

---

### 4️⃣ `qvm-prefs <vm-name>`
نمایش یا تغییر تنظیمات هر VM.

```bash
qvm-prefs work
qvm-prefs work netvm sys-firewall
```

💡 مثال: اختصاص فایروال به ماشین «work».

---

### 5️⃣ `qvm-run <vm-name> <command>`
اجرای دستور خاص در داخل VM بدون ورود به آن.

```bash
qvm-run personal 'firefox https://qubes-os.org'
```

⚙️ مثال: اجرای مرورگر در VM شخصی برای وب‌گردی امن.

---

### 6️⃣ `qvm-copy-to-vm <target-vm>`
انتقال فایل از یک VM به VM دیگر به‌صورت امن.

```bash
qvm-copy-to-vm work myfile.txt
```

🛡️ فایل‌ها به‌صورت رمزگذاری‌شده بین دامنه‌ها منتقل می‌شوند.

---

### 7️⃣ `qvm-backup`
ایجاد نسخه پشتیبان از کل سیستم یا یک VM خاص.

```bash
qvm-backup /home/user/backups --yes work personal sys-net
```

📦 فایل پشتیبان به‌صورت رمزگذاری‌شده ذخیره می‌شود.

---

### 8️⃣ `qvm-firewall`
مدیریت قوانین فایروال برای هر ماشین مجازی.

```bash
qvm-firewall work list
qvm-firewall work add accept dsthost=example.com
```

🚧 مثال: اجازه‌ی اتصال VM کاری فقط به یک سایت خاص.

---

### 9️⃣ `qvm-device`
مدیریت اتصال دستگاه‌های سخت‌افزاری به VMها (مثل USB، میکروفون و وب‌کم).

```bash
qvm-device list
qvm-device attach work sys-usb:2-3
```

⚠️ به‌دلیل امنیت، دستگاه‌ها مستقیماً به Dom0 متصل نمی‌شوند.

---

### 🔟 `qvm-template`
مدیریت TemplateVMها (قالب‌های اصلی VM).

```bash
qvm-template list
qvm-template install fedora-39
```

🧩 هر AppVM از TemplateVM ارث‌بری می‌کند و تغییر در قالب روی همه اعمال می‌شود.

---

## ⚙️ ابزارهای خاص در Qubes OS

- **Qube Manager** 🧱  
  ابزار گرافیکی برای مدیریت VMها (Start/Stop/Settings)

- **Qubes Update Tool** 🔄  
  به‌روزرسانی همه‌ی TemplateVMها و AppVMها از یک رابط مرکزی

- **Qubes Firewall Manager** 🔒  
  پیکربندی بصری قوانین شبکه برای VMها

---

## 📊 جمع‌بندی

Qubes OS یکی از **امن‌ترین سیستم‌عامل‌های دنیا** است، چون فلسفه‌اش این است که:
> "اگر یک بخش آسیب ببیند، بقیه در امان بمانند."

برای توسعه‌دهندگان امنیت، پژوهشگران، و کاربران حساس به حریم خصوصی، Qubes OS یک انتخاب ایده‌آل است.

---

## 📚 منابع

- [Qubes OS Official Website](https://www.qubes-os.org)  
- [Qubes Documentation](https://www.qubes-os.org/doc/)  
- [Wikipedia: Qubes OS](https://en.wikipedia.org/wiki/Qubes_OS)  
- Joanna Rutkowska's Blog: [Invisible Things Lab](https://blog.invisiblethings.org)

---
> 💬 *تهیه و تدوین توسط: محمدامین داودیان*  
> *دانشگاه ملی مهارت استان مازندران*  
> *درس آزمایشگاه سیستم‌عامل ۱*

---
