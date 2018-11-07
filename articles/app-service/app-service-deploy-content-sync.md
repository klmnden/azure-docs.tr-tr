---
title: Azure App Service'e bir bulut klasöründen eşitleme içeriği
description: Uygulamanızı Azure App Service'e içerik eşitleme aracılığıyla bulut klasöründen dağıtmayı öğrenin.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: cephalin;dariagrigoriu
ms.openlocfilehash: 3796f5c8956b633a4789baaf31a439746dc96b96
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51233771"
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Azure App Service'e bir bulut klasöründen eşitleme içeriği
Bu makalede, içeriğinize eşitleme işlemini göstermektedir [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) Dropbox ve OneDrive. 

İsteğe bağlı içerik eşitleme dağıtımı, App Service tarafından desteklenen [Kudu dağıtım altyapısı](https://github.com/projectkudu/kudu/wiki). Uygulama kodu ve belirlenen bulut klasöründeki içerik çalışmak ve sonra App Service için bir düğmeye tıklayarak ile eşitleme olabilir. İçerik eşitleme Kudu derleme sunucusu kullanır. 

## <a name="enable-content-sync-deployment"></a>İçerik eşitleme dağıtımı etkinleştir

İçerik eşitleme etkinleştirmek için App Service uygulama sayfasına gidebilirsiniz [Azure portalında](https://portal.azure.com).

Sol menüde **Dağıtım Merkezi** > **OneDrive** veya **Dropbox** > **Authorize**. Yetkilendirme yönergeleri izleyin. 

![](media/app-service-deploy-content-sync/choose-source.png)

OneDrive veya Dropbox ile bir kez yetkilendirmek yeterlidir. Zaten sahip olduğunuz, tıklamanız **devam**. Yetkili OneDrive veya Dropbox hesabı tıklayarak değiştirebilirsiniz **hesabını değiştir**.

![](media/app-service-deploy-content-sync/continue.png)

İçinde **yapılandırma** sayfasında, eşitlemek istediğiniz klasörü seçin. Bu klasör, OneDrive veya Dropbox aşağıdaki belirtilen içerik yolu altında oluşturulur. 
   
* **OneDrive**: `Apps\Azure Web Apps`
* **Dropbox**: `Apps\Azure`

İşiniz bittiğinde tıklayın **devam**.

İçinde **özeti** sayfasında, seçeneklerinizi doğrulayın ve tıklayın **son**.

## <a name="synchronize-content"></a>İçerik eşitleme

App Service ile bulut klasördeki içeriği eşitlemek istediğiniz zaman geri dönüp **Dağıtım Merkezi** sayfasında ve tıklayın **eşitleme**.

![](media/app-service-deploy-content-sync/synchronize.png)
   
   > [!NOTE]
   > API'lerde temeldeki farklar nedeniyle **OneDrive iş** şu anda desteklenmiyor. 
   > 
   > 

## <a name="disable-content-sync-deployment"></a>İçerik eşitleme dağıtımı devre dışı bırak

İçerik eşitleme devre dışı bırakmak için App Service uygulama sayfasına gidebilirsiniz [Azure portalında](https://portal.azure.com).

Sol menüde **Dağıtım Merkezi** > **OneDrive** veya **Dropbox** > **Bağlantıyı Kes**.

![](media/app-service-deploy-content-sync/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerel Git deposundan dağıtın](app-service-deploy-local-git.md)
