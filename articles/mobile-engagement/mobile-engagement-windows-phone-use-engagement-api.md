---
title: Windows Phone Silverlight'ın katılım API kullanma
description: Windows Phone Silverlight'ın katılım API kullanma
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: ae2ba2e8-f75b-4dee-a164-a7dd65d35a23
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 03673924ee7734fcf6f1f4f7c744616844fdc87a
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="how-to-use-the-engagement-api-on-windows-phone-silverlight"></a>Windows Phone Silverlight'ın katılım API kullanma
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

Bu belge belgeye bir eklentidir [Mobile Engagement Windows Phone Silverlight uygulamanıza tümleştirmek nasıl](mobile-engagement-windows-phone-integrate-engagement.md). Katılım API uygulama istatistikleri rapor için nasıl kullanılacağı hakkındaki derinliği ayrıntıları sağlar.

Yalnızca uygulamanızın oturumları, etkinlikleri, kilitlenme ve teknik bilgileri raporlamak için katılım istediğiniz sonra en basit yolu, tüm olmasını sağlamaktır, `PhoneApplicationPage` alt sınıfları `EngagementPage` sınıfı.

Daha fazla bilgi için uygulama belirli olaylar, hatalar ve işleri, rapor gerekiyorsa örnek yapmak istiyorsanız veya uygulamanızın etkinlikleri uygulanan bir daha farklı bir şekilde bildirmek varsa `EngagementPage` sınıfları yeniden katılım API'sini kullanmanız gerekiyor.

Katılım API'si tarafından sağlanan `EngagementAgent` sınıfı. Bu yöntemlerle erişebilirsiniz `EngagementAgent.Instance`.

Aracı modülü başlatılmadı olsa bile, her API çağrısı ertelenir ve aracı kullanılabilir olduğunda yeniden yürütülür.

## <a name="engagement-concepts"></a>Engagement kavramları
Aşağıdaki bölümleri Windows Phone platformu için Mobile Engagement kavramları daraltın.

### <a name="session-and-activity"></a>`Session` ve `Activity`
Bir *etkinlik* yani genellikle uygulama bir sayfayla ilişkilendirilen *etkinlik* sayfası görüntülenir ve sayfa kapatıldığında durdurduğunda başlatır: Engagement SDK'sını kullanarak tümleştirildiğinde bu durumda `EngagementPage` sınıfı.

Ancak *etkinlikleri* de el ile katılım API'si kullanılarak denetlenebilir. Bu, birkaç alt bölümde bu sayfaya (örneğin bilinen ne sıklıkta ve ne kadar süre içinde bu sayfa iletişim kutuları kullanılır) kullanımı hakkında daha fazla bilgi almak için belirli bir sayfa bölmek için sağlar.

## <a name="reporting-activities"></a>Raporlama etkinlikleri
### <a name="user-starts-a-new-activity"></a>Kullanıcı yeni bir etkinlik başlatır
#### <a name="reference"></a>Başvuru
            void StartActivity(string name, Dictionary<object, object> extras = null)

Çağırmanız gerekir `StartActivity()` kullanıcı etkinliği değişiklikleri her zaman. Bu işlev ilk çağrıda yeni bir kullanıcı oturumu başlatır.

> [!IMPORTANT]
> SDK otomatik olarak çağrısı EndActivity yöntemi uygulama kapatıldığında. Bu nedenle, kullanıcı değiştirme ve hiçbir zaman için etkinlik çağrı her EndActivity yöntemi bu yöntemi çağırmadan sonlandırılmak üzere geçerli oturumdaki zorlar beri StartActivity yöntemini çağırmak için önerilir.
> 
> 

#### <a name="example"></a>Örnek
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Kullanıcı kendi geçerli etkinliği sona erer
#### <a name="reference"></a>Başvuru
            void EndActivity()

Çağırmanız gerekir `EndActivity()` en az bir kez kullanıcı tamamlandığında son etkinliğini. Bu kullanıcı şu anda boşta kalır ve bir kez oturum zaman aşımı kapatılacak kullanıcı oturumunu gereksinim dolacak Engagement SDK'sı bildirir (çağırırsanız `StartActivity()` oturum, yalnızca oturum zaman aşımı süresi dolmadan önce devam ettirildiğinde).

#### <a name="example"></a>Örnek
            EngagementAgent.Instance.EndActivity();

## <a name="reporting-jobs"></a>Raporlama işleri
### <a name="start-a-job"></a>Bir işi Başlat
#### <a name="reference"></a>Başvuru
            void StartJob(string name, Dictionary<object, object> extras = null)

