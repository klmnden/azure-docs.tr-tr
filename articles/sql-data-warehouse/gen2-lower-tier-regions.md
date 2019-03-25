---
title: Azure SQL veri ambarı Gen2, alt bilgi işlem katmanını destekler | Microsoft Docs
description: Azure SQL veri ambarı Gen2, artık daha düşük bilgi işlem katmanı destekler
author: julieMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 12/03/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 23617f3a7eca72e549bcf0e44805d21eef6ffa9b
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400311"
---
# <a name="azure-sql-data-warehouse-gen2-support-for-lower-compute-tiers"></a>Alt bilgi işlem katmanı için Azure SQL veri ambarı Gen2 desteği

Microsoft, sorgular çok hızlı Azure SQL veri ambarı Gen2 için daha düşük bilgi işlem katmanı ekleyerek yoğun işleme yeteneğine sahip bir veri ambarı'nı çalıştıran giriş düzeyi maliyetini aşağı sürücü yardımcı oluyor. Müşteriler 100 cDWU (veri ambarı birimi) ile başlayan Azure SQL veri ambarı'nın önde gelen performans, esneklik ve güvenlik deneyimi özellikleri ve 30. 000 cDWU için dakikalar içinde ölçeklendirin. Gen2 performans ve esneklik, daha düşük bilgi işlem katmanı ile müşteriler yararlanabilir. 

Giriş noktası için tasarlanan yeni nesil veri ambarı bırakarak Microsoft hangi deneme ortamı için en iyi tahmin olmadan güvenli, yüksek performanslı veri ambarı tüm avantajlarını değerlendirmek istediğiniz değer temelli müşteriler kapılarını açar.  Müşteriler geçerli 500 cDWU giriş noktasından aşağı 100 cDWU düşük başlatmak mümkün olacaktır.  SQL veri ambarı Gen2'ye duraklatma destekler ve işlemleri ve yalnızca işlem esneklik ötesinde uygulanmaya devam eder.  Gen2 de sütun deposu sınırsız depolama kapasitesi sorgu başına 2,5 kat daha fazla bellek birlikte en fazla 128 eş zamanlı sorguları destekler ve ortalama 5 kat daha fazla performans üzerinde aynı veri ambarı birimine karşılaştırma yapmak deneyimleri Uyarlamalı önbelleğe alma özellikleri Gen1 aynı fiyat üzerinden.  Coğrafi olarak yedekli yedeklemeler, yerleşik garantili veri korumasıyla için 2. nesil standarttır. Azure SQL veri ambarı Gen2, işiniz ölçeklendirmek hazırdır.

## <a name="getting-started-with-azure-sql-data-warehouse-gen2"></a>Azure SQL veri ambarı Gen2 ile çalışmaya başlama 

Müşteriler, yeni bir Gen2 örneği dağıtmak veya İleri nesil veri ambarı performans ve esneklik deneyimi için mevcut bir Gen1 veri ambarı örneği yükseltmek seçebilirsiniz. 

Deneyin [Azure SQL veri ambarı bilgi işlem, en iyi duruma getirilmiş Gen2 katmanı.](https://azure.microsoft.com/services/sql-data-warehouse/?v=17.44)
Yükseltme [Azure SQL veri ambarı işlem için iyileştirilmiş Gen1 Gen2'ye](https://docs.microsoft.com/azure/sql-data-warehouse/upgrade-to-latest-generation) izleyin Azure SQL veri ambarı Gen2 bu uygulamada [Microsoft Mechanics videosunu.](https://www.youtube.com/watch?v=Ap8I3UZonzI&feature=youtu.be)


## <a name="supported-regions-for-lower-compute-tiers"></a>Alt bilgi işlem katmanı için desteklenen bölgeler

[Desteklenen bir bölge kullanılabilirliği tablosu](gen2-migration-schedule.md#automated-schedule-and-region-availability-table)

## <a name="next-steps"></a>Sonraki adımlar

- [Daha fazla bilgi edinin](upgrade-to-latest-generation.md) yükselterek SQL veri ambarı performans için iyileştirilmiş işlem hakkında. 
