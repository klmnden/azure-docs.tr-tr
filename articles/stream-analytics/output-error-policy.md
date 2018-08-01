---
title: Azure Stream analytics'te çıkış hatası ilkeleri
description: Azure Stream Analytics'te kullanılabilir ilkeleri işleme çıkış hatası hakkında bilgi edinin.
services: stream-analytics
author: sidram
ms.author: sidram
manager: jeanb
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 07/27/2018
ms.openlocfilehash: f854e8ce35ac9d0a99b4a7dc1cdc66724114a5c4
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39391742"
---
# <a name="output-error-policy"></a>Çıkış hata İlkesi
Bu makalede, Azure Stream Analytics'te yapılandırılabilir ilkeleri işleme çıkış veri hatası açıklanır.

Çıkış verilerine ilişkin hata işleme çıkış olayı, bir Stream Analytics işi tarafından üretilen kullanırken oluşan veri dönüştürme hataları ilkeleri uygulamak için hedef havuz şemaya uymuyor. Bu ilke ya da seçerek yapılandırabileceğiniz **yeniden deneyin** veya **bırak**. Azure portalında bir Stream Analytics işinde altında çalışırken **yapılandırma**seçin **hata İlkesi** seçiminizi yapma.

![Hata ilkesi - konum](./media/stream-analytics-error-policy/stream-analytics-error-policy-locate.PNG)


## <a name="retry"></a>Yeniden Dene
Ne zaman bir hata, yazma süresiz olarak başarılı olana kadar olayı yazmayı Azure Stream Analytics yeniden denemeler gerçekleşir. İçin yeniden deneme zaman aşımı yoktur. Sonunda tüm sonraki olayların işlenmesini deniyor olay tarafından engellenir. İlke işleme varsayılan çıkış hatası seçenektir.

## <a name="drop"></a>Kaldır
Azure Stream Analytics veri dönüştürme hatası sonuçları herhangi bir çıkış olayı kaldıracağız. Bırakılan olayları, daha sonra yeniden işlemeyerek için kurtarılamıyor.


Çıkış hatası işleme ilkesi yapılandırması bağımsız olarak tüm geçici hatalar (örneğin, ağ hataları) denenir.


## <a name="next-steps"></a>Sonraki adımlar
[Azure Stream Analytics için sorun giderme kılavuzu](stream-analytics-troubleshooting-guide.md)
