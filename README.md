# INVAI - منصة الذكاء الاصطناعي للاستثمار 🤖

![GitHub stars](https://img.shields.io/github/stars/open-webui/open-webui?style=social)
![GitHub forks](https://img.shields.io/github/forks/open-webui/open-webui?style=social)
![GitHub watchers](https://img.shields.io/github/watchers/open-webui/open-webui?style=social)
![GitHub repo size](https://img.shields.io/github/repo-size/open-webui/open-webui)
![GitHub language count](https://img.shields.io/github/languages/count/open-webui/open-webui)
![GitHub top language](https://img.shields.io/github/languages/top/open-webui/open-webui)
![GitHub last commit](https://img.shields.io/github/last-commit/open-webui/open-webui?color=red)
[![Discord](https://img.shields.io/badge/Discord-Open_WebUI-blue?logo=discord&logoColor=white)](https://discord.gg/5rJgQTnV4s)
[![](https://img.shields.io/static/v1?label=Sponsor&message=%E2%9D%A4&logo=GitHub&color=%23fe8e86)](https://github.com/sponsors/tjbck)

**INVAI هي منصة ذكاء اصطناعي متخصصة للاستثمار مبنية على Open WebUI** مع دعم كامل للغة العربية وميزات متقدمة لإدارة المعرفة والخصوصية. تدعم المنصة نماذج **Ollama** و **OpenAI-compatible APIs** مع محرك استنتاج مدمج لتقنية **RAG** (Retrieval-Augmented Generation).

**مخصصة حصرياً لمنصة Invast Oman** 🇴🇲

---

## 🎥 فيديو تعليمي سريع

> **شاهد هذا الفيديو لتعلم كيفية تشغيل منصة INVAI بسرعة وسهولة** 🚀

<div align="center">
  <a href="https://www.youtube.com/watch?v=eTNDgxoSqvk" target="_blank">
    <img src="https://img.youtube.com/vi/eTNDgxoSqvk/maxresdefault.jpg" alt="شرح تشغيل منصة INVAI" width="600" style="border-radius: 10px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
  </a>
</div>

<div align="center">
  <strong>👆 اضغط على الصورة لمشاهدة الفيديو على YouTube</strong>
</div>

---

## 🌟 الميزات الرئيسية

- 🤖 **مساعد ذكي للاستثمار**: مخصص حصرياً لمنصة Invast Oman مع نظام RAG متطور
- 🔒 **نظام خصوصية متقدم**: حماية كاملة للملفات والبيانات مع إعدادات خصوصية متقدمة
- 🌐 **دعم اللغة العربية**: واجهة وتفاعل كامل باللغة العربية مع دعم RTL
- 📚 **إدارة المعرفة الذكية**: نظام RAG متطور لاستخدام البيانات الداخلية فقط
- 🎨 **واجهة حديثة**: تصميم عصري ومتجاوب مع أفضل ممارسات UX
- 🐳 **سهولة النشر**: دعم Docker للنشر السريع والفعال
- 🛡️ **أمان متقدم**: نظام أذونات متدرج وحماية على مستوى API
- 📱 **تطبيق ويب تقدمي**: تجربة تطبيق أصلي على الأجهزة المحمولة

---

## 🚀 طرق التشغيل

### 📦 التشغيل باستخدام Docker (الطريقة المُوصى بها)

#### 1. سحب الصورة المخصصة
```bash
docker pull faisalanqoudi/open-webui-custom:0.6.18
```

#### 2. تشغيل الحاوية
```bash
docker run -d -p 3000:8080 \
  --name invai-platform \
  -v open-webui:/app/backend/data \
  --restart always \
  faisalanqoudi/open-webui-custom:0.6.18
```

#### 3. الوصول للمنصة
افتح المتصفح وانتقل إلى: **http://localhost:3000**

---

### 💻 التشغيل المحلي (للتطوير)

#### المتطلبات الأساسية
- **Node.js** 18+ 
- **Python** 3.11+
- **npm** أو **yarn**
- **Git**

#### خطوات التشغيل

1. **استنساخ المشروع**
```bash
git clone https://github.com/alanqoudif/invai-new.git
cd invai-new
```

2. **تثبيت التبعيات الأمامية**
```bash
npm install
```

3. **إعداد البيئة الخلفية**
```bash
cd backend
python -m venv venv

# على macOS/Linux
source venv/bin/activate

# على Windows
venv\Scripts\activate

pip install -r requirements.txt
```

4. **تشغيل الخادم**
```bash
# العودة للمجلد الرئيسي
cd ..

# تشغيل الخادم
npm run dev
```

5. **الوصول للمنصة**
افتح المتصفح وانتقل إلى: **http://localhost:5173**

---

## 🦙 إعداد Ollama ونموذج Qwen 3

### 1. تثبيت Ollama

#### على macOS
```bash
# باستخدام Homebrew
brew install ollama

# أو تحميل المثبت من الموقع الرسمي
# https://ollama.com/download
```

#### على Linux
```bash
# التثبيت التلقائي
curl -fsSL https://ollama.com/install.sh | sh

# أو باستخدام package manager
# Ubuntu/Debian
sudo apt update && sudo apt install ollama

# CentOS/RHEL
sudo yum install ollama
```

#### على Windows
1. حمّل المثبت من: **https://ollama.com/download**
2. شغّل الملف المحمل واتبع التعليمات
3. أعد تشغيل الكمبيوتر بعد التثبيت

### 2. تشغيل خدمة Ollama
```bash
# تشغيل الخدمة
ollama serve

# للتحقق من حالة الخدمة
curl http://localhost:11434/api/tags
```

### 3. تحميل نموذج Qwen 3 (8B Parameters)

```bash
# تحميل النموذج الأساسي (8 مليار معامل)
ollama pull qwen2.5:8b

# أو النموذج المحسن للدردشة (مُوصى به)
ollama pull qwen2.5:8b-instruct

# للنماذج الأكبر (إذا كان لديك ذاكرة كافية)
ollama pull qwen2.5:14b-instruct  # 14 مليار معامل
ollama pull qwen2.5:32b-instruct  # 32 مليار معامل
```

### 4. التحقق من تثبيت النماذج
```bash
# عرض جميع النماذج المثبتة
ollama list

# عرض معلومات نموذج محدد
ollama show qwen2.5:8b-instruct
```

### 5. اختبار النموذج
```bash
# اختبار سريع
ollama run qwen2.5:8b-instruct "مرحباً، كيف يمكنني مساعدتك في الاستثمار؟"

# دخول وضع الدردشة التفاعلية
ollama run qwen2.5:8b-instruct
```

---

## 🔗 ربط Ollama بمنصة INVAI

### 1. التأكد من تشغيل Ollama
```bash
# التحقق من حالة الخدمة
curl http://localhost:11434/api/tags

# إذا لم تعمل، تأكد من تشغيل الخدمة
ollama serve
```

### 2. إعداد الاتصال في INVAI

1. **افتح منصة INVAI** في المتصفح
2. **سجّل دخولك** أو أنشئ حساباً جديداً
3. انتقل إلى **⚙️ الإعدادات** (Settings)
4. اختر **🤖 النماذج** (Models) من القائمة الجانبية
5. في قسم **Ollama API**:
   - **عنوان الخادم**: `http://localhost:11434`
   - **مهلة الاتصال**: `120` ثانية
6. انقر على **💾 حفظ الإعدادات**

### 3. اختيار النموذج للدردشة

1. في **واجهة الدردشة الرئيسية**
2. انقر على **قائمة النماذج** في الأعلى
3. اختر `qwen2.5:8b-instruct` من القائمة
4. تأكد من ظهور النموذج كـ **متصل** ✅
5. ابدأ المحادثة!

---

## ⚙️ إعداد System Prompt للمساعد الذكي

### 1. الوصول لإعدادات النموذج

1. انتقل إلى **🏢 Workspace** → **🤖 Models**
2. ابحث عن النموذج `qwen2.5:8b-instruct`
3. انقر على **✏️ تحرير** (Edit) بجانب النموذج

### 2. إضافة System Prompt المخصص

انسخ والصق النص التالي في حقل **System Prompt**:

```
You are invai — the intelligent investment assistant exclusively for the Invast Oman platform. 
You operate with Retrieval-Augmented Generation (RAG) and may ONLY use Invast Oman's internal data. 

For every query: 
1) Detect the user's language and answer in that language. 
2) Use ONLY Invast Oman data. Do not guess or use external sources. 
3) Provide a highly detailed answer with deep explanations and inline internal citations (e.g., "[Logistics Sector, p. 5]"). 
4) If data is missing, reply exactly: "No sufficient information in Invast Oman data for this request." 
5) Always end with a short **Summary** section. 

Tables / conceptual views (auto-offer + triggers): 
6) Heuristic: If your drafted answer contains 3 or more distinct numbers, ranges, dates, or percentages, then — AFTER the detailed answer and before the Summary — append an offer sentence in the user's language: 
   - Arabic (use exactly this text): 
     **"إذا تحبّ، أجمع لك المثال أعلاه في كود صفحة HTML/CSS بسيط لعرض التعريفات والمراحل والجداول تفاعليًا، أو أدرج رسمًا بيانيًا يوضّح التدفق. بس علمني وش تفضّل!"** 
   - English: 
     **"If you'd like, I can turn this into a simple HTML/CSS page with interactive definitions, steps, and tables, or include a flow diagram—just tell me which you prefer!"** 

7) If the user replies with any of the following triggers, act immediately without further confirmation: 
   • Table triggers (Arabic): "جدول", "اعمل جدول", "سجّلها بجدول" 
   • Table triggers (English): "table" 
     → Insert a clean, well-formatted table inline (include headers, units, and sources). 

   • Conceptual/page triggers (Arabic): "نظري", "عرض نظري", "صفحة", "صفحة HTML", "اعمل صفحة" 
   • Conceptual/page triggers (English): "conceptual", "concept page", "HTML page" 
     → Output a minimal, standalone, RTL-aware HTML/CSS snippet that: 
        - has a title header, 
        - a "Definitions" section for key terms, 
        - a numbered list of stages/steps, 
        - a data table block, 
        - an optional simple flow/diagram placeholder, 
        - is responsive (no external libraries unless explicitly requested). 

8) If the user simply responds "نعم/إيه/Yes" to the offer, ask once: "جدول أو عرض نظري (HTML/CSS)؟ / Table or conceptual HTML page?" 

Style: 
9) Be professional, clear, and precise. Thorough but not verbose. Keep citations and units consistent. 
END_OF_SYSTEM
```

### 3. إعدادات إضافية للنموذج

- **Temperature**: `0.7` (للتوازن بين الإبداع والدقة)
- **Top P**: `0.9`
- **Max Tokens**: `4096`
- **Context Length**: `8192`

### 4. حفظ الإعدادات
انقر على **💾 حفظ** (Save) لتطبيق جميع التغييرات.

---

## 📚 إضافة المعرفة (Knowledge) للمساعد الذكي

### 1. إنشاء مجموعة معرفة جديدة

1. انتقل إلى **🏢 Workspace** → **📚 Knowledge**
2. انقر على **➕ إنشاء مجموعة** (Create Collection)
3. املأ البيانات التالية:
   - **اسم المجموعة**: `Invast Oman Investment Data`
   - **الوصف**: `البيانات الاستثمارية الداخلية لمنصة Invast Oman`
   - **اللغة**: `Arabic & English`
4. انقر على **إنشاء**

### 2. رفع الملفات والوثائق

#### أنواع الملفات المدعومة:
- **PDF**: التقارير والدراسات
- **DOCX/DOC**: الوثائق النصية
- **TXT**: الملفات النصية البسيطة
- **CSV**: البيانات المجدولة
- **XLSX**: جداول البيانات
- **MD**: ملفات Markdown

#### خطوات الرفع:
1. انقر على **📁 رفع ملفات** (Upload Files)
2. اختر الملفات من جهازك أو اسحبها للمنطقة المخصصة
3. انتظر حتى اكتمال **المعالجة والفهرسة**
4. تأكد من ظهور ✅ بجانب كل ملف

### 3. ربط المعرفة بالنموذج

1. ارجع إلى **🤖 Models** → اختر `qwen2.5:8b-instruct`
2. في قسم **📚 Knowledge Base**:
   - اختر `Invast Oman Investment Data`
   - فعّل **RAG Mode**
   - اضبط **Similarity Threshold**: `0.7`
3. **احفظ التغييرات**

### 4. اختبار نظام المعرفة

```
# أمثلة على الأسئلة للاختبار:

"ما هي أحدث التوقعات للقطاع اللوجستي في عُمان؟"

"أعطني تحليلاً مفصلاً عن فرص الاستثمار في قطاع التكنولوجيا"

"ما هي المخاطر المرتبطة بالاستثمار في الأسواق الناشئة؟"
```

---

## 🔧 إعدادات متقدمة

### تحسين أداء Ollama

```bash
# زيادة حجم السياق للنموذج
ollama run qwen2.5:8b-instruct
# داخل الدردشة:
/set parameter num_ctx 8192
/set parameter temperature 0.7
/set parameter top_p 0.9
```

### إعدادات Docker المتقدمة

#### مع دعم GPU
```bash
docker run -d -p 3000:8080 \
  --gpus all \
  --name invai-platform \
  -v open-webui:/app/backend/data \
  -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
  -e WEBUI_SECRET_KEY="your-secret-key-here" \
  --restart always \
  faisalanqoudi/open-webui-custom:0.6.18
```

#### مع متغيرات البيئة المخصصة
```bash
docker run -d -p 3000:8080 \
  --name invai-platform \
  -v open-webui:/app/backend/data \
  -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
  -e DEFAULT_LOCALE=ar \
  -e ENABLE_RAG_WEB_SEARCH=true \
  -e RAG_WEB_SEARCH_ENGINE=duckduckgo \
  --restart always \
  faisalanqoudi/open-webui-custom:0.6.18
```

### استخدام Docker Compose

أنشئ ملف `docker-compose.yml`:

```yaml
version: '3.8'

services:
  invai:
    image: faisalanqoudi/open-webui-custom:0.6.18
    container_name: invai-platform
    ports:
      - "3000:8080"
    volumes:
      - open-webui:/app/backend/data
    environment:
      - OLLAMA_BASE_URL=http://host.docker.internal:11434
      - DEFAULT_LOCALE=ar
      - ENABLE_RAG_WEB_SEARCH=true
    restart: always
    depends_on:
      - ollama

  ollama:
    image: ollama/ollama:latest
    container_name: ollama-server
    ports:
      - "11434:11434"
    volumes:
      - ollama:/root/.ollama
    restart: always

volumes:
  open-webui:
  ollama:
```

ثم شغّل:
```bash
docker-compose up -d
```

---

## 🌐 المنافذ والشبكات

### المنافذ المستخدمة
- **3000**: منصة INVAI (عبر Docker)
- **5173**: منصة INVAI (التشغيل المحلي)
- **8080**: المنفذ الداخلي للحاوية
- **11434**: خدمة Ollama API

### إعدادات الشبكة
```bash
# للوصول من الشبكة المحلية
docker run -d -p 0.0.0.0:3000:8080 \
  --name invai-platform \
  -v open-webui:/app/backend/data \
  faisalanqoudi/open-webui-custom:0.6.18

# مع تقييد الوصول لـ localhost فقط
docker run -d -p 127.0.0.1:3000:8080 \
  --name invai-platform \
  -v open-webui:/app/backend/data \
  faisalanqoudi/open-webui-custom:0.6.18
```

---

## 📋 متطلبات النظام

### الحد الأدنى
- **المعالج**: 4 أنوية (Intel i5 أو AMD Ryzen 5)
- **الذاكرة**: 8 جيجابايت RAM
- **التخزين**: 20 جيجابايت مساحة فارغة
- **نظام التشغيل**: Linux, macOS 10.15+, Windows 10+
- **الشبكة**: اتصال إنترنت لتحميل النماذج

### المُوصى به
- **المعالج**: 8 أنوية أو أكثر (Intel i7/i9 أو AMD Ryzen 7/9)
- **الذاكرة**: 16 جيجابايت RAM أو أكثر
- **كرت الرسوميات**: GPU مع 8+ جيجابايت VRAM (NVIDIA RTX 3070 أو أفضل)
- **التخزين**: SSD مع 50+ جيجابايت مساحة فارغة
- **الشبكة**: اتصال سريع (100+ Mbps)

### متطلبات النماذج
- **Qwen 2.5 8B**: 6-8 جيجابايت RAM
- **Qwen 2.5 14B**: 10-12 جيجابايت RAM
- **Qwen 2.5 32B**: 24-32 جيجابايت RAM

---

## 🔍 استكشاف الأخطاء وحلها

### مشاكل الاتصال بـ Ollama

#### المشكلة: "Connection refused" أو "Server not found"
```bash
# 1. تحقق من حالة Ollama
ollama list

# 2. إعادة تشغيل الخدمة
ollama serve

# 3. تحقق من المنفذ
netstat -an | grep 11434

# 4. اختبار الاتصال
curl http://localhost:11434/api/tags
```

#### المشكلة: Ollama يعمل لكن INVAI لا يتصل
```bash
# تأكد من إعداد متغير البيئة
export OLLAMA_HOST=0.0.0.0:11434
ollama serve

# أو استخدم Docker مع host networking
docker run -d --network=host \
  -v open-webui:/app/backend/data \
  --name invai-platform \
  faisalanqoudi/open-webui-custom:0.6.18
```

### مشاكل تحميل النماذج

#### المشكلة: فشل في تحميل النموذج
```bash
# 1. حذف النموذج المعطوب
ollama rm qwen2.5:8b-instruct

# 2. تنظيف الذاكرة المؤقتة
ollama prune

# 3. إعادة تحميل النموذج
ollama pull qwen2.5:8b-instruct

# 4. التحقق من المساحة المتاحة
df -h
```

#### المشكلة: النموذج بطيء جداً
```bash
# تحسين إعدادات النموذج
ollama run qwen2.5:8b-instruct
/set parameter num_thread 8
/set parameter num_gpu 1
/set parameter num_ctx 4096
```

### مشاكل Docker

#### المشكلة: الحاوية لا تبدأ
```bash
# 1. عرض سجلات الحاوية
docker logs invai-platform

# 2. التحقق من حالة الحاوية
docker ps -a

# 3. إعادة تشغيل الحاوية
docker restart invai-platform

# 4. حذف وإعادة إنشاء الحاوية
docker rm -f invai-platform
docker run -d -p 3000:8080 \
  --name invai-platform \
  -v open-webui:/app/backend/data \
  faisalanqoudi/open-webui-custom:0.6.18
```

#### المشكلة: فقدان البيانات بعد إعادة التشغيل
```bash
# تأكد من ربط المجلد بشكل صحيح
docker run -d -p 3000:8080 \
  --name invai-platform \
  -v $(pwd)/data:/app/backend/data \
  faisalanqoudi/open-webui-custom:0.6.18

# أو استخدم named volume
docker volume create invai-data
docker run -d -p 3000:8080 \
  --name invai-platform \
  -v invai-data:/app/backend/data \
  faisalanqoudi/open-webui-custom:0.6.18
```

### مشاكل الأداء

#### المشكلة: استجابة بطيئة
```bash
# 1. زيادة موارد Docker
docker update --memory=8g --cpus=4 invai-platform

# 2. تحسين إعدادات Ollama
export OLLAMA_NUM_PARALLEL=2
export OLLAMA_MAX_LOADED_MODELS=2
ollama serve

# 3. استخدام GPU إذا كان متاحاً
docker run -d -p 3000:8080 \
  --gpus all \
  --name invai-platform \
  -v open-webui:/app/backend/data \
  faisalanqoudi/open-webui-custom:0.6.18
```

### مشاكل نظام RAG

#### المشكلة: لا يجد المعلومات في الملفات المرفوعة
1. **تحقق من معالجة الملفات**:
   - انتقل إلى Knowledge → تأكد من ✅ بجانب الملفات
   - أعد رفع الملفات إذا لزم الأمر

2. **اضبط إعدادات البحث**:
   - قلل Similarity Threshold إلى `0.5`
   - زد عدد النتائج المسترجعة

3. **تحسين جودة الملفات**:
   - استخدم ملفات PDF نصية (ليس صور)
   - تأكد من وضوح النصوص
   - قسّم الملفات الكبيرة

---

## 🔐 الأمان والخصوصية

### ميزات الأمان
- **🔒 تشفير البيانات**: جميع البيانات مشفرة أثناء النقل والتخزين
- **🚫 عدم مشاركة البيانات**: لا يتم إرسال أي بيانات لخوادم خارجية
- **👥 التحكم في الوصول**: نظام أذونات متقدم للمستخدمين والمجموعات
- **📊 سجلات الأمان**: تتبع كامل لجميع العمليات والوصول
- **🛡️ حماية API**: حماية على مستوى API مع rate limiting

### إعدادات الخصوصية
```bash
# تشغيل في وضع offline كامل
docker run -d -p 3000:8080 \
  --name invai-platform \
  -v open-webui:/app/backend/data \
  -e HF_HUB_OFFLINE=1 \
  -e ENABLE_COMMUNITY_SHARING=false \
  --network none \
  faisalanqoudi/open-webui-custom:0.6.18
```

### نصائح الأمان
1. **استخدم كلمات مرور قوية** للحسابات
2. **فعّل المصادقة الثنائية** إذا كانت متاحة
3. **حدّث النظام بانتظام** للحصول على آخر التحديثات الأمنية
4. **راقب سجلات الوصول** بانتظام
5. **استخدم HTTPS** في البيئات الإنتاجية

---

## 🤝 المساهمة في المشروع

### إعداد بيئة التطوير

```bash
# 1. استنساخ المشروع
git clone https://github.com/alanqoudif/invai-new.git
cd invai-new

# 2. إنشاء فرع جديد
git checkout -b feature/new-feature

# 3. تثبيت التبعيات
npm install
cd backend && pip install -r requirements.txt

# 4. تشغيل الاختبارات
npm test
pytest backend/tests/

# 5. تشغيل الخادم للتطوير
npm run dev
```

### إرشادات المساهمة

1. **اتبع معايير الكود**: استخدم Prettier و ESLint
2. **اكتب اختبارات**: لكل ميزة جديدة
3. **وثّق التغييرات**: في ملف CHANGELOG.md
4. **استخدم commit messages واضحة**:
   ```
   feat: إضافة ميزة البحث المتقدم
   fix: إصلاح مشكلة في تحميل الملفات
   docs: تحديث دليل التثبيت
   ```

### إرسال التحديثات

```bash
# 1. إضافة التغييرات
git add .
git commit -m "feat: وصف التحديث بوضوح"

# 2. رفع التحديثات
git push origin feature/new-feature

# 3. إنشاء Pull Request
# انتقل إلى GitHub وأنشئ PR
```

## 📞 الدعم والمساعدة

### قنوات الدعم
- **📖 الوثائق الرسمية**: [docs.openwebui.com](https://docs.openwebui.com/)
- **💬 مجتمع Discord**: [Open WebUI Discord](https://discord.gg/5rJgQTnV4s)
- **🐛 الإبلاغ عن المشاكل**: [GitHub Issues](https://github.com/alanqoudif/invai-new/issues)
- **📧 البريد الإلكتروني**: support@invai.com
- **📱 التليجرام**: @invai_support

### الأسئلة الشائعة (FAQ)

**س: هل يمكن استخدام INVAI بدون إنترنت؟**
ج: نعم، بعد تحميل النماذج، يمكن تشغيل المنصة بالكامل offline.

**س: ما هو الحد الأقصى لحجم الملفات المرفوعة؟**
ج: الحد الافتراضي هو 100 ميجابايت، يمكن زيادته في الإعدادات.

**س: هل تدعم المنصة اللغات الأخرى غير العربية؟**
ج: نعم، تدعم المنصة أكثر من 30 لغة مع تركيز خاص على العربية.

**س: كيف يمكنني نسخ احتياطي من البيانات؟**
ج: استخدم `docker cp` لنسخ مجلد البيانات أو استخدم volume backup.

---

## 🚀 التحديثات والصيانة

### تحديث المنصة

```bash
# 1. إيقاف الحاوية الحالية
docker stop invai-platform

# 2. سحب أحدث إصدار
docker pull faisalanqoudi/open-webui-custom:latest

# 3. حذف الحاوية القديمة
docker rm invai-platform

# 4. تشغيل الإصدار الجديد
docker run -d -p 3000:8080 \
  --name invai-platform \
  -v open-webui:/app/backend/data \
  --restart always \
  faisalanqoudi/open-webui-custom:latest
```

### تحديث تلقائي مع Watchtower

```bash
# تثبيت Watchtower للتحديث التلقائي
docker run -d \
  --name watchtower \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower \
  --schedule "0 0 2 * * *" \
  invai-platform
```

### نسخ احتياطي للبيانات

```bash
# إنشاء نسخة احتياطية
docker run --rm \
  -v open-webui:/data \
  -v $(pwd):/backup \
  alpine tar czf /backup/invai-backup-$(date +%Y%m%d).tar.gz -C /data .

# استعادة النسخة الاحتياطية
docker run --rm \
  -v open-webui:/data \
  -v $(pwd):/backup \
  alpine tar xzf /backup/invai-backup-20241201.tar.gz -C /data
```

---

## 📊 المراقبة والتحليلات

### مراقبة الأداء

```bash
# مراقبة استخدام الموارد
docker stats invai-platform

# عرض سجلات النظام
docker logs -f invai-platform

# مراقبة استخدام القرص
docker system df
```

### إعداد Prometheus للمراقبة

```yaml
# docker-compose.monitoring.yml
version: '3.8'

services:
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml

  grafana:
    image: grafana/grafana
    ports:
      - "3001:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
```

---

## 📄 الترخيص والحقوق

هذا المشروع مرخص تحت **رخصة MIT المعدلة**. يمكنك:

✅ **الاستخدام التجاري والشخصي**
✅ **التعديل والتطوير**
✅ **التوزيع**
✅ **الاستخدام في المشاريع المغلقة المصدر**

❗ **مع الحفاظ على**:
- إشعار حقوق الطبع والنشر
- نص الترخيص
- العلامة التجارية "INVAI" و "Open WebUI"

للتفاصيل الكاملة، راجع ملف [LICENSE](LICENSE).

---

## 🏆 الشكر والتقدير

### فريق التطوير
- **المطور الرئيسي**: فيصل الأنقودي
- **فريق Invast Oman**: للدعم والتوجيه
- **مجتمع Open WebUI**: للمنصة الأساسية الرائعة

### المساهمون
شكر خاص لجميع المساهمين في تطوير وتحسين المنصة:
- مطوري الواجهة الأمامية
- مطوري الخلفية
- مختبري النظام
- مترجمي المحتوى

### التقنيات المستخدمة
- **Frontend**: Svelte, TypeScript, Tailwind CSS
- **Backend**: Python, FastAPI, SQLAlchemy
- **AI Models**: Ollama, Qwen 2.5
- **Database**: SQLite/PostgreSQL
- **Containerization**: Docker, Docker Compose
- **Deployment**: Kubernetes, Helm

---

## 🌟 خارطة الطريق

### الإصدار القادم (v1.1)
- 🔍 **بحث متقدم** في قاعدة المعرفة
- 📊 **لوحة تحكم تحليلية** للاستثمارات
- 🤖 **نماذج AI متخصصة** لقطاعات مختلفة
- 🔗 **تكامل مع APIs خارجية** للبيانات المالية
- 📱 **تطبيق محمول** أصلي

### الإصدار المستقبلي (v2.0)
- 🧠 **ذكاء اصطناعي تنبؤي** للأسواق
- 🌐 **دعم متعدد المؤسسات**
- 🔐 **مصادقة متقدمة** (SSO, LDAP)
- 📈 **تحليلات في الوقت الفعلي**
- 🎯 **توصيات استثمارية ذكية**

---

## 💝 فريق التطوير

تم تطوير INVAI بواسطة فريق **NuqtaAI** مع ❤️ في **سلطنة عُمان** 🇴🇲

---

*آخر تحديث: ديسمبر 2024*

**للمزيد من المعلومات، تفضل بزيارة [موقعنا الرسمي](https://invast.om) أو تواصل معنا عبر [البريد الإلكتروني](mailto:info@invast.om)**
