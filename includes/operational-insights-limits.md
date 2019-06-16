---
title: include dosyası
description: include dosyası
services: log-analytics
author: MGoedtel
ms.service: log-analytics
ms.topic: include
ms.date: 06/10/2019
ms.author: magoedte
ms.custom: include file
ms.openlocfilehash: c5fedc59c80c68fc222693a67664ef60ddd210a9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133513"
---
Nisan 2018'de tanıtılan geçerli tüketim tabanlı fiyatlandırma katmanı, her bir Log Analytics çalışma alanı için aşağıdaki sınırlar geçerlidir:

|     | GB 2018 |
| --- | --- | 
| Gün başına toplanan veri birimi | None |
| Veri saklama süresi | 30 ile 730 gün<sup>1</sup> |

Aşağıdaki sınırlar için her bir Log Analytics çalışma alanı fiyatlandırma katmanları en son eski geçerlidir:

|  | Boş | Tek başına (GB başına) | Düğüm başına (OMS) |
| --- | --- | --- | --- | --- | --- |--- |
| Gün başına toplanan veri birimi |500 MB<sup>2</sup> |None |None |
| Veri saklama süresi |7 gün | 30 ile 730 gün<sup>1</sup> | 30 ile 730 gün<sup>1</sup> |

Aşağıdaki sınırlar için her bir Log Analytics çalışma alanı fiyatlandırma katmanları eski eski geçerlidir:

|  | Standart | Premium | 
| --- | --- | --- | --- | --- | --- |--- |
| Gün başına toplanan veri birimi | None | None | 
| Veri saklama süresi |30 gün | 365 gün |

<sup>1</sup>31 gün sonra veri saklama ek ücretleri için kullanılabilir. Daha fazla bilgi edinin [Azure İzleyici fiyatlandırma](https://azure.microsoft.com/pricing/details/monitor/).

<sup>2</sup>500 MB günlük veri aktarım limiti, veri analizi çalışma alanınızı ulaştığında durur ve sonraki günün başlangıcında devam eder. Bir gün için UTC temel alınır.

>[!NOTE]
>Ne kadar süreyle Log Analytics kullanmakta olduğunuz bağlı olarak, eski fiyatlandırma katmanları erişiminiz olması. Daha fazla bilgi edinin [Log Analytics eski fiyatlandırma katmanları](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#legacy-pricing-tiers). 
>

Abonelik başına (çalışma alanı) Azure Log Analytics kaynaklarına aşağıdaki sınırlar geçerlidir.

| Fiyatlandırma katmanı    | Çalışma alanı başına abonelik sayısı | Açıklamalar
| --- | --- | --- |
| Ücretsiz katmanı  | 10 | Bu sınır yükseltilemez. |
| Ücretsiz dışında tüm katmanları | Yok | Bir kaynak grubundaki kaynak sayısı ve abonelik başına kaynak gruplarının sayısını sınırlama getirilir. | 

Log Analytics API'leri için aşağıdaki sınırlar geçerlidir:

| Kategori | Limits | Açıklamalar
| --- | --- | --- |
| Veri Toplayıcı API’si | Tek bir gönderi için boyut üst sınırı 30 MB ' dir.<br>Alan değerleri için maksimum boyut 32 KB'dir. | Daha büyük birimleri birden fazla gönderiye bölme.<br>32 KB'den daha uzun alanlar kesilir. |
| Arama API’si | Toplu olmayan veriler için 5000 kayıt döndürdü.<br>500\.000 kayıtları toplu veriler için. | Toplu veriler içeren bir arama olduğundan `summarize` komutu.
 
