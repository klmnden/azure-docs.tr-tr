---
title: Eşitleme içerikleri bir bulut klasöründen - Azure App Service
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
ms.date: 12/03/2018
ms.author: cephalin;dariagrigoriu
ms.custom: seodec18
ms.openlocfilehash: 65f372196671e95f7d9af7f47011e9ca1f9de316
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60769659"
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

Sol menüde **Dağıtım Merkezi** > **Bağlantıyı Kes**.

![](media/app-service-deploy-content-sync/disable.png)

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerel Git deposundan dağıtın](deploy-local-git.md)
