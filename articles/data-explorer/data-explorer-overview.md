---
title: Azure Veri Gezgini nedir?
description: Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: overview
ms.date: 09/24/2018
ms.openlocfilehash: ebce27f3558661aca9e1bd6e7130c96c396d33ee
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51257131"
---
# <a name="what-is-azure-data-explorer"></a>Azure Veri Gezgini nedir?

Azure Veri Gezgini, günlük ve telemetri verileri için hızlı ve üst düzeyde ölçeklenebilir veri keşfetme hizmetidir. Modern yazılımlar tarafından oluşturulan birçok veri akışını işleyerek verileri toplamanıza, depolamanıza ve analiz etmenize yardımcı olur. Azure Veri Gezgini web siteleri, uygulamalar, IoT cihazları ve benzeri veri kaynaklarından gelen yüksek miktarlardaki çeşitli verileri analiz etmek için idealdir. Bu veriler tanılama, izleme, raporlama, makine öğrenmesi ve ek analiz özellikleri için kullanılır. Azure Veri Gezgini bu verilerin alınmasını kolaylaştırır ve saniyeler içinde veriler üzerinde karmaşık hızlı sorgular yapabilmenizi sağlar.

## <a name="what-makes-azure-data-explorer-unique"></a>Azure Veri Gezgini'ni benzersiz yapan nedir?

- Dakikalar içinde hızla terabaytlarca veriye ölçeklendirilir ve uygun içgörüleri keşfetmek için veri incelemesini hızla yinelemeye olanak tanır.

- Yüksek performanslı veri analizi için iyileştirilmiş, yenilikçi bir sorgu dili sunar.

- Büyük hacimli heterojen verilerin (yapılandırılmış ve yapılandırılmamış) analizini destekler.

- Kapsayıcı, güçlü ve etkileşimli bir veri analizi çözümü sunmak için diğer hizmetlerle birleşerek tam olarak ihtiyacınız olanları oluşturmanızı ve dağıtmanızı sağlar.

## <a name="data-warehousing-workflow"></a>Veri ambarı iş akışı

Azure Veri Gezgini diğer önemli hizmetlerle tümleştirilir ve veri toplama, veri alımı, depolama, dizin oluşturma, sorgulama ve görselleştirmeyi içeren uçtan uca bir çözüm sağlanır. Veri ambarı iş akışında birbirinden faklı terabaytlarca ham verinin akışında **ARAŞTIRMA** adımını yürüterek çok önemli bir ol oynar.

![Data ambarı diyagramı](media/data-explorer-overview/data-warehouse.png)

Azure Veri Gezgini; Event Hub gibi yaygın kullanılan hizmetlerin bağlayıcıları, .NET ve Python gibi SDK'ları kullanarak program aracılığıyla alma ve araştırma amacıyla altyapıya doğrudan erişim de dahil olmak üzere çeşitli veri alımı yöntemlerini destekler. Azure Veri Gezgini ek analiz ve görselleştirme verileri için analiz ve modelleme hizmetleriyle tümleştirilir.

## <a name="azure-data-explorer-flow"></a>Azure Veri Gezgini akışı

Aşağıdaki diyagram Azure Veri Gezgini ile çalışmanın farklı yönlerini gösterir.

![Azure Veri Gezgini akışı](media/data-explorer-overview/workflow.png)

Azure Veri Gezgini'nde çalışma genellikle şu desene uyar:

1. **Veritabanı oluşturma:** Bir *küme* oluşturun ve ardından bu kümenin içinde bir veya birden çok *veritabanı* oluşturun. [Hızlı başlangıç: Azure Veri Gezgini kümesi ve veritabanı oluşturma](create-cluster-database-portal.md)

1. **Verileri alma:** Veriler üzerinde sorgular çalıştırmak için verileri veritabanı tablolarına yükleyin. [Hızlı başlangıç: Verileri Event Hub'dan Azure Veri Gezgini'ne alma](ingest-data-event-hub.md)

1. **Veritabanını sorgulama:** Sorguları ve sonuçlarını çalıştırmak, gözden geçirmek ve paylaşmak için web uygulamamızı kullanın. Bu uygulama Azure portalında ve tek başına bir uygulama olarak sağlanır. Ayrıca, sorguları program aracılığıyla (SDK kullanarak) veya REST API uç noktasına da gönderebilirsiniz. [Hızlı başlangıç: Azure Veri Gezgini'nde verileri sorgulama](web-query-data.md)

## <a name="query-experience"></a>Sorgu deneyimi

Azure Veri Gezgini'nde sorgu, verileri veya meta verileri değiştirmeden verilerin işlenmesine ve bu işlemenin sonuçlarının döndürülmesine yönelik salt okunur bir istektir. Analizinizi tamamlayana kadar sorgularınızı iyileştirmeye devam edersiniz. Azure Veri Gezgini çok hızlı sorgu deneyimi sayesinde bu işlemi kolaylaştırır.

Azure Veri Gezgini çok büyük miktarlarda yapılandırılmış, yarı yapılandırılmış (JSON benzeri iç içe türler) ve yapılandırılmamış (serbest metin) verileri ayırt etmeden iyi bir şekilde işler. Metinde belirli terimleri aramanıza, belirli olayları bulmanıza, yapılandırılmış veriler üzerinde ölçüm stilinde hesaplamalar yapmanıza olanak tanır. Azure Veri Gezgini, çalışma zamanında serbest biçimli metin alanlarındaki değerleri ayıklayarak, yapılandırılmamış metin günlükleri ile yapılandırılmış sayılar ve boyutlar arasında köprü işlevi görür. Hızlı metin dizinleri oluşturma, sütun deposu ve zaman serisi işlemleri birleştirilerek veri keşfi basitleştirilir.

Azure Veri Gezgini'nin özellikleri, gezginin güçlü sorgu dili üzerine kurulmuş [Log Analytics](/azure/log-analytics/), [Application Insights](/azure/application-insights/), [Time Series Insights](/azure/time-series-insights/) ve [Windows Defender Gelişmiş Tehdit Koruması](/windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection/) gibi diğer hizmetler tarafından genişletilir.

## <a name="feedback"></a>Geri Bildirim

Azure Veri Gezgini ve onun sorgu dili ile ilgili geri bildirimlerinizi şu adreslerde sabırsızlıkla bekliyoruz:

- Soru sorun
  - [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-data-explorer)
  - [Microsoft Teknoloji Topluluğu](https://techcommunity.microsoft.com/t5/Azure-Data-Explorer/bd-p/Kusto)
  - [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureKusto)
- [User Voice'te ürün önerilerinde bulunun](https://aka.ms/AzureDataExplorer.UserVoice)

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı başlangıç: Azure Veri Gezgini kümesi ve veritabanı oluşturma](create-cluster-database-portal.md)

[Hızlı başlangıç: Verileri Event Hub'dan Azure Veri Gezgini'ne alma](ingest-data-event-hub.md)

[Hızlı başlangıç: Azure Veri Gezgini'nde verileri sorgulama](web-query-data.md)
