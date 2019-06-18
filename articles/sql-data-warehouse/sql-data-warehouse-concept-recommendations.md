---
title: SQL veri ambarı önerileri - kavramları | Microsoft Docs
description: SQL veri ambarı öneriler ve nasıl oluşturulacağını öğrenin
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 11/05/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: b275f23209979e1a8068ecd99465f7b52392bc6c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61421231"
---
# <a name="sql-data-warehouse-recommendations"></a>SQL veri ambarı önerileri

Bu makalede, Azure Danışmanı üzerinden SQL veri ambarı tarafından sunulan önerileri açıklanmaktadır.  

SQL veri ambarı veri Ambarınızı emin olmak için öneriler tutarlı performans için iyileştirilmiş sağlar. Veri ambarı önerileri ile sıkı bir şekilde tümleştirilir [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations) içinde doğrudan en iyi yöntemler sağlamak üzere [Azure portalında](https://aka.ms/Azureadvisor). SQL veri ambarı, veri ambarının geçerli durumunu analiz eder, telemetri ve yüzeyleri öneriler etkin iş yükünüz için günlük bir tempoyla toplar. Desteklenen veri ambarı öneri senaryoları, önerilen eylemleri uygulamak nasıl birlikte aşağıda özetlenmiştir.

SQL veri ambarı Advisor hakkında geri bildirimde bulunmak veya herhangi bir sorunla karşılaşırsanız, ulaşın [ sqldwadvisor@service.microsoft.com ](mailto:sqldwadvisor@service.microsoft.com).   

Tıklayın [burada](https://aka.ms/Azureadvisor) önerilerinizi bugün denetlemek için! Şu anda bu özellik yalnızca 2. nesil veri ambarları için geçerlidir. 

## <a name="data-skew"></a>Veri dengesizliği

Veri dengesizliği yükünüz çalıştırırken ek veri hareketi veya kaynak darboğazları neden olabilir. Aşağıdaki belgeler, veri dengesizliği tanımlamak ve uygun dağıtım anahtar seçerek oluşmasını önlemek için Göster açıklar.

- [Belirlenmesi ve kaldırılmasına eğme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice) 

## <a name="no-or-outdated-statistics"></a>Hayır veya güncel olmayan istatistikleri

Yetersiz sorgu planlarına oluşturmak SQL veri ambarı sorgu iyileştiricisi neden olabileceği yetersiz istatistik sorgu performansı önemli ölçüde etkileyebilir. Aşağıdaki belgeler, oluşturma ve istatistikleri güncelleştirmeyi en iyi uygulamalar açıklanmaktadır:

- [Tablo istatistikleri oluşturma ve güncelleştirme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics)

Bu öneri tarafından etkilenen tabloların listesini görmek için aşağıdaki komutu çalıştırın. [T-SQL betiği](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables). Danışman, sürekli olarak bu önerileri oluşturmak için aynı T-SQL betiği çalıştırır.

## <a name="replicate-tables"></a>Tablo çoğaltma

Çoğaltılmış tablo önerileri için aşağıdaki fiziksel özelliklerine göre tablo adayları Advisor algılar:

- Çoğaltılmış tablo boyutu
- Sütun sayısı
- Tablo dağıtım türü
- Bölüm sayısı

Boyutu ve yüksek kaliteli önerileri oluşturulan emin olmak için etkinlik eşikleri geçici veri ambarı ve sürekli olarak Advisor satırları ortalama olarak, döndürülen tablo erişim sıklığı gibi iş yükü tabanlı buluşsal yararlanır. 

İş yükü tabanlı buluşsal yöntemler her çoğaltılmış tablo öneri için Azure portalında bulabilirsiniz aşağıda açıklanmıştır:

- Tarama ortalama-son yedi gün içindeki tablodaki her tablo erişim için döndürülen satırları ortalama yüzdesi
- Sık gerçekleştirilen okuma, güncelleştirme - tablonun son yedi erişim etkinliğini gösteren sırasında günde güncelleştirilmemiş olduğunu gösterir.
- Okuma/güncelleştirme oranı - nasıl sık tablonun son yedi gün içindeki ne zaman güncelleştirildiğini göre erişildi oranı
- Etkinliği - erişim etkinliklere göre kullanımını ölçer. Bu tablo erişim etkinliğini göre ortalama tablo erişim etkinliğini son yedi gün içindeki veri ambarı arasında karşılaştırır. 

Şu anda Advisor yalnızca en fazla dört çoğaltılmış tablo adayları aynı anda en yüksek etkinlik öncelik kümelenmiş columnstore dizinleri ile birlikte gösterilir.

> [!IMPORTANT]
> Çoğaltılmış tablo öneri tam kavram değil ve hesabı veri taşıma işlemleri almaz. Bu bir buluşsal yöntem eklemek için çalışıyoruz ancak bu sırada, her zaman iş yükünüz öneri uygulandıktan sonra doğrulamalıdır. Lütfen başvurun sqldwadvisor@service.microsoft.com ilerletmek için İş yükünüzün neden çoğaltılmış tablo önerileri fark ederseniz. Aşağıdaki çoğaltılmış tablolar hakkında daha fazla bilgi için ziyaret [belgeleri](https://docs.microsoft.com/azure/sql-data-warehouse/design-guidance-for-replicated-tables#what-is-a-replicated-table).
