---
title: (KULLANIM DIŞI) Azure Container Service altyapısı ve Swarm modu ile CI/CD
description: Azure Container Service altyapısı sürekli .NET Core çok kapsayıcılı bir uygulama sunmak için Docker Swarm modu, bir Azure Container Registry ve Azure DevOps kullanın
services: container-service
author: diegomrtnzg
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/27/2017
ms.author: diegomrtnzg
ms.custom: mvc
ms.openlocfilehash: 8aa62e4ed65f8223071786ac165f8343cb6901d5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60430668"
---
# <a name="deprecated-full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-acs-engine-and-docker-swarm-mode-using-azure-devops"></a>(KULLANIM DIŞI) ACS altyapısı ve Azure DevOps kullanarak Docker Swarm modu ile Azure Container Service üzerinde çok kapsayıcılı bir uygulama dağıtmak için tam CI/CD işlem hattı

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

*Bu makalede dayanır [tam CI/CD işlem hattı, Azure DevOps kullanarak Docker Swarm ile Azure Container Service üzerinde çok kapsayıcılı bir uygulama dağıtmak için](container-service-docker-swarm-setup-ci-cd.md) belgeleri*

Günümüzde, modern bulut uygulamaları geliştirirken en büyük zorluklardan biri bu uygulamaları sürekli olarak sunmak çağrılabilmesidir. Bu makalede, bir tam bir sürekli tümleştirme ve dağıtım (CI/CD) işlem hattı kullanarak uygulama öğreneceksiniz: 
* Docker Swarm modu ile Azure Container Service altyapısı
* Azure Container Registry
* Azure DevOps

