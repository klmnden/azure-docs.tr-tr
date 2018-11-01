---
title: Azure HDInsight - veri geçişi en iyi uygulamaları şirket içi Apache Hadoop kümelerini geçirme
description: Veri geçiş için en iyi Azure HDInsight geçirme şirket içi Hadoop kümelerine öğrenin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: 6b06b8eb8d5e18acd3107ec5cccac79fc7be7edc
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50418186"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---data-migration-best-practices"></a>Azure HDInsight - veri geçişi en iyi uygulamaları şirket içi Apache Hadoop kümelerini geçirme

Bu makalede, Azure HDInsight için veri geçiş için öneriler sunar. Geçirme şirket içi Apache Hadoop sistemler ile Azure HDInsight için yardımcı olması için en iyi yöntemler sağlayan bir dizi gereksinimlerimizim bir parçasıdır.

## <a name="migrate-data-from-on-premises-to-azure"></a>Verileri şirket içinden Azure'a geçirme

Verileri şirket içinden Azure ortamına geçirmek için iki ana seçeneğiniz vardır:

1.  TLS ile ağ üzerinden veri aktarımı
    1.  İnternet üzerinden
    2.  Express Route
2.  Veri aktarma
    1.  İçeri / dışarı aktarma hizmeti
        - İç SATA HDD veya SSD yalnızca
        - BEKLEME sırasında şifrelenmiş (AES-128 / AES-256)
        - İçeri aktarma işi için 10 adede kadar disk barındırabilir
        - Kullanılabilir tüm genel bölgelerde & GA
    1.  Data Box
        - En fazla 80 TB veri kutusu başına veri
        - (AES-256) bekleme durumundayken şifrelenir
        - NAS protokollerini kullanır ve genel veri kopyalama araçlarını destekler
        - Sağlamlaştırılmış donanım
        - Yalnızca ABD & Genel önizlemede kullanılabilir

Aşağıdaki tabloda veri birimi ve ağ bant genişliğine göre yaklaşık veri aktarım süresi vardır. Bir veri kutusu veri geçişi üç hafta daha uzun sürmesi beklendiğinde kullanın.

|**Veri miktarı**|**Ağ bant genişliği**|||
|---|---|---|---|
|| **45 MB/sn (T3)**|**100 MB/sn**|**1 GB/sn**|**10 GB/sn**
|1 TB|2 gün|1 gün| 2 saat|14 dakika|
|10 TB|22 günü|10 gün|1 gün|2 saat|
|35 TB|76 gün|34 gün|3 gün|8 saat|
|80 TB|173 gün|78 gün|8 gün|19 saat|
|100 TB|216 gün|97 gün|10 gün|1 gün|
|200 TB|1 yıl|194 gün|19 gün|2 gün|
|500 TB|3 yıl|1 yıl|49 gün|5 gün|
|1 PB|6 yıl|3 yıl|97 gün|10 gün|
|2 PB|12 yıl|5 yıl|194 gün|19 gün|

Araçlar azure'a DistCp ve Azure Data Factory AzureCp, gibi yerel ağ üzerinden veri aktarmak için kullanılabilir. Üçüncü taraf aracı WANDisco, aynı amaçla da kullanılabilir. Kafka Mirrormaker ve Sqoop için Azure depolama sistemleri şirket içi devam eden veri aktarımı için kullanılabilir.

## <a name="performance-considerations-when-using-apache-distcp"></a>Apache DistCp kullanırken performans konuları

DistCp bir harita MapReduce işi veri aktarımı, hataları işleme ve bu hatalardan kurtarmak için kullandığı bir Apache projesidir. Kaynak dosyaların listesini her eşleme göreve atar. Harita görev sonra tüm atanan dosyaları hedefe kopyalar. Var olan çeşitli teknikler DistCp performansını geliştirebilir.

### <a name="increase-the-number-of-mappers"></a>Azaltıcının sayısını artırın

DistCp harita görevleri oluşturun, böylece her biri yaklaşık aynı sayıda bayt kopyalar dener. Varsayılan olarak 20 azaltıcının DistCp işleri kullanın. Daha fazla Azaltıcının için Distcp kullanma (ile 'M ' komut satırı parametresi) veri aktarım işlemi sırasında paralellik artar ve veri aktarımı uzunluğunu azaltır. Ancak, Azaltıcının sayısının artırılması sırasında dikkate alınması gereken iki şey vardır:

1. DistCp'ın en düşük ayrıntı düzeyi tek bir dosyadır. Kaynak dosyalarının sayısından daha fazla Azaltıcının sayısını belirten olmaz ve kullanılabilir küme kaynaklarının boşa harcanmasına neden.
1. Yarn bellek Azaltıcının sayısını belirlemek için küme üzerinde göz önünde bulundurun. Her harita görev Yarn kapsayıcı olarak başlatılır. Diğer ağır iş yükleri kümede çalışan varsayarak, aşağıdaki formülle Azaltıcının sayısı belirlenebilir: m = (çalışan düğümü sayısı \* her çalışan düğümü için YARN bellek) / YARN kapsayıcı boyutu. Diğer uygulamaları bellek kullanıyorsa, ancak yalnızca YARN belleğin bir kısmını DistCp işler için kullanmak üzere seçin.

