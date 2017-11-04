---
title: "Azure zaman serisi Öngörüler ortamınızı ölçeklendirme | Microsoft Docs"
description: "Bu öğretici, Azure zaman serisi Öngörüler ortamınızı ölçeklendirme kapsar"
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: ba6bd1ab05bb7e24dd1bc307218e7a772fbde601
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-scale-your-time-series-insights-environment"></a>Zaman serisi Öngörüler ortamınızı ölçeklendirme

Bu öğretici, zaman serisinin Öngörüler ortamınızı ölçeklendirmek nasıl ele alınmaktadır.

> [!NOTE]
> Ölçek büyütme sku türleri arasında izin verilmiyor. S1 Sku olan bir ortamda bir S2 ortamına dönüştürülemiyor.

## <a name="s1-sku-ingress-rates-and-capacities"></a>S1 SKU giriş hızları ve kapasiteleri

| S1 SKU kapasitesi | Giriş oranı | Maksimum depolama kapasitesi
| --- | --- | --- |
| 1 | 1 GB (1 milyon olay) | Aylık 30 GB (30 milyon olay) |
| 10 | 10 GB (10 milyon olay) | Ayda 300 GB (300 milyon olay) |

## <a name="s2-sku-ingress-rates-and-capacities"></a>S2 SKU giriş hızları ve kapasiteleri

| S2 SKU kapasitesi | Giriş oranı | Maksimum depolama kapasitesi
| --- | --- | --- |
| 1 | 10 GB (10 milyon olay) | Ayda 300 GB (300 milyon olay) |
| 10 | 100 GB (100 milyon olay) | Aylık 3 TB (3 milyon olay) |

Kapasiteleri doğrusal olarak, S1 sku kapasitesi 2 ile her ay gün giriş oranı ve 60 GB (60 milyon olayları) başına 2 GB (2 milyon) olayları destekler şekilde ölçeklendirilir.

## <a name="changing-the-capacity-of-your-environment"></a>Ortamınızı kapasitesini değiştirme

1. Azure portalında kapasite değiştirmek istediğiniz ortamı seçin.
1. Ayarlar altında Yapılandır'ı tıklatın.
1. Kapasite kaydırıcıyı giriş ücretlerinizi gereksinimlerini karşılayan kapasitesini ve depolama kapasitesini seçmek için kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* Yeni Kapasite azaltma önlemek yeterli olduğunu doğrulayın. Daha fazla ayrıntı için bkz: *ortamınızı bastırma* bölüm [burada](time-series-insights-diagnose-and-solve-problems.md).