---
title: "Redis önbelleği kullanma yay önyükleme Başlatıcı uygulamasını yapılandırma"
description: "Azure Redis önbelleği kullanma yay Initializr ile oluşturulmuş bir yay önyükleme uygulaması yapılandırmayı öğrenin."
services: redis-cache
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
keywords: "İlkbahar, yay önyükleme Starter, Redis önbelleği"
ms.assetid: 
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: java
ms.topic: article
ms.date: 08/31/2017
ms.author: robmcm;zhijzhao;yidon
ms.openlocfilehash: 7a6ec549654d00975494bac8594a6777af5ec415
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-a-spring-boot-initializer-app-to-use-redis-cache"></a>Redis önbelleği kullanma yay önyükleme Başlatıcı uygulamasını yapılandırma

## <a name="overview"></a>Genel Bakış

**[Yay Framework]**  Java geliştiriciler kuruluş düzeyinde uygulamalar oluşturmanıza yardımcı olan bir açık kaynaklı bir çözümdür. Yerleşik daha popüler projelerden biri üzerinde üst o platformudur [yay önyükleme], tek başına Java uygulamaları oluşturmak için basitleştirilmiş bir yaklaşım sağlar. Yay önyükleme ile çalışmaya başlama geliştiricilerin yardımcı olmak için birkaç örnek yay önyükleme paketleri kullanılabilir <https://github.com/spring-guides/>. Temel yay önyükleme projeleri, listeden seçerek ek olarak  **[yay Initializr]**  özel yay önyükleme uygulamalar oluşturmaya başlamak geliştiricilere yardımcı olur.

Bu makalede sonra kullanarak Azure portalını kullanarak Redis önbelleği oluşturma ile anlatılmaktadır **yay Initializr** özel bir uygulama ve depolar ve alır, Redis kullanarak verileri bir Java web uygulaması oluşturma oluşturmak için önbelleği.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları için aşağıdaki önkoşullar gereklidir:

* Bir Azure aboneliği; bir Azure aboneliği zaten sahip değilseniz, etkinleştirebilir, [MSDN abone Avantajlarınızı] veya kaydolun bir [ücretsiz Azure hesabı].

