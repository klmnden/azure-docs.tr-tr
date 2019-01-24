---
title: Azure SQL veri ambarı Gen2, alt bilgi işlem katmanını destekler | Microsoft Docs
description: Azure SQL veri ambarı Gen2, artık daha düşük bilgi işlem katmanı destekler
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 12/03/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 2aa513617f24201dfb341f9ab72ab9e3a221450d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54819365"
---
# <a name="azure-sql-data-warehouse-gen2-support-for-lower-compute-tiers"></a>Alt bilgi işlem katmanı için Azure SQL veri ambarı Gen2 desteği

Microsoft, sorgular çok hızlı Azure SQL veri ambarı Gen2 için daha düşük bilgi işlem katmanı ekleyerek yoğun işleme yeteneğine sahip bir veri ambarı'nı çalıştıran giriş düzeyi maliyetini aşağı sürücü yardımcı oluyor. Müşteriler 100 cDWU (veri ambarı birimi) ile başlayan Azure SQL veri ambarı'nın önde gelen performans, esneklik ve güvenlik deneyimi özellikleri ve 30. 000 cDWU için dakikalar içinde ölçeklendirin. Gen2 performans ve esneklik, daha düşük bilgi işlem katmanı ile müşteriler yararlanabilir. 

Giriş noktası için tasarlanan yeni nesil veri ambarı bırakarak Microsoft hangi deneme ortamı için en iyi tahmin olmadan güvenli, yüksek performanslı veri ambarı tüm avantajlarını değerlendirmek istediğiniz değer temelli müşteriler kapılarını açar.  Müşteriler geçerli 500 cDWU giriş noktasından aşağı 100 cDWU düşük başlatmak mümkün olacaktır.  SQL veri ambarı Gen2'ye duraklatma destekler ve işlemleri ve yalnızca işlem esneklik ötesinde uygulanmaya devam eder.  Gen2 de sütun deposu sınırsız depolama kapasitesi sorgu başına 2,5 kat daha fazla bellek birlikte en fazla 128 eş zamanlı sorguları destekler ve ortalama 5 kat daha fazla performans üzerinde aynı veri ambarı birimine karşılaştırma yapmak deneyimleri Uyarlamalı önbelleğe alma özellikleri Gen1 aynı fiyat üzerinden.  Coğrafi olarak yedekli yedeklemeler, yerleşik garantili veri korumasıyla için 2. nesil standarttır. Azure SQL veri ambarı Gen2, işiniz ölçeklendirmek hazırdır.

Şu anda portal dağıtımı veya daha düşük Katman 2. nesil örneklerine ölçeklendirme desteklemez. Bu işlevsellik, lütfen bu arada etkinleştirmek için çalışıyoruz [bilet gönderin](sql-data-warehouse-get-started-create-support-ticket.md) bu yeni katmanı avantajından istiyorsanız.

## <a name="getting-started-with-azure-sql-data-warehouse-gen2"></a>Azure SQL veri ambarı Gen2 ile çalışmaya başlama 

Müşteriler, yeni bir Gen2 örneği dağıtmak veya İleri nesil veri ambarı performans ve esneklik deneyimi için mevcut bir Gen1 veri ambarı örneği yükseltmek seçebilirsiniz. 

Deneyin [Azure SQL veri ambarı bilgi işlem, en iyi duruma getirilmiş Gen2 katmanı.](https://azure.microsoft.com/services/sql-data-warehouse/?v=17.44)
Yükseltme [Azure SQL veri ambarı işlem için iyileştirilmiş Gen1 Gen2'ye](https://docs.microsoft.com/azure/sql-data-warehouse/upgrade-to-latest-generation) izleyin Azure SQL veri ambarı Gen2 bu uygulamada [Microsoft Mechanics videosunu.](https://www.youtube.com/watch?v=Ap8I3UZonzI&feature=youtu.be)


## <a name="supported-regions-for-lower-compute-tiers"></a>Alt bilgi işlem katmanı için desteklenen bölgeler

- Doğu US1 
- Doğu ABD 2
- Batı Avrupa
- Kuzey Avrupa
- Batı ABD 2
- Güneydoğu Asya
- ABD kullanımdadır
- Orta ABD 
- Doğu Asya
- Japonya Doğu
- Hindistan Orta
- Avustralya Doğu
- Orta Kanada
- Japonya Batı 
- Orta Kanada

## <a name="next-steps"></a>Sonraki adımlar

- [Daha fazla bilgi edinin](upgrade-to-latest-generation.md) yükselterek SQL veri ambarı performans için iyileştirilmiş işlem hakkında. 
