---
title: Jenkins Azure Kubenetes Service için Azure geliştirme alanları eklentisini kullanma
description: Azure geliştirme alanları eklentisi bir sürekli tümleştirme işlem hattında kullanmayı öğrenin.
author: tomarchermsft
ms.author: tarcher
ms.service: jenkins
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/18/2019
ms.openlocfilehash: 8c47b8caf2d289ed17647b8003cc702156f3cddb
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67592087"
---
<!-- GMinchAQ, 06/18/19 -->

# <a name="tutorial-using-the-azure-dev-spaces-plugin-for-jenkins-with-azure-kubenetes-service"></a>Öğretici: Jenkins Azure Kubenetes Service için Azure geliştirme alanları eklentisini kullanma 

Azure geliştirme alanları çalıştırmalarınızı çoğaltmak veya bağımlılıkları sahte gerek kalmadan Azure Kubernetes Service (AKS) çalışan mikro hizmet uygulamanızı geliştirin ve test etmenizi sağlar. Jenkins için Azure geliştirme alanları eklentisi, sürekli tümleştirme ve teslim (CI/CD) işlem hattı geliştirme alanları kullanmanıza yardımcı olur.

Bu öğretici ayrıca Azure Container Registry (ACR) kullanır. ACR görüntülerini depolar ve Docker ve Helm yapıtları ACR görev oluşturur. ACR ve ACR görev yapısı neslin kullanarak Jenkins sunucunuzda Docker gibi ek yazılımlar yüklemek için gereksinimini ortadan kaldırır. 

Bu öğreticide, bu görevleri tamamlamak:

> [!div class="checklist"]
> * Azure geliştirme alanları etkin AKS kümesi oluşturma
> * Aks'ye bir çok hizmet uygulaması dağıtma
> * Jenkins sunucunuzu hazırlama
> * Kod değişiklikleri projeye birleştirmeden önce önizlemek için bir Jenkins işlem hattı, Azure geliştirme alanları eklentisini kullanma

