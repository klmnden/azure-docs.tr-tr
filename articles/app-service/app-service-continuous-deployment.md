---
title: Azure uygulama Hizmeti'ne sürekli dağıtım | Microsoft Docs
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
ms.openlocfilehash: 4d3f1c66c6403720bf02c80af1d6833dc3cee3f1
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2018
ms.locfileid: "42055597"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Azure uygulama Hizmeti'ne sürekli dağıtım
Bu makale için sürekli dağıtım yapılandırma işlemi gösterilmektedir [Azure App Service](app-service-web-overview.md). App Service; BitBucket, GitHub, sürekli dağıtımı sağlar ve [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) Bu hizmetlerden biri olarak mevcut deponuzdaki en son güncelleştirmeleri çekerek.

El ile Azure portal tarafından listelenmeyen bir bulut deposunda sürekli dağıtımını yapılandırmak nasıl kaydolacağınızı (gibi [GitLab](https://gitlab.com/)), bkz: [el ile adımları kullanarak sürekli dağıtım ayarlama](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

Hazırlanan deponuzu desteklenen hizmetlerden biri için yayımlayın. Projenizi bu hizmetlerde yayımlama hakkında daha fazla bilgi için bkz. [Depo oluşturma (GitHub)], [Depo oluşturma (BitBucket)] ve [VSTS kullanmaya başlama].

## <a name="deploy-continuously-from-github"></a>Github'dan sürekli dağıtım

GitHub ile sürekli dağıtımı etkinleştirmek için App Service uygulama sayfasına gidebilirsiniz [Azure portalında](https://portal.azure.com).

Sol menüde **Dağıtım Merkezi** > **GitHub** > **Authorize**. Yetkilendirme yönergeleri izleyin. 

![](media/app-service-continuous-deployment/github-choose-source.png)

GitHub ile bir kez yetkilendirmek yeterlidir. Zaten sahip olduğunuz, tıklamanız **devam**. Yetkili bir GitHub hesabı tıklayarak değiştirebilirsiniz **hesabını değiştir**.

![](media/app-service-continuous-deployment/github-continue.png)

İçinde **oluşturma sağlayıcısını** sayfasında derleme sağlayıcıyı seçin ve tıklayın > **devam**.

### <a name="option-1-use-app-service-kudu-build-server"></a>1. seçenek: App Service Kudu derleme sunucusu kullanma

İçinde **yapılandırma** sayfasında, sürekli olarak dağıtmak istediğiniz kuruluş, depoyu ve dalı seçin. İşiniz bittiğinde tıklayın **devam**.

### <a name="option-2-use-vsts-continuous-delivery"></a>2. seçenek: VSTS sürekli teslim kullanma

> [!NOTE]
> App Service'nın gerekli derleme ve yayın tanımları VSTS hesabınızdaki rolü Azure hesabınızın olması gerekir **sahibi** Azure aboneliğinizdeki.
>

İçinde **yapılandırma** sayfasında **kod** bölümünde, sürekli olarak dağıtmak istediğiniz kuruluş, depoyu ve dalı seçin. İşiniz bittiğinde tıklayın **devam**.

İçinde **yapılandırma** sayfasında **derleme** bölümünde, yeni bir VSTS hesabı yapılandırın veya mevcut bir hesabı belirtin. İşiniz bittiğinde tıklayın **devam**.

> [!NOTE]
> Listede olmayan mevcut bir VSTS hesabı kullanmak istiyorsanız, yapmanız [VSTS hesabı Azure aboneliğinize bağlayın](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

İçinde **Test** sayfasında, yük testleri etkinleştirin ve ardından yüklememeyi **devam**.

Yapılandırmanıza bağlı olarak [fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/plans/) de görebilirsiniz, App Service planı, bir **hazırlama Dağıt** sayfası. Seçin kullanılıp kullanılmayacağını [dağıtım yuvasını etkinleştirmeniz](web-sites-staged-publishing.md), ardından **devam**.

### <a name="finish-configuration"></a>Son yapılandırma

İçinde **özeti** sayfasında, seçeneklerinizi doğrulayın ve tıklayın **son**.

Yapılandırma tamamlandığında, seçili depo yeni işlemeler App Service uygulamanızı sürekli olarak dağıtılır.

![](media/app-service-continuous-deployment/github-finished.png)

## <a name="deploy-continuously-from-bitbucket"></a>Bitbucket'tan sürekli dağıtım

BitBucket ile sürekli dağıtımı etkinleştirmek için App Service uygulama sayfasına gidebilirsiniz [Azure portalında](https://portal.azure.com).

Sol menüde **Dağıtım Merkezi** > **BitBucket** > **Authorize**. Yetkilendirme yönergeleri izleyin. 

![](media/app-service-continuous-deployment/bitbucket-choose-source.png)

BitBucket ile bir kez yetkilendirmek yeterlidir. Zaten sahip olduğunuz, tıklamanız **devam**. BitBucket Anlaşmazlık tıklayarak değiştirebilirsiniz **hesabını değiştir**.

![](media/app-service-continuous-deployment/bitbucket-continue.png)

İçinde **yapılandırma** sayfasında, sürekli olarak dağıtmak istediğiniz depoyu ve dalı seçin. İşiniz bittiğinde tıklayın **devam**.

İçinde **özeti** sayfasında, seçeneklerinizi doğrulayın ve tıklayın **son**.

Yapılandırma tamamlandığında, seçili depo yeni işlemeler App Service uygulamanızı sürekli olarak dağıtılır.

## <a name="deploy-continuously-from-vsts"></a>VSTS ile sürekli dağıtım

VSTS ile sürekli dağıtımı etkinleştirmek için App Service uygulama sayfasına gidebilirsiniz [Azure portalında](https://portal.azure.com).

Sol menüde **Dağıtım Merkezi** > **VSTS** > **devam**. 

![](media/app-service-continuous-deployment/vsts-choose-source.png)

İçinde **oluşturma sağlayıcısını** sayfasında derleme sağlayıcıyı seçin ve tıklayın > **devam**.

### <a name="option-1-use-app-service-kudu-build-server"></a>1. seçenek: App Service Kudu derleme sunucusu kullanma

İçinde **yapılandırma** sayfasında, VSTS hesabı, proje, depo ve sürekli dağıtım istediğiniz dalı seçin. İşiniz bittiğinde tıklayın **devam**.

### <a name="option-2-use-vsts-continuous-delivery"></a>2. seçenek: VSTS sürekli teslim kullanma

> [!NOTE]
> App Service'nın gerekli derleme ve yayın tanımları VSTS hesabınızdaki rolü Azure hesabınızın olması gerekir **sahibi** Azure aboneliğinizdeki.
>

İçinde **yapılandırma** sayfasında **kod** bölümünde, VSTS hesabı, proje, depo ve sürekli dağıtım istediğiniz dalı seçin. İşiniz bittiğinde tıklayın **devam**.

> [!NOTE]
> Listede olmayan mevcut bir VSTS hesabı kullanmak istiyorsanız, yapmanız [VSTS hesabı Azure aboneliğinize bağlayın](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

İçinde **yapılandırma** sayfasında **derleme** bölümünde, VSTS, seçili depo için derleme görevleri çalıştırmak için kullanacağı dil çerçevesi belirtin. İşiniz bittiğinde tıklayın **devam**.

İçinde **Test** sayfasında, yük testleri etkinleştirin ve ardından yüklememeyi **devam**.

Yapılandırmanıza bağlı olarak [fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/plans/) de görebilirsiniz, App Service planı, bir **hazırlama Dağıt** sayfası. Seçin kullanılıp kullanılmayacağını [dağıtım yuvasını etkinleştirmeniz](web-sites-staged-publishing.md), ardından **devam**. 

### <a name="finish-configuration"></a>Son yapılandırma

İçinde **özeti** sayfasında, seçeneklerinizi doğrulayın ve tıklayın **son**.

Yapılandırma tamamlandığında, seçili depo yeni işlemeler App Service uygulamanızı sürekli olarak dağıtılır.

## <a name="disable-continuous-deployment"></a>Sürekli dağıtımı devre dışı bırakma

Sürekli dağıtımı devre dışı bırakmak için App Service uygulama sayfasına gidebilirsiniz [Azure portalında](https://portal.azure.com).

Sol menüde **Dağıtım Merkezi** > **GitHub** veya **VSTS** veya **BitBucket**  >  **Kesin**.

![](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="additional-resources"></a>Ek Kaynaklar

* [Sürekli dağıtımla ilgili yaygın sorunları araştırma](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Azure için PowerShell'i kullanma]
* [Git belgeleri]
* [Kudu Projesi](https://github.com/projectkudu/kudu/wiki)
* [Otomatik olarak bir ASP.NET 4 uygulamasını dağıtmak için bir CI/CD işlem hattı oluşturmak için Azure'u kullanın](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

[Azure portal]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure için PowerShell'i kullanma]: /powershell/azureps-cmdlets-docs
[Git Belgeleri]: http://git-scm.com/documentation

[Depo oluşturma (GitHub)]: https://help.github.com/articles/create-a-repo
[Depo oluşturma (BitBucket)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[VSTS kullanmaya başlama]: https://www.visualstudio.com/docs/vsts-tfs-overview
