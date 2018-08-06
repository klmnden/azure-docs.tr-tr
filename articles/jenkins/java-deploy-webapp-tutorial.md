---
title: Web uygulamalarınızı Azure’a dağıtmak için Jenkins’i kullanma
description: Jenkins ve Docker’ı kullanarak Java web uygulamalarınız için GitHub’dan Azure App Service’e sürekli tümleştirme ayarlarını yapın.
ms.topic: tutorial
ms.author: tarcher
author: tomarcher
manager: jpconnock
ms.service: devops
ms.custom: jenkins
ms.date: 07/31/2018
ms.openlocfilehash: e880d84c3ae0fd23c11bb9b30733544bd5f28872
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39389951"
---
# <a name="set-up-continuous-integration-and-deployment-to-azure-app-service-with-jenkins"></a>Jenkins ile Azure App Service’e sürekli tümleştirme ve dağıtım ayarlama

Bu öğreticide Jenkins kullanarak [Spring Boot](http://projects.spring.io/spring-boot/) çerçevesi ile geliştirilmiş olan örnek Java uygulaması için [Azure App Service Linux üzerinde Web App](/azure/app-service/containers/app-service-linux-intro) sürekli tümleştirme ve dağıtımı (CI/CD) ayarlama adımları anlatılmaktadır.

Bu öğreticide aşağıdaki görevleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Azure App Service’e dağıtmak için gerekli Jenkins eklentilerini yükleme.
> * Yeni bir işleme gönderildiğinde GitHub deposundan Docker görüntüleri oluşturacak bir Jenkins işi tanımlama.
> * Yeni bir Linux için Azure Web Uygulaması tanımlama ve Azure Container Registry’ye gönderilen Docker görüntülerini dağıtacak şekilde yapılandırma.
> * Azure App Service Jenkins eklentisini yapılandırma.
> * Örnek uygulamayı Azure App Service’e el ile derleyerek dağıtma.
> * Değişiklikleri GitHub’a göndererek Jenkins derlemesi tetikleme ve web uygulamasını güncelleştirme.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* JDK ve Maven araçları yapılandırılmış [Jenkins](https://jenkins.io/) sistemi. Jenkins sisteminiz yoksa [Jenkins çözüm şablonunu](/azure/jenkins/install-jenkins-solution-template) kullanarak şimdi Azure’da oluşturabilirsiniz.
* Bir [GitHub](https://github.com) hesabı.
* Yerel komut satırınızdan veya [Azure Cloud Shell](/azure/cloud-shell/overview)’den [Azure CLI 2.0](/cli/azure)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="install-jenkins-plug-ins"></a>Jenkins eklentilerini yükleme

1. Bir web tarayıcısı açıp Jenkins konsolunuza gidin ve sol taraftaki menüden **Manage Jenkins** (Jenkins’i Yönet) ve ardından **Manage Plugins** (Eklentileri Yönet) öğelerini seçin.
2. **Available** (Kullanılabilir) sekmesini seçin.
3. Aşağıdaki eklentileri arayın ve yanındaki onay kutusunu işaretleyin:   

    - [Azure App Service Plug-in](https://plugins.jenkins.io/azure-app-service)
    - [GitHub Branch Source Plug-in](https://plugins.jenkins.io/github-branch-source)

    Eklentiler görünmüyorsa **Installed** (Yüklü) sekmesine bakarak yüklenip yüklenmediklerini kontrol edin.

1. Eklentilerin Jenkins yapılandırmanıza eklenmesi için **Download now and install after restart** (Şimdi indir ve yeniden başlattıktan sonra yükle) öğesini seçin.

## <a name="configure-github-and-jenkins"></a>GitHub’ı ve Jenkins’i yapılandırma

Jenkins’i hesabınızdaki bir depoya yeni işlemeler gönderildiğinde [GitHub web kancalarını](https://developer.github.com/webhooks/) alacak şekilde ayarlayın.

1. **Manage Jenkins** (Jenkins’i yönet) ve **Configure System** (Sistemi Yapılandır) öğelerini seçin. **GitHub** bölümünde **Manage hooks** (Kancaları yönet) öğesinin seçili olduğundan emin olduktan sonra **Manage additional GitHub actions** (Ek GitHub eylemlerini yönet) ve **Convert login and password to token** (Oturum açma adını ve parolayı belirtece dönüştür) öğelerini seçin.
2. **From login and password** (Oturum açma adı ve paroladan) radyo düğmesini seçip GitHub kullanıcı adınızı ve parolanızı girin. **Create token credentials** (Belirteç kimlik bilgilerini oluştur) öğesini seçerek yeni bir [GitHub Personal Access Token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) (GitHub Kişisel Erişim Belirteci) oluşturun.   
   ![Oturum açma adı ve paroladan GitHub PAT oluşturma](media/jenkins-java-quickstart/create_github_credentials.png)
3.  GitHub Servers (GitHub Sunucuları) yapılandırma adımındaki **Credentials** (Kimlik Bilgileri) açılan menüsünden yeni oluşturduğunuz belirteci seçin. Kimlik doğrulamasının çalıştığını doğrulamak için **Test connection** (Bağlantıyı test et) öğesini seçin.   
   ![PAT yapılandırıldıktan sonra GitHub bağlantısını doğrulama](media/jenkins-java-quickstart/verify_github_connection.png)

> [!NOTE]
> GitHub hesabınızda iki faktörlü kimlik doğrulaması etkinse belirteci GitHub’dan oluşturup Jenkins’i bunu kullanacak şekilde yapılandırın. Ayrıntılı bilgi için [Jenkins GitHub eklentisi](https://wiki.jenkins.io/display/JENKINS/Github+Plugin) belgelerini inceleyin.

## <a name="fork-the-sample-repo-and-create-a-jenkins-job"></a>Örnek depo çatalını ve Jenkins işini oluşturma 

1. [Spring Boot örnek uygulama deposunu](https://github.com/spring-guides/gs-spring-boot-docker) açın ve sağ üst köşedeki **Fork** (Çatal Oluştur) öğesini seçerek kendi GitHub hesabınızda çatal oluşturun.   
    ![GitHub’dan çatal oluşturma](media/jenkins-java-quickstart/fork_github_repo.png)
1. Jenkins web konsolunda **New Item** (Yeni Öğe) seçimini yapın, **MyJavaApp** adını verin, **Freestyle project** (Serbest tarzda proje) ve ardından **OK** (Tamam) öğesini seçin.   
    ![Yeni Jenkins Serbest Tarz Projesi](media/jenkins-java-quickstart/jenkins_freestyle.png)
2. **General** (Genel) bölümünden **GitHub** projesini seçin ve çatal oluşturduğunuz deponuzun URL’sini https://github.com/raisa/gs-spring-boot-docker biçiminde girin.
3. **Source code management** (Kaynak kodu yönetimi) bölümünde **Git**’i seçip çatal oluşturduğunuz deponuzun `.git` URL’sini https://github.com/raisa/gs-spring-boot-docker.git biçiminde girin.
4. **Derleme Tetikleyicileri** bölümünden **GITScm yoklaması için GitHub kanca tetikleyicisi**’ni seçin.
5. **Build** (Derleme) bölümünde **Add build step** (Derleme adımı ekle) ve ardından **Invoke top-level Maven targets** (Üst düzey Maven hedeflerini çağır) öğesini seçin. **Goals** (Hedefler) alanına `package` yazın.
6. **Kaydet**’i seçin. Proje sayfasından **Build Now** (Şimdi Derle) öğesini seçerek işinizi test edebilirsiniz.

## <a name="configure-azure-app-service"></a>Azure App Service’i yapılandırma 

1. Azure CLI veya [Cloud Shell](/azure/cloud-shell/overview) kullanarak yeni bir [Linux üzerinde Web App](/azure/app-service/containers/app-service-linux-intro) oluşturun. Bu öğreticideki web uygulamasına `myJavaApp` adı verilmiştir ancak uygulamanız için benzersiz bir ad kullanmanız gerekir.
   
    ```azurecli-interactive
    az group create --name myResourceGroupJenkins --location westus
    az appservice plan create --is-linux --name myLinuxAppServicePlan --resource-group myResourceGroupJenkins 
    az webapp create --name myJavaApp --resource-group myResourceGroupJenkins --plan myLinuxAppServicePlan --runtime "java|1.8|Tomcat|8.5"
    ```

2. Jenkins tarafından oluşturulacak Docker görüntülerini depolamak için bir [Azure Container Registry](/azure/container-registry/container-registry-intro) oluşturun. Bu öğreticide kullanılan kapsayıcı kayıt defterinin adı `jenkinsregistry` olarak belirlenmiştir ancak kapsayıcı kayıt defteriniz için benzersiz bir ad kullanmanız gerekir. 

    ```azurecli-interactive
    az acr create --name jenkinsregistry --resource-group myResourceGroupJenkins --sku Basic --admin-enabled
    ```
3. Web uygulamasını kapsayıcı kayıt defterine gönderilen Docker görüntülerini çalıştıracak ve kapsayıcıda çalışan uygulamayı 8080 numaralı bağlantı noktası üzerinden gönderilen istekleri dinleyecek şekilde yapılandırın.   

    ```azurecli-interactive
    az webapp config container set -c jenkinsregistry/webapp --resource-group myResourceGroupJenkins --name myJavaApp
    az webapp config appsettings set --resource-group myResourceGroupJenkins --name myJavaApp --settings PORT=8080
    ```

## <a name="configure-the-azure-app-service-jenkins-plug-in"></a>Azure App Service Jenkins eklentisini yapılandırma

1. Jenkins web konsolunda oluşturduğunuz **MyJavaApp** işini ve ardından sayfanın sol tarafındaki **Configure** (Yapılandır) öğesini seçin.
2. Sayfayı aşağı kaydırarak **Post-build Actions** (Derleme Sonrası Eylemler) bölümüne inip **Add post-build action** (Derleme sonrası eylem ekle) ve **Publish an Azure Web App** (Azure Web Uygulaması Yayımla) öğelerini seçin.
3. **Azure Profile Configuration** (Azure Profili Yapılandırması) bölümünde **Azure Credentials** (Azure Kimlik Bilgileri) öğesinin yanındaki **Add** (Ekle) öğesini ve ardından **Jenkins**’i seçin.
4. **Add Credentials** (Kimlik Bilgilerini Ekle) iletişim kutusundaki **Kind** (Tür) açılan menüsünden **Microsoft Azure Service Principal** (Microsoft Azure Hizmet Sorumlusu) girişini seçin.
5. Azure CLI veya [Cloud Shell](/azure/cloud-shell/overview) ile bir Active Directory hizmet sorumlusu oluşturun.
    
    ```azurecli-interactive
    az ad sp create-for-rbac --name jenkins_sp --password secure_password
    ```

    ```json
    {
        "appId": "BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBB",
        "displayName": "jenkins_sp",
        "name": "http://jenkins_sp",
        "password": "secure_password",
        "tenant": "CCCCCCCC-CCCC-CCCC-CCCCCCCCCCC"
    }
    ```
6. Hizmet sorumlusunun kimlik bilgilerini **Add credentials** (Kimlik bilgilerini ekle) iletişim kutusuna girin. Azure abonelik kimliğinizi bilmiyorsanız CLI ile sorgulayabilirsiniz:
     
     ```azurecli-interactive
     az account list
     ```

     ```json
        {
            "cloudName": "AzureCloud",
            "id": "AAAAAAAA-AAAA-AAAA-AAAA-AAAAAAAAAAAA",
            "isDefault": true,
            "name": "Visual Studio Enterprise",
            "state": "Enabled",
            "tenantId": "CCCCCCCC-CCCC-CCCC-CCCC-CCCCCCCCCCC",
            "user": {
            "name": "raisa@fabrikam.com",
            "type": "user"
            }
     ```

    ![Azure hizmet sorumlusunu yapılandırma](media/jenkins-java-quickstart/azure_service_principal.png)
6. **Verify Service Principal** (Hizmet Sorumlusunu Doğrula) öğesini seçerek hizmet sorumlusunun Azure kimlik doğrulamasından geçtiğinden emin olun. 
7. Kimlik bilgilerini kaydetmek için **Add** (Ekle) öğesini seçin.
8. **Publish an Azure Web App** (Azure Web Uygulaması Yayımla) yapılandırması açıldığında **Azure Credentials** (Azure Kimlik Bilgileri) açılan menüsünden önceki adımda eklediğiniz hizmet sorumlusu kimlik bilgilerini seçin.
9. **App Configuration** (Uygulama Yapılandırması) bölümündeki açılan menüsünden kaynak grubunuzun ve web uygulamanızın adını seçin.
10. **Publish via Docker** (Docker aracılığıyla yayımla) radyo düğmesini seçin.
11. **Dockerfile path** (Dockerfile yolu) bölümüne `complete/Dockerfile` yazın.
12. **Docker registry URL** (Docker kayıt defteri URL’si) alanına `https://jenkinsregistry.azurecr.io` yazın.
13. **Registry Credentials** (Kayıt Defteri Kimlik Bilgileri) öğesinin yanındaki **Add** (Ekle) öğesini seçin. 
14. **Username** (Kullanıcı adı) alanına oluşturduğunuz Azure Container Registry’nin yönetici kullanıcı adını girin.
15. **Password** (Parola) alanına Azure Container Registry parolasını girin. Kullanıcı adınızı ve parolanızı Azure portaldan veya aşağıdaki CLI komutunu kullanarak alabilirsiniz:

    ```azurecli-interactive
    az acr credential show -n jenkinsregistry
    ```
    ![Kapsayıcı kayıt defteri kimlik bilgilerinizi ekleme](media/jenkins-java-quickstart/enter_acr_credentials.png)
15. Kimlik bilgilerini kaydetmek için **Add** (Ekle) öğesini seçin.
16. **Publish an Azure Web App** (Azure Web Uygulaması Yayımla) bölümündeki **App Configuration** (Uygulama Yapılandırması) panelinde bulunan **Registry credentials** (Kayıt defteri kimlik bilgileri) alanından yeni oluşturduğunuz kimlik bilgilerini seçin. Tamamlanmış derleme sonrası eylem aşağıdaki resimdeki gibi görünmelidir:   
    ![Azure App Service dağıtımı için derleme sonrası eylem yapılandırması](media/jenkins-java-quickstart/appservice_plugin_configuration.png)
17. İş yapılandırmasını kaydetmek için **Save** (Kaydet) öğesini seçin.

## <a name="deploy-the-app-from-github"></a>Uygulamayı GitHub’dan dağıtma

1. Örnek uygulamayı Azure’a dağıtmak için Jenkins projesinden **Build Now** (Şimdi Derle) öğesini seçin.
2. Derleme tamamlandıktan sonra uygulamanıza Azure’daki yayımlama URL’sinden ulaşabilirsiniz; örneğin, http://myjavaapp.azurewebsites.net.   
   ![Dağıtılan uygulamanızı Azure’da görüntüleme](media/jenkins-java-quickstart/hello_docker_world_unedited.png)

## <a name="push-changes-and-redeploy"></a>Değişiklikleri gönderme ve yeniden dağıtma

1. Web üzerindeki GitHub çatalınızdan `complete/src/main/java/Hello/Application.java` dizinine gidin. GitHub arabiriminin sağ tarafındaki **Edit this file** (Bu dosyayı düzenle) bağlantısını seçin.
2. `home()` metodunda aşağıdaki değişikliği yapın ve değişikliği deponun ana dalına işleyin.
   
    ```java
    return "Hello Docker World on Azure";
    ```
3. Jenkins üzerinde deponun `master` dalındaki yeni işlemenin tetiklediği yeni bir derleme başlatılır. Derleme tamamlandıktan sonra uygulamanızı Azure’da yeniden yükleyin.     
      ![Dağıtılan uygulamanızı Azure’da görüntüleme](media/jenkins-java-quickstart/hello_docker_world.png)

## <a name="troubleshooting-the-jenkins-plugin"></a>Jenkins eklentisiyle ilgili sorunlarını giderme

Jenkins eklentileriyle ilgili hatalarla karşılaşırsanız [Jenkins JIRA](https://issues.jenkins-ci.org/) sayfasında söz konusu bileşenle ilgili sorun bildirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure VM’lerini derleme aracısı olarak kullanma](/azure/jenkins/jenkins-azure-vm-agents)