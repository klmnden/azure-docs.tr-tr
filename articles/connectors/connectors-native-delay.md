---
title: Logic apps'te bir gecikme Ekle | Microsoft Docs
description: Gecikmeli ve Gecikmeli genel bakış-Eylemler ve bunların Azure mantıksal uygulaması ile nasıl kullanılacağını kadar.
services: ''
documentationcenter: ''
author: jeffhollan
manager: erikre
editor: ''
tags: connectors
ms.assetid: 915f48bf-3bd8-4656-be73-91a941d0afcd
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: 15e581454b60319ab734f2fa5faf0d90e0a7c8bf
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58893733"
---
# <a name="get-started-with-the-delay-and-delay-until-actions"></a>Gecikmeli ve Gecikmeli kullanmaya başlama-Eylemler kadar
Gecikme süresini kullanarak ve "gecikme-kadar" Eylemler, iş akışı senaryoları tamamlayabilirsiniz.

Örneğin, şunları yapabilirsiniz:

* Durum güncelleştirmesi e-posta göndermek için bir hafta kadar bekleyin.
* İş akışı, HTTP çağrısı sonucu alma ve sürdürme önce tamamlanması zaman sahip oluncaya kadar gecikme.

Bir mantıksal uygulamada gecikme eylemi kullanmaya başlamak için bkz: [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="use-the-delay-actions"></a>Gecikmeli eylemleri kullanın

Bir eylem mantıksal uygulamada tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. 
[Eylemler hakkında daha fazla bilgi](../connectors/apis-list.md).

Gecikme adım bir mantıksal uygulamada kullanma konusunda bir örnek sırası şöyledir:

1. Bir tetikleyici ekledikten sonra tıklayın **yeni adım** eylem ekleme.
2. Arama **gecikme** gecikme eylemleri getirilecek. Bu örnekte, seçeceğiz **gecikme**.
   
    ![Gecikmeli eylemleri](./media/connectors-native-delay/using-action-1.png)
3. Gecikmesini yapılandırmak için eylem özelliklerinden herhangi birini tamamlayın.
   
    ![Gecikme yapılandırma](./media/connectors-native-delay/using-action-2.png)
4. Tıklayın **Kaydet** yayımlama ve mantıksal uygulama etkinleştirin.

## <a name="action-details"></a>Eylem ayrıntıları
Yinelenme tetikleyicisini yapılandırılabilen aşağıdaki özelliklere sahiptir.

### <a name="delay-action"></a>Gecikme eylemi
Bu eylem çalıştırmak için belirli bir süre erteler.
A * gerekli alan olduğu anlamına gelir.

| Görünen ad | Özellik adı | Açıklama |
| --- | --- | --- |
| Sayısı * |count |Gecikme zaman birimlerinin sayısı |
| Unit* |birim |Zaman birimi: `Second`, `Minute`, `Hour`, veya `Day` |

<br>

### <a name="delay-until-action"></a>Gecikme-eylem kadar
Bu eylem, belirtilen bir tarih/saat kadar çalıştırma geciktirir.
A * gerekli alan olduğu anlamına gelir.

| Görünen ad | Özellik adı | Açıklama |
| --- | --- | --- |
| Yıl * |timestamp |(GMT) kadar gecikme yıl |
| Month* |timestamp |(GMT) kadar gecikme ay |
| Gün * |timestamp |(GMT) kadar gecikme bir gün |

<br>

## <a name="next-steps"></a>Sonraki adımlar
Şimdi, platformu deneyin ve [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). Diğer bağlayıcıları logic apps'teki bakarak keşfedebilirsiniz bizim [API listesi](apis-list.md).

