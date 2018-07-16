---
title: include dosyası
description: include dosyası
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 03/28/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 22c1d24042072de5d57d41da379a5fad18180de7
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972527"
---
1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Kaynak oluştur** > **Mobil** > **Bildirim Hub'ı** seçeneğini belirleyin.
   
      ![Azure portalı - Bildirim hub'ı oluşturma](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)
      
3. **Bildirim Hub'ı** kutusuna benzersiz bir ad yazın. **Bölge**, **Abonelik** ve **Kaynak Grubu** (zaten varsa) seçimi yapın. 
   
      Hizmet veri yolu ad alanınız yoksa hub adına göre oluşturulan (ad alanı adı varsa) varsayılan adı kullanabilirsiniz.
    
      Hub'ı oluşturmak istediğiniz bir hizmet veri yolu ad alanı varsa aşağıdaki adımları izleyin

    a. **Ad Alanı** alanında **Var Olanı Seç** bağlantısını seçin. 
   
    b. **Oluştur**’u seçin.
   
      ![Azure portalı - Bildirim hub'ı özelliklerini ayarlama](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

4. **Bildirimler**’i (Zil simgesi) ve **Kaynağa git**’i seçin. 

      ![Azure portalı - bildirimler -> Kaynağa git](./media/notification-hubs-portal-create-new-hub/notification-go-to-resource.png)    
5. Listeden **Erişim İlkeleri**'ni seçin. Verilen iki bağlantı dizesini not edin. Bu dizelere daha sonra anında iletme bildirimleri için ihtiyaç duyacaksınız.

      >[!IMPORTANT]
      >Uygulamanızda DefaultFullSharedAccessSignature **KULLANMAYIN**. Bunu yalnızca arka ucunuzda kullanmanız gerekir.
      >
   
      ![Azure portalı - Bildirim hub'ı bağlantı dizeleri](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)

