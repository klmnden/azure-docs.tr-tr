---
title: Azure zaman serisi görüşleri ortamınıza saklamayı yapılandırmak nasıl | Microsoft Docs
description: Bu makalede, Azure zaman serisi görüşleri ortamınıza saklamayı yapılandırmak açıklar.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/09/2018
ms.custom: seodec18
ms.openlocfilehash: d88bf309d2afeb4e6fdd09ff1317bec3fec2fe75
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64695844"
---
# <a name="configuring-retention-in-time-series-insights"></a>Zaman serisi Öngörülerinde saklama yapılandırma
Bu makalede nasıl yapılandırılacağını açıklar **veri saklama zamanı** ve **depolama sınırı aştı davranışı** Azure zaman serisi görüşleri'nde.

Her zaman serisi öngörüleri (TSI) ortamı yapılandırmak için bir ayarı vardır **veri saklama zamanı**. Değer 1'den 400 gün olarak yayılır. Veriler ortamı depolama kapasitesi veya bekletme süresine (1-400) göre silinir, hangisinin önce geldiğine.

Her TSI ortam ek ayarının **depolama sınırı aştı davranışı**. Bu ayar, bir ortamın maksimum kapasite üst sınırına ulaşıldığında giriş ve temizleme davranışını denetler. Aralarından seçim yapabileceğiniz iki davranışları vardır:
- **Eski veri temizleme** (varsayılan)  
- **Duraklatma giriş**

Bu ayarları daha iyi anlamak ayrıntılı bilgi için gözden [anlama bekletme zaman serisi görüşleri'nde](time-series-insights-concepts-retention.md).  

## <a name="configure-data-retention"></a>Veri saklamayı yapılandırma

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Mevcut Time Series Insights ortamınızı bulun. Seçin **tüm kaynakları** Azure portalının sol tarafındaki menüde. Zaman Serisi Görüşleri ortamınızı seçin.

3. Altında **ayarları** başlığı seçin **yapılandırma**.

4. Seçin **veri saklama zamanı** kaydırıcı çubuğunu kullanarak bekletme yapılandırma veya metin kutusuna bir sayı yazın.

5. Not **kapasite** ayarı olduğundan, bu yapılandırma, Maksimum sayıda veri olaylarını ve veri depolama için toplam depolama kapasitesi etkiler. 

6. İki durumlu **depolama sınırı aştı davranışı** ayarı. Seçin **eski veri temizleme** veya **duraklatma giriş** davranışı.

7. Seçin **Kaydet** değişiklikleri yapılandırmak için.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için gözden [anlama bekletme zaman serisi görüşleri'nde](time-series-insights-concepts-retention.md).