---
title: Linux üzerindeki Azure App Service’te Java web uygulaması oluşturma hızlı başlangıcı
description: Bu hızlı başlangıçta, Linux üzerindeki Azure App Service’te ilk Java Merhaba Dünya uygulamanızı birkaç dakika içinde dağıtacaksınız.
services: app-service\web
documentationcenter: ''
author: msangapu
manager: cfowler
editor: ''
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: quickstart
ms.date: 03/07/2018
ms.author: msangapu
ms.custom: mvc
ms.openlocfilehash: 2018f5b7051f2b6906372dad3319c763974b93b1
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34355194"
---
# <a name="quickstart-create-a-java-web-app-in-app-service-on-linux"></a>Hızlı Başlangıç: Linux üzerindeki App Service’te Java web uygulaması oluşturma

Linux üzerinde App Service şu anda Java web uygulamalarını desteklemek için bir önizleme özelliği sunmaktadır. Önizlemeler hakkında daha fazla bilgi için lütfen [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)’nı inceleyin. 

[Linux üzerindeki App Service](app-service-linux-intro.md) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. Bu hızlı başlangıçta, [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)’yı [Azure Web Apps için Maven Eklentisi (Önizleme)](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) ile birlikte kullanarak yerleşik bir Linux görüntüsü ile bir Java web uygulamasını dağıtma işlemi gösterilmektedir.

![Azure'da çalışan örnek uygulama](media/quickstart-java/java-hello-world-in-browser.png)

[IntelliJ için Azure Araç Setini kullanarak Java web uygulamalarını buluttaki bir Linux kapsayıcısına dağıtmak](https://docs.microsoft.com/java/azure/intellij/azure-toolkit-for-intellij-hello-world-web-app-linux), Java uygulamanızı kendi kapsayıcınıza dağıtmanın alternatif bir yoludur.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için: 

* Yerel olarak yüklü [Azure CLI 2.0 veya üzeri](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* [Apache Maven](http://maven.apache.org/).



## <a name="create-a-java-app"></a>Java uygulaması oluşturma

Yeni bir *helloworld* web uygulaması oluşturmak için Maven kullanarak aşağıdaki komutu yürütün:  

    mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DarchetypeArtifactId=maven-archetype-webapp

Yeni *helloworld* proje dizinine geçin ve aşağıdaki komutu kullanarak tüm modüllerin derleyin:

    mvn verify

Bu komut, *helloworld/target* alt dizinindeki *helloworld.war* dosyası dahil olmak üzere tüm modülleri doğrulayıp oluşturur.


## <a name="deploying-the-java-app-to-app-service-on-linux"></a>Java uygulamasını Linux üzerinde App Service'e dağıtma

Java web uygulamalarınızı Linux üzerinde App Service’e dağıtmak için birden fazla dağıtım seçeneği mevcuttur. Bu seçenekler şunlardır:

* [Azure Web Apps için Maven Eklentisi ile dağıtma](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin)
* [ZIP veya WAR aracılığıyla dağıtma](https://docs.microsoft.com/azure/app-service/app-service-deploy-zip)
* [FTP aracılığıyla dağıtma](https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp)

Bu hızlı başlangıçta, Azure web uygulamaları için Maven eklentisini kullanacaksınız. Maven’a göre kullanımının kolay olması ve sizin için gerekli Azure kaynaklarını (kaynak grubu, app service planı ve web uygulaması) oluşturması bakımından avantajları vardır.

### <a name="deploy-with-maven"></a>Maven ile dağıtma

Maven’dan dağıtmak için *pom.xml* dosyasındaki `<build>` öğesinin içinde bulunan aşağıdaki eklenti tanımını ekleyin:

```xml
    <plugins>
      <plugin>
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-webapp-maven-plugin</artifactId> 
        <version>1.1.0</version>
        <configuration> 
          <resourceGroup>YOUR_RESOURCE_GROUP</resourceGroup> 
          <appName>YOUR_WEB_APP</appName> 
          <linuxRuntime>tomcat 9.0-jre8</linuxRuntime>
          <deploymentType>ftp</deploymentType> 
          <resources> 
              <resource> 
                  <directory>${project.basedir}/target</directory> 
                  <targetPath>webapps</targetPath> 
                  <includes> 
                      <include>*.war</include> 
                  </includes> 
                  <excludes> 
                      <exclude>*.xml</exclude> 
                  </excludes> 
              </resource> 
          </resources> 
        </configuration>
      </plugin>
    </plugins>
```    

Eklenti yapılandırmasında aşağıdaki yer tutucuları güncelleştirin:

| Yer tutucu | Açıklama |
| ----------- | ----------- |
| `YOUR_RESOURCE_GROUP` | Web uygulamanızın oluşturulacağı yeni kaynak grubunun adı. Uygulamanın tüm kaynaklarını bir gruba koyarak birlikte yönetebilirsiniz. Örneğin, kaynak grubunu sildiğinizde uygulamayla ilişkili tüm kaynaklar da silinir. Bu değeri *TestResources* gibi benzersiz bir yeni kaynak grubu adı ile güncelleştirin. Bu kaynak grubunu daha sonraki bir bölümde tüm Azure kaynaklarını temizlemek için kullanacaksınız. |
| `YOUR_WEB_APP` | Uygulama adı, Azure’a dağıtıldığında web uygulamasının ana bilgisayar adının bir parçası olacaktır (YOUR_WEB_APP.azurewebsites.net). Bu değeri, Java uygulamanızı barındıracak yeni Azure web uygulamanızın benzersiz adıyla (*contoso* gibi) değiştirin. |

Yapılandırmanın `linuxRuntime` öğesi, uygulamanızla birlikte hangi yerleşik Linux görüntüsünün kullanıldığını denetler.

Azure CLI ile kimlik doğrulaması yapmak için aşağıdaki komutu yürütün ve tüm yönergeleri izleyin:

    az login

Aşağıdaki komutu kullanarak Java uygulamanızı web uygulamasına dağıtın:

    mvn clean package azure-webapp:deploy


Dağıtım tamamlandıktan sonra, web tarayıcınızda aşağıdaki URL’yi kullanarak dağıtılan uygulamanın konumuna göz atın.

```bash
http://<app_name>.azurewebsites.net/helloworld
```

Java örnek kodu bir web uygulaması yerleşik görüntüsünde çalışır.

![Azure'da çalışan örnek uygulama](media/quickstart-java/java-hello-world-in-browser-curl.png)

**Tebrikler!** Linux üzerinde App Service’e ilk Java uygulamanızı dağıttınız.


[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Maven kullanarak bir Java web uygulaması oluşturdunuz, ardından Java web uygulamasını Linux üzerindeki App Service’e dağıttınız. Azure ile Java kullanma hakkında daha fazla bilgi almak için aşağıdaki bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Java Geliştiricileri için Azure](https://docs.microsoft.com/java/azure/)

