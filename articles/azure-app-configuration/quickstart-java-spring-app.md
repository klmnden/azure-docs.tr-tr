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
ms.openlocfilehash: 83dc6436c69038a43f32588d485790a701c2e18d
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57549264"
---
# <a name="quickstart-create-a-java-spring-app-with-app-configuration"></a>Hızlı Başlangıç: Uygulama yapılandırması ile bir Java Spring uygulaması oluşturma

Azure uygulama yapılandırması, azure'da yönetilen yapılandırma hizmetidir. Kolayca depolayıp kodunuzdan tüm uygulama ayarlarınızı ayrılmış tek bir yerden yönetmenize olanak tanır. Bu hızlı başlangıçta bir Java Spring uygulamanıza hizmet gösterilmektedir.

Bu hızlı başlangıçtaki adımları tamamlamak için herhangi bir kod düzenleyicisini kullanabilirsiniz. Ancak, Windows, macOS ve Linux platformlarında sağlanan [Visual Studio Code](https://code.visualstudio.com/) mükemmel bir seçenektir.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için desteklenen bir yükleme [Java Development Kit (JDK)](https://aka.ms/azure-jdks) sürüm 8 ile ve [Apache Maven](https://maven.apache.org/) sürüm 3.0 veya üzeri.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Bir uygulama yapılandırma deposu oluşturma

1. Yeni uygulama yapılandırma deposu oluşturmak için ilk kez oturum için [Azure portalında](https://aka.ms/azconfig/portal). Sayfanın sol üst kısmında **+ Kaynak oluştur**'a tıklayın. İçinde **markette Ara** metin kutusuna **uygulama yapılandırması** basın **Enter**.

    ![Uygulama yapılandırması için arama](./media/quickstarts/azure-app-configuration-new.png)

2. Tıklayın **uygulama yapılandırması** arama sonuçlarına ve ardından **Oluştur**.

3. İçinde **uygulama yapılandırması** > **Oluştur** sayfasında, aşağıdaki ayarları girin:

    | Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    | **Kaynak adı** | Genel olarak benzersiz bir ad | Uygulama yapılandırma deposu kaynağı için kullanılacak benzersiz kaynak adını girin. Ad 1 - 63 karakter arasında bir dize olmalı ve yalnızca rakam, harf ve `-` karakterini içermelidir. Ad `-` karakteriyle başlayamaz veya bitemez ve ardışık `-` karakterler geçerli olmaz.  |
    | **Abonelik** | Aboneliğiniz | Uygulama yapılandırması test etmek için kullanmak istediğiniz Azure aboneliğini seçin. Hesabınızda yalnızca bir abonelik varsa bu otomatik olarak seçilir ve **Abonelik** açılan penceresi görüntülenmez. |
    | **Kaynak Grubu** | *AppConfigTestResources* | Uygulama yapılandırma deposu kaynağınız için bir kaynak grubu oluşturun veya seçin. Bu grup, kaynak grubunu silerek aynı anda silmek isteyebileceğiniz birden fazla kaynağı düzenlemek için kullanışlıdır. Daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için Kaynak gruplarını kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). |
    | **Konum** | *Orta ABD* | SignalR kaynağınızın barındırılacağı coğrafi konumu belirtmek için **Konum**’u kullanın. En iyi performans için kaynağınızı uygulamanızın diğer bileşenleriyle aynı bölgede oluşturmanızı öneririz. |

    ![Bir uygulama yapılandırma deposu oluşturma](./media/quickstarts/azure-app-configuration-create.png)

4. **Oluştur**’a tıklayın. Dağıtımın tamamlanması birkaç dakika sürebilir.

5. Dağıtım tamamlandıktan sonra tıklayın **ayarları** > **erişim anahtarlarını**. Her iki birincil salt okunur veya birincil salt okunur anahtar bağlantı dizesini not edin. Bu daha az önce oluşturduğunuz uygulama yapılandırma deposu ile iletişim kurmak için uygulamanızı yapılandırmak için kullanın. Bağlantı dizesi aşağıdaki biçime sahiptir:

        Endpoint=<your_endpoint>;Id=<your_id>;Secret=<your_secret>

    Uygulamanızda dizenin tamamını kullanmanız gerekir.

6. Tıklayın **anahtar/değer Gezgini** ve **+ Oluştur** aşağıdaki anahtar-değer çiftlerini eklemek için:

    | Anahtar | Değer |
    |---|---|
    | /application/config.message | Merhaba |

    Size vermeyecek **etiket** ve **içerik türü** şimdilik boş.

## <a name="create-a-spring-boot-app"></a>Oluşturma bir Spring Boot uygulaması

Kullanacağınız [Initializr](https://start.spring.io/) yeni bir Spring Boot proje oluşturmaktır.

1. <https://start.spring.io/> adresine gidin.

2. Aşağıdaki seçenekleri belirtin:

   * Oluşturmak bir **Maven** ile proje **Java**.
   * Belirtin bir **Spring Boot** 2.0 büyüktür veya ona eşit bir sürüm.
   * Belirtin **grubu** ve **Yapıt** uygulamanız için adları.
   * Ekleme **Web** bağımlılık.

3. Yukarıda listelenen seçenekleri belirtildiğinde tıklayın **proje oluşturma**. İstendiğinde, yerel bilgisayarınızda bir yol için projeyi indirin.

## <a name="connect-to-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanın

1. Yerel sisteminizde dosyalarını ayıkladıktan sonra basit bir Spring Boot uygulaması düzenleme için hazır hale gelirsiniz. Bulun *pom.xml* uygulamanızın kök dizininde dosya.

2. Açık *pom.xml* dosyasını bir metin düzenleyicisinde ve Spring Cloud Azure Config Başlatıcı listesine eklemek `<dependencies>`:

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-starter-azure-appconfiguration-config</artifactId>
        <version>1.1.0.M1</version>
    </dependency>
    ```

3. Adlı yeni bir Java dosya oluşturma *MessageProperties.java* uygulamanızın paket dizinde. Satırları ekleyin.

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

4. Adlı yeni bir Java dosya oluşturma *HelloController.java* uygulamanızın paket dizinde. Satırları ekleyin.

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

6. Adlı yeni bir dosya oluşturun `bootstrap.properties` uygulamanızın kaynak dizini altındaki dosyasına aşağıdaki satırları ekleyin ve örnek değerleri uygulama yapılandırma deponuz için uygun özellikleri değiştirin.

    ```properties
    spring.cloud.azure.appconfiguration.stores[0].connection-string=[your-connection-string]
    ```

## <a name="build-and-run-the-app-locally"></a>Derleme ve uygulamayı yerel olarak çalıştırma

1. Maven ile Spring Boot uygulamanızı derleyin ve çalıştırın; Örneğin:

    ```shell
    mvn clean package
    mvn spring-boot:run
    ```
2. Uygulamanızı çalışır duruma geçtikten sonra kullanabileceğiniz *curl* ; uygulamanızı test etmek için örneğin:

      ```shell
      curl -X GET http://localhost:8080/
      ```
    Uygulama yapılandırma deposuna girdiğiniz iletiyi görmeniz gerekir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir uygulama yapılandırma deposu oluşturulur ve bir Java Spring uygulamasıyla kullanılan. Ziyaret [Azure giriş sayfasındaki Spring](https://docs.microsoft.com/java/azure/spring-framework/) için daha fazla bilgi için.

Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi edinmek için kimlik doğrulaması gösteren bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Azure kaynaklarını tümleştirme için yönetilen kimlik](./integrate-azure-managed-service-identity.md)