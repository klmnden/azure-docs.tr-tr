---
title: "CI/CD Azure kapsayıcı hizmeti ve Swarm ile | Microsoft Docs"
description: "Azure kapsayıcı hizmeti sürekli olarak çok kapsayıcı .NET Core uygulama göndermek için Docker Swarm, bir Azure kapsayıcı kayıt defteri ve Visual Studio Team Services ile kullanma"
services: container-service
documentationcenter: " "
author: jcorioland
manager: pierlag
tags: acs, azure-container-service
keywords: "Docker, kapsayıcıları, mikro-services, Azure, Visual Studio Team Services, DevOps Swarm"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/08/2016
ms.author: jucoriol
ms.custom: mvc
ms.openlocfilehash: 99c27c37218a35d2a3416d6edd5e0a871cd5c011
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="full-cicd-pipeline-to-deploy-a-multi-container-application-on-azure-container-service-with-docker-swarm-using-visual-studio-team-services"></a>Visual Studio Team Services kullanarak Docker Swarm ile Azure kapsayıcı hizmeti üzerinde çok kapsayıcı uygulama dağıtmak için tam CI/CD ardışık düzen

Modern bulut uygulamaları geliştirirken en büyük zorluklardan biri, bu uygulamalara sürekli olarak teslim etmek mümkün yapılıyor. Bu makalede, bir tam sürekli tümleştirme ve Docker Swarm, Azure kapsayıcı kayıt defteri ve Visual Studio Team Services yapı ile Azure kapsayıcı hizmeti kullanılarak (CI/CD) dağıtım ardışık düzen uygulamak ve yayın Yönetimi öğrenin.

