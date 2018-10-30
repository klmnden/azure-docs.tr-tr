---
title: Azure HDInsight için - amacı ve yararları şirket içi Apache Hadoop kümelerini geçirme
description: Azure HDInsight geçirme şirket içi Hadoop kümelerine avantajları ve motivasyon öğrenin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: efaedccde854d8cd7fa777aa9457db7203ce833a
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50221943"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---motivation-and-benefits"></a>Azure HDInsight için - amacı ve yararları şirket içi Apache Hadoop kümelerini geçirme

Bu makale, geçiş için en iyi yöntemler üzerinde serideki ilk Azure HDInsight için Apache Hadoop ekonomik sistem dağıtımları şirket içinde yazılmıştır. Bu makale serisi, tasarım, dağıtım ve geçiş Azure HDInsight, Apache Hadoop çözümlerinin sorumlu olan kişilere yöneliktir. Bu makalelerinden yararlanabilir rolleri, bulut mimarları, Hadoop yöneticileri ve DevOps mühendisleri içerir. Yazılım geliştiricileri, veri mühendisleri ve veri uzmanları da kümeleri iş bulutta nasıl farklı türde açıklaması yararlı.

## <a name="why-to-migrate-to-azure-hdinsight"></a>Neden Azure HDInsight için geçirmek için

Azure HDInsight, kaynaklı Hadoop bileşenlerinin bulut dağıtımıdır [Hortonworks veri Platform(HDP)](https://hortonworks.com/products/data-center/hdp/). Azure HDInsight, devasa miktarlardaki verileri işlemeyi kolay, hızlı ve uygun maliyetli hale getirir. HDInsight gibi en popüler açık kaynak çerçevelerini içerir:

- Apache Hadoop
- Apache Spark
- Apache Hive, LLAP ile
- Apache Kafka
- Apache Storm
- Apache HBase
- R

## <a name="advantages-that-azure-hdinsight-offers-over-on-premises-hadoop"></a>Azure HDInsight şirket içi Hadoop sunduğu avantajları

- **Düşük maliyetli** -maliyetleri tarafından indirgenebilir [isteğe bağlı kümeler oluşturma](../hdinsight-hadoop-create-linux-clusters-adf.md) ve yalnızca kullandığınız kadarı için ödeme yapma. Ayrılmış işlem ve depolama veri hacmi küme boyutu bağımsız tutarak esneklik sağlar.
- **Küme oluşturma otomatik** - otomatik küme oluşturma, minimal kurulumu ve yapılandırması gerektirir. Otomasyon, isteğe bağlı kümeler için kullanılabilir.
- **Yönetilen donanım ve yapılandırma** -fiziksel donanım veya bir HDInsight kümesi ile altyapı hakkında endişelenmenize gerek yoktur. Yeni küme yapılandırmasını belirtin ve bunu Azure ayarlar.
- **Kolayca ölçeklenebilir** -HDInsight olanak tanır [ölçek](../hdinsight-administer-use-portal-linux.md) iş yükleri artırın veya azaltın. Azure veri yeniden dağıtımı ve iş yükü veri işleme işleri kesintiye uğratmadan yeniden Dengeleme üstlenir.
- **Genel kullanılabilirlik** -HDInsight, daha fazla kullanılabilir [bölgeleri](https://azure.microsoft.com/regions/services/) herhangi bir büyük veri analizi teklifi daha. Azure HDInsight ayrıca temel bağımsız bölgelerde kurumsal ihtiyaçlarınızı karşılamanıza olanak sağlayan Azure Kamu, Çin ve Almanya’da da kullanılabilir.
- **Güvenli ve uyumlu** -HDInsight, kurumsal veri varlıklarınızı korumanıza olanak sağlayan [Azure sanal ağı](../hdinsight-extend-hadoop-virtual-network.md), [şifreleme](../hdinsight-hadoop-create-linux-clusters-with-secure-transfer-storage.md)ve tümleştirmesiyle [Azure Active Dizin](../domain-joined/apache-domain-joined-introduction.md). HDInsight ayrıca en popüler sektör ve kamu karşılayan [uyumluluk standartlarını](https://azure.microsoft.com/overview/trusted-cloud).
- **Sürüm Yönetimi Basitleştirilmiş** -Azure HDInsight Hadoop ekonomik sistem bileşenleri yönetir ve güncel kalmasını sağlar. Yazılım güncelleştirmeleri, genellikle şirket içi dağıtımlar için karmaşık bir işlem var.
- **Bileşenleri arasında daha az bağımlılıkları olan belirli iş yükleri için en iyi duruma getirilmiş daha küçük kümeleri** -tipik şirket içi Hadoop Kurulumu çok amaçlı tek bir küme kullanır. Azure HDInsight ile iş yüküne özgü kümeleri oluşturulabilir. Belirli iş yükleri için kümeleri oluşturma, artan karmaşıklığı ile tek bir küme bakım işlemlerinin karmaşıklığını ortadan kaldırır.
- **Üretkenlik** -çeşitli araçlar Hadoop ve Spark'a yönelik tercih edilen geliştirme ortamınızda kullanabilirsiniz.
- **Özel araçları veya üçüncü taraf uygulamaları ile genişletilebilirlik** -HDInsight kümeleri ile yüklü bileşenlerin uzatılabilir ve kullanarak diğer büyük veri çözümleriyle de tümleştirilebilir [tek tıklamayla](https://azure.microsoft.com/services/hdinsight/partner-ecosystem/)  Azure Marketi dağıtımları.
- **Kolay yönetim, yönetim ve izleme** -Azure HDInsight ile tümleştirilir [Azure Log Analytics](../hdinsight-hadoop-oms-log-analytics-tutorial.md) ile izlemek için kullanabileceğiniz tüm kümelerinizi tek bir arabirim sağlamak üzere.
- **Diğer Azure hizmetleriyle tümleştirme** -HDInsight kolayca tümleştirilebilir aşağıdaki gibi diğer popüler Azure hizmetleriyle:

    - Azure Data Factory (ADF)
    - Azure Blob Depolama
    - Azure Data Lake Storage Gen2
    - Azure Cosmos DB
    - Azure SQL Database
    - Azure Analysis Services

- **İşlemler ve bileşenlerini kendi kendine iyileştirme** -HDInsight sürekli izleme kendi altyapısını kullanarak altyapı ve açık kaynak bileşenlerini denetler. Ayrıca otomatik olarak, açık kaynak bileşenlerini ve düğümlerin kullanılabilir olmaması gibi kritik hataları kurtarır. Herhangi bir OSS bileşeni başarısız olursa Ambari uyarılar tetiklenir.

Daha fazla bilgi için bkz [Azure HDInsight ve Hadoop teknoloji yığını nedir](../hadoop/apache-hadoop-introduction.md).

## <a name="migration-planning-process"></a>Geçiş Planlama işlemi

Aşağıdaki adımlar, Azure HDInsight şirket içi Hadoop kümelerine geçişini planlama için önerilir:

1. Geçerli şirket içi dağıtım ve Topolojileri anlayın.
2. Geçerli Proje kapsamını, zaman çizelgeleri ve takım uzmanlığı anlayın.
3. Azure gereksinimleri öğrenin.
4. En iyi uygulamalarına göre ayrıntılı bir plana oluşturmak.

## <a name="next-steps"></a>Sonraki adımlar

Bu serideki sonraki makaleyi okuyun:

- [Şirket içi-Azure HDInsight Hadoop geçiş için en iyi mimari](apache-hadoop-on-premises-migration-best-practices-architecture.md)