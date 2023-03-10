# تمرین گروهی ۱.۱ - مستند طراحی

گروه
-----
 > نام و آدرس پست الکترونیکی اعضای گروه را در این قسمت بنویسید.

نام و نام خانوادگی <example@example.com>

نام و نام خانوادگی <example@example.com> 

نام و نام خانوادگی <example@example.com> 

نام و نام خانوادگی <example@example.com> 

مقدمات
----------
> اگر نکات اضافه‌ای در مورد تمرین یا برای دستیاران آموزشی دارید در این قسمت  بنویسید.

> لطفا در این قسمت تمامی منابعی (غیر از مستندات Pintos، اسلاید‌ها و دیگر منابع درس) را که برای تمرین از آن‌ها استفاده کرده‌اید در این قسمت بنویسید.

پاس‌دادن آرگومان
============
داده‌ساختار‌ها
----------------
> در این قسمت تعریف هر یک از `struct` ها، اعضای `struct` ها، متغیرهای سراسری یا ایستا، `typedef` ها یا `enum` هایی که ایجاد کرده‌اید یا تغییر داده‌اید را بنویسید و دلیل هر کدام را در حداکثر ۲۵ کلمه توضیح دهید.

نیاز داریم تا ورودی تایع `process_execute` را گرفته و با کاراکتر فاصله (`' '`) به‌اصطلاح توکنایز کنیم. همچنین یک حد بالا نیز برای طول رشته ورودی داریم. درنهایت برای Initialize کردن `argv` به یک حد بالا از تعداد آرگومان‌های ورودی نیز نیاز داریم.

```c
#define DELIMITER " "
```

```c
#define COMMAND_MAX_LEN 2048
```

```c
#define ARGS_MAX_COUNT 64
```

خروجی نیز دو متغیر به شکل زیر می‌باشد:

```c
int argc;
char *argv[ARGS_MAX_COUNT];
```

optional
-------

می‌توانیم آدرس هرکدام از اعضای `argv` را نیز در یک ‌آرایه جدا ذخیره کنیم. به‌هنگام استخراج آرگومان‌ها این آرایه نیز پر می‌شود.

```c
char* addrs;
```

الگوریتم‌ها
------------
> به‌طور خلاصه توضیح دهید چگونه آرگومان‌ها را پردازش کرده‌اید؟ چگونه اعضای `argv[]` را به ترتیب درست در پشته قرار داده‌اید؟ و چگونه از سرریز پشته جلوگیری کرده‌اید؟

با استفاده از `strtok_r` رشته ورودی را با `DELIMITER` جدا می‌کنیم و به ترتیب وارد `argv` می‌کنیم و در هرمرحله یکی به مقدار `argc` اضافه می‌کنیم.

بعد از اجرای تایع `setup_stack` عملیات پر کردن استک با اعضای `argv` انجام می‌شود.
ابتدا فضای موردنیاز را به استک می‌دهیم. آرگومان‌های موجود در `argv` را به‌صورت معکوس وارد استک می‌کنیم (و مقدار  `esp` نیز طبیعتا در هر مرحله کاهش می‌یابد.) اگر یوینتر به استک اصطلاحا `word aligned` نباشد، مقدار `NULL` را وارد استک می‌کنیم تا `align` شود. در نهایت بعد از پوش شدن همه توکن‌ها یک مقدار `NULL` وارد استک می‌کنیم که نشانگر اتمام توکن‌های `argv` می‌باشد. حال مقادیر `addrs` که آدرس آرگومان‌ها بود را به‌صورت معکوس  وارد استک می‌کنیم. در مرحله بعد آدرس `argv` و بعد از آن `argc` را وارد استک می‌کنیم. در نهایت آدرس بازگشت وارد استک می‌شود.

قابل ذکر است عملیات درج در استک با `memset` یا `memcpy` انجام می‌شود.

متغیرهای `COMMAND_MAX_LEN` و `ARGS_MAX_COUNT` برای جلوگیری از بزرگ‌شدن حجم استک قرار داده‌شده اند و طول ورودی و تعداد آرگومان‌ها چک می‌شوند.


منطق طراحی
-----------------
> چرا Pintos به‌جای تابع‌ `strtok()` تابع‌ `strtok_r()` را پیاده‌سازی کرده‌است؟

