# PeDitX repo
بسته های برنامه های کاربردی جامعه اصلاح OpenWrt را به راحتی نصب و ارتقا دهید (مانند: OpenClash، Passwall، ShadowSocksR+ Plus، Wegare STL، Tiny File Manager، Xderm Mini، v2rayA، Modeminfo و غیره).

مزایای نصب و به روز رسانی با استفاده از سرور سفارشی مانند این عبارتند از:
1. بدون نیاز به زحمت استفاده از wget و curl که بسیار طولانی و پیچیده هستند.
2. برای نصب بسته ipk می توان از «opkg install package-name» استفاده کرد.
3. با نصب بسته IPK می توان از ویژگی **System - software** در LuCI OpenWrt نیز استفاده کرد.

## فهرست مطالب
- [Architecture-list](#architecture-list)
- [نحوه افزودن مخزن به نرم افزار به روز رسانی OpenWrt] (#how-to-add-repository-to-software-update-openwrt)
- [نحوه نصب و به‌روزرسانی بسته‌ها] (#how-to-install-and-update-packages)
- [نحوه بررسی نصب یا عدم نصب بسته] (#how-to-check-package-installed-or-no)
- [اعتبار] (#اعتبار)

## فهرست معماری
این مخزن از معماری های زیر پشتیبانی می کند:

```
aarch64_cortex-a53
aarch64_cortex-a72
aarch64_generic
arm_arm1176jzf-s_vfp
arm_cortex-a7_neon-vfpv4
i386_pentium4
mips_24kc
mipsel_24kc
x86_64
```

## نحوه افزودن مخزن به آپدیت نرم افزار OpenWrt
نحوه اضافه کردن این مخزن به سیستم عامل، می توانید از 2 روش استفاده کنید، یعنی:
- [استفاده از LuCI] (#using-luci)
- [استفاده از ترمینال] (#using-terminal) مانند JuiceSSH/Termius/Termux

### با استفاده از LuCI

  1. آدرس روتر را وارد کنید (مثال: 192.168.10.1)، وارد شوید، به مسیر **System -> Software -> Configuration** بروید.
  
  2. یک علامت # (حصار) در جلوی خط «گزینه چک_امضا»، به عنوان مثال زیر اضافه کنید
  
      متن زیر را تغییر دهید
      
      ```
      option check_signature
      ```
      
     به این شکل تغییر کند
      
      ```
      # option check_signature
      ```

  3. در قسمت فیدهای سفارشی (custom feeds )، لیست زیر را اضافه کنید

      ```
      src/gz custom_generic https://raw.githubusercontent.com/PeDitX/PeDitX-repo/main/generic
      src/gz custom_arch https://raw.githubusercontent.com/PeDitX/PeDitX-repo/main/arm_cortex-a7_neon-vfpv4
      ```

**arm_cortex-a7_neon-vfpv4** را تغییر دهید و معماری CPU روتر OpenWrt خود را تنظیم کنید

      ![](https://raw.githubusercontent.com/lrdrdn/my-opkg-repo/main/preview/preview1.gif)
 
### استفاده از خط فرمان

  1. از یکی از برنامه ترمینال که در زیر آمده استفاده کنید
      - Terminal TTYD (Paket OpenWrt)
      - JuiceSSH
      - Termius
      - Termux

 > توجه: کاربران می توانند از برنامه های ترمینال غیر از موارد ذکر شده در بالا استفاده کنند
  
  2. با کپی پیست دستور زیر در ترمینال یه صورت خودکار معماری سی پی یو ی شما شناساسیی و جایگزین می شود
      
      ```
      sed -i 's/option check_signature/# option check_signature/g' /etc/opkg.conf
      echo "src/gz custom_generic https://raw.githubusercontent.com/PeDitX/PeDitX-repo/main/generic" >> /etc/opkg/customfeeds.conf
      echo "src/gz custom_arch https://raw.githubusercontent.com/PeDitX/PeDitX-repo/main/$(grep "OPENWRT_ARCH" /etc/os-release | awk -F '"' '{print $2}')" >> /etc/opkg/customfeeds.conf
      ```

> توجه: برای سیستم عامل OpenWrt 19.07 هنوز چیزهایی وجود دارد که باید به صورت دستی نصب شوند، مانند «kcptun-client»، «xray-core» و «libcap-bin».
    
      ![](https://raw.githubusercontent.com/lrdrdn/my-opkg-repo/main/preview/preview2.gif)
    

## نحوه نصب و به روز رسانی بسته ها
برای نصب این مخزن، می توانید از 2 روش استفاده کنید
- [LuCI](#install-dan-update-paket-menggunakan-luci)
- [Terminal](#install-dan-update-paket-menggunakan-terminal)  JuiceSSH/Termius/Termux

### نصب و به روز رسانی بسته ها با استفاده از LuCI

  1. آدرس روتر را وارد کنید (مثال: 192.168.10.1)، وارد شوید، به مسیر **System -> Software -> Configuration** بروید
  2. دکمه **Update Lists** بزنید.
  3. نام بسته مورد نظر خود را در فیلد **Filter** وارد نمایید.
  4. دکمه**Find Package** رابزنید.
  5. در زیر این دو نام را خواهید دید **Installed packages** و **Available packages** :
      - Installed packages : بسته های نصب شده
      - Available packages : بسته های موجود
  6. بر روی **Available packages** کلیک نمایید .
  7. وبعد در کنار بسه مورد نظر گزینه **Install** را کلیک کنید.
 
### نصب و آپدیت از طریق خط فرمان
  1. با نرم افزار مورد نظر خودتون وارد ترمینال روتر شوید
  2. برای بروز رسانی لیست نرم افزار ها دستور زیر را وارد نمایید
      ```
      opkg update
      ```
  
  3. برای نصب برنامه به این شکل عمل کنید `opkg install nama-paket`.
  
  > مثال: در دستور زیر نرم افزار پسوال نصب می شود
      
      ```
      opkg install luci-app-passwall
      ```

## نحوه بررسی نصب یا عدم نصب بسته
این مورد از دوبخش امکانپدیر است:
- [LuCI](#نحوه_بررسی_وضعیت_بسته_با_LuCI)
- [Terminal](#نحوه_بررسی-وضعیت-بسته-با-ترمینال) seperti JuiceSSH/Termius/Termux

### نحوه بررسی وضعیت بسته با LuCI
1. آدرس روتر را وارد کنید (به عنوان مثال: 192.168.1.1)، سپس Login.
      - اگر بسته‌ای را نصب کنید که حاوی کلمه «luci-app» باشد، معمولاً در **System/Services/NAS/VPN/Modem/Network** , موارد دیگر ظاهر می‌شود.
      - اگر بسته ای را نصب کنید که حاوی کلمه «luci-proto» باشد، معمولاً در مسیر **Network -> Pilih salah satu interface -> General Setup -> Protocol** ظاهر می شود.
      - اگر بسته ای را نصب کنید که حاوی کلمه «luci-theme» باشد، معمولاً در مسیر **System -> System Properties -> Language and Style -> Design** ظاهر می شود.
      - اگر بسته نصب شده حاوی کلمه luci نباشد، چیزی در LuCI نمایش نمی دهد.

### نحوه بررسی وضعیت بسته با ترمینال
  1. نرم افزار ترمینال خودرا باز کنید و به روتر متصل شوید
2. دستور «opkg list-installed package-name» را اجرا کنید، «package-name» را به نام بسته موجود تغییر دهید (در این مثال از بسته «luci-app-passwall» استفاده خواهیم کرد).
      ```
      opkg list-installed luci-app-passwall
      ```
      
اگر در ترمینال "luci-app-passwall - 4.43-2" ظاهر شود، بسته برنامه قبلاً نصب شده است، اگر در آنجا نباشد، بسته نصب نشده است. شماره «4.43-2» در ترمینال نسخه بسته برنامه نصب شده است.
      
      
### تشکر ویژه
- [Nugroho](https://radenku.com) sebagai pemilik repo, builder dan yang buat video contoh.
- [Helmi Amirudin](https://helmiau.com/about) sebagai tukang dokumentasi.
