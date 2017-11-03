---
title: "Android API katılım kullanma"
description: "En son Android SDK - Android API katılım kullanma"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 09b62659-82ae-4a55-8784-fca0b6b22eaf
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: na
ms.topic: article
ms.date: 07/25/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: d353cd2fe47c54a0282cc5bb1b22b4a56e0cd82c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-engagement-api-on-android"></a>Android API katılım kullanma
Bu belge belgeye bir eklentidir [Android Mobile Engagement SDK'sı için Gelişmiş raporlama seçenekleri](mobile-engagement-android-advanced-reporting.md). Katılım API uygulama istatistikleri rapor için nasıl kullanılacağı hakkındaki derinliği ayrıntıları sağlar.

Uygulamanızın oturumları, etkinlikleri, kilitlenme ve teknik bilgileri raporlamak için katılım yalnızca istiyorsanız, sonra en basit yolu tüm olduğunu aklınızda bulundurun, `Activity` alt sınıfları devralır denk gelen `EngagementActivity` sınıfı.

Daha fazla bilgi için uygulama belirli olaylar, hatalar ve işleri, rapor gerekiyorsa örnek yapmak istiyorsanız veya uygulamanızın etkinlikleri uygulanan bir daha farklı bir şekilde bildirmek varsa `EngagementActivity` sınıfları yeniden katılım API'sini kullanmanız gerekiyor.

Katılım API'si tarafından sağlanan `EngagementAgent` sınıfı. Bu sınıfın örneğini çağırarak alınabilir `EngagementAgent.getInstance(Context)` statik yöntemi (unutmayın `EngagementAgent` döndürülen tek nesnesidir).

## <a name="engagement-concepts"></a>Engagement kavramları
Aşağıdaki bölümleri yaygın İyileştir [Mobile Engagement kavramları](mobile-engagement-concepts.md), Android platformu için.

### <a name="session-and-activity"></a>`Session` ve `Activity`
Kullanıcı birden fazla birkaç saniye arasında iki boşta kalırsa *etkinlikleri*, daha sonra kendi dizisini *etkinlikleri* iki ayrı bölünen *oturumları*. Bu birkaç saniye "oturum zaman aşımı" adı verilir.

Bir *etkinlik* yani genellikle uygulamanın bir ekran ile ilişkilendirilen *etkinlik* ekranı görüntülenir ve ekran kapatıldığında durdurduğunda başlatır: Engagement SDK'sını kullanarak tümleştirildiğinde bu durumda `EngagementActivity` sınıfları.

Ancak *etkinlikleri* de el ile katılım API'si kullanılarak denetlenebilir. Bu, birkaç alt bölümde bu ekrana (örneğin bilinen ne sıklıkta ve ne kadar süreyle iletişim kutuları içinde bu ekran kullanılır) kullanımı hakkında daha fazla bilgi almak için belirli bir ekran bölmek için sağlar.

## <a name="reporting-activities"></a>Raporlama etkinlikleri
> [!IMPORTANT]
> Etkinlikler gibi rapor kullanıyorsanız, bu bölümde açıklanan gerekmeyen `EngagementActivity` sınıfı ve türevleri tümleştirmek katılım nasıl Android belgede açıklandığı gibi.
> 
> 

### <a name="user-starts-a-new-activity"></a>Kullanıcı yeni bir etkinlik başlatır
            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing the current activity is required for Reach to display in-app notifications, passing null will postpone such announcements and polls.

Çağırmanız gerekir `startActivity()` kullanıcı etkinliği değişiklikleri her zaman. Bu işlev ilk çağrıda yeni bir kullanıcı oturumu başlatır.

Bu işlevi çağırmak için en iyi her faaliyete yerdir `onResume` geri çağırma.

### <a name="user-ends-his-current-activity"></a>Kullanıcı kendi geçerli etkinliği sona erer
            EngagementAgent.getInstance(this).endActivity();

Çağırmanız gerekir `endActivity()` en az bir kez kullanıcı tamamlandığında son etkinliğini. Bu kullanıcı şu anda boşta kalır ve bir kez oturum zaman aşımı kapatılacak kullanıcı oturumunu gereksinim dolacak Engagement SDK'sı bildirir (çağırırsanız `startActivity()` oturumu yalnızca oturum zaman aşımı süresi dolmadan önce sürdürüldü).

Bu işlevi çağırmak için en iyi her faaliyete yerdir `onPause` geri çağırma.

## <a name="reporting-events"></a>Raporlama olayları
### <a name="session-events"></a>Oturum olayları
Oturum olaylar, genellikle kendi oturumu sırasında bir kullanıcı tarafından gerçekleştirilen eylemleri bildirmek için kullanılır.

**Ek veriler olmadan örneği:**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**Örnek ek veriler ile:**

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a>Tek başına olayları
Oturum olayları aykırı bir oturum bağlamı dışında tek başına olaylar gerçekleşebilir.

**Örnek:**

Bir yayın alıcı tetiklendiğinde rapor olayların için istediğinizi varsayalım:

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

## <a name="reporting-errors"></a>Hata Raporlama
### <a name="session-errors"></a>Oturum hataları
Oturum hatalar genellikle kendi oturumu sırasında kullanıcı etkileyen hatalarını bildirmek için kullanılır.

**Örnek:**

            /** The user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* The user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a>Tek başına hataları
Oturum hataları aykırı bir oturum bağlamı dışında tek başına hatalar oluşabilir.

**Örnek:**

Aşağıdaki örnek, uygulama işlemi çalışırken bellek telefonda azaldığında her bir hata raporu gösterilmektedir.

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

## <a name="reporting-jobs"></a>Raporlama işleri
### <a name="example"></a>Örnek
Oturum açma işleminiz süresini rapor istediğinizi varsayalım:

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a>Bir işi sırasında hatalarını raporla
Geçerli kullanıcı oturumuyla ilgili yerine çalıştırılan bir iş hataları ile ilgili olabilir.

**Örnek:**

Raporu, sırasında bir hata oturum açma işlemi istediğinizi varsayalım:

[...] Ortak void Signın (bağlam bağlamı,...) {

              /* We need an Android context to call the Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try to sign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report the error to Engagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a>Raporlama işi sırasında olayları
Geçerli kullanıcı oturumuyla ilgili yerine çalıştırılan bir iş olayları ile ilgili olabilir.

**Örnek:**

Sosyal ağ sahibiz ve rapor için bir iş sunucusuna bağlı kullanıcı sırasında toplam süre kullanırız varsayalım. Bu yüzden hiçbir oturum kullanıcı kendisinin başka bir uygulama kullanırken ya da telefon Uyuma arka planda bile bağlı kalabilir.

Kullanıcı kendi arkadaşlarınızdan ileti alabilir, bu proje bir olaydır.

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

## <a name="extra-parameters"></a>Ek parametreler
Rastgele veriler, olaylar, hatalar, etkinlikler ve işler eklenebilir.

Bu verileri yapılandırılmış, Android'ın paket sınıfı kullanır (aslında, Android hedefleri ek parametreleri gibi çalışır). Bir paket dizileri veya başka bir paket örneklerini içerebileceğini unutmayın.

> [!IMPORTANT]
> Parcelable veya seri hale getirilebilir parametrelerinde yerleştirirseniz emin olun, `toString()` yöntemi okunabilir dize döndürecek şekilde gerçekleştirilir. Çağıracaksınız sırasında serileştirilebilir olmayan geçici olmayan alanlar içeriyor serileştirilebilir sınıflardan Android kilitlenme yapar`bundle.putSerializable("key",value);`
> 
> [!WARNING]
> Ek parametreler seyrek dizilerde desteklenmez, diğer bir deyişle, bir dizi olarak seri hale getirilmesi olmaz. Bunları standart diziye ek parametreler kullanmadan önce dönüştürmeniz.
> 
> 

### <a name="example"></a>Örnek
            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a>Sınırlar
#### <a name="keys"></a>Anahtarlar
Her anahtarında `Bundle` şu normal ifadeyle eşleşen gerekir:

`^[a-zA-Z][a-zA-Z_0-9]*`

Anahtarları harfler, sayılar veya alt çizgi izlemelidir en az bir harf ile başlamalıdır anlamına gelir (\_).

#### <a name="size"></a>Boyut
Ek özellikler sınırlı **1024** (kez JSON'de katılım hizmeti tarafından kodlanmış) çağrı başına karakter.

Önceki örnekte, sunucuya gönderilen JSON 58 karakter olacak:

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a>Uygulama bilgilerini raporlama
İzleme bilgileri (veya diğer uygulama belirli bilgileri) kullanarak el ile raporlayabilirsiniz `sendAppInfo()` işlevi.

Bu bilgi artımlı olarak gönderilebilir Not: belirli bir aygıt için belirli bir anahtar için yalnızca en son değeri korunur.

Olay ek özellikler gibi paket sınıfı uygulama bilgilerini soyut, kullanılan diziler veya alt paketleri (JSON serileştirmesi kullanan) düz dize olarak kabul olduğunu unutmayın.

### <a name="example"></a>Örnek
Kullanıcı cinsiyeti ve doğum tarihi göndermek için bir kod örneği şöyledir:

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a>Sınırlar
#### <a name="keys"></a>Anahtarlar
Her anahtarında `Bundle` şu normal ifadeyle eşleşen gerekir:

`^[a-zA-Z][a-zA-Z_0-9]*`

Anahtarları harfler, sayılar veya alt çizgi izlemelidir en az bir harf ile başlamalıdır anlamına gelir (\_).

#### <a name="size"></a>Boyut
Uygulama bilgilerini sınırlı **1024** (kez JSON'de katılım hizmeti tarafından kodlanmış) çağrı başına karakter.

Önceki örnekte, sunucuya gönderilen JSON 44 karakter olacak:

            {"expiration":"2016-12-07","status":"premium"}
