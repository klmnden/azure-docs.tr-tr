---
title: Azure App Service'e bir bulut klasöründen eşitleme içerik
description: Uygulamanızı Azure App Service'e içerik eşitleme bulut klasöründen dağıtmayı öğrenin.
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
ms.openlocfilehash: d456ae2ffbd3745ef976ad94219a3f998838066b
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850228"
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Azure App Service'e bir bulut klasöründen eşitleme içerik
Bu makalede içeriğinize eşitlemeye gösterilmiştir [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Dropbox ve OneDrive. 

İsteğe bağlı içerik eşitleme dağıtım App Service tarafından desteklenen [Kudu dağıtım altyapısı](https://github.com/projectkudu/kudu/wiki). Belirlenen cloud klasöründeki içerik ve uygulama kodu ile çalışma ve sonra App Service'e düğmeyi tıklatarak, bir eşitleme olabilir. İçerik eşitleme Kudu yapı sunucusu kullanır. 

## <a name="enable-content-sync-deployment"></a>İçerik eşitleme dağıtımı etkinleştir

İçerik eşitlemeyi etkinleştirmek için uygulama hizmeti uygulaması sayfanıza gidin [Azure portal](https://portal.azure.com).

Soldaki menüde tıklatın **Dağıtım Merkezi** > **OneDrive** veya **Dropbox** > **Authorize**. Yetkilendirme istemleri izleyin. 

![](media/app-service-deploy-content-sync/choose-source.png)

Yalnızca bir kez OneDrive veya Dropbox yetkilendirmek gerekir. Yetki tıklatmanız **devam**. Yetkili OneDrive veya Dropbox hesabı tıklatarak değiştirebilirsiniz **hesabını değiştir**.

![](media/app-service-deploy-content-sync/continue.png)

İçinde **yapılandırma** sayfasında, eşitlemek istediğiniz klasörü seçin. Bu klasör, OneDrive veya Dropbox aşağıdaki atanmış içerik yolu altında oluşturulur. 
   
* **OneDrive**: `Apps\Azure Web Apps`
* **Dropbox**: `Apps\Azure`

Tamamlandığında tıklatarak **devam**.

İçinde **Özet** sayfasında, seçeneklerinizi doğrulayın ve tıklayın **son**.

## <a name="synchronize-content"></a>İçerik eşitleyin

İçeriği cloud klasöründeki App Service ile eşitlemek istediğiniz zaman geri dönüp **Dağıtım Merkezi** sayfasında ve tıklayın **eşitleme**.

![](media/app-service-deploy-content-sync/synchronize.png)
   
   > [!NOTE]
   > API'lerde temeldeki farklar nedeniyle **OneDrive iş** şu anda desteklenmiyor. 
   > 
   > 

## <a name="disable-content-sync-deployment"></a>İçerik eşitleme dağıtımı devre dışı bırak

Uygulama hizmeti uygulaması sayfanıza gitmek içerik eşitleme devre dışı bırakmak için [Azure portal](https://portal.azure.com).

Soldaki menüde tıklatın **Dağıtım Merkezi** > **OneDrive** veya **Dropbox** > **Bağlantıyı Kes**.

![](media/app-service-deploy-content-sync/disable.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerel Git deposu dağıtma](app-service-deploy-local-git.md)
