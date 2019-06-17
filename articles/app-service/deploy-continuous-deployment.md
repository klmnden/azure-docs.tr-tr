---
title: Sürekli dağıtım - Azure App Service | Microsoft Docs
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
ms.date: 12/03/2018
ms.author: cephalin;dariagrigoriu
ms.custom: seodec18
ms.openlocfilehash: 384f709bb32f973efec39518eaa895e25136fe23
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66390645"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Azure uygulama Hizmeti'ne sürekli dağıtım
Bu makale için sürekli dağıtım yapılandırma işlemi gösterilmektedir [Azure App Service](overview.md). App Service; BitBucket, GitHub, sürekli dağıtımı sağlar ve [Azure DevOps Hizmetleri](https://www.visualstudio.com/team-services/) Bu hizmetlerden biri olarak mevcut deponuzdaki en son güncelleştirmeleri çekerek.

El ile Azure portal tarafından listelenmeyen bir bulut deposunda sürekli dağıtımını yapılandırmak nasıl kaydolacağınızı (gibi [GitLab](https://gitlab.com/)), bkz: [el ile adımları kullanarak sürekli dağıtım ayarlama](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

Hazırlanan deponuzu desteklenen hizmetlerden biri için yayımlayın. Projenizi bu hizmetlerde yayımlama hakkında daha fazla bilgi için bkz: [depo oluşturma (GitHub)], [depo oluşturma (BitBucket)], ve [Azure DevOps hizmetleriyle çalışmaya başlama].

## <a name="deploy-continuously-from-github"></a>Github'dan sürekli dağıtım

GitHub ile sürekli dağıtımı etkinleştirmek için App Service uygulama sayfasına gidebilirsiniz [Azure portalında](https://portal.azure.com).

Sol menüde **Dağıtım Merkezi** > **GitHub** > **Authorize**. Yetkilendirme yönergeleri izleyin. 

![](media/app-service-continuous-deployment/github-choose-source.png)

GitHub ile bir kez yetkilendirmek yeterlidir. Zaten sahip olduğunuz, tıklamanız **devam**. Yetkili bir GitHub hesabı tıklayarak değiştirebilirsiniz **hesabını değiştir**.

![](media/app-service-continuous-deployment/github-continue.png)

İçinde **oluşturma sağlayıcısını** sayfasında derleme sağlayıcıyı seçin ve tıklayın > **devam**.

### <a name="option-1-use-app-service-kudu-build-server"></a>1\. seçenek: App Service Kudu derleme sunucusu kullanma

İçinde **yapılandırma** sayfasında, sürekli olarak dağıtmak istediğiniz kuruluş, depoyu ve dalı seçin. İşiniz bittiğinde tıklayın **devam**.

GitHub kuruluşuna bir depodan dağıtmak için Github'da göz atın ve Git **ayarları** > **uygulamaları** > **OAuth yetkili uygulamalar**. "Azure App Service"'ye tıklayın.

![Ayarlar > Uygulamalar > OAuth uygulamalar yetkili > Azure uygulama hizmeti](media/app-service-continuous-deployment/github-settings-navigation.png)

Sonraki sayfada, sağ taraftaki "Verme" düğmesine tıklayarak kuruluşunuzun depolarına App Service erişimi verin.

!["App Service kuruluşun depoları erişim vermek için Grant" tıklayın](media/app-service-continuous-deployment/grant-access.png)

Kuruluşunuz artık "Kuruluş" listesinde göstermelidir **yapılandırma** Dağıtım Merkezi sayfasında.

### <a name="option-2-use-azure-pipelines-preview"></a>2\. seçenek: Azure işlem hatları (Önizleme) kullanma

> [!NOTE]
> App Service'nın Azure DevOps Hizmetleri kuruluşunuzda gerekli Azure işlem hatları oluşturmak rolü Azure hesabınızın olması gerekir **sahibi** Azure aboneliğinizdeki.
>

İçinde **yapılandırma** sayfasında **kod** bölümünde, sürekli olarak dağıtmak istediğiniz kuruluş, depoyu ve dalı seçin. İşiniz bittiğinde tıklayın **devam**.

İçinde **yapılandırma** sayfasında **derleme** bölümünde, yeni bir Azure DevOps Hizmetleri kuruluş yapılandırın veya mevcut bir kuruluşa belirtin. İşiniz bittiğinde tıklayın **devam**.

> [!NOTE]
> Listede olmayan mevcut bir Azure DevOps Hizmetleri kuruluşa kullanmak istiyorsanız, yapmanız [Azure DevOps hizmetler kuruluşundan Azure aboneliğinize bağlayın](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

İçinde **Test** sayfasında, yük testleri etkinleştirin ve ardından yüklememeyi **devam**.

Yapılandırmanıza bağlı olarak [fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/plans/) de görebilirsiniz, App Service planı, bir **hazırlama Dağıt** sayfası. Seçin kullanılıp kullanılmayacağını [dağıtım yuvasını etkinleştirmeniz](deploy-staging-slots.md), ardından **devam**.

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

## <a name="deploy-continuously-from-azure-repos-devops-services"></a>Azure depoları (DevOps Hizmetleri) sürekli dağıtım

İle sürekli dağıtımı etkinleştirmek için [Azure depoları](https://docs.microsoft.com/azure/devops/repos/index), App Service uygulama sayfanıza gidin [Azure portalında](https://portal.azure.com).

Sol menüde **Dağıtım Merkezi** > **Azure depoları** > **devam**. 

![](media/app-service-continuous-deployment/vsts-choose-source.png)

İçinde **oluşturma sağlayıcısını** sayfasında derleme sağlayıcıyı seçin ve tıklayın > **devam**.

> [!NOTE]
> Listede olmayan mevcut bir Azure DevOps Hizmetleri kuruluşa kullanmak istiyorsanız, yapmanız [Azure DevOps hizmetler kuruluşundan Azure aboneliğinize bağlayın](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

### <a name="option-1-use-app-service-kudu-build-server"></a>1\. seçenek: App Service Kudu derleme sunucusu kullanma

İçinde **yapılandırma** sayfasında, Azure DevOps hizmetler kuruluşundan, proje, depo ve dal sürekli olarak dağıtmak istediğiniz seçin. İşiniz bittiğinde tıklayın **devam**.

### <a name="option-2-use-azure-devops-services-continuous-delivery"></a>2\. seçenek: Azure DevOps Hizmetleri sürekli teslim kullanma

> [!NOTE]
> App Service'nın Azure DevOps Hizmetleri kuruluşunuzda gerekli Azure işlem hatları oluşturmak rolü Azure hesabınızın olması gerekir **sahibi** Azure aboneliğinizdeki.
>

İçinde **yapılandırma** sayfasında **kod** bölümünde, Azure DevOps hizmetler kuruluşundan, proje, depo ve dal sürekli olarak dağıtmak istediğiniz seçin. İşiniz bittiğinde tıklayın **devam**.

İçinde **yapılandırma** sayfasında **derleme** bölümünde, Azure DevOps Hizmetleri, seçili depo için derleme görevleri çalıştırmak için kullanacağı dil çerçevesi belirtin. İşiniz bittiğinde tıklayın **devam**.

İçinde **Test** sayfasında, yük testleri etkinleştirin ve ardından yüklememeyi **devam**.

Yapılandırmanıza bağlı olarak [fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/plans/) de görebilirsiniz, App Service planı, bir **hazırlama Dağıt** sayfası. Seçin kullanılıp kullanılmayacağını [dağıtım yuvasını etkinleştirmeniz](deploy-staging-slots.md), ardından **devam**. DevOps, sürekli teslim üretim yuvasına izin vermez. Bu, üretime yanlışlıkla dağıtımı önlemek için tasarım gereğidir. Hazırlama yuvasına sürekli teslimat ayarlayın, değişiklikleri doğrulayın ve hazır olduğunuzda Yuvalar.

### <a name="finish-configuration"></a>Son yapılandırma

İçinde **özeti** sayfasında, seçeneklerinizi doğrulayın ve tıklayın **son**.

Yapılandırma tamamlandığında, seçili depo yeni işlemeler App Service uygulamanızı sürekli olarak dağıtılır.

## <a name="disable-continuous-deployment"></a>Sürekli dağıtımı devre dışı bırakma

Sürekli dağıtımı devre dışı bırakmak için App Service uygulama sayfasına gidebilirsiniz [Azure portalında](https://portal.azure.com).

Sol menüde **Dağıtım Merkezi** > **GitHub** veya **Azure DevOps Hizmetleri** veya **BitBucket**  >  **Kesin**.

![](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="additional-resources"></a>Ek Kaynaklar

* [Sürekli dağıtımla ilgili yaygın sorunları araştırma](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Azure için PowerShell'i kullanma]
* [Git belgeleri]
* [Kudu Projesi](https://github.com/projectkudu/kudu/wiki)
* [Otomatik olarak bir ASP.NET 4 uygulamasını dağıtmak için bir CI/CD işlem hattı oluşturmak için Azure'u kullanın](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

[Azure portal]: https://portal.azure.com
[Azure DevOps portal]: https://azure.microsoft.com/services/devops/
[Installing Git]: https://git-scm.com/book/en/Getting-Started-Installing-Git
[Azure için PowerShell'i kullanma]: /powershell/azureps-cmdlets-docs
[Git Belgeleri]: https://git-scm.com/documentation

[Depo oluşturma (GitHub)]: https://help.github.com/articles/create-a-repo
[Depo oluşturma (BitBucket)]: https://confluence.atlassian.com/get-started-with-bitbucket/create-a-repository-861178559.html
[Azure DevOps hizmetleriyle çalışmaya başlama]: https://docs.microsoft.com/azure/devops/user-guide/devops-alm-overview