* A [Java Geliştirme Seti (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), 1,7 veya sonraki bir sürümü.

* [Apache Maven](http://maven.apache.org/), sürüm 3.0 veya üstü.

## <a name="create-a-redis-cache-on-azure"></a>Azure’da Redis Cache oluşturma

1. Azure portalında göz <https://portal.azure.com/> öğe için tıklatıp **+ yeni**.

   ![Azure portalına][AZ01]

1. Tıklatın **veritabanı**ve ardından **Redis önbelleği**.

   ![Azure portalına][AZ02]

1. Üzerinde **yeni Redis önbelleği** sayfasında, aşağıdaki bilgileri belirtin:

   * Girin **DNS adı** önbelleğiniz için.
   * Belirtin, **abonelik**, **kaynak grubu**, **konumu**, ve **fiyatlandırma katmanı**.
   * Bu öğretici için seçin **6379 bağlantı noktasının engelini kaldırmak**.

   > [!NOTE]
   >
   > SSL ile Redis önbellekleri kullanabilirsiniz, ancak Jedis gibi farklı bir Redis istemcinin kullanmanız gerekir. Daha fazla bilgi için bkz: [Java ile Azure Redis önbelleği kullanmayı][Redis Cache with Java].
   >

   Bu seçenek belirtildiğinde tıklatın **oluşturma** önbelleğiniz oluşturmak için.

   ![Azure portalına][AZ03]

1. Önbelleğinizi tamamlandığında, Azure üzerinde listelendiğini göreceksiniz **Pano**altında da olarak **tüm kaynakları**, ve **Redis önbellekleri** sayfaları. Önbelleğinizi önbelleğiniz için Özellikler sayfasını açmak için bu konumların hiçbirinde tıklatabilirsiniz.

   ![Azure portalına][AZ04]

1. Önbelleğinizi görüntülenen için özellikler listesini içeren sayfasını tıklattığınızda **erişim anahtarları** ve önbelleğiniz için erişim tuşlarınızı kopyalayın.

   ![Azure portalına][AZ05]

## <a name="create-a-custom-application-using-the-spring-initializr"></a>Yay Initializr kullanarak özel bir uygulama oluşturma

1. Gözat <https://start.spring.io/>.

1. Oluşturmak istediğiniz belirtin bir **Maven** ile proje **Java**, girin **grup** ve **Aritifact** , uygulamanız için adları ve bağlantısını tıklatın **geçiş tam sürüme** yay Initializr biri.

   ![Basic yay Initializr seçenekleri][SI01]

   > [!NOTE]
   >
   > Yay Initializr kullanacağı **grup** ve **Aritifact** paket adı; oluşturmak için adlarını, örneğin: *com.contoso.myazuredemo*.
   >

1. Aşağı kaydırın **Web** bölümünde ve için kutuyu **Web**, ardından aşağı kaydırın **NoSQL** bölümünde ve için kutuyu **Redis**, sonra sayfanın alt kısmına kaydırın ve düğmesine tıklayın **proje oluştur**.

   ![Tam yay Initializr seçenekleri][SI02]

1. İstendiğinde, yerel bilgisayarınızda bir yola projenizi indirin.

   ![Özel yay önyükleme projenizi indirin][SI03]

1. Yerel sisteminizde dosyaları ayıkladıktan sonra özel yay önyükleme uygulamanız düzenleme için hazır olacaktır.

   ![Özel yay önyükleme proje dosyaları][SI04]

## <a name="configure-your-custom-spring-boot-to-use-your-redis-cache"></a>Redis önbelleği kullanma özel yay önyükleme yapılandırma

1. Bulun *application.properties* dosyasını *kaynakları* , uygulamanızın dizin veya zaten yoksa, dosyayı oluşturun.

   ![Application.properties dosyasını bulun][RE01]

1. Açık *application.properties* önbelleğiniz uygun özelliklerinden örnek değerler yerine dosya bir metin düzenleyicisinde ve dosyasına aşağıdaki satırları ekleyin:

   ```yaml
   # Specify the DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify the port for your Redis cache.
   spring.redis.port=6379

   # Specify the access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![Application.properties dosya düzenleme][RE02]

   > [!NOTE]
   >
   > SSL sağlar Jedis gibi farklı bir Redis istemcinin kullanıyorsanız, bağlantı noktası 6380 belirtirsiniz, *application.properties* dosya. Daha fazla bilgi için bkz: [Java ile Azure Redis önbelleği kullanmayı][Redis Cache with Java].
   >

1. Kaydet ve Kapat *application.properties* dosya.

1. Adlı bir klasör oluşturun *denetleyicisi* paketinizi; kaynak klasörü altında örneğin:

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   -veya-

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. Adlı yeni bir dosya oluşturun *HelloController.java* içinde *denetleyicisi* klasör. Dosyayı bir metin düzenleyicisinde açın ve aşağıdaki kodu ekleyin:

   ```java
   package com.contoso.myazuredemo;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.data.redis.core.StringRedisTemplate;
   import org.springframework.data.redis.core.ValueOperations;

   @RestController
   public class HelloController {
   
      @Autowired
      private StringRedisTemplate template;

      @RequestMapping("/")
      // Define the Hello World controller.
      public String hello() {
      
         ValueOperations<String, String> ops = this.template.opsForValue();

         // Add a Hello World string to your cache.
         String key = "greeting";
         if (!this.template.hasKey(key)) {
             ops.set(key, "Hello World!");
         }

         // Return the string from your cache.
         return ops.get(key);
      }
   }
   ```
   
   Burada gerekir değiştirmek `com.contoso.myazuredemo` projeniz için paket adı ile.

1. Kaydet ve Kapat *HelloController.java* dosya.

1. Yay önyükleme uygulamanızı Maven ile ve çalıştırın; Örneğin:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. Web uygulaması http://localhost: 8080 bir web tarayıcısı kullanarak göz atarak test veya curl kullanılabilir varsa aşağıdaki örnekteki gibi sözdizimini kullanın:

   ```shell
   curl http://localhost:8080
   ```

   "Hello World!" görmeniz gerekir hangi Redis önbellekten dinamik olarak alındığını görüntülenen örnek denetleyicinizi ileti.

## <a name="next-steps"></a>Sonraki adımlar

Azure üzerinde yay önyükleme uygulamalarında kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Yay önyükleme uygulamasını Azure App Service'e dağıtma](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [Azure kapsayıcı hizmeti Kubernetes kümesinde bir yay önyükleme uygulama çalıştıran](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

Azure’u Java ile kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure Java Geliştirici Merkezi] ve [Visual Studio Team Services için Java Araçları].

Redis önbelleği ile Azure üzerinde Java kullanmaya başlama hakkında daha fazla bilgi görmek için [Java ile Azure Redis önbelleği kullanmayı][Redis Cache with Java].

<!-- URL List -->

[Azure Java Geliştirici Merkezi]: https://azure.microsoft.com/develop/java/
[ücretsiz Azure hesabı]: https://azure.microsoft.com/pricing/free-trial/
[Visual Studio Team Services için Java Araçları]: https://java.visualstudio.com/
[MSDN abone Avantajlarınızı]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[yay önyükleme]: http://projects.spring.io/spring-boot/
[yay Initializr]: https://start.spring.io/
[Yay Framework]: https://spring.io/
[Redis Cache with Java]: cache-java-get-started.md

<!-- IMG List -->

[AZ01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ01.png
[AZ02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ02.png
[AZ03]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ03.png
[AZ04]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ04.png
[AZ05]: ./media/cache-java-spring-boot-initializer-with-redis-cache/AZ05.png

[SI01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI01.png
[SI02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI02.png
[SI03]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI03.png
[SI04]: ./media/cache-java-spring-boot-initializer-with-redis-cache/SI04.png

[RE01]: ./media/cache-java-spring-boot-initializer-with-redis-cache/RE01.png
[RE02]: ./media/cache-java-spring-boot-initializer-with-redis-cache/RE02.png
