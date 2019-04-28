---
title: Notification hubs'ı Java ile kullanma
description: Bir Java arka ucunuzdan Azure Notification hubs'ı kullanmayı öğrenin.
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 4c3f966d-0158-4a48-b949-9fa3666cb7e4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: java
ms.devlang: java
ms.topic: article
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: 993eafd5a0b74be706d13fe8e06483c223f81eb6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61461213"
---
# <a name="how-to-use-notification-hubs-from-java"></a>Java'dan Notification Hubs'ı kullanma

[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Bu konu, yeni tam olarak desteklenen resmi Azure Notification Hub Java SDK'sı anahtar özelliklerini açıklar.
Bu proje bir açık kaynak projesidir ve tüm SDK kodu görüntüleyebilirsiniz [Java SDK].

Genel olarak, tüm Notification Hubs özellikleri bir Java/PHP/Python/Ruby MSDN konu başlığı altında açıklandığı gibi bildirim hub'ı REST arabirimini kullanarak uç erişebileceğiniz [Notification Hubs REST API'leri](https://msdn.microsoft.com/library/dn223264.aspx). Bu Java SDK'sı, Java bu REST arabirimleri üzerinde ince bir sarmalayıcı sağlar.

SDK'sı şu anda destekler:

* Bildirim hub'ları üzerinde CRUD
* CRUD kayıtları hakkında
* Yükleme Yönetimi
* Kayıtları içeri/dışarı aktarma
* Normal gönderir
* Zamanlanmış gönderir
* Java NIO aracılığıyla zaman uyumsuz işlemler
* Desteklenen platformlar: APNS (iOS), FCM (Android), WNS (Windows Store uygulamaları), MPNS (Windows Phone), ADM (Amazon Kindle Fire), Baidu (Android Google Hizmetleri olmadan)

## <a name="sdk-usage"></a>SDK kullanımı

### <a name="compile-and-build"></a>Derleme ve oluşturma

Kullanım [Maven]

Derlemek için:

    mvn package

## <a name="code"></a>Kod

### <a name="notification-hub-cruds"></a>Bildirim hub'ı CRUDs

**Bir NamespaceManager oluşturun:**

    ```java
    NamespaceManager namespaceManager = new NamespaceManager("connection string")
    ```

**Bildirim hub'ı oluşturun:**

    ```java
    NotificationHubDescription hub = new NotificationHubDescription("hubname");
    hub.setWindowsCredential(new WindowsCredential("sid","key"));
    hub = namespaceManager.createNotificationHub(hub);
    ```

 OR

    ```java
    hub = new NotificationHub("connection string", "hubname");
    ```

**Bildirim hub'ı alın:**

    ```java
    hub = namespaceManager.getNotificationHub("hubname");
    ```

**Bildirim hub'ı güncelleştirin:**

    ```java
    hub.setMpnsCredential(new MpnsCredential("mpnscert", "mpnskey"));
    hub = namespaceManager.updateNotificationHub(hub);
    ```

**Bildirim hub'ı silin:**

    ```java
    namespaceManager.deleteNotificationHub("hubname");
    ```

### <a name="registration-cruds"></a>Kayıt CRUDs

**Bildirim hub'ı istemci oluşturun:**

    ```java
    hub = new NotificationHub("connection string", "hubname");
    ```

**Windows kaydı oluşturun:**

    ```java
    WindowsRegistration reg = new WindowsRegistration(new URI(CHANNELURI));
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);
    ```

**İOS kaydı oluşturun:**

    ```java
    AppleRegistration reg = new AppleRegistration(DEVICETOKEN);
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);
    ```

Benzer şekilde kayıtları Android'e (FCM), Windows Phone (MPNS) ve Kindle Fire (ADM) oluşturabilirsiniz.

**Şablon kayıtları oluşturun:**

    ```java
    WindowsTemplateRegistration reg = new WindowsTemplateRegistration(new URI(CHANNELURI), WNSBODYTEMPLATE);
    reg.getHeaders().put("X-WNS-Type", "wns/toast");
    hub.createRegistration(reg);
    ```

**Kayıtları oluşturma kullanarak oluşturduğunuz kayıt kimliği + upsert Desen:**

Cihazda kayıt kimliklerini depolamak kayıp yanıtları nedeniyle yinelemeleri kaldırır:

    ```java
    String id = hub.createRegistrationId();
    WindowsRegistration reg = new WindowsRegistration(id, new URI(CHANNELURI));
    hub.upsertRegistration(reg);
    ```

**Güncelleştirme kayıtları:**

    ```java
    hub.updateRegistration(reg);
    ```

**Kayıtları silin:**

    ```java
    hub.deleteRegistration(regid);
    ```

**Sorgu kayıtları:**

* **Tek bir kayıt alın:**

    ```java
    hub.getRegistration(regid);
    ```

* **Hub'ında tüm kayıtları alın:**

    ```java
    hub.getRegistrations();
    ```

* **Etiket ile kayıtları alın:**

    ```java
    hub.getRegistrationsByTag("myTag");
    ```

* **Kanal tarafından kayıtları alın:**

    ```java
    hub.getRegistrationsByChannel("devicetoken");
    ```

Tüm koleksiyon sorguları, $top ve devamlılık belirteçleri destekler.

### <a name="installation-api-usage"></a>Yükleme API kullanımı

API kayıt yönetimi için alternatif bir mekanizma yüklemedir. Önemsiz olmayan ve yanlış veya verimsiz kolayca yapılabilir, birden çok kayıtları Bakımı yerine artık bir yükleme tek nesnesini kullanmak mümkündür.

Yükleme ihtiyaç duyduğunuz her şeyi içerir: anında iletme kanal (cihaz belirteci), etiketler, şablonlar, ikincil kutucuk (WNS ve APNS). Hizmet kimliği artık Al - yalnızca GUID veya diğer herhangi bir tanımlayıcı oluşturur, cihazda tutun ve arka ucunuzdan anında iletme kanal (cihaz belirteci) ile birlikte göndermek için arama gerek yoktur.

Arka uçta tek bir çağrı yalnızca yapmalısınız `CreateOrUpdateInstallation`; tam olarak bir kez etkili olduğundan, bu nedenle gerekirse yeniden çekinmeyin.

Amazon Kindle Fire örneği:

    ```java
    Installation installation = new Installation("installation-id", NotificationPlatform.Adm, "adm-push-channel");
    hub.createOrUpdateInstallation(installation);
    ```

Güncelleştirmek istiyorsanız:

    ```java
    installation.addTag("foo");
    installation.addTemplate("template1", new InstallationTemplate("{\"data\":{\"key1\":\"$(value1)\"}}","tag-for-template1"));
    installation.addTemplate("template2", new InstallationTemplate("{\"data\":{\"key2\":\"$(value2)\"}}","tag-for-template2"));
    hub.createOrUpdateInstallation(installation);
    ```

Gelişmiş senaryolar için yükleme nesnesi yalnızca belirli özelliklerini değiştirmenizi sağlayan kısmi güncelleştirme özelliğini kullanın. Kısmi güncelleştirmeyi yükleme nesneye karşı çalıştırabilirsiniz JSON Patch işlemleri alt kümesidir.

    ```java
    PartialUpdateOperation addChannel = new PartialUpdateOperation(UpdateOperationType.Add, "/pushChannel", "adm-push-channel2");
    PartialUpdateOperation addTag = new PartialUpdateOperation(UpdateOperationType.Add, "/tags", "bar");
    PartialUpdateOperation replaceTemplate = new PartialUpdateOperation(UpdateOperationType.Replace, "/templates/template1", new InstallationTemplate("{\"data\":{\"key3\":\"$(value3)\"}}","tag-for-template1")).toJson());
    hub.patchInstallation("installation-id", addChannel, addTag, replaceTemplate);
    ```

Yükleme silin:

    ```java
    hub.deleteInstallation(installation.getInstallationId());
    ```

`CreateOrUpdate`, `Patch`, ve `Delete` birlikte sonunda tutarlı `Get`. İstenen işlemi yalnızca çağrı sırasında sistem kuyruğa gider ve arka planda çalıştırılır. Get ana çalışma zamanı senaryosu için ancak yalnızca hata ayıklama için tasarlanmamıştır ve sorun giderme amacıyla, sıkı bir şekilde hizmet tarafından kısıtlanır.

Gönderme akışı yüklemeleri için kayıtları ile aynıdır. Bildirim belirli yüklemeyi hedef-yalnızca etiketini kullanın "InstallationID: {desired-kimliği}". Bu durumda, kodu verilmiştir:

    ```java
    Notification n = Notification.createWindowsNotification("WNS body");
    hub.sendNotification(n, "InstallationId:{installation-id}");
    ```

Çeşitli şablonlardan biri için:

    ```java
    Map<String, String> prop =  new HashMap<String, String>();
    prop.put("value3", "some value");
    Notification n = Notification.createTemplateNotification(prop);
    hub.sendNotification(n, "InstallationId:{installation-id} && tag-for-template1");
    ```

### <a name="schedule-notifications-available-for-standard-tier"></a>(Standart katmanında kullanılabilir) bildirimleri zamanlayın

Aynı normal gönderme ancak bir ek parametre - bildirim teslim zaman gerektirdiğinizde scheduledTime. Service now + 5 dakika arasındaki zaman herhangi bir noktasını kabul eder ve şimdi + 7 gün.

**Windows yerel bildirimi zamanla:**

    ```java
    Calendar c = Calendar.getInstance();
    c.add(Calendar.DATE, 1);
    Notification n = Notification.createWindowsNotification("WNS body");
    hub.scheduleNotification(n, c.getTime());
    ```

### <a name="importexport-available-for-standard-tier"></a>İçeri/dışarı aktarma (standart katmanında kullanılabilir)

Kayıtları toplu işlemi gerçekleştirmeniz gerekebilir. Genellikle başka bir sistem veya etiketleri güncelleştirmek için devasa bir düzeltme ile tümleştirme içindir. Binlerce kaydı söz konusuysa alma/güncelleştirme flow kullanımı önerilmemektedir. Sistemin içeri/dışarı aktarma özelliği, senaryo kapsayacak şekilde tasarlanmıştır. Gelen veri ve çıkış için konum kaynağı olarak depolama hesabınız kapsamında bir blob kapsayıcıya erişimi sağlarız.

**Dışarı aktarma işi gönder:**

    ```java
    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ExportRegistrations);
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);
    ```

**İçeri aktarma işi gönder:**

    ```java
    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ImportCreateRegistrations);
    job.setImportFileUri("input file uri with SAS signature");
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);
    ```

**Bir iş tamamlanana kadar bekleyin:**

    ```java
    while(true){
        Thread.sleep(1000);
        job = hub.getNotificationHubJob(job.getJobId());
        if(job.getJobStatus() == NotificationHubJobStatus.Completed)
            break;
    }
    ```

**Tüm işleri Al:**

    ```java
    List<NotificationHubJob> jobs = hub.getAllNotificationHubJobs();
    ```

**URI'sı ile SAS imzası:**

 Bu, bir blob dosyası veya blob kapsayıcısı URL'si artı yanı sıra hesabın SAS anahtarı kullanılarak yapılan tüm bu işlemler imzası izinleri ve süre sonu gibi parametreleri kümesini URL'dir. Azure depolama Java SDK'sı, bu bir URI'leri oluşturulmasını da dahil olmak üzere Zengin özellikleri vardır. Basit bir alternatif göz atın `ImportExportE2E` test imza algoritması, temel ve Sıkıştır uygulamasına sahip sınıfından (GitHub konum).

### <a name="send-notifications"></a>Bildirimleri Gönder

Bildirim nesnesi bir gövde sadece üst bilgilerini, bazı yardımcı program yöntemleri yerel ve şablon bildirimleri nesnelerini oluşturmaya yardımcı olur.

* **Windows Store ve Windows Phone 8.1 (Silverlight olmayan)**

    ```java
    String toast = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello from Java!</text></binding></visual></toast>";
    Notification n = Notification.createWindowsNotification(toast);
    hub.sendNotification(n);
    ```

* **iOS**

    ```java
    String alert = "{\"aps\":{\"alert\":\"Hello from Java!\"}}";
    Notification n = Notification.createAppleNotification(alert);
    hub.sendNotification(n);
    ```

* **Android**

    ```java
    String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
    Notification n = Notification.createFcmNotification(message);
    hub.sendNotification(n);
    ```

* **Windows Phone 8.0 ve 8.1 Silverlight**

    ```java
    String toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                    "<wp:Toast>" +
                        "<wp:Text1>Hello from Java!</wp:Text1>" +
                    "</wp:Toast> " +
                "</wp:Notification>";
    Notification n = Notification.createMpnsNotification(toast);
    hub.sendNotification(n);
    ```

* **Kindle Fire**

    ```java
    String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
    Notification n = Notification.createAdmNotification(message);
    hub.sendNotification(n);
    ```

* **Etiketleri Gönder**
  
    ```java
    Set<String> tags = new HashSet<String>();
    tags.add("boo");
    tags.add("foo");
    hub.sendNotification(n, tags);
    ```

* **Etiket ifadesine Gönder**

    ```java
    hub.sendNotification(n, "foo && ! bar");
    ```

* **Şablon bildirimi gönder**

    ```java
    Map<String, String> prop =  new HashMap<String, String>();
    prop.put("prop1", "v1");
    prop.put("prop2", "v2");
    Notification n = Notification.createTemplateNotification(prop);
    hub.sendNotification(n);
    ```

Java Kodunuzu çalıştıran artık hedef Cihazınızda görünen bir bildirim üretir.

## <a name="next-steps"></a>Sonraki Adımlar

Bu konuda, basit bir Java REST istemci bildirim hub'ları için nasıl oluşturulacağını gösterir. Buradan şunları yapabilirsiniz:

* Tam indirme [Java SDK], tüm SDK kodunu içerir.
* Örneklerle Yürüt:
  * [Notification Hubs ile çalışmaya başlama]
  * [Son dakika haberleri göndermek]
  * [Yerelleştirilmiş son dakika haberleri göndermek]
  * [Kimliği doğrulanmış kullanıcılara bildirimler gönderme]
  * [Kimliği doğrulanmış kullanıcılara çapraz platform bildirimleri gönder]

[Java SDK]: https://github.com/Azure/azure-notificationhubs-java-backend
[Get started tutorial]: notification-hubs-ios-apple-push-notification-apns-get-started.md
[Notification Hubs ile çalışmaya başlama]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Son dakika haberleri göndermek]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Yerelleştirilmiş son dakika haberleri göndermek]: notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md
[Kimliği doğrulanmış kullanıcılara bildirimler gönderme]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Kimliği doğrulanmış kullanıcılara çapraz platform bildirimleri gönder]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Maven]: https://maven.apache.org/
