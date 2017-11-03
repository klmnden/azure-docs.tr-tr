---
title: API Engagement Windows Evrensel kullanma
description: API Engagement Windows Evrensel kullanma
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: bb501fca-9cfe-4495-81df-b5efd6e0137b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 75fc134a5535e6113331470cf61df9c06eb8e2ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-engagement-api-on-windows-universal"></a>API Engagement Windows Evrensel kullanma
Bu belge belgeye bir eklentidir [tümleştirmek Engagement Windows Evrensel üzerinde nasıl](mobile-engagement-windows-store-integrate-engagement.md): katılım API uygulama istatistikleri rapor için nasıl kullanılacağı hakkında derinliği ayrıntıları sağlar.

Uygulamanızın oturumları, etkinlikleri, kilitlenme ve teknik bilgileri raporlamak için katılım yalnızca istiyorsanız, sonra en basit yolu tüm olduğunu aklınızda bulundurun, `Page` alt sınıfları `EngagementPage` sınıfı.

Daha fazla bilgi için uygulama belirli olaylar, hatalar ve işleri, rapor gerekiyorsa örnek yapmak istiyorsanız veya uygulamanızın etkinlikleri uygulanan bir daha farklı bir şekilde bildirmek varsa `EngagementPage` sınıfları yeniden katılım API'sini kullanmanız gerekiyor.

Katılım API'si tarafından sağlanan `EngagementAgent` sınıfı. Bu yöntemlerle erişebilirsiniz `EngagementAgent.Instance`.

Aracı modülü başlatılmadı olsa bile, her API çağrısı ertelenir ve aracı kullanılabilir olduğunda yeniden yürütülür.

## <a name="engagement-concepts"></a>Engagement kavramları
Aşağıdaki bölümleri yaygın İyileştir [Mobile Engagement kavramları](mobile-engagement-concepts.md) Evrensel Windows platformu için.

### <a name="session-and-activity"></a>`Session` ve `Activity`
Bir *etkinlik* yani genellikle uygulama bir sayfayla ilişkilendirilen *etkinlik* sayfası görüntülenir ve sayfa kapatıldığında durdurduğunda başlatır: Engagement SDK'sını kullanarak tümleştirildiğinde bu durumda `EngagementPage` sınıfı.

Ancak *etkinlikleri* de el ile katılım API'si kullanılarak denetlenebilir. Bu, belirli bir sayfa (örneğin ne sıklıkta ve ne kadar süreyle iletişim kutuları bu sayfa içinde kullanılan bilmeniz) bu sayfanın kullanımı hakkında daha fazla bilgi almak için birkaç alt bölümlerinde bölmek sağlar.

## <a name="reporting-activities"></a>Raporlama etkinlikleri
### <a name="user-starts-a-new-activity"></a>Kullanıcı yeni bir etkinlik başlatır
#### <a name="reference"></a>Başvuru
            void StartActivity(string name, Dictionary<object, object> extras = null)

Çağırmanız gerekir `StartActivity()` kullanıcı etkinliği değişiklikleri her zaman. Bu işlev ilk çağrıda yeni bir kullanıcı oturumu başlatır.

> [!IMPORTANT]
> Uygulama kapatıldığında SDK'yı otomatik olarak EndActivity yöntemini çağırır. Bu nedenle, kullanıcı değişiklikleri ve hiçbir zaman etkinliği çağrı her EndActivity yöntemi bu yöntemi çağırmadan sonlandırılmak üzere geçerli oturumdaki zorlar beri StartActivity yöntemini çağırmak için önerilir.
> 
> 

#### <a name="example"></a>Örnek
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Kullanıcı kendi geçerli etkinliği sona erer
#### <a name="reference"></a>Başvuru
            void EndActivity()

Bu etkinliği ve oturum sona erer. Gerçekleştirmekte olduğunuz gerçekten bilmiyorsanız bu yöntem çağırmalıdır değil.

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
            void SendCrash(Exception e)

Katılım ayrıca işlenmeyen özel durumlar varsa göndermek üzere bir yöntem sağlar **devre dışı** katılım otomatik **kilitlenme** raporlama. Uygulama UnhandledException olay işleyicisinin içinden kullanıldığında bu özellikle yararlıdır.

Bu yöntem olacak **her zaman** adlı sonra işler ve katılım oturum sonlandırılacak.

#### <a name="example"></a>Örnek
Kendi UnhandledExceptionEventArgs işleyici uygulamak için kullanabilirsiniz. Örneğin, ekleyin `Current_UnhandledException` yöntemi `App.xaml.cs` dosyası:

            // In your App.xaml.cs file

            // Code to execute on Unhandled Exceptions
            void Current_UnhandledException(object sender, UnhandledExceptionEventArgs e)
            {
               EngagementAgent.Instance.SendCrash(e.Exception,false);
            }

App.xaml.cs dosyasında "Ortak App() {}" ekleyin:

            Application.Current.UnhandledException += Current_UnhandledException;

## <a name="device-id"></a>Cihaz kimliği
            String EngagementAgent.Instance.GetDeviceId()

Bu yöntemi çağrılarak engagement cihaz kimliğini elde edebilirsiniz.

## <a name="extras-parameters"></a>Ek özellikler parametreleri
Rastgele veriler, bir olay, bir hata, bir etkinlik veya bir iş için eklenebilir. Bu verilerin bir sözlüğünü kullanarak yapılandırılmış. Anahtarları ve değerleri herhangi bir türde olabilir.

Bu tür için bir veri sözleşmesi eklemek zorunda ek özellikler kendi türü eklemek istiyorsanız şekilde ek özellikler veri serileştirilir.

### <a name="example"></a>Örnek
Yeni bir sınıf "Kişi" oluşturuyoruz.

            using System.Runtime.Serialization;

            namespace Microsoft.Azure.Engagement
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

Bu verileri artımlı olarak gönderilebilir Not: belirli bir aygıt için belirli bir anahtar için yalnızca en son değeri korunur. Olay ek özellikler gibi sözlük kullanma\<nesne, nesne\> veri eklemek için.

### <a name="example"></a>Örnek
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
              {
                {"birthdate", "1983-12-07"},
                {"gender", "female"}
              };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Sınırlar
#### <a name="keys"></a>Anahtarlar
Nesne tablosundaki her anahtarın şu normal ifadeyle aynı olması gerekir.

`^[a-zA-Z][a-zA-Z_0-9]*$`

Anahtarları harfler, sayılar veya alt çizgi izlemelidir en az bir harf ile başlamalıdır anlamına gelir (\_).

#### <a name="size"></a>Boyut
Uygulama bilgilerini sınırlıdır **1024** çağrı başına karakter.

Önceki örnekte, sunucuya gönderilen JSON 44 karakter olacak:

            {"birthdate":"1983-12-07","gender":"female"}

## <a name="logging"></a>Günlüğe kaydetme
### <a name="enable-logging"></a>Günlük kaydını etkinleştir
SDK, test günlüklerinin IDE konsolunda üretmek için yapılandırılabilir.
Bu günlükler varsayılan olarak etkinleştirilmez. Bu özelleştirmek için özellik güncelleştirme `EngagementAgent.Instance.TestLogEnabled` bulunan değer birine `EngagementTestLogLevel` numaralandırma, örneğin:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

