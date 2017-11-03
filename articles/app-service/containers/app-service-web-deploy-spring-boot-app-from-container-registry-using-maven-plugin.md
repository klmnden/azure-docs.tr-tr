---
title: "Azure kapsayıcı kayıt defteri yay önyükleme uygulamada Azure App Service'e dağıtmak için Azure Web uygulamaları için Maven eklentisi kullanma"
description: "Bu öğretici bir Maven eklentisi kullanarak Azure kapsayıcı kayıt defteri yay önyükleme uygulamada Azure için Azure App Service'e dağıtma adımları ancak yol gösterir."
services: 
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: b087003b3a1e236e4a306678904107b8bf99395e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-maven-plugin-for-azure-web-apps-to-deploy-a-spring-boot-app-in-azure-container-registry-to-azure-app-service"></a>Azure kapsayıcı kayıt defteri yay önyükleme uygulamada Azure App Service'e dağıtmak için Azure Web uygulamaları için Maven eklentisi kullanma

**[Yay Framework]**  , Java geliştiricilerinin web, mobil ve API uygulamaları oluşturma yardımcı olan bir popüler açık kaynak çerçevedir. Bu öğretici kullanılarak oluşturulmuş bir örnek uygulama kullanır [yay önyükleme], kuralı güdümlü bir yaklaşım yay hızlıca başlamak için kullanma.

Bu makalede, Azure kapsayıcı kayıt defteri bir örnek yay önyükleme uygulamayı dağıtmak ve ardından uygulamanızı Azure App Service'e dağıtmak için Azure Web uygulamaları için Maven eklentisi kullanma gösterilmektedir.

> [!NOTE]
>
> Azure Web uygulamaları için Maven eklentisi şu anda önizleme olarak kullanılabilir. Ek özellikler gelecek için planlanan olsa da, şimdilik yalnızca FTP Yayımlama, desteklenir.
>

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşullara sahip olmanız gerekir:

* Bir Azure aboneliği; bir Azure aboneliği zaten sahip değilseniz, etkinleştirebilir, [MSDN abone Avantajlarınızı] veya kaydolun bir [ücretsiz Azure hesabı].
* [Azure komut satırı arabirimi (CLI)].
* Güncel [Java Geliştirme Seti (JDK)], 1,7 veya sonraki bir sürümü.
* Apache'nın [Maven] aracını (sürüm 3) yapılandırma.
* A [Git] istemci.
* A [Docker] istemci.

> [!NOTE]
>
> Bu öğretici sanallaştırma gereksinimleri nedeniyle, bir sanal makinede bu makaledeki adımları izleyin olamaz; sanallaştırma özellikleri etkin bir fiziksel bilgisayar kullanmanız gerekir.
>

## <a name="clone-the-sample-spring-boot-on-docker-web-app"></a>Docker web uygulaması üzerinde yay önyükleme örnek kopyalama

Bu bölümde kapsayıcılı yay önyükleme uygulama kopyalamak ve yerel olarak test.

1. Bir komut istemi veya terminal penceresi açın ve yay önyükleme uygulamanızı tutun ve bu dizine değiştirmek için yerel bir dizin oluşturun; Örneğin:
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   --ya da--
   ```shell
   md /users/robert/SpringBoot
   cd /users/robert/SpringBoot
   ```

1. Kopya [Docker Başlarken yay önyüklemede] örnek proje dizine oluşturduğunuz; örneğin:
   ```shell
   git clone -b private-registry https://github.com/Microsoft/gs-spring-boot-docker
   ```

1. Projeyi Dizin Değiştir; Örneğin:
   ```shell
   cd gs-spring-boot-docker/complete
   ```

1. Maven kullanarak JAR dosyasını oluşturun; Örneğin:
   ```shell
   mvn clean package
   ```

1. Web uygulaması oluşturduğunuzda Maven kullanarak web uygulaması başlatın; Örneğin:
   ```shell
   mvn spring-boot:run
   ```

