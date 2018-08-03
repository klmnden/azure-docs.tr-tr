---
title: Azure CLI ile Jenkins yürütme | Microsoft Docs
description: Jenkins işlem hattındaki azure'a bir Java web uygulaması dağıtmak için Azure CLI'yı kullanmayı öğrenin
services: app-service\web
documentationcenter: ''
author: mlearned
manager: douge
editor: ''
ms.assetid: ''
ms.service: jenkins
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 1796e9f76e39334c8bbdd03463a0f91e9b47cb17
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421313"
---
# <a name="deploy-to-azure-app-service-with-jenkins-and-the-azure-cli"></a>Jenkins ve Azure CLI ile Azure App Service'e dağıtma
Bir Java web uygulamasını Azure'a dağıtmak için Azure CLI'yi kullanabilirsiniz [Jenkins işlem hattı](https://jenkins.io/doc/book/pipeline/). Bu öğreticide, aşağıdakileri öğrenerek bir Azure sanal makinesinde CI/CD işlem hattı oluşturursunuz:

> [!div class="checklist"]
> * Jenkins sanal makinesi oluşturma
> * Jenkins’i yapılandırma
> * Azure'da bir web uygulaması oluşturma
> * GitHub depo hazırlama
> * Jenkins işlem hattı oluşturma
> * İşlem hattı çalıştırma ve web uygulaması doğrulayın

Bu öğretici, Azure CLI 2.0.4 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltme gerekiyorsa, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-and-configure-jenkins-instance"></a>Oluşturma ve yapılandırma Jenkins örneği
Bir Jenkins ana zaten yoksa başlayın [çözüm şablonu](install-jenkins-solution-template.md), gerekli içeren [Azure kimlik bilgileri](https://plugins.jenkins.io/azure-credentials) varsayılan eklenti. 

Azure kimlik bilgisi eklentisi, Microsoft Azure hizmet sorumlusu kimlik bilgilerinin Jenkins'te depolanmasına olanak tanır. Sürüm 1.2 Azure kimlik bilgileri, Jenkins işlem hattı çalıştırmasını alabilmeniz için destek ekledik. 

1.2 veya sonraki bir sürümü bulunduğundan emin olun:
* Jenkins panosunda tıklayın **Jenkins'i Yönet -> Eklenti Yöneticisi ->** araması **Azure kimlik bilgileri**. 
* Eklenti sürüm 1.2 önceyse güncelleştirin.

Ayrıca, Java JDK ve Maven Jenkins ana gereklidir. Yüklemek için SSH kullanarak Jenkins ana için oturum açın ve aşağıdaki komutları çalıştırın:
```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

## <a name="add-azure-service-principal-to-jenkins-credential"></a>Jenkins kimlik bilgisi ile Azure hizmet sorumlusu ekleme

Bir Azure kimlik bilgisi, Azure CLI'yı çalıştırmak için gereklidir.

* Jenkins panosunda tıklayın **kimlik bilgileri -> Sistem ->**. Tıklayın **genel credentials(unrestricted)**.
* Tıklayın **kimlik bilgileri ekleme** eklemek için bir [Microsoft Azure hizmet sorumlusu](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) abonelik kimliği, istemci kimliği, istemci gizli anahtarı ve OAuth 2.0 belirteç uç noktası doldurarak. Sonraki adımda kullanmak üzere bir kimliği sağlayın.

![Kimlik bilgileri ekleme](./media/execute-cli-jenkins-pipeline/add-credentials.png)

## <a name="create-an-azure-app-service-for-deploying-the-java-web-app"></a>Java web uygulaması dağıtmak için bir Azure App Service oluştur

Bir Azure App Service planı oluşturun **ücretsiz** fiyatlandırma katmanını kullanarak [az appservice plan oluşturma](/cli/azure/appservice/plan#az-appservice-plan-create) CLI komutu. Uygulama hizmeti planı, uygulamalarınızı barındırmak için kullanılan fiziksel kaynakları tanımlar. Uygulama hizmeti planına atanan tüm uygulamalar bu kaynakları paylaşarak birden çok uygulamayı barındırırken, maliyetten tasarruf etmenize imkan sağlar. 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

Plan hazır olduğunda, Azure CLI aşağıdaki örnekte gösterilene benzer bir çıkış görüntüler:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a>Azure Web uygulaması oluşturma

 Kullanım [az webapp oluşturma](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) bir web uygulaması tanımı oluşturmak için CLI komutu `myAppServicePlan` App Service planı. Web uygulaması tanımı, uygulamanıza erişebilmek için bir URL sağlar ve çeşitli seçenekleri yapılandırarak kodunuzu Azure'a dağıtır. 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

`<app_name>` yer tutucusunu kendi benzersiz uygulama adınızla değiştirin. Bu benzersiz ad, web uygulamasının varsayılan etki alanı adının bir parçası olduğundan, adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. Uygulamanızı kullanıcılarınızın kullanımına sunmadan önce web uygulamasına bir özel etki alanı adı girdisi eşleyebilirsiniz.

Web uygulaması tanımı hazır olduğunda Azure CLI aşağıda yer alan örnekteki gibi bilgiler gösterir: 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
   ...
  < Output has been truncated for readability >
}
```

### <a name="configure-java"></a>Java'yı yapılandırma 

Uygulamanıza gereken Java Çalışma zamanı yapılandırmasını ayarlayın [az appservice web config update](/cli/azure/appservice/web/config#az-appservice-web-config-update) komutu.

Aşağıdaki komut web uygulamasını yeni Java 8 JDK ve [Apache Tomcat](http://tomcat.apache.org/) 8.0 üzerinde çalıştırılacak şekilde yapılandırır.

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a>GitHub depo hazırlama
Açık [Azure için basit bir Java Web uygulaması](https://github.com/azure-devops/javawebappsample) depo. GitHub hesabınıza depo çatalı oluşturma için tıklatın **çatal** sağ üst köşesindeki düğme.

* GitHub web kullanıcı Arabiriminde, açık **Jenkinsfile** dosya. Kaynak grubu ve 20 ve 21 satırında web uygulamanızın adına sırasıyla güncelleştirmek için bu dosyayı düzenlemek için Kalem simgesine tıklayın.

```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<app_name>'
```

* Jenkins Örneğinize içinde kimlik bilgileri güncelleştirmek için 23. satıra değiştirme

```java
withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
```

## <a name="create-jenkins-pipeline"></a>Jenkins işlem hattı oluşturma
Jenkins web tarayıcısında açın, **yeni öğe**. 

* Seçin ve iş için bir ad sağlayın **işlem hattı**. **Tamam** düğmesine tıklayın.
* Tıklayın **işlem hattı** sonraki sekme. 
* İçin **tanımı**seçin **işlem hattı SCM betikten**.
* İçin **SCM**seçin **Git**.
* Çatalı oluşturulan deponuzu için GitHub URL'sini girin: https:\<çatalı oluşturulan deponuzu\>.git
* **Kaydet**’e tıklayın

## <a name="test-your-pipeline"></a>İşlem hattınızı test etme
* ' A tıklayın, oluşturduğunuz işlem hattına gidin **şimdi Derle**
* Bir derleme, birkaç saniye içinde başarılı olması gerekir ve yapıya gidin ve tıklayın **konsol çıktısı** ayrıntıları görmek için

## <a name="verify-your-web-app"></a>Web uygulamanızı doğrulayın
WAR doğrulamak için dosya web uygulamanızı başarılı bir şekilde dağıtılır. Bir web tarayıcısı açın:

* Http:// Git&lt;app_name >.azurewebsites.net/api/calculator/ping  
Görürsünüz:

        Welcome to Java Web App!!! This is updated!
        Sun Jun 17 16:39:10 UTC 2017

* Http:// gidin&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (değiştirin &lt;x > ve &lt;y > sayılar içeren) x toplamını almak ve y

![Hesaplayıcı: Ekle](./media/execute-cli-jenkins-pipeline/calculator-add.png)

## <a name="deploy-to-azure-web-app-on-linux"></a>Linux üzerinde Azure Web uygulamasına dağıtma
Jenkins hattınızdaki Azure CLI'yı nasıl kullanılacağını, Linux üzerinde Azure Web uygulaması dağıtmak için komut dosyasını değiştirebilirsiniz.

Linux üzerinde Web App, Docker kullanmaktır dağıtım yapmak için farklı bir şekilde destekler. Dağıtmak için hizmet çalışma zamanı web uygulamanızla bir Docker görüntüsü halinde paketler bir Dockerfile'ı sağlamanız gerekir. Eklenti sonra görüntüyü oluşturmak, bir Docker kayıt defterine gönderin ve görüntüsünü web uygulamanıza dağıtın.

* Adımları [burada](../app-service/containers/quickstart-nodejs.md) Linux üzerinde çalışan bir Azure Web uygulaması oluşturmak için.
* Bu yönergeleri takip ederek, Jenkins Örneğinize Docker yükleme [makale](https://docs.docker.com/engine/installation/linux/ubuntu/).
* Adımları kullanarak Azure portalında kapsayıcı kayıt defteri oluşturma [burada](/azure/container-registry/container-registry-get-started-azure-cli).
* Aynı [Azure için basit bir Java Web uygulaması](https://github.com/azure-devops/javawebappsample) deponun çatal, Düzen **Jenkinsfile2** dosyası:
    * 18-21 satır, kaynak grubu, web uygulaması ve ACR adlarına sırasıyla güncelleştirin. 
        ```
        def webAppResourceGroup = '<myResourceGroup>'
        def webAppName = '<app_name>'
        def acrName = '<myRegistry>'
        ```

    * Satır 24, güncelleştirme \<azsrvprincipal\> kimlik bilgisi kimliğinizin
        ```
        withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
        ```

* Yalnızca bu kez, Windows, Azure web uygulamasına dağıtırken kullandığınız gibi yeni Jenkins işlem hattı oluşturma **Jenkinsfile2** yerine.
* Yeni iş çalıştırın.
* Doğrulanacak Azure CLI'da çalıştırın:

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    Aşağıdaki sonucu alın:
    
    ```
    [
    "calculator"
    ]
    ```
    
    Http:// Git&lt;app_name >.azurewebsites.net/api/calculator/ping. İletisini görürsünüz: 
    
        Welcome to Java Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    Http:// gidin&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (değiştirin &lt;x > ve &lt;y > sayılar içeren) x toplamını almak ve y
    
## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, GitHub deposunu kaynak kodunda kullanıma bir Jenkins işlem hattı yapılandırılmış. Bir war dosyası oluşturmak için Maven'ı çalıştıran ve Azure App Service'e dağıtmak için Azure CLI'yı kullanır. Şunları öğrendiniz:

> [!div class="checklist"]
> * Jenkins sanal makinesi oluşturma
> * Jenkins’i yapılandırma
> * Azure'da bir web uygulaması oluşturma
> * GitHub depo hazırlama
> * Jenkins işlem hattı oluşturma
> * İşlem hattı çalıştırma ve web uygulaması doğrulayın
