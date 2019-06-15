---
title: Azure HDInsight için - amacı ve yararları şirket içi Apache Hadoop kümelerini geçirme
description: Azure HDInsight geçirme şirket içi Hadoop kümelerine avantajları ve motivasyon öğrenin.
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: a03a778b2a057235b31d02e90e5ce87e9559b38a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67058565"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---motivation-and-benefits"></a>Azure HDInsight için - amacı ve yararları şirket içi Apache Hadoop kümelerini geçirme

Bu makale, geçiş için en iyi yöntemler üzerinde serideki ilk Azure HDInsight için Apache Hadoop ekonomik sistem dağıtımları şirket içinde yazılmıştır. Bu makale serisi, tasarım, dağıtım ve geçiş Azure HDInsight, Apache Hadoop çözümlerinin sorumlu olan kişilere yöneliktir. Bu makalelerinden yararlanabilir rolleri, bulut mimarları, Hadoop yöneticileri ve DevOps mühendisleri içerir. Yazılım geliştiricileri, veri mühendisleri ve veri uzmanları da kümeleri iş bulutta nasıl farklı türde açıklaması yararlı.

## <a name="why-to-migrate-to-azure-hdinsight"></a>Neden Azure HDInsight için geçirmek için

Azure HDInsight, Hadoop bileşenlerinin bulut dağıtımıdır. Azure HDInsight, devasa miktarlardaki verileri işlemeyi kolay, hızlı ve uygun maliyetli hale getirir. HDInsight gibi en popüler açık kaynak çerçevelerini içerir:

- Apache Hadoop
- Apache Spark
- Apache Hive, LLAP ile
- Apache Kafka
- Apache Storm
- Apache HBase
- R

## <a name="azure-hdinsight-advantages-over-on-premises-hadoop"></a>Şirket içi Hadoop'u Azure HDInsight avantajları

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

- **Kolay yönetim, yönetim ve izleme** -Azure HDInsight ile tümleştirilir [Azure İzleyici günlükleri](../hdinsight-hadoop-oms-log-analytics-tutorial.md) ile izlemek için kullanabileceğiniz tüm kümelerinizi tek bir arabirim sağlamak üzere.

- **Diğer Azure hizmetleriyle tümleştirme** -HDInsight kolayca tümleştirilebilir aşağıdaki gibi diğer popüler Azure hizmetleriyle:

    - Azure Data Factory (ADF)
    - Azure Blob Depolama
    - Azure Data Lake Storage Gen2
    - Azure Cosmos DB
    - Azure SQL Veritabanı
    - Azure Analysis Services

- **İşlemler ve bileşenlerini kendi kendine iyileştirme** -HDInsight sürekli izleme kendi altyapısını kullanarak altyapı ve açık kaynak bileşenlerini denetler. Ayrıca otomatik olarak, açık kaynak bileşenlerini ve düğümlerin kullanılabilir olmaması gibi kritik hataları kurtarır. Herhangi bir OSS bileşeni başarısız olursa Ambari uyarılar tetiklenir.

Daha fazla bilgi için bkz [Azure HDInsight ve Apache Hadoop teknoloji yığını nedir](../hadoop/apache-hadoop-introduction.md).

## <a name="migration-planning-process"></a>Geçiş Planlama işlemi

Aşağıdaki adımlar, Azure HDInsight şirket içi Hadoop kümelerine geçişini planlama için önerilir:

1. Geçerli şirket içi dağıtım ve Topolojileri anlayın.
2. Geçerli Proje kapsamını, zaman çizelgeleri ve takım uzmanlığı anlayın.
3. Azure gereksinimleri öğrenin.
4. En iyi uygulamalarına göre ayrıntılı bir plana oluşturmak.

## <a name="gathering-details-to-prepare-for-a-migration"></a>Bir geçişe hazırlamak için ayrıntıları toplanıyor

Bu bölüm hakkında önemli bilgiler toplamak amacıyla şablon soru sağlar:

- Şirket içi dağıtım
- Proje Ayrıntıları
- Azure gereksinimleri

### <a name="on-premises-deployment-questionnaire"></a>Şirket içi dağıtım anketi

| **Soru** | **Örnek** | **Yanıt** |
|---|---|---|
|**konu**: **Ortam**|||
|Küme dağıtım sürümü|HDP 2.6.5, CDH 5.7|
|Büyük veri ekonomik sistemi bileşenleri|HDFS, Yarn, Hive, LLAP, Impala, Kudu, HBase, Spark, MapReduce, Kafka, Zookeeper, Solr, Sqoop, Oozie, Ranger, Atlas, Falcon, Zeppelin, R|
|Küme türleri|Hadoop, Spark, Confluent Kafka, Storm, Solr|
|Küme sayısı|4|
|Ana düğüm sayısı|2|
|Çalışan düğümü sayısı|100|
|Edge düğüm sayısı| 5|
|Toplam Disk alanı|100 TB|
|Ana düğüm yapılandırması|m/y, cpu, disk, vs.|
|Veri düğümleri yapılandırma|m/y, cpu, disk, vs.|
|Kenar düğümleri yapılandırma|m/y, cpu, disk, vs.|
|HDFS şifreleme?|Evet|
|Yüksek Kullanılabilirlik|HDFS HA, HA meta veri deposu|
|Olağanüstü Durum Kurtarma / yedekleme|Yedekleme kümesi?|  
|Kümede bağımlı sistemleri|SQL Server, Teradata, Power BI, MongoDB|
|Üçüncü taraf tümleştirme|Tableau, GridGain Qubole, Informatica, Splunk|
|**konu**: **Güvenlik**|||
|Çevre güvenliği|Güvenlik Duvarları|
|Küme kimlik doğrulaması ve yetkilendirme|Active Directory, Ambari, Cloudera Yöneticisi, kimlik doğrulama yok|
|HDFS erişim denetimi|  El ile ssh kullanıcıları|
|Hive kimlik doğrulaması ve yetkilendirme|Sentry, LDAP, Kerberos Ranger AD|
|Denetim|Ambari, Cloudera Gezgin Ranger|
|İzleme|Grafit, toplanan statsd, Telegraf, InfluxDB|
|Uyarı|Datadog Kapacitor, Prometheus,|
|Veri tutma süresi| 3 yıl, 5 yıl|
|Küme yöneticileri|Birden çok yöneticisi olan tek yönetici|

