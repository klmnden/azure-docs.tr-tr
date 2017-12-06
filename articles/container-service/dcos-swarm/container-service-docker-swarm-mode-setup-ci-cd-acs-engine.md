---
title: "CI/CD Azure kapsayıcı hizmeti altyapısı ve Swarm modu"
description: "Azure kapsayıcı Hizmeti altyapısının birden çok kapsayıcı .NET Core uygulama sürekli olarak göndermek için Docker Swarm modu, bir Azure kapsayıcı kayıt defteri ve Visual Studio Team Services ile kullanma"
services: container-service
author: diegomrtnzg
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 05/27/2017
ms.author: diegomrtnzg
ms.custom: mvc
ms.openlocfilehash: 6aa690ff7ec0689db78ff1225d36171adb30ee2c
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-acs-engine-and-docker-swarm-mode-using-visual-studio-team-services"></a>ACS altyapısı ve Docker Swarm Visual Studio Team Services kullanarak modu ile Azure kapsayıcı hizmeti üzerinde çok kapsayıcı uygulama dağıtmak için tam CI/CD ardışık düzen

*Bu makalede dayanır [Visual Studio Team Services kullanarak Docker Swarm ile Azure kapsayıcı hizmeti üzerinde çok kapsayıcı uygulama dağıtmak için tam CI/CD ardışık düzen](container-service-docker-swarm-setup-ci-cd.md) belgeleri*

Günümüzde, modern bulut uygulamaları geliştirirken en büyük zorluklardan biri bu uygulamalara sürekli olarak teslim etmek mümkün. Bu makalede, bir tam sürekli tümleştirme ve dağıtım (CI/CD) kullanılarak ardışık düzeni uygulamak öğrenin: 
* Azure kapsayıcı hizmeti altyapısı Docker Swarm modu
* Azure Container Kayıt Defteri
* Visual Studio Team Services

Bu makalede, kullanılabilen basit bir uygulama dayalı [GitHub](https://github.com/jcorioland/MyShop/tree/docker-linux), ASP.NET Core ile geliştirilen. Uygulama dört farklı hizmetlerinden oluşur: üç API'ları ve bir web ön uç web:

![MyShop örnek uygulama](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/myshop-application.png)

Amacı, bu uygulama, Visual Studio Team Services kullanarak bir Docker Swarm modu kümede sürekli olarak teslim etmek sağlamaktır. Aşağıdaki şekilde bu kesintisiz teslim ardışık düzen ayrıntıları:

![MyShop örnek uygulama](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/full-ci-cd-pipeline.png)

Kısa bir açıklama adımları şöyledir:

1. Kod değişiklikleri kaynak kodu depoya taahhüt (burada, GitHub) 
2. Visual Studio Team Services derlemede GitHub tetikler 
3. Visual Studio Team Services kaynakları en son sürümünü alır ve uygulamayı oluşturan tüm görüntü oluşturur 
4. Visual Studio Team Services her görüntü Azure kapsayıcı kayıt defteri hizmeti kullanılarak oluşturulan bir Docker kayıt defterine iter. 
5. Yeni bir sürüm Visual Studio Team Services tetikler 
6. Azure kapsayıcı hizmeti küme ana düğümünde SSH kullanarak bazı komutları yayın çalıştırır 
7. Docker Swarm modu küme üzerinde görüntüleri en son sürümünü çeker 
8. Uygulamanın yeni sürümü Docker yığını kullanılarak dağıtılır 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce aşağıdaki görevleri tamamlamanız gerekir:

- [ACS altyapı ile Azure kapsayıcı Hizmeti'nde bir Swarm modu kümesi oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acsengine-swarmmode)
- [Azure Container Service'teki Swarm kümesine bağlanma](../container-service-connect.md)
- [Azure kapsayıcı kayıt defteri oluşturma](../../container-registry/container-registry-get-started-portal.md)
- [Oluşturulan Visual Studio Team Services hesabı ve ekip projesinde olması](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [GitHub hesabınızda GitHub depoyu çatallaştırmanız](https://github.com/jcorioland/MyShop/tree/docker-linux)

>[!NOTE]
> Azure Container Service’teki Docker Swarm düzenleyicisi eski tek başına Swarm’u kullanır. Şu anda, tümleşik [Swarm modu](https://docs.docker.com/engine/swarm/) (Docker 1.12 ve daha sonraki sürümleri) Azure Container Service'te desteklenen bir düzenleyici değildir. Bu nedenle, kullanıyoruz [ACS altyapısı](https://github.com/Azure/acs-engine/blob/master/docs/swarmmode.md), topluluk katkıda [hızlı başlatma şablonunu](https://azure.microsoft.com/resources/templates/101-acsengine-swarmmode/), veya bir Docker çözümde [Azure Marketi](https://azuremarketplace.microsoft.com).
>

## <a name="step-1-configure-your-visual-studio-team-services-account"></a>1. adım: Visual Studio Team Services hesabınızın yapılandırma 

Bu bölümde, Visual Studio Team Services hesabınızın yapılandırın. VSTS Hizmetleri uç noktaları, Visual Studio Team Services projenizde yapılandırmak için tıklatın **ayarları** simgesini seçin ve araç **Hizmetleri**.

![Açık hizmet uç noktası](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/services-vsts.PNG)

### <a name="connect-visual-studio-team-services-and-azure-account"></a>Visual Studio Team Services ve Azure hesabına bağlanma

VSTS projeniz ve Azure hesabınızda arasında bir bağlantı ayarlayın.

1. Sol bölmede, tıklatın **yeni hizmet uç noktası** > **Azure Resource Manager**.
2. Azure hesabınızla çalışmaya VSTS yetki vermek için seçin, **abonelik** tıklatıp **Tamam**.

    ![Visual Studio Team Services - Azure yetkilendirmek](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-azure.PNG)

### <a name="connect-visual-studio-team-services-and-github"></a>Visual Studio Team Services ve GitHub Bağlan

VSTS projeniz, GitHub hesabınızda arasında bir bağlantı ayarlayın.

1. Sol bölmede, tıklatın **yeni hizmet uç noktası** > **GitHub**.
2. GitHub hesabınızla çalışmaya VSTS yetkilendirmek için tıklatın **Authorize** ve açılır pencere yordamı izleyin.

    ![Visual Studio Team Services - GitHub yetkilendirmek](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github.png)

### <a name="connect-vsts-to-your-azure-container-service-cluster"></a>VSTS, Azure kapsayıcı hizmeti kümesine bağlanma

CI/CD ardışık düzenine alma önce son Azure'da Docker Swarm kümesi dış bağlantıları yapılandırmak için adımlardır. 

1. Docker Swarm kümesi için bir uç nokta türü ekleme **SSH**. Ardından Swarm kümenizin (ana düğüm) SSH bağlantı bilgilerini girin.

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-ssh.png)

Tüm yapılandırma şimdi yapılır. Sonraki adımda oluşturan ve Docker Swarm kümesi uygulamaya dağıtır CI/CD ardışık düzen oluşturun. 

## <a name="step-2-create-the-build-definition"></a>2. adım: derleme tanımı oluşturma

Bu adımda, bir derleme tanımını VSTS projeniz için ayarlar ve yapı iş akışı kapsayıcı görüntülerinizi tanımlayın

### <a name="initial-definition-setup"></a>Başlangıç tanım Kurulumu

1. Yapı tanımı oluşturmak için Visual Studio Team Services projenize bağlanmak ve **yapı & yayın**. İçinde **yapı tanımları** 'yi tıklatın **+ yeni**. 

    ![Visual Studio Team Services - yeni yapı tanımı](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-build-vsts.PNG)

2. Seçin **boş işlem**.

    ![Visual Studio Team Services - yeni ve boş yapı tanımı](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/create-empty-build-vsts.PNG)

4. Ardından **değişkenleri** sekmesinde ve iki yeni değişken oluşturma: **RegistryURL** ve **AgentURL**. Kayıt defteri ve küme aracıları DNS değerlerini yapıştırın.

    ![Visual Studio Team Services - derleme değişkenleri yapılandırması](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-variables.png)

5. Üzerinde **yapı tanımlarını** sayfasında, açık **Tetikleyicileri** sekmesinde ve önkoşullar oluşturulan MyShop proje çatalı ile sürekli tümleştirme kullanmak için yapıyı yapılandırın. Ardından, seçin **toplu değişiklikleri**. Seçtiğinizden emin olun *docker linux* olarak **dal belirtimi**.

    ![Visual Studio Team Services - derleme deposu yapılandırma](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-github-repo-conf.PNG)


6. Son olarak, tıklatın **seçenekleri** sekmesinde ve varsayılan aracı kuyruğuna yapılandırma **barındırılan Linux Önizleme**.

    ![Visual Studio Team Services - konak aracı yapılandırması](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-agent.png)

### <a name="define-the-build-workflow"></a>Yapı iş akışı tanımlayın
Sonraki adımlar yapı iş akışı tanımlayın. İlk olarak, kaynak kodunun yapılandırmanız gerekir. Yapmak için seçin **GitHub** ve **deposu** ve **şube** (docker-linux).

![Visual Studio Team Services - yapılandırma kod kaynağı](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-source-code.png)

Beş kapsayıcı görüntülerini oluşturmak için *MyShop* uygulama. Her görüntü proje klasörleri'nde bulunan Dockerfile kullanılarak oluşturulur:

* ProductsApi
* Ara sunucu
* RatingsApi
* RecommandationsApi
* ShopFront

Her görüntü, görüntü oluşturmak için diğeri Azure kapsayıcı kayıt defterinde görüntü göndermek için iki Docker adımları gerekir. 

1. Yapı iş akışında bir adım eklemek için tıklatın **+ Ekle derleme adımı** seçip **Docker**.

    ![Visual Studio Team Services - eklemek derleme adımları](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-add-task.png)

2. Her görüntü için kullandığı bir adım yapılandırma `docker build` komutu.

    ![Visual Studio Team Services - Docker derleme](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-build.png)

    Derleme işlemi için Azure kapsayıcı kaydınız seçin **görüntü yapı** eylem ve her görüntü tanımlar Dockerfile. Ayarlama **çalışma dizini** Dockerfile kök dizini olarak tanımlamak **görüntü adı**seçip **dahil en son etiket**.
    
    Görüntü adı bu biçimde olması gerekir: ```$(RegistryURL)/[NAME]:$(Build.BuildId)```. Değiştir **[NAME]** görüntü adı ile:
    - ```proxy```
    - ```products-api```
    - ```ratings-api```
    - ```recommendations-api```
    - ```shopfront```

3. Her görüntü için kullanır ikinci bir adım yapılandırma `docker push` komutu.

    ![Visual Studio Team Services - Docker gönderme](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-docker-push.png)

    Gönderme işlemi için Azure kapsayıcı kayıt defteri seçin **görüntü anında** eylemi girin **görüntü adı** seçin ve önceki adımda içinde yerleşik **dahil en son etiket**.

4. Derleme ve anında iletme adımları her beş görüntüleri için yapılandırdıktan sonra yapı iş akışında üç adım ekleyin.

   ![Visual Studio Team Services - komut satırı görev ekleme](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-command-task.png)

      1. Değiştirmek için bir bash komut dosyası kullanan bir komut satırı görevi *RegistryURL* RegistryURL değişkeniyle docker-compose.yml dosyası geçişi. 
    
          ```-c "sed -i 's/RegistryUrl/$(RegistryURL)/g' src/docker-compose-v3.yml"```

          ![Visual Studio Team Services - kayıt defteri URL dosyasıyla güncelleştirme oluştur](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-replace-registry.png)

      2. Değiştirmek için bir bash komut dosyası kullanan bir komut satırı görevi *AgentURL* AgentURL değişkeniyle docker-compose.yml dosyası geçişi.
  
          ```-c "sed -i 's/AgentUrl/$(AgentURL)/g' src/docker-compose-v3.yml"```

     3. Bu sürümde kullanılabilir, böylece güncelleştirilmiş Oluştur dosya derleme yapısı olarak bırakır bir görev. Ayrıntılar için aşağıdaki ekran görüntüsüne bakın.

         ![Visual Studio Team Services - yayımlama yapı](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish.png) 

         ![Visual Studio Team Services - Oluştur yayımlama dosyası](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-publish-compose.png) 

5. Tıklatın **sıraya & Kaydet** yapı tanımınızı test etmek için.

   ![Visual Studio Team Services - Kaydet ve sırası](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-save.png) 

   ![Visual Studio Team Services - yeni kuyruk](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-queue.png) 

6. Varsa **yapı** doğru olan bu ekranı görmek:

  ![Visual Studio Team Services - oluşturma başarılı oldu](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-build-succeeded.png) 

## <a name="step-3-create-the-release-definition"></a>3. adım: sürüm tanımı oluşturma

Visual Studio Team Services olanak tanır [sürümleri arasında ortamlarını](https://www.visualstudio.com/team-services/release-management/). Kesintisiz bir şekilde uygulamanız farklı ortamınızı (örneğin, geliştirme, test, ön üretim ve üretim) üzerinde dağıtıldığından emin olmak sürekli dağıtım etkinleştirebilirsiniz. Azure kapsayıcı hizmeti Docker Swarm modu kümenizi temsil eden bir ortam oluşturabilirsiniz.

![Visual Studio Team Services - ACS sürüme](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>Kurulum ilk sürüm

1. Bir yayın tanımı oluşturmak için tıklatın **sürümleri** > **+ sürüm**

2. Yapı kaynağını yapılandırmak için tıklatın **yapıları** > **bir yapı kaynak bağlantı**. Burada, bu yeni sürüm tanımı önceki adımda tanımlanan yapı bağlayın. Bundan sonra docker-compose.yml dosyası yayın işlemde kullanılabilir.

    ![Visual Studio Team Services - sürüm yapıları](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-artefacts.png) 

3. Yayın tetikleyici yapılandırmak için tıklatın **Tetikleyicileri** seçip **sürekli dağıtım**. Tetikleyici aynı yapı kaynağında ayarlayın. Bu ayar, yapı başarıyla tamamlandığında, yeni bir sürüm başlayacağını sağlar.

    ![Visual Studio Team Services - sürüm Tetikleyicileri](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-trigger.png) 

4. Yayın değişkenleri yapılandırmak için tıklatın **değişkenleri** seçip **+ değişken** üç yeni kayıt defteri bilgiyle değişkenlerinin: **docker.username**, **docker.password**, ve **docker.registry**. Kayıt defteri ve küme aracıları DNS değerlerini yapıştırın.

    ![Visual Studio Team Services - derleme deposu yapılandırma](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-variables.png)

    >[!IMPORTANT]
    > Önceki ekranda gösterildiği gibi tıklayın **kilit** docker.password onay kutusu. Bu ayar parolanın kısıtlamak önemlidir.
    >

### <a name="define-the-release-workflow"></a>Yayın iş akışı tanımlama

Yayın iş akışı eklediğiniz iki görevlerin oluşur.

1. Güvenli bir şekilde Oluştur dosyasına kopyalamak için bir görevi yapılandırmaya bir *dağıtmak* daha önce yapılandırdığınız SSH bağlantısını kullanarak Docker Swarm ana düğümde, klasör. Ayrıntılar için aşağıdaki ekran görüntüsüne bakın.
    
    Kaynak klasörü:```$(System.DefaultWorkingDirectory)/MyShop-CI/drop```

    ![Visual Studio Team Services - sürüm SCP](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-scp.png)

2. Çalıştırılacak bash komutu yürütmek için ikinci bir görevi yapılandırmaya `docker` ve `docker stack deploy` ana düğümde komutları. Ayrıntılar için aşağıdaki ekran görüntüsüne bakın.

    ```docker login -u $(docker.username) -p $(docker.password) $(docker.registry) && export DOCKER_HOST=:2375 && cd deploy && docker stack deploy --compose-file docker-compose-v3.yml myshop --with-registry-auth```

    ![Visual Studio Team Services - sürüm Bash](./media/container-service-docker-swarm-mode-setup-ci-cd-acs-engine/vsts-release-bash.png)

    Asıl yürütülen komutu, aşağıdaki görevleri gerçekleştirmek için Docker CLI ve Docker Compose CLI kullanır:

    - Azure kapsayıcı kayıt defterine oturum açma (tanımlanan üç derleme değişkenleri kullanır **değişkenleri** sekmesinde)
    - Tanımlamak **DOCKER_HOST** Swarm uç noktalarına ile çalışmak için değişken (: 2375)
    - Gidin *dağıtmak* önceki güvenli kopyalama görevi tarafından oluşturulan ve docker-compose.yml dosyası içeren klasör 
    - Yürütme `docker stack deploy` yeni görüntüleri çekmek ve kapsayıcıları oluşturma komutları.

    >[!IMPORTANT]
    > Önceki ekranda gösterildiği gibi bırakın **STDERR üzerinde başarısız** onay kutusu işaretli. Bu ayarı nedeniyle yayın işlemini tamamlamak için bize sağlar `docker-compose` durdurma veya standart hata çıktı siliniyor kapsayıcılardır gibi birkaç tanılama iletilerini yazdırır. Onay kutusunu işaretlerseniz, tüm aşsa bile iyi Visual Studio Team Services hataları yayın sırasında oluştuğunu bildiriyor.
    >
3. Bu yeni sürüm tanımını kaydedin.

## <a name="step-4-test-the-cicd-pipeline"></a>4. adım: Test CI/CD ardışık düzen

Yapılandırma ile yapılır, bu yeni CI/CD ardışık düzen test zamanı geldi. Test etmek için kolay kaynak kodunu güncelleştirin ve GitHub deponuz içine değişiklikleri yoludur. Birkaç saniye kod itme sonra Visual Studio Team Services içinde çalışan yeni bir derleme görürsünüz. Başarıyla tamamlandığında, yeni bir sürüm tetiklenir ve Azure kapsayıcı hizmeti kümesinde uygulamanın yeni sürümü dağıtılır.

## <a name="next-steps"></a>Sonraki adımlar

* CI/CD Visual Studio Team Services ile ilgili daha fazla bilgi için bkz: [VSTS derleme genel bakış](https://www.visualstudio.com/docs/build/overview).
* ACS altyapısı hakkında daha fazla bilgi için bkz: [ACS altyapısı GitHub deposuna](https://github.com/Azure/acs-engine).
* Docker Swarm modu hakkında daha fazla bilgi için bkz: [Docker Swarm moduna genel bakış](https://docs.docker.com/engine/swarm/).