Bu makale, bir basit uygulama, kullanılabilir temel [GitHub](https://github.com/jcorioland/MyShop/tree/docker-linux), ASP.NET Core ile geliştirilen. Uygulama dört farklı hizmetlerinden oluşur: üç web API'leri ve bir web ön ucu:

![MyShop örnek uygulaması](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/myshop-application.png)

Amacı, bu uygulama, Azure DevOps kullanarak Docker Swarm modu kümesi sürekli olarak kullanıma sunun sağlamaktır. Aşağıdaki şekil bu sürekli teslim işlem hattı ayrıntıları:

![MyShop örnek uygulaması](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/full-ci-cd-pipeline.png)

Kısa bir açıklama adımları şu şekildedir:

1. Kod değişiklikleri için kaynak kodu deposu taahhüt (burada, GitHub) 
2. Azure DevOps yapıda GitHub tetikler 
3. Azure DevOps, kaynakları en son sürümünü alır ve uygulama oluşturan tüm görüntüleri oluşturur 
4. Azure DevOps her görüntü Azure Container Registry hizmeti kullanılarak oluşturulan bir Docker kayıt defterine gönderir. 
5. Azure DevOps yeni bir yayın tetiklenir. 
6. Azure kapsayıcı hizmeti küme ana düğümüne SSH kullanarak bazı komutlar yayın çalıştırır 
7. Docker Swarm modu kümesi üzerinde görüntüleri en son sürümünü çeker 
8. Uygulamanın yeni sürümü Docker yığını kullanılarak dağıtılır 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce aşağıdaki görevleri tamamlamanız gerekir:

- [ACS altyapısı ile Azure Container Service'te Swarm modu kümesi oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acsengine-swarmmode)
- [Azure Container Service'teki Swarm kümesine bağlanma](../container-service-connect.md)
- [Azure container registry oluşturma](../../container-registry/container-registry-get-started-portal.md)
- [Bir Azure DevOps kuruluş ve oluşturulan proje sahip](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student)
- [GitHub hesabınıza GitHub depo çatalı oluşturma](https://github.com/jcorioland/MyShop/tree/docker-linux)

>[!NOTE]
> Azure Container Service’teki Docker Swarm düzenleyicisi eski tek başına Swarm’u kullanır. Şu anda, tümleşik [Swarm modu](https://docs.docker.com/engine/swarm/) (Docker 1.12 ve daha sonraki sürümleri) Azure Container Service'te desteklenen bir düzenleyici değildir. Bu nedenle, kullanıyoruz [ACS altyapısı](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), topluluk katkısıyla [Hızlı Başlangıç şablonu](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), ya da Docker çözümde [Azure Marketi](https://azuremarketplace.microsoft.com).
>

## <a name="step-1-configure-your-azure-devops-organization"></a>1. Adım: Azure DevOps kuruluşunuz yapılandırın 

Bu bölümde, Azure DevOps kuruluşunuz yapılandırın. Azure DevOps Hizmetleri uç noktaları, Azure DevOps projenizi yapılandırmak için tıklayın **ayarları** simgesini seçin ve araç **Hizmetleri**.

![Açık hizmet uç noktası](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/services-vsts.PNG)

### <a name="connect-azure-devops-and-azure-account"></a>Azure DevOps ve Azure hesabı bağlama

Azure DevOps projenizi ve Azure hesabınız arasında bir bağlantı ayarlayın.

1. Sol tarafta, tıklayın **yeni hizmet uç noktası** > **Azure Resource Manager**.
2. Azure DevOps, Azure hesabınızla çalışmak için yetkilendirmek için seçin, **abonelik** tıklatıp **Tamam**.

    ![Azure DevOps - Azure Yetkilendir](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-azure.PNG)

### <a name="connect-azure-devops-and-github"></a>Azure DevOps ve GitHub'ı bağlama

Azure DevOps projenizi ve GitHub hesabınızı arasında bir bağlantı ayarlayın.

1. Sol tarafta, tıklayın **yeni hizmet uç noktası** > **GitHub**.
2. Azure DevOps, GitHub hesabınızı ile çalışmak için yetkilendirmek için tıklatın **Authorize** ve açılan pencerede verilen yordamı izleyin.

    ![Azure DevOps - GitHub Yetkilendir](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github.png)

### <a name="connect-azure-devops-to-your-azure-container-service-cluster"></a>Azure DevOps, Azure Container Service kümesine bağlanma

Azure'da Docker Swarm kümenizi dış bağlantıları yapılandırmak için CI/CD ardışık alma önce son adımlar yer almaktadır. 

1. Uç nokta türü için Docker Swarm kümesi ekleme **SSH**. Ardından, Swarm kümesine (ana düğüm) SSH bağlantı bilgilerini girin.

    ![Azure DevOps - SSH](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-ssh.png)

Tüm yapılandırma artık gerçekleştirilir. Sonraki adımlarda derler ve uygulamayı Docker Swarm kümesi dağıtır CI/CD işlem hattı oluşturun. 

## <a name="step-2-create-the-build-pipeline"></a>2. Adım: Derleme işlem hattı oluşturma

Bu adımda, Azure DevOps projesi için bir derleme işlem hattı ayarlayın ve yapı iş akışı için kapsayıcı görüntülerinizi tanımlayın

### <a name="initial-pipeline-setup"></a>İlk komut zinciri Kurulumu

1. Bir derleme işlem hattı oluşturmak için Azure DevOps projenizi bağlama ve **derleme ve yayınlama**. İçinde **yapı tanımları** bölümünde **+ yeni**. 

    ![Azure DevOps - yeni işlem hattı oluşturma](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-build-vsts.PNG)

2. Seçin **boş işlem**.

    ![Azure DevOps - yeni ve boş işlem hattı oluşturma](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-empty-build-vsts.PNG)

4. ' A tıklayarak **değişkenleri** sekmesini ve iki yeni değişkeni oluşturun: **RegistryURL** ve **AgentURL**. Küme aracıları DNS ve kayıt defteri değerlerini yapıştırın.

    ![Azure DevOps - derleme değişkenleri yapılandırması](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-variables.png)

5. Üzerinde **derleme tanımları** sayfasını açık **Tetikleyicileri** sekme ve önkoşullarda oluşturduğunuz MyShop projesinin çatalı ile sürekli tümleştirme kullanılacak derlemeyi yapılandırın. Ardından, **toplu değişiklikler**. Seçtiğinizden emin olun *docker-linux* olarak **dal belirtimi**.

    ![Azure DevOps - derleme deposu yapılandırması](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github-repo-conf.PNG)


6. Son olarak, tıklayın **seçenekleri** sekmesini ve yapılandırmak için varsayılan aracı kuyruğu **barındırılan Linux Önizleme**.

    ![Azure DevOps - konak aracı yapılandırması](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-agent.png)

### <a name="define-the-build-workflow"></a>Yapı iş akışı tanımlama
Sonraki adımlar, yapı iş akışı tanımlayın. İlk olarak, kaynak kodun yapılandırmanız gerekir. Yapmak için **GitHub** ve **depo** ve **dal** (docker-linux).

![Azure DevOps - yapılandırma kod kaynağı](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-source-code.png)

Oluşturmak için beş kapsayıcı görüntülerini vardır *MyShop* uygulama. Her bir görüntü kullanarak proje klasörleri'nde bulunan Dockerfile oluşturulmuştur:

* ProductsApi
* Ara sunucu
* RatingsApi
* RecommendationsApi
* ShopFront

Docker aşamanın her görüntü, bir görüntü oluşturun ve bir Azure kapsayıcı kayıt defterini kullanarak görüntüyü gönderin ihtiyacınız vardır. 

1. Derleme iş akışında bir adım eklemek için tıklatın **+ derleme adımı Ekle** seçip **Docker**.

    ![Azure DevOps - derleme adımları Ekle](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-add-task.png)

2. Her bir görüntü için kullandığı bir adım yapılandırma `docker build` komutu.

    ![Azure DevOps - Docker derleme](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-build.png)

    Oluşturma işlemi için Azure Container Registry seçin **bir görüntü oluşturun** eylem ve her görüntü tanımlar Dockerfile. Ayarlama **çalışma dizini** Dockerfile kök dizini olarak tanımlamak **görüntü adı**seçip **dahil en son etiket**.
    
    Görüntü adı şu biçimde olması gerekir: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```. Değiştirin **[NAME]** görüntü adı ile:
    - ```proxy```
    - ```products-api```
    - ```ratings-api```
    - ```recommendations-api```
    - ```shopfront```

3. Her bir görüntü için kullandığı ikinci bir adım yapılandırma `docker push` komutu.

    ![Azure DevOps - Docker itme](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-push.png)

    Gönderme işlemi için Azure kapsayıcı kayıt defteri seçin **görüntü gönderebilmeniz** eylem girin **görüntü adı** seçin ve önceki adımda oluşturulan **dahil en son etiket**.

4. Derleme ve gönderme adımları her beş görüntüleri için yapılandırdıktan sonra derleme iş akışında üç adım daha ekleyin.

   ![Azure DevOps - komut satırı görev ekleyin](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-command-task.png)

   1. Değiştirmek için bir bash komut dosyası kullanan bir komut satırı görevi *RegistryURL* RegistryURL değişkeni ile docker-compose.yml dosyasında, oluşumunu. 
    
       ```-c "sed -i 's/RegistryUrl/$(RegistryURL)/g' src/docker-compose-v3.yml"```

       ![Azure DevOps - güncelleştirme Compose dosyasının kayıt defteri URL'si](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-replace-registry.png)

   2. Değiştirmek için bir bash komut dosyası kullanan bir komut satırı görevi *AgentURL* AgentURL değişkeni ile docker-compose.yml dosyasında, oluşumunu.
  
       ```-c "sed -i 's/AgentUrl/$(AgentURL)/g' src/docker-compose-v3.yml"```

      1. Bu sürümde kullanılabilir, böylece güncelleştirilmiş Compose dosyası bir derleme yapıtı bıraktığı bir görev. Ayrıntılar için aşağıdaki ekranı görürsünüz.

      ![Azure DevOps - Yapıt yayımlama](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish.png) 

      ![Azure DevOps - yayımlama Compose dosyası](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish-compose.png) 

5. Tıklayın **Kaydet ve kuyruğa** derleme işlem hattınızı test etmek için.

   ![Azure DevOps - Kaydet ve kuyruğa](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-save.png) 

   ![Azure DevOps - yeni bir kuyruk](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-queue.png) 

6. Varsa **derleme** doğru bu ekranı görmeniz gerekir:

   ![Azure DevOps - derleme başarılı oldu](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-succeeded.png) 

## <a name="step-3-create-the-release-pipeline"></a>3. Adım: Yayın işlem hattı oluşturma

Azure DevOps sayesinde [ortamlar genelinde sürümleri yönetmek](https://www.visualstudio.com/team-services/release-management/). Uygulamanızın düzgün bir şekilde (örneğin, geliştirme, test, üretim öncesi ve üretim gibi) farklı ortamlarınızda şekilde dağıtıldığından emin olmak sürekli dağıtımı etkinleştirebilirsiniz. Azure Container Service Docker Swarm modu kümesi temsil eden bir ortam oluşturabilirsiniz.

![Azure DevOps - ACS sürümüne](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>İlk yayın Kurulumu

1. Yayın işlem hattı oluşturmak için tıklayın **yayınlar** > **+ yayın**

2. Yapıt kaynağı yapılandırmak için tıklayın **Yapıtları** > **yapıt kaynağı Bağla**. Burada, bu yeni yayın ardışık düzeni önceki adımda tanımlanan yapı bağlayın. Bundan sonra docker-compose.yml dosyası sürüm sürecinizde kullanılabilir.

    ![Azure DevOps - sürüm yapıları](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-artefacts.png) 

3. Yayın tetikleyicisi yapılandırmak için tıklayın **Tetikleyicileri** seçip **sürekli dağıtım**. Aynı yapıt kaynağında tetikleyici ayarlayın. Bu ayar, yeni bir yayın oluşturma işlemi başarıyla tamamlandığında başlar sağlar.

    ![Azure DevOps - yayın Tetikleyicileri](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-trigger.png) 

4. Yayın değişkenleri yapılandırmak için tıklayın **değişkenleri** seçip **+ değişken** üç yeni kayıt defteri bilgileri değişkenlerinin: **docker.username**, **docker.password**, ve **docker.registry**. Küme aracıları DNS ve kayıt defteri değerlerini yapıştırın.

    ![Azure DevOps - derleme deposu yapılandırması](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-variables.png)

    >[!IMPORTANT]
    > Önceki ekranda gösterildiği gibi tıklayın **kilit** docker.password onay kutusunu işaretleyin. Bu ayar, parolanın kısıtlamak önemlidir.
    >

### <a name="define-the-release-workflow"></a>Yayın iş akışı tanımlama

Yayın iş akışı, eklediğiniz iki görevlerini oluşur.

1. Compose dosyası için güvenli bir şekilde kopyalamak için bir görevi yapılandırmaya bir *dağıtma* daha önce yapılandırdığınız SSH bağlantısını kullanarak Docker Swarm ana düğüme klasör. Ayrıntılar için aşağıdaki ekranı görürsünüz.
    
    Kaynak klasör: ```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```

    ![Azure DevOps - sürüm SCP](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-scp.png)

2. Çalıştırılacak bir bash komut yürütmek için ikinci bir görevi yapılandırmaya `docker` ve `docker stack deploy` ana düğüm komutları. Ayrıntılar için aşağıdaki ekranı görürsünüz.

    ```
    docker login -u $(docker.username) -p $(docker.password) $(docker.registry) && export DOCKER_HOST=:2375 && cd deploy && docker stack deploy --compose-file docker-compose-v3.yml myshop --with-registry-auth
    ```

    ![Azure DevOps - sürüm Bash](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-bash.png)

    Ana yürütülen komut, aşağıdaki görevleri gerçekleştirmek için Docker CLI ve Docker-Compose CLI'yı kullanır:

   - Azure container registry'ye oturum açın (tanımlanan üç yapı değişkenleri kullanır **değişkenleri** sekmesinde)
   - Tanımlama **DOCKER_HOST** Swarm uç nokta ile çalışmaya değişkeni (: 2375)
   - Gidin *dağıtma* önceki güvenli kopyalama görevi tarafından oluşturulan ve docker-compose.yml dosyasını içeren klasör 
   - Yürütme `docker stack deploy` yeni görüntüleri çekmek ve kapsayıcı oluşturma komutları.

     >[!IMPORTANT]
     > Önceki ekranda göründüğü gibi bırakın **STDERR üzerinde başarısız** onay kutusunu işaretlemeden. Bu ayarı nedeniyle yayın işlemini tamamlamak sağlıyor `docker-compose` gibi durdurma veya standart hata çıktı silinmesini kapsayıcılardır birkaç tanılama iletilerini yazdırır. Onay kutusunu işaretleyin, tüm aşsa bile iyi Azure DevOps yayın sırasında hataları oluştuğunu bildirir.
     >
3. Bu yeni yayın ardışık düzeni kaydedin.

## <a name="step-4-test-the-cicd-pipeline"></a>4. Adım: CI/CD işlem hattı test

Yapılandırmasını tamamladıktan sonra bunu bu yeni CI/CD işlem hattı test etme vakti. Test etmek için en kolay yolu, kaynak kodunu güncelleştirin ve değişiklikleri GitHub deponuza sağlamaktır. Kod gönderdikten sonra birkaç saniye içinde Azure DevOps çalıştıran yeni bir derleme görürsünüz. Başarıyla tamamlandığında, yeni bir yayın tetiklenir ve Azure Container Service kümesi uygulamasının yeni sürümü dağıtılır.

## <a name="next-steps"></a>Sonraki adımlar

* Azure DevOps ile CI/CD hakkında daha fazla bilgi için bkz: [genel DevOps Azure yapı](https://www.visualstudio.com/docs/build/overview).
* ACS altyapısı hakkında daha fazla bilgi için bkz: [ACS altyapısı GitHub deposunu](https://github.com/Azure/acs-engine).
* Docker Swarm modu hakkında daha fazla bilgi için bkz. [Docker Swarm modu genel bakış](https://docs.docker.com/engine/swarm/).
