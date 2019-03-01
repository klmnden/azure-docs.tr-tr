---
title: Gözcü Azure önizlemede Azure etkinlik veri toplama | Microsoft Docs
description: Gözcü Azure, Azure etkinlik verilerini nasıl toplayacağınızı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 8c25baa8-b93b-41da-9e6c-15bb7b5c5511
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 068da0c196cba8095aca24a572eb4dfaec685c7a
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2019
ms.locfileid: "56992946"
---
# <a name="collect-data-from-azure-activity-log"></a>Azure etkinlik günlüğü'nden veri toplama

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Günlüklerinden akışını [Azure etkinlik günlüğü](../azure-monitor/platform/activity-logs-overview.md) Azure Gözcü tek bir tıklamayla içine. Etkinlik günlüğü oluştu Azure'da abonelik düzeyindeki olayların bir anlayış sağlayan bir abonelik günlüktür. Bu verileri, hizmet durumu olayları güncelleştirmelerinin Azure Resource Manager işletimsel verileri içerir. Etkinlik günlüğü'nü kullanarak belirleyebilirsiniz ' ne, kim ve ne zaman ' işlemi (PUT, POST, DELETE), aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen herhangi bir yazma için. Ayrıca, işlemi ve ilgili diğer özellikleri durumunu anlayabilirsiniz. Etkinlik günlüğünü okuma (GET) işlemlerini ya da kullanan Klasik kaynakları işlemlerinde içerip içermediğini / "RDFE" modeli. 


## <a name="prerequisites"></a>Önkoşullar

- Genel yönetici veya güvenlik yöneticisi izinleri ile kullanıcı


## <a name="connect-to-azure-activity-log"></a>Azure etkinlik günlüğü için Bağlan

1. Azure Gözcü içinde seçin **veri toplama** ve ardından **Azure etkinlik günlüğü** Döşe.

2. Azure etkinlik günlüğü bölmesinde Azure Gözcü akışını sağlamak istediğiniz abonelikleri seçin. 

3. **Bağlan**'a tıklayın.

4. İlgili şema Log Analytics'te Azure etkinlik uyarılarını kullanmak için arama **AzureActivity**.


 

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gözcü için Azure etkinlik günlüğü bağlantısını yapmayı öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- Bilgi nasıl [görünürlük almak, veri ve olası tehditleri](quickstart-get-visibility.md).
- Başlama [Azure Gözcü kullanarak tehditleri algılama](tutorial-detect-threats.md).
