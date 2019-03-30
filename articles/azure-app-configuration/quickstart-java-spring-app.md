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
ms.openlocfilehash: fec72a4fac6baa3869928c0203aeb29e53ce5ea4
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648478"
---
# <a name="quickstart-create-a-java-spring-app-with-app-configuration"></a>Hızlı Başlangıç: Uygulama yapılandırması ile bir Java Spring uygulaması oluşturma

Azure uygulama yapılandırması, azure'da yönetilen yapılandırma hizmetidir. Kolayca depolayın ve kodunuzdan tüm uygulama ayarlarınızı ayrılmış tek bir yerden yönetmek için kullanabilirsiniz. Bu hızlı başlangıçta bir Java Spring uygulamanıza hizmet gösterilmektedir.

Bu hızlı başlangıçtaki adımları uygulamak için herhangi bir kod Düzenleyicisi'ni kullanabilirsiniz. [Visual Studio Code](https://code.visualstudio.com/) Windows, macOS ve Linux platformlarını mükemmel bir seçenek kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç yapmak için desteklenen bir yükleme [Java Development Kit (JDK)](https://aka.ms/azure-jdks) sürüm 8 ile ve [Apache Maven](https://maven.apache.org/) sürüm 3.0 veya üzeri.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Bir uygulama yapılandırma deposu oluşturma

1. Yeni uygulama yapılandırma deposu oluşturmak için oturum açın [Azure portalında](https://aka.ms/azconfig/portal). Sayfanın sol üst köşesinde bulunan seçin **+ kaynak Oluştur**. İçinde **markette Ara** kutusuna **uygulama yapılandırması** ve Enter tuşuna basın.

    ![Uygulama yapılandırması için arama](./media/quickstarts/azure-app-configuration-new.png)

2. Seçin **uygulama yapılandırması** seçin ve arama sonuçlarını **Oluştur**.

3. Üzerinde **uygulama yapılandırması** > **Oluştur** sayfasında, aşağıdaki ayarları girin.

    | Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    | **Kaynak adı** | Genel olarak benzersiz bir ad | Uygulama yapılandırma deposu kaynağı için kullanılacak benzersiz kaynak adını girin. Ad 1 - 63 karakter arasında bir dize olmalı ve yalnızca rakam, harf ve `-` karakterini içermelidir. Adın başında veya sonunda `-` karakter ve ardışık `-` karakterler geçerli değildir.  |
    | **Abonelik** | Aboneliğiniz | Uygulama yapılandırması test etmek için kullanmak istediğiniz Azure aboneliğini seçin. Hesabınızda yalnızca bir aboneliğiniz varsa, otomatik olarak seçilir ve **abonelik** açılan görüntülenmiyorsa. |
    | **Kaynak grubu** | *AppConfigTestResources* | Uygulama yapılandırma deposu kaynağınız için bir kaynak grubu oluşturun veya seçin. Bu grup, kaynak grubunu silerek aynı anda silmek isteyebilirsiniz birden fazla kaynak düzenlemek için kullanışlıdır. Daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview). |
    | **Konum** | *Orta ABD* | SignalR kaynağınızın barındırılacağı coğrafi konumu belirtmek için **Konum**’u kullanın. En iyi performans için kaynak, uygulamanızın diğer bileşenlerle aynı bölgede oluşturun. |

    ![Bir uygulama yapılandırma deposu oluşturma](./media/quickstarts/azure-app-configuration-create.png)

4. **Oluştur**’u seçin. Dağıtımın tamamlanması birkaç dakika sürebilir.

5. Dağıtım tamamlandıktan sonra seçin **ayarları** > **erişim anahtarlarını**. Bir ya da birincil salt okunur veya birincil salt okunur anahtar bağlantı dizesini not edin. Bu bağlantı dizesi daha sonra oluşturduğunuz uygulama yapılandırma deposu ile iletişim kurmak için uygulamanızı yapılandırmak için kullanırsınız. Bağlantı dizesi aşağıdaki biçime sahiptir:

        Endpoint=<your_endpoint>;Id=<your_id>;Secret=<your_secret>

    Uygulamanızın tüm dize kullanın.

6. Seçin **anahtar/değer Gezgini** > **+ Oluştur** aşağıdaki anahtar-değer çiftlerini eklemek için:

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
> [Azure kaynaklarını tümleştirme için yönetilen kimlik](./integrate-azure-managed-service-identity.md)
