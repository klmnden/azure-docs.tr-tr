---
title: "Azure kapsayıcı Hizmeti'nde Linux'ta yay önyükleme Web uygulaması dağıtma | Microsoft Docs"
description: "Bu öğreticide, ancak Microsoft azure'da bir Linux web uygulaması olarak yay önyükleme uygulama dağıtma adımları açıklanmaktadır."
services: container-service
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: container-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/04/2017
ms.author: asirveda;robmcm
ms.custom: mvc
ms.openlocfilehash: daa0ed81a6b9f20e146698947099a991da42cd6d
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="deploy-a-spring-boot-application-on-linux-in-the-azure-container-service"></a>Azure kapsayıcı Hizmeti'nde Linux'ta yay önyükleme uygulamasını dağıtma

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

**[Yay Framework]**  Java geliştiriciler kuruluş düzeyinde uygulamalar oluşturmanıza yardımcı olan bir açık kaynaklı bir çözümdür. Yerleşik daha popüler projelerden biri üzerinde üst o platformudur [yay önyükleme], tek başına Java uygulamaları oluşturmak için basitleştirilmiş bir yaklaşım sağlar.

**[Docker]**  olan geliştiricilere yardımcı açık kaynak çözümleri otomatikleştirmek dağıtım, ölçeklendirme ve kapsayıcılarında çalışan kendi uygulamalarını yönetimi.

