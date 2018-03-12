---
title: "Web uygulamalarınızı Azure'a dağıtmak için Jenkins kullanın | Microsoft Docs"
description: "Sürekli Tümleştirme kurup Github'dan Jenkins ve Docker kullanarak, Java web uygulamaları için Azure App Service'e ayarlayın."
author: rloutlaw
manager: douge
ms.service: jenkins
ms.search.scope: 
ms.devlang: java
ms.topic: article
ms.workload: web
ms.date: 08/02/2017
ms.author: routlaw
ms.custom: Jenkins, devcenter
ms.openlocfilehash: b2606acba341d4cfbc16314048e134fa30ff8606
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="set-up-continuous-integration-and-deployment-to-azure-app-service-with-jenkins"></a>Sürekli tümleştirme ve dağıtım Azure App Service'e Jenkins ile ayarlama

Bu öğretici sürekli tümleştirme kurup ayarlar ve örnek bir Java web uygulaması (CI/CD) dağıtımını geliştirilmiş ile [yay önyükleme](http://projects.spring.io/spring-boot/) framework [Azure App Service Web uygulaması Linux'ta](/azure/app-service/containers/app-service-linux-intro) Jenkins kullanarak.

Bu öğreticide aşağıdaki görevleri gerçekleştirmeniz gerekecektir:

> [!div class="checklist"]
> * Jenkins Azure App Service'e dağıtmak için gereken eklentiler yükleyin.
> * Yeni bir yürütme gönderildiğinde GitHub deposuna Docker görüntüleri oluşturmak için bir Jenkins işi tanımlayın.
> * Linux için yeni bir Azure Web uygulaması tanımlayabilir ve Azure kapsayıcı kayıt defterine gönderilen Docker görüntülerini dağıtmak için yapılandırabilirsiniz.
> * Azure App Service Jenkins eklentisini yapılandırın.
> * Örnek uygulamayı el ile bir yapı ile Azure App Service'e dağıtın.
> * Jenkins derlemeyi tetiklemeyi ve web uygulaması için GitHub değişiklikleri ileterek güncelleştirin.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* [Jenkins](https://jenkins.io/) yapılandırılmış JDK ve Maven araçları ile. Jenkins sistem yoksa oluşturmak artık Azure içinde [Jenkins çözüm şablonu](/azure/jenkins/install-jenkins-solution-template).
* Bir [GitHub](https://github.com) hesabı.
* [Azure CLI 2.0](/cli/azure), yerel komut satırından veya buna [Azure bulut Kabuğu](/azure/cloud-shell/overview)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="install-jenkins-plug-ins"></a>Jenkins eklentilerini yükleyin

1. Jenkins web konsoluna bir web tarayıcısı açın ve seçin **yönetmek Jenkins** sol taraftaki menüden seçip **eklentileri yönetme**.
2. Seçin **kullanılabilir** sekmesi.
3. Arayın ve aşağıdaki eklentilerin yanındaki onay kutusunu seçin:   

    - [Azure uygulama hizmeti eklentisi](https://plugins.jenkins.io/azure-app-service)
    - [GitHub şube kaynağı eklentisi](https://plugins.jenkins.io/github-branch-source)

    Eklenti gösterilmezse olmayan zaten yüklü denetleyerek emin olun **yüklü** sekmesi.

1. Seçin **şimdi yükleyip yeniden başlatma sonrasında** Jenkins yapılandırmanızda Eklentileri etkinleştirmek için.

## <a name="configure-github-and-jenkins"></a>GitHub ve Jenkins yapılandırın

Jenkins alacak şekilde ayarlanmış [GitHub Web kancası](https://developer.github.com/webhooks/) yeni işlemeleri zaman gönderilen hesabınızda depo için.

1. Seçin **Jenkins yönetmek**, ardından **sistem yapılandırma**. İçinde **GitHub** bölümünde, emin olun **yönetmek kancaları** seçilir ve ardından **ek GitHub işlemleri yönetmenizi** ve **dönüştürme oturum açma ve belirteç için parolayı**.
2. Seçin **oturum açma ve parola** radyo düğmesinin ve GitHub kullanıcı adını ve parolayı girin. Seçin **belirteç kimlik bilgileri oluşturun** yeni [GitHub kişisel erişim belirteci](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).   
   ![GitHub PAT oturum açma ve parola oluşturun](media/jenkins-java-quickstart/create_github_credentials.png)
3.  Yeni oluşturulan belirteçten seçin **kimlik bilgileri** GitHub sunucuları yapılandırmasında açılır. Seçin **Bağlantıyı Sına** kimlik doğrulaması çalıştığını doğrulayın.   
   ![PAT yapılandırıldıktan sonra GitHub bağlantı doğrulanamıyor](media/jenkins-java-quickstart/verify_github_connection.png)

> [!NOTE]
> GitHub hesabınızda iki öğeli kimlik doğrulaması etkin varsa, GitHub üzerinde belirteç oluşturmak ve kullanmak için Jenkins yapılandırın. Gözden geçirme [Jenkins GitHub eklenti](https://wiki.jenkins.io/display/JENKINS/Github+Plugin) tam Ayrıntılar için belgelerine bakın.

## <a name="fork-the-sample-repo-and-create-a-jenkins-job"></a>Örnek depoyu çatallaştırma ve Jenkins işi oluşturma 

1. Açık [yay önyükleme örnek uygulama deposu](https://github.com/spring-guides/gs-spring-boot-docker) ve seçerek kendi GitHub hesabı çatallaştırma **çatalı** sağ üst köşesindeki.   
    ![Github'dan çatalı](media/jenkins-java-quickstart/fork_github_repo.png)
1. Jenkins web konsolunda seçin **yeni öğe**, bir ad verin **MyJavaApp**seçin **Serbest stilde proje**seçeneğini belirleyip **Tamam**.   
    ![New Jenkins Freestyle Project](media/jenkins-java-quickstart/jenkins_freestyle.png)
2. Altında **genel** bölümünde, select **GitHub** proje ve https://github.com/raisa/gs-spring-boot-docker gibi çatallanmış depodaki URL'nizi girin
3. Altında **kaynak kodu Yönetimi** bölümünde, select **Git**, forked depodaki girin `.git` https://github.com/raisa/gs-spring-boot-docker.git gibi URL
4. **Derleme Tetikleyicileri** bölümünden **GITScm yoklaması için GitHub kanca tetikleyicisi**’ni seçin.
5. Altında **yapı** bölümünde, select **Ekle derleme adımı** ve **en üst düzey Maven hedefleri çağırma**. Girin `package` içinde **hedefleri** alan.
6. **Kaydet**’i seçin. İşinizi seçerek sınayabilirsiniz **şimdi yapı** proje sayfasından.

## <a name="configure-azure-app-service"></a>Azure uygulama hizmeti yapılandırın 

1. Azure CLI kullanarak veya [bulut Kabuk](/azure/cloud-shell/overview), yeni bir [Linux üzerinde Web uygulaması](/azure/app-service/containers/app-service-linux-intro). Bu öğreticide web uygulaması adı `myJavaApp`, ancak kendi uygulama için benzersiz bir ad kullanmanız gerekir.
   
    ```azurecli-interactive
    az group create --name myResourceGroupJenkins --location westus
    az appservice plan create --is-linux --name myLinuxAppServicePlan --resource-group myResourceGroupJenkins 
    az webapp create --name myJavaApp --resource-group myResourceGroupJenkins --plan myLinuxAppServicePlan --runtime "java|1.8|Tomcat|8.5"
    ```

2. Oluşturma bir [Azure kapsayıcı kayıt defteri](/azure/container-registry/container-registry-intro) Jenkins tarafından oluşturulmuş Docker görüntüleri saklamak için. Bu öğreticide kullanılan kapsayıcı kayıt defteri adı `jenkinsregistry`, ancak kendi kapsayıcı kayıt defteri için benzersiz bir ad kullanmanız gerekir. 

    ```azurecli-interactive
    az acr create --name jenkinsregistry --resource-group myResourceGroupJenkins --sku Basic --admin-enabled
    ```
3. Kapsayıcı kayıt defterine gönderilen Docker görüntüleri çalıştırmak için web uygulamasını yapılandırma ve bağlantı noktası 8080 isteklerini dinler kapsayıcıda çalışan uygulama belirtin.   

    ```azurecli-interactive
    az webapp config container set -c jenkinsregistry/webapp --resource-group myResourceGroupJenkins --name myJavaApp
    az webapp config appsettings set --resource-group myResourceGroupJenkins --name myJavaApp --settings PORT=8080
    ```

## <a name="configure-the-azure-app-service-jenkins-plug-in"></a>Azure App Service Jenkins eklentisi yapılandırma

1. Jenkins web konsolunda seçin **MyJavaApp** , oluşturulan işi ve ardından **yapılandırma** sayfanın sol taraftaki üzerinde.
2. Aşağı kaydırın **oluşturma sonrası eylemleri**seçeneğini belirleyip **oluşturma sonrası eylem eklemek** ve **Azure Web uygulaması yayımlama**.
3. Altında **Azure profili yapılandırması**seçin **Ekle** yanına **Azure kimlik bilgilerini** ve **Jenkins**.
4. İçinde **kimlik bilgilerini eklemek** iletişim kutusunda **Microsoft Azure hizmet sorumlusu** gelen **türü** açılır.
5. Azure CLI üzerinden bir Active Directory Hizmet sorumlusu oluşturmak veya [bulut Kabuk](/azure/cloud-shell/overview).
    
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
6. Hizmet sorumlusu içine arasında kimlik bilgilerini girin **kimlik bilgileri ekleyin** iletişim. Azure abonelik Kimliğinizi bilmiyorsanız, CLI üzerinden sorgulama yapabilirsiniz:
     
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

    ![Azure hizmet sorumlusu yapılandırın](media/jenkins-java-quickstart/azure_service_principal.png)
6. Hizmet sorumlusu seçerek Azure ile kimlik doğrulaması doğrulayın **doğrulayın hizmet sorumlusu**. 
7. Seçin **Ekle** kimlik bilgilerini kaydetmek için.
8. Yeni eklenen hizmet asıl kimlik bilgisi seçin **Azure kimlik bilgilerini** açılan geri olduğunda **Azure Web uygulaması yayımlama** yapılandırma.
9. İçinde **uygulama yapılandırması**web açılan uygulama adını ve kaynak grubunu seçin.
10. Seçin **Docker aracılığıyla yayımlama** radyo düğmesi.
11. Girin `complete/Dockerfile` için **Dockerfile yolu**.
12. Girin `https://jenkinsregistry.azurecr.io` içinde **Docker kayıt defteri URL** alan.
13. Seçin **Ekle** yanına **kayıt defteri kimlik**. 
14. Azure kapsayıcı kayıt defteri oluşturduğunuz için yönetici kullanıcı adı girin **kullanıcıadı**.
15. Azure kapsayıcı kayıt defterinde için parolayı girin **parola** alan. Kullanıcı adı ve parola Azure portalından veya aşağıdaki CLI komutu aracılığıyla elde edebilirsiniz:

    ```azurecli-interactive
    az acr credential show -n jenkinsregistry
    ```
    ![Kapsayıcı kayıt defteri kimlik bilgilerinizi ekleyin](media/jenkins-java-quickstart/enter_acr_credentials.png)
15. Seçin **Ekle** kimlik bilgisi kaydetmek için.
16. Yeni oluşturulan kimlik bilgisinden seçin **kayıt defteri kimlik** açılan **uygulama yapılandırması** için panel **Azure Web uygulaması yayımlama**. Tamamlanmış oluşturma sonrası eylem aşağıdaki görüntü gibi görünmelidir:   
    ![Azure uygulama hizmeti dağıtmak için POST yapı eylem yapılandırması](media/jenkins-java-quickstart/appservice_plugin_configuration.png)
17. Seçin **kaydetmek** iş yapılandırmasını kaydetmek için.

## <a name="deploy-the-app-from-github"></a>GitHub uygulamasını dağıtın

1. Jenkins projeden seçin **şimdi yapı** örnek uygulamayı Azure'a dağıtmak için.
2. Derleme işlemi tamamlandıktan sonra URL'de kaydının yayımlama, örneğin http://myjavaapp.azurewebsites.net azure'da canlı bir uygulamadır.   
   ![Azure üzerinde dağıtılan uygulamanızı görüntülemek](media/jenkins-java-quickstart/hello_docker_world_unedited.png)

## <a name="push-changes-and-redeploy"></a>Değişiklikleri gönderin ve yeniden dağıtın

1. Web üzerinde Github çatalı Gözat `complete/src/main/java/Hello/Application.java`. Seçin **bu dosyayı düzenlemek** GitHub arabirimi sağ tarafında bağlantısından.
2. Aşağıdaki değişiklik `home()` yöntemi ve yürütme depoyu ait ana dala geçin.
   
    ```java
    return "Hello Docker World on Azure";
    ```
3. Yeni bir derleme üzerinde yeni tamamlama tarafından tetiklenen Jenkins başlatır `master` depoyu dalı. Tamamlandığında, uygulamanızı Azure üzerinde yeniden yükleyin.     
      ![Azure üzerinde dağıtılan uygulamanızı görüntülemek](media/jenkins-java-quickstart/hello_docker_world.png)
  
## <a name="next-steps"></a>Sonraki adımlar

- [Azure yapı aracıları gibi VM'ler kullanın](/azure/jenkins/jenkins-azure-vm-agents)
- [İşlerini ve ardışık düzen Azure CLI ile kaynakları yönetme](/azure/jenkins/execute-cli-jenkins-pipeline)
 
