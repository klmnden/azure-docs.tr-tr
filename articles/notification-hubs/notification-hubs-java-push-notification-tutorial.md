---
title: Notification Hubs Java ile kullanma
description: Bir Java arka ucunu Azure bildirim hub'ları kullanmayı öğrenin.
services: notification-hubs
documentationcenter: ''
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 4c3f966d-0158-4a48-b949-9fa3666cb7e4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: java
ms.devlang: java
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 88e3ab3cc03cc1e760672120bc5c484af1ba4722
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-use-notification-hubs-from-java"></a>Java'dan Notification Hubs kullanma
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Bu konu, yeni tam olarak desteklenen resmi Azure Notification Hub Java SDK'sı anahtar özelliklerini açıklar. Bu proje bir açık kaynaklı proje mi ve tüm SDK koda görüntüleyebilirsiniz [Java SDK'sı]. 

Genel olarak, bir Java/PHP/Python/Ruby MSDN konusunda açıklandığı gibi bildirim hub'ı REST arabirimini kullanarak uç tüm bildirim hub'ları özellikleri erişebileceğiniz [bildirim hub'ları REST API'leri](http://msdn.microsoft.com/library/dn223264.aspx). Bu Java SDK'sı, Java bu REST arabirimleri üzerinden ince sarmalayıcı sağlar. 

SDK'sı şu anda destekler:

* Bildirim hub'ları üzerinde CRUD 
* CRUD kayıtlar hakkında
* Yükleme Yönetimi
* Kayıtlar içeri/dışarı aktarma
* Normal gönderir
* Zamanlanmış gönderir
* Java NIO üzerinden zaman uyumsuz işlemler
* Desteklenen platformlar: APNS (iOS), GCM (Android), WNS (Windows mağazası uygulamaları), MPNS (Windows Phone), ADM (Amazon Kindle yangın), Baidu (Android Google Hizmetleri olmadan) 

## <a name="sdk-usage"></a>SDK kullanımı
### <a name="compile-and-build"></a>Derleme ve oluşturma
Kullanım [Maven]

Oluşturmak için:

    mvn package

## <a name="code"></a>Kod
### <a name="notification-hub-cruds"></a>Bildirim hub'ı CRUDs
**Bir NamespaceManager oluşturun:**

    NamespaceManager namespaceManager = new NamespaceManager("connection string")

**Bildirim hub'ı oluşturun:**

    NotificationHubDescription hub = new NotificationHubDescription("hubname");
    hub.setWindowsCredential(new WindowsCredential("sid","key"));
    hub = namespaceManager.createNotificationHub(hub);

 OR

    hub = new NotificationHub("connection string", "hubname");

**Bildirim hub'ı alın:**

    hub = namespaceManager.getNotificationHub("hubname");

**Bildirim hub'ı güncelleştirin:**

    hub.setMpnsCredential(new MpnsCredential("mpnscert", "mpnskey"));
    hub = namespaceManager.updateNotificationHub(hub);

**Bildirim hub'ı silin:**

    namespaceManager.deleteNotificationHub("hubname");

### <a name="registration-cruds"></a>Kayıt CRUDs
**Bir bildirim hub'ı istemci oluşturun:**

    hub = new NotificationHub("connection string", "hubname");

**Windows kaydı oluşturun:**

    WindowsRegistration reg = new WindowsRegistration(new URI(CHANNELURI));
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");    
    hub.createRegistration(reg);

**İOS kaydı oluşturun:**

    AppleRegistration reg = new AppleRegistration(DEVICETOKEN);
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);

Benzer şekilde, Android (GCM), Windows Phone (MPNS) ve Kindle yangın (ADM) kayıtlar oluşturabilirsiniz.

**Şablon kayıtları oluşturun:**

    WindowsTemplateRegistration reg = new WindowsTemplateRegistration(new URI(CHANNELURI), WNSBODYTEMPLATE);
    reg.getHeaders().put("X-WNS-Type", "wns/toast");
    hub.createRegistration(reg);

**Kayıtlar oluşturmak kullanarak oluşturduğunuz kayıt kimliği + upsert düzeni**

Kayıp yanıtları nedeniyle yinelenen kayıt kimlikleri aygıtta depolanıyorsa kaldırır:

    String id = hub.createRegistrationId();
    WindowsRegistration reg = new WindowsRegistration(id, new URI(CHANNELURI));
    hub.upsertRegistration(reg);

**Kayıtlar güncelleştirin:**

    hub.updateRegistration(reg);

**Kayıtları silin:**

    hub.deleteRegistration(regid);

**Sorgu kayıtları:**

* **Tek kayıt alın:**
  
        hub.getRegistration(regid);

* **Tüm kayıtlar hub alın:**
  
        hub.getRegistrations();

