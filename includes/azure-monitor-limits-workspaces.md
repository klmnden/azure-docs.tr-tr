---
title: include dosyası
description: include dosyası
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 02/07/2019
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 21e2d3f75028d239175effa7a3608cc18ccfc95c
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305344"
---
**Veri koleksiyonu hacmi ve saklama** 

| Katman | Gün başına sınırla | Veri saklama | Yorum |
|:---|:---|:---|:---|
| Geçerli GB başına fiyatlandırma katmanı<br>(Nisan 2018 sunulmuştur) | Sınırsız | 30 - 730 gün | 31 gün sonra veri saklama ek ücretleri için kullanılabilir. Azure İzleyici fiyatlandırması hakkında daha fazla bilgi edinin. |
| Eski ücretsiz katmanı<br>(Nisan 2016'da tanıtılan) | 500 MB | 7 gün | Çalışma başına günlük sınır 500 MB'ın ulaştığında, veri alımı durur ve sonraki günün başlangıcında devam eder. Bir gün için UTC temel alınır. Azure Güvenlik Merkezi tarafından toplanan verileri olmadığına dikkat edin, bu günlük sınır 500 MB dahil ve toplanan sınırınızın üstünde olmaya devam edecektir.  |
| Eski tek başına GB başına katmanı<br>(Nisan 2016'da tanıtılan) | Sınırsız | 30 ile 730 gün | 31 gün sonra veri saklama ek ücretleri için kullanılabilir. Azure İzleyici fiyatlandırması hakkında daha fazla bilgi edinin. |
| Düğüm başına (OMS) eski<br>(Nisan 2016'da tanıtılan) | Sınırsız | 30 ile 730 gün | 31 gün sonra veri saklama ek ücretleri için kullanılabilir. Azure İzleyici fiyatlandırması hakkında daha fazla bilgi edinin. |
| Eski standart katman | Sınırsız | 30 gün  | Bekletme değiştirilemez |
| Eski Premium katmanı | Sınırsız | 365 gün  | Bekletme değiştirilemez |

**Abonelik başına çalışma alanı sayısı.**

| Fiyatlandırma katmanı    | Çalışma alanı sınırı | Açıklamalar
|:---|:---|:---|
| Ücretsiz katmanı  | 10 | Bu sınır yükseltilemez. |
| Tüm Katmanlar | Sınırsız | Bir kaynak grubundaki kaynak sayısı ve abonelik başına kaynak gruplarının sayısını sınırlama getirilir. |

**Azure portal**

| Kategori | Limits | Açıklamalar |
|:---|:---|:---|
| Günlük sorgu tarafından döndürülen en fazla kayıt | 10,000 | Sorguyu Sorgu kapsamını, zaman aralığı ve filtreleri kullanarak sonuçları azaltır. |


**Veri Toplayıcı API'si**

| Kategori | Limits | Açıklamalar |
|:---|:---|:---|
| Tek bir gönderi için boyut üst sınırı | 30 MB | Daha büyük birimleri birden fazla gönderiye bölme. |
| Alan değerleri için boyut üst sınırı  | 32 KB | 32 KB'den daha uzun alanlar kesilir. |

**Arama API'si**

| Kategori | Limits | Açıklamalar |
|:---|:---|:---|
| Toplu olmayan veriler için döndürülen en fazla kayıt | 5,000 | |
| Toplu veriler için en fazla kayıt | 500,000 | Toplu veriler içeren bir arama olduğundan `summarize` komutu. |
| Döndürülen veriler en büyük boyutu | 64,000,000 bayt (~ 61 MiB)| |
| Maksimum sorgu çalıştırma süresi | 10 dakika | Bkz: [zaman aşımları](https://dev.loganalytics.io/documentation/Using-the-API/Timeouts) Ayrıntılar için.  |
| İstek hızı üst sınırı | AAD kullanıcı veya istemci IP adresi başına 30 saniye başına 200 istek | Bkz: [oran sınırları](https://dev.loganalytics.io/documentation/Using-the-API/Limits) Ayrıntılar için. |

**Genel çalışma alanı sınırları**

| Kategori | Limits | Açıklamalar |
|:---|:---|:---|
| Bir tablodaki en fazla sütun         | 500 | |
| Sütun adı için en fazla karakter | 500 | |
| Kapasite bölgeler | Batı Orta ABD | Geçici kapasite sınırında olduğundan şu anda bu bölgede yeni bir çalışma alanı oluşturulamıyor. Bu sınır, Eylül 2019 sonunda ilgilenilmesi yayılması planlanmaktadır. |
| Verileri dışarı aktarma | şu anda kullanılamıyor | Azure işlev veya mantıksal uygulama, toplama ve veri dışarı aktarmak için kullanın. | 

>[!NOTE]
>Ne kadar süreyle Log Analytics kullanmakta olduğunuz bağlı olarak, eski fiyatlandırma katmanları erişiminiz olması. Daha fazla bilgi edinin [Log Analytics eski fiyatlandırma katmanları](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#legacy-pricing-tiers). 