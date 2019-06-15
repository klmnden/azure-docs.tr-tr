---
title: Azure için NetApp dosyaları hizmet düzeyleri | Microsoft Docs
description: Azure NetApp dosyaları hizmet düzeyleri için aktarım hızı performansı açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: b-juche
ms.openlocfilehash: 1f9c427045c9d42f6a11cc4bcc798cfc47a4428c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65523116"
---
# <a name="service-levels-for-azure-netapp-files"></a>Azure NetApp Files için hizmet düzeyleri
Hizmet düzeyleri kapasitesi havuzu özniteliği var. Hizmet düzeyleri tanımlanmış ve bir birimde birime atanan kota göre kapasitesi havuzu için izin verilen en yüksek aktarım tarafından ayrıştırılan.

## <a name="supported-service-levels"></a>Desteklenen hizmet düzeyleri

Azure NetApp dosyaları, üç hizmet düzeylerini destekler: *Ultra yüksek*, *Premium*, ve *standart*. 

* <a name="Ultra"></a>Ultra yüksek depolama

    128 MiB/sn üretilen iş birimi kotasının atanmış 1 TiB başına en fazla Ultra yüksek depolama katmanı sağlar. 

* <a name="Premium"></a>Premium depolama

    Premium depolama katmanını 64 MiB/sn üretilen iş birimi kotasının atanmış 1 TiB başına en fazla sağlar. 

* <a name="Standard"></a>Standart depolama

    16 MiB/sn üretilen iş birimi kotasının atanmış 1 TiB başına en fazla standart depolama katmanı sağlar.

## <a name="throughput-limits"></a>İşleme sınırları

Bir birim için aktarım hızı sınırı aşağıdaki faktörleri birleşimi tarafından belirlenir:
* Hizmet düzeyi ait olduğu birimin kapasitesi havuzu
* Birime atanan kota  

Bu kavram Aşağıdaki diyagramda gösterilmiştir:

![Hizmet düzeyi çizim](../media/azure-netapp-files/azure-netapp-files-service-levels.png)

Yukarıdaki örnek 1'de bir kapasitesi havuzu atanan kota 2 TiB Premium depolama katmanı ile bir birimden 128 MiB/sn aktarım hızı sınırı atanacak (2 TiB * 64 MiB/sn). Bu senaryo, kapasitesi havuzu boyutu veya gerçek hacmi tüketim bağımsız olarak geçerlidir.

Yukarıdaki örnek 2'de bir kapasitesi havuzu atanan kota 100 GiB Premium depolama katmanı ile bir birimden 6.25 MiB/sn aktarım hızı sınırı atanacak (0.09765625 TiB * 64 MiB/sn). Bu senaryo, kapasitesi havuzu boyutu veya gerçek hacmi tüketim bağımsız olarak geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [Azure NetApp fiyatlandırma sayfasına dosyaları](https://azure.microsoft.com/pricing/details/storage/netapp/) fiyatı, farklı hizmet düzeyleri için
- Bkz: [Azure NetApp dosyaları için maliyet modeli](azure-netapp-files-cost-model.md) kapasitesi havuzu kapasite tüketimini hesaplama 
- [Kapasitesi havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)