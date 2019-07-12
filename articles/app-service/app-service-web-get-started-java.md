---
title: Windows - Azure App Service'te Java web uygulaması oluşturma
description: Bu hızlı başlangıçta, ilk Java Merhaba Dünya uygulamanızı Windows üzerinde Azure App Service'te dakikalar içinde dağıtın.
keywords: Azure app service, web uygulaması, windows, java, maven, hızlı başlangıç
services: app-service\web
documentationcenter: ''
author: msangapu-msft
manager: jeconnoc
editor: ''
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: quickstart
ms.date: 05/29/2019
ms.author: jafreebe
ms.custom: mvc
ms.openlocfilehash: 0ca50bae0748570932c7a4cc3bb10cde17c940f5
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67617719"
---
# <a name="quickstart-create-a-java-app-in-app-service"></a>Hızlı Başlangıç: App Service'te bir Java uygulaması oluşturma

> [!NOTE]
> Bu makalede bir uygulamanın Windows üzerinde App Service'e dağıtımı yapılır. App Service dağıtmak için _Linux_, bkz: [Linux üzerinde web uygulaması oluşturma Java](./containers/quickstart-java.md).
>

[Azure App Service](overview.md), yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar.  Bu hızlı başlangıçta nasıl kullanılacağını gösterir [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) ile [Azure App Service için Maven Plugin](https://github.com/Microsoft/azure-maven-plugins/tree/develop/azure-webapp-maven-plugin) bir Java web arşivi (WAR) dosyasına dağıtılacak.

> [!NOTE]
> Aynı şeyi de yapılabilir Intellij ve Eclipse gibi popüler Ide'leri kullanarak. Bizim benzer belgeleri kullanıma [Intellij Hızlı Başlangıç için Azure Araç Seti](/java/azure/intellij/azure-toolkit-for-intellij-create-hello-world-web-app) veya [Eclipse Hızlı Başlangıç için Azure Araç Seti](/java/azure/eclipse/azure-toolkit-for-eclipse-create-hello-world-web-app).
>
![Azure'da çalışan örnek uygulama](./media/app-service-web-get-started-java/java-hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-java-app"></a>Java uygulaması oluşturma

Cloud Shell isteminde adlı yeni bir uygulama oluşturmak için aşağıdaki Maven komutunu yürütün `helloworld`:

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
    <!-- Deploy to Tomcat in App Service Windows         -->
    <!--*************************************************-->
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.6.0</version>
        <configuration>
            <!-- Specify v2 schema -->
            <schemaVersion>v2</schemaVersion>
            <!-- App information -->
            <subscriptionId>${SUBSCRIPTION_ID}</subscriptionId>
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
            <!-- Java Runtime Stack for App Service on Windows-->
            <runtime>
                <os>windows</os>
                <javaVersion>1.8</javaVersion>
                <webContainer>tomcat 9.0</webContainer>
            </runtime>
            <deployment>
                <resources>
                    <resource>
                        <directory>${project.basedir}/target</directory>
                        <includes>
                            <include>*.war</include>
                        </includes>
                    </resource>
                </resources>
            </deployment>
        </configuration>
    </plugin>
</plugins>
```

> [!NOTE]
> Bu makalede yalnızca WAR dosyalarıyla paketlenmiş Java uygulamalarıyla çalışacağız. Eklenti ayrıca JAR web uygulamalarını da destekler. Denemek için [Linux'ta App Service'e Java SE JAR dosyası dağıtma](https://docs.microsoft.com/java/azure/spring-framework/deploy-spring-boot-java-app-with-maven-plugin?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json).


Eklenti yapılandırmasında aşağıdaki yer tutucuları güncelleştirin:

| Yer tutucu | Açıklama |
| ----------- | ----------- |
| `SUBSCRIPTION_ID` | Uygulamanıza dağıtmak istediğiniz abonelik benzersiz kimliği. Cloud Shell veya CLI kullanarak varsayılan aboneliğin kimliği bulunabilir `az account show` komutu. Kullanılabilir abonelikler tüm için kullanmak `az account list` komutu.|
| `RESOURCEGROUP_NAME` | Uygulamanızın oluşturulacağı yeni kaynak grubunun adı. Uygulamanın tüm kaynaklarını bir gruba koyarak birlikte yönetebilirsiniz. Örneğin, kaynak grubunu sildiğinizde uygulamayla ilişkili tüm kaynaklar da silinir. Bu değer bir benzersiz yeni kaynak grubu adı ile Örneğin, güncelleştirme *myResourceGroup*. Bu kaynak grubunu daha sonraki bir bölümde tüm Azure kaynaklarını temizlemek için kullanacaksınız. |
| `WEBAPP_NAME` | Uygulama adı (WEBAPP_NAME.azurewebsites.net) Azure'a dağıtırken uygulama için ana bilgisayar adı bölümü olacaktır. Bu değer Java uygulamanızı örneğin barındıracak yeni App Service uygulaması için benzersiz bir adla güncelleştirin *contoso*. |
| `REGION` | Uygulamanın barındırıldığı, örneğin bir Azure bölgesi *westus2*. Cloud Shell'den veya CLI'dan `az account list-locations` komutunu kullanarak bölgelerin bir listesini alabilirsiniz. |

## <a name="deploy-the-app"></a>Uygulamayı dağıtma

Aşağıdaki komutu kullanarak Java uygulamanızı Azure'a dağıtın:

```bash
mvn package azure-webapp:deploy
```

Dağıtım tamamlandıktan sonra, web tarayıcınızda aşağıdaki URL’yi kullanarak dağıtılan uygulamanın konumuna gidin; örneğin `http://<webapp>.azurewebsites.net/`.

![Azure'da çalışan örnek uygulama](./media/app-service-web-get-started-java/java-hello-world-in-browser.png)

**Tebrikler!** Windows üzerinde App Service'e ilk Java uygulamanızı dağıttınız.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure için Java geliştiricilerinin kaynakları](/java/azure/)

> [!div class="nextstepaction"]
> [Özel etki alanı eşleme](app-service-web-tutorial-custom-domain.md)