1. Web uygulaması, bir web tarayıcısı kullanarak yerel olarak kendisine göz atarak sınayın. Örneğin, curl kullanılabilir varsa, aşağıdaki komutu kullanabilirsiniz:
   ```shell
   curl http://localhost:8080
   ```

1. Aşağıdaki ileti görürsünüz: **Hello Docker World**

   ![Örnek uygulamayı yerel olarak göz atın][SB01]

## <a name="create-an-azure-service-principal"></a>Bir Azure hizmet sorumlusu oluşturma

Bu bölümde, Azure, kapsayıcı Azure'a dağıtırken Maven eklentisi kullanan hizmet sorumlusu oluşturun.

1. Bir komut istemi açın.

1. Azure CLI kullanarak Azure hesabınızda oturum açın:
   ```azurecli
   az login
   ```
   Oturum açma işlemini tamamlamak için yönergeleri izleyin.

1. Bir Azure hizmet sorumlusu oluşturun:
   ```azurecli
   az ad sp create-for-rbac --name "uuuuuuuu" --password "pppppppp"
   ```
   Burada `uuuuuuuu` kullanıcı adı ve `pppppppp` için hizmet sorumlusu paroladır.

1. Azure, aşağıdaki örneğe benzer JSON ile yanıt verir:
   ```json
   {
      "appId": "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa",
      "displayName": "uuuuuuuu",
      "name": "http://uuuuuuuu",
      "password": "pppppppp",
      "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

   > [!NOTE]
   >
   > Azure için kapsayıcıyı dağıtmak için Maven eklentisi yapılandırdığınızda bu JSON yanıt değerleri kullanır. `aaaaaaaa`, `uuuuuuuu`, `pppppppp`, Ve `tttttttt` Bu örnekte, Maven yapılandırdığınızda bu değerleri kendi ilgili öğelerine eşlemek kolaylaştırmak için kullanılan yer tutucu değerlerini olduğunuz `settings.xml` sonraki bölümde dosya.
   >
   >

## <a name="create-an-azure-container-registry-using-the-azure-cli"></a>Azure CLI kullanarak bir Azure kapsayıcı kayıt defteri oluşturma

1. Bir komut istemi açın.

1. Azure hesabınızda oturum açın:
   ```azurecli
   az login
   ```

1. Bu makalede kullanacağınız Azure kaynakları için bir kaynak grubu oluşturun:
   ```azurecli
   az group create --name=wingtiptoysresources --location=westus
   ```
   Değiştir `wingtiptoysresources` Bu örnekte, kaynak grubu için benzersiz bir ada sahip.

1. Bir özel Azure kapsayıcı kayıt defteri yay önyükleme uygulamanız için kaynak grubu oluşturun: 
   ```azurecli
   az acr create --admin-enabled --resource-group wingtiptoysresources --location westus --name wingtiptoysregistry --sku Basic
   ```
   Değiştir `wingtiptoysregistry` Bu örnekte, kapsayıcı kayıt defteri için benzersiz bir ad ile.

1. Kapsayıcı kaydınız parolasını Al:
   ```azurecli
   az acr credential show --name wingtiptoysregistry --query passwords[0]
   ```
   Azure parolanızla yanıt verir; Örneğin:
   ```json
   {
      "name": "password",
      "value": "xxxxxxxxxx"
   }
   ```

## <a name="add-your-azure-container-registry-and-azure-service-principal-to-your-maven-settings"></a>Azure kapsayıcı kayıt defteri ve Azure hizmet sorumlusu Maven ayarlarınızı ekleme

1. Maven açmak `settings.xml` dosyasını bir metin düzenleyicisinde; bu dosyayı aşağıdaki örneklerde gibi bir yol olabilir:
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. Bu makalenin önceki bölümdeki Azure kapsayıcı kayıt defteri erişim ayarlarınız eklemek `<servers>` koleksiyonunda *settings.xml* dosya; örneğin:

   ```xml
   <servers>
      <server>
         <id>wingtiptoysregistry</id>
         <username>wingtiptoysregistry</username>
         <password>xxxxxxxxxx</password>
      </server>
   </servers>
   ```
   Konumlar:
   Öğesi | Açıklama
   ---|---|---
   `<id>` | Özel Azure kapsayıcı kayıt defteri adını içerir.
   `<username>` | Özel Azure kapsayıcı kayıt defteri adını içerir.
   `<password>` | Bu makalenin önceki bölümünde aldığınız parolayı içerir.

1. Bu makalenin önceki bir bölümünden Azure hizmet asıl ayarlarınızı ekleme `<servers>` koleksiyonunda *settings.xml* dosya; örneğin:

   ```xml
   <servers>
      <server>
        <id>azure-auth</id>
         <configuration>
            <client>aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa</client>
            <tenant>tttttttt-tttt-tttt-tttt-tttttttttttt</tenant>
            <key>pppppppp</key>
            <environment>AZURE</environment>
         </configuration>
      </server>
   </servers>
   ```
   Konumlar:
   Öğesi | Açıklama
   ---|---|---
   `<id>` | Web uygulamanızı Azure'a dağıtırken, güvenlik ayarlarınızı aramak için Maven kullanan benzersiz bir ad belirtir.
   `<client>` | İçeren `appId` hizmet sorumlusu değeri.
   `<tenant>` | İçeren `tenant` hizmet sorumlusu değeri.
   `<key>` | İçeren `password` hizmet sorumlusu değeri.
   `<environment>` | Olduğu hedef Azure bulut ortamı tanımlar `AZURE` Bu örnekte. (Ortamlar tam listesi kullanılabilir [Azure Web uygulamaları için Maven eklentisi] belgeleri)

1. Kaydet ve Kapat *settings.xml* dosya.

## <a name="build-your-docker-container-image-and-push-it-to-your-azure-container-registry"></a>Docker kapsayıcısı yansıması oluştur ve Azure kapsayıcı kaydınız gönderme

1. Yay önyükleme uygulamanız için projeyi dizin (örneğin gidin "*C:\SpringBoot\gs-spring-boot-docker\complete*"veya"*/users/robert/SpringBoot/gs-spring-boot-docker/complete*") ve açın *pom.xml* dosyasını bir metin düzenleyicisiyle.

1. Güncelleştirme `<properties>` koleksiyonunda *pom.xml* oturum açma sunucusu değeri bir dosyayla Azure kapsayıcı kaydınız Bu öğreticinin; önceki bölümdeki için örneğin:

   ```xml
   <properties>
      <azure.containerRegistry>wingtiptoysregistry</azure.containerRegistry>
      <docker.image.prefix>${azure.containerRegistry}.azurecr.io</docker.image.prefix>
      <java.version>1.8</java.version>
      <maven.build.timestamp.format>yyyyMMddHHmmssSSS</maven.build.timestamp.format>
   </properties>
   ```
   Konumlar:
   Öğesi | Açıklama
   ---|---|---
   `<azure.containerRegistry>` | Özel Azure kapsayıcı kayıt defteri adını belirtir.
   `<docker.image.prefix>` | Ekleyerek türetilmiş özel Azure kapsayıcı kaydınız URL'sini belirtir ". azurecr.io" özel kapsayıcı kayıt adı.

1. Doğrulayın `<plugin>` Docker eklenti için *pom.xml* dosyası bu öğreticinin önceki adımdan oturum açma sunucusu adresi ve kayıt defteri adı için doğru özellikler içerir. Örneğin:

   ```xml
   <plugin>
      <groupId>com.spotify</groupId>
      <artifactId>docker-maven-plugin</artifactId>
      <version>0.4.11</version>
      <configuration>
         <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
         <registryUrl>https://${docker.image.prefix}</registryUrl>
         <serverId>${azure.containerRegistry}</serverId>
         <dockerDirectory>src/main/docker</dockerDirectory>
         <resources>
            <resource>
               <targetPath>/</targetPath>
               <directory>${project.build.directory}</directory>
               <include>${project.build.finalName}.jar</include>
            </resource>
         </resources>
      </configuration>
   </plugin>
   ```
   Konumlar:
   Öğesi | Açıklama
   ---|---|---
   `<serverId>` | Özel Azure kapsayıcı kayıt defteri adını içeren özellik belirtir.
   `<registryUrl>` | Özel Azure kapsayıcı kaydınız URL'sini içeren özellik belirtir.

1. Yay önyükleme uygulamanız için projeyi dizinine gidin ve uygulamayı yeniden ve Azure kapsayıcı kaydınız kapsayıcı gönderme için aşağıdaki komutu çalıştırın:

   ```
   mvn package docker:build -DpushImage 
   ```

1. İsteğe bağlı: Gözat [Azure portal] ve Docker kapsayıcısı görüntü adlı olduğundan emin olun **yay önyükleme docker gs** kapsayıcı kayıt defterinizde.

   ![Azure portalında kapsayıcı doğrulayın][CR01]

## <a name="customize-your-pomxml-then-build-and-deploy-your-container-to-azure"></a>Pom.xml özelleştirme sonra derleme ve Azure'a, kapsayıcı dağıtma

Açık `pom.xml` dosyasını bir metin düzenleyicisinde yay önyükleme uygulamanız için ve bulun `<plugin>` öğesi için `azure-webapp-maven-plugin`. Bu öğe aşağıdaki örneğe benzemelidir:

   ```xml
   <plugin>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-webapp-maven-plugin</artifactId>
      <version>0.1.3</version>
      <configuration>
         <authentication>
            <serverId>azure-auth</serverId>
         </authentication>
         <resourceGroup>wingtiptoysresources</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
            <registryUrl>https://${docker.image.prefix}</registryUrl>
            <serverId>${azure.containerRegistry}</serverId>
         </containerSettings>
         <appSettings>
            <property>
               <name>PORT</name>
               <value>8080</value>
            </property>
         </appSettings>
      </configuration>
   </plugin>
   ```

Maven eklentisi için değiştirebileceğiniz değerlerden vardır ve bu öğelerin her biri için ayrıntılı bir açıklama kullanılabilir [Azure Web uygulamaları için Maven eklentisi] belgeleri. Denirse, bu makaledeki vurgulama değer olan birkaç değerleri vardır:

Öğesi | Açıklama
---|---|---
`<version>` | Sürümünü belirtir [Azure Web uygulamaları için Maven eklentisi]. Listelenen sürüm denetlemelisiniz [Maven merkezi depo](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) en son sürümünü kullandığınızdan emin olmak için.
`<authentication>` | İçerir, bu örnekte, Azure için kimlik bilgilerini belirtir bir `<serverId>` içeren öğeyi `azure-auth`; Maven Azure hizmet sorumlusu değerleri, Maven içinde aramak için bu değeri kullanır *settings.xml* bu makalenin önceki bölümde tanımlanan dosya.
`<resourceGroup>` | Olan hedef kaynak grubu belirtir `wingtiptoysresources` Bu örnekte. Kaynak grubu, zaten yoksa, dağıtım sırasında oluşturulur.
`<appName>` | Web uygulamanız için bir hedef adı belirtir. Bu örnekte, hedef addır `maven-linux-app-${maven.build.timestamp}`, burada `${maven.build.timestamp}` soneki çakışmayı önlemek için bu örnekte eklenir. (Zaman damgasını isteğe bağlıdır; uygulama adı için benzersiz bir dize belirtin.)
`<region>` | Olan bu örnekte hedef bölgesini belirtir `westus`. (Tam bir liste olarak [Azure Web uygulamaları için Maven eklentisi] belgelerine.)
`<containerSettings>` | Ad ve, kapsayıcı URL'sini içeren özellikleri belirtir.
`<appSettings>` | Web uygulamanızı Azure'a dağıtırken kullanılacak Maven benzersiz ayarlarını belirtir. Bu örnekte, bir `<property>` öğesi içeren bir ad/değer çifti alt öğelerinin uygulamanız için bağlantı noktasını belirtin.

> [!NOTE]
>
> Bu örnekte bağlantı noktası numarasını değiştirmek için ayarları, yalnızca varsayılan bağlantı noktası değiştirilirken gereklidir.
>

1. Komut istemi ya da daha önce kullandığınız terminal penceresi herhangi bir değişiklik yaptıysanız Maven kullanarak JAR dosyasını yeniden *pom.xml* dosya; örneğin:
   ```shell
   mvn clean package
   ```

1. Web uygulamanızı Maven kullanarak Azure'a dağıtma; Örneğin:
   ```shell
   mvn azure-webapp:deploy
   ```

Maven web uygulamanızı Azure'a dağıtacak; web uygulaması zaten mevcut değilse oluşturulur.

> [!NOTE]
>
> Varsa, belirttiğiniz bölge `<region>` öğesi, *pom.xml* dağıtımınızı başlattığınızda dosya sunucuları yeterli yok, aşağıdaki örneğe benzer bir hata görebilirsiniz:
>
> ```
> [INFO] Start deploying to Web App maven-linux-app-20170804...
> [INFO] ------------------------------------------------------------------------
> [INFO] BUILD FAILURE
> [INFO] ------------------------------------------------------------------------
> [INFO] Total time: 31.059 s
> [INFO] Finished at: 2017-08-04T12:15:47-07:00
> [INFO] Final Memory: 51M/279M
> [INFO] ------------------------------------------------------------------------
> [ERROR] Failed to execute goal com.microsoft.azure:azure-webapp-maven-plugin:0.1.3:deploy (default-cli) on project gs-spring-boot-docker: null: MojoExecutionException: CloudException: OnError while emitting onNext value: retrofit2.Response.class
> ```
>
> Bu durumda, başka bir bölge belirtin ve uygulamanızı dağıtmak için Maven komutunu yeniden çalıştırın.
>
>

Web dağıtıldığında, kullanarak yönetmek kullanamazsınız [Azure portal].

* Web uygulamanızı listelenen **uygulama hizmetleri**:

   ![Azure portalında uygulama hizmetleri listelenen web uygulaması][AP01]

* Web uygulamanız için URL içinde listelenen ve **genel bakış** web uygulamanız için:

   ![Web uygulamanız için URL belirleme][AP02]

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede ele alınan çeşitli teknolojiler hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Web uygulamaları için Maven eklentisi]

* [Oturum açmak için Azure Azure CLI üzerinden](/azure/xplat-cli-connect)

* [Bir Azure hizmet sorumlusu Azure CLI 2.0 ile oluşturun.](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven ayarları başvurusu](https://maven.apache.org/settings.html)

* [Maven için docker eklentisi]

<!-- URL List -->

[Azure komut satırı arabirimi (CLI)]: /cli/azure/overview
[Azure Container Service (ACS)]: https://azure.microsoft.com/services/container-service/
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Azure Web uygulamaları için Maven eklentisi]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin
[Create a private Docker container registry using the Azure portal]: /azure/container-registry/container-registry-get-started-portal
[Using a custom Docker image for Azure Web App on Linux]: tutorial-custom-docker-image.md
[Docker]: https://www.docker.com/
[Maven için docker eklentisi]: https://github.com/spotify/docker-maven-plugin
[ücretsiz Azure hesabı]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN abone Avantajlarınızı]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[yay önyükleme]: http://projects.spring.io/spring-boot/
[Docker Başlarken yay önyüklemede]: https://github.com/spring-guides/gs-spring-boot-docker
[Yay Framework]: https://spring.io/

<!-- IMG List -->

[SB01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/SB01.png
[CR01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/CR01.png
[AP01]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-spring-boot-app-from-container-registry-using-maven-plugin/AP02.png