Bu öğretici, geliştirmek ve bir Linux ana yay önyükleme uygulama dağıtmak için Docker kullanarak kılavuzluk [Azure kapsayıcı Hizmeti'ni (ACS)].

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşullara sahip olmanız gerekir:

* Bir Azure aboneliği; bir Azure aboneliği zaten sahip değilseniz, etkinleştirebilir, [MSDN abone Avantajlarınızı] veya kaydolun bir [ücretsiz Azure hesabı].
* [Azure komut satırı arabirimi (CLI)].
* Güncel bir [Java Geliştirme Seti (JDK)].
* Apache'nın [Maven] aracını (sürüm 3) yapılandırma.
* A [Git] istemci.
* A [Docker] istemci.

> [!NOTE]
>
> Bu öğretici sanallaştırma gereksinimleri nedeniyle, bir sanal makinede bu makaledeki adımları izleyin olamaz; sanallaştırma özellikleri etkin bir fiziksel bilgisayar kullanmanız gerekir.
>

## <a name="create-the-spring-boot-on-docker-getting-started-web-app"></a>Yay önyükleme Docker Başlarken web uygulaması oluştur

Aşağıdaki adımları basit bir yay önyükleme web uygulaması oluşturma ve yerel olarak test etmek için gereken adımlarda size yol.

1. Bir komut istemi açın ve uygulamanızı tutun ve bu dizine değiştirmek için yerel bir dizin oluşturun; Örneğin:
   ```
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   --ya da--
   ```
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Kopya [Docker Başlarken yay önyüklemede] örnek proje dizine oluşturduğunuz; örneğin:
   ```
   git clone https://github.com/spring-guides/gs-spring-boot-docker.git
   ```

1. Projeyi Dizin Değiştir; Örneğin:
   ```
   cd gs-spring-boot-docker/complete
   ```

1. Maven kullanarak JAR dosyasını oluşturun; Örneğin:
   ```
   mvn package
   ```

1. Web uygulaması oluşturulduktan sonra dizine geçin `target` burada JAR dosyasını bulunur ve web uygulaması; başlangıç dizini örneğin:
   ```
   cd target
   java -jar gs-spring-boot-docker-0.1.0.jar
   ```

1. Web uygulaması, bir web tarayıcısı kullanarak yerel olarak kendisine göz atarak sınayın. Örneğin, varsa curl kullanılabilir ve Tomcat sunucusunu bağlantı noktası 80 üzerinde çalışacak şekilde yapılandırılmış:
   ```
   curl http://localhost
   ```

1. Aşağıdaki ileti görürsünüz: **Hello Docker World!**

   ![Örnek uygulamayı yerel olarak göz atın][SB01]

## <a name="create-an-azure-container-registry-to-use-as-a-private-docker-registry"></a>Özel bir Docker kayıt defteri kullanmak için bir Azure kapsayıcı kayıt oluşturun

Aşağıdaki adımlar, Azure portalını kullanarak Azure kapsayıcı kayıt defteri aracılığıyla yol.

> [!NOTE]
>
> Azure portal yerine Azure CLI kullanmak istiyorsanız, adımları [Azure CLI 2.0 kullanan özel bir Docker kapsayıcısı kayıt](../../container-registry/container-registry-get-started-azure-cli.md).
>

1. Gözat [Azure portal] ve oturum açın.

   Hesabınıza Azure portalında oturum açtıktan sonra içindeki adımları takip edebilirsiniz [Azure portalını kullanarak özel bir Docker kapsayıcısı kayıt] aşağıdaki adımları için sake açıklanır makale expediency.

1. Menü simgesini **+ yeni**, ardından **kapsayıcıları**ve ardından **Azure kapsayıcı kayıt defteri**.
   
   ![Yeni bir Azure kapsayıcı kayıt defteri oluşturma][AR01]

1. Bilgi sayfası Azure kapsayıcı kayıt defteri şablon görüntülenen için tıklattığınızda **oluşturma**. 

   ![Yeni bir Azure kapsayıcı kayıt defteri oluşturma][AR02]

1. Zaman **oluşturma kapsayıcı kayıt defteri** sayfası görüntülenirse, girin, **kayıt defteri adı** ve **kaynak grubu**, seçin **etkinleştirmek** için **Yönetici kullanıcı**ve ardından **oluşturma**.

   ![Azure kapsayıcı kayıt defteri ayarlarını yapılandırma][AR03]

1. Kapsayıcı kaydınız oluşturulduktan sonra Azure portalında kapsayıcı kayıt gidin ve ardından **erişim tuşları**. Kullanıcı adı ve parola sonraki adımlar için not edin.

   ![Azure kapsayıcı kayıt defteri erişim tuşları][AR04]

## <a name="configure-maven-to-use-your-azure-container-registry-access-keys"></a>Azure kapsayıcı kayıt defteri erişim tuşlarınızı kullanmak için Maven yapılandırın

1. Maven yüklemeniz için yapılandırma dizinine gidin ve açmak *settings.xml* dosyasını bir metin düzenleyicisiyle.

1. Bu öğreticinin önceki bölümdeki Azure kapsayıcı kayıt defteri erişim ayarlarınız eklemek `<servers>` koleksiyonunda *settings.xml* dosya; örneğin:

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>AbCdEfGhIjKlMnOpQrStUvWxYz</password>
      </server>
   </servers>
   ```

1. Yay önyükleme uygulamanız için projeyi dizinine gidin (örneğin: "*C:\SpringBoot\gs-spring-boot-docker\complete*"veya"*/users/robert/SpringBoot/gs-spring-boot-docker/complete* ") ve açın *pom.xml* dosyasını bir metin düzenleyicisiyle.

1. Güncelleştirme `<properties>` koleksiyonunda *pom.xml* oturum açma sunucusu değeri bir dosyayla Azure kapsayıcı kaydınız Bu öğreticinin; önceki bölümdeki için örneğin:

   ```xml
   <properties>
      <docker.image.prefix>wingtiptoysregistry.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
   </properties>
   ```

1. Güncelleştirme `<plugins>` koleksiyonunda *pom.xml* dosya böylece `<plugin>` Azure kapsayıcı kaydınız Bu öğreticinin önceki bölümdeki için oturum açma sunucusu adresi ve kayıt defteri adını içerir. Örneğin:

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
         <serverId>wingtiptoysregistry</serverId>
         <registryUrl>https://wingtiptoysregistry.azurecr.io</registryUrl>
      </configuration>
   </plugin>
   ```

1. Yay önyükleme uygulamanız için projeyi dizinine gidin ve uygulamayı yeniden ve Azure kapsayıcı kaydınız kapsayıcı gönderme için aşağıdaki komutu çalıştırın:

   ```
   mvn package docker:build -DpushImage 
   ```

> [!NOTE]
>
> Docker kapsayıcısı Azure'a dağıtmaya, Docker kapsayıcısı başarıyla oluşturuldu ancak, aşağıdakilerden birini benzer bir hata iletisi bile alabilirsiniz:
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: no basic auth credentials`
>
> * `[ERROR] Failed to execute goal com.spotify:docker-maven-plugin:0.4.11:build (default-cli) on project gs-spring-boot-docker: Exception caught: Incomplete Docker registry authorization credentials. Please provide all of username, password, and email or none.`
>
> Bu durumda, Docker komut satırından Azure hesabınızda oturum açmak gerekebilir; Örneğin:
>
> `docker login -u wingtiptoysregistry -p "AbCdEfGhIjKlMnOpQrStUvWxYz" wingtiptoysregistry.azurecr.io`
>
> Ardından, komut satırından, kapsayıcı gönderebilir; Örneğin:
>
> `docker push wingtiptoysregistry.azurecr.io/gs-spring-boot-docker`
>

## <a name="create-a-web-app-on-linux-on-azure-app-service-using-your-container-image"></a>Linux kapsayıcı görüntünüzü kullanarak Azure App Service'te bir web uygulaması oluşturma

1. Gözat [Azure portal] ve oturum açın.

1. Menü simgesini **+ yeni**, ardından **Web + mobil**ve ardından **Linux üzerinde Web uygulaması**.
   
   ![Azure portalında yeni bir web uygulaması oluşturma][LX01]

1. Zaman **Linux üzerinde Web uygulaması** sayfası görüntülenirse, aşağıdaki bilgileri girin:

   a. İçin benzersiz bir ad girin **uygulama adı**; örneğin: "*wingtiptoyslinux*."

   b. Seçin, **abonelik** aşağı açılan listeden.

   c. Var olan seçin **kaynak grubu**, veya yeni bir kaynak grubu oluşturmak için bir ad belirtin.

   d. Tıklatın **yapılandırma kapsayıcısı** ve aşağıdaki bilgileri girin:

      * Seçin **özel kayıt defteri**.

      * **Görüntü ve isteğe bağlı etiket**: daha önce; Örneğin, kapsayıcı adı belirtin: "*wingtiptoysregistry.azurecr.io/gs-spring-boot-docker:latest*"

      * **Sunucu URL'si**: kayıt defteri URL'den belirtin önceki; örneğin: "*https://wingtiptoysregistry.azurecr.io*"

      * **Oturum açma kullanıcı** ve **parola**: oturum açma kimlik bilgilerinizi belirtin, **erişim tuşları** önceki adımlarda kullanılan.
   
   e. Yukarıdaki bilgilerin tümünü girdikten sonra tıklatın **Tamam**.

   ![Web uygulaması ayarlarını yapılandır][LX02]

1. **Oluştur**'a tıklayın.

> [!NOTE]
>
> Azure 80 veya 8080 standart bağlantı noktalarında çalışan katıştırılmış Tomcat sunucuya Internet istekleri otomatik olarak eşler. Ancak, özel bir bağlantı noktası üzerinde çalıştırmak üzere katıştırılmış Tomcat sunucunuzu yapılandırdıysanız, katıştırılmış Tomcat sunucunuz için bağlantı noktasını tanımlar web uygulamanız için bir ortam değişkeni eklemeniz gerekir. Bunu yapmak için aşağıdaki adımları kullanın:
>
> 1. Gözat [Azure portal] ve oturum açın.
> 
> 2. Simgesini **uygulama hizmetleri**. (Aşağıdaki görüntü #1 öğesinde bakın.)
>
> 3. Listeden web uygulamanızı seçin. (Aşağıdaki görüntü #2 öğe.)
>
> 4. Tıklatın **uygulama ayarları**. (Aşağıdaki görüntü #3 öğe.)
>
> 5. İçinde **uygulama ayarları** bölümünde, adlı yeni bir ortam değişkeni ekleyin **bağlantı noktası** ve özel bağlantı noktası numarası için bir değer girin. (Aşağıdaki görüntü #4 öğe.)
>
> 6. **Kaydet** düğmesine tıklayın. (Aşağıdaki görüntü #5 öğe.)
>
> ![Azure Portalı'nda bir özel bağlantı noktası numarası kaydetme][LX03]
>

<!--
##  OPTIONAL: Configure the embedded Tomcat server to run on a different port

The embedded Tomcat server in the sample Spring Boot application is configured to run on port 8080 by default. However, if you want to run the embedded Tomcat server to run on a different port, such as port 80 for local testing, you can configure the port by using the following steps.

1. Go to the *resources* directory (or create the directory if it does not exist); for example:
   ```shell
   cd src/main/resources
   ```

1. Open the *application.yml* file in a text editor if it exists, or create a new YAML file if it does not exist.

1. Modify the **server** setting so that the server runs on port 80; for example:
   ```yaml
   server:
      port: 80
   ```

1. Save and close the *application.yml* file.
-->

## <a name="next-steps"></a>Sonraki adımlar

Azure üzerinde yay önyükleme uygulamalarında kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Yay önyükleme uygulamasını Azure App Service'e dağıtma](../../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)
* [Azure kapsayıcı hizmeti Kubernetes kümesinde yay önyükleme uygulamasını dağıtma](container-service-deploy-spring-boot-app-on-kubernetes.md)

Azure’u Java ile kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure Java Geliştirici Merkezi] ve [Visual Studio Team Services için Java Araçları].

Docker örnek proje yay önyüklemede hakkında daha fazla ayrıntı için bkz: [Docker Başlarken yay önyüklemede].

Kendi yay önyükleme uygulamaları ile çalışmaya başlama hakkında bilgi için bkz: **yay Initializr** https://start.spring.io/ adresindeki.

Basit bir yay önyükleme uygulaması oluşturma ile çalışmaya başlama hakkında daha fazla bilgi için https://start.spring.io/ adresindeki yay Initializr bakın.

Azure ile özel Docker görüntülerinizi kullanımı için ek örnekler için bkz: [Linux Azure Web uygulaması için özel bir Docker görüntü kullanarak].

<!-- URL List -->

[Azure komut satırı arabirimi (CLI)]: /cli/azure/overview
[Azure kapsayıcı Hizmeti'ni (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Geliştirici Merkezi]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Azure portalını kullanarak özel bir Docker kapsayıcısı kayıt]: /azure/container-registry/container-registry-get-started-portal
[Linux Azure Web uygulaması için özel bir Docker görüntü kullanarak]: /azure/app-service-web/app-service-linux-using-custom-docker-image
[Docker]: https://www.docker.com/
[ücretsiz Azure hesabı]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Geliştirme Seti (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Visual Studio Team Services için Java Araçları]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN abone Avantajlarınızı]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[yay önyükleme]: http://projects.spring.io/spring-boot/
[Docker Başlarken yay önyüklemede]: https://github.com/spring-guides/gs-spring-boot-docker
[Yay Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/container-service-deploy-spring-boot-app-on-linux/SB01.png
[SB02]: ./media/container-service-deploy-spring-boot-app-on-linux/SB02.png

[AR01]: ./media/container-service-deploy-spring-boot-app-on-linux/AR01.png
[AR02]: ./media/container-service-deploy-spring-boot-app-on-linux/AR02.png
[AR03]: ./media/container-service-deploy-spring-boot-app-on-linux/AR03.png
[AR04]: ./media/container-service-deploy-spring-boot-app-on-linux/AR04.png

[LX01]: ./media/container-service-deploy-spring-boot-app-on-linux/LX01.png
[LX02]: ./media/container-service-deploy-spring-boot-app-on-linux/LX02.png
[LX03]: ./media/container-service-deploy-spring-boot-app-on-linux/LX03.png
