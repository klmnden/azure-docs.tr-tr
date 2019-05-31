---
title: Azure zaman serisi görüşleri ortamınıza saklamayı yapılandırmak nasıl | Microsoft Docs
description: Bu makalede, Azure zaman serisi görüşleri ortamınıza saklamayı yapılandırmak açıklar.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/30/2019
ms.custom: seodec18
ms.openlocfilehash: f5f34983d1818679450249aa40bbb325e743cc57
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66239013"
---
# <a name="configuring-retention-in-time-series-insights"></a>Zaman serisi Öngörülerinde saklama yapılandırma

Bu makalede nasıl yapılandırılacağını açıklar **veri saklama zamanı** ve **depolama sınırı aştı davranışı** Azure zaman serisi görüşleri'nde.

## <a name="summary"></a>Özet

Her zaman serisi öngörüleri (TSI) ortamı yapılandırmak için bir ayarı vardır **veri saklama zamanı**. Değer 1'den 400 gün olarak yayılır. Veriler ortamı depolama kapasitesi veya bekletme süresine (1-400) göre silinir, hangisinin önce geldiğine.

Her TSI ortam ek ayarının **depolama sınırı aştı davranışı**. Bu ayar, bir ortamın maksimum kapasite üst sınırına ulaşıldığında giriş ve temizleme davranışını denetler. Aralarından seçim yapabileceğiniz iki davranışları vardır:

- **Eski veri temizleme** (varsayılan)
- **Duraklatma giriş**

Bu ayarları daha iyi anlamak ayrıntılı bilgi için gözden [anlama bekletme zaman serisi görüşleri'nde](time-series-insights-concepts-retention.md).  

## <a name="configure-data-retention"></a>Veri saklamayı yapılandırma

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Mevcut Time Series Insights ortamınızı bulun. Seçin **tüm kaynakları** Azure portalının sol tarafındaki menüde. Zaman Serisi Görüşleri ortamınızı seçin.

1. Altında **ayarları** başlığı seçin **yapılandırma**.

1. Seçin **veri saklama zamanı** kaydırıcı çubuğunu kullanarak bekletme yapılandırma veya metin kutusuna bir sayı yazın.

1. Not **kapasite** ayarı olduğundan, bu yapılandırma, Maksimum sayıda veri olaylarını ve veri depolama için toplam depolama kapasitesi etkiler.

1. İki durumlu **depolama sınırı aştı davranışı** ayarı. Seçin **eski veri temizleme** veya **duraklatma giriş** davranışı.

1. Seçin **Kaydet** değişiklikleri yapılandırmak için.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için gözden [anlama bekletme zaman serisi görüşleri'nde](time-series-insights-concepts-retention.md).