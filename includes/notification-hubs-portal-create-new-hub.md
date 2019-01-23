---
title: include dosyası
description: include dosyası
services: notification-hubs
author: jwargo
ms.service: notification-hubs
ms.topic: include
ms.date: 01/17/2019
ms.author: jowargo
ms.custom: include file
ms.openlocfilehash: b68fa345d4772134c30ce8b8b559f98113a0496f
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54453106"
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
