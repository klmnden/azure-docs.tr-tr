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
ms.date: 06/18/2019
ms.author: cephalin;dariagrigoriu
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: 3a207f5513c24f2c33571df929f7490ec7f2eba5
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836783"
---
# <a name="continuous-deployment-to-azure-app-service"></a>Azure uygulama Hizmeti'ne sürekli dağıtım

[Azure App Service](overview.md) sürekli dağıtımını github, BitBucket, sağlar ve [Azure depoları](https://azure.microsoft.com/services/devops/repos/) en son güncelleştirmeleri çekerek depolar. Bu makalede, uygulamanızı Kudu derleme hizmeti aracılığıyla sürekli olarak dağıtmak için Azure portalını kullanmayı gösterir veya [Azure işlem hatları](https://azure.microsoft.com/services/devops/pipelines/). 

Kaynak denetimi hizmetleri hakkında daha fazla bilgi için bkz. [depo oluşturma (GitHub)], [depo oluşturma (BitBucket)], veya [yeni Git deposu (Azure depoları) oluşturma].

Portal doğrudan, gibi desteklemeyen bir bulut deposundan sürekli dağıtımı el ile yapılandırmak için [GitLab](https://gitlab.com/), bkz: [el ile adımları kullanarak sürekli dağıtım ayarlama](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

## <a name="authorize-azure-app-service"></a>Azure App Service'ı Yetkilendir 

Azure depoları kullanmak için Azure DevOps kuruluşunuz Azure aboneliğinize bağlı olduğundan emin olun. Daha fazla bilgi için [uygulamayı bir web uygulamasına dağıtmak için bir Azure DevOps hizmetleri hesabı ayarlamanız](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

Bitbucket veya GitHub deponuza bağlamak için Azure App Service yetkilendirin. Yalnızca bir kez bir kaynak denetimi hizmetiyle yetkilendirmek gerekir. 

1. Seçin **uygulama hizmetleri** içinde [Azure portalında](https://portal.azure.com) sol gezinti ve ardından dağıtmak istediğiniz web uygulamasını seçin. 
   
1. Uygulama sayfasında **Dağıtım Merkezi** soldaki menüde.
   
1. Üzerinde **Dağıtım Merkezi** sayfasında **GitHub** veya **Bitbucket**ve ardından **Authorize**. 
   
   ![Kaynak denetimi hizmetini seçin, ardından Yetkilendir'e seçin.](media/app-service-continuous-deployment/github-choose-source.png)
   
1. Hizmete gerekirse oturum açın ve yetkilendirme yönergeleri izleyin. 

## <a name="enable-continuous-deployment"></a>Sürekli dağıtımı etkinleştirme 

Kaynak denetimi hizmetini yetkilendirmek sonra yerleşik sürekli dağıtım için uygulamanızı yapılandırma [App Service Kudu derleme sunucusu](#option-1-use-the-app-service-build-service), aracılığıyla veya [Azure işlem hatları](#option-2-use-azure-pipelines). 

### <a name="option-1-use-the-app-service-build-service"></a>1\. seçenek: App Service derleme hizmetini kullanın

App Service Kudu derleme sunucunuzu sürekli olarak dağıtmak için GitHub, Bitbucket veya Azure depoları yerleşik kullanabilirsiniz. 

1. Seçin **uygulama hizmetleri** içinde [Azure portalında](https://portal.azure.com) sol gezinti ve ardından dağıtmak istediğiniz web uygulamasını seçin. 
   
1. Uygulama sayfasında **Dağıtım Merkezi** soldaki menüde.
   
1. Yetkili kaynak denetimi sağlayıcınız seçin **Dağıtım Merkezi** sayfasında ve seçin **devam**. GitHub veya Bitbucket için de seçebilirsiniz **hesabını değiştir** yetkili hesabı değiştirmek için. 
   
   > [!NOTE]
   > Azure depoları kullanmak için Azure DevOps Hizmetleri kuruluşunuz Azure aboneliğinize bağlı olduğundan emin olun. Daha fazla bilgi için [uygulamayı bir web uygulamasına dağıtmak için bir Azure DevOps hizmetleri hesabı ayarlamanız](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
   
1. GitHub veya Azure depoları, üzerinde **oluşturma sağlayıcısını** sayfasında **App Service derleme hizmeti**ve ardından **devam**. Bitbucket, her zaman App Service derleme hizmeti kullanır.
   
   ![App Service derleme hizmeti seçin ve devam'ı seçin.](media/app-service-continuous-deployment/choose-kudu.png)
   
1. Üzerinde **yapılandırma** sayfası:
   
   - GitHub'ı için açılır listesine tıklayıp **kuruluş**, **depo**, ve **dal** sürekli olarak dağıtmak istediğiniz.
     
     > [!NOTE]
     > Tüm depolar görmüyorsanız, github'da Azure App Service'ı yetkilendirme gerekebilir. GitHub deponuza göz atıp gidin **ayarları** > **uygulamaları** > **OAuth uygulamalar yetkili**. Seçin **Azure App Service**ve ardından **Grant**.
     
   - Bitbucket için Bitbucket seçin **takım**, **depo**, ve **dal** sürekli olarak dağıtmak istediğiniz.
     
   - Azure depolarda seçin **Azure DevOps kuruluş**, **proje**, **depo**, ve **dal** sürekli olarak dağıtmak istediğiniz.
     
     > [!NOTE]
     > Azure DevOps kuruluşunuz listede yoksa, Azure aboneliğinize bağlı olduğundan emin olun. Daha fazla bilgi için [uygulamayı bir web uygulamasına dağıtmak için bir Azure DevOps hizmetleri hesabı ayarlamanız](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App)...
     
1. Seçin **devam**.
   
   ![Depo bilgileri doldurun ve ardından Devam'ı seçin.](media/app-service-continuous-deployment/configure-kudu.png)
   
1. Derleme sağlayıcı yapılandırdıktan sonra ayarları gözden **özeti** sayfasında ve ardından **son**.
   
   Seçili depo ve dal yeni işlemeler artık App Service uygulamanızı sürekli olarak dağıtın. Üzerinde işlemeler ve dağıtımları izleyebilirsiniz **Dağıtım Merkezi** sayfası.
   
   ![İşlemeler ve dağıtım merkezi dağıtımda izleyin](media/app-service-continuous-deployment/github-finished.png)

### <a name="option-2-use-azure-pipelines"></a>2\. seçenek: Azure Pipelines'ı kullanın 

Hesabınızın gerekli izinlere sahip değilse, GitHub veya Azure depoları depolarından sürekli olarak dağıtmak için Azure işlem hatlarını ayarlayabilirsiniz. Azure işlem hatları dağıtma hakkında daha fazla bilgi için bkz. [Azure uygulama hizmetleri için bir web uygulaması dağıtma](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps).

Sürekli teslim oluşturmak Azure App Service için Azure, Azure DevOps kuruluşunuzda işlem hatları: 

- Azure hesabınız, Azure Active Directory'ye yazma ve hizmet oluşturmak için izinleri olmalıdır. 
  
- Azure hesabınızın olması gerekir **sahibi** Azure aboneliğinizdeki rol.

- Kullanmak istediğiniz Azure DevOps projesi içinde bir yönetici olması gerekir.

Azure işlem hatları (Önizleme) yapılandırmak için:

1. Seçin **uygulama hizmetleri** içinde [Azure portalında](https://portal.azure.com) sol gezinti ve ardından dağıtmak istediğiniz web uygulamasını seçin. 
   
1. Uygulama sayfasında **Dağıtım Merkezi** soldaki menüde.
   
1. Üzerinde **oluşturma sağlayıcısını** sayfasında **Azure işlem hatları (Önizleme)** ve ardından **devam**. 
   
1. Üzerinde **yapılandırma** sayfasında **kod** bölümü:
   
   - GitHub'ı için açılır listesine tıklayıp **kuruluş**, **depo**, ve **dal** sürekli olarak dağıtmak istediğiniz.
     
     > [!NOTE]
     > Tüm depolar görmüyorsanız, github'da Azure App Service'ı yetkilendirme gerekebilir. GitHub deponuza göz atıp gidin **ayarları** > **uygulamaları** > **OAuth uygulamalar yetkili**. Seçin **Azure App Service**ve ardından **Grant**.
     
   - Azure depolarda seçin **Azure DevOps kuruluş**, **proje**, **depo**, ve **dal** sürekli olarak dağıtmak istediğiniz veya yeni bir Azure DevOps kuruluş yapılandırın.
     
     > [!NOTE]
     > Mevcut Azure DevOps kuruluşunuz listede yoksa, Azure aboneliğinize bağlayın gerekebilir. Daha fazla bilgi için [CD yayın işlem hattınızı tanımlamak](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps#cd).
     
1. Seçin **devam**.
   
1. Azure depoları için de **derleme** bölümünde, Azure işlem hatları derleme görevleri çalıştırmak için kullanın ve ardından dil framework belirtin **devam**.
   
1. Üzerinde **Test** sayfasında, yük testleri etkinleştirin ve ardından isteyip istemediğinizi seçin **devam**.
   
1. App Service planınıza bağlı olarak [fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/app-service/plans/), görebileceğiniz bir **hazırlama Dağıt** sayfası. Seçin kullanılıp kullanılmayacağını [dağıtım yuvasını etkinleştirmeniz](deploy-staging-slots.md)ve ardından **devam**.
   
   > [!NOTE]
   > Azure işlem hatları üretim yuvasına sürekli teslim izin yoktur. Bu kısıtlama, yanlışlıkla dağıtımı üretime engeller. Hazırlama yuvasına sürekli teslimat ayarlayın, değişiklikleri doğrulayın ve hazır olduğunuzda sonra Yuvalar.
   
1. Derleme sağlayıcı yapılandırdıktan sonra ayarları gözden **özeti** sayfasında ve ardından **son**.
   
   Seçili depo ve dal yeni işlemeler artık App Service uygulamanızı sürekli olarak dağıtın. Üzerinde işlemeler ve dağıtımları izleyebilirsiniz **Dağıtım Merkezi** sayfası.
   
   ![İşlemeler ve dağıtım merkezi dağıtımda izleyin](media/app-service-continuous-deployment/github-finished.png)

## <a name="disable-continuous-deployment"></a>Sürekli dağıtımı devre dışı bırakma

Sürekli dağıtımı devre dışı bırakmak için seçin **Bağlantıyı Kes** uygulamanızın üst kısmındaki **Dağıtım Merkezi** sayfası.

![Sürekli dağıtımı devre dışı bırakma](media/app-service-continuous-deployment/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="additional-resources"></a>Ek kaynaklar

* [Sürekli dağıtım ile ilgili yaygın sorunları araştırma](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Azure PowerShell’i kullanma](/powershell/azureps-cmdlets-docs)
* [Git belgeleri](https://git-scm.com/documentation)
* [Kudu Projesi](https://github.com/projectkudu/kudu/wiki)

[Depo oluşturma (GitHub)]: https://help.github.com/articles/create-a-repo
[Depo oluşturma (BitBucket)]: https://confluence.atlassian.com/get-started-with-bitbucket/create-a-repository-861178559.html
[Yeni Git deposu (Azure depoları) oluşturma]: /azure/devops/repos/git/creatingrepo
