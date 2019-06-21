---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/20/2019
ms.author: tamram
ms.openlocfilehash: 2319a7620b70547d186a4ef5cb96f5ca6684c62c
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305347"
---
| Resource | Hedef |
|----------|---------------|
| Tek bir tablonun en büyük boyutu | 500 TiB |
| Tablo varlığı en büyük boyutu | 1 MiB |
| Tablo varlığı özelliklerinde sayısı | 255 üç Sistem özellikleri içerir: PartitionKey ve RowKey zaman damgası |
| Bir varlıktaki özellik değerlerini maksimum toplam boyutu | 1 MiB |
| Bir varlıktaki tek bir özellik maksimum toplam boyutu | Özellik türüne göre değişir. Daha fazla bilgi için **özellik türleri** içinde [tablo hizmeti veri modelini anlama](/rest/api/storageservices/understanding-the-table-service-data-model). |
| Saklı erişim ilkeleri tablo başına en fazla sayısı | 5 |
| Depolama hesabı başına en fazla istek hızı | 20.000 saniyede bir 1 KiB varlık boyutu varsayan, |
| Hedef performans düzeyleri (1 KiB-varlıklar) tek bir tabloyu bölümü | Saniye başına en fazla 2.000 varlıklar |
