---
title: Azure App Service için sürekli dağıtım | Microsoft Docs
description: Azure Uygulama Hizmeti’ne sürekli dağıtımı etkinleştirme hakkında bilgi edinin.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: cephalin;dariagrigoriu
ms.openlocfilehash: d83d1ad74d04356f73f18a744c2d1509b5efc280
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233853"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Azure App Service için sürekli dağıtım
Bu makalede için sürekli dağıtımının nasıl yapılandırılacağı gösterilmektedir [Azure App Service](app-service-web-overview.md). Uygulama hizmeti, BitBucket, GitHub, sürekli dağıtımını sağlar ve [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) varolan havuzunuzdan Bu hizmetlerden biri de en son güncelleştirmeleri çekme tarafından.

El ile Azure portal tarafından listelenmeyen bir bulut deposu sürekli dağıtımını yapılandırmak nasıl öğrenmek için (gibi [GitLab](https://gitlab.com/)), bkz: [el ile adımları kullanarak sürekli dağıtım ayarı](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

Hazırlanan deponuz desteklenen hizmetlerden biri için yayımlayın. Projenizi bu hizmetlerde yayımlama hakkında daha fazla bilgi için bkz. [Depo oluşturma (GitHub)], [Depo oluşturma (BitBucket)] ve [VSTS kullanmaya başlama].

## <a name="deploy-continuously-from-github"></a>Sürekli olarak Github'dan dağıtma

Uygulama hizmeti uygulaması sayfanıza gitmek GitHub ile sürekli dağıtımını etkinleştirmek için [Azure portal](https://portal.azure.com).

Soldaki menüde tıklatın **Dağıtım Merkezi** > **GitHub** > **Authorize**. Yetkilendirme istemleri izleyin. 

![](media/app-service-continuous-deployment/github-choose-source.png)

Yalnızca bir kez GitHub ile yetkilendirmek gerekir. Yetki tıklatmanız **devam**. Yetkili GitHub hesabı tıklatarak değiştirebilirsiniz **hesabını değiştir**.

![](media/app-service-continuous-deployment/github-continue.png)

İçinde **derleme sağlayıcısı** sayfasında, yapı sağlayıcısını seçin ve tıklayın > **devam**.

### <a name="option-1-use-app-service-kudu-build-server"></a>Seçenek 1: App Service Kudu yapı sunucusu kullan

İçinde **yapılandırma** sayfasında, istediğiniz sürekli olarak dağıtmak kuruluş, depo ve şube seçin. Tamamlandığında tıklatarak **devam**.

### <a name="option-2-use-vsts-continuous-delivery"></a>Seçenek 2: VSTS kesintisiz teslim kullanın

> [!NOTE]
> App Service'nın gerekli yapı oluşturmak ve tanımları VSTS hesabınızda yayın rolü, Azure hesabınızın olması gerekir **sahibi** Azure aboneliğinizde.
>

İçinde **yapılandırma** sayfasında **kod** bölümünde, istediğiniz sürekli olarak dağıtmak kuruluş, depo ve şube seçin. Tamamlandığında tıklatarak **devam**.

İçinde **yapılandırma** sayfasında **yapı** bölümünde, yeni bir VSTS hesabı yapılandırın veya var olan bir hesap belirtin. Tamamlandığında tıklatarak **devam**.

> [!NOTE]
> Listede olmayan VSTS hesabınız kullanmak istiyorsanız, gerek [Azure aboneliğinize VSTS hesabı bağlamak](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

İçinde **Test** sayfasında, yük testleri etkinleştirin ve ardından isteyip istemediğinizi seçin **devam**.

Bağlı olarak [fiyatlandırma katmanı](/pricing/details/app-service/plans/) de görebilirsiniz, uygulama hizmeti planını bir **hazırlama Dağıt** sayfası. Seçin kullanılıp kullanılmayacağını [dağıtım yuvasını etkinleştirmeniz](web-sites-staged-publishing.md), ardından **devam**.

### <a name="finish-configuration"></a>Yapılandırmayı Bitir

İçinde **Özet** sayfasında, seçeneklerinizi doğrulayın ve tıklayın **son**.

Yapılandırma tamamlandıktan sonra yeni işlemeleri seçili deposunda uygulama hizmeti uygulamanızı sürekli olarak dağıtılır.

![](media/app-service-continuous-deployment/github-finished.png)

## <a name="deploy-continuously-from-bitbucket"></a>Bitbucket'tan sürekli olarak dağıtma

Uygulama hizmeti uygulaması sayfanıza gitmek BitBucket ile sürekli dağıtımını etkinleştirmek için [Azure portal](https://portal.azure.com).

Soldaki menüde tıklatın **Dağıtım Merkezi** > **BitBucket** > **Authorize**. Yetkilendirme istemleri izleyin. 

![](media/app-service-continuous-deployment/bitbucket-choose-source.png)

Yalnızca bir kez BitBucket ile yetkilendirmek gerekir. Yetki tıklatmanız **devam**. Yetkili BitBucket hesap tıklatarak değiştirebilirsiniz **hesabını değiştir**.

![](media/app-service-continuous-deployment/bitbucket-continue.png)

İçinde **yapılandırma** sayfasında, istediğiniz sürekli olarak dağıtmak depo ve dalı seçin. Tamamlandığında tıklatarak **devam**.

İçinde **Özet** sayfasında, seçeneklerinizi doğrulayın ve tıklayın **son**.

Yapılandırma tamamlandıktan sonra yeni işlemeleri seçili deposunda uygulama hizmeti uygulamanızı sürekli olarak dağıtılır.

## <a name="deploy-continuously-from-vsts"></a>VSTS sürekli olarak dağıtma

VSTS ile sürekli dağıtımını etkinleştirmek için uygulama hizmeti uygulaması sayfanıza gidin [Azure portal](https://portal.azure.com).

Soldaki menüde tıklatın **Dağıtım Merkezi** > **VSTS** > **devam**. 

![](media/app-service-continuous-deployment/vsts-choose-source.png)

İçinde **derleme sağlayıcısı** sayfasında, yapı sağlayıcısını seçin ve tıklayın > **devam**.

### <a name="option-1-use-app-service-kudu-build-server"></a>Seçenek 1: App Service Kudu yapı sunucusu kullan

İçinde **yapılandırma** sayfasında, VSTS hesap, proje, depo ve kendisinden sürekli olarak dağıtmak istediğiniz şube seçin. Tamamlandığında tıklatarak **devam**.

### <a name="option-2-use-vsts-continuous-delivery"></a>Seçenek 2: VSTS kesintisiz teslim kullanın

> [!NOTE]
> App Service'nın gerekli yapı oluşturmak ve tanımları VSTS hesabınızda yayın rolü, Azure hesabınızın olması gerekir **sahibi** Azure aboneliğinizde.
>

İçinde **yapılandırma** sayfasında **kod** bölümünde, VSTS hesap, proje, depo ve kendisinden sürekli olarak dağıtmak istediğiniz şube seçin. Tamamlandığında tıklatarak **devam**.

> [!NOTE]
> Listede olmayan VSTS hesabınız kullanmak istiyorsanız, gerek [Azure aboneliğinize VSTS hesabı bağlamak](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

İçinde **yapılandırma** sayfasında **yapı** bölümünde, VSTS seçili deponuz derleme görevleri çalıştırmak için kullanması gereken dili çerçevesini belirtin. Tamamlandığında tıklatarak **devam**.

İçinde **Test** sayfasında, yük testleri etkinleştirin ve ardından isteyip istemediğinizi seçin **devam**.

Bağlı olarak [fiyatlandırma katmanı](/pricing/details/app-service/plans/) de görebilirsiniz, uygulama hizmeti planını bir **hazırlama Dağıt** sayfası. Seçin kullanılıp kullanılmayacağını [dağıtım yuvasını etkinleştirmeniz](web-sites-staged-publishing.md), ardından **devam**. 

### <a name="finish-configuration"></a>Yapılandırmayı Bitir

İçinde **Özet** sayfasında, seçeneklerinizi doğrulayın ve tıklayın **son**.

Yapılandırma tamamlandıktan sonra yeni işlemeleri seçili deposunda uygulama hizmeti uygulamanızı sürekli olarak dağıtılır.

## <a name="disable-continuous-deployment"></a>Sürekli dağıtım devre dışı bırak

Uygulama hizmeti uygulaması sayfanıza gitmek sürekli dağıtımı devre dışı bırakmak için [Azure portal](https://portal.azure.com).

Soldaki menüde tıklatın **Dağıtım Merkezi** > **GitHub** veya **VSTS** veya **BitBucket**  >  **Bağlantısını**.

![](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="additional-resources"></a>Ek Kaynaklar

* [Sürekli dağıtımla ilgili yaygın sorunları araştırma](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Azure için PowerShell'i kullanma]
* [Git belgeleri]
* [Kudu Projesi](https://github.com/projectkudu/kudu/wiki)
* [Otomatik olarak ASP.NET 4 uygulama dağıtmak için bir CI/CD ardışık düzen oluşturmak için Azure kullanın](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

[Azure portal]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure için PowerShell'i kullanma]: /powershell/azureps-cmdlets-docs
[Git Belgeleri]: http://git-scm.com/documentation

[Depo oluşturma (GitHub)]: https://help.github.com/articles/create-a-repo
[Depo oluşturma (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[VSTS kullanmaya başlama]: https://www.visualstudio.com/docs/vsts-tfs-overview
