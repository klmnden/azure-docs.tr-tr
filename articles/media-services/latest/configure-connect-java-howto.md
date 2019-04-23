---
title: Azure Media Services v3 API'sine - Java bağlanma
description: Java ile Media Services v3 API bağlanma hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2019
ms.author: juliako
ms.openlocfilehash: 68e09ec6ce4aeb91e00c2a15caa8ec81f40064c1
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59997198"
---
# <a name="connect-to-media-services-v3-api---java"></a>Media Services v3 API'sine - Java bağlanma

Bu makale için Azure Media Services v3 Java SDK'sına nasıl hizmet sorumlusu oturum açma yöntemiyle gösterir.

Bu makalede, Visual Studio Code, örnek uygulama geliştirmek için kullanılır.

## <a name="prerequisites"></a>Önkoşullar

- İzleyin [Visual Studio Code ile yazma Java](https://code.visualstudio.com/docs/java/java-tutorial) yüklemek için:

   - JDK
   - Apache Maven
   - Java uzantı paketi
- Ayarladığınızdan emin olun `JAVA_HOME` ve `PATH` ortam değişkenleri.
- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md). Kaynak grubu adı ve Media Services hesap adını hatırlamak emin olun.
- Bağlantısındaki [erişimi API'leri](access-api-cli-how-to.md) konu. Bir sonraki adımda, abonelik kimliği, uygulama kimliği (istemci kimliği), kimlik doğrulama anahtarı (gizli) ve gereken Kiracı kimliği kaydedin.

Ayrıca gözden geçirin:

- [Visual Studio code'da Java](https://code.visualstudio.com/docs/languages/java)
- [VS code'da Java proje yönetimi](https://code.visualstudio.com/docs/java/java-project)

## <a name="create-a-maven-project"></a>Bir Maven projesi oluşturun

Bir komut satırı aracı'nı açın ve `cd` , projeyi oluşturmak istediğiniz bir dizine.
    
```
mvn archetype:generate -DgroupId=com.azure.ams -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

Komutu çalıştırdığınızda `pom.xml`, `App.java`, ve diğer dosyalar oluşturulur. 

## <a name="add-dependencies"></a>Bağımlılıkları ekleyin

1. Visual Studio Code'da projenizi bulunduğu klasörü açın.
1. Bulma ve açma `pom.xml`
1. Gerekli bağımlılıkları Ekle

    ```xml
   <dependency>
     <groupId>com.microsoft.azure.mediaservices.v2018_07_01</groupId>
     <artifactId>azure-mgmt-media</artifactId>
     <version>1.0.0-beta-3</version>
   </dependency>
   <dependency>
     <groupId>com.microsoft.rest</groupId>
     <artifactId>client-runtime</artifactId>
     <version>1.6.6</version>
   </dependency>
   <dependency>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>azure-client-authentication</artifactId>
     <version>1.6.6</version>
   </dependency>
```

## <a name="connect-to-the-java-client"></a>Bağlanmak için Java istemcisi

1. Açık `App.java` altında dosya `src\main\java\com\azure\ams` ve paketinizin en üstünde eklenmiş olduğundan emin olun:

    ```java
    package com.azure.ams;
    ```
1. Paket bildirimi altında eklemeniz içeri aktarma deyimleri:
   
   ```java
   import com.microsoft.azure.AzureEnvironment;
   import com.microsoft.azure.credentials.ApplicationTokenCredentials;
   import com.microsoft.azure.management.mediaservices.v2018_07_01.implementation.MediaManager;
   import com.microsoft.rest.LogLevel;
   ```
1. İsteğinde bulunmak için gereken Active Directory kimlik bilgilerini oluşturmak için App sınıfının ana yönteme aşağıdaki kodu ekleyin ve aldığınız değerleri [erişimi API'leri](access-api-cli-how-to.md):
   
   ```java
   final String clientId = "00000000-0000-0000-0000-000000000000";
   final String tenantId = "00000000-0000-0000-0000-000000000000";
   final String clientSecret = "00000000-0000-0000-0000-000000000000";
   final String subscriptionId = "00000000-0000-0000-0000-000000000000";

   try {
      ApplicationTokenCredentials credentials = new ApplicationTokenCredentials(clientId, tenantId, clientSecret, AzureEnvironment.AZURE);
      credentials.withDefaultSubscriptionId(subscriptionId);

      MediaManager manager = MediaManager
              .configure()
              .withLogLevel(LogLevel.BODY_AND_HEADERS)
              .authenticate(credentials, credentials.defaultSubscriptionId());
      System.out.println("signed in");
   }
   catch (Exception e) {
      System.out.println("Exception encountered.");
      System.out.println(e.toString());
   }
   ```
1. Uygulamayı çalıştırın.

## <a name="see-also"></a>Ayrıca bkz.

- [Media Services kavramları](concepts-overview.md)
- [Java SDK](https://aka.ms/ams-v3-java-sdk)
- [Java başvurusu](https://aka.ms/ams-v3-java-ref)
- [com.microsoft.azure.mediaservices.v2018_07_01:azure-mgmt-media](https://search.maven.org/artifact/com.microsoft.azure.mediaservices.v2018_07_01/azure-mgmt-media/1.0.0-beta/jar)

## <a name="next-steps"></a>Sonraki adımlar

Artık içerebilir `import com.microsoft.azure.management.mediaservices.v2018_07_01.*;` ve varlıkları işleme başlayın.
