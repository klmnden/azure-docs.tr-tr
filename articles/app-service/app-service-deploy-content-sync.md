---
title: "Azure App Service'e bir bulut klasöründen eşitleme içerik"
description: "Uygulamanızı Azure App Service'e içerik eşitleme bulut klasöründen dağıtmayı öğrenin."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 88d3a670-303a-4fa2-9de9-715cc904acec
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 010e7dc492abefaa3afe814c0322af9f6fe5acd2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Azure App Service'e bir bulut klasöründen eşitleme içerik
Bu öğreticide, dağıtma gösterilir [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) içeriğinizi Dropbox ve OneDrive gibi popüler bulut depolama hizmetlerinden eşitleniyor tarafından. 

## <a name="overview"></a>İçerik eşitleme dağıtımına genel bakış
İsteğe bağlı içerik eşitleme dağıtım tarafından sağlanmıştır [Kudu dağıtım altyapısı](https://github.com/projectkudu/kudu/wiki) App Service ile tümleşiktir. İçinde [Azure Portal](https://portal.azure.com), bulut depolama alanınızda bir klasör belirleyin, çalışma uygulama kodu ve bu klasördeki içerik ve App Service'e bir düğmeyi tıklatarak ile eşitleme. İçerik eşitleme derleme ve dağıtım için Kudu işlemi kullanır. 

## <a name="contentsync"></a>İçerik eşitleme dağıtımı etkinleştirme
İçerik eşitlemenin etkinleştirmek için [Azure Portal](https://portal.azure.com), şu adımları izleyin:

1. Azure Portal'da uygulamanızın dikey penceresinde tıklayın **ayarları** > **dağıtım kaynağı**. Tıklatın **Kaynağı Seç**seçeneğini belirleyip **OneDrive** veya **Dropbox** dağıtım kaynağı olarak. 
   
    ![İçerik eşitleme](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > API'lerde temeldeki farklar nedeniyle **OneDrive iş** şu anda desteklenmiyor. 
   > 
   > 
2. OneDrive veya Dropbox tüm uygulama hizmeti içeriğinize depolanacağı için belirli bir ön tanımlı belirtilen yola erişmek uygulama hizmeti etkinleştirmek için yetkilendirme iş akışı tamamlayın.  
    Uygulama hizmeti yetkilendirme sonra platform atanmış içerik yolu altında içerik bir klasör oluşturun veya bu belirtilen içerik yolu altında varolan bir içerik klasörü seçmek için seçeneğini verir. Uygulama hizmeti eşitleme için kullanılan bulut depolama hesaplarınızı altında belirtilen içerik yolları şunlardır:  
   
   * **OneDrive**:`Apps\Azure Web Apps` 
   * **Dropbox**:`Dropbox\Apps\Azure`
3. İlk içerik eşitlemeden sonra Azure portalından isteğe bağlı içerik eşitleme başlatılabilir. Dağıtım geçmişi ile kullanılabilir **dağıtımları** dikey.
   
    ![Dağıtım geçmişi](./media/app-service-deploy-content-sync/onedrive_sync.png)

Daha fazla bilgi için Dropbox dağıtım altında kullanılabilir [Dropbox gelen dağıtma](http://blogs.msdn.com/b/windowsazure/archive/2013/03/19/new-deploy-to-windows-azure-web-sites-from-dropbox.aspx). 

