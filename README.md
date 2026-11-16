# Shorts Bot: الموزة والفريز

كل نهار 3 فيديوهات كرتونية مضحكة كيتولدو وكيتنشرو أوتوماتيكيا على YouTube Shorts وTikTok وInstagram Reels.

## كيفاش خدام
Claude كيكتب السيناريو ← Replicate (FLUX) كيرسم الصور ← edge-tts كيدير الصوت ← ffmpeg كيركب الفيديو ب subtitles ← النشر.

## خطوات التركيب

1. **GitHub**: دير repo جديد (private ممكن) وحط فيه هاد الملفات كما هي.
2. **Anthropic**: خود API key من console.anthropic.com ← سر `ANTHROPIC_API_KEY`.
3. **Replicate**: دير حساب وخود token ← سر `REPLICATE_API_TOKEN`.
4. **جرب بلا نشر**: Actions ← Daily shorts ← Run workflow (خلي dry_run مفعل) ← حمل الفيديو من Artifacts وشوفو.
5. **YouTube**:
   - Google Cloud Console: دير project، فعل YouTube Data API v3، دير OAuth client من نوع Desktop.
   - OAuth consent screen خليه **In production** (إلا بقا Testing الـ refresh token كيسالي من بعد 7 أيام).
   - شغل `tools/get_youtube_token.py client_secret.json` فالكومبيوتر ديالك.
   - الأسرار: `YT_CLIENT_ID`, `YT_CLIENT_SECRET`, `YT_REFRESH_TOKEN`.
   - الفيديوهات المرفوعة من مشروع API ما مراجعش كتبقى private حتى تدوز audit ديال YouTube.
   - صرح بصدق واش المحتوى made for kids: Variable `YT_MADE_FOR_KIDS=true` إلا كان موجه للأطفال.
6. **TikTok**:
   - developers.tiktok.com: دير app، زيد Content Posting API، ودير Direct Post.
   - دير OAuth مرة وحدة (scope `video.publish`) باش تاخد refresh token.
   - الأسرار: `TIKTOK_CLIENT_KEY`, `TIKTOK_CLIENT_SECRET`, `TIKTOK_REFRESH_TOKEN`.
   - قبل audit ديال TikTok الفيديوهات كتبقى private فقط (`SELF_ONLY`). من بعد الموافقة بدل Variable `TIKTOK_PRIVACY=PUBLIC_TO_EVERYONE`.
7. **Instagram**:
   - الحساب خاصو يكون Business ولا Creator ومربوط بصفحة Facebook.
   - developers.facebook.com: app مع Instagram Graph API وصلاحيات `instagram_content_publish`.
   - الأسرار: `IG_USER_ID`, `IG_ACCESS_TOKEN` (long-lived، كتحتاج تجديد).
   - Instagram كيطلب رابط عمومي للفيديو، لهذا خاصك bucket (Cloudflare R2 فيه free tier) مع public domain.
   - الأسرار: `S3_ENDPOINT`, `S3_BUCKET`, `S3_KEY_ID`, `S3_SECRET`, `PUBLIC_BASE_URL`.
8. **شغل أوتوماتيكيا**: الـ cron كيدير 3 مرات فاليوم (09:00، 14:00، 19:00 بتوقيت المغرب، بدلهم فالـ yml حسب رمضان).
   باش توقف منصة وحدة: Variable `ENABLED_PLATFORMS=youtube,instagram`.

## ملاحظات
- GitHub Actions cron ممكن يتأخر شي دقائق.
- الأصوات: `TTS_VOICE=ar-MA-MounaNeural` للصوت النسائي.
- شخصيات الموزة والفريز كتتبدل من `src/config.py`.