İş, bir süre boyunca bazı görevleri izlemek için kullanabilirsiniz.

#### <a name="example"></a>Örnek
            // An upload begins...

            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");

            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>Bir işin bitiş
#### <a name="reference"></a>Başvuru
            void EndJob(string name)

Bir iş tarafından izlenen bir görev sonlandırıldı hemen iş adı sağlayarak bu proje için EndJob yöntemi çağırmanız gerekir.

#### <a name="example"></a>Örnek
            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends

            EngagementAgent.Instance.EndJob("uploadData");

## <a name="reporting-events"></a>Raporlama olayları
Üç tür olay vardır:

* Tek başına olayları
* Oturum olayları
* İş olayları

### <a name="standalone-events"></a>Tek başına olayları
#### <a name="reference"></a>Başvuru
            void SendEvent(string name, Dictionary<object, object> extras = null)

Tek başına olaylar oturum bağlamı dışında gerçekleşebilir.

#### <a name="example"></a>Örnek
            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Oturum olayları
#### <a name="reference"></a>Başvuru
            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Oturum olaylar, genellikle kendi oturumu sırasında bir kullanıcı tarafından gerçekleştirilen eylemleri bildirmek için kullanılır.

#### <a name="example"></a>Örnek
**Veri olmadan:**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");

            // or

            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**Verilerle:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>İş olayları
#### <a name="reference"></a>Başvuru
            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

İş olaylar, genellikle bir işi sırasında bir kullanıcı tarafından gerçekleştirilen eylemleri bildirmek için kullanılır.

#### <a name="example"></a>Örnek
            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## <a name="reporting-errors"></a>Hata Raporlama
Üç tür hataları vardır:

* Tek başına hataları
* Oturum hataları
* İş hataları

### <a name="standalone-errors"></a>Tek başına hataları
#### <a name="reference"></a>Başvuru
            void SendError(string name, Dictionary<object, object> extras = null)

Oturum hataları aykırı bir oturum bağlamı dışında tek başına hatalar oluşabilir.

#### <a name="example"></a>Örnek
            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Oturum hataları
#### <a name="reference"></a>Başvuru
            void SendSessionError(string name, Dictionary<object, object> extras = null)

Oturum hatalar genellikle kendi oturumu sırasında kullanıcı etkileyen hatalarını bildirmek için kullanılır.

#### <a name="example"></a>Örnek
            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>İş hataları
#### <a name="reference"></a>Başvuru
            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Geçerli kullanıcı oturumuyla ilgili yerine çalıştırılan bir iş hataları ile ilgili olabilir.

#### <a name="example"></a>Örnek
            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## <a name="reporting-crashes"></a>Raporlama çökme (Crash)
Aracı çökme (Crash) ile mücadele etmek için iki yöntem sunar.

### <a name="send-an-exception"></a>Bir özel durum Gönder
#### <a name="reference"></a>Başvuru
            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Örnek
Bir özel durum herhangi bir zamanda çağırarak gönderebilirsiniz:

            EngagementAgent.Instance.SendCrash(aCatchedException);

İsteğe bağlı parametresi, kilitlenme gönderme daha aynı anda katılım oturumu sona erdirmek için de kullanabilirsiniz. Bunu yapmak için arayın:

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Bunu yaparsanız, oturum ve işleri kilitlenme gönderdikten sonra kapatılacak.

### <a name="send-an-unhandled-exception"></a>İşlenmeyen bir özel durum Gönder
#### <a name="reference"></a>Başvuru
            void SendCrash(ApplicationUnhandledExceptionEventArgs e)

Katılım ayrıca işlenmeyen özel durumlar göndermek için bir yöntem sunar. Silverlight UnhandledException olay işleyicisinin içinden kullanıldığında bu özellikle yararlıdır.

Bu yöntem olacak **her zaman** adlı sonra işler ve katılım oturum sonlandırılacak.

#### <a name="example"></a>Örnek
Kendi UnhandledException işleyicisi (özellikle, raporlama katılım özelliği otomatik kilitlenme devre dışı bıraktıysanız) uygulamak için kullanabilirsiniz. Örneğin, `Application_UnhandledException` yöntemi `App.xaml.cs` dosyası:

            // In your App.xaml.cs file

            // Code to execute on Unhandled Exceptions
            private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
            {
              // your own code

              EngagementAgent.Instance.SendCrash(e);
            }

## <a name="onactivated"></a>OnActivated
### <a name="reference"></a>Başvuru
            void OnActivated(ActivatedEventArgs e)

