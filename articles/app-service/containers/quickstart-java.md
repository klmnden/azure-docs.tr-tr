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
ms.openlocfilehash: b9e8d2b9eacfa5c427ffe3f27ea99bbd35651d57
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47165985"
---
# <a name="quickstart-create-a-java-web-app-in-app-service-on-linux"></a>Hızlı Başlangıç: Linux üzerindeki App Service’te Java web uygulaması oluşturma

[Linux üzerindeki App Service](app-service-linux-intro.md) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. Bu hızlı başlangıç, [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)'nın [Azure Web Uygulamaları için Maven Eklentisi (Önizleme)](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) bileşeni ile bir Java Web uygulaması Web arşiv (WAR) dosyası dağıtmak için nasıl kullanılacağını göstermektedir.

![Azure'da çalışan örnek uygulama](media/quickstart-java/java-hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Java uygulaması oluşturma

`helloworld` adlı yeni bir Web uygulaması oluşturmak için Cloud Shell isteminde aşağıdaki Maven komutunu yürütün:

```bash
mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DarchetypeArtifactId=maven-archetype-webapp
```

## <a name="configure-the-maven-plugin"></a>Maven eklentisini yapılandırma

Maven'dan dağıtım için, Cloud Shell'deki kod düzenleyiciyi kullanarak `helloworld` dizinindeki proje `pom.xml` dosyasını açın. 

```bash
code pom.xml
```

Sonra `pom.xml` dosyasının `<build>` öğesinin içine aşağıdaki eklenti tanımını ekleyin.

```xml
<plugins>
    <!--*************************************************-->
    <!-- Deploy to Tomcat in App Service Linux           -->
    <!--*************************************************-->
      
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.4.0</version>
        <configuration>
   
            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
   
            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>tomcat 8.5-jre8</linuxRuntime>
   
        </configuration>
    </plugin>
</plugins>
```    


> [!NOTE] 
> Bu makalede yalnızca WAR dosyalarıyla paketlenmiş Java uygulamalarıyla çalışacağız. Eklenti ayrıca JAR web uygulamalarını da destekler. Bu uygulamalar için aşağıdaki alternatif eklenti tanımını kullanın. Bu yapılandırma Maven ile derlenmiş bir JAR dosyasını yerel dosya sisteminizde `${project.build.directory}/${project.build.finalName}.jar` konumuna dağıtır.
>
>```xml
><plugin>
>            <groupId>com.microsoft.azure</groupId>
>            <artifactId>azure-webapp-maven-plugin</artifactId>
>            <version>1.4.0</version>
>            <configuration>
>                <deploymentType>jar</deploymentType>
>
>           <!-- Web App information -->
>            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
>            <appName>${WEBAPP_NAME}</appName>
>            <region>${REGION}</region>  
>
>                <!-- Java Runtime Stack for Web App on Linux-->
>                <linuxRuntime>jre8</linuxRuntime>
>            </configuration>
>         </plugin>
>```    


Eklenti yapılandırmasında aşağıdaki yer tutucuları güncelleştirin:

| Yer tutucu | Açıklama |
| ----------- | ----------- |
| `RESOURCEGROUP_NAME` | Web uygulamanızın oluşturulacağı yeni kaynak grubunun adı. Uygulamanın tüm kaynaklarını bir gruba koyarak birlikte yönetebilirsiniz. Örneğin, kaynak grubunu sildiğinizde uygulamayla ilişkili tüm kaynaklar da silinir. Bu değeri *TestResources* gibi benzersiz bir yeni kaynak grubu adı ile güncelleştirin. Bu kaynak grubunu daha sonraki bir bölümde tüm Azure kaynaklarını temizlemek için kullanacaksınız. |
| `WEBAPP_NAME` | Uygulama adı, Azure’a dağıtıldığında web uygulamasının konak adının parçası haline gelir (UYGULAMA_ADI.azurewebsites.net). Bu değeri, Java uygulamanızı barındıracak yeni Azure web uygulamanızın benzersiz adıyla (*contoso* gibi) değiştirin. |
| `REGION` | Web uygulamasının barındırıldığı bir Azure bölgesi; örneğin `westus2`. Cloud Shell'den veya CLI'dan `az account list-locations` komutunu kullanarak bölgelerin bir listesini alabilirsiniz. |

## <a name="deploy-the-app"></a>Uygulamayı dağıtma

Aşağıdaki komutu kullanarak Java uygulamanızı Azure'a dağıtın:

```bash
mvn package azure-webapp:deploy
```

Dağıtım tamamlandıktan sonra, web tarayıcınızda aşağıdaki URL’yi kullanarak dağıtılan uygulamanın konumuna gidin; örneğin `http://<webapp>.azurewebsites.net/helloworld`. 

![Azure'da çalışan örnek uygulama](media/quickstart-java/java-hello-world-in-browser-curl.png)

**Tebrikler!** Linux üzerinde App Service’e ilk Java uygulamanızı dağıttınız.


[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Java uygulaması oluşturmak için Maven kullandınız, [Azure Web Apps için Maven Eklentisi (Önizleme)](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) bileşenini yapılandırdınız, ardından Web arşiviyle paketlenmiş bir Java Web uygulamasını Linux üzerinde App Service'e dağıttınız. Azure ile Java kullanma hakkında daha fazla bilgi almak için aşağıdaki bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Java Geliştiricileri için Azure](https://docs.microsoft.com/java/azure/)

