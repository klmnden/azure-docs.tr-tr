---
title: Kullanma Apache Phoenix ve SQLLine Azure HDInsight içinde HBase ile
description: HDInsight Apache Phoenix kullanmayı öğrenin. Ayrıca, yükleme ve SQLLine HDInsight içinde HBase kümesi bağlanmak için bilgisayarınızda ayarlama konusunda bilgi edinin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/03/2018
ms.author: hrasheed
ms.openlocfilehash: 0bef586540635ee1bada3475f6316c84dea4308c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64722693"
---
# <a name="use-apache-phoenix-with-linux-based-apache-hbase-clusters-in-hdinsight"></a>HDInsight kümelerinde Linux tabanlı Apache HBase ile Apache Phoenix kullanma
Nasıl kullanacağınızı öğrenin [Apache Phoenix](https://phoenix.apache.org/) Azure HDInsight ve SQLLine kullanma. Phoenix hakkında daha fazla bilgi için bkz: [Apache Phoenix 15 dakika veya daha az](https://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Phoenix dilbilgisi için bkz: [Apache Phoenix Dilbilgisi](https://phoenix.apache.org/language/index.html).

> [!NOTE]  
> Phoenix sürüm HDInsight hakkında bilgi için [HDInsight tarafından sağlanan Apache Hadoop küme sürümlerindeki yenilikler](../hdinsight-component-versioning.md).
>
>

## <a name="use-sqlline"></a>SQLLine kullanma
[SQLLine](http://sqlline.sourceforge.net/) SQL yürütmek için bir komut satırı yardımcı programıdır.

### <a name="prerequisites"></a>Önkoşullar
SQLLine kullanabilmeniz için önce aşağıdaki öğelere sahip olmanız gerekir:

* **HDInsight, Apache HBase kümesi**. Oluşturmak için bkz: [HDInsight, Apache HBase kullanmaya başlama](./apache-hbase-tutorial-get-started-linux.md).

Bir HBase kümesi için bağlandığınızda birine bağlanması gereken [Apache ZooKeeper](https://zookeeper.apache.org/) VM'ler. Üç ZooKeeper Vm'leri her HDInsight kümesi vardır.

**ZooKeeper konak adını almak için**

1. Açık [Apache Ambari](https://ambari.apache.org/) atarak **https://\<küme adı\>. azurehdinsight.net**.
2. Oturum açmak için HTTP (küme) kullanıcı adını ve parolasını girin.
3. Sol menüde **ZooKeeper**. Üç **ZooKeeper sunucusu** örnekleri listelenir.
4. Birini **ZooKeeper sunucusu** örnekleri. Üzerinde **özeti** bölmesinde Bul **Hostname**. Benzer şekilde görünüyor *zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**SQLLine kullanmak için**

1. SSH kullanarak kümeye bağlanın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. SSH SQLLine çalıştırmak için aşağıdaki komutları kullanın:

        cd /usr/hdp/current/phoenix-client/bin
        ./sqlline.py <ZOOKEEPER SERVER FQDN>:2181:/hbase-unsecure
3. Bir HBase tablosu oluşturmayı ve bazı verileri eklemek için aşağıdaki komutları çalıştırın:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

Daha fazla bilgi için [SQLLine el ile](http://sqlline.sourceforge.net/#manual) ve [Apache Phoenix Dilbilgisi](https://phoenix.apache.org/language/index.html).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, HDInsight Apache Phoenix kullanmayı öğrendiniz. Daha fazla bilgi için şu makalelere bakın:

* [HDInsight Hbase'e genel bakış][hdinsight-hbase-overview].
  Apache HBase, büyük miktarlarda yapılandırmamış ve yarı yapılandırılmış veri için rastgele erişim ve güçlü tutarlılık sağlayan, Apache Hadoop üzerinde kurulu bir Apache, açık kaynaklı NoSQL veritabanıdır.
* [Azure sanal ağında sağlama Apache HBase kümeleri][hdinsight-hbase-provision-vnet].
  Uygulamaların Hbase'le doğrudan iletişim kurabilmesi sanal ağ ile tümleştirme, uygulamalarınızı, aynı sanal ağ için Apache HBase kümelerine dağıtılabilir.
* [HDInsight Apache HBase çoğaltmayı yapılandırma](apache-hbase-replication.md). İki Azure veri merkezleri arasında Apache HBase çoğaltma işlemini ayarlama konusunda bilgi edinin.


[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-hbase-provision-vnet]:apache-hbase-provision-vnet.md
[hdinsight-hbase-overview]:apache-hbase-overview.md