### <a name="project-details-questionnaire"></a>Proje Ayrıntıları anketi

|**Soru**|**Örnek**|**Yanıt**|
|---|---|---|
|**konu**: **İş yükleri ve sıklığı**|||
|MapReduce işleri|10 işleri--günde iki kez||
|Hive işleri|100 iş--her saat||
|Spark toplu işler|50 işleri--15 dakikada bir||
|Spark akış işleri|5 iş--3 dakikada bir||
|Yapılandırılmış akış işleri|5 iş--dakika başı||
|ML Model eğitim işleri|bir hafta içerisinde bir kez 2 iş--||
|Programlama Dilleri|Python, Scala, Java||
|Komut dosyaları|Shell, Python||
|**konu**: **Veri**|||
|Veri kaynakları|Düz dosyaları, Json, Kafka, RDBMS||
|Verileri düzenleme|Oozie iş akışları, hava akışı||
|Bellek arama|Apache Ignite, ignite Redis||
|Veri hedefleri|HDFS, RDBMS, Kafka, MPP ||
|**konu**: **Meta verileri**|||
|Hive veritabanı türü|MySQL, Postgres||
|Hayır. Hive meta depolar|2||
|Hayır. Hive tabloları|100||
|Hayır. Ranger ilkeleri|20||
|Hayır. Oozie iş akışları|100||
|**konu**: **Ölçeklendirme**|||
|Çoğaltma dahil olmak üzere veri hacmi|100 TB||
|Günlük alımı hacmi|50 GB||
|Veri büyüme oranı|yıl başına %10||
|Küme düğümleri büyüme oranı|%5 yılda
|**konu**: **Küme kullanımı**|||
|Ortalama CPU yüzdesi|60%||
|Ortalama bellek yüzdesi|75%||
|Kullanılan disk alanı|75%||
|Ortalama ağ yüzdesi|%25
|**konu**: **Ekibi**|||
|Hayır. Yöneticileri|2||
|Hayır. Geliştiriciler|10||
|Hayır. Son kullanıcıları|100||
|Beceriler|Hadoop, Spark||
|Hayır. Geçiş çalışmalarında kullanılabilir kaynakları|2||
|**konu**: **Sınırlamalar**|||
|Geçerli sınırlamalar|Gecikmesi||
|Geçerli zorlukları|Eşzamanlılık sorunu||

### <a name="azure-requirements-questionnaire"></a>Azure gereksinimleri anketi

|**konu**: **Altyapı** |||
|---|---|---|
|**Soru**|**Örnek**|**Yanıt**|
| Tercih edilen bölge|ABD Doğu||
|Tercih edilen VNet?|Evet||
|HA / DR gerekli?|Evet||
|Diğer bulut hizmetleriyle tümleştirme?|ADF, CosmosDB||
|**konu**:   **Veri Taşıma**  |||
|İlk yükleme tercihi|DistCp, veri, ADF, WANDisco kutusu||
|Veri aktarımı değişim|DistCp, AzCopy||
|Sürekli artımlı veri aktarımı|DistCp, Sqoop||
|**konu**:   **İzleme ve uyarı** |||
|Azure izleme ve uyarı Vs tümleştirme üçüncü taraf izleme kullanın|İzleme ve uyarı Azure kullanın||
|**konu**:   **Güvenlik tercihleri** |||
|Özel ve korumalı veri işlem hattı?|Evet||
|Etki alanına katılmış kümeye (ESP)?|     Evet||
|Böylece, şirket içinde AD eşitleme buluta?|     Evet||
|Hayır. AD kullanıcıları eşitleme?|          100||
|Bulut için parola eşitlemeyi Tamam?|    Evet||
|Kullanıcılar yalnızca bulut?|                 Evet||
|MFA gerekli?|                       Hayır|| 
|Veri yetkilendirme gereksinimleri var mı?|  Evet||
|Rol tabanlı erişim denetimi?|        Evet||
|Denetim gerekli?|                  Evet||
|Bekleyen verileri şifreleme?|          Evet||
|Aktarımdaki verileri şifreleme?|       Evet||
|**konu**:   **RE mimarisi tercihleri** |||
|Tek küme vs belirli küme türlerinin|Belirli küme türlerinin||
|Birlikte bulunan depolama Vs Uzak Depolama?|Uzaktaki Depolama Birimi||
|Veri olarak daha küçük bir küme boyutu uzaktan depolanır?|Daha küçük bir küme boyutu||
|Tek bir büyük küme yerine birden çok daha küçük bir küme kullanılır?|Birden çok daha küçük bir küme kullanın||
|Uzak meta veri deposu kullanabilir?|Evet||
|Paylaşım meta depolar farklı sunucular arasında?|Evet||
|İş yükleri Ayrıştır?|Spark işlerinde Hive işlerini değiştirin||
|ADF veri düzenlemesi için kullanılır?|Hayır||

## <a name="next-steps"></a>Sonraki adımlar

Bu serideki sonraki makaleyi okuyun:

- [Şirket içi-Azure HDInsight Hadoop geçiş için en iyi mimari](apache-hadoop-on-premises-migration-best-practices-architecture.md)
