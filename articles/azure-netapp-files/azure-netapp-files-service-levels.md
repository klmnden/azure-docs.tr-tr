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
ms.topic: concepts
ms.date: 02/14/2019
ms.author: b-juche
ms.openlocfilehash: a10ac319fe362011531e00f832bb8e471fd56fdf
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430902"
---
# <a name="service-levels-for-azure-netapp-files"></a>Azure NetApp dosyaları için hizmet düzeyleri
Azure NetApp dosyaları iki hizmet düzeylerini destekler: Premium ve standart. 

## <a name="Premium"></a>Premium depolama

*Premium* depolama TiB üretilen iş başına en fazla 64 MiB/sn sağlar. Aktarım hızı performansı, birim kotaya dizine alınır. Örneğin, aktarım hızı 128 MiB/sn (gerçek kullanım) bağımsız olarak sağlanan kotasının 2 TiB Premium depolama biriminden vardır.

## <a name="Standard"></a>Standart depolama

*Standart* depolama TiB üretilen iş başına en fazla 16 MiB/sn sağlar. Aktarım hızı performansı, birim kotaya dizine alınır. Örneğin, aktarım hızı 32 MiB/sn (gerçek kullanım) bağımsız olarak sağlanan kotasının 2 TiB ile standart depolama biriminden vardır.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [Azure NetApp fiyatlandırma sayfasına dosyaları](https://azure.microsoft.com/pricing/details/storage/netapp/) fiyatı, farklı hizmet düzeyleri için
- [Kapasitesi havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
