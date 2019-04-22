---
title: Azure HDInsight için sürüm notları
description: Azure HDInsight için en son sürüm notları. Hadoop, Spark, R Server, Hive ve daha fazla bilgi için geliştirme ipuçları ve ayrıntıları alın.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/15/2019
ms.openlocfilehash: 5769f90ef69a82497194ff6de01b378acc84deec
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59678403"
---
# <a name="release-notes-for-azure-hdinsight"></a>Azure HDInsight için sürüm notları

Bu makalede, hakkında bilgi sağlar. **en son** Azure HDInsight sürüm güncelleştirmeleri. Önceki sürümler hakkında daha fazla bilgi için bkz: [HDInsight sürüm notları arşiv](hdinsight-release-notes-archive.md).

> [!IMPORTANT]  
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için [HDInsight sürüm makale](hdinsight-component-versioning.md).

## <a name="summary"></a>Özet

Azure HDInsight, Azure üzerinde açık kaynaklı Apache Hadoop ve Apache Spark analiz için en popüler hizmetler Kurumsal müşteriler arasında biridir.

## <a name="new-features"></a>Yeni Özellikler

HDInsight 4.0 ile önemli değişiklikler hakkında daha fazla bilgi için., bkz: [HDI 4. 0'yenilikler nelerdir?](../hdinsight/hdinsight-version-release.md).

## <a name="component-versions"></a>Bileşen sürümü

Tüm HDInsight 4.0 bileşenlerin resmi Apache sürümlerini aşağıda verilmiştir. Listelenen bileşenleri kullanılabilir en son kararlı sürüm sürümlerdir.

- Apache Ambari 2.7.1
- Apache Hadoop 3.1.1
- Apache HBase 2.0.0
- Apache Hive 3.1.0
- Apache Kafka 1.1.1
- Apache Mahout 0.9.0+
- Apache Oozie 4.2.0
- Apache Phoenix 4.7.0
- Apache Pig 0.16.0
- Apache Ranger 0.7.0
- Apache kaydırıcı 0.92.0
- Apache Spark 2.3.1
- Apache Sqoop 1.4.7
- Apache TEZ 0.9.1
- Apache Zeppelin 0.8.0
- Apache ZooKeeper 3.4.6

Apache bileşenlerin sonraki sürümlerini bazen HDP dağıtımında yukarıda listelenen sürümlere ek olarak paketlenir. Bu durumda, bu sonraki sürümleri Technical Preview sürümleri tabloda listelenen ve bir üretim ortamında Yukarıdaki listeye Apache bileşen sürümleri yerine değil.

## <a name="apache-patch-information"></a>Apache düzeltme bilgileri

Aşağıdaki tabloda her ürün için listeleme düzeltme eki HDInsight 4. 0 ' bir düzeltme hakkında daha fazla bilgi için bkz.

| Ürün adı | Düzeltme eki bilgileri |
|---|---|
| Ambari | [Ambari düzeltme bilgileri](https://docs.hortonworks.com/HDPDocuments/Ambari-2.7.1.0/bk_ambari-release-notes/content/ambari_relnotes-2.7.1.0-patch-information.html) |
| Hadoop | [Hadoop düzeltme bilgileri](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_hadoop.html) |
| HBase | [HBase düzeltme bilgileri](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_hbase.html) |
| Hive  | Bu sürümde Hive ile ek bir Apache düzeltme eklerini 3.1.0 sağlar.  |
| Kafka | Bu sürüm, Kafka ile ek bir Apache düzeltme eklerini 1.1.1 sağlar. |
| Oozie | [Oozie düzeltme bilgileri](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_oozie.html) |
| Phoenix | [Phoenix düzeltme bilgileri](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_phoenix.html) |
| Pig | [Pig düzeltme bilgileri](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_pig.html) |
| Ranger | [Ranger düzeltme bilgileri](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_ranger.html) |
| Spark | [Spark düzeltme bilgileri](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_spark.html) |
| Sqoop | Bu sürüm ile ek bir Apache düzeltme eklerini 1.4.7 Sqoop sağlar. |
| Tez | Bu sürümde, Tez 0.9.1 ek bir Apache düzeltme ekleri ile sağlar. |
| Zeppelin | Bu sürüm ile ek bir Apache düzeltme eklerini 0.8.0 Zeppelin sağlar. |
| Zookeeper | [Zookeeper düzeltme bilgileri](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/patch_zookeeper.html) |

## <a name="fixed-common-vulnerabilities-and-exposures"></a>Yaygın güvenlik açıklarına ve exposures'ı sabit

Güvenlik hakkında daha fazla bilgi için bu çözümlenen sorunlar sürüm, Hortonworks bkz [sabit yaygın güvenlik açıklarına and exposures'ı için HDP 3.0.1](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.1/release-notes/content/cve.html).

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="replication-is-broken-for-secure-hbase-with-default-installation"></a>Çoğaltma için HBase güvenli varsayılan yüklemesiyle bozuk

HDInsight 4.0 için aşağıdaki adımları uygulayın:

1. Kümeler arası iletişimi etkinleştirin.
1. Etkin baş düğüme oturum açın.
1. Aşağıdaki komutla çoğaltmayı etkinleştirmek için bir betik indirin:

    ```
    sudo wget https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh
    ```
1. Komuttan `sudo kinit <domainuser>`.
1. Betiği çalıştırmak için aşağıdaki komutu yazın:

    ```
    sudo bash hdi_enable_replication.sh -m <hn0> -s <srclusterdns> -d <dstclusterdns> -sp <srcclusterpasswd> -dp <dstclusterpasswd> -copydata
    ```
HDInsight 3.6 için aşağıdakileri yapın:

1. Etkin HMaster ZK için oturum açın.
1. Aşağıdaki komutla çoğaltmayı etkinleştirmek için bir betik indirin:
    ```
    sudo wget https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh
    ```
1. Komuttan `sudo kinit -k -t /etc/security/keytabs/hbase.service.keytab hbase/<FQDN>@<DOMAIN>`.
1. Aşağıdaki komutu yazın:

    ```bash
    sudo bash hdi_enable_replication.sh -s <srclusterdns> -d <dstclusterdns> -sp <srcclusterpasswd> -dp <dstclusterpasswd> -copydata
    ```

### <a name="phoenix-sqlline-stops-working-after-migrating-hbase-cluster-to-hdinsight-40"></a>Phoenix ve Sqlline geçirdikten sonra çalışmayı durdurur 4.0 HDInsight için HBase kümesi

Aşağıdaki adımları uygulayın:

1. Aşağıdaki tablolarda Phoenix bırakın:
    1. `SYSTEM.FUNCTION`
    1. `SYSTEM.SEQUENCE`
    1. `SYSTEM.STATS`
    1. `SYSTEM.MUTEX`
    1. `SYSTEM.CATALOG`
1. Tabloların silemiyorsanız, HBase tabloları bağlantılarına temizlemek için yeniden başlatın.
1. `sqlline.py` komutunu yeniden çalıştırın. Phoenix tüm 1. adımda silinen tablolarını yeniden oluşturur.
1. Phoenix tabloları ve görünümleri HBase verileriniz için yeniden oluşturun.

### <a name="phoenix-sqlline-stops-working-after-replicating-hbase-phoenix-metadata-from-hdinsight-36-to-40"></a>Phoenix ve Sqlline 4.0 HDInsight 3.6 HBase Phoenix meta veri çoğaltma sonra çalışmayı durduruyor.

Aşağıdaki adımları uygulayın:

1. Çoğaltma yaptığınızda önce hedef 4.0 kümeye gidin ve yürütme `sqlline.py`. Bu komut, Phoenix tablolar gibi oluşturacak `SYSTEM.MUTEX` ve `SYSTEM.LOG` 4. 0'yalnızca mevcut.
1. Aşağıdaki tablolarda bırakın:
    1. `SYSTEM.FUNCTION`
    1. `SYSTEM.SEQUENCE`
    1. `SYSTEM.STATS`
    1. `SYSTEM.CATALOG`
1. HBase çoğaltmayı Başlat

## <a name="deprecation"></a>Kullanımdan kaldırma

Apache Storm ve ML Hizmetleri, HDInsight 4. 0'kullanılamaz.
