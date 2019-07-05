---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/20/2019
ms.author: tamram
ms.openlocfilehash: 5ab03b682dd0ed1dc7b198e89c86e7a74c6275cd
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67457581"
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