Bu öğreticide, AKS, ACR, Azure geliştirme alanları, Jenkins Azure Hizmetleri çekirdeği Ara bilgi varsayılır [işlem hatları](https://jenkins.io/doc/book/pipeline/) ve eklenti ve GitHub. Kubectl ve Helm gibi araçları desteği ile temel bilgisi yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure hesabı. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* GitHub hesabı. Bir GitHub hesabı yoksa, oluşturun bir [ücretsiz bir hesap](https://github.com/) başlamadan önce.

* [Visual Studio Code](https://code.visualstudio.com/download) ile [Azure geliştirme alanları](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) uzantısı yüklü.

* [Azure CLI'yı yüklü](/cli/azure/install-azure-cli?view=azure-cli-latest), 2.0.43 sürümü veya üzeri.

* Bir Jenkins ana sunucusu. Bir Jenkins ana yoksa dağıtma [Jenkins](https://aka.ms/jenkins-on-azure) bu adımları izleyerek azure'da [hızlı](https://docs.microsoft.com/azure/jenkins/install-jenkins-solution-template). 

* Jenkins sunucusunu, bu öğreticinin ilerleyen bölümlerinde açıklandığı gibi Helm hem kubectl yüklü ve Jenkins hesabı için kullanılabilir olmalıdır.

* VS Code, VS Code Terminal veya WSL ve Bash. 


## <a name="create-azure-resources"></a>Azure kaynakları oluşturma

Bu bölümde, Azure kaynaklarını oluşturun:

* Bu öğretici için tüm Azure kaynakları içerecek bir kaynak grubu.
* Bir [Azure Kubernetes hizmeti](https://docs.microsoft.com/azure/aks/) (AKS) kümesi.
* Bir [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) (ACR görevleri kullanarak) oluşturun ve Docker görüntülerini depolamak için (ACR).

1. Bir kaynak grubu oluşturun.

    ```bash
    az group create --name MyResourceGroup --location westus2
    ```

2. AKS kümesi oluşturma. AKS kümesinde oluşturma bir [geliştirme alanları destekleyen bir bölge](https://docs.microsoft.com/azure/dev-spaces/#a-rapid,-iterative-kubernetes-development-experience-for-teams).

    ```bash
    az aks create --resource-group MyResourceGroup --name MyAKS --location westus2 --kubernetes-version 1.11.9 --enable-addons http_application_routing --generate-ssh-keys --node-count 1 --node-vm-size Standard_D1_v2
    ```

3. AKS geliştirme alanlarını kullanmak için yapılandırın.

    ```bash
    az aks use-dev-spaces --resource-group MyResourceGroup --name MyAKS
    ```
    Bu adım yükler `azds` CLI uzantısı.

4. Kapsayıcı kayıt defteri oluşturun.

    ```bash
    az acr create -n MyACR -g MyResourceGroup --sku Basic --admin-enabled true
    ```

## <a name="deploy-sample-apps-to-the-aks-cluster"></a>Örnek uygulamalar için AKS kümesi dağıtma

Bu bölümde, bir geliştirme yer ayarlayın ve son bölümde oluşturduğunuz AKS kümesi için örnek uygulama dağıtırsınız. Uygulaması iki bölümden oluşur *webfrontend* ve *mywebapi*. Her iki bileşenin bir geliştirme alanında dağıtılır. Bu öğreticinin ilerleyen bölümlerinde, Jenkins CI işlem hattında tetiklemek için bir çekme isteği mywebapi karşı göndermeniz.

Azure geliştirme alanları ile Azure geliştirme alanları ve hizmetli geliştirme kullanma hakkında daha fazla bilgi için bkz. [Java ile Azure geliştirme alanlarında başlama](https://docs.microsoft.com/azure/dev-spaces/get-started-java), ve [Azure geliştirme alanları ile birden çok hizmet geliştirme](https://docs.microsoft.com/azure/dev-spaces/multi-service-java). Bu öğreticiler buraya dahil edilmeyen ek arka plan bilgileri sağlar.

1. İndirme https://github.com/Azure/dev-spaces GitHub deposu.

2. Açık `samples/java/getting-started/webfrontend` VS code'da klasörü. (Hata ayıklama varlıkları eklemek veya projeyi geri yüklemek için tüm varsayılan istemleri yoksayabilirsiniz.)

3. Güncelleştirme `/src/main/java/com/ms/sample/webfrontend/Application.java` aşağıdaki gibi aramak için:

    ```java
    package com.ms.sample.webfrontend;

    import java.io.*;
    import java.net.*;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.web.bind.annotation.*;

    @SpringBootApplication
    @RestController
    public class Application {
        public static void main(String[] args) {
            SpringApplication.run(Application.class, args);
        }

        @RequestMapping(value = "/greeting", produces = "text/plain")
        public String greeting(@RequestHeader(value = "azds-route-as", required = false) String azdsRouteAs) throws Exception {
            URLConnection conn = new URL("http://mywebapi/").openConnection();
            conn.setRequestProperty("azds-route-as", azdsRouteAs); // propagate dev space routing header
            try (BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream())))
            {
                return "Hello from webfrontend and " + reader.lines().reduce("\n", String::concat);
            }
        }
    }
    ```

4. Tıklayın **görünümü** ardından **Terminal** VS Code tümleşik Terminalini açmak için.

5. Çalıştırma `azds prep` bir geliştirme alanında çalışması için uygulamanızı hazırlamak için komutu. Bu komutu çalıştırmanız gerekir `dev-spaces/samples/java/getting-started/webfrontend` uygulamanızın doğru şekilde hazırlamak için:

    ```bash
    azds prep --public
    ```

    Geliştirme alanları CLI'ın `azds prep` komutu, Docker ve Kubernetes varlıklar varsayılan ayarlarla oluşturur. Bu dosyalar projenin ömrü boyunca kalır ve bunlar özelleştirilebilir:

    * `./Dockerfile` ve `./Dockerfile.develop` uygulamanın kapsayıcı görüntüsü ve kaynak kodunu nasıl oluşturulur ve kapsayıcı içinde çalışan açıklanmaktadır.
    * `./charts/webfrontend` altındaki [Helm grafiği](https://helm.sh/docs/developing_charts/), kapsayıcının Kubernetes'de nasıl dağıtıldığını açıklar.
    * `./azds.yaml` Azure geliştirme alanları yapılandırma dosyasıdır.

    Daha fazla bilgi için [nasıl Azure geliştirme alanları çalışır ve olan yapılandırılmış](https://docs.microsoft.com/azure/dev-spaces/how-dev-spaces-works).

6. Derleme ve AKS kullanarak uygulama çalıştırma `azds up` komutu:

    ```bash
    azds up
    ```
    <a name="test_endpoint"></a>Konsol çıktısı tarafından oluşturulan URL hakkında daha fazla bilgi için tarama `up` komutu. Şu biçimde olacaktır:

    ```bash
    (pending registration) Service 'webfrontend' port 'http' will be available at '<url>'
    Service 'webfrontend' port 80 (TCP) is available at 'http://localhost:<port>'
    ```
    Bu URL'yi bir tarayıcı penceresi açın ve web uygulamasını görmeniz gerekir. Kapsayıcı yürütülürken, terminal penceresine `stdout` ve `stderr` çıkışının akışı yapılır.

8. Ardından, ayarlama ve dağıtma *mywebapi*:

    1. Dizini `dev-spaces/samples/java/getting-started/mywebapi`

    2. Çalıştırın

        ```bash
        azds prep
        ```

    3. Çalıştırın

        ```bash
        azds up -d
        ```

## <a name="prepare-jenkins-server"></a>Jenkins sunucusunu hazırlama

Bu bölümde, örnek CI işlem hattı çalıştırması için Jenkins sunucusunu hazırlayın.

* Eklentileri yükle
* Helm ve Kubernetes CLI'yi yükleme
* Kimlik bilgileri ekleme

### <a name="install-plugins"></a>Eklentileri yükle

1. Jenkins sunucunuzun oturum açın. Seçin **Jenkins Yönet > eklentileri yönetme**.
2. Üzerinde **kullanılabilir** sekmesinde, aşağıdaki eklentileri seçin:
    * [Azure geliştirme alanları](https://plugins.jenkins.io/azure-dev-spaces)
    * [Azure kapsayıcı kayıt defteri görevleri](https://plugins.jenkins.io/azure-container-registry-tasks)
    * [Ortam Enjektörü](https://plugins.jenkins.io/envinject)
    * [GitHub tümleştirmesi](https://plugins.jenkins.io/github-pullrequest)

    Bu eklentiler listesinde görünmezse, kontrol **yüklü** bunların yüklenmiş olup olmadığını görmek için sekmesinde.

3. Eklenti yüklemek için seçin **hemen indirin ve yeniden başlatma işleminden sonra yükleme**.

4. Yüklemeyi tamamlamak için Jenkins sunucunuzu yeniden başlatın.

### <a name="install-helm-and-kubectl"></a>Helm ve kubectl yükleyin

Örnek işlem hattı geliştirme alanına dağıtmak için Helm ve kubectl kullanır. Jenkins yüklendiğinde adlı bir yönetici hesabı oluşturur. *jenkins*. Helm hem kubectl jenkins kullanıcı tarafından erişilebilir olması gerekiyor.

1. Jenkins ana bir SSH bağlantısı. 

2. Geçiş `jenkins` kullanıcı:
    ```bash
    sudo su jenkins
    ```

3. Helm CLI yükleyin. Daha fazla bilgi için [yükleme Helm](https://helm.sh/docs/using_helm/#installing-helm).

4. Kubectl yükleyin. Daha fazla bilgi için [ **az acs kubernetes yükleme-cli**](https://helm.sh/docs/using_helm/#installing-helm).

### <a name="add-credentials-to-jenkins"></a>Jenkins için kimlik bilgilerini ekleyin

1. Jenkins, kimlik doğrulaması ve Azure kaynaklarına erişmek için bir Azure hizmet sorumlusu gerekir. Hizmet sorumlusu oluşturmak için başvurmak [hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/jenkins/tutorial-jenkins-deploy-web-app-azure-app-service#create-service-principal) Azure App Service Öğreticisi dağıtma bölümünde. Çıkışı bir kopyasını kaydetmek mutlaka `create-for-rbac` sonraki adımı tamamlamak için bu bilgilere ihtiyacınız olduğundan. Çıktı aşağıdakine benzer görünecektir:

    ```json
    {
      "appId": "f4150da1-xxx-xxxx-xxx-xxxxxxxxxxxx",
      "displayName": "xxxxxxxjenkinssp",
      "name": "http://xxxxxxxjenkinssp",
      "password": "f6e4d839-xxx-xxxx-xxx-xxxxxxxxxxxx",
      "tenant": "72f988bf--xxx-xxxx-xxx-xxxxxxxxxxxx"
    }
    ```

2. Ekleme bir *Microsoft Azure hizmet sorumlusu* kimlik bilgisi, önceki adımdan gelen hizmet sorumlusu bilgileri kullanarak, Jenkins yazın. Aşağıdaki ekran görüntüsünde gösterilen adları çıktısı karşılık gelen `create-for-rbac`.

    **Kimliği** Jenkins kimlik bilgisi adı, hizmet sorumlusu için bir alandır. Değerini örnekte `displayName` (Bu örnekte, `xxxxxxxjenkinssp`), ancak istediğiniz metni kullanabilirsiniz. Bu kimlik bilgisi adı, sonraki bölümde AZURE_CRED_ID ortam değişkeninin değerdir.

    ![Jenkins ile hizmet sorumlusu kimlik bilgileri ekleme](media/tutorial-jenkins-dev-spaces/add-service-principal-credentials.png)

    **Açıklama** isteğe bağlıdır. Daha ayrıntılı yönergeler için bkz. [Jenkins Ekle hizmet sorumlusuna](https://docs.microsoft.com/azure/jenkins/tutorial-jenkins-deploy-web-app-azure-app-service#add-service-principal-to-jenkins) Azure App Service Öğreticisi dağıtma bölümünde. 



3. ACR kimlik bilgilerinizi görüntülemek için şu komutu çalıştırın:

    ```bash
    az acr credential show -n <yourRegistryName>
    ```

    JSON çıkışında, hangi aşağıdakine benzer görünmelidir bir kopyasını alın:

    ```json
    {
      "passwords": [
        {
          "name": "password",
          "value": "vGBP=zzzzzzzzzzzzzzzzzzzzzzzzzzz"
        },
        {
          "name": "password2",
          "value": "zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz"
        }
      ],
      "username": "acr01"
    }
    ```

4. Ekleme bir *kullanıcı adıyla parola* kimlik bilgisi, Jenkins yazın. **Kullanıcıadı** kullanıcı adı Bu örnekte, son adımda `acr01`. **Parola** değeri bu örnekte ilk parolayı `vGBP=zzzzzzzzzzzzzzzzzzzzzzzzzzz`. **Kimliği** bu kimlik bilgisi ACR_CRED_ID değeridir.

5. Bir AKS kimlik bilgisi ayarlayın. Ekleme bir *Kubernetes yapılandırma (kubeconfig'i denetleyin)* kimlik bilgisi türü jenkins'te ("Doğrudan Enter" seçeneğini kullanın). AKS kümenizi için erişim kimlik bilgilerini almak için aşağıdaki komutu çalıştırın:

    ```cmd
    az aks get-credentials -g MyResourceGroup -n <yourAKSName> -f -
    ```

   **Kimliği** bu kimlik bilgisi KUBE_CONFIG_ID sonraki bölümde değeridir.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

Örnek işlem hattı bir gerçek desenini esas için seçilen senaryo: Bir CI tetikleyicileri, işlem hattı bir çekme isteği oluşturur ve sonra bir Azure geliştirme alanına test etme ve gözden geçirme için önerilen değişikliklerin dağıtır. Gözden geçirme sonucunu bağlı olarak, değişiklikleri birleştirilir ve AKS için dağıtılan veya atıldı. Son olarak, geliştirme boşluk kaldırılır.

Jenkins ardışık düzen yapılandırması ve Jenkinsfile aşamaları CI işlem hattı tanımlayın. Bu akış, ardışık düzen aşamaları ve Jenkinsfile tarafından tanımlanan karar noktaları gösterilmektedir:

![Jenkins işlem hattı akış](media/tutorial-jenkins-dev-spaces/jenkins-pipeline-flow.png)

1. Değiştirilmiş bir sürümünü indirin *mywebapi* gelen proje https://github.com/azure-devops/mywebapi. Bu proje içeren bir işlem hattı oluşturmak için gereken çeşitli dosyaları dahil olmak üzere *Jenkinsfile*, *dockerfile'ları*ve Helm grafiği.

2. Jenkins oturum açın. Sol taraftaki menüden **Öğe Ekle**.

3. Seçin **işlem hattı**ve ardından bir ad girin **bir öğe adı girin** kutusu. Seçin **Tamam**, ardışık düzen yapılandırma ekranına daha sonra otomatik olarak açılır.

4. Üzerinde **genel** sekmesinde, onay **çalıştırma için ortam hazırlama**. 

5. Denetleme **tut ortam değişkenlerini Jenkins** ve **Jenkins derleme değişkenleri tutmak**.

6. İçinde **özellikleri içerik** kutusunda, aşağıdaki ortam değişkenlerini girin:

    ```
    AZURE_CRED_ID=[your credential ID of service principal]
    RES_GROUP=[your resource group of the function app]
    ACR_RES_GROUP=[your ACR resource group]
    ACR_NAME=[your ACR name]
    ACR_REGISTRY=[your ACR registry url, without http schema]
    ACR_CRED_ID=[your credential id of your ACR account]
    AKS_RES_GROUP=[your AKS resource group]
    AKS_NAME=[your AKS name]
    IMAGE_NAME=[name of Docker image you will push to ACR, without registry prefix]
    KUBE_CONFIG_ID=[your kubeconfig id]
    PARENT_DEV_SPACE=[shared dev space name]
    TEST_ENDPOINT=[your web frontend end point for testing. Should be webfrontend.XXXXXXXXXXXXXXXXXXX.xxxxxx.aksapp.io]
    ```

    Önceki bölümde verilen örnek değerleri kullanarak, ortam değişkenleri listesi aşağıdaki gibi görünmelidir:

    ![Jenkins işlem hattı ortam değişkenleri](media/tutorial-jenkins-dev-spaces/jenkins-pipeline-environment.png)

7. Seçin **işlem hattı SCM betikten** içinde **işlem hattı > tanımı**.
8. İçinde **SCM**, seçin **Git** depo URL'nizi girin.
9. İçinde **dal tanımlayıcısı**, girin `refs/remotes/origin/${GITHUB_PR_SOURCE_BRANCH}`.
10. SCM depo URL'si ve betik yolu "Jenkinsfile" doldurun.
11. **Basit bir kullanıma alma** denetlenmelidir.

## <a name="create-a-pull-request-to-trigger-the-pipeline"></a>İşlem hattını tetikleyen bir çekme isteği oluşturun

Tam adım 3'te bu bölümde, Jenkinsfile parçası açıklama gerekir, aksi takdirde, yeni ve eski sürümleri yan yana görüntülemeye çalıştığınızda bir 404 hatası alırsınız. Varsayılan olarak, çekme isteği birleştirme seçtiğinizde mywebapi paylaşılan önceki sürümünden kaldırılacak ve yeni bir sürümüyle değiştirmenin gerekmesidir. Aşağıdaki değişikliği Jenkinsfile için 1. adım tamamlamadan önce yapın:

```Groovy
    if (userInput == true) {
        stage('deploy') {
            // Apply the deployment to shared namespace in AKS using acsDeploy, Helm or kubectl   
        }
        
        stage('Verify') {
            // verify the staging environment is working properly
        }
        
        /* Comment the cleanup stage to allow side by side comparison with the new child dev space

        stage('cleanup') {
            devSpacesCleanup aksName: env.AKS_NAME, 
                azureCredentialsId: env.AZURE_CRED_ID, 
                devSpaceName: devSpaceNamespace, 
                kubeConfigId: env.KUBE_CONFIG_ID, 
                resourceGroupName: env.AKS_RES_GROUP,
                helmReleaseName: releaseName
        }
        */

    } else {
        // Send a notification
    }
```

1. Bir değişiklik `mywebapi/src/main/java/com/ms/sample/mywebapi/Application.java`ve ardından bir çekme isteği oluşturun. Örneğin:

    ```java
    public String index() {
        return "Hello from mywebapi in my private dev space";
    }
    ```

2. Jenkins ile oturum açın ve işlem hattı adı seçin ve ardından **artık yapı**. 

    Ayrıca ayarlayabileceğiniz bir *Web kancası* otomatik olarak Jenkins işlem hattını tetikleyen. Bir çekme isteği girildiğinde GitHub işlem hattını tetiklemesini bir GÖNDERİ Jenkins için yayınlar. Bir Web kancası ayarlama hakkında daha fazla bilgi için bkz. [github'a bağlanma Jenkins](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/jenkins/tutorial-jenkins-deploy-web-app-azure-app-service.md#connect-jenkins-to-github).

3. Geçerli paylaşılan sürümünde değişiklik karşılaştırın:

    1. Tarayıcınızı açın ve paylaştırılmış sürümüne gidin `https://webfrontend.XXXXXXXXXXXXXXXXXXX.eastus.aksapp.io`. TEST_ENDPOINT URL içerir.

    2. Başka bir sekmesini açın ve ardından çekme isteği geliştirme alanı URL'sini girin. Benzer şekilde `https://<yourdevspacename>.s.webfrontend.XXXXXXXXXXXXXXXXXXX.eastus.aksapp.io`. Bağlantıyı bulabilirsiniz **derleme geçmişi >< derleme # >> konsol çıktısı** Jenkins işi için. Sayfa için arama `aksapp`, ya da yalnızca ön eki görmek için arama `azdsprefix`.

 

### <a name="constructing-the-url-to-the-child-dev-space"></a>Alt geliştirme alanı URL'sine oluşturma

Bir çekme isteğinde dosya, Jenkins takımın paylaşılan geliştirme alanı esas alt geliştirme alanı oluşturur ve kod çekme isteğiniz o alt geliştirme alanı çalışır. Alt geliştirme alanı URL'sini alır `http://$env.azdsprefix.<test_endpoint>`. 

**$env.azdsprefix** işlem hattı yürütme sırasında Azure geliştirme alanları eklenti tarafından belirlenir **devSpacesCreate**:

```Groovy
stage('create dev space') {
    devSpacesCreate aksName: env.AKS_NAME,
        azureCredentialsId: env.AZURE_CRED_ID,
        kubeconfigId: env.KUBE_CONFIG_ID,
        resourceGroupName: env.AKS_RES_GROUP,
        sharedSpaceName: env.PARENT_DEV_SPACE,
        spaceName: devSpaceNamespace
}
```

`test_endpoint` URL daha önce dağıttığınız kullanarak webfrontend uygulamaya `azds up`içinde [örnek uygulamaları 7. adım, AKS kümesi dağıtma](#test_endpoint). Değerini `$env.TEST_ENDPOINT` işlem hattı yapılandırmasında ayarlayın. 

Aşağıdaki kod parçacığı alt geliştirme alanı URL nasıl kullanıldığını gösteren `smoketest` aşaması. Kod alt geliştirme alanı TEST_ENDPOINT kullanılabilir ve bu durumda, stdout Karşılama metin indirir olup olmadığını denetler:

```Groovy
stage('smoketest') {
    // CI testing against http://$env.azdsprefix.$env.TEST_ENDPOINT" 
    SLEEP_TIME = 30
    SITE_UP = false
    for (int i = 0; i < 10; i++) {
        sh "sleep ${SLEEP_TIME}"
        code = "0"
        try {
            code = sh returnStdout: true, script: "curl -sL -w '%{http_code}' 'http://$env.azdsprefix.$env.TEST_ENDPOINT/greeting' -o /dev/null"
        } catch (Exception e){
            // ignore
        }
        if (code == "200") {
            sh "curl http://$env.azdsprefix.$env.TEST_ENDPOINT/greeting"
            SITE_UP = true
            break
        }
    }
    if(!SITE_UP) {
        echo "The site has not been up after five minutes"
    }
}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bitirdiğinizde örnek uygulaması kullanarak, kaynak grubunu silerek Azure kaynakları temizleme:

```bash
az group delete -y --no-wait -n MyResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Jenkins için Azure geliştirme alanları eklentisi ve Azure Container Registry eklenti Kodu derlemek ve dağıtmak için bir geliştirme alanı için nasıl kullanılacağını öğrendiniz.

Aşağıdaki listede yer alan kaynakları, Azure geliştirme alanları, ACR görevleri ve Jenkins ile CI/CD daha fazla bilgi sağlar.

Azure geliştirme alanları:
* [Azure Dev Spaces çalışma ve yapılandırma süreçleri](https://docs.microsoft.com/azure/dev-spaces/how-dev-spaces-works)

ACR görevler:
* [ACR Görevleri ile işletim sistemi ve çerçeve düzeltme eki uygulamayı otomatikleştirme](https://docs.microsoft.com/azure/container-registry/container-registry-tasks-overview)
* [Otomatik derleme üzerinde kod yürütme](https://docs.microsoft.com/azure/container-registry/container-registry-tasks-overview)

Azure üzerinde Jenkins ile CI/CD:
* [Jenkins sürekli dağıtım](https://docs.microsoft.com/azure/aks/jenkins-continuous-deployment)
