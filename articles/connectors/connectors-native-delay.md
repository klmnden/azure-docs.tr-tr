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
ms.openlocfilehash: 6cde5b8ba8d770a07199816286b666e952394de1
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-delay-and-delay-until-actions"></a>Gecikme ve gecikme kullanmaya başlama-Eylemler kadar
Gecikme kullanarak ve "gecikme-kadar" Eylemler, iş akışı senaryoları tamamlayabilirsiniz.

Örneğin, şunları yapabilirsiniz:

* Bir durum güncelleştirmesi e-posta göndermek için bir hafta kadar bekleyin.
* Bir HTTP çağrısıyla sonucu alma ve sürdürme önce tamamlanması zaman olana kadar iş akışı gecikme.

Bir mantıksal uygulama gecikme eylem kullanmaya başlamak için bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

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
| Sayısı * |sayı |Gecikme süresi birim sayısı |
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
Şimdi, platform deneyin ve [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Logic apps diğer kullanılabilir bağlayıcılar bakarak keşfedebilirsiniz bizim [API'leri listesi](apis-list.md).

