DigitalZone — النسخة النهائية لإعادة البناء

المشروع:
- index.html: متجر العملاء.
- admin.html: لوحة الإدارة.
- المتجر واللوحة يستخدمان مشروع Supabase الحالي: smwbaohdcacfzebcxmlc.
- لوحة الإدارة محمية عبر public.admin_profiles وRLS؛ لا يوجد service_role key في الواجهة.
- المنتجات منظمة حسب category، واللوحة تعرضها كأقسام وكروت بصور. عند عدم وجود image_url يتم توليد صورة SVG موحدة تلقائيًا من اسم المنتج والقسم.
- إدارة المنتجات: إضافة، تعديل، نشر/إخفاء، السعر، تكلفة المورد، الوصف، المدة، الحالة.
- الطلبات: إنشاء الطلب عبر Edge Function create-order، ومراجعة الدفع عبر review-payment.
- إثبات التحويل يبقى في bucket خاص ويُفتح للمدير عبر signed URL مؤقت.
- تسجيل Google موجود في المتجر عبر Supabase Auth OAuth.
- مفاتيح Supabase المستخدمة في الواجهة publishable/anon فقط.

اختبارات البناء:
- تم فحص JavaScript في index.html وadmin.html بعد البناء، والملفان صالحان نحويًا.
- قاعدة البيانات الحالية تحتوي 243 منتجًا، منها 5 منشورة و238 مسودة، ومديرًا واحدًا وطلبين.
- Edge Functions create-order وreview-payment حالتهما ACTIVE وverify_jwt=true.
- مراجعة RLS الحالية تظهر حماية للمنتجات والطلبات والتكاليف والـadmin_profiles.

النشر:
- نسخة GitHub النهائية جاهزة للدمج.
- نشر Worker مباشرة من اتصال Cloudflare المتاح حاليًا رفضه الحساب بصلاحية الكتابة؛ لذلك لا يُدّعى أن رابط Cloudflare الجديد تم نشره حتى يتم تأكيد النشر فعليًا.