### <a name="use-more-than-one-distcp-job"></a>Birden fazla DistCp iş kullanın

Taşınacak veri kümesi boyutu 1 TB'den büyük olduğunda, birden fazla DistCp işi kullanın. Birden fazla iş kullanarak hatalarının etkisini sınırlar. Herhangi bir işi başarısız olursa, yalnızca bu belirli iş yerine tüm işlerini yeniden başlatmanız gerekir.

### <a name="consider-splitting-files"></a>Dosyaları bölmeyi göz önünde bulundurun

Büyük dosyaların küçük bir sayı varsa, bunları daha fazla Azaltıcının ile daha fazla olası eşzamanlılık almak için 256 MB'lık dosya öbeklere bölmeyi düşünün.

### <a name="use-the-strategy-command-line-parameter"></a>'Stratejisi' komut satırı parametresini kullanın

Kullanmayı `strategy = dynamic` komut satırı parametresi. Varsayılan değer olan `strategy` parametresi `uniform size`, yaklaşık aynı bayt sayısı, bu durumda her eşleme kopyalar. Ne zaman bu parametre değiştiğinde `dynamic`, çeşitli "öbek-dosyalarına" listeleme dosyası bölünür. Haritalar sayısının katı öbek dosya sayısıdır. Her harita görev öbek dosyalarından birine atanır. Bir öbek tüm yolları işlendikten sonra geçerli yığında silinir ve yeni öbek alınır. İşlem, öbeği kullanılabilir olana kadar devam eder. "Dinamik" Bu yaklaşım, böylece DistCp işini genel hızlandırma daha yavaş yapılandırılanlardan daha yollarını kullanmak daha hızlı eşleme-görevleri sağlar.

### <a name="increase-the-number-of-threads"></a>İş parçacığı sayısını artırın

Artan olup `-numListstatusThreads` parametresi performansı artırır. Bu parametre, dosya listesi oluşturmak için kullanılacak iş parçacığı sayısını denetler ve 40 en yüksek değerdir.

### <a name="use-the-output-committer-algorithm"></a>Çıkış teslim eden algoritma kullanın

Parametre geçirme olup `-Dmapreduce.fileoutputcommitter.algorithm.version=2` DistCp performansını artırır. Bu çıkış teslim eden algoritma çıkış dosyaları hedefe yazma etrafında iyileştirmeler vardır. Aşağıdaki komut, farklı parametreler kullanımını gösteren bir örnek şudur:

```bash
hadoop distcp -Dmapreduce.fileoutputcommitter.algorithm.version=2 -numListstatusThreads 30 -m 100 -strategy dynamic hdfs://nn1:8020/foo/bar wasb://<container_name>@<storage_account_name>.blob.core.windows.net/foo/
```

## <a name="metadata-migration"></a>Meta veri geçişi

### <a name="hive"></a>Hive

Hive meta veri deposu, komut dosyalarını kullanarak veya veritabanı çoğaltma kullanılarak geçirilebilir.

#### <a name="hive-metastore-migration-using-scripts"></a>Hive meta veri deposu geçiş betiklerini kullanma

1. Hive DDLs şirket içi Hive meta veri deposu oluşturur. Bu adım yapılabilir [sarmalayıcı bash betiğini] kullanarak. (https://github.com/hdinsight/hdinsight.github.io/blob/master/hive/hive-export-import-metastore.md)
1. HDFS url ADLS/WASB/ABFS URL'leri ile değiştirme için oluşturulan DDL Düzenle
1. Güncelleştirilmiş DDL meta veri deposu HDInsight kümesinden çalıştırın.
1. Hive meta veri deposu sürümü şirket içi ile bulut arasında uyumlu olduğundan emin olun

#### <a name="hive-metastore-migration-using-db-replication"></a>Hive meta veri deposu geçiş DB çoğaltma kullanma

- Şirket içi Hive meta veri deposu DB ve HDInsight DB meta veri deposu arasında veritabanı çoğaltma işlemini ayarlama
- "Hive MetaTool" HDFS URL'si, örneğin ADLS/WASB/ABFS URL'ler ile değiştirmek için kullanın:

```bash
./hive --service metatool -updateLocation hdfs://nn1:8020/ wasb://<container_name>@<storage_account_name>.blob.core.windows.net/
```

### <a name="ranger"></a>Ranger

- Şirket içi Ranger ilkelerini xml dosyasına dışarı aktarın.
- Şirket içi belirli HDFS tabanlı yollara WASB/ADLS XSLT gibi bir araç kullanarak dönüştürün.
- HDInsight üzerinde çalışan Ranger oturum ilkeleri içeri aktarın.

## <a name="next-steps"></a>Sonraki adımlar

Bu serideki sonraki makaleyi okuyun:

- [Şirket içi-Azure HDInsight Hadoop geçiş için güvenlik ve DevOps en iyi uygulamalar](apache-hadoop-on-premises-migration-best-practices-security-devops.md)