```c
char *strtok(char *str, const char *delim) {
    static char *save;
    return strtok_r(str, delim, &save);
}
```
با رجوع به کد مرجع `strtok` می‌توان فهمید در فراخوانی‌های مجدد، `save` مقداردهی اولیه نمی‌شود. همچنین `strtok_r` برخلاف `strtok` یک تابع `reentrant` است و قابلیت `Thread safety` را داراست. بدین معنی که `state` ورودی را در یک بافر غیر استاتیک (`saveptr`) ذخیره می‌کند که توسط ریسه‌های دیگر تغییر نمی‌کند.

<div dir="ltr">

The **saveptr** (**save**) argument is a pointer to a `char *` variable that is used internally by `strtok_r()` in order to maintain context between
       successive calls that parse the same string.

</div>


> در Pintos عمل جدا کردن نام فایل از آرگومان‌ها، در داخل کرنل انجام می‌شود. در سیستم عامل‌های برپایه‌ی Unix، این عمل توسط shell انجام می‌شود. حداقل دو مورد از برتری‌های رویکرد Unix را توضیح دهید.

<!-- <div dir="rtl"> -->

۱. دسترسی‌های فضای کرنل بیشتر از فضای کاربر (در اینجا `shell`) می‌باشد. در این حالت نگرانی‌های امنیتی نیز پایین است. (`security`)

۲. عملیات `parse` به یک شیوه واحد انجام نمی‌شود و هر کابر و `shell` می‌تواند `parser` خود را داشته باشد. (`flexibility`)

۳. تمرکز کرنل روی اجرای دستورات است و بقیه ملاحظات در سطح `shell` انچام می‌گیرد و پیچیدگی‌ای به کرنل اضافه نمی‌شود. (`efficiency`)

<!-- </div> -->

فراخوانی‌های سیستمی
================
داده‌ساختار‌ها
----------------
> در این قسمت تعریف هر یک از `struct` ها، اعضای `struct` ها، متغیرهای سراسری یا ایستا، `typedef` ها یا `enum` هایی که ای.جاد کرده‌اید یا تغییر داده‌اید را بنویسید و دلیل هر کدام را در حداکثر ۲۵ کلمه توضیح دهید.

- In `syscall.h` and `syscall.c`, the following functions will be added:
```c
int sys_practice (int i);
int sys_halt (void);
int sys_exit (int status);
pid_t sys_exec (const char *cmd line);
int sys_wait (pid_t pid);
```

- In `thread.h`, the following struct will be created:
```c
struct exit_status 
  {
    int exit_code;              /* Child’s exit code. */
    struct semaphore sema;      /* Initialized to 0. Decrement by parent on wait and increment by child on exit. */ 
    int ref_count;              /* Initialized to 2. Decrement if either child or parent exits. */
    struct lock lock;           /* Lock to protect EXIT_CODE and REF_COUNT. */
    struct list_elem elem;      /* Stored in parent thread. */
    tid_t tid;                  /* Child thread's id. */
  }
```

- In `struct thread`, the following attributes will be added:
```c
struct list child_es_list;      /* List of the exit statuses of child threads. */
struct exit_status es;          /* The thread’s own exit status. */
```

**userprog/syscall.c**
```c
/* Modification */
/* Handle system call functions and use validate_addr to check if the address is valid. */
static void syscall_handler (struct intr_frame *f UNUSED); 
/* Addition */
/* If the address is valid, return it. Otherwise exit(1). */
void *validate_addr (void *ptr); 
```

> توضیح دهید که توصیف‌کننده‌های فایل چگونه به فایل‌های باز مربوط می‌شوند. آیا این توصیف‌کننده‌ها در کل سیستم‌عامل به‌طور یکتا مشخص می‌شوند یا فقط برای هر پردازه یکتا هستند؟

الگوریتم‌ها
------------
> توضیح دهید خواندن و نوشتن داده‌های کاربر از داخل هسته، در کد شما چگونه انجام شده است.

- File descriptor management: To correctly allocate and deallocate the file descriptor, for each process, we use an int array of length 1024 to record available file descriptor numbers to allocate. We use int field `1` to indicate the corresponding index is already used as an `fd`, and `0` meaning not being used. We initialize the array to be all zero, except for `arr[0]` and `arr[1]` having  `1` because they are reserved for `STDIN_FILENO` and `STDOUT_FILENO`. Then to find a usable `fd` to allocate, we simply iterate through the array until we find an empty spot, i.e. `0`, and set it to `1`. When we finish using the `fd` number, we set `arr[fd]` back to `0`. 1024 should be far more enough than the file descriptors we will possibly need. The above process is part of implementation inside `add_file` and `remove_file`. 

