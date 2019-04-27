---
title: (KULLANIM DIŞI) Azure Container Service ve Swarm ile CI/CD
description: Azure Container Service Docker Swarm, bir Azure Container Registry ve Azure DevOps ile sürekli .NET Core çok kapsayıcılı bir uygulama sunmak için kullanın
services: container-service
author: jcorioland
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 12/08/2016
ms.author: jucoriol
ms.custom: mvc
ms.openlocfilehash: f28ea3dd2837a241c538057bd118409d4f5b858a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60643787"
---
# <a name="deprecated-full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-azure-devops-services"></a>(KULLANIM DIŞI) Azure DevOps Hizmetleri'ni kullanarak Docker Swarm ile Azure Container Service üzerinde çok kapsayıcılı bir uygulama dağıtmak için tam CI/CD işlem hattı

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Modern bulut uygulamaları geliştirirken en büyük zorluklardan biri bu uygulamaları sürekli teslim çağrılabilmesidir. Bu makalede, bir tam bir sürekli tümleştirme ve dağıtım (CI/CD) işlem hattı Azure Container Service ile Docker Swarm, Azure Container Registry ve Azure işlem hatları yönetimini kullanarak uygulama öğrenin.

Bu makale, bir basit uygulama, kullanılabilir temel [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), ASP.NET Core ile geliştirilen. Uygulama dört farklı hizmetlerinden oluşur: üç web API'leri ve bir web ön ucu:

