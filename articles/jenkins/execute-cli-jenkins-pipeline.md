---
title: Jenkins ile Azure CLI yürütme | Microsoft Docs
description: Azure Jenkins ardışık düzeninde bir Java web uygulaması dağıtmak için Azure CLI kullanmayı öğrenin
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
ms.openlocfilehash: 2b568bd22858a42178e2821e0e97a3b4ebdfccd5
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
ms.locfileid: "28926939"
---
# <a name="deploy-to-azure-app-service-with-jenkins-and-the-azure-cli"></a>Jenkins ve Azure CLI ile Azure App Service'e dağıtma
Java web uygulaması Azure'a dağıtmak için Azure CLI kullanabileceğiniz [Jenkins ardışık düzen](https://jenkins.io/doc/book/pipeline/). Bu öğreticide, aşağıdakileri öğrenerek bir Azure sanal makinesinde CI/CD işlem hattı oluşturursunuz:

> [!div class="checklist"]
> * Jenkins sanal makinesi oluşturma
> * Jenkins’i Yapılandırma
> * Bir web uygulaması oluşturma
> * GitHub depo hazırlama
> * Jenkins işlem hattı oluşturma
> * Ardışık Düzen çalıştırın ve web uygulaması doğrulayın

Bu öğretici, Azure CLI 2.0.4 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükseltme gerekiyorsa, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-and-configure-jenkins-instance"></a>Oluşturma ve yapılandırma Jenkins örneği
Jenkins asıl zaten yoksa başlayın [çözüm şablonu](install-jenkins-solution-template.md), gerekli içeren [Azure kimlik bilgilerini](https://plugins.jenkins.io/azure-credentials) varsayılan eklentisi. 

Azure kimlik bilgisi eklentisi, Microsoft Azure hizmet asıl kimlik Jenkins içinde depolamanızı sağlar. Bu Jenkins ardışık düzen Azure kimlik sınıflandırıp sürüm 1.2 biz desteği eklendi. 

1.2 veya sonraki bir sürümü olduğundan emin olun:
* Jenkins Pano içinde tıklatın **Jenkins Yönet -> eklentisi Yöneticisi ->** arayın ve **Azure kimlik bilgisi**. 
* Sürüm 1.2 eski ise eklentisi güncelleştirin.

Ayrıca Java JDK ve Maven Jenkins asıl gereklidir. Yüklemek için Jenkins asıl SSH kullanarak oturum açın ve aşağıdaki komutları çalıştırın:
```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

## <a name="add-azure-service-principal-to-jenkins-credential"></a>Azure hizmet sorumlusu için Jenkins kimlik bilgileri ekleme

Bir Azure kimlik bilgileri, Azure CLI yürütmek için gereklidir.

* Jenkins Pano içinde tıklatın **kimlik bilgileri -> Sistem ->**. Tıklatın **genel credentials(unrestricted)**.
* Tıklatın **kimlik bilgilerini eklemek** eklemek için bir [Microsoft Azure hizmet sorumlusu](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) abonelik kimliği, istemci kimliği, gizli ve OAuth 2.0 belirteç uç noktası doldurarak. Sonraki adımda kullanmak için bir kimlik belirtin.

![Kimlik bilgileri Ekle](./media/execute-cli-jenkins-pipeline/add-credentials.png)

## <a name="create-an-azure-app-service-for-deploying-the-java-web-app"></a>Java web uygulaması dağıtmak için bir Azure uygulama hizmeti oluşturma

İle bir Azure uygulama hizmeti planı oluştur **serbest** fiyatlandırma katmanı kullanarak [az uygulama hizmeti planı oluşturma](/cli/azure/appservice/plan#az_appservice_plan_create) CLI komutu. Uygulama hizmeti planı, uygulamalarınızı barındırmak için kullanılan fiziksel kaynakları tanımlar. Bir uygulama hizmeti planı atanan tüm uygulamalar maliyet birden fazla uygulama barındırdığında kaydetmenize izin vererek, bu kaynakları paylaşır. 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

Plan hazır olduğunda, Azure CLI aşağıdaki örneğe benzer çıkış şunları gösterir:

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

### <a name="create-an-azure-web-app"></a>Bir Azure Web uygulaması oluşturma

 Kullanım [az webapp oluşturma](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) bir web uygulaması tanımı oluşturmak için CLI komutu `myAppServicePlan` uygulama hizmeti planı. Web uygulaması tanımı uygulamanızla erişmek için bir URL sağlar ve kodunuzu Azure'a dağıtmak için çeşitli seçenekler yapılandırır. 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

Yedek `<app_name>` kendi benzersiz uygulama adıyla yer tutucu. Adının Azure içinde tüm uygulamalar arasında benzersiz olması gerekir böylece bu benzersiz bir ad web uygulaması için varsayılan etki alanı adı bir parçasıdır. Kullanıcılarınız için kullanıma önce web uygulaması için bir özel etki alanı adı girişi eşleyebilirsiniz.

Web uygulaması tanımı hazır olduğunda, Azure CLI bilgileri aşağıdaki örneğe benzer şekilde gösterir: 

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

### <a name="configure-java"></a>Java yapılandırın 

İle uygulamanızın gerektiğini Java Çalışma zamanı yapılandırma ayarlama [az appservice web yapılandırma güncelleştirmesi](/cli/azure/appservice/web/config#az_appservice_web_config_update) komutu.

Aşağıdaki komut, yeni bir Java 8 JDK üzerinde çalıştırmak için web uygulaması yapılandırır ve [Apache Tomcat](http://tomcat.apache.org/) 8.0.

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

## <a name="prepare-a-github-repository"></a>GitHub depo hazırlama
Açık [basit Java Web uygulaması için Azure](https://github.com/azure-devops/javawebappsample) deposu. Kendi GitHub hesabınızda depoyu çatallaştırma için tıklatın **çatalı** sağ üst köşesindeki düğmesi.

* GitHub web kullanıcı Arabirimi, açık **Jenkinsfile** dosya. Web uygulamanızı 20 ve 21 satırındaki adını ve kaynak grubu sırasıyla güncelleştirmek için bu dosyayı düzenlemek için Kalem simgesine tıklayın.

```java
def resourceGroup = '<myResourceGroup>'
def webAppName = '<app_name>'
```

* Satır kimlik bilgisi kimliği Jenkins Örneğinizde güncelleştirmek için 23 değiştirme

```java
withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
```

## <a name="create-jenkins-pipeline"></a>Jenkins işlem hattı oluşturma
Jenkins bir web tarayıcısında açın, **yeni öğe**. 

* Seçin ve iş için bir ad **ardışık düzen**. **Tamam**’a tıklayın.
* Tıklatın **ardışık düzen** sekmesinde sonraki. 
* İçin **tanımı**seçin **kanal SCM betikten**.
* İçin **SCM**seçin **Git**.
* Forked, depodaki için GitHub URL'yi girin: https:\<forked repo\>.git
* Tıklatın **Kaydet**

## <a name="test-your-pipeline"></a>İşlem hattınızı test etme
* Oluşturduğunuz ardışık düzene gidin, tıklatın **şimdi derleme**
* Birkaç saniye içinde bir yapı başarılı olması gerekir ve yapı gidin ve tıklayın **konsol çıktısı** ayrıntıları görmek için

## <a name="verify-your-web-app"></a>Web uygulamanızı doğrulayın
WAR doğrulamak için dosyası, web uygulamanızın başarıyla dağıtılır. Bir web tarayıcısı açın:

* Http:// gidin&lt;app_name >.azurewebsites.net/api/calculator/ping  
Görürsünüz:

        Welcome to Java Web App!!! This is updated!
        Sun Jun 17 16:39:10 UTC 2017

* Http:// gidin&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (alternatif &lt;x > ve &lt;y > sayılar içeren) x toplamını almak için ve y

![Hesaplayıcı: Ekle](./media/execute-cli-jenkins-pipeline/calculator-add.png)

## <a name="deploy-to-azure-web-app-on-linux"></a>Linux üzerinde Azure Web uygulamasına dağıtma
Jenkins hattınızı Azure CLI kullanma bildiğinize göre Azure Web uygulaması Linux'ta dağıtmak için komut dosyasını değiştirebilirsiniz.

Web uygulaması Linux'ta Docker kullanmaktır dağıtım yapmak için farklı bir şekilde destekler. Dağıtmak için web uygulamanızı hizmet çalışma zamanı Docker görüntüsüne paketleri bir Dockerfile sağlamanız gerekir. Eklenti sonra görüntüsünü oluşturabilirsiniz, Docker kayıt defterine anında ve görüntü web uygulamanızı dağıtın.

* Adımları [burada](../app-service/containers/quickstart-nodejs.md) Linux üzerinde çalışan bir Azure Web uygulaması oluşturma.
* Bu yönergeleri izleyerek Jenkins örneğinde Docker yükleme [makale](https://docs.docker.com/engine/installation/linux/ubuntu/).
* Bir kapsayıcı kayıt adımları kullanarak Azure portalında oluşturmak [burada](/azure/container-registry/container-registry-get-started-azure-cli).
* Aynı [basit Java Web uygulaması için Azure](https://github.com/azure-devops/javawebappsample) repo çatallanmış, düzenleme **Jenkinsfile2** dosyası:
    * 18-21 satır, kaynak grubu, web uygulaması ve ACR adlarını sırasıyla güncelleştirin. 
        ```
        def webAppResourceGroup = '<myResourceGroup>'
        def webAppName = '<app_name>'
        def acrName = '<myRegistry>'
        ```

    * Satır 24, güncelleştirme \<azsrvprincipal\> kimlik bilgisi kimliği
        ```
        withCredentials([azureServicePrincipal('<mySrvPrincipal>')]) {
        ```

* Windows Azure web uygulaması için yalnızca bu kez dağıtırken kullandığınız gibi yeni Jenkins işlem hattı oluşturma **Jenkinsfile2** yerine.
* Yeni işinizi çalıştırmak.
* , Azure CLI'da doğrulamak için çalıştırın:

    ```
    az acr repository list -n <myRegistry> -o json
    ```

    Aşağıdaki sonucu alın:
    
    ```
    [
    "calculator"
    ]
    ```
    
    Http:// gidin&lt;app_name >.azurewebsites.net/api/calculator/ping. İletiyi görürsünüz: 
    
        Welcome to Java Web App!!! This is updated!
        Sun Jul 09 16:39:10 UTC 2017

    Http:// gidin&lt;app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y > (alternatif &lt;x > ve &lt;y > sayılar içeren) x toplamını almak için ve y
    
## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, GitHub deposuna kaynak kodunda kullanıma Jenkins ardışık yapılandırılmış. War dosyasını oluşturmak için Maven çalışır ve Azure App Service'e dağıtmak için Azure CLI kullanır. Şunları öğrendiniz:

> [!div class="checklist"]
> * Jenkins sanal makinesi oluşturma
> * Jenkins’i Yapılandırma
> * Bir web uygulaması oluşturma
> * GitHub depo hazırlama
> * Jenkins işlem hattı oluşturma
> * Ardışık Düzen çalıştırın ve web uygulaması doğrulayın
