---
title: Azure Container Service ve Swarm ile CI/CD
description: Azure Container Service Docker Swarm, bir Azure Container Registry ve Visual Studio Team Services ile sürekli olarak bir çok kapsayıcılı .NET Core uygulaması sunma
services: container-service
author: jcorioland
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 12/08/2016
ms.author: jucoriol
ms.custom: mvc
ms.openlocfilehash: ac3133ac093d578c89d24bddd1cc0a7c9588c2fd
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715007"
---
# <a name="full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-visual-studio-team-services"></a>Visual Studio Team Services kullanarak Docker Swarm ile Azure Container Service üzerinde çok kapsayıcılı bir uygulama dağıtmak için tam CI/CD işlem hattı

Modern bulut uygulamaları geliştirirken en büyük zorluklardan biri bu uygulamaları sürekli teslim çağrılabilmesidir. Bu makalede, bir tam bir sürekli tümleştirme ve dağıtım (CI/CD) işlem hattı Azure Container Service ile Docker Swarm, Azure Container Registry ve Visual Studio Team Services derleme kullanarak uygulayın ve release management hakkında bilgi edinin.

Bu makale, bir basit uygulama, kullanılabilir temel [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), ASP.NET Core ile geliştirilen. Uygulama dört farklı hizmetlerinden oluşur: üç web API'leri ve bir web ön ucu:

