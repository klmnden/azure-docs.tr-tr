---
title: Azure Databricks nedir? | Microsoft Belgeleri
description: "Azure Databricks nedir ve nasıl Databricks üzerinde Spark Azure'a getirir hakkında bilgi edinin. Azure Databricks Microsoft Azure bulut hizmetleri platformu için en iyi hale getirilmiş bir Apache Spark tabanlı analytics platformudur."
services: azure-databricks
documentationcenter: 
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: nitinme
ms.openlocfilehash: 7ced38cda2669cf03e51f50fbbbeea0344da9277
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="what-is-azure-databricks"></a>Azure Databricks nedir?

Azure Databricks Microsoft Azure bulut hizmetleri platformu için en iyi hale getirilmiş bir Apache Spark tabanlı analytics platformudur. Apache Spark genel ile tasarlanmış, Databricks tek tıklatmayla kurulumu, kolaylaştırılmış iş akışları ve veri bilimcileri, veri mühendisleri ve iş analistleri arasında işbirliğini sağlayan bir etkileşimli çalışma alanı sağlamak üzere Azure ile tümleşiktir.

![Azure Databricks nedir? ] (./media/what-is-azure-databricks/azure-databricks-overview.png "Azure Databricks nedir?")

## <a name="apache-spark-based-analytics-platform"></a>Apache Spark tabanlı analiz platformu

Azure Databricks tam açık kaynaklı Apache Spark küme teknolojileri ve özellikleri içerir. Azure Databricks Spark aşağıdaki bileşenleri içerir:

![Apache Spark Azure Databricks](./media/what-is-azure-databricks/apache-spark-ecosystem-databricks.png "Azure Databricks Apache Spark")

* **Spark SQL ve DataFrames**: Spark SQL yapılandırılmış verilerle çalışmak için Spark modül değil. Bir DataFrame dağıtılmış adlandırılmış sütunlara düzenlenmiş veriler koleksiyonudur. Kavramsal olarak bir tablo ilişkisel bir veritabanındaki veya R/Python veri çerçevede eşdeğerdir.

* **Akış**: Gerçek zamanlı veri işleme ve analizi Analitik ve etkileşimli uygulamalar için. HDFS, Flume ve Kafka ile tümleştirilir.

* **MLib**: Machine Learning kitaplığı ortak algoritmaları ve yardımcı programlar öğrenme, Sınıflandırma, regresyon, kümeleme, işbirliği filtreleme, boyut azaltma dahil olmak üzere, hem de en iyi duruma getirme temelleri temel oluşan.

* **GraphX**: grafikleri ve grafik hesaplama geniş bir kapsam bilişsel analytics çalışmalarından veri keşfi için kullanın.

* **Spark Core API**: R, SQL, Python, Scala ve Java için destek içerir.

## <a name="apache-spark-in-azure-databricks"></a>Azure Databricks Apache Spark

Azure Databricks, içeren sıfır yönetim bulut platformu sağlayarak Spark özellikleri üzerine yapılandırılmıştır:

- Tam olarak yönetilen Spark kümeleri
- Keşfi ve görselleştirme için etkileşimli bir çalışma alanı
- Sık kullanılan Spark tabanlı uygulamaları destekleyen bir platform

### <a name="fully-managed-apache-spark-clusters-in-the-cloud"></a>Bulutta tam olarak yönetilen Apache Spark kümeleri

Azure Databricks, yönetilen ve Spark uzmanlar tarafından desteklenen bulutta güvenli ve güvenilir üretim ortamında sahiptir. Şunları yapabilirsiniz:

* Saniye cinsinden kümeleri oluşturun.
* Dinamik olarak otomatik ölçeklendirme yukarı ve aşağı sunucusuz kümeleri dahil olmak üzere kümeleri ve bunları takımlar arasında paylaşın. 
* Kümeler REST API'lerini kullanarak programlı olarak kullanın. 
* Verilerinizi merkezileşmeyi olmadan bütünleştirin sağlayan Spark üzerinde kurulu güvenli veri tümleştirme yetenekleri kullanın. 
* En son Apache Spark özelliklere her sürümle anında erişin.

### <a name="databricks-runtime"></a>Databricks çalışma zamanı
Databricks çalışma zamanı Apache Spark üzerinde kurulu ve yerel olarak Azure bulut için yapılandırılmış. 

İle **sunucusuz** seçeneği, Azure Databricks tamamen soyutlar altyapı karmaşıklığı ve ayarlama ve yapılandırma verileri altyapınız özelleştirilmiş uzmanlık gereksinimini çıkışı. Sunucusuz seçeneği veri bilimcilerine ekip olarak hızla yineleme yardımcı olur.

Üretim işleri performansı hakkında dikkat edin, veri mühendisleri için Azure Databricks hızlıdır altyapısı ve kullanıcı çeşitli iyileştirmeler g/ç katman ve işleme katman (Databricks g/ç) aracılığıyla bir Spark sağlar.

### <a name="workspace-for-collaboration"></a>İşbirliği için çalışma

İşbirliğine dayalı ve tümleşik bir ortam, Azure Databricks veri, prototip oluşturma ve Spark çalışan veri tabanlı uygulamalarda keşfetme işlemini kolaylaştırır.

* Kolay veri araştırması ile veri kullanmayı belirler.
* R, Python, Scala veya SQL not defterlerinde ilerlemenizi belge.
* Birkaç tıklama verileri görselleştirmek ve tanıdık Araçlar Matplotlib, ggoplot veya d3 gibi kullanın.
* Etkileşimli panolar dinamik raporlar oluşturmak için kullanın.
* Verilerle aynı anda etkileşimli ve Spark kullanın.

## <a name="enterprise-security"></a>Kurumsal güvenlik

Azure Databricks Kurumsal düzeyde Azure Active Directory ile tümleştirme, rol tabanlı denetimleri ve verilerinizi ve işinizin koruma SLA'ları dahil olmak üzere Azure güvenliği sağlar.

* Azure Active Directory ile tümleştirme Azure Databricks kullanan tam Azure tabanlı çözümler çalıştırmanıza olanak sağlar.
* Hassas kullanıcı izinlerini dizüstü bilgisayarlar, kümeleri, işleri ve verileri Azure Databricks rol tabanlı erişim sağlar.
* Kurumsal düzeyde SLA. 

## <a name="integration-with-azure-services"></a>Azure hizmetleriyle tümleştirme

Azure Databricks tümleştirir derine Azure veritabanları ve depolar: SQL Data Warehouse, Cosmos DB, Data Lake Store ve Blob Depolama. 

## <a name="integration-with-power-bi"></a>Power BI ile tümleştirme
Power BI ile zengin tümleştirmesi Azure Databricks bulmak ve etkili öngörülerinizi hızla ve kolayca paylaşmanızı sağlar. JDBC/ODBC küme uç noktaları aracılığıyla Tableau yazılımı gibi diğer BI araçları da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı Başlangıç: bir Azure Databricks üzerinde Spark çalıştırın](quickstart-create-databricks-workspace-portal.md)
* [Spark kümeleri ile çalışma](https://docs.azuredatabricks.net/user-guide/clusters/index.html)
* [Dizüstü bilgisayarlar ile çalışma](https://docs.azuredatabricks.net/user-guide/notebooks/index.html)
* [Spark işleri oluşturma](https://docs.azuredatabricks.net/user-guide/jobs.html)

 