* **Kayıtlar etiketiyle alın:**
  
        hub.getRegistrationsByTag("myTag");

* **Kayıtlar kanal tarafından alın:**
  
        hub.getRegistrationsByChannel("devicetoken");


Tüm koleksiyon sorgular, $top ve devamlılık belirteçleri destekler.

### <a name="installation-api-usage"></a>Yükleme API kullanımı
API kayıt yönetimi için alternatif bir mekanizma yüklemesidir. Önemsiz olmayan ve kolayca yanlış veya inefficiently yapılabilir, birden çok kayıtlar koruma yerine artık tek bir yükleme nesnesini kullanmak mümkündür. Yükleme ihtiyaç duyduğunuz her şeyi içerir: itme kanal (cihaz belirteci), etiketler, şablonlar, ikincil döşeme (WNS ve APNS için). Kimliği artık get - yalnızca GUID veya başka bir tanımlayıcı oluşturmak, cihazda tutmak ve anında iletme kanal (cihaz belirteci) ile birlikte arka göndermek için hizmetini çağırmak gerekmez. Arka uçta yalnızca tek bir çağrı yapmanız gerekir: CreateOrUpdateInstallation, onu tam olarak ıdempotent, böylece gerektiğinde yeniden deneme çekinmeyin.

Amazon Kindle yangın için örnek:

    Installation installation = new Installation("installation-id", NotificationPlatform.Adm, "adm-push-channel");
    hub.createOrUpdateInstallation(installation);

Güncelleştirmek istiyorsanız: 

    installation.addTag("foo");
    installation.addTemplate("template1", new InstallationTemplate("{\"data\":{\"key1\":\"$(value1)\"}}","tag-for-template1"));
    installation.addTemplate("template2", new InstallationTemplate("{\"data\":{\"key2\":\"$(value2)\"}}","tag-for-template2"));
    hub.createOrUpdateInstallation(installation);

Gelişmiş senaryolar için yükleme nesnesi yalnızca belirli özelliklerini değiştirmek için sağlar kısmi güncelleştirme özelliğini kullanın. Kısmi güncelleştirme yükleme nesnesi karşı çalıştırabilirsiniz JSON düzeltme işlemleri alt kümesidir.

    PartialUpdateOperation addChannel = new PartialUpdateOperation(UpdateOperationType.Add, "/pushChannel", "adm-push-channel2");
    PartialUpdateOperation addTag = new PartialUpdateOperation(UpdateOperationType.Add, "/tags", "bar");
    PartialUpdateOperation replaceTemplate = new PartialUpdateOperation(UpdateOperationType.Replace, "/templates/template1", new InstallationTemplate("{\"data\":{\"key3\":\"$(value3)\"}}","tag-for-template1")).toJson());
    hub.patchInstallation("installation-id", addChannel, addTag, replaceTemplate);

Yükleme Sil:

    hub.deleteInstallation(installation.getInstallationId());

CreateOrUpdate, düzeltme eki ve Delete sonunda Get ile tutarlı değil. İstenen işlem yalnızca çağrı sırasında sistem kuyruğa gider ve arka planda yürütülür. Get ana çalışma zamanı senaryosu için ancak yalnızca hata ayıklama için tasarlanmamıştır ve sorun giderme amacıyla, bu sıkı bir şekilde hizmet tarafından kısıtlanıyor.

Yüklemeleri için gönderme akışı kayıtlar ile aynıdır. Bildirim belirli yüklemeyi hedef - yalnızca etiket kullanmak için "InstallationID: {desired-id}". Bu durumda, kodu verilmiştir:

    Notification n = Notification.createWindowsNotification("WNS body");
    hub.sendNotification(n, "InstallationId:{installation-id}");

Biri çeşitli şablonlar için:

    Map<String, String> prop =  new HashMap<String, String>();
    prop.put("value3", "some value");
    Notification n = Notification.createTemplateNotification(prop);
    hub.sendNotification(n, "InstallationId:{installation-id} && tag-for-template1");

### <a name="schedule-notifications-available-for-standard-tier"></a>Bildirimleri (standart katmanı için kullanılabilir) zamanlama
Aynı normal gönderme ancak bir ek parametre - bildirim ne zaman gönderilmesi gereken diyor scheduledTime. Hizmet şimdi + 5 dakika arasında geçen süreyi herhangi bir noktasını kabul eder ve şimdi + 7 gün.

**Windows yerel bildirim zamanla:**

    Calendar c = Calendar.getInstance();
    c.add(Calendar.DATE, 1);    
    Notification n = Notification.createWindowsNotification("WNS body");
    hub.scheduleNotification(n, c.getTime());

