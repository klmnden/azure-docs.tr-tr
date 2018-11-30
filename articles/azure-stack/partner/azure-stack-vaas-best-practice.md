---
title: Azure Stack doğrulama en iyi yöntemler. | Microsoft Docs
description: Bu makalede, bir hizmet olarak doğrulama için en iyi yöntemleri bulunur.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: John.Haskin
ms.openlocfilehash: d83eb501bbe685890decb3acbb8116164f4199db
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422669"
---
# <a name="best-practices-for-validation-as-a-service"></a>Hizmet olarak doğrulama için en iyi uygulamalar

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Bu makalede, doğrulama (VaaS) hizmet olarak kaynakları yönetmek için en iyi uygulamaları kapsar. VaaS kaynaklarına genel bakış için bkz. [doğrulama hizmeti temel kavramları olarak](azure-stack-vaas-key-concepts.md).

## <a name="solution-management"></a>Çözüm Yönetimi

### <a name="naming-convention-for-vaas-solutions"></a>VaaS çözümleri için adlandırma kuralı

VaaS içinde kayıtlı olan tüm çözümleri için tutarlı bir adlandırma kuralını kullanın. Örneğin, çözüm adının donanım özelliklerinden şu şekilde oluşturulabilir:

|Ürün Adı | Benzersiz donanım öğesi 1 | Benzersiz donanım öğe 2 | Çözüm adı
|---|---|---|---|
Çözümüm XYZ |  Tüm Flash | My anahtar X01 | MySolutionXYZ_AllFlash_MySwitchX01

### <a name="when-to-create-a-new-vaas-solution"></a>Yeni bir VaaS çözüm oluşturma zamanı

Aynı donanımı SKU karşı çalışan iş akışları aynı VaaS çözümü kullanın. Yalnızca donanım SKU bir değişiklik olduğunda yeni bir VaaS çözüm oluşturulmalıdır.

## <a name="workflow-management"></a>İş akışı yönetimi

### <a name="naming-convention-for-vaas-workflows"></a>VaaS iş akışları için adlandırma kuralı

Tutarlı bir adlandırma kuralı tüm VaaS iş akışı çalıştırmaları için kullanın. Örneğin, yapı iş akışı adını derleme özelliklerinden gibi:

|Derleme numarası (yüksek) | Tarih | Çözüm boyutu | İş Akışı Adı
|---|---|---| ---|
1808 | 081518 | 4NODE | 1808_081518_4NODE

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [doğrulama olarak hizmet temel kavramları](azure-stack-vaas-key-concepts.md)
