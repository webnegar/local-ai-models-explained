# GGUF Flow: from a model file to an API and a UI

<img width="990" height="707" alt="image" src="https://github.com/user-attachments/assets/6d78e4ff-d12b-4614-b26a-d7c40dcb5429" />


**[English](#english) · [فارسی](#فارسی)**

---

## English

An interactive, animated motion graphic (a single HTML file) that shows how a **GGUF** model file is loaded by **llama.cpp**, **Ollama**, **LM Studio** and **vLLM**, and how each one exposes an **API** or a **user interface**.

Open `index.html` in a browser. No build step, no dependencies.

### Features

- Animated flow: dashed lines move from the `model.gguf` file, through each engine, to the API and UI outputs.
- Click an engine box to highlight its path and see its command, port and output in the panel below. Click again to return to the overview.
- An `i` button on every engine box opens extra info: description, special feature, whether it has a UI, and its website.
- Click `model.gguf`, the API box or the Interface box for details (sources, endpoints, frontends).
- Click a quantization type (Q2_K, Q4_K_M, Q5_K_M, Q6_K, Q8_0, F16) for what it means and a rough size for an 8B model.
- Click a model family (Qwen, gpt-oss, Gemma, Llama, Mistral, DeepSeek) for common sizes and variants.
- English (default) and Persian, switchable at runtime. The layout mirrors for right-to-left.
- Dark theme, keyboard accessible, responsive, pause/play button, respects `prefers-reduced-motion`.

### What it shows

| Tool | How the GGUF file is attached | API | Interface |
|---|---|---|---|
| llama.cpp | `llama-server -m ./model.gguf --port 8080` | OpenAI-compatible, `localhost:8080/v1` | Built-in web UI |
| Ollama | `FROM ./model.gguf` in a Modelfile, then `ollama create` | Native `/api` and OpenAI-compatible, `localhost:11434` | Desktop app, terminal chat |
| LM Studio | `lms import ./model.gguf` or the models folder | OpenAI-compatible, `localhost:1234/v1` | Full graphical chat |
| vLLM | `vllm serve ./model.Q4_K_M.gguf --tokenizer org/model-name` | OpenAI-compatible, `localhost:8000/v1` | None (use Open WebUI) |

Ports are the usual defaults and can differ on your setup. GGUF support in vLLM is experimental.

### Run it

```bash
git clone https://github.com/<your-user>/<your-repo>.git
cd <your-repo>
# open index.html in your browser
```

To publish it: GitHub repo → **Settings → Pages** → deploy from the `main` branch, root folder.

### Customize

Everything lives in one file, inside the `<script>` block:

- `L`: all texts for `en` and `fa` (titles, tooltips, panel content). Add a new language by adding a key here.
- `T`: the list of tools (name, accent color, command, port).
- `URLS`, `SURL`: website links shown in the info boxes.
- Colors are CSS variables at the top of the `<style>` block.

### Record it as a video

```python
# pip install playwright && playwright install chromium
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    b = p.chromium.launch()
    ctx = b.new_context(viewport={"width": 1280, "height": 900},
                        record_video_dir="out",
                        record_video_size={"width": 1280, "height": 900})
    page = ctx.new_page()
    page.goto("file:///PATH/TO/index.html")
    page.wait_for_timeout(5000)
    for i in range(4):                       # click each engine box
        page.click(f".ecard[data-i='{i}']")
        page.wait_for_timeout(6000)
    ctx.close(); b.close()
```

Convert to MP4: `ffmpeg -i out/*.webm -c:v libx264 -pix_fmt yuv420p video.mp4`. For the Persian version, first click `.seg button[data-l="fa"]`.

### Notes

- The page is self-contained. The only external request is the Vazirmatn font from Google Fonts, with system fallbacks.
- Quantization sizes are approximate (bits per weight × parameters). Model families and versions are examples, not a complete list.
- Verify the website links before publishing.

### Author

**Ehsan Mohsenpourian**
Email: [Mohsenpourian@gmail.com](mailto:Mohsenpourian@gmail.com)
YouTube: [@DeeplySimpleTech](https://www.youtube.com/@DeeplySimpleTech)

### License


---

## فارسی

<div dir="rtl" align="right">

یک موشن‌گرافیک تعاملی و متحرک (در قالب یک فایل HTML) که نشان می‌دهد فایل مدل **GGUF** چطور توسط **llama.cpp**، **Ollama**، **LM Studio** و **vLLM** بارگذاری می‌شود و هر کدام چطور یک **API** یا **رابط کاربری** در اختیار می‌گذارند.

فایل `index.html` را در مرورگر باز کنید. نه نیاز به build دارد و نه وابستگی‌ای.

### ویژگی‌ها

- جریان متحرک: خط‌های بریده‌بریده از فایل `model.gguf` عبور می‌کنند، به هر موتور می‌رسند و از آنجا به خروجی‌های API و رابط کاربری می‌روند.
- با کلیک روی کادر هر موتور، مسیرش پررنگ می‌شود و دستور، پورت و خروجی‌اش در پنل پایین دیده می‌شود. کلیک دوباره به نمای کلی برمی‌گردد.
- روی هر کادر موتور یک دکمه `i` هست که اطلاعات اضافه را باز می‌کند: توضیح، ویژگی خاص، داشتن یا نداشتن رابط کاربری و آدرس سایت.
- با کلیک روی `model.gguf`، کادر API یا کادر رابط کاربری، توضیح آن‌ها (منابع، اندپوینت‌ها، فرانت‌اندها) نمایش داده می‌شود.
- با کلیک روی هر نوع کوانتایز (Q2_K، Q4_K_M، Q5_K_M، Q6_K، Q8_0، F16) معنی آن و حجم تقریبی برای مدل ۸ میلیاردی دیده می‌شود.
- با کلیک روی هر خانواده مدل (Qwen، gpt-oss، Gemma، Llama، Mistral، DeepSeek) اندازه‌ها و نسخه‌های رایج نمایش داده می‌شود.
- دو زبان انگلیسی (پیش‌فرض) و فارسی با قابلیت جابه‌جایی در لحظه. چیدمان برای راست‌به‌چپ آینه می‌شود.
- تم تیره، قابل استفاده با صفحه‌کلید، واکنش‌گرا، دکمه توقف/پخش و رعایت `prefers-reduced-motion`.

### چه چیزی را نشان می‌دهد

| ابزار | نحوه معرفی فایل GGUF | API | رابط کاربری |
|---|---|---|---|
| llama.cpp | `llama-server -m ./model.gguf --port 8080` | سازگار با OpenAI، `localhost:8080/v1` | رابط وب داخلی |
| Ollama | نوشتن `FROM ./model.gguf` در Modelfile و سپس `ollama create` | API بومی `/api` و سازگار با OpenAI، `localhost:11434` | اپ دسکتاپ و چت ترمینال |
| LM Studio | `lms import ./model.gguf` یا گذاشتن فایل در پوشه مدل‌ها | سازگار با OpenAI، `localhost:1234/v1` | چت گرافیکی کامل |
| vLLM | `vllm serve ./model.Q4_K_M.gguf --tokenizer org/model-name` | سازگار با OpenAI، `localhost:8000/v1` | ندارد (با Open WebUI وصل شود) |

پورت‌ها مقدار پیش‌فرض رایج هستند و ممکن است در سیستم شما فرق کنند. پشتیبانی vLLM از GGUF هنوز تجربی است.

### اجرا

```bash
git clone https://github.com/<your-user>/<your-repo>.git
cd <your-repo>
# فایل index.html را در مرورگر باز کنید
```

برای انتشار: مخزن GitHub ← **Settings ← Pages** ← انتشار از شاخه `main` و پوشه ریشه.

### شخصی‌سازی

همه‌چیز در یک فایل و داخل بلوک `<script>` است:

- `L`: همه متن‌ها برای `en` و `fa` (عنوان‌ها، توضیح‌ها، محتوای پنل). برای زبان جدید یک کلید تازه اضافه کنید.
- `T`: فهرست ابزارها (نام، رنگ، دستور، پورت).
- `URLS` و `SURL`: لینک سایت‌هایی که در کادر اطلاعات دیده می‌شوند.
- رنگ‌ها متغیرهای CSS در ابتدای بلوک `<style>` هستند.

### تبدیل به ویدیو

```python
# pip install playwright && playwright install chromium
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    b = p.chromium.launch()
    ctx = b.new_context(viewport={"width": 1280, "height": 900},
                        record_video_dir="out",
                        record_video_size={"width": 1280, "height": 900})
    page = ctx.new_page()
    page.goto("file:///PATH/TO/index.html")
    page.wait_for_timeout(5000)
    for i in range(4):                       # کلیک روی کادر هر موتور
        page.click(f".ecard[data-i='{i}']")
        page.wait_for_timeout(6000)
    ctx.close(); b.close()
```

تبدیل به MP4: `ffmpeg -i out/*.webm -c:v libx264 -pix_fmt yuv420p video.mp4`. برای نسخه فارسی، اول روی `.seg button[data-l="fa"]` کلیک کنید.

### نکته‌ها

- صفحه مستقل است. تنها درخواست بیرونی، فونت Vazirmatn از Google Fonts است و فونت‌های سیستم جایگزین آن می‌شوند.
- حجم‌های کوانتایز تقریبی‌اند (تعداد بیت هر وزن × تعداد پارامترها). خانواده‌ها و نسخه‌های مدل نمونه‌اند، نه فهرست کامل.
- قبل از انتشار، لینک سایت‌ها را بررسی کنید.

### سازنده

**Ehsan Mohsenpourian**
ایمیل: [Mohsenpourian@gmail.com](mailto:Mohsenpourian@gmail.com)
یوتیوب: [@DeeplySimpleTech](https://www.youtube.com/@DeeplySimpleTech)

### مجوز


</div>
