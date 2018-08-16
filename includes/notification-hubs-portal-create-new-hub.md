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
ms.openlocfilehash: 19ea9c749b58f6f81dc2087caa77573062d883b5
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39486003"
---
1. [Azure Portal](https://portal.azure.com) oturum açın.

1. **Kaynak oluştur** > **Mobil** > **Bildirim Hub'ı** seçeneğini belirleyin.
   
      ![Azure portalı - Bildirim hub'ı oluşturma](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)
      
1. **Bildirim Hub'ı** kutusuna benzersiz bir ad yazın. **Bölge**, **Abonelik** ve **Kaynak Grubu** (zaten varsa) seçimi yapın. 
   
      Hizmet veri yolu ad alanınız yoksa hub adına göre oluşturulan (ad alanı adı varsa) varsayılan adı kullanabilirsiniz.
    
      Hub'ı oluşturmak istediğiniz bir hizmet veri yolu ad alanı varsa aşağıdaki adımları izleyin

    a. **Ad Alanı** alanında **Var Olanı Seç** bağlantısını seçin. 
   
    b. **Oluştur**’u seçin.
   
      ![Azure portalı - Bildirim hub'ı özelliklerini ayarlama](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

1. **Bildirimler**’i (Zil simgesi) ve **Kaynağa git**’i seçin. 

      ![Azure portalı - bildirimler -> Kaynağa git](./media/notification-hubs-portal-create-new-hub/notification-go-to-resource.png)    
1. Listeden **Erişim İlkeleri**'ni seçin. Verilen iki bağlantı dizesini not edin. Bu dizelere daha sonra anında iletme bildirimleri için ihtiyaç duyacaksınız.

      >[!IMPORTANT]
      >Uygulamanızda DefaultFullSharedAccessSignature **KULLANMAYIN**. Bunu yalnızca arka ucunuzda kullanmanız gerekir.
      >
   
      ![Azure portalı - Bildirim hub'ı bağlantı dizeleri](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)