Bu makalede, kullanılabilen basit bir uygulama dayalı [GitHub](https://github.com/jcorioland/MyShop/tree/acs-docs), ASP.NET Core ile geliştirilen. Uygulama dört farklı hizmetlerinden oluşur: üç API'ları ve bir web ön uç web:

![MyShop örnek uygulama](./media/container-service-docker-swarm-setup-ci-cd/myshop-application.png)

Amacı, bu uygulama, Visual Studio Team Services kullanarak bir Docker Swarm kümesinde, sürekli olarak teslim etmek sağlamaktır. Aşağıdaki şekilde bu kesintisiz teslim ardışık düzen ayrıntıları:

![MyShop örnek uygulama](./media/container-service-docker-swarm-setup-ci-cd/full-ci-cd-pipeline.png)

Kısa bir açıklama adımları şöyledir:

1. Kod değişiklikleri kaynak kodu depoya taahhüt (burada, GitHub) 
2. Visual Studio Team Services derlemede GitHub tetikler 
3. Visual Studio Team Services kaynakları en son sürümünü alır ve uygulamayı oluşturan tüm görüntü oluşturur 
4. Visual Studio Team Services her görüntü Azure kapsayıcı kayıt defteri hizmeti kullanılarak oluşturulan bir Docker kayıt defterine iter. 
5. Yeni bir sürüm Visual Studio Team Services tetikler 
6. Azure kapsayıcı hizmeti küme ana düğümünde SSH kullanarak bazı komutları yayın çalıştırır 
7. Docker Swarm kümesinde görüntüleri en son sürümünü çeker 
8. Uygulamanın yeni sürümü Docker Compose kullanılarak dağıtılır 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce aşağıdaki görevleri tamamlamanız gerekir:

- [Azure Container Service'te Swarm kümesi oluşturma](container-service-deployment.md)
- [Azure Container Service'teki Swarm kümesine bağlanma](../container-service-connect.md)
- [Azure kapsayıcı kayıt defteri oluşturma](../../container-registry/container-registry-get-started-portal.md)
- [Oluşturulan Visual Studio Team Services hesabı ve ekip projesinde olması](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
- [GitHub hesabınızda GitHub depoyu çatallaştırmanız](https://github.com/jcorioland/MyShop/)

[!INCLUDE [container-service-swarm-mode-note](../../../includes/container-service-swarm-mode-note.md)]

Ayrıca bir Ubuntu (14.04 veya 16.04) makinede yüklü Docker ile gerekir. Bu makinede Visual Studio Team Services tarafından derleme ve sürüm işlemleri sırasında kullanılır. Bu makine oluşturmanın yollarından biri, görüntünün bulunan kullanmaktır [Azure Marketi](https://azure.microsoft.com/marketplace/partners/canonicalandmsopentech/dockeronubuntuserver1404lts/). 

## <a name="step-1-configure-your-visual-studio-team-services-account"></a>1. adım: Visual Studio Team Services hesabınızın yapılandırma 

Bu bölümde, Visual Studio Team Services hesabınızın yapılandırın.

### <a name="configure-a-visual-studio-team-services-linux-build-agent"></a>Visual Studio Team Services Linux derleme Aracısı'nı yapılandırma

Docker görüntüleri oluşturmak ve bu görüntüleri Visual Studio Team Services derleme Azure kapsayıcı kayıt defterine itme için Linux Aracısı kaydetmeniz gerekir. Bu yükleme seçeneğiniz vardır:

* [Linux üzerinde bir aracıyla Dağıt](https://www.visualstudio.com/docs/build/admin/agents/v2-linux)

* [VSTS Aracısı'nı çalıştırmak için Docker kullanın](https://hub.docker.com/r/microsoft/vsts-agent)

### <a name="install-the-docker-integration-vsts-extension"></a>Docker tümleştirmesi VSTS uzantısını yükleyin

Microsoft, yapı Docker ile çalışmaya ve işlemler yayın VSTS uzantısı sağlar. Bu uzantı kullanılabilir [VSTS Market](https://marketplace.visualstudio.com/items?itemName=ms-vscs-rm.docker). Tıklatın **yükleme** bu uzantı VSTS hesabınızı eklemek için:

![Docker tümleştirmesi yükleyin](./media/container-service-docker-swarm-setup-ci-cd/install-docker-vsts.png)

VSTS hesabınıza kimlik bilgilerinizi kullanarak bağlanın istenir. 

### <a name="connect-visual-studio-team-services-and-github"></a>Visual Studio Team Services ve GitHub Bağlan

VSTS projeniz, GitHub hesabınızda arasında bir bağlantı ayarlayın.

1. Visual Studio Team Services projenizi tıklatın **ayarları** simgesini seçin ve araç **Hizmetleri**.

    ![Visual Studio Team Services - dış bağlantı](./media/container-service-docker-swarm-setup-ci-cd/vsts-services-menu.png)

2. Sol bölmede, tıklatın **yeni hizmet uç noktası** > **GitHub**.

    ![Visual Studio Team Services - GitHub](./media/container-service-docker-swarm-setup-ci-cd/vsts-github.png)

3. GitHub hesabınızla çalışmaya VSTS yetkilendirmek için tıklatın **Authorize** ve açılır pencere yordamı izleyin.

    ![Visual Studio Team Services - GitHub yetkilendirmek](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-authorize.png)

### <a name="connect-vsts-to-your-azure-container-registry-and-azure-container-service-cluster"></a>VSTS, Azure kapsayıcı kayıt defteri ve Azure kapsayıcı hizmeti kümesine bağlanma

CI/CD ardışık düzenine alma önce son Azure'da kapsayıcı kaydınız ve Docker Swarm kümesi dış bağlantıları yapılandırmak için adımlardır. 

1. İçinde **Hizmetleri** Visual Studio Team Services projenizin ayarları ekleme Hizmeti uç noktası türü **Docker kayıt defteri**. 

2. Açılır açılan penceresinde, URL ve Azure kapsayıcı kaydınız kimlik bilgilerini girin.

    ![Visual Studio Team Services - Docker kayıt defteri](./media/container-service-docker-swarm-setup-ci-cd/vsts-registry.png)

3. Docker Swarm kümesi için bir uç nokta türü ekleme **SSH**. Ardından Swarm kümenizin SSH bağlantı bilgilerini girin.

    ![Visual Studio Team Services - SSH](./media/container-service-docker-swarm-setup-ci-cd/vsts-ssh.png)

Tüm yapılandırma şimdi yapılır. Sonraki adımda oluşturan ve Docker Swarm kümesi uygulamaya dağıtır CI/CD ardışık düzen oluşturun. 

## <a name="step-2-create-the-build-definition"></a>2. adım: derleme tanımı oluşturma

Bu adımda, bir yapı definitionfor VSTS projenizi ayarlayın ve yapı iş akışı için kapsayıcı görüntülerinizi tanımlayın

### <a name="initial-definition-setup"></a>Başlangıç tanım Kurulumu

1. Yapı tanımı oluşturmak için Visual Studio Team Services projenize bağlanmak ve **yapı & yayın**. 

2. İçinde **yapı tanımları** 'yi tıklatın **+ yeni**. Seçin **boş** şablonu.

    ![Visual Studio Team Services - yeni yapı tanımı](./media/container-service-docker-swarm-setup-ci-cd/create-build-vsts.png)

3. GitHub depo ile kaynak denetimi yeni bir yapı yapılandırma **sürekli tümleştirme**ve Linux Aracısı kayıtlı olduğu Aracısı kuyruğu seçin. Tıklatın **oluşturma** derleme tanımı oluşturmak için.

    ![Visual Studio Team Services - derleme tanımı oluştur](./media/container-service-docker-swarm-setup-ci-cd/vsts-create-build-github.png)

4. Üzerinde **yapı tanımlarını** sayfasında, ilk açmak **depo** sekmesinde ve önkoşullar oluşturulan MyShop proje çatalı kullanmak için yapıyı yapılandırın. Seçtiğinizden emin olun *acs belgeleri* olarak **varsayılan dalı**.

    ![Visual Studio Team Services - derleme deposu yapılandırma](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-repo-conf.png)

5. Üzerinde **Tetikleyicileri** sekmesinde, her yürütme tetiklenmesi için yapıyı yapılandırın. Seçin **sürekli tümleştirme** ve **toplu değişiklikleri**.

    ![Visual Studio Team Services - derleme tetikleyici yapılandırması](./media/container-service-docker-swarm-setup-ci-cd/vsts-github-trigger-conf.png)

### <a name="define-the-build-workflow"></a>Yapı iş akışı tanımlayın
Sonraki adımlar yapı iş akışı tanımlayın. Beş kapsayıcı görüntülerini oluşturmak için *MyShop* uygulama. Her görüntü proje klasörleri'nde bulunan Dockerfile kullanılarak oluşturulur:

* ProductsApi
* Ara sunucu
* RatingsApi
* RecommandationsApi
* ShopFront

Her görüntü, görüntü oluşturmak için diğeri Azure kapsayıcı kayıt defterinde Görüntü göndermeyi için iki Docker adımı eklemeniz gerekir. 

1. Yapı iş akışında bir adım eklemek için tıklatın **+ Ekle derleme adımı** seçip **Docker**.

    ![Visual Studio Team Services - eklemek derleme adımları](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-add-task.png)

2. Her görüntü için kullandığı bir adım yapılandırma `docker build` komutu.

    ![Visual Studio Team Services - Docker derleme](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-build.png)

    Derleme işlemi için Azure kapsayıcı kayıt defteri seçin **görüntü yapı** eylem ve her görüntü tanımlar Dockerfile. Ayarlama **yapı bağlam** Dockerfile kök dizini ve tanımlayın **görüntü adı**. 
    
    Önceki ekranda gösterildiği gibi görüntü adı Azure kapsayıcı kaydınız URI ile başlatın. (Ayrıca bir yapı değişkeni Bu örnekte derleme tanımlayıcı gibi görüntü etiketi Parametreleştirme için kullanabileceğiniz.)

3. Her görüntü için kullanır ikinci bir adım yapılandırma `docker push` komutu.

    ![Visual Studio Team Services - Docker gönderme](./media/container-service-docker-swarm-setup-ci-cd/vsts-docker-push.png)

    Gönderme işlemi için Azure kapsayıcı kayıt defteri seçin **görüntü anında** eylemi ve girin **görüntü adı** önceki adımda oluşturulmuş.

4. Derleme ve anında iletme adımları her beş görüntüleri için yapılandırdıktan sonra iki daha fazla adım yapı iş akışında ekleyin.

    a. Değiştirmek için bir bash komut dosyası kullanan bir komut satırı görevi *BuildNumber* docker-compose.yml dosyası geçerli yineleme derleme kimliği Ayrıntılar için aşağıdaki ekran görüntüsüne bakın.

    ![Visual Studio Team Services - güncelleştirme oluşturma dosya](./media/container-service-docker-swarm-setup-ci-cd/vsts-build-replace-build-number.png)

    b. Bu sürümde kullanılabilir, böylece güncelleştirilmiş Oluştur dosya derleme yapısı olarak bırakır bir görev. Ayrıntılar için aşağıdaki ekran görüntüsüne bakın.

    ![Visual Studio Team Services - Oluştur yayımlama dosyası](./media/container-service-docker-swarm-setup-ci-cd/vsts-publish-compose.png) 

5. Tıklatın **kaydetmek** ve yapı tanımının adı.

## <a name="step-3-create-the-release-definition"></a>3. adım: sürüm tanımı oluşturma

Visual Studio Team Services olanak tanır [sürümleri arasında ortamlarını](https://www.visualstudio.com/team-services/release-management/). Kesintisiz bir şekilde uygulamanız farklı ortamınızı (örneğin, geliştirme, test, ön üretim ve üretim) üzerinde dağıtıldığından emin olmak sürekli dağıtım etkinleştirebilirsiniz. Azure kapsayıcı hizmeti Docker Swarm kümesi temsil eden yeni bir ortam oluşturabilirsiniz.

![Visual Studio Team Services - ACS sürüme](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-acs.png) 

### <a name="initial-release-setup"></a>Kurulum ilk sürüm

1. Bir yayın tanımı oluşturmak için tıklatın **sürümleri** > **+ sürüm**

2. Yapı kaynağını yapılandırmak için tıklatın **yapıları** > **bir yapı kaynak bağlantı**. Burada, bu yeni sürüm tanımı önceki adımda tanımlanan yapı bağlayın. Bunu yaparak, docker-compose.yml dosyası yayın işlemde kullanılabilir.

    ![Visual Studio Team Services - sürüm yapıları](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-artefacts.png) 

3. Yayın tetikleyici yapılandırmak için tıklatın **Tetikleyicileri** seçip **sürekli dağıtım**. Tetikleyici aynı yapı kaynağında ayarlayın. Bu ayar, yapı başarıyla tamamlanır tamamlanmaz yeni bir sürüm başlayacağını sağlar.

    ![Visual Studio Team Services - sürüm Tetikleyicileri](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-trigger.png) 

### <a name="define-the-release-workflow"></a>Yayın iş akışı tanımlama

Yayın iş akışı eklediğiniz iki görevlerin oluşur.

1. Güvenli bir şekilde Oluştur dosyasına kopyalamak için bir görevi yapılandırmaya bir *dağıtmak* daha önce yapılandırdığınız SSH bağlantısını kullanarak Docker Swarm ana düğümde, klasör. Ayrıntılar için aşağıdaki ekran görüntüsüne bakın.

    ![Visual Studio Team Services - sürüm SCP](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-scp.png)

2. Çalıştırılacak bash komutu yürütmek için ikinci bir görevi yapılandırmaya `docker` ve `docker-compose` ana düğümde komutları. Ayrıntılar için aşağıdaki ekran görüntüsüne bakın.

    ![Visual Studio Team Services - sürüm Bash](./media/container-service-docker-swarm-setup-ci-cd/vsts-release-bash.png)

    Komutu, Docker CLI ve Docker Compose CLI aşağıdaki görevleri gerçekleştirmek için ana kullanımda yürütülebilir:

    - Azure kapsayıcı kayıt defterine oturum açma (tanımlanan üç yapı variab'les kullanan **değişkenleri** sekmesinde)
    - Tanımlamak **DOCKER_HOST** Swarm uç noktalarına ile çalışmak için değişken (: 2375)
    - Gidin *dağıtmak* önceki güvenli kopyalama görevi tarafından oluşturulan ve docker-compose.yml dosyası içeren klasör 
    - Yürütme `docker-compose` yeni görüntüleri pull komutlarını hizmetleri durdurun, olan hizmetleri kaldırın ve kapsayıcıları oluşturma.

    >[!IMPORTANT]
    > Önceki ekranda gösterildiği gibi bırakın **STDERR üzerinde başarısız** onay kutusu işaretli. Bunun nedeni bir önemli ayarı, `docker-compose` durdurma veya standart hata çıktı siliniyor kapsayıcılardır gibi birkaç tanılama iletilerini yazdırır. Onay kutusunu işaretlerseniz, tüm aşsa bile iyi Visual Studio Team Services hataları yayın sırasında oluştuğunu bildiriyor.
    >
3. Bu yeni sürüm tanımını kaydedin.


>[!NOTE]
>Biz eski hizmetleri durdurma ve yeni bir çalışan olduğundan bu dağıtım miktar kapalı kalma süresi içerir. Mavi yeşil dağıtım yaparak bu durumu önlemek mümkündür.
>

## <a name="step-4-test-the-cicd-pipeline"></a>4. Adım. CI/CD ardışık düzen test

Yapılandırma ile yapılır, bu yeni CI/CD ardışık düzen test zamanı geldi. Test etmek için kolay kaynak kodunu güncelleştirin ve GitHub deponuz içine değişiklikleri yoludur. Birkaç saniye kod itme sonra Visual Studio Team Services içinde çalışan yeni bir derleme görürsünüz. Başarıyla tamamlandığında, yeni bir sürüm tetiklenir ve Azure kapsayıcı hizmeti kümesi uygulamanın yeni sürümünü dağıtır.

## <a name="next-steps"></a>Sonraki Adımlar

* CI/CD Visual Studio Team Services ile ilgili daha fazla bilgi için bkz: [VSTS derleme genel bakış](https://www.visualstudio.com/docs/build/overview).
