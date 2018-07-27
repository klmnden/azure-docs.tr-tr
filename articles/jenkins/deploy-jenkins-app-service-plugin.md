---
title: Jenkins eklentisini kullanarak Azure App Service'e dağıtma
description: Azure App Service Jenkins eklentisini Jenkins Azure'da bir Java web uygulaması dağıtmak için kullanmayı öğrenin
ms.topic: article
ms.author: tarcher
author: tomarcher
manager: jpconnock
ms.service: devops
ms.custom: jenkins
ms.date: 07/25/2018
ms.openlocfilehash: 407ec2bbb145e73b1a903886204b660aadc9a65f
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284423"
---
# <a name="deploy-to-azure-app-service-by-using-the-jenkins-plugin"></a>Jenkins eklentisini kullanarak Azure App Service'e dağıtma 

Bir Java web uygulamasını Azure'a dağıtmak için Azure CLI'yi kullanabilirsiniz [Jenkins işlem hattı](/azure/jenkins/execute-cli-jenkins-pipeline) veya [Azure App Service Jenkins eklentisi](https://plugins.jenkins.io/azure-app-service). Jenkins eklentisi sürüm 1.0 ile Azure App Service'in Web Apps özelliğini kullanarak sürekli dağıtımı destekler:
* Git veya FTP.
* Linux üzerinde Web Apps için docker.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Git veya FTP üzerinden Web uygulamaları dağıtmak için Jenkins yapılandırın.
> * Kapsayıcılar için Web App'e dağıtmak için Jenkins yapılandırın.

## <a name="create-and-configure-a-jenkins-instance"></a>Oluşturma ve Jenkins örneği yapılandırma

Bir Jenkins ana yoksa başlayın [çözüm şablonu](install-jenkins-solution-template.md), Java Development Kit (JDK) 8 sürümünü içerir ve aşağıdaki Jenkins eklenti gereklidir:

* [Jenkins Git istemci eklentisi](https://plugins.jenkins.io/git-client) 2.4.6 sürümü 
* [Docker Commons eklentisi](https://plugins.jenkins.io/docker-commons) sürüm 1.4.0
* [Azure kimlik](https://plugins.jenkins.io/azure-credentials) sürüm 1.2
* [Azure App Service](https://plugins.jenkins.io/azure-app-service) sürüm 0.1

Jenkins eklentisi, C#, PHP, Java ve Node.js gibi Web Apps tarafından desteklenen herhangi bir dilde bir web uygulamasında dağıtmak için kullanabilirsiniz. Bu öğreticide, kullandığımız bir [Azure için basit bir Java web uygulaması](https://github.com/azure-devops/javawebappsample). GitHub hesabınıza depo çatalı oluşturma için seçin **çatal** GitHub arabirimini sağ üst köşesindeki düğme.  
> [!NOTE]
> Java JDK ve Maven Java projesi oluşturmak için gereklidir. Aracı için sürekli tümleştirme kullanıyorsanız bu bileşenlerin üzerinde Jenkins ana veya VM aracısını yükleyin. 

Bileşenleri yüklemek için SSH ile Jenkins örneğine oturum açın ve aşağıdaki komutları çalıştırın:

```bash
sudo apt-get install -y openjdk-7-jdk
sudo apt-get install -y maven
```

Kapsayıcılar için Web App'e dağıtmak için Docker üzerinde Jenkins ana veya derleme için kullanılan VM aracısını yükleyin. Yönergeler için [ubuntu'da Docker yükleme](https://docs.docker.com/engine/installation/linux/ubuntu/).

##<a name="service-principal"></a> Jenkins kimlik bilgileri için bir Azure hizmet sorumlusu ekleme

Azure'a dağıtmak için bir Azure hizmet sorumlusu gerekir. 


1. Bir Azure hizmet sorumlusu oluşturmak için kullanın [Azure CLI](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) veya [Azure portalında](/azure/azure-resource-manager/resource-group-create-service-principal-portal).
2. Jenkins panosunda seçin **kimlik bilgilerini** > **sistem**. Ardından, **genel credentials(unrestricted)**.
3. Microsoft Azure hizmet sorumlusu eklemek için seçin **kimlik bilgileri ekleme**. İçin değerler sağlayın **abonelik kimliği**, **istemci kimliği**, **gizli**, ve **OAuth 2.0 belirteç uç noktası** alanları. Ayarlama **kimliği** alanı **bileşene mySp**. Bu makalede, sonraki adımlarda bu kimliği kullanın.


## <a name="configure-jenkins-to-deploy-web-apps-by-uploading-files"></a>Dosyaları karşıya yükleyerek Web uygulamalarını dağıtmak için Jenkins yapılandırın

Projeniz için Web uygulamaları dağıtmak için Git veya FTP kullanarak derleme yapıtlarınızı (örneğin, Java WAR dosyası) karşıya yükleyebilirsiniz.

Jenkins işi ayarlama önce Azure App Service planı ve bir web uygulaması Java uygulamasını çalıştırmak için gerekir.


1. Bir Azure App Service planı oluşturun **ücretsiz** fiyatlandırma katmanını kullanarak `az appservice plan create` [Azure CLI komutunu](/cli/azure/appservice/plan#az_appservice_plan_create). App Service planı, uygulamalarınızı barındırmak için kullanılan fiziksel kaynakları tanımlar. Bir App Service planına atanan tüm uygulamalar, bu kaynakları paylaşır. Paylaşılan kaynaklar birden çok uygulamayı barındırırken maliyetlerinden tasarruf etmenize yardımcı olur.
2. Bir web uygulaması oluşturun. Kullanabileceğiniz [Azure portalında](/azure/app-service-web/web-sites-configure) veya aşağıdaki `az` Azure CLI komutunu:
    ```azurecli-interactive 
    az webapp create --name <myAppName> --resource-group <myResourceGroup> --plan <myAppServicePlan>
    ```
    
3. Uygulamanıza gereken Java Çalışma zamanı yapılandırmasını ayarlayın. Web uygulaması, son JDK 8'de çalıştırmak için aşağıdaki Azure CLI komutunu yapılandırır ve [Apache Tomcat](http://tomcat.apache.org/) sürüm 8.0:
    ```azurecli-interactive
    az webapp config set \
    --name <myAppName> \
    --resource-group <myResourceGroup> \
    --java-version 1.8 \
    --java-container Tomcat \
    --java-container-version 8.0
    ```

### <a name="set-up-the-jenkins-job"></a>Jenkins işi ayarlama

1. Yeni bir **freestyle** Jenkins panosuna projede.
2. Yapılandırma **kaynak kodu Yönetimi** yerel çatalınızda kullanılacak alanı [Azure için basit bir Java web uygulaması](https://github.com/azure-devops/javawebappsample). Sağlamak **depo URL'si** değeri. Örneğin: http://github.com/&lt; your_ID > / javawebappsample.
3. Maven kullanarak ekleyerek Projeyi derlemek için bir adım eklemek **Kabuğu Yürüt** komutu. Bu örnekte, yeniden adlandırmak için ek bir komut ihtiyacımız \*.war dosyasını hedef klasöre **ROOT.war**:   
    ```bash
    mvn clean package
    mv target/*.war target/ROOT.war
    ```

4. Oluşturma sonrası eylem seçerek ekleyin **bir Azure Web uygulaması yayımlamak**.
5. Tedarik **bileşene mySp** Azure hizmet sorumlusu olarak. Bu asıl olarak depolanan [Azure kimlik bilgileri](#service-principal) önceki bir adımda.
6. İçinde **uygulama yapılandırması** bölümünde, aboneliğinizde bir kaynak grubu ve web uygulamasını seçin. Jenkins eklentisi, Windows veya Linux üzerinde web uygulaması dayalı olup olmadığını otomatik olarak algılar. Bir Windows web uygulaması için **yayımlama dosyaları** seçeneği sunulur.
7. Dağıtmak istediğiniz dosyaları doldurun. Örneğin, Java kullanıyorsanız, WAR paketi belirtin. İsteğe bağlı **kaynak dizini** ve **hedef dizin** dosya karşıya yükleme için kullanılacak kaynak ve hedef klasörler belirtmek için parametreleri. Azure'da Java web uygulaması bir Tomcat sunucusunda çalıştırılır. Bu nedenle Java için WAR paketinizi webapps klasörüne yükleyin. Bu örnek için set **kaynak dizini** değerini **hedef** ve **hedef dizin** değerini **webapps**.
8. Üretim dışında bir yuvaya dağıtmak isterseniz de ayarlayabilirsiniz **yuvası** adı.
9. Projeyi kaydedin ve derleyin. Derleme tamamlandığında web uygulamanızı Azure'a dağıtılır.

### <a name="deploy-web-apps-by-uploading-files-using-jenkins-pipeline"></a>Jenkins işlem hattı kullanarak dosyaları karşıya yükleyerek Web uygulamalarını dağıtma

Azure App Service Jenkins eklentisi ardışık düzen hazır olur. Aşağıdaki örnek GitHub deposunda başvurabilirsiniz.

1. GitHub arabiriminde açmak **Jenkinsfile_ftp_plugin** dosya. Dosyayı düzenlemek için kalem simgesini seçin. Güncelleştirme **resourceGroup** ve **webAppName** tanımları üzerinde web Apps uygulamanız için 11 ve 12, sırasıyla satırlar:
    ```java
    def resourceGroup = '<myResourceGroup>'
    def webAppName = '<myAppName>'
    ```

2. Ayarlama **withCredentials** kimlik bilgileri Jenkins Örneğinizdeki 14 satırındaki tanımı:
    ```java
    withCredentials([azureServicePrincipal('<mySp>')]) {
    ```

### <a name="create-a-jenkins-pipeline"></a>Jenkins işlem hattı oluşturma

1. Jenkins, bir web tarayıcısında açın. **Yeni Öğe**’yi seçin.
2. Seçin ve iş için bir ad sağlayın **işlem hattı**. **Tamam**’ı seçin.
3. Seçin **işlem hattı** sekmesi.
4. İçin **tanımı** değeri, select **işlem hattı SCM betikten**.
5. İçin **SCM** değeri, select **Git**. Çatalı oluşturulan deponuzu için GitHub URL'sini girin. Örneğin: https://&lt;your_forked_repo > .git.
6. Güncelleştirme **betik yolu** değerini **Jenkinsfile_ftp_plugin**.
7. Seçin **Kaydet** ve işi çalıştırın.

## <a name="configure-jenkins-to-deploy-web-app-for-containers"></a>Kapsayıcılar için Web App'e dağıtmak için Jenkins yapılandırın

Linux üzerinde Web Apps, Docker'ı kullanarak dağıtımı destekler. Docker kullanarak web uygulamanızı dağıtmak için bir hizmet çalışma zamanı web uygulamanızla bir Docker görüntüsü halinde paketler bir Dockerfile'ı sağlamanız gerekir. Jenkins eklentisi ardından görüntüyü oluşturur, bir Docker kayıt defterine iletir ve görüntüsünü web uygulamanıza dağıtır.

Linux üzerinde Web Apps, geleneksel dağıtım yöntemleri, Git ve FTP gibi ancak yalnızca yerleşik dilleri (.NET Core, Node.js, PHP ve Ruby) için de destekler. Diğer diller için uygulama kodu ve hizmet çalışma zamanı birlikte bir Docker görüntüsü halinde paketlemek ve dağıtmak için Docker'ı kullanma gerekir.

Jenkins işi ayarlamadan önce Linux üzerinde web uygulaması gerekir. Ayrıca, özel Docker kapsayıcı görüntülerini yönetmek için bir kapsayıcı kayıt defteri gerekir. DockerHub, kapsayıcı kayıt defteri oluşturmak için kullanabilirsiniz. Bu örnekte, Azure Container Registry kullanırız.

* [Linux üzerinde web uygulamanızı oluşturma](../app-service/containers/quickstart-nodejs.md).
* Azure Container Registry olan yönetilen [Docker kayıt defteri](https://docs.docker.com/registry/) açık kaynak Docker kayıt defteri sürüm 2.0 temel hizmet. [Azure container registry oluşturma](/azure/container-registry/container-registry-get-started-azure-cli). DockerHub de kullanabilirsiniz.

### <a name="set-up-the-jenkins-job-for-docker"></a>Jenkins işi için Docker ayarlayın

1. Yeni bir **freestyle** Jenkins panosuna projede.
2. Yapılandırma **kaynak kodu Yönetimi** yerel çatalınızda kullanılacak alanı [Azure için basit bir Java web uygulaması](https://github.com/azure-devops/javawebappsample). Sağlamak **depo URL'si** değeri. Örneğin: http://github.com/&lt; your_ID > / javawebappsample.
3. Maven kullanarak ekleyerek Projeyi derlemek için bir adım eklemek bir **Kabuğu Yürüt** komutu. Komut içinde aşağıdaki satırı ekleyin:
    ```bash
    mvn clean package
    ```

4. Oluşturma sonrası eylem seçerek ekleyin **bir Azure Web uygulaması yayımlamak**.
5. Tedarik **bileşene mySp** Azure hizmet sorumlusu olarak. Bu asıl olarak depolanan [Azure kimlik bilgileri](#service-principal) önceki bir adımda.
6. İçinde **uygulama yapılandırması** bölümünde, aboneliğinizde bir kaynak grubu ve bir Linux web uygulaması seçin.
7. Seçin **Docker yayımlama**.
8. Doldurun **Dockerfile** yol değeri. Varsayılan değer /Dockerfile tutabilirsiniz.
İçin **Docker kayıt defteri URL'si** değeri, URL'nin biçimi https:// kullanarak tedarik&lt;yourRegistry >. Azure Container Registry kullanırsanız azurecr.io. DockerHub kullanırsanız, değeri boş bırakın.
9. İçin **kayıt defteri kimlik bilgilerini** değeri, kapsayıcı kayıt defteri için kimlik bilgisi ekleyin. Azure CLI içinde aşağıdaki komutları çalıştırarak kullanıcı kimliği ve parola alabilirsiniz. İlk komut, yönetici hesabını etkinleştirir:
    ```azurecli-interactive
    az acr update -n <yourRegistry> --admin-enabled true
    az acr credential show -n <yourRegistry>
    ```

10. Docker görüntü adı ve etiket değeri **Gelişmiş** sekmesi isteğe bağlı. Varsayılan olarak, Azure portalında yapılandırılmış görüntü adı görüntü adı için değer alınır **Docker kapsayıcısı** ayarı. Etiket $BUILD_NUMBER oluşturulur.
    > [!NOTE]
    > Azure portalında görüntü adı belirtin veya sağlamak mutlaka bir **Docker görüntüsü** değerini **Gelişmiş** sekmesi. Bu örnek için set **Docker görüntüsü** değerini &lt;your_Registry >.azurecr.io/calculator ve ayrılma **Docker resim etiketi** değeri boş.

11. Bir yerleşik Docker görüntüsü ayarı kullanıyorsanız, dağıtım başarısız olur. Özel görüntüsünü kullanmak için Docker yapılandırmasını değiştirme **Docker kapsayıcısı** Azure portalında ayarlama. Yerleşik bir görüntü için bir dosya karşıya yükleme yaklaşım dağıtmak için kullanın.
12. Benzer şekilde dosya karşıya yükleme yaklaşım, farklı bir seçebileceğiniz **yuvası** dışında ad **üretim**.
13. Kaydedin ve projeyi derleyin. Kapsayıcı görüntünüzü kayıt defterinize itilir ve web uygulaması dağıttınız.

### <a name="deploy-web-app-for-containers-by-using-jenkins-pipeline"></a>Jenkins işlem hattı kullanarak kapsayıcılar için Web uygulaması dağıtma

1. GitHub arabiriminde açmak **Jenkinsfile_container_plugin** dosya. Dosyayı düzenlemek için kalem simgesini seçin. Güncelleştirme **resourceGroup** ve **webAppName** tanımları üzerinde web Apps uygulamanız için 11 ve 12, sırasıyla satırlar:
    ```java
    def resourceGroup = '<myResourceGroup>'
    def webAppName = '<myAppName>'
    ```

2. 13. satır, kapsayıcı kayıt defteri sunucunuza değiştirin:
    ```java
    def registryServer = '<registryURL>'
    ```

3. Jenkins Örneğinize kimlik bilgileri kullanılacak 16 satırı değiştirin:
    ```java
    azureWebAppPublish azureCredentialsId: '<mySp>', publishType: 'docker', resourceGroup: resourceGroup, appName: webAppName, dockerImageName: imageName, dockerImageTag: imageTag, dockerRegistryEndpoint: [credentialsId: 'acr', url: "http://$registryServer"]
    ```

### <a name="create-a-jenkins-pipeline"></a>Jenkins işlem hattı oluşturma    

1. Jenkins, bir web tarayıcısında açın. **Yeni Öğe**’yi seçin.
2. Seçin ve iş için bir ad sağlayın **işlem hattı**. **Tamam**’ı seçin.
3. Seçin **işlem hattı** sekmesi.
4. İçin **tanımı** değeri, select **işlem hattı SCM betikten**.
5. İçin **SCM** değeri, select **Git**. Çatalı oluşturulan deponuzu için GitHub URL'sini girin. Örneğin: https://&lt;your_forked_repo > .git.
7. Güncelleştirme **betik yolu** değerini **Jenkinsfile_container_plugin**.
8. Seçin **Kaydet** ve işi çalıştırın.

## <a name="verify-your-web-app"></a>Web uygulamanızı doğrulayın

1. WAR dosyasını web uygulamanıza başarıyla dağıtıldığını doğrulamak için bir web tarayıcısı açın.
2. Http:// Git&lt;your_app_name >.azurewebsites.net/api/calculator/ping. Değiştirin &lt;your_app_name > web uygulamanızın adına sahip. İletisini görürsünüz:
    ```
    Welcome to Java Web App!!! This is updated!
    Sun Jun 17 16:39:10 UTC 2017
    ```

3. Http:// Git&lt;your_app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y >. Değiştirin &lt;x > ve &lt;y > x toplamını almak için herhangi bir sayı ile + y. Hesaplayıcı toplamı gösterilir: ![hesaplayıcı: Ekle](./media/execute-cli-jenkins-pipeline/calculator-add.png)

### <a name="for-azure-app-service-on-linux"></a>Linux üzerinde Azure App Service için

1. Web uygulamanızı doğrulamak için Azure CLI içinde aşağıdaki komutu çalıştırın:
    ```CLI
    az acr repository list -n <myRegistry> -o json
    ```
    Aşağıdaki ileti görüntülenir:
    ```CLI
    ["calculator"]
    ```
    
2. Http:// Git&lt;your_app_name >.azurewebsites.net/api/calculator/ping. Değiştirin &lt;your_app_name > web uygulamanızın adına sahip. İletisini görürsünüz: 
    ```
    Welcome to Java Web App!!! This is updated!
    Sun Jul 09 16:39:10 UTC 2017
    ```

3. Http:// Git&lt;your_app_name >.azurewebsites.net/api/calculator/add?x=&lt;x > & y =&lt;y >. Değiştirin &lt;x > ve &lt;y > x toplamını almak için herhangi bir sayı ile + y.
    
## <a name="troubleshooting"></a>Sorun giderme

Tüm hatalar Jenkins eklentileri ile karşılaşırsanız, sorunu bildirin [Jenkins JIRA](https://issues.jenkins-ci.org/) belirli bileşeni.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure App Service Jenkins eklentisini Azure'a dağıtmak için kullanılır.

Şunları öğrendiniz:

> [!div class="checklist"]
> * FTP aracılığıyla Azure App Service'e dağıtım yapmak, Jenkins'i yapılandırma 
> * Kapsayıcılar için Web App'e dağıtmak için Jenkins yapılandırın 