![MyShop örnek uygulaması](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

Amacı, bu uygulama Visual Studio Team Services kullanarak bir Docker Swarm kümesinde sürekli teslim sağlamaktır. Aşağıdaki şekil bu sürekli teslim işlem hattı ayrıntıları:

![MyShop örnek uygulaması](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

Kısa bir açıklama adımları şu şekildedir:

1. Kod değişiklikleri için kaynak kodu deposu taahhüt (burada, GitHub) 
1. GitHub, Visual Studio Team Services'da bir derleme tetikler 
1. Visual Studio Team Services, kaynakları en son sürümünü alır ve uygulama oluşturan tüm görüntüleri oluşturur 
1. Visual Studio Team Services her görüntü Azure Container Registry hizmeti kullanılarak oluşturulan bir Docker kayıt defterine gönderir. 
1. Visual Studio Team Services'ı yeni bir yayın Tetikleyicileri 
1. Azure kapsayıcı hizmeti küme ana düğümüne SSH kullanarak bazı komutlar yayın çalıştırır 
1. Docker Swarm kümesinde görüntüleri en son sürümünü çeker 
1. Docker Compose kullanarak dağıtılan uygulamanın yeni sürümü 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce aşağıdaki görevleri tamamlamanız gerekir:

- [Azure Container Service'te Swarm kümesi oluşturma](container-service-deployment.md)
- [Azure Container Service'teki Swarm kümesine bağlanma](../container-service-connect.md)
- [Azure container registry oluşturma](../../container-registry/container-registry-get-started-portal.md)
- [Oluşturulan bir Visual Studio Team Services hesabı ve takım projesine sahip olmak](https://docs.microsoft.com/vsts/organizations/accounts/create-organization-msa-or-work-student)
- [GitHub hesabınıza GitHub depo çatalı oluşturma](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

Ayrıca Docker ile yüklü bir Ubuntu (14.04 veya 16.04) makine gerekir. Bu makine tarafından Visual Studio Team Services derleme ve yayın işlemleri sırasında kullanılır. Bu makine oluşturmanın yollarından biri, görüntünün kullanılabilir kullanmaktır [Azure Marketi](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/). 

## <a name="step-1-configure-your-visual-studio-team-services-account"></a>1. adım: Visual Studio Team Services hesabınızı yapılandırın 

Bu bölümde, Visual Studio Team Services hesabınızı yapılandırın.

### <a name="configure-a-visual-studio-team-services-linux-build-agent"></a>Visual Studio Team Services Linux derleme Aracısı'nı yapılandırma

Bu görüntüler, Visual Studio Team Services derleme bir Azure kapsayıcı kayıt defterine gönderin ve Docker görüntüleri oluşturma hakkında bilgi için bir Linux Aracısı'ı kaydetmeniz gerekir. Bu yükleme seçeneğiniz vardır:

* [Linux üzerinde aracı dağıtma](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [VSTS Aracısı'nı çalıştırmak için Docker'ı kullanma](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-the-docker-integration-vsts-extension"></a>Docker tümleştirmesi VSTS uzantısı yükleme

Microsoft yapı Docker'la çalışmak ve işlemleri serbest bırakmak için bir VSTS uzantısı sağlar. Bu uzantı kullanılabilir [VSTS Market](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker). Tıklayın **yükleme** bu uzantı için VSTS hesabınızı eklemek için:

![Docker tümleştirmesi yükleyin](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

Kimlik bilgilerinizi kullanarak VSTS hesabınıza bağlanmak için istenir. 

### <a name="connect-visual-studio-team-services-and-github"></a>Visual Studio Team Services ve GitHub'ı bağlama

VSTS projenizin ve GitHub hesabınızı arasında bir bağlantı ayarlayın.

1. Visual Studio Team Services projenizde tıklayın **ayarları** simgesini seçin ve araç **Hizmetleri**.

    ![Visual Studio Team Services - dış bağlantı](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

1. Sol tarafta, tıklayın **yeni hizmet uç noktası** > **GitHub**.

    ![Visual Studio Team Services - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

1. VSTS, GitHub hesabınızla çalışmak için yetkilendirmek için tıklatın **Authorize** ve açılan pencerede verilen yordamı izleyin.

    ![Visual Studio Team Services - GitHub Yetkilendir](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-vsts-to-your-azure-container-registry-and-azure-container-service-cluster"></a>VSTS, Azure kapsayıcı kayıt defteri ve Azure Container Service kümesine bağlanma

Azure kapsayıcı kayıt defterinizde ve Docker Swarm kümenizi dış bağlantıları yapılandırmak için CI/CD ardışık alma önce son adımlar yer almaktadır. 

1. İçinde **Hizmetleri** ayarlarını Visual Studio Team Services projenize bir hizmet uç noktası türü ekleme **Docker kayıt defteri**. 

1. Açılan pencerede açılan URL'yi ve Azure kapsayıcı kayıt defterinizin kimlik bilgilerini girin.

    ![Visual Studio Team Services - Docker kayıt defteri](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

1. Uç nokta türü için Docker Swarm kümesi ekleme **SSH**. Ardından, Swarm kümesine SSH bağlantı bilgilerini girin.

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

Tüm yapılandırma artık gerçekleştirilir. Sonraki adımlarda derler ve uygulamayı Docker Swarm kümesi dağıtır CI/CD işlem hattı oluşturun. 

## <a name="step-2-create-the-build-definition"></a>2. adım: derleme tanımı oluşturma

Bu adımda, bir derleme definitionfor VSTS projenizi ayarlar ve yapı iş akışı için kapsayıcı görüntülerinizi tanımlayın

### <a name="initial-definition-setup"></a>İlk tanım Kurulumu

1. Bir yapı tanımı oluşturmak için Visual Studio Team Services projenize bağlayın ve **derleme ve yayınlama**. 

1. İçinde **yapı tanımları** bölümünde **+ yeni**. Seçin **boş** şablonu.

    ![Visual Studio Team Services - yeni derleme tanımı](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

1. Bir GitHub deposu kaynağı ile onay yeni derleme yapılandırma **sürekli tümleştirme**, kayıtlı olduğu Linux aracınızı aracı kuyruğu seçin. Tıklayın **Oluştur** yapı tanımı oluşturmak için.

    ![Visual Studio Team Services - derleme tanımı oluşturma](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

1. Üzerinde **derleme tanımları** sayfasında, ilk açın **depo** sekme ve çatal oluşturduğunuz önkoşullarda MyShop projesinin kullanılacak derlemeyi yapılandırın. Seçtiğinizden emin olun *acs-docs* olarak **varsayılan dal**.

    ![Visual Studio Team Services - derleme deposu yapılandırması](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

1. Üzerinde **Tetikleyicileri** sekmesinde, sonra her bir işlemeyi tetiklenmesi için yapı yapılandırın. Seçin **sürekli tümleştirme** ve **toplu değişiklikler**.

    ![Visual Studio Team Services - derleme tetikleyici yapılandırması](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-the-build-workflow"></a>Yapı iş akışı tanımlama
Sonraki adımlar, yapı iş akışı tanımlayın. Oluşturmak için beş kapsayıcı görüntülerini vardır *MyShop* uygulama. Her bir görüntü kullanarak proje klasörleri'nde bulunan Dockerfile oluşturulmuştur:

* ProductsApi
* Ara sunucu
* RatingsApi
* RecommandationsApi
* ShopFront

Her bir görüntü, bir görüntü oluşturun ve bir Azure kapsayıcı kayıt defterini kullanarak görüntüyü gönderin iki Docker adımı eklemeniz gerekir. 

1. Derleme iş akışında bir adım eklemek için tıklatın **+ derleme adımı Ekle** seçip **Docker**.

    ![Visual Studio Team Services - derleme adımları Ekle](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

1. Her bir görüntü için kullandığı bir adım yapılandırma `docker build` komutu.

    ![Visual Studio Team Services - Docker derleme](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    Oluşturma işlemi için Azure kapsayıcı kayıt defteri seçin **bir görüntü oluşturun** eylem ve her görüntü tanımlar Dockerfile. Ayarlama **derleme bağlamı** Dockerfile kök dizini ve tanımlama **görüntü adı**. 
    
    Görüntü adı, önceki ekranda gösterildiği gibi Azure kapsayıcı kayıt defterinizin URI ile başlayın. (Ayrıca bir derleme değişkeni etiket resminin derleme tanımlayıcısı bu örnekteki gibi parametre haline getirmek için kullanabilirsiniz.)

1. Her bir görüntü için kullandığı ikinci bir adım yapılandırma `docker push` komutu.

    ![Visual Studio Team Services - Docker itme](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    Gönderme işlemi için Azure kapsayıcı kayıt defteri seçin **görüntü gönderebilmeniz** eylem girin **görüntü adı** önceki adımda oluşturulan.

1. Derleme ve gönderme adımları her beş görüntüleri için yapılandırdıktan sonra yapı iş akışınızda iki adım daha ekleyin.

    a. Değiştirmek için bir bash komut dosyası kullanan bir komut satırı görevi *BuildNumber* docker-compose.yml dosyası geçerli yineleme derleme kimliği. Ayrıntılar için aşağıdaki ekranı görürsünüz.

    ![Visual Studio Team Services - güncelleştirme Compose dosyası](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    b. Bu sürümde kullanılabilir, böylece güncelleştirilmiş Compose dosyası bir derleme yapıtı bıraktığı bir görev. Ayrıntılar için aşağıdaki ekranı görürsünüz.

    ![Visual Studio Team Services - yayımlama Compose dosyası](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

1. Tıklayın **Kaydet** ve yapı tanımınızı adlandırın.

## <a name="step-3-create-the-release-definition"></a>3. adım: yayın tanımı oluşturma

Visual Studio Team Services sayesinde [ortamlar genelinde sürümleri yönetmek](https://www.visualstudio.com/team-services/release-management/). Uygulamanızın düzgün bir şekilde (örneğin, geliştirme, test, üretim öncesi ve üretim gibi) farklı ortamlarınızda şekilde dağıtıldığından emin olmak sürekli dağıtımı etkinleştirebilirsiniz. Azure Container Service, Docker Swarm kümenizi temsil eden yeni bir ortam oluşturabilirsiniz.

![Visual Studio Team Services - ACS sürümüne](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>İlk yayın Kurulumu

1. Bir yayın tanımı oluşturmak için tıklayın **yayınlar** > **+ yayın**

1. Yapıt kaynağı yapılandırmak için tıklayın **Yapıtları** > **yapıt kaynağı Bağla**. Burada, bu yeni yayın tanımı önceki adımda tanımlanan yapı bağlayın. Bunu yaparak, docker-compose.yml dosyası sürüm sürecinizde kullanılabilir.

    ![Visual Studio Team Services - sürüm yapıları](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

1. Yayın tetikleyicisi yapılandırmak için tıklayın **Tetikleyicileri** seçip **sürekli dağıtım**. Aynı yapıt kaynağında tetikleyici ayarlayın. Bu ayar, yeni bir yayın derleme başarıyla tamamlanır tamamlanmaz başlar sağlar.

    ![Visual Studio Team Services - yayın Tetikleyicileri](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-the-release-workflow"></a>Yayın iş akışı tanımlama

Yayın iş akışı, eklediğiniz iki görevlerini oluşur.

1. Compose dosyası için güvenli bir şekilde kopyalamak için bir görevi yapılandırmaya bir *dağıtma* daha önce yapılandırdığınız SSH bağlantısını kullanarak Docker Swarm ana düğüme klasör. Ayrıntılar için aşağıdaki ekranı görürsünüz.

    ![Visual Studio Team Services - sürüm SCP](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

1. Çalıştırılacak bir bash komut yürütmek için ikinci bir görevi yapılandırmaya `docker` ve `docker-compose` ana düğüm komutları. Ayrıntılar için aşağıdaki ekranı görürsünüz.

    ![Visual Studio Team Services - sürüm Bash](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    Komutu, Docker CLI ve Docker-Compose CLI'yı aşağıdaki görevleri gerçekleştirmek için ana kullanım yürütüldü:

    - Azure container Registry'de oturum açın (tanımlanan üç derleme variab'les kullanan **değişkenleri** sekmesinde)
    - Tanımlama **DOCKER_HOST** Swarm uç nokta ile çalışmaya değişkeni (: 2375)
    - Gidin *dağıtma* önceki güvenli kopyalama görevi tarafından oluşturulan ve docker-compose.yml dosyasını içeren klasör 
    - Yürütme `docker-compose` yeni görüntüleri çekmek komutları hizmetleri durdurun, olan hizmetleri kaldırın ve kapsayıcı oluşturma.

    >[!IMPORTANT]
    > Önceki ekranda göründüğü gibi bırakın **STDERR üzerinde başarısız** onay kutusunu işaretlemeden. Bunun nedeni bir önemli ayarı, `docker-compose` gibi durdurma veya standart hata çıktı silinmesini kapsayıcılardır birkaç tanılama iletilerini yazdırır. Onay kutusunu işaretleyin, tüm aşsa bile iyi Visual Studio Team Services yayın sırasında hataları oluştuğunu bildirir.
    >
1. Bu yeni yayın tanımı kaydedin.


>[!NOTE]
>Bu dağıtım, çünkü biz eski hizmetleri durdurma ve yenisini çalıştıran bazı kapalı kalma süresi içerir. Mavi-yeşil dağıtım yaparak bu durumu önlemek mümkündür.
>

## <a name="step-4-test-the-cicd-pipeline"></a>4. Adım. CI/CD işlem hattı test

Yapılandırmasını tamamladıktan sonra bunu bu yeni CI/CD işlem hattı test etme vakti. Test etmek için en kolay yolu, kaynak kodunu güncelleştirin ve değişiklikleri GitHub deponuza sağlamaktır. Kod gönderdikten sonra birkaç saniye içinde Visual Studio Team Services'ı çalıştıran yeni bir derleme görürsünüz. Başarıyla tamamlandığında, yeni bir yayın tetiklenir ve Azure Container Service kümesinde uygulamanın yeni sürümünü dağıtır.

## <a name="next-steps"></a>Sonraki Adımlar

* Visual Studio Team Services ile CI/CD hakkında daha fazla bilgi için bkz: [VSTS derleme genel bakış](https://www.visualstudio.com/docs/build/overview).