- Before any file operation call through the `syscall_handler`, we will have to call `validate_addr()` (implemented in task2) to validate the input argument address. If not valid, we simply exit the user process.
- After validating the arguments, we will have to obtain the global lock `filesys_lock` so that no multiple ﬁlesystem functions are called concurrently. After that, we should retrieve the arguments from `argv`, call the corresponding file operation functions according to the input system call number. If any of the call fails due to reasons such as invalid file descriptor, we should exit the user process. 
- Right before we return to user, we should release the global lock `filesys_lock`. To avoid  being verbose, we don't repeat this inside each function.

> فرض کنید یک فراخوانی سیستمی باعث شود یک صفحه‌ی کامل (۴۰۹۶ بایت) از فضای کاربر در فضای هسته کپی شود. بیشترین و کمترین تعداد بررسی‌‌های جدول صفحات (page table) چقدر است؟ (تعداد دفعاتی که `pagedir_get_page()` صدا زده می‌شود.) در‌ یک فراخوانی سیستمی که فقط ۲ بایت کپی می‌شود چطور؟ آیا این عددها می‌توانند بهبود یابند؟ چقدر؟

> پیاده‌سازی فراخوانی سیستمی `wait` را توضیح دهید و بگویید چگونه با پایان یافتن پردازه در ارتباط است.

**wait**
In `process_wait`
- Iterate through `children` list in current thread and look for the thread which has the same tid as the one provided.
- If no thread can match the given tid, return -1
- If the thread exists, `sema_down` `dead` in child's `wait_status`.
- After `sema_down`, init a new local variable `exit_code` equal to `exit_code` in child's `wait_status`. 
- Acquire `lock` in child's `wait_status` and decrease `ref_cnt` in child's `wait_status` by 1 following by a `release_lock`. 
- If child's `ref_cnt` in `wait_status` equal to 0, free child's `wait_status` and remove `elem` which represents child's `wait_status` from list. 
- Return local variable `exit_code`.

> هر دستیابی هسته به حافظه‌ی برنامه‌ی کاربر، که آدرس آن را کاربر مشخص کرده است، ممکن است به دلیل مقدار نامعتبر اشاره‌گر منجر به شکست شود. در این صورت باید پردازه‌ی کاربر خاتمه داده شود. فراخوانی های سیستمی پر از چنین دستیابی‌هایی هستند. برای مثال فراخوانی سیستمی `write‍` نیاز دارد ابتدا شماره‌ی فراخوانی سیستمی را از پشته‌ی کاربر بخواند، سپس باید سه آرگومان ورودی و بعد از آن مقدار دلخواهی از حافظه کاربر را (که آرگومان ها به آن اشاره می کنند) بخواند. هر یک از این دسترسی ها به حافظه ممکن است با شکست مواجه شود. بدین ترتیب با یک مسئله‌ی طراحی و رسیدگی به خطا (error handling) مواجهیم. بهترین روشی که به ذهن شما می‌رسد تا از گم‌شدن مفهوم اصلی کد در بین شروط رسیدگی به خطا جلوگیری کند چیست؟ همچنین چگونه بعد از تشخیص خطا، از آزاد شدن تمامی منابع موقتی‌ای که تخصیص داده‌اید (قفل‌ها، بافر‌ها و...) مطمئن می‌شوید؟ در تعداد کمی پاراگراف، استراتژی خود را برای مدیریت این مسائل با ذکر مثال بیان کنید.

همگام‌سازی
---------------
> فراخوانی سیستمی `exec` نباید قبل از پایان بارگذاری فایل اجرایی برگردد، چون در صورتی که بارگذاری فایل اجرایی با خطا مواجه شود باید `-۱` برگرداند. کد شما چگونه از این موضوع اطمینان حاصل می‌کند؟ چگونه وضعیت موفقیت یا شکست در اجرا به ریسه‌ای که `exec` را فراخوانی کرده اطلاع داده می‌شود؟