### <a name="importexport-available-for-standard-tier"></a>İçeri/dışarı aktarma (standart katmanı için kullanılabilir)
Bazen kayıtlar karşı toplu işlemi gerçekleştirmek için gereklidir. Genellikle başka bir sistemi ile tümleştirme için veya yalnızca yoğun düzeltme söylemek için etiketler güncelleştirin. Kayıtlar binlerce söz konusuysa Get/güncelleştirme akış kullanmak için önerilmez. İçeri/dışarı aktarma yeteneği senaryo karşılamak üzere tasarlanmıştır. Temel olarak, bir erişim bazı blob kapsayıcısı için depolama hesabınızın altında bir gelen veri kaynağı ve çıkış konumu olarak sağlar.

**Dışarı aktarma işini gönder:**

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ExportRegistrations);
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);


**İçe aktarma işi gönder:**

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ImportCreateRegistrations);
    job.setImportFileUri("input file uri with SAS signature");
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);

**İş tamamlanana kadar bekleyin:**

    while(true){
        Thread.sleep(1000);
        job = hub.getNotificationHubJob(job.getJobId());
        if(job.getJobStatus() == NotificationHubJobStatus.Completed)
            break;
    }       

**Tüm işleri görüntüleyin:**

    List<NotificationHubJob> jobs = hub.getAllNotificationHubJobs();

**SAS imzayla URI:** bu URL'dir bazı blob dosya veya blob kapsayıcı URL'si artı izinleri ve süre sonu gibi parametreleri kümesini artı hesabın SAS anahtarı kullanılarak yapılan tüm bu işlemler imzası. Azure depolama Java SDK'sı, bu tür bir URI oluşturma dahil olmak üzere Zengin özellikleri vardır. Basit alternatif olarak, imza algoritması, temel ve compact uygulaması olan ImportExportE2E test sınıfı (github konumdan) göz atın.

### <a name="send-notifications"></a>Bildirimleri gönderme
Bildirim nesnesi yalnızca üstbilgileri gövde, bazı yardımcı program yöntemleri yerel ve şablon bildirimleri nesneleri oluşturmaya yardımcı olur.

* **Windows mağazası ve Windows Phone 8.1 (Silverlight olmayan)**
  
        String toast = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello from Java!</text></binding></visual></toast>";
        Notification n = Notification.createWindowsNotification(toast);
        hub.sendNotification(n);
* **iOS**
  
        String alert = "{\"aps\":{\"alert\":\"Hello from Java!\"}}";
        Notification n = Notification.createAppleNotification(alert);
        hub.sendNotification(n);
* **Android**
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createGcmNotification(message);
        hub.sendNotification(n);
* **Windows Phone 8.0 ve 8.1 Silverlight**
  
        String toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>Hello from Java!</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
        Notification n = Notification.createMpnsNotification(toast);
        hub.sendNotification(n);
* **Yangın kindle**
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createAdmNotification(message);
        hub.sendNotification(n);
* **Etiketleri Gönder**
  
        Set<String> tags = new HashSet<String>();
        tags.add("boo");
        tags.add("foo");
        hub.sendNotification(n, tags);
* **Etiket ifadesi Gönder**       
  
        hub.sendNotification(n, "foo && ! bar");
* **Şablon bildirim gönder**
  
        Map<String, String> prop =  new HashMap<String, String>();
        prop.put("prop1", "v1");
        prop.put("prop2", "v2");
        Notification n = Notification.createTemplateNotification(prop);
        hub.sendNotification(n);

Java Kodunuzu çalıştırmaya şimdi, hedef cihazda görünen bir bildirim üretir.

## <a name="next-steps"></a>Sonraki adımlar
Bu konuda, basit bir Java REST istemci bildirim hub'ları için nasıl oluşturulacağını gösterir. Buradan şunları yapabilirsiniz:

* Tam karşıdan [Java SDK'sı], tüm SDK kodunu içerir. 
* Örneklerle Yürüt:
  * [Notification Hubs ile çalışmaya başlama]
  * [Son dakika haberleri göndermek]
  * [Yerelleştirilmiş son dakika haberleri göndermek]
  * [Kimliği doğrulanmış kullanıcılara bildirim göndermek]
  * [Kimliği doğrulanmış kullanıcılara platformlar arası bildirimleri gönderin]

[Java SDK'sı]: https://github.com/Azure/azure-notificationhubs-java-backend
[Get started tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Notification Hubs ile çalışmaya başlama]: http://www.windowsazure.com/manage/services/notification-hubs/getting-started-windows-dotnet/
[Son dakika haberleri göndermek]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-dotnet/
[Yerelleştirilmiş son dakika haberleri göndermek]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-localized-dotnet/
[Kimliği doğrulanmış kullanıcılara bildirim göndermek]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users/
[Kimliği doğrulanmış kullanıcılara platformlar arası bildirimleri gönderin]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users-xplat-mobile-services/
[Maven]: http://maven.apache.org/

