---
title: Öğretici - Github'dan Jenkins ile Azure App Service'e dağıtma
description: Jenkins'i sürekli tümleştirme (CI) için GitHub ve sürekli dağıtım (CD) Azure App Service'te Java web uygulamaları için ayarlama
services: jenkins
ms.service: jenkins
author: tomarchermsft
ms.author: tarcher
manager: jeconnoc
ms.topic: tutorial
ms.date: 11/15/2018
ms.openlocfilehash: 90f89f9ffb1d55e7621c87f168375251c78d9730
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60641827"
---
# <a name="tutorial-deploy-from-github-to-azure-app-service-with-jenkins-continuous-integration-and-deployment"></a>Öğretici: Jenkins sürekli tümleştirme ve dağıtım ile Github'dan Azure App Service'e dağıtma

Bu öğretici için github'dan örnek bir Java web uygulaması dağıtır [Linux üzerinde Azure App Service'te](/azure/app-service/containers/app-service-linux-intro) kurarak sürekli tümleştirme (CI) ve Jenkins sürekli dağıtım (CD). Github'a işlemeleri ileterek uygulamayı güncelleştirdiğinizde, Jenkins, otomatik olarak oluşturur ve uygulamanızı Azure App Service'e yeniden yayımlar. Bu öğreticideki örnek uygulaması kullanarak geliştirilmiştir [Spring Boot](https://projects.spring.io/spring-boot/) framework. 

![Genel Bakış](media/tutorial-jenkins-deploy-web-app-azure-app-service/overview.png)

Bu öğreticide, bu görevleri tamamlamak:

> [!div class="checklist"]
> * Yükleme Jenkins eklentileri Github'dan oluşturmanıza yardımcı olacak Azure App Service'e dağıtma ve diğer ilgili görevler.
> * Bu nedenle çalışma kopyası olması örnek GitHub depo çatalı oluşturma.
> * Jenkins Github'a bağlanın.
> * Jenkins Azure kimlik bilgilerinizi kullanarak olmadan erişebilmesi için bir Azure hizmet sorumlusu oluşturun.
> * Jenkins ile hizmet sorumlunuzu ekleyin.
> * Derlemeler ve GitHub uygulamada her güncelleştirdiğinizde örnek uygulama dağıtan Jenkins işlem hattı oluşturursunuz.
> * Derleme ve dağıtım için Jenkins işlem hattınızı oluşturun.
> * Jenkins işlem hattınızı derleme ve dağıtım komut gelin.
> * Elle bir yapı çalıştırarak, örnek uygulamanızı Azure'a dağıtın.
> * Uygulama güncelleştirmesi, derleme ve Azure'a yeniden dağıtmak için Jenkins tetikler github'da gönderin.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için bu öğeler gerekir:

* A [Jenkins](https://jenkins.io/) sunucusu ile bir Azure Linux VM'de yüklü Java Development Kit (JDK) ve Maven araçları

  Bir Jenkins sunucusu yoksa, şimdi Azure portalında aşağıdaki adımları tamamlayın: [Azure Linux VM'de bir Jenkins sunucusu oluşturma](/azure/jenkins/install-jenkins-solution-template)

* A [GitHub](https://github.com) örnek Java web uygulaması için bir çalışma kopyası (Çatal) alabilmeniz için hesap. 

* [Azure CLI](/cli/azure/install-azure-cli),'nden ya da yerel komut satırınızda çalıştırabileceğiniz veya [Azure Cloud Shell](/azure/cloud-shell/overview)

## <a name="install-jenkins-plug-ins"></a>Jenkins eklentilerini yükleme

1. Bu konumda, Jenkins web konsoluna oturum açın:

   `https://<Jenkins-server-name>.<Azure-region>.cloudapp.azure.com`

1. Jenkins ana sayfasında göze seçin **Jenkins'i Yönet** > **Eklentileri Yönet**.

   ![Jenkins eklentileri yönetme](media/tutorial-jenkins-deploy-web-app-azure-app-service/manage-jenkins-plugins.png)

1. Üzerinde **kullanılabilir** sekmesinde, bu eklentilerin seçin:

   - [Azure App Service](https://plugins.jenkins.io/azure-app-service)
   - [GitHub dal kaynağı](https://plugins.jenkins.io/github-branch-source)
   - Jenkins [ortam Enjektörü eklentisi](https://plugins.jenkins.io/envinject)
   - [Azure kimlik bilgileri](https://plugins.jenkins.io/azure-credentials)

   Bu eklentilerin görünmüyorsa bunların zaten yüklü kontrol ederek emin olun **yüklü** sekmesi.

1. Seçilen eklentilerini yüklemek için seçin **hemen indirin ve yeniden başlatma işleminden sonra yükleme**.

1. Jenkins menüsünde bitirdikten sonra seçin **Jenkins'i Yönet** böylece sonraki adımlar için Jenkins Yönetim sayfasına dönün.

## <a name="fork-sample-github-repo"></a>Örnek GitHub depo çatalı oluşturma

1. [Oturum açmak için Spring Boot örnek uygulaması için GitHub deposunu](https://github.com/spring-guides/gs-spring-boot). 

1. Github'da sağ üst köşedeki seçin **çatal**.

   ![GitHub örnek deposundan çatal](media/tutorial-jenkins-deploy-web-app-azure-app-service/fork-github-repo.png)

1. GitHub hesabınızı seçin ve çatal tamamlamak için yönergeleri izleyin.

Ardından, Jenkins'i GitHub kimlik bilgilerinizle ayarlayın.

## <a name="connect-jenkins-to-github"></a>Jenkins Github'a bağlanma

Jenkins, GitHub'ı izleme ve web uygulamanıza GitHub çatalınızdaki yeni işlemeleri itilir yanıt için etkinleştirme [GitHub Web kancası](https://developer.github.com/webhooks/) jenkins'te.

> [!NOTE]
> 
> Bu adımları kişisel erişim belirteci Jenkins, GitHub kullanıcı adınızı ve parolanızı kullanarak, GitHub ile çalışmak kimlik bilgilerini oluşturun. 
> Ancak, GitHub hesabınızda iki öğeli kimlik doğrulaması kullanıyorsa, Github'da belirtecinizi oluşturmak ve bu belirteci kullanmayı Jenkins'i ayarlama. 
> Daha fazla bilgi için [Jenkins GitHub eklentisi](https://wiki.jenkins.io/display/JENKINS/GitHub+Plugin) belgeleri.

1. Gelen **Jenkins'i Yönet** sayfasında **yapılandırma sistemi**. 

   ![Sistem Yapılandırma](media/tutorial-jenkins-deploy-web-app-azure-app-service/manage-jenkins-configure-system.png)

1. İçinde **GitHub** bölümünde, GitHub sunucunuz için ayrıntıları sağlayın. Gelen **GitHub Sunucusu Ekle** listesinden **GitHub sunucu**. 

   ![GitHub sunucusu ekleme](media/tutorial-jenkins-deploy-web-app-azure-app-service/add-GitHub-server.png)

1. Varsa **yönetme kancaları** seçili özelliği olmadığından, bu özelliği seçin. Seçin **Gelişmiş** böylece diğer ayarları belirtebilirsiniz. 

   ![Diğer ayarlar için "Gelişmiş" seçin](media/tutorial-jenkins-deploy-web-app-azure-app-service/advanced-GitHub-settings.png)

1. Gelen **ek GitHub işlemleri yönetmenizi** listesinden **için belirteci oturum açma adı ve parola dönüştürme**.

   !["Ek GitHub Eylemler Yönet" seçin](media/tutorial-jenkins-deploy-web-app-azure-app-service/manage-additional-actions.png)

1. Seçin **oturum açma ve parola** GitHub kullanıcı adınızı ve parolanızı girmenizi sağlar. İşiniz bittiğinde seçin **belirteç kimlik bilgileri oluşturma**, oluşturan bir [GitHub kişisel erişim belirteci (PAT)](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/).   

   ![GitHub PAT oturum açma ve parola oluşturun](media/tutorial-jenkins-deploy-web-app-azure-app-service/create-github-token-credentials.png)

1. İçinde **GitHub sunucu** bölümü, gelen **kimlik bilgilerini** listesinde, yeni belirtecinizi seçin. Söz konusu kimlik doğrulamasını seçerek çalıştığını denetleyin **Bağlantıyı Sına**.

   ![GitHub sunucusuna bir bağlantı ile yeni bir PAT denetleyin](media/tutorial-jenkins-deploy-web-app-azure-app-service/check-github-connection.png)

Ardından, Azure kimlik doğrulaması ve Azure kaynaklarına erişim için Jenkins kullanan bir hizmet sorumlusu oluşturun.

## <a name="create-service-principal"></a>Hizmet sorumlusu oluşturma

Bir sonraki bölümde, Github'dan uygulamanızı oluşturur ve uygulamanızı Azure App Service'e dağıtan bir Jenkins işlem hattı işinde oluşturun. Jenkins Azure kimlik bilgilerinizi girmeden erişmek, oluşturmak için bir [hizmet sorumlusu](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals) Jenkins için Azure Active Directory'de. Bir hizmet sorumlusu Jenkins Azure kaynaklarına erişimi kimlik doğrulaması için kullanabileceğiniz ayrı bir kimliktir. Bu hizmet sorumlusunu oluşturmak için Azure CLI komutunu çalıştırın. [ **`az ad sp create-for-rbac`** ](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest), yerel komut satırı veya Azure Cloud Shell, örneğin: 

```azurecli-interactive
az ad sp create-for-rbac --name "yourAzureServicePrincipalName" --password yourSecurePassword
```

Hizmet asıl adı çift tırnak kullandığınızdan emin olun. Ayrıca, temel güçlü bir parola oluşturmak [Azure Active Directory parola kurallarını ve kısıtlamalarını](/azure/active-directory/active-directory-passwords-policy). Bir parola sağlamazsanız, Azure CLI'yı bir parola oluşturur. 

Tarafından oluşturulan çıktı şu şekildedir **`create-for-rbac`** komutu: 

```json
{
   "appId": "yourAzureServicePrincipal-ID", // A GUID such as AAAAAAAA-AAAA-AAAA-AAAA-AAAAAAAAAAAA
   "displayName": "yourAzureServicePrincipalName", // A user-friendly name for your Azure service principal
   "name": "http://yourAzureServicePrincipalName",
   "password": "yourSecurePassword",
   "tenant": "yourAzureActiveDirectoryTenant-ID" // A GUID such as BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBBB
}
```

> [!TIP]
> 
> Bu kimlik bir hizmet sorumlusu zaten varsa, bunun yerine yeniden kullanabilirsiniz.
> Kimlik doğrulaması için hizmet sorumlusu değerleri sağlayarak kullanırsanız `appId`, `password`, ve `tenant` özellik değerleri. 
> Mevcut bir hizmet sorumlusu ararken kullanmak `displayName` özellik değeri.

## <a name="add-service-principal-to-jenkins"></a>Jenkins ile hizmet sorumlusu ekleme

1. Jenkins ana sayfasında göze seçin **kimlik bilgilerini** > **sistem**. 

1. Üzerinde **sistem** sayfasındaki **etki alanı**seçin **(sınırsız) genel kimlik bilgileri**.

1. Sol menüden **kimlik bilgileri ekleme**.

1. Gelen **tür** listesinden **Azure hizmet sorumlusu**.

1. Bu adımda tablosu tarafından açıklanan özellikler, hizmet sorumlusu ve Azure aboneliğinize için bilgileri sağlayın:

   ![Azure hizmet sorumlusu kimlik bilgileri ekleme](media/tutorial-jenkins-deploy-web-app-azure-app-service/add-service-principal-credentials.png)

   | Özellik | Değer | Açıklama | 
   |----------|-------|-------------| 
   | **Abonelik kimliği** | <*yourAzureSubscription kimliği*> | Azure aboneliğiniz için GUID değeri <p>**İpucu**: Azure abonelik Kimliğinizi bilmiyorsanız, komut satırından veya Cloud shell'de bu Azure CLI komutunu çalıştırın ve ardından `id` GUID değeri: <p>`az account list` | 
   | **İstemci kimliği** | <*yourAzureServicePrincipal kimliği*> | `appId` Azure hizmet sorumlunuz için daha önce oluşturulan GUID değeri | 
   | **İstemci gizli anahtarı** | <*yourSecurePassword*> | `password` , Değeri veya "gizli", Azure hizmet sorumlusu için sağlanan | 
   | **Kiracı kimliği** | <*yourAzureActiveDirectoryTenant kimliği*> | `tenant` Azure Active Directory kiracınız için GUID değeri | 
   | **ID** | <*yourAzureServicePrincipalName*> | `displayName` , Azure hizmet sorumlusu için değer | 
   |||| 

1. Hizmet sorumlunuzu çalıştığını doğrulamak için şunu seçin **doğrulayın hizmet sorumlusu**. İşiniz bittiğinde seçin **Tamam**.

Ardından, oluşturur ve uygulamanızı dağıtan Jenkins işlem hattı oluşturursunuz.

## <a name="create-jenkins-pipeline"></a>Jenkins işlem hattı oluşturma

Jenkins, oluşturma ve uygulamanızı dağıtmak için işlem hattı işinde oluşturun.

1. Jenkins giriş sayfanıza dönün ve seçin **yeni öğe**. 

   !["Yeni öğe" seçin](media/tutorial-jenkins-deploy-web-app-azure-app-service/jenkins-select-new-item.png)

1. Bir ad belirtin, işlem hattı işiniz için örneğin, "My-Java-Web-App" ve select **işlem hattı**. Alt kısmında seçin **Tamam**.  

   !["İşlem hattı" seçin](media/tutorial-jenkins-deploy-web-app-azure-app-service/jenkins-select-pipeline.png)

1. Jenkins Azure için kendi kimlik bilgilerini kullanmadan ucunuzun, hizmet sorumlusu ile Jenkins'i ayarlayın.

   1. Üzerinde **genel** sekmesinde **çalıştırma için ortam hazırlama**. 

   1. İçinde **özellikleri içerik** kutusu göründüğünde, bu görev değişkenlerine ve değerlerine ekleyin. 

      ```ini
      AZURE_CRED_ID=yourAzureServicePrincipalName
      RES_GROUP=yourWebAppAzureResourceGroupName
      WEB_APP=yourWebAppName
      ```

      ![Seç "çalıştırma için ortam Hazırlama" ve ortam değişkenlerini ayarlama](media/tutorial-jenkins-deploy-web-app-azure-app-service/prepare-environment-for-run.png)

1. İşiniz bittiğinde **Kaydet**’i seçin.

Ardından, Jenkins için derleme ve dağıtım betikleri oluşturun.

## <a name="create-build-and-deployment-files"></a>Derleme ve dağıtım dosyaları oluşturma

Şimdi, Jenkins oluşturmak ve uygulamanızı dağıtmak için kullandığı dosyalarını oluşturun.

1. GitHub çatalınızın içinde `src/main/resources/` klasöründe adlı bu uygulama yapılandırma dosyası oluşturma `web.config`, bu XML içeriyor ancak değiştirin `$(JAR_FILE_NAME)` ile `gs-spring-boot-0.1.0.jar`:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
      <system.webServer>
         <handlers>
            <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
         </handlers>
         <httpPlatform processPath="%JAVA_HOME%\bin\java.exe" arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\${JAR_FILE_NAME}&quot;"></httpPlatform>
      </system.webServer>
   </configuration>
   ```

1. GitHub çatalınızın kök klasöründe adlı bu derleme ve dağıtım betiğini oluşturma `Jenkinsfile`, bu metni içeren ([burada kaynak github'da](https://github.com/Microsoft/todo-app-java-on-azure/blob/master/doc/resources/jenkins/Jenkinsfile-webapp-se)):

   ```groovy
   node {
      stage('init') {
         checkout scm
      }
      stage('build') {
         sh '''
            mvn clean package
            cd target
            cp ../src/main/resources/web.config web.config
            cp todo-app-java-on-azure-1.0-SNAPSHOT.jar app.jar 
            zip todo.zip app.jar web.config
         '''
      }
      stage('deploy') {
         azureWebAppPublish azureCredentialsId: env.AZURE_CRED_ID,
         resourceGroup: env.RES_GROUP, appName: env.WEB_APP, filePath: "**/todo.zip"
      }
   }
   ```

1. Hem işleme `web.config` ve `Jenkinsfile` dosyaları, GitHub çatalını oluşturmanız ve değişikliklerinizi gönderme.

## <a name="point-pipeline-at-script"></a>Kod noktası ardışık

Şimdi, Jenkins kullanmak istediğiniz derleme ve dağıtım betiğini belirtin.

1. Jenkins, önceden oluşturduğunuz işlem hattı işinizi seçin. 

   ![Web uygulamanız için işlem hattı işi seçin](media/tutorial-jenkins-deploy-web-app-azure-app-service/select-pipeline-job.png)

1. Sol menüden **yapılandırma**.

1. Üzerinde **işlem hattı** sekmesi, gelen **tanımı** listesinden **işlem hattı SCM betikten**.

   1. İçinde **SCM** görüntülenirse, seçin kutusu **Git** kaynak denetimi olarak. 

   1. İçinde **depoları** bölüm için **depo URL'si**, GitHub çatalınızın URL'sini girin, örneğin: 

      `https://github.com/<your-GitHub-username>/gs-spring-boot`

   1. İçin **kimlik bilgilerini**, önceden oluşturulmuş GitHub kişisel erişim belirtecinizi seçin.

   1. İçinde **betik yolu** kutusunda, "Jenkinsfile" betiğinizi yolunu ekleyin.

   İşiniz bittiğinde, işlem hattı tanımınızı şu örnekteki gibi görünür: 

   ![Kod noktası ardışık](media/tutorial-jenkins-deploy-web-app-azure-app-service/set-up-jenkins-github.png)

1. İşiniz bittiğinde **Kaydet**’i seçin.

Ardından, oluşturun ve uygulamanızı Azure App Service'e dağıtın. 

## <a name="build-and-deploy-to-azure"></a>Derleme ve Azure'a dağıtma

1. Ya da komut satırı veya Azure Cloud Shell, Azure CLI ile oluşturmak bir [Linux üzerinde Azure App Service web uygulaması](/azure/app-service/containers/app-service-linux-intro) burada Jenkins dağıtır web uygulamanızı bir derleme tamamlandıktan sonra. Web uygulamanızı benzersiz bir ad olduğundan emin olun.

   ```azurecli-interactive
   az group create --name yourWebAppAzureResourceGroupName --location yourAzureRegion
   az appservice plan create --name yourLinuxAppServicePlanName --resource-group yourWebAppAzureResourceGroupName --is-linux
   az webapp create --name yourWebAppName --resource-group yourWebAppAzureResourceGroupName --plan yourLinuxAppServicePlanName --runtime "java|1.8|Tomcat|8.5"
   ```

   Bu Azure CLI komutları hakkında daha fazla bilgi için şu sayfalara bakın:

   * [**`az group create`**](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create)

   * [**`az appservice plan create`**](https://docs.microsoft.com/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create)

   * [**`az webapp create`**](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest#az-webapp-create)

1. Jenkins, işlem hattı işinizi seçin ve seçin **artık yapı**.

   Derleme tamamlandıktan sonra Jenkins artık Örneğin canlı yayını URL'de azure'da uygulamanızı dağıtır: 

   `http://<your-Java-web-app>.azurewebsites.net`

   ![Dağıtılan uygulamanızı Azure’da görüntüleme](media/tutorial-jenkins-deploy-web-app-azure-app-service/greetings-unedited.png)

## <a name="push-changes-and-redeploy"></a>Değişiklikleri gönderme ve yeniden dağıtma

1. Web tarayıcınızda web uygulamanızın GitHub çatalınıza şu konuma gidin:

   `complete/src/main/java/Hello/Application.java`
   
1. Github'da sağ üst köşeden seçin **bu dosyayı Düzenle**.

1. Bu değişiklik `commandLineRunner()` yöntemi ve işleme deponun değişikliği `master` dal. Bu işlemede `master` dal Jenkins derleme başlar. 
   
   ```java
   System.out.println("Let's inspect the beans provided by Spring Boot on Azure");
   ```

1. Yapı tamamlanana ve Azure için Jenkins yeniden dağıtır sonra güncelleştirmeyi gösterdiğini uygulamanızı yenileyin.

   ![Dağıtılan uygulamanızı Azure’da görüntüleme](media/tutorial-jenkins-deploy-web-app-azure-app-service/greetings-edited.png)

## <a name="troubleshooting-the-jenkins-plug-in"></a>Jenkins eklentisi sorunlarını giderme

Tüm hatalar Jenkins eklentiler ile karşılaşırsanız, sorunu bildirin [Jenkins JIRA](https://issues.jenkins-ci.org/) belirli bileşeni.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure VM’lerini derleme aracısı olarak kullanma](/azure/jenkins/jenkins-azure-vm-agents)
