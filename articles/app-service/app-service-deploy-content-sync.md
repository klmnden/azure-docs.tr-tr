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
ms.date: 06/14/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 8e132e4d4a65588d57e3cfb969e785f5a164206c
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="sync-content-from-a-cloud-folder-to-azure-app-service"></a>Azure App Service'e bir bulut klasöründen eşitleme içerik
Bu öğreticide, içeriğinize eşitlemenin nasıl gösterilir [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Dropbox ve OneDrive gibi popüler bulut depolama hizmetlerinden. 

## <a name="overview"></a>İçerik eşitleme dağıtımına genel bakış
İsteğe bağlı içerik eşitleme dağıtım tarafından sağlanmıştır [Kudu dağıtım altyapısı](https://github.com/projectkudu/kudu/wiki) App Service ile tümleşiktir. İçinde [Azure portal](https://portal.azure.com), bulut depolama alanınızda bir klasör belirleyin, çalışma uygulama kodu ve bu klasördeki içerik ve App Service'e bir düğmeyi tıklatarak ile eşitleme. İçerik eşitleme derleme ve dağıtım için Kudu işlemi kullanır. 

## <a name="contentsync"></a>İçerik eşitleme dağıtımı etkinleştirme
İçerik eşitlemenin etkinleştirmek için [Azure portal](https://portal.azure.com), şu adımları izleyin:

1. Azure portalında, uygulamanızın sayfasında tıklatın **ayarları** > **dağıtım kaynağı**. Tıklatın **Kaynağı Seç**seçeneğini belirleyip **OneDrive** veya **Dropbox** dağıtım kaynağı olarak. 
   
    ![İçerik eşitleme](./media/app-service-deploy-content-sync/deployment_source.png)
   
   > [!NOTE]
   > API'lerde temeldeki farklar nedeniyle **OneDrive iş** şu anda desteklenmiyor. 
   > 
   > 
2. Tam OneDrive veya Dropbox, burada tüm uygulama hizmetiniz içerik için belirli bir ön tanımlı belirtilen yola erişmek uygulama hizmeti etkinleştirmek için yetkilendirme iş akışı depolanır. Yetkilendirme sonrasında, App Service platformu atanmış içerik yolu altında içerik bir klasör oluşturun veya bu belirtilen içerik yolu altında varolan bir içerik klasörü seçmek için seçeneği sunar. Uygulama hizmeti eşitleme için kullanılan bulut depolama hesaplarınızı altında belirtilen içerik yolları şunlardır:  
   
   * **OneDrive**: `Apps\Azure Web Apps` 
   * **Dropbox**: `Dropbox\Apps\Azure`
3. İlk içerik eşitlemeden sonra Azure portalından isteğe bağlı içerik eşitleme başlatılabilir. Dağıtım geçmişi edinilebilir **dağıtımları** sayfası.
   
    ![Dağıtım geçmişi](./media/app-service-deploy-content-sync/onedrive_sync.png)

[Dağıtma] Dropbox gelen altında daha fazla bilgi Dropbox dağıtım için kullanılabilir (https://azure.microsoft.com/en-in/blog/new-deploy-to-windows-azure-web-sites-from-dropbox/).

