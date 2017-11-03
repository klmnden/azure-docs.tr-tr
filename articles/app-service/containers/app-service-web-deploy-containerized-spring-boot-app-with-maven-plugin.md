---
title: "Maven eklentisi kapsayıcıları için Web uygulaması için bir kapsayıcılı yay önyükleme uygulamayı Azure'a dağıtmak için nasıl kullanılacağını"
description: "Maven eklentisi kapsayıcıları için Web uygulaması için bir yay önyükleme uygulamayı Azure'a dağıtmak için nasıl kullanılacağını öğrenin."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/07/2017
ms.author: robmcm;kevinzha
ms.openlocfilehash: 0329aa9b88c7542ab3235a104a0652cd217ff872
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="how-to-use-the-maven-plugin-for-web-app-for-containers-to-deploy-a-containerized-spring-boot-app-to-azure"></a>Maven eklentisi kapsayıcıları için Web uygulaması için bir kapsayıcılı yay önyükleme uygulamayı Azure'a dağıtmak için nasıl kullanılacağını

[Kapsayıcıları için Web uygulaması için Maven eklentisi](https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin) için [Apache Maven](http://maven.apache.org/) Maven projelerine Azure App Service'in sorunsuz tümleştirme sağlar ve geliştiricilerin web uygulamalarını dağıtmak işlemini kolaylaştırır Azure uygulama hizmeti.

Bu makalede, Web uygulaması kapsayıcıları için Docker kapsayıcısı bir örnek yay önyükleme uygulama dağıtmak için Web uygulamasının kapsayıcıları için Maven eklentisi kullanarak gösterilmektedir.

> [!NOTE]
>
> Maven eklentisi kapsayıcıları için Web uygulaması için Önizleme olarak şu anda kullanılabilir değil. Ek özellikler gelecek için planlanan olsa da, şimdilik yalnızca FTP Yayımlama, desteklenir.
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

## <a name="clone-the-sample-spring-boot-application"></a>Örnek yay önyükleme uygulama kopyalama

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

1. [Docker yay önyüklemesini] Örnek Proje oluşturduğunuz dizine kopyalayın; Örneğin:
   ```shell
   git clone https://github.com/microsoft/gs-spring-boot-docker
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

## <a name="create-an-azure-service-principal"></a>Bir Azure hizmet sorumlusu oluşturma

Bu bölümde, Azure, kapsayıcı Azure'a dağıtırken Maven eklentisi kullanan hizmet sorumlusu oluşturun.

1. Bir komut istemi açın.

1. Azure CLI kullanarak Azure hesabınızda oturum açın:
   ```shell
   az login
   ```
   Oturum açma işlemini tamamlamak için yönergeleri izleyin.

1. Bir Azure hizmet sorumlusu oluşturun:
   ```shell
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

## <a name="configure-maven-to-use-your-azure-service-principal"></a>Azure hizmet asıl adı kullanmasını Maven yapılandırın

Bu bölümde, değerleri Azure hizmetinizden asıl Maven, kapsayıcı Azure'a dağıtırken kullanacağı kimlik doğrulamasını yapılandırmak için kullanın.

1. Maven açmak `settings.xml` dosyasını bir metin düzenleyicisinde; bu dosyayı aşağıdaki örneklerde gibi bir yol olabilir:
   * `/etc/maven/settings.xml`
   * `%ProgramFiles%\apache-maven\3.5.0\conf\settings.xml`
   * `$HOME/.m2/settings.xml`

1. Bu öğreticinin önceki bölümdeki Azure hizmet asıl ayarlarınızı ekleme `<servers>` koleksiyonunda *settings.xml* dosya; örneğin:

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
   `<environment>` | Olduğu hedef Azure bulut ortamı tanımlar `AZURE` Bu örnekte. (Ortamlar tam listesi kullanılabilir [kapsayıcıları için Web uygulaması için Maven eklentisi] belgeleri)

1. Kaydet ve Kapat *settings.xml* dosya.

## <a name="optional-deploy-your-local-docker-file-to-docker-hub"></a>İsteğe bağlı: yerel Docker dosyanızı Docker hub'a dağıtma

Docker hesabınız varsa, Docker kapsayıcısı görüntüsünü yerel olarak oluşturabilirsiniz ve Docker hub'a anında. Bunu yapmak için aşağıdaki adımları kullanın.

1. Açık `pom.xml` dosyasını bir metin düzenleyicisinde yay önyükleme uygulamanız için.

1. Bulun `<imageName>` alt öğesi olan `<containerSettings>` öğesi.

1. Güncelleştirme `${docker.image.prefix}` Docker hesap adınızı değerle:
   ```xml
   <containerSettings>
      <imageName>mydockeraccountname/${project.artifactId}</imageName>
   </containerSettings>
   ```

1. Aşağıdaki dağıtım yöntemlerinden birini seçin:

   * Kapsayıcı görüntünüzü Maven ile yerel olarak oluşturun ve sonra kapsayıcı Docker hub'a anında için Docker kullanın:
      ```shell
      mvn clean package docker:build
      docker push
      ```

   * Varsa [Maven için Docker eklentisi] yüklü, otomatik olarak oluşturabilir ve, kapsayıcı kullanarak Docker hub'a görüntü `-DpushImage` parametre:
      ```shell
      mvn clean package docker:build -DpushImage
      ```

## <a name="optional-customize-your-pomxml-before-deploying-your-container-to-azure"></a>İsteğe bağlı:, pom.xml, kapsayıcı Azure'a dağıtmadan önce özelleştirme

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
         <resourceGroup>maven-plugin</resourceGroup>
         <appName>maven-linux-app-${maven.build.timestamp}</appName>
         <region>westus</region>
         <containerSettings>
            <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
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

Maven eklentisi için değiştirebileceğiniz değerlerden vardır ve bu öğelerin her biri için ayrıntılı bir açıklama kullanılabilir [kapsayıcıları için Web uygulaması için Maven eklentisi] belgeleri. Denirse, bu makaledeki vurgulama değer olan birkaç değerleri vardır:

Öğesi | Açıklama
---|---|---
`<version>` | Sürümünü belirtir [kapsayıcıları için Web uygulaması için Maven eklentisi]. Listelenen sürüm denetlemelisiniz [Maven merkezi depo](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-webapp-maven-plugin%22) en son sürümünü kullandığınızdan emin olmak için.
`<authentication>` | İçerir, bu örnekte, Azure için kimlik bilgilerini belirtir bir `<serverId>` içeren öğeyi `azure-auth`; Maven Azure hizmet sorumlusu değerleri, Maven içinde aramak için bu değeri kullanır *settings.xml* bu makalenin önceki bölümde tanımlanan dosya.
`<resourceGroup>` | Olan hedef kaynak grubu belirtir `maven-plugin` Bu örnekte. Kaynak grubu, zaten yoksa, dağıtım sırasında oluşturulur.
`<appName>` | Web uygulamanız için bir hedef adı belirtir. Bu örnekte, hedef addır `maven-linux-app-${maven.build.timestamp}`, burada `${maven.build.timestamp}` soneki çakışmayı önlemek için bu örnekte eklenir. (Zaman damgasını isteğe bağlıdır; uygulama adı için benzersiz bir dize belirtin.)
`<region>` | Olan bu örnekte hedef bölgesini belirtir `westus`. (Tam bir liste olarak [kapsayıcıları için Web uygulaması için Maven eklentisi] belgelerine.)
`<appSettings>` | Web uygulamanızı Azure'a dağıtırken kullanılacak Maven benzersiz ayarlarını belirtir. Bu örnekte, bir `<property>` öğesi içeren bir ad/değer çifti alt öğelerinin uygulamanız için bağlantı noktasını belirtin.

> [!NOTE]
>
> Bu örnekte bağlantı noktası numarasını değiştirmek için ayarları, yalnızca varsayılan bağlantı noktası değiştirilirken gereklidir.
>

## <a name="build-and-deploy-your-container-to-azure"></a>Derleme ve Azure'a, kapsayıcı dağıtma

Tüm ayarlar, bu makalenin önceki bölümlerinde yapılandırdıktan sonra kapsayıcı Azure'a dağıtmak hazır olursunuz. Bunu yapmak için aşağıdaki adımları kullanın:

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
> ```bash
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

Bu makalede ele alınan çeşitli teknolojiler hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [kapsayıcıları için Web uygulaması için Maven eklentisi]

* [Oturum açmak için Azure Azure CLI üzerinden](/azure/xplat-cli-connect)

* [Maven eklentisi kapsayıcıları için Web uygulaması için Azure App Service Linux'ta yay önyükleme uygulama dağıtmak için nasıl kullanılacağını](../app-service-web-deploy-spring-boot-app-with-maven-plugin.md)

* [Bir Azure hizmet sorumlusu Azure CLI 2.0 ile oluşturun.](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven ayarları başvurusu](https://maven.apache.org/settings.html)

* [Maven için Docker eklentisi]

<!-- URL List -->

[Azure komut satırı arabirimi (CLI)]: /cli/azure/overview
[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Azure portal]: https://portal.azure.com/
[Docker]: https://www.docker.com/
[Maven için Docker eklentisi]: https://github.com/spotify/docker-maven-plugin
[ücretsiz Azure hesabı]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Java Developer Kit (JDK)]: http://www.oracle.com/technetwork/java/javase/downloads/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/
[Maven]: http://maven.apache.org/
[MSDN abone Avantajlarınızı]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot on Docker Getting Started]: https://github.com/spring-guides/gs-spring-boot-docker
[Spring Framework]: https://spring.io/
[kapsayıcıları için Web uygulaması için Maven eklentisi]: https://github.com/Microsoft/azure-maven-plugins/tree/master/azure-webapp-maven-plugin

<!-- IMG List -->

[AP01]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP01.png
[AP02]: ./media/app-service-web-deploy-containerized-spring-boot-app-with-maven-plugin/AP02.png
