---
title: Azure Databricks nedir? | Microsoft Docs
description: "Azure Databricks’in ne olduğu ve Databricks üzerinde Spark’ı Azure’a nasıl getirdiği hakkında bilgi edinin. Azure Databricks, Microsoft Azure bulut hizmetleri platformu için iyileştirilen Apache Spark tabanlı bir analiz platformudur."
services: azure-databricks
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 01/22/2018
ms.author: nitinme
ms.custom: mvc
ms.openlocfilehash: 2c42f8a9dd67ec2debecffd7cb2cdb902ab203a2
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="what-is-azure-databricks"></a>Azure Databricks nedir?

Azure Databricks, Microsoft Azure bulut hizmetleri platformu için iyileştirilen Apache Spark tabanlı bir analiz platformudur. Apache Spark’ın kurucuları ile birlikte tasarlanan Databricks, tek tıklama ile kurulum olanağı ve kolaylaştırılmış iş akışlarının yanı sıra veri uzmanları, veri mühendisleri ve iş analistleri arasında işbirliği sağlayan etkileşimli bir çalışma alanı sunmak amacıyla Azure ile tümleştirilmiştir.

![Azure Databricks nedir?](./media/what-is-azure-databricks/azure-databricks-overview.png "What is Azure Databricks?")

## <a name="apache-spark-based-analytics-platform"></a>Apache Spark tabanlı analiz platformu

Azure Databricks tam açık kaynaklı Apache Spark küme teknolojileri ve özellikleri içerir. Azure Databricks’te Spark aşağıdaki bileşenleri içerir:

![Azure Databricks’te Apache Spark](./media/what-is-azure-databricks/apache-spark-ecosystem-databricks.png "Apache Spark in Azure Databricks")

* **Spark SQL ve DataFrames**: Spark SQL, yapılandırılmış verilerle çalışmaya yönelik Spark modülüdür. Bir DataFrame, adlandırılmış sütunlar halinde düzenlenmiş, dağıtılmış bir veri koleksiyonudur. Kavramsal olarak, ilişkisel bir veritabanındaki tabloya veya R/Python’daki veri çerçevesine eşdeğerdir.

* **Akış**: Analitik ve etkileşimli uygulamalar için gerçek zamanlı veri işleme ve analizi. HDFS, Flume ve Kafka ile tümleştirilir.

* **MLib**: Sınıflandırma, regresyon, kümeleme, ortak filtreleme, boyut düzeyi azaltma gibi genel öğrenme algoritmaları ve yardımcı programlarının yanı sıra temel alınan iyileştirme temellerinden oluşan Machine Learning kitaplığı.

* **GraphX**: Bilişsel analizden veri keşfine varan geniş kullanım örnekleri kapsamı için grafikler ve grafik hesaplamaları.

* **Spark Core API’si**: R, SQL, Python, Scala ve Java desteği içerir.

## <a name="apache-spark-in-azure-databricks"></a>Azure Databricks’te Apache Spark

Azure Databricks, aşağıdakileri içeren sıfır yönetimli bir bulut platformu sağlayarak Spark özelliklerine katkıda bulunur:

- Tam yönetilen Spark kümeleri
- Keşif ve görselleştirme için etkileşimli bir çalışma alanı
- Sık kullandığınız Spark tabanlı uygulamalarınızı destekleyen bir platform

### <a name="fully-managed-apache-spark-clusters-in-the-cloud"></a>Bulutta tam yönetilen Apache Spark kümeleri

Azure Databricks, Spark uzmanları tarafından yönetilip desteklenen, bulutta güvenli ve güvenilir bir üretim ortamına sahiptir. Şunları yapabilirsiniz:

* Birkaç saniye içinde kümeler oluşturun.
* Sunucusuz kümeler de dahil olmak üzere kümelerin ölçeğini dinamik olarak artırıp azaltın ve ekipler arasında paylaşın. 
* REST API'lerini kullanarak kümeleri programlı olarak kullanın. 
* Merkezileştirme olmadan verilerinizi birleştirmenizi sağlayan, Spark üzerine eklenmiş güvenli veri tümleştirme yeteneklerini kullanın. 
* Her yayınla en son Apache Spark özelliklerine anında erişin.

### <a name="databricks-runtime"></a>Databricks Çalışma Zamanı
Databricks Çalışma Zamanı, Apache Spark’ın üzerine kurulmuştur ve Azure bulutu için yerel olarak oluşturulmuştur. 

**Sunucusuz** seçeneği ile Azure Databricks, altyapı karmaşıklığını ve veri altyapınızı ayarlamak ve yapılandırmak için özel uzmanlık gereksinimini tamamen ortadan kaldırır. Sunucusuz seçeneği, veri bilimcilerinin ekip olarak hızla yinelemesine yardımcı olur.

Üretim işlerinin performansıyla ilgilenen veri mühendisleri, G/Ç katmanında ve işleme katmanında (Databricks G/Ç) çeşitli iyileştirmeler aracılığıyla daha hızlı ve daha iyi performans gösteren bir Spark altyapısı sağlar.

### <a name="workspace-for-collaboration"></a>İşbirliği için çalışma alanı

Azure Databricks, işbirliğine dayalı ve tümleşik bir ortam aracılığıyla veri araştırma, prototip oluşturma ve Spark’ta veri temelli uygulamalar çalıştırma işlemini kolaylaştırır.

* Kolay veri araştırması ile verileri nasıl kullanacağınızı belirleyin.
* İlerleme durumunuzu R, Python, Scala veya SQL dilinde not defterlerine kaydedin.
* Verileri birkaç tıklama ile görselleştirin, Matplotlib, ggplot veya d3 gibi tanıdık araçları kullanın.
* Dinamik raporlar oluşturmak için etkileşimli panolar kullanın.
* Spark kullanın ve verilerle eşzamanlı etkileşim kurun.

## <a name="enterprise-security"></a>Kurumsal güvenlik

Azure Databricks, Azure Active Directory tümleştirmesi, rol tabanlı denetimler ve verilerinizi ve işinizi koruyan SLA'lar dahil olmak üzere kurumsal düzeyde Azure güvenliği sağlar.

* Azure Active Directory ile tümleştirme, Azure Databricks kullanarak tam Azure tabanlı çözümler çalıştırmanıza olanak sağlar.
* Azure Databrick rol tabanlı erişimi; not defterleri, kümeler, işler ve veriler için ayrıntılı kullanıcı izinleri sağlar.
* Kurumsal düzeyde SLA’lar. 

## <a name="integration-with-azure-services"></a>Azure hizmetleriyle tümleştirme

Azure Databricks, Azure veritabanları ve depolarıyla derin bir şekilde tümleştirilir: SQL Veri Ambarı, Cosmos DB, Data Lake Store ve Blob Depolama. 

## <a name="integration-with-power-bi"></a>Power BI ile tümleştirme
Power BI ile zengin tümleştirme sayesinde Azure Databricks, etkili öngörülerinizi hızlı ve kolay bir şekilde keşfedip paylaşmanızı sağlar. JDBC/ODBC küme uç noktaları aracılığıyla Tableau yazılımı gibi diğer BI araçlarını da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı Başlangıç: Azure Databricks üzerinde bir Spark işi çalıştırma](quickstart-create-databricks-workspace-portal.md)
* [Spark kümeleri ile çalışma](https://docs.azuredatabricks.net/user-guide/clusters/index.html)
* [Not defterleri ile çalışma](https://docs.azuredatabricks.net/user-guide/notebooks/index.html)
* [Spark işleri oluşturma](https://docs.azuredatabricks.net/user-guide/jobs.html)

 









