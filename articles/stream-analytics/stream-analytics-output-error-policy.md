---
title: Azure Stream analytics'te çıkış hatası ilkeleri
description: Azure Stream Analytics'te kullanılabilir ilkeleri işleme çıkış hatası hakkında bilgi edinin.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: b31530966d2c5ca9a3f82f3e74ba349e66053a83
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61478932"
---
# <a name="azure-stream-analytics-output-error-policy"></a>Azure Stream Analytics çıkış hata İlkesi
Bu makalede, Azure Stream Analytics'te yapılandırılabilir ilkeleri işleme çıkış veri hatası açıklanır.

Çıkış verilerine ilişkin hata işleme çıkış olayı, bir Stream Analytics işi tarafından üretilen kullanırken oluşan veri dönüştürme hataları ilkeleri uygulamak için hedef havuz şemaya uymuyor. Bu ilke ya da seçerek yapılandırabileceğiniz **yeniden deneyin** veya **bırak**. Azure portalında bir Stream Analytics işinde altında çalışırken **yapılandırma**seçin **hata İlkesi** seçiminizi yapma.

![Azure Stream Analytics çıkış hata ilkesi konumu](./media/stream-analytics-output-error-policy/stream-analytics-error-policy-locate.png)


## <a name="retry"></a>Yeniden Dene
Ne zaman bir hata, yazma süresiz olarak başarılı olana kadar olayı yazmayı Azure Stream Analytics yeniden denemeler gerçekleşir. İçin yeniden deneme zaman aşımı yoktur. Sonunda tüm sonraki olayların işlenmesini deniyor olay tarafından engellenir. İlke işleme varsayılan çıkış hatası seçenektir.

## <a name="drop"></a>Kaldır
Azure Stream Analytics veri dönüştürme hatası sonuçları herhangi bir çıkış olayı kaldıracağız. Bırakılan olayları, daha sonra yeniden işlemeyerek için kurtarılamıyor.


Çıkış hatası işleme ilkesi yapılandırması bağımsız olarak tüm geçici hatalar (örneğin, ağ hataları) denenir.


## <a name="next-steps"></a>Sonraki adımlar
[Azure Stream Analytics için sorun giderme kılavuzu](stream-analytics-troubleshooting-guide.md)
