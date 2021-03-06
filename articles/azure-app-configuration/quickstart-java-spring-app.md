---
title: Azure uygulama yapılandırmasını kullanma hakkında bilgi edinmek için hızlı başlangıç | Microsoft Docs
description: Azure uygulama yapılandırması ile Java Spring uygulamaları kullanmak için bir hızlı başlangıcı.
services: azure-app-configuration
documentationcenter: ''
author: yidon
manager: jeffya
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: Spring
ms.workload: tbd
ms.date: 01/08/2019
ms.author: yidon
ms.openlocfilehash: a91c61edd773b5742b092f5d72a5a22f1d90e63b
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393543"
---
# <a name="quickstart-create-a-java-spring-app-with-app-configuration"></a>Hızlı Başlangıç: Uygulama yapılandırması ile bir Java Spring uygulaması oluşturma

Azure uygulama yapılandırması, azure'da yönetilen yapılandırma hizmetidir. Kolayca depolayın ve kodunuzdan tüm uygulama ayarlarınızı ayrılmış tek bir yerden yönetmek için kullanabilirsiniz. Bu hızlı başlangıçta bir Java Spring uygulamanıza hizmet gösterilmektedir.

Bu hızlı başlangıçtaki adımları uygulamak için herhangi bir kod Düzenleyicisi'ni kullanabilirsiniz. [Visual Studio Code](https://code.visualstudio.com/) Windows, macOS ve Linux platformlarını mükemmel bir seçenek kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç yapmak için desteklenen bir yükleme [Java Development Kit (JDK)](https://docs.microsoft.com/java/azure/jdk) sürüm 8 ile ve [Apache Maven](https://maven.apache.org/) sürüm 3.0 veya üzeri.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Bir uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Seçin **yapılandırması Gezgini** >  **+ Oluştur** aşağıdaki anahtar-değer çiftlerini eklemek için:

    | Anahtar | Değer |
    |---|---|
    | /application/config.message | Merhaba |

    Bırakın **etiket** ve **içerik türü** şimdilik boş.

## <a name="create-a-spring-boot-app"></a>Oluşturma bir Spring Boot uygulaması

Kullandığınız [Initializr](https://start.spring.io/) yeni bir Spring Boot proje oluşturmaktır.

1. konumuna gözatın <https://start.spring.io/>.

2. Aşağıdaki seçenekleri belirtin:

   * Oluşturmak bir **Maven** ile proje **Java**.
   * Belirtin bir **Spring Boot** 2.0 büyüktür veya ona eşit bir sürüm.
   * Belirtin **grubu** ve **Yapıt** uygulamanız için adları.
   * Ekleme **Web** bağımlılık.

3. Önceki seçenekleri belirttikten sonra Seç **proje oluşturma**. İstendiğinde, yerel bilgisayarınızda bir yol için projeyi indirin.

## <a name="connect-to-an-app-configuration-store"></a>Bir uygulama yapılandırma deposuna bağlanın

1. Yerel sisteminizde dosyalarını ayıkladıktan sonra basit bir Spring Boot uygulaması düzenleme için hazırdır. Bulun *pom.xml* uygulamanızın kök dizininde dosya.

2. Açık *pom.xml* dosyasını bir metin düzenleyicisinde ve Spring Cloud Azure Config Başlatıcı listesine eklemek `<dependencies>`:

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-appconfiguration-config</artifactId>
        <version>1.1.0.M3</version>
    </dependency>
    ```

3. Adlı yeni bir Java dosya oluşturma *MessageProperties.java* uygulamanızın paket dizinde. Aşağıdaki satırları ekleyin:

    ```java
    @ConfigurationProperties(prefix = "config")
    public class MessageProperties {
        private String message;

        public String getMessage() {
            return message;
        }

        public void setMessage(String message) {
            this.message = message;
        }
    }
    ```

4. Adlı yeni bir Java dosya oluşturma *HelloController.java* uygulamanızın paket dizinde. Aşağıdaki satırları ekleyin:

    ```java
    @RestController
    public class HelloController {
        private final MessageProperties properties;

        public HelloController(MessageProperties properties) {
            this.properties = properties;
        }

        @GetMapping
        public String getMessage() {
            return "Message: " + properties.getMessage();
        }
    }
    ```

5. Ana uygulama Java dosyasını açın ve eklemek `@EnableConfigurationProperties` bu özelliği etkinleştirmek için.

    ```java
    @SpringBootApplication
    @EnableConfigurationProperties(MessageProperties.class)
    public class AzureConfigApplication {
        public static void main(String[] args) {
            SpringApplication.run(AzureConfigApplication.class, args);
        }
    }
    ```

6. Adlı yeni bir dosya oluşturun `bootstrap.properties` uygulamanızın kaynak dizini altında ve dosyasına aşağıdaki satırları ekleyin. Örnek değerler, uygulama yapılandırma deposu için uygun özellikleri değiştirin.

    ```properties
    spring.cloud.azure.appconfiguration.stores[0].connection-string=[your-connection-string]
    ```

## <a name="build-and-run-the-app-locally"></a>Derleme ve uygulamayı yerel olarak çalıştırma

1. Maven ile Spring Boot uygulamanızı derleyin ve çalıştırın, örneğin:

    ```shell
    mvn clean package
    mvn spring-boot:run
    ```
2. Uygulamanız çalışmaya başladıktan sonra kullanın *curl* Örneğin, uygulamanızı test etmek için:

      ```shell
      curl -X GET http://localhost:8080/
      ```
    Uygulama yapılandırma deposunda girdiğiniz iletisini görürsünüz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir uygulama yapılandırma deposu oluşturuldu ve ile bir Java Spring uygulaması kullanılır. Daha fazla bilgi için [azure'da Spring](https://docs.microsoft.com/java/azure/spring-framework/).

Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi için kimlik doğrulaması gösteren bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirme](./howto-integrate-azure-managed-service-identity.md)