Devre dışı olay yükseltildikten sonra kullanıcı İleri, bir uygulama çıktığınızda gittiğinde işletim sistemini uygulama etkinliği olmayan bir duruma dener. Ardından, kaldırıldı olarak işaretleme uygulamasıdır. Bu işlemde bir uygulama sonlandırıldı, ancak uygulama ve uygulama içinde tek tek sayfaları durumuyla ilgili bazı veriler korunur.

Eklemek zorunda `EngagementAgent.Instance.OnActivated(e)` içinde `Application_Activated` kaldırıldı uygulama kaldırıldığında katılım Aracısı sıfırlamaya App.xaml.cs dosyasındaki yöntemi.

### <a name="example"></a>Örnek
            // Inside your App.xaml.cs file

            // Code to execute when the application is activated (brought to foreground)
            // This code will not execute when the application is first launched
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
              EngagementAgent.Instance.OnActivated(e);
            }

## <a name="device-id"></a>Cihaz Kimliği
            String GetDeviceId()

Bu yöntemi çağrılarak engagement cihaz kimliğini elde edebilirsiniz.

## <a name="extras-parameters"></a>Ek özellikler parametreleri
Rastgele veriler, bir olay, bir hata, bir etkinlik veya bir iş için eklenebilir. Bu verilerin bir sözlüğünü kullanarak yapılandırılmış. Anahtarları ve değerleri herhangi bir türde olabilir.

Bu tür için bir veri sözleşmesi eklemek zorunda ek özellikler kendi türü eklemek istiyorsanız şekilde ek özellikler veri serileştirilir.

### <a name="example"></a>Örnek
Yeni bir sınıf "Kişi" oluşturuyoruz.

            using System.Runtime.Serialization;

            namespace Engagement.Agent
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }

                // Properties

                [DataMember]
                public int Age
                {
                  get;
                  set;
                }

                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

Ardından, ekleyeceğiz bir `Person` fazladan örneğine.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);

            EngagementAgent.Instance.SendEvent("Event", extras);

> [!WARNING]
> Diğer nesne türlerini yerleştirirseniz, kendi ToString() yöntemini İnsan okunabilir dize döndürecek şekilde uygulanan emin olun.
> 
> 

### <a name="limits"></a>Sınırlar
#### <a name="keys"></a>Anahtarlar
Nesne tablosundaki her anahtarın şu normal ifadeyle aynı olması gerekir.

`^[a-zA-Z][a-zA-Z_0-9]*$`

Anahtarları harfler, sayılar veya alt çizgi izlemelidir en az bir harf ile başlamalıdır anlamına gelir (\_).

#### <a name="size"></a>Boyut
Ek özellikler sınırlı **1024** çağrı başına karakter.

## <a name="reporting-application-information"></a>Uygulama bilgilerini raporlama
### <a name="reference"></a>Başvuru
            void SendAppInfo(Dictionary<object, object> appInfos)

El ile SendAppInfo() kullanarak bilgi (veya diğer uygulama belirli bilgileri) izleme işlevini bildirebilirsiniz.

Bu bilgi artımlı olarak gönderilebilir Not: belirli bir aygıt için belirli bir anahtar için yalnızca en son değeri korunur. Olay ek özellikler gibi sözlük kullanma\<nesne, nesne\> bilgileri eklemek için.

### <a name="example"></a>Örnek
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
            {
               {"subscription", "2013-12-07"},
               {"premium", "true"}
            };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Sınırlar
#### <a name="keys"></a>Anahtarlar
Nesne tablosundaki her anahtarın şu normal ifadeyle aynı olması gerekir.

`^[a-zA-Z][a-zA-Z_0-9]*$`

Anahtarları harfler, sayılar veya alt çizgi izlemelidir en az bir harf ile başlamalıdır anlamına gelir (\_).

#### <a name="size"></a>Boyut
Uygulama bilgilerini sınırlı **1024** çağrı başına karakter.

Önceki örnekte, sunucuya gönderilen JSON 44 karakter olacak:

            {"subscription":"2013-12-07","premium":"true"}

## <a name="logging"></a>Günlüğe kaydetme
### <a name="enable-logging"></a>Günlük kaydını etkinleştir
SDK, test günlüklerinin IDE konsolunda üretmek için yapılandırılabilir.
Bu günlükler varsayılan olarak etkinleştirilmez. Bu özelleştirmek için özellik güncelleştirme `EngagementAgent.Instance.TestLogEnabled` bulunan değer birine `EngagementTestLogLevel` numaralandırma, örneğin:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
