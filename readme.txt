DigitalZone — Starter with Admin Catalog Management

Files:
- index.html: Arabic RTL storefront connected to the DigitalZone Supabase project.

Customer checkout:
1. Customer signs in and chooses an active product.
2. Checkout shows the configured Vodafone Cash wallet number.
3. Customer enters sender phone, optional transfer reference, payment screenshot, and delivery destination.
4. Order is submitted for manual review. The 10-minute notice is an estimate during business hours, not a guaranteed SLA.
5. Admin can review orders and open a short-lived signed URL for the private payment proof.

Admin catalog:
- Admin-only catalog section lists products, including hidden drafts.
- Admin can edit product name, slug, category, description, duration, image URL, sale price, stock status, and publication state.
- Admin can add new products as hidden drafts.
- Supplier cost is stored in the private public.product_costs table with RLS allowing only authenticated users listed in public.admin_profiles. It is not included in public product queries.
- Estimated margin is sale price minus supplier cost.
- Publishing is blocked in the UI when price is zero or stock status is unavailable. Review resale rights, account/region restrictions, supplier availability, price, and delivery method before publishing.

Security notes:
- Payment proof images are stored in the private Supabase Storage bucket `order-payment-proofs`.
- Never put a Supabase service-role/secret key in this file.
- Never confirm payment only because an image was uploaded. Verify the transfer independently.
- Do not ask customers for passwords or OTPs. Use only permitted gifting/transfer methods.
- This remains a prototype and has not been verified end-to-end. Before launch, test sign-in, uploads, order creation, admin review, product editing, and delivery with test orders.

Deployment:
- This ZIP contains the updated local prototype only; it does not publish the website or modify any existing Netlify deployment.

Admin dashboard summary:
- The catalog dashboard displays total products, published products, hidden drafts, zero-price products, and unavailable products.
- These counters are calculated from the admin-only catalog result and do not expose supplier costs to customers.


لوحة الإدارة المستقلة (admin.html)
- افتح admin.html من نفس الاستضافة لواجهة الإدارة المنفصلة.
- سجّل الدخول بالبريد المعتمد الموجود في public.admin_profiles.
- لوحة الإدارة تستخدم نفس مشروع Supabase وقاعدة البيانات وسياسات RLS؛ لا تضع service_role key في المتصفح.
- تعديلات المنتجات والأسعار وتكلفة المورد وحالة النشر والطلبات تتم من لوحة الإدارة، دون تعديل ملفات المتجر لكل تغيير.
- النسخة غير منشورة تلقائيًا، ولم يكتمل اختبار دورة الطلب الحقيقية؛ ارفعها للاستضافة ثم اختبر تسجيل الدخول والطلبات قبل استقبال أموال.

Storefront design update (responsive concept 1)
- The storefront uses a clean, balanced product grid with a strong brand hero and Arabic marketing message.
- `digitalzone-branding.png` is the supplied DigitalZone branding artwork used in the hero.
- `digitalzone-mark.jpg` is the compact header mark.
- CSS layouts adapt for desktop, tablet, and mobile. The product grid uses two columns on typical mobile widths and one column on very narrow screens.
- The existing Supabase client logic, order flow, admin catalog, and `admin.html` were preserved; only storefront markup/styles and local image assets were changed.
- JavaScript syntax check passed locally. This does not replace live integration testing against Supabase.


Authentication update (v6):
- Signup flow asks for a six-digit email OTP and verifies it with Supabase Auth.
- Login shows a clear Arabic message for invalid credentials.
- Forgot-password flow requests a recovery code, verifies it, then updates the password.
- Supabase Auth email templates must be configured to send the 6-digit token (use {{ .Token }} in the Confirm signup and Reset Password templates). If templates still send only links, the in-page six-digit OTP experience will not work until the templates are updated in Supabase Dashboard > Authentication > Email Templates.
- Confirm Site URL and Redirect URLs for the Netlify deployment under Authentication > URL Configuration.
- This package has passed static JavaScript syntax and ZIP integrity checks; live email delivery and full auth flow require testing on the deployed Supabase project.
