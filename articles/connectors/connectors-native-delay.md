---
title: "Logic apps içinde bir gecikme ekleme | Microsoft Docs"
description: "Gecikme ve gecikme genel bakış-Eylemler ve bunların Azure mantıksal uygulama ile nasıl kullanılacağını kadar."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 915f48bf-3bd8-4656-be73-91a941d0afcd
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: 5f4f7052d48b4ca4ed91212d970551141e78e852
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-the-delay-and-delay-until-actions"></a>Gecikme ve gecikme kullanmaya başlama-Eylemler kadar
Gecikme kullanarak ve "gecikme-kadar" Eylemler, iş akışı senaryoları tamamlayabilirsiniz.

Örneğin, şunları yapabilirsiniz:

* Bir durum güncelleştirmesi e-posta göndermek için bir hafta kadar bekleyin.
* Bir HTTP çağrısıyla sonucu alma ve sürdürme önce tamamlanması zaman olana kadar iş akışı gecikme.

Bir mantıksal uygulama gecikme eylem kullanmaya başlamak için bkz: [mantıksal uygulama oluşturma](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-the-delay-actions"></a>Gecikme eylemlerini kullanın
Bir eylem, bir mantıksal uygulama içinde tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](connectors-overview.md).

Bir mantıksal uygulama gecikme adımını kullanmak nasıl bir örnek sırası şöyledir:

1. Bir tetikleyici ekledikten sonra tıklatın **yeni adım** bir eylem eklemek için.
2. Arama **gecikme** gecikme eylemleri getirmek için. Bu örnekte, biz seçecektir **gecikme**.
   
    ![Gecikme Eylemler](./media/connectors-native-delay/using-action-1.png)
3. Gecikme yapılandırmak için eylem özelliklerinden herhangi birini tamamlayın.
   
    ![Gecikme yapılandırma](./media/connectors-native-delay/using-action-2.png)
4. Tıklatın **kaydetmek** yayımlama ve mantıksal uygulama etkinleştirin.

## <a name="action-details"></a>Eylem ayrıntıları
Yineleme tetikleyici yapılandırılabilir aşağıdaki özelliklere sahiptir.

### <a name="delay-action"></a>Gecikme eylemi
Bu eylem çalıştırmak için belirli bir zaman aralığı geciktirir.
A * gerekli bir alan olduğu anlamına gelir.

| Görünen ad | Özellik adı | Açıklama |
| --- | --- | --- |
| Sayısı * |Sayısı |Gecikme süresi birim sayısı |
| Birim * |Birim |Zaman birimi: `Second`, `Minute`, `Hour`, veya`Day` |

<br>

### <a name="delay-until-action"></a>Gecikme-eylem kadar
Bu eylem Çalıştır belirtilen bir tarih/saat kadar geciktirir.
A * gerekli bir alan olduğu anlamına gelir.

| Görünen ad | Özellik adı | Açıklama |
| --- | --- | --- |
| Yıl * |timestamp |(GMT) kadar gecikme yıl |
| Ay * |timestamp |(GMT) kadar gecikme ay |
| Gün * |timestamp |(GMT) kadar gecikme gün |

<br>

## <a name="next-steps"></a>Sonraki adımlar
Şimdi, platform deneyin ve [mantıksal uygulama oluşturma](../logic-apps/logic-apps-create-a-logic-app.md). Logic apps diğer kullanılabilir bağlayıcılar bakarak keşfedebilirsiniz bizim [API'leri listesi](apis-list.md).

