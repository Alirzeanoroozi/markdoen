# نصب متلب
ابتدا متلب از این 
[لینک](https://www.downloadha.com/software/download2-r2021b-matlab/)
دانلود شد.  
برای این تمرین از نسخه 2022a استفاده شده است.

# اجرای simple

## پیش‌نیاز

برای اجرا کردن مثال‌ها ابتدا باید فایل init_truetime.m اجرا شود.

![image](https://user-images.githubusercontent.com/36403983/160651286-434c15f6-7b31-44ae-9c40-1284e11380d7.png)

![image](https://user-images.githubusercontent.com/36403983/160651318-3ab419f6-2f21-4294-bb37-304a9d2fb3fe.png)

این فایل  فولدر‌های لازم را به اضافه می‌کند.


## اجرای شبیه‌سازی
ابتدا باید به فولدر simple بروید.  
با دستور زیر می‌توانید اینکار را انجام دهید:
```cd examples\simple\matlab\```

سپس فایل `simple.slx` را اجرا کنید.

![image](https://user-images.githubusercontent.com/36403983/160651753-76e40342-b99c-40d8-9048-674a232bfdee.png)

![image](https://user-images.githubusercontent.com/36403983/160651788-c829f7c7-20d1-47c5-94da-4252abbd14a2.png)

با زدن دکمه Run می‌توانید شبیه سازی را آغاز کنید.  
با کلیک کردن روی input, Schedule و Output می‌توانید این سه مورد را مشاهده کنید

![image](https://user-images.githubusercontent.com/36403983/160652623-de6b28fe-c00e-44de-b716-70c9171d81a9.png)


مقدار‌های دیفالت ورودی به صورت زیر است:

| Arg       | Value |
|-----------|-------|
| K         | 2     |
| exectime  | 0.5   |
| starttime | 0.0   |
| period    | 0.5   |

این مقدار‌ها را می‌توانید در فایل `simple_init.m` عوض کنید:

![image](https://user-images.githubusercontent.com/36403983/160652558-02758a8b-5bec-42ff-86ff-15713e0f3f66.png)

## تغییر ورودی‌ها

نتیجه هر جدول ورودی در زیر آن آمده است:


| Arg       | Value |
|-----------|-------|
| K         | 10     |
| exectime  | 0.5   |
| starttime | 0.0   |
| period    | 0.5   |

![image](https://user-images.githubusercontent.com/36403983/160652881-bf1afef4-4214-41d2-b5a6-75b122483269.png)


| Arg       | Value |
|-----------|-------|
| K         | 1     |
| exectime  | 0.5   |
| starttime | 0.0   |
| period    | 0.5   |

![image](https://user-images.githubusercontent.com/36403983/160652945-0c634085-cb92-45a4-a4d8-aeecb22a08e9.png)


| Arg       | Value |
|-----------|-------|
| K         | 2     |
| exectime  | 1.5   |
| starttime | 0.0   |
| period    | 0.5   |

![image](https://user-images.githubusercontent.com/36403983/160653005-b58ba6ba-0d91-41ab-9708-e310510afd2f.png)


| Arg       | Value |
|-----------|-------|
| K         | 2     |
| exectime  | 0.05   |
| starttime | 0.0   |
| period    | 0.5   |

![image](https://user-images.githubusercontent.com/36403983/160653049-68dcc95a-a791-4ce2-801f-d6c788e0e559.png)


| Arg       | Value |
|-----------|-------|
| K         | 2     |
| exectime  | 0.5   |
| starttime | 0.25   |
| period    | 0.5   |

![image](https://user-images.githubusercontent.com/36403983/160653074-b5ece98a-c143-4600-89c4-e492a85ec1f5.png)


| Arg       | Value |
|-----------|-------|
| K         | 2     |
| exectime  | 0.5   |
| starttime | 0.0   |
| period    | 1.5   |

![image](https://user-images.githubusercontent.com/36403983/160653093-5606f90a-f595-4d75-8abe-6cde37c898b7.png)


| Arg       | Value |
|-----------|-------|
| K         | 2     |
| exectime  | 0.5   |
| starttime | 0.0   |
| period    | 0.1   |

![image](https://user-images.githubusercontent.com/36403983/160653114-b6f66c2b-0d86-444c-b005-a4e5b3a7859c.png)


## فوتبال
[فیلم شبیه‌سازی soccer](https://drive.google.com/uc?export=view&id=1Ajfr1Iiif0uU2Tpx15pQLgq7_DFB5Rhs)
