<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "ac2459c0d5cc823922e3d9240a95028c",
  "translation_date": "2025-07-13T19:06:06+00:00",
  "source_file": "03-GettingStarted/03-llm-client/solution/java/README.md",
  "language_code": "ar"
}
-->
# عميل حاسبة LLM

تطبيق جافا يوضح كيفية استخدام LangChain4j للاتصال بخدمة حاسبة MCP (بروتوكول سياق النموذج) مع تكامل نماذج GitHub.

## المتطلبات الأساسية

- جافا 21 أو أحدث  
- مافن 3.6+ (أو استخدم ملف مافن المرفق)  
- حساب GitHub مع صلاحية الوصول إلى نماذج GitHub  
- خدمة حاسبة MCP تعمل على `http://localhost:8080`  

## الحصول على رمز GitHub

يستخدم هذا التطبيق نماذج GitHub التي تتطلب رمز وصول شخصي من GitHub. اتبع الخطوات التالية للحصول على الرمز:

### 1. الوصول إلى نماذج GitHub  
1. اذهب إلى [نماذج GitHub](https://github.com/marketplace/models)  
2. سجّل الدخول باستخدام حساب GitHub الخاص بك  
3. اطلب الوصول إلى نماذج GitHub إذا لم تكن قد فعلت ذلك مسبقًا  

### 2. إنشاء رمز وصول شخصي  
1. اذهب إلى [إعدادات GitHub → إعدادات المطور → رموز الوصول الشخصية → الرموز (الكلاسيكية)](https://github.com/settings/tokens)  
2. اضغط على "إنشاء رمز جديد" → "إنشاء رمز جديد (كلاسيكي)"  
3. امنح الرمز اسمًا وصفيًا (مثل "عميل حاسبة MCP")  
4. اضبط تاريخ الانتهاء حسب الحاجة  
5. اختر الصلاحيات التالية:  
   - `repo` (إذا كنت تحتاج الوصول إلى المستودعات الخاصة)  
   - `user:email`  
6. اضغط على "إنشاء رمز"  
7. **مهم**: انسخ الرمز فورًا - لن تتمكن من رؤيته مرة أخرى!  

### 3. تعيين متغير البيئة

#### على ويندوز (موجه الأوامر):  
```cmd
set GITHUB_TOKEN=your_github_token_here
```

#### على ويندوز (PowerShell):  
```powershell
$env:GITHUB_TOKEN="your_github_token_here"
```

#### على macOS/Linux:  
```bash
export GITHUB_TOKEN=your_github_token_here
```

## الإعداد والتثبيت

1. **استنساخ المشروع أو الانتقال إلى مجلد المشروع**

2. **تثبيت التبعيات**:  
   ```cmd
   mvnw clean install
   ```  
   أو إذا كان لديك مافن مثبتًا عالميًا:  
   ```cmd
   mvn clean install
   ```

3. **إعداد متغير البيئة** (راجع قسم "الحصول على رمز GitHub" أعلاه)

4. **تشغيل خدمة حاسبة MCP**:  
   تأكد من تشغيل خدمة حاسبة MCP من الفصل الأول على `http://localhost:8080/sse`. يجب أن تكون الخدمة تعمل قبل بدء العميل.

## تشغيل التطبيق

```cmd
mvnw clean package
java -jar target\calculator-llm-client-0.0.1-SNAPSHOT.jar
```

## ما الذي يفعله التطبيق

يعرض التطبيق ثلاث تفاعلات رئيسية مع خدمة الحاسبة:

1. **الجمع**: يحسب مجموع 24.5 و 17.3  
2. **الجذر التربيعي**: يحسب الجذر التربيعي للعدد 144  
3. **المساعدة**: يعرض الوظائف المتاحة في الحاسبة  

## المخرجات المتوقعة

عند التشغيل بنجاح، سترى مخرجات مشابهة لـ:

```
The sum of 24.5 and 17.3 is 41.8.
The square root of 144 is 12.
The calculator service provides the following functions: add, subtract, multiply, divide, sqrt, power...
```

## استكشاف الأخطاء وإصلاحها

### المشاكل الشائعة

1. **"متغير البيئة GITHUB_TOKEN غير مضبوط"**  
   - تأكد من تعيين متغير البيئة `GITHUB_TOKEN`  
   - أعد تشغيل الطرفية/موجه الأوامر بعد تعيين المتغير  

2. **"تم رفض الاتصال بـ localhost:8080"**  
   - تحقق من تشغيل خدمة حاسبة MCP على المنفذ 8080  
   - تأكد من عدم وجود خدمة أخرى تستخدم المنفذ 8080  

3. **"فشل التوثيق"**  
   - تحقق من صلاحية رمز GitHub الخاص بك وأنه يمتلك الأذونات الصحيحة  
   - تأكد من أن لديك حق الوصول إلى نماذج GitHub  

4. **أخطاء بناء مافن**  
   - تأكد من استخدام جافا 21 أو أحدث: `java -version`  
   - جرب تنظيف البناء: `mvnw clean`  

### التصحيح

لتمكين تسجيل الأخطاء التفصيلي، أضف الوسيطة التالية لـ JVM عند التشغيل:  
```cmd
java -Dlogging.level.dev.langchain4j=DEBUG -jar target\calculator-llm-client-0.0.1-SNAPSHOT.jar
```

## التهيئة

تم تكوين التطبيق ليقوم بـ:  
- استخدام نماذج GitHub مع نموذج `gpt-4.1-nano`  
- الاتصال بخدمة MCP على `http://localhost:8080/sse`  
- استخدام مهلة 60 ثانية للطلبات  
- تمكين تسجيل الطلبات/الاستجابات لأغراض التصحيح  

## التبعيات

التبعيات الرئيسية المستخدمة في هذا المشروع:  
- **LangChain4j**: للتكامل مع الذكاء الاصطناعي وإدارة الأدوات  
- **LangChain4j MCP**: لدعم بروتوكول سياق النموذج  
- **LangChain4j GitHub Models**: لتكامل نماذج GitHub  
- **Spring Boot**: لإطار العمل وحقن التبعيات  

## الترخيص

هذا المشروع مرخص بموجب رخصة Apache 2.0 - راجع ملف [LICENSE](../../../../../../03-GettingStarted/03-llm-client/solution/java/LICENSE) للتفاصيل.

**إخلاء المسؤولية**:  
تمت ترجمة هذا المستند باستخدام خدمة الترجمة الآلية [Co-op Translator](https://github.com/Azure/co-op-translator). بينما نسعى لتحقيق الدقة، يرجى العلم أن الترجمات الآلية قد تحتوي على أخطاء أو عدم دقة. يجب اعتبار المستند الأصلي بلغته الأصلية المصدر الموثوق به. للمعلومات الهامة، يُنصح بالاعتماد على الترجمة البشرية المهنية. نحن غير مسؤولين عن أي سوء فهم أو تفسير ناتج عن استخدام هذه الترجمة.