![MyShop örnek uygulaması](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

Amacı, bu uygulamayı Azure DevOps Hizmetler'i kullanarak bir Docker Swarm kümesinde sürekli teslim sağlamaktır. Aşağıdaki şekil bu sürekli teslim işlem hattı ayrıntıları:

![MyShop örnek uygulaması](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

Kısa bir açıklama adımları şu şekildedir:

1. Kod değişiklikleri için kaynak kodu deposu taahhüt (burada, GitHub) 
1. GitHub Azure DevOps Hizmetleri'nde bir derleme tetikler. 
1. Azure DevOps Hizmetleri, kaynakları en son sürümünü alır ve uygulama oluşturan tüm görüntüleri oluşturur 
1. Azure DevOps hizmetleriyle her görüntü Azure Container Registry hizmeti kullanılarak oluşturulan bir Docker kayıt defterine gönderir. 
1. Azure DevOps hizmetleriyle yeni bir yayın tetiklenir. 
1. Azure kapsayıcı hizmeti küme ana düğümüne SSH kullanarak bazı komutlar yayın çalıştırır 
1. Docker Swarm kümesinde görüntüleri en son sürümünü çeker 
1. Docker Compose kullanarak dağıtılan uygulamanın yeni sürümü 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce aşağıdaki görevleri tamamlamanız gerekir:

- [Azure Container Service'te Swarm kümesi oluşturma](container-service-deployment.md)
- [Azure Container Service'teki Swarm kümesine bağlanma](../container-service-connect.md)
- [Azure container registry oluşturma](../../container-registry/container-registry-get-started-portal.md)
- [Bir Azure DevOps hizmetlerini kuruluş ve oluşturulan proje sahip](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student)
- [GitHub hesabınıza GitHub depo çatalı oluşturma](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

Ayrıca Docker ile yüklü bir Ubuntu (14.04 veya 16.04) makine gerekir. Bu makine, Azure işlem hatları işlemleri sırasında Azure DevOps Hizmetleri tarafından kullanılır. Bu makine oluşturmanın yollarından biri, görüntünün kullanılabilir kullanmaktır [Azure Marketi](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/). 

## <a name="step-1-configure-your-azure-devops-services-organization"></a>1. Adım: Azure DevOps Hizmetleri kuruluşunuz yapılandırın 

Bu bölümde, Azure DevOps Hizmetleri kuruluşunuz yapılandırın.

### <a name="configure-an-azure-devops-services-linux-build-agent"></a>Bir Azure DevOps Hizmetleri Linux derleme Aracısı'nı yapılandırma

Docker görüntülerinizi oluşturmak ve bu görüntüleri bir Azure DevOps Hizmetleri derlemeden bir Azure kapsayıcı kayıt defterine itme için Linux Aracısı'ı kaydetmeniz gerekir. Bu yükleme seçeneğiniz vardır:

* [Linux üzerinde aracı dağıtma](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [Azure DevOps Hizmetleri Aracısı'nı çalıştırmak için Docker'ı kullanma](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-the-docker-integration-azure-devops-services-extension"></a>Docker tümleştirmesi Azure DevOps hizmetler uzantıyı yükleme

Microsoft Azure işlem hatları işlemlerinde Docker'la çalışmak için bir Azure DevOps Hizmetleri Uzantısı sağlar. Bu uzantı kullanılabilir [Azure DevOps Hizmetleri Marketi](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker). Tıklayın **yükleme** bu uzantı, Azure DevOps Hizmetleri kuruluşa eklemek için:

![Docker tümleştirmesi yükleyin](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

Kimlik bilgilerinizi kullanarak Azure DevOps Hizmetleri kuruluşunuza bağlanan istenir. 

### <a name="connect-azure-devops-services-and-github"></a>Azure DevOps bağlanmak Services ve GitHub

Azure DevOps Services projenizi ve GitHub hesabınızı arasında bir bağlantı ayarlayın.

1. Azure DevOps Services projenizde tıklayın **ayarları** simgesini seçin ve araç **Hizmetleri**.

    ![Azure DevOps Hizmetleri - dış bağlantı](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

1. Sol tarafta, tıklayın **yeni hizmet uç noktası** > **GitHub**.

    ![Azure DevOps Hizmetleri - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

1. Azure DevOps Services'ı, GitHub hesabınızı çalışmak üzere yetkilendirmek için tıklatın **Authorize** ve açılan pencerede verilen yordamı izleyin.

    ![Azure DevOps Hizmetleri - GitHub Yetkilendir](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-azure-devops-services-to-your-azure-container-registry-and-azure-container-service-cluster"></a>Azure DevOps Hizmetleri, Azure kapsayıcı kayıt defteri ve Azure Container Service kümesine bağlanma

Azure kapsayıcı kayıt defterinizde ve Docker Swarm kümenizi dış bağlantıları yapılandırmak için CI/CD ardışık alma önce son adımlar yer almaktadır. 

1. İçinde **Hizmetleri** ayarları, Azure DevOps Hizmetleri projenizin bir hizmet uç noktası türü ekleme **Docker kayıt defteri**. 

1. Açılan pencerede açılan URL'yi ve Azure kapsayıcı kayıt defterinizin kimlik bilgilerini girin.

    ![Azure DevOps Hizmetleri - Docker kayıt defteri](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

1. Uç nokta türü için Docker Swarm kümesi ekleme **SSH**. Ardından, Swarm kümesine SSH bağlantı bilgilerini girin.

    ![Azure DevOps Hizmetleri - SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

Tüm yapılandırma artık gerçekleştirilir. Sonraki adımlarda derler ve uygulamayı Docker Swarm kümesi dağıtır CI/CD işlem hattı oluşturun. 

## <a name="step-2-create-the-build-pipeline"></a>2. Adım: Derleme işlem hattı oluşturma

Bu adımda, Azure DevOps Hizmetleri projeniz için bir derleme işlem hattı ayarlayın ve yapı iş akışı için kapsayıcı görüntülerinizi tanımlayın

### <a name="initial-pipeline-setup"></a>İlk komut zinciri Kurulumu

1. Bir derleme işlem hattı oluşturmak için Azure DevOps Services projenize bağlayın ve **derleme ve yayınlama**. 

1. İçinde **yapı tanımları** bölümünde **+ yeni**. Seçin **boş** şablonu.

    ![Azure DevOps - yeni işlem hattı oluşturma](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

1. Bir GitHub deposu kaynağı ile onay yeni derleme yapılandırma **sürekli tümleştirme**, kayıtlı olduğu Linux aracınızı aracı kuyruğu seçin. Tıklayın **Oluştur** derleme işlem hattı oluşturursunuz.

    ![Azure DevOps Hizmetleri - derleme işlem hattı oluşturma](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

1. Üzerinde **derleme tanımları** sayfasında, ilk açın **depo** sekme ve çatal oluşturduğunuz önkoşullarda MyShop projesinin kullanılacak derlemeyi yapılandırın. Seçtiğinizden emin olun *acs-docs* olarak **varsayılan dal**.

    ![Azure DevOps Hizmetleri - derleme deposu yapılandırması](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

1. Üzerinde **Tetikleyicileri** sekmesinde, sonra her bir işlemeyi tetiklenmesi için yapı yapılandırın. Seçin **sürekli tümleştirme** ve **toplu değişiklikler**.

    ![Azure DevOps Hizmetleri - derleme tetikleyici yapılandırması](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-the-build-workflow"></a>Yapı iş akışı tanımlama
Sonraki adımlar, yapı iş akışı tanımlayın. Oluşturmak için beş kapsayıcı görüntülerini vardır *MyShop* uygulama. Her bir görüntü kullanarak proje klasörleri'nde bulunan Dockerfile oluşturulmuştur:

* ProductsApi
* Ara sunucu
* RatingsApi
* RecommendationsApi
* ShopFront

Her bir görüntü, bir görüntü oluşturun ve bir Azure kapsayıcı kayıt defterini kullanarak görüntüyü gönderin iki Docker adımı eklemeniz gerekir. 

1. Derleme iş akışında bir adım eklemek için tıklatın **+ derleme adımı Ekle** seçip **Docker**.

    ![Azure DevOps Hizmetleri - derleme adımları Ekle](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

1. Her bir görüntü için kullandığı bir adım yapılandırma `docker build` komutu.

    ![Azure DevOps Hizmetleri - Docker derleme](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    Oluşturma işlemi için Azure kapsayıcı kayıt defteri seçin **bir görüntü oluşturun** eylem ve her görüntü tanımlar Dockerfile. Ayarlama **derleme bağlamı** Dockerfile kök dizini ve tanımlama **görüntü adı**. 
    
    Görüntü adı, önceki ekranda gösterildiği gibi Azure kapsayıcı kayıt defterinizin URI ile başlayın. (Ayrıca bir derleme değişkeni etiket resminin derleme tanımlayıcısı bu örnekteki gibi parametre haline getirmek için kullanabilirsiniz.)

1. Her bir görüntü için kullandığı ikinci bir adım yapılandırma `docker push` komutu.

    ![Azure DevOps Hizmetleri - Docker itme](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    Gönderme işlemi için Azure kapsayıcı kayıt defteri seçin **görüntü gönderebilmeniz** eylem girin **görüntü adı** önceki adımda oluşturulan.

1. Derleme ve gönderme adımları her beş görüntüleri için yapılandırdıktan sonra yapı iş akışınızda iki adım daha ekleyin.

    a. Değiştirmek için bir bash komut dosyası kullanan bir komut satırı görevi *BuildNumber* docker-compose.yml dosyası geçerli yineleme derleme kimliği. Ayrıntılar için aşağıdaki ekranı görürsünüz.

    ![Azure DevOps Hizmetleri - güncelleştirme Compose dosyası](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    b. Bu sürümde kullanılabilir, böylece güncelleştirilmiş Compose dosyası bir derleme yapıtı bıraktığı bir görev. Ayrıntılar için aşağıdaki ekranı görürsünüz.

    ![Azure DevOps hizmetleri - yayımlama Compose dosyası](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

1. Tıklayın **Kaydet** ve, derleme işlem hattı adı.

## <a name="step-3-create-the-release-pipeline"></a>3. Adım: Yayın işlem hattı oluşturma

Azure DevOps hizmetleri sayesinde [ortamlar genelinde sürümleri yönetmek](https://www.visualstudio.com/team-services/release-management/). Uygulamanızın düzgün bir şekilde (örneğin, geliştirme, test, üretim öncesi ve üretim gibi) farklı ortamlarınızda şekilde dağıtıldığından emin olmak sürekli dağıtımı etkinleştirebilirsiniz. Azure Container Service, Docker Swarm kümenizi temsil eden yeni bir ortam oluşturabilirsiniz.

![Azure DevOps Hizmetleri - ACS sürümüne](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>İlk yayın Kurulumu

1. Yayın işlem hattı oluşturmak için tıklayın **yayınlar** > **+ yayın**

1. Yapıt kaynağı yapılandırmak için tıklayın **Yapıtları** > **yapıt kaynağı Bağla**. Burada, bu yeni yayın ardışık düzeni önceki adımda tanımlanan yapı bağlayın. Bunu yaparak, docker-compose.yml dosyası sürüm sürecinizde kullanılabilir.

    ![Azure DevOps Hizmetleri - sürüm yapıları](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

1. Yayın tetikleyicisi yapılandırmak için tıklayın **Tetikleyicileri** seçip **sürekli dağıtım**. Aynı yapıt kaynağında tetikleyici ayarlayın. Bu ayar, yeni bir yayın derleme başarıyla tamamlanır tamamlanmaz başlar sağlar.

    ![Azure DevOps Hizmetleri - yayın Tetikleyicileri](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-the-release-workflow"></a>Yayın iş akışı tanımlama

Yayın iş akışı, eklediğiniz iki görevlerini oluşur.

1. Compose dosyası için güvenli bir şekilde kopyalamak için bir görevi yapılandırmaya bir *dağıtma* daha önce yapılandırdığınız SSH bağlantısını kullanarak Docker Swarm ana düğüme klasör. Ayrıntılar için aşağıdaki ekranı görürsünüz.

    ![Azure DevOps Hizmetleri - sürüm SCP](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

1. Çalıştırılacak bir bash komut yürütmek için ikinci bir görevi yapılandırmaya `docker` ve `docker-compose` ana düğüm komutları. Ayrıntılar için aşağıdaki ekranı görürsünüz.

    ![Azure DevOps Hizmetleri - sürüm Bash](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    Komutu, Docker CLI ve Docker-Compose CLI'yı aşağıdaki görevleri gerçekleştirmek için ana kullanım yürütüldü:

   - Azure container Registry'de oturum açın (tanımlanan üç derleme variab'les kullanan **değişkenleri** sekmesinde)
   - Tanımlama **DOCKER_HOST** Swarm uç nokta ile çalışmaya değişkeni (: 2375)
   - Gidin *dağıtma* önceki güvenli kopyalama görevi tarafından oluşturulan ve docker-compose.yml dosyasını içeren klasör 
   - Yürütme `docker-compose` yeni görüntüleri çekmek komutları hizmetleri durdurun, olan hizmetleri kaldırın ve kapsayıcı oluşturma.

     >[!IMPORTANT]
     > Önceki ekranda göründüğü gibi bırakın **STDERR üzerinde başarısız** onay kutusunu işaretlemeden. Bunun nedeni bir önemli ayarı, `docker-compose` gibi durdurma veya standart hata çıktı silinmesini kapsayıcılardır birkaç tanılama iletilerini yazdırır. Onay kutusunu işaretlerseniz, tüm bile yapıldığında Azure DevOps Hizmetleri yayın sırasında hata oluştuğunu bildirir.
     >
1. Bu yeni yayın ardışık düzeni kaydedin.


>[!NOTE]
>Bu dağıtım, çünkü biz eski hizmetleri durdurma ve yenisini çalıştıran bazı kapalı kalma süresi içerir. Mavi-yeşil dağıtım yaparak bu durumu önlemek mümkündür.
>

## <a name="step-4-test-the-cicd-pipeline"></a>4. Adım. CI/CD işlem hattı test

Yapılandırmasını tamamladıktan sonra bunu bu yeni CI/CD işlem hattı test etme vakti. Test etmek için en kolay yolu, kaynak kodunu güncelleştirin ve değişiklikleri GitHub deponuza sağlamaktır. Kod gönderdikten sonra birkaç saniye içinde Azure DevOps Hizmetleri çalıştıran yeni bir derleme görürsünüz. Başarıyla tamamlandığında, yeni bir yayın tetiklenir ve Azure Container Service kümesinde uygulamanın yeni sürümünü dağıtır.

## <a name="next-steps"></a>Sonraki Adımlar

* Azure DevOps Services ile CI/CD hakkında daha fazla bilgi için bkz: [Azure DevOps derleme hizmetleri genel bakış](https://www.visualstudio.com/docs/build/overview).