**exec**
- Two semaphores `child_load_sema` and `parent_check_load_sema` are designed to synchronize `process_execute` (which runs in the parent thread) and `start_process` (which runs in the child thread). 
- - `child_load_sema` is to make sure that the logic check of child's load status in parent's `process_execute` happens after loading child thread in `start_process`.
- - `parent_check_load_sema` is to make sure that parent process can still get child's `load_success` even if child process has a higher priority than parent thread.
- Aqcuire lock and release lock before and after editing and accessing `ref_cnt` to make sure `ref_cnt` can only be edited by one thread at a time.

> پردازه‌ی والد P و پردازه‌ی فرزند C را درنظر بگیرید. هنگامی که P فراخوانی `wait(C)` را اجرا می‌کند و C  هنوز خارج نشده است، توضیح دهید که چگونه همگام‌سازی مناسب را برای جلوگیری از ایجاد شرایط مسابقه (race condition) پیاده‌سازی کرده‌اید. وقتی که C از قبل خارج شده باشد چطور؟ در هر حالت چگونه از آزاد شدن تمامی منابع اطمینان حاصل می‌کنید؟ اگر P بدون منتظر ماندن، قبل از C خارج شود چطور؟ اگر بدون منتظر ماندن بعد از C خارج شود چطور؟ آیا حالت‌های خاصی وجود دارد؟

منطق طراحی
-----------------
> به چه دلیل روش دسترسی به حافظه سطح کاربر از داخل هسته را این‌گونه پیاده‌سازی کرده‌اید؟

> طراحی شما برای توصیف‌کننده‌های فایل چه نقاط قوت و ضعفی دارد؟

> در حالت پیش‌فرض نگاشت `tid` به `pid` یک نگاشت همانی است. اگر این را تغییر داده‌اید، روی‌کرد شما چه نقاط قوتی دارد؟

سوالات افزون بر طراحی
===========
> تستی را که هنگام اجرای فراخوانی سیستمی از یک اشاره‌گر پشته‌ی(esp) نامعتبر استفاده کرده است بیابید. پاسخ شما باید دقیق بوده و نام تست و چگونگی کارکرد آن را شامل شود.

Test case: `sc-bad-sp.c`

Problem: In line 18, a negative virtual address which lies approximately 64MB below the code segment is assigned to esp. Because this address is not mapped to any valid memory, the test process must be terminated with -1 exit code.

<br>

> تستی را که هنگام اجرای فراخوانی سیستمی از یک اشاره‌گر پشته‌ی معتبر استفاده کرده ولی اشاره‌گر پشته آنقدر به مرز صفحه نزدیک است که برخی از آرگومان‌های فراخوانی سیستمی در جای نامعتبر مموری قرار گرفته اند مشخص کنید. پاسخ شما باید دقیق بوده و نام تست و چگونگی کارکرد آن را شامل شود.یک قسمت از خواسته‌های تمرین را که توسط مجموعه تست موجود تست نشده‌است، نام ببرید. سپس مشخص کنید تستی که این خواسته را پوشش بدهد چگونه باید باشد.

Test case: `sc-bad-arg.c`

Problem: In line 14, the top boundary of stack which is `0xbffffffc` is assigned to esp. When the system call number of `SYS_EXIT` is stored at this address, the argument to the `SYS_EXIT` would be above the top of the user address space. When `SYS_EXIT` is invoked, the test process should be terminated with -1 exit code.



سوالات نظرخواهی
==============
پاسخ به این سوالات اختیاری است، ولی پاسخ به آن‌ها می‌تواند به ما در بهبود درس در ترم‌های آینده کمک کند. هر چه در ذهن خود دارید بگویید. این سوالات برای دریافت افکار شما هستند. هم‌چنین می‌توانید پاسخ خود را به صورت ناشناس در انتهای ترم ارائه دهید.

> به نظر شما، این تمرین یا هر یک از سه بخش آن، آسان یا سخت بودند؟ آیا وقت خیلی کم یا وقت خیلی زیادی گرفتند؟

> آیا شما بخشی را در تمرین یافتید که دید عمیق‌تری نسبت به طراحی سیستم عامل به شما بدهد؟

> آیا مسئله یا راهنمایی خاصی وجود دارد که بخواهید برای حل مسائل تمرین به دانشجویان ترم‌های آینده بگویید؟

> آیا توصیه‌ای برای دستیاران آموزشی دارید که چگونه دانشجویان را در ترم‌های آینده یا در ادامه‌ی ترم بهتر یاری کنند؟

> اگر نظر یا بازخورد دیگری دارید در این قسمت بنویسید.
