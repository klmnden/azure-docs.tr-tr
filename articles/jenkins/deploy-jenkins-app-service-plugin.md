---
title: Jenkins eklentisi kullanarak Azure App Service'e dağıtma | Microsoft Docs
description: Java web uygulaması Jenkins Azure'da dağıtmak için Azure App Service Jenkins eklentisi kullanmayı öğrenin
services: app-service\web
documentationcenter: ''
author: mlearned
manager: douge
editor: ''
ms.assetid: ''
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 7/24/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 0128ad37e3ba66710279de42cf4eae0ce5431b5b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31418436"
---
# <a name="deploy-to-azure-app-service-by-using-the-jenkins-plugin"></a>Jenkins eklentisi kullanarak Azure App Service'e dağıtma 

Java web uygulaması Azure'a dağıtmak için Azure CLI kullanabileceğiniz [Jenkins ardışık düzen](/azure/jenkins/execute-cli-jenkins-pipeline) veya kullanabilirsiniz [Azure App Service Jenkins eklentisi](https://plugins.jenkins.io/azure-app-service). Eklentisi sürüm 1.0 Jenkins Azure App Service Web Apps özelliğini kullanarak sürekli dağıtım destekler:
* Git veya FTP.
* Docker Linux üzerinde Web uygulamaları için.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Git veya FTP üzerinden Web uygulamaları dağıtmak için Jenkins yapılandırın.
> * Jenkins kapsayıcıları için Web uygulamasını dağıtmak için yapılandırın.

## <a name="create-and-configure-a-jenkins-instance"></a>Oluşturma ve Jenkins örnek yapılandırma

Jenkins asıl zaten sahip değilseniz, başlayın [çözüm şablonu](install-jenkins-solution-template.md), Java Geliştirme Seti (JDK) 8 sürümünü içerir ve aşağıdaki Jenkins eklentileri gerekli:

* [Jenkins Git istemcisi eklentisi](https://plugins.jenkins.io/git-client) sürüm 2.4.6 
* [Docker Commons eklentisi](https://plugins.jenkins.io/docker-commons) sürüm 1.4.0
* [Azure kimlik](https://plugins.jenkins.io/azure-credentials) sürüm 1.2
* [Azure uygulama hizmeti](https://plugins.jenkins.io/azure-app-service) sürüm 0.1

Jenkins eklenti, C#, PHP, Java ve Node.js gibi Web uygulamaları tarafından desteklenen herhangi bir dil bir web uygulamasını dağıtmak için kullanabilirsiniz. Bu öğreticide, kullandığımız bir [basit Java web uygulaması için Azure](https://github.com/azure-devops/javawebappsample). Kendi GitHub hesabınızda depoyu çatallaştırma için seçin **çatalı** GitHub arabirimi sağ üst köşesindeki düğmesini.  
> [!NOTE]
> Maven ve Java JDK Java projesi derlemek için gereklidir. Sürekli tümleştirme için aracı kullanıyorsanız, bu bileşenlerin üzerinde Jenkins asıl veya VM aracısı yükleyin. 

Bileşenleri yüklemek için SSH Jenkins örneğiyle oturum açın ve aşağıdaki komutları çalıştırın:

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

Web uygulaması kapsayıcıları için dağıtmak için Docker üzerinde Jenkins asıl veya derleme için kullanılan VM aracısı yükleyin. Yönergeler için bkz: [yükleme Docker Ubuntu üzerinde](https://docs.docker.com/engine/installation/linux/ubuntu/).

##<a name="service-principal"></a> Jenkins kimlik bilgileri için bir Azure hizmet sorumlusu ekleme

Azure'a dağıtmak için bir Azure hizmet sorumlusu gerekir. 


1. Bir Azure hizmet sorumlusu oluşturmak için kullanmak [Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) veya [Azure portal](/azure/azure-resource-manager/resource-group-create-service-principal-portal).
2. Jenkins panosunda seçin **kimlik bilgileri** > **sistem**. Ardından, seçin **genel credentials(unrestricted)**.
3. Bir Microsoft Azure hizmet sorumlusu eklemek için seçin **kimlik bilgilerini eklemek**. İçin değerler sağlayın **abonelik kimliği**, **istemci kimliği**, **gizli**, ve **OAuth 2.0 belirteç uç noktası** alanları. Ayarlama **kimliği** alanı **bileşene mySp**. Bu makalenin sonraki adımlarda bu kimliği kullanırız.


## <a name="configure-jenkins-to-deploy-web-apps-by-uploading-files"></a>Dosyaları karşıya yükleyerek Web uygulamalarını dağıtmak için Jenkins yapılandırın

Projeniz için Web uygulamalarını dağıtmak için Git veya FTP kullanarak derleme yapıtları (örneğin, Java WAR dosyası) karşıya yükleyebilir.

Jenkins işinde ayarlama önce Azure App Service planı ve bir web uygulaması Java uygulama çalıştırmak için gerekir.


1. İle bir Azure uygulama hizmeti planı oluştur **serbest** fiyatlandırma katmanı kullanarak `az appservice plan create` [Azure CLI komutu](/cli/azure/appservice/plan#az_appservice_plan_create). Uygulama hizmeti planı, uygulamalarınızı barındırmak için kullanılan fiziksel kaynakları tanımlar. Bir uygulama hizmeti planına atanmış olan tüm uygulamalar, bu kaynakları paylaşır. Paylaşılan kaynakları birden fazla uygulama barındırdığında giderlerinden tasarruf edersiniz yardımcı olur.
2. Bir web uygulaması oluşturun. Kullanabileceğiniz [Azure portal](/azure/app-service-web/web-sites-configure) veya aşağıdaki `az` Azure CLI komutu:
    ```azurecli-interactive 
    az webapp create --name <myAppName> --resource-group <myResourceGroup> --plan <myAppServicePlan>
    ```
    
3. Uygulamanız gereken Java Çalışma zamanı yapılandırmasını ayarlayın. Son JDK 8 üzerinde çalışması için web uygulama aşağıdaki Azure CLI komutu yapılandırır ve [Apache Tomcat](http://tomcat.apache.org/) sürüm 8.0:
    ```azurecli-interactive
    az webapp config set \
    --name <myAppName> \
    --resource-group <myResourceGroup> \
    --java-version 1.8 \
    --java-container Tomcat \
    --java-container-version 8.0
    ```

### <a name="set-up-the-jenkins-job"></a>Jenkins iş ayarlayın

1. Yeni bir **freestyle** Jenkins Pano projede.
2. Yapılandırma **kaynak kodu Yönetimi** , yerel çatalı, kullanılacak alanı [basit Java web uygulaması için Azure](https://github.com/azure-devops/javawebappsample). Sağlamak **depo URL'si** değeri. Örneğin: http://github.com/ &lt;your_ID > / javawebappsample.
3. Ekleyerek Maven kullanarak Projeyi derlemek için bir adım eklemek **Kabuk yürütme** komutu. Bu örnekte, yeniden adlandırmak için ek bir komut ihtiyacımız \*hedef klasöre .war dosyasında **ROOT.war**:   
    ```bash
    mvn clean package
    mv target/*.war target/ROOT.war
    ```

4. Bir oluşturma sonrası eylemi seçerek eklemek **Azure Web uygulaması yayımlama**.
5. Tedarik **bileşene mySp** Azure hizmet sorumlusu olarak. Bu asıl olarak depolanan [Azure kimlik bilgilerini](#service-principal) bir önceki adımda.
6. İçinde **uygulama yapılandırması** bölümünde, aboneliğinizde kaynak grubu ve web uygulaması seçin. Jenkins eklentisi, web uygulaması Windows veya Linux göre otomatik olarak algılar. Bir Windows web uygulaması için **yayımlama dosyaları** seçeneği sunulur.
7. Dağıtmak istediğiniz dosyaları doldurun. Örneğin, Java kullanılıyorsa WAR paketi belirtin. İsteğe bağlı kullanmak **kaynak dizin** ve **hedef dizin** dosya karşıya yükleme için kullanılacak kaynak ve hedef klasörler belirtmek üzere parametreler. Azure'da Java web uygulaması Tomcat Server'da çalıştırılır. Bu nedenle Java için WAR paketinizi webapps klasörüne yükleyin. Bu örnek için set **kaynak dizin** değeri **hedef** ve **hedef dizin** değeri **webapps**.
8. Bir yuva üretim dışında dağıtmak istiyorsanız, ayrıca ayarlayabilirsiniz **yuvası** adı.
9. Projeyi kaydedin ve onu oluşturun. Yapı tamamlandığında, web uygulamanızı Azure'a dağıtılır.

### <a name="deploy-web-apps-by-uploading-files-using-jenkins-pipeline"></a>Jenkins ardışık düzen kullanarak dosyaları karşıya yükleyerek Web uygulamalarını dağıtma

Azure App Service Jenkins eklentisi ardışık düzeni için hazır olur. GitHub depo aşağıdaki örnekte başvurabilirsiniz.

1. GitHub arabiriminde açmak **Jenkinsfile_ftp_plugin** dosya. Dosyayı düzenlemek için Kurşun Kalem simgesini seçin. Güncelleştirme **resourceGroup** ve **webAppName** web uygulamanız için tanımları satırları 11 ve 12, sırasıyla:
    ```java
    def resourceGroup = '<myResourceGroup>'
    def webAppName = '<myAppName>'
    ```

2. Ayarlama **withCredentials** tanımı 14 Jenkins Örneğinizde kimlik bilgisi kimliği:
    ```java
    withCredentials([azureServicePrincipal('<mySp>')]) {
    ```

### <a name="create-a-jenkins-pipeline"></a>Jenkins işlem hattı oluşturma

1. Jenkins bir web tarayıcısında açın. Seçin **yeni öğe**.
2. Seçin ve iş için bir ad **ardışık düzen**. **Tamam**’ı seçin.
3. Seçin **ardışık düzen** sekmesi.
4. İçin **tanımı** değeri, select **kanal SCM betikten**.
5. İçin **SCM** değeri, select **Git**. GitHub URL için forked, depodaki girin. Örneğin: https://&lt;your_forked_repo > .git.
6. Güncelleştirme **betik yolu** değeri **Jenkinsfile_ftp_plugin**.
7. Seçin **kaydetmek** ve işini çalıştırın.

## <a name="configure-jenkins-to-deploy-web-app-for-containers"></a>Jenkins kapsayıcıları için Web uygulamasını dağıtmak için yapılandırma

Web uygulamaları Linux'ta Docker kullanarak dağıtımını destekler. Docker kullanarak web uygulamanızı dağıtmak için web uygulamanızı bir hizmet çalışma zamanı ile Docker görüntüsüne paketleri bir Dockerfile sağlamanız gerekir. Jenkins eklentisi sonra görüntü oluşturur, Docker kayıt defterine iter ve web uygulamanıza görüntüyü dağıtır.

Web uygulamaları Linux'ta geleneksel dağıtım yöntemleri, Git ve FTP gibi ancak yalnızca yerleşik dilleri (.NET Core, Node.js, PHP ve Ruby) da destekler. Diğer diller için uygulama kodu ve hizmet çalışma zamanı birlikte Docker görüntüsüne paketini ve dağıtmak için Docker kullanmanız gerekir.

Jenkins işinde ayarlamadan önce Linux üzerinde bir web uygulaması gerekir. Ayrıca depolamak ve özel Docker kapsayıcısı görüntülerinizi yönetmek için bir kapsayıcı kayıt defteri gerekir. DockerHub kapsayıcı kayıt oluşturmak için kullanabilirsiniz. Bu örnekte, Azure kapsayıcı kayıt defteri kullanırız.

* [Web uygulamanızı oluşturma Linux](../app-service/containers/quickstart-nodejs.md).
* Azure kapsayıcı kayıt defteri olan yönetilen [Docker kayıt defteri](https://docs.docker.com/registry/) açık kaynak Docker kayıt defteri sürüm 2.0 bağlı hizmeti. [Bir Azure kapsayıcı kayıt](/azure/container-registry/container-registry-get-started-azure-cli). DockerHub de kullanabilirsiniz.

### <a name="set-up-the-jenkins-job-for-docker"></a>Jenkins iş için Docker ayarlayın

1. Yeni bir **freestyle** Jenkins Pano projede.
2. Yapılandırma **kaynak kodu Yönetimi** , yerel çatalı, kullanılacak alanı [basit Java web uygulaması için Azure](https://github.com/azure-devops/javawebappsample). Sağlamak **depo URL'si** değeri. Örneğin: http://github.com/ &lt;your_ID > / javawebappsample.
3. Ekleyerek Maven kullanarak Projeyi derlemek için bir adım eklemek bir **Kabuk yürütme** komutu. Komutta aşağıdaki satırı ekleyin:
    ```bash
    mvn clean package
    ```

4. Bir oluşturma sonrası eylemi seçerek eklemek **Azure Web uygulaması yayımlama**.
5. Tedarik **bileşene mySp** Azure hizmet sorumlusu olarak. Bu asıl olarak depolanan [Azure kimlik bilgilerini](#service-principal) bir önceki adımda.
6. İçinde **uygulama yapılandırması** bölümünde, aboneliğinizde kaynak grubu ve Linux web uygulaması seçin.
7. Seçin **Docker yayımlamak**.
8. Doldurmak **Dockerfile** yol değeri. Varsayılan değer /Dockerfile kullanmaya devam edebilir.
İçin **Docker kayıt defteri URL** değeri, URL biçimi https:// kullanarak temin&lt;yourRegistry >. Azure kapsayıcı kayıt defteri kullanırsanız azurecr.io. DockerHub kullanırsanız, değer boş bırakın.
9. İçin **kayıt defteri kimlik** değeri, kapsayıcı kayıt defteri için kimlik bilgisi ekleyin. Azure CLI aşağıdaki komutları çalıştırarak kullanıcı kimliği ve parola alabilirsiniz. İlk komut yönetici hesabını etkinleştirir:
    ```azurecli-interactive
    az acr update -n <yourRegistry> --admin-enabled true
    az acr credential show -n <yourRegistry>
    ```

10. Docker görüntü adı ve etiket değeri **Gelişmiş** sekmesi isteğe bağlı. Varsayılan olarak, görüntü adı için değer Azure Portalı'nda yapılandırılan görüntü adı elde edilir **Docker kapsayıcısı** ayarı. Etiket $BUILD_NUMBER oluşturulur.
    > [!NOTE]
    > Azure portalında görüntü adı belirtin veya sağlayın mutlaka bir **Docker görüntü** değeri **Gelişmiş** sekmesi. Bu örnek için set **Docker görüntü** değeri &lt;your_Registry >.azurecr.io/calculator ve bırakın **Docker resim etiketi** değeri boş.

11. Bir yerleşik Docker görüntü ayarını kullanırsanız dağıtma başarısız olur. Özel görüntü kullanmak için Docker yapılandırmasını değiştirme **Docker kapsayıcısı** Azure portalında ayarlama. Yerleşik bir görüntü için dosya karşıya yükleme yaklaşımı dağıtmak için kullanın.
12. Benzer şekilde dosya karşıya yükleme yaklaşım, farklı bir seçebileceğiniz **yuvası** dışında ad **üretim**.
13. Kaydedin ve projeyi oluşturun. Kapsayıcı görüntünüzü kayıt defterine gönderilir ve web uygulaması dağıtılır.

### <a name="deploy-web-app-for-containers-by-using-jenkins-pipeline"></a>Jenkins komut zincirini kullanarak kapsayıcıları için Web uygulaması dağıtma

1. GitHub arabiriminde açmak **Jenkinsfile_container_plugin** dosya. Dosyayı düzenlemek için Kurşun Kalem simgesini seçin. Güncelleştirme **resourceGroup** ve **webAppName** web uygulamanız için tanımları satırları 11 ve 12, sırasıyla:
    ```java
    def resourceGroup = '<myResourceGroup>'
    def webAppName = '<myAppName>'
    ```

2. Satırı 13 kapsayıcı kayıt defteri sunucunuza değiştirin:
    ```java
    def registryServer = '<registryURL>'
    ```

3. Kimlik bilgisi kimliği Jenkins örneğinizi kullanmak için 16 satırı değiştirin:
    ```java
    azureWebAppPublish azureCredentialsId: '<mySp>', publishType: 'docker', resourceGroup: resourceGroup, appName: webAppName, dockerImageName: imageName, dockerImageTag: imageTag, dockerRegistryEndpoint: [credentialsId: 'acr', url: "http://$registryServer"]
    ```

### <a name="create-a-jenkins-pipeline"></a>Jenkins işlem hattı oluşturma    

1. Jenkins bir web tarayıcısında açın. Seçin **yeni öğe**.
2. Seçin ve iş için bir ad **ardışık düzen**. **Tamam**’ı seçin.
3. Seçin **ardışık düzen** sekmesi.
4. İçin **tanımı** değeri, select **kanal SCM betikten**.
5. İçin **SCM** değeri, select **Git**. GitHub URL için forked, depodaki girin. Örneğin: https://&lt;your_forked_repo > .git.
7. Güncelleştirme **betik yolu** değeri **Jenkinsfile_container_plugin**.
8. Seçin **kaydetmek** ve işini çalıştırın.

## <a name="verify-your-web-app"></a>Web uygulamanızı doğrulayın

1. WAR dosyası, web uygulamanızın başarıyla dağıtıldığını doğrulamak için bir web tarayıcısı açın.
2. Http:// gidin&lt;your_app_name >.azurewebsites.net/api/calculator/ping. Değiştir &lt;your_app_name >, web uygulamanızın adı. İletiyi görürsünüz:
    ```
    Welcome to Java Web App!!! This is updated!
    Sun Jun 17 16:39:10 UTC 2017
    ```

3. Http:// gidin&lt;your_app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y >. Değiştir &lt;x > ve &lt;y > x toplamını almak için herhangi bir sayı ile + y. Sum hesaplayıcı gösterir: ![hesaplayıcı: Ekle](./media/execute-cli-jenkins-pipeline/calculator-add.png)

### <a name="for-azure-app-service-on-linux"></a>Linux üzerinde Azure uygulama hizmeti

1. Web uygulamanızı doğrulamak için Azure CLI aşağıdaki komutu çalıştırın:
    ```CLI
    az acr repository list -n <myRegistry> -o json
    ```
    Aşağıdaki ileti görüntülenir:
    ```CLI
    ["calculator"]
    ```
    
2. Http:// gidin&lt;your_app_name >.azurewebsites.net/api/calculator/ping. Değiştir &lt;your_app_name >, web uygulamanızın adı. İletiyi görürsünüz: 
    ```
    Welcome to Java Web App!!! This is updated!
    Sun Jul 09 16:39:10 UTC 2017
    ```

3. Http:// gidin&lt;your_app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y >. Değiştir &lt;x > ve &lt;y > x toplamını almak için herhangi bir sayı ile + y.
    
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure App Service Jenkins eklentisi Azure'a dağıtmak için kullanılır.

Şunları öğrendiniz:

> [!div class="checklist"]
> * FTP aracılığıyla Azure uygulama hizmeti dağıtmak için Jenkins yapılandırın 
> * Jenkins kapsayıcıları için Web uygulamasına dağıtmak için yapılandırma 
