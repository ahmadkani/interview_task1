<B> تسک: طراحی یک سیستم مدیریت فایل با Web Worker و IndexedDB

- شرح کلی تسک
</B>

شما باید یک سیستم مدیریت فایل ساده طراحی کنید که دارای ویژگی‌های زیر باشد:

1. ساختار درختی (Tree View) برای نمایش فایل‌ها و فولدرها.
2. امکان اضافه کردن، حذف کردن، و تغییر نام فایل‌ها و فولدرها از طریق رابط کاربری.
3. ذخیره و مدیریت فایل‌ها و فولدرها در IndexedDB.
4. استفاده از Web Worker برای انجام عملیات مرتبط با IndexedDB (مانند ذخیره‌سازی، حذف، و بازیابی داده‌ها) و برقراری ارتباط بین رابط کاربری و دیتابیس.
<B>

- جزئیات تسک
</B>

بخش اول: رابط کاربری (UI)
شما باید یک رابط کاربری ساده طراحی کنید که به صورت ساختار درختی فایل‌ها و فولدرها را نمایش دهد.

- نمایش فایل‌ها و فولدرها:

فایل‌ها و فولدرها باید در قالب یک نمای درختی نمایش داده شوند.

- اضافه کردن فایل یا فولدر:

با کلیک روی یک فولدر، دکمه ای برای اضافه کردن فایل یا فولدر جدید نمایش داده شود.

- تغییر نام فایل یا فولدر:

کاربر باید بتواند با با زدن دکمه ویرایش نام فایل تغییر دهد.

- حذف فایل یا فولدر:

کاربر باید بتواند فایل‌ها یا فولدرها را حذف کند.

حذف یک فولدر باید تمام محتویات آن (فایل‌ها و فولدرهای داخلی) را نیز حذف کند.


بخش دوم: Web Worker

ورکر تنها مسئول انجام تمام عملیات مربوط به IndexedDB است و رابط کاربری تنها از طریق ارسال پیام‌ها با آن تعامل دارد.

ویژگی‌های مورد انتظار از Web Worker:
1. دریافت پیام‌ها از UI برای انجام عملیات زیر:

- افزودن فایل یا فولدر.
- حذف فایل یا فولدر.
- تغییر نام فایل یا فولدر.
- بازیابی داده‌ها برای بارگذاری مجدد درخت فایل هنگام شروع برنامه.
- 
2. ذخیره و بازیابی داده‌ها در IndexedDB:

- تمام داده‌ها (فایل‌ها و فولدرها) باید در IndexedDB ذخیره شوند.
- ورکر باید هنگام شروع برنامه داده‌ها را از IndexedDB خوانده و به UI ارسال کند.
- 
بخش سوم: IndexedDB

- فایل‌ها و فولدرها باید در IndexedDB ذخیره شوند. هر فایل یا فولدر باید دارای یک ساختار داده مشخص باشد:
```
{
  id: "unique_id",
  name: "file_or_folder",
  type: "file" | "folder",
  parentId: "parent_id",
}
```
- عملیات IndexedDB:
1. افزودن: افزودن یک فایل یا فولدر جدید به دیتابیس.
2. حذف: حذف یک فایل یا فولدر و تمام زیرمجموعه‌های آن.
3. بازیابی: بازگرداندن کل ساختار فایل‌ها و فولدرها به صورت سلسله‌مراتبی.
4. به‌روزرسانی: تغییر نام فایل یا فولدر.
5. ارتباط بین UI و Web Worker
ارتباط UI با Web Worker از طریق پیام‌ها انجام می‌شود:


پیام‌های UI به Web Worker:

افزودن فایل یا فولدر:

```
worker.postMessage({ action: "add", data: { name, type, parentId } });
```
حذف فایل یا فولدر:

```
worker.postMessage({ action: "delete", data: { id } });
```
تغییر نام فایل یا فولدر:
```
worker.postMessage({ action: "rename", data: { id, newName } });
```
بازیابی داده‌ها:
```
worker.postMessage({ action: "fetch" });
```
پیام‌های Web Worker به UI:
ورکر باید نتیجه عملیات را به UI ارسال کند.
مثال:

```
self.postMessage({ action: "updateTree", data: updatedTree });
```

شما باید کدهای زیر را ارسال کنید:

- کد Web Worker: مدیریت IndexedDB و ارتباط با UI.
- کد رابط کاربری: شامل HTML، CSS، و جاوااسکریپت.
- توضیح مختصری از معماری و نحوه عملکرد برنامه.
