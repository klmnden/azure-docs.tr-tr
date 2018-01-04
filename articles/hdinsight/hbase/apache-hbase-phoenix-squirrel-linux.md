---
title: "Apache kullanmak Phoenix ve Azure hdınsight'ta HBase ile SQLLine | Microsoft Docs"
description: "Hdınsight'ta Apache Phoenix kullanmayı öğrenin. Ayrıca, yüklemek ve SQLLine hdınsight'ta HBase kümesi bağlanmak için bilgisayarınızda ayarlamak öğrenin."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: cda0f33b-a2e8-494c-972f-ae0bb482b818
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/22/2017
ms.author: jgao
ms.openlocfilehash: 70f8786bae555456dd019ad76bda974667cec5ba
ms.sourcegitcommit: 234c397676d8d7ba3b5ab9fe4cb6724b60cb7d25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="use-apache-phoenix-with-linux-based-hbase-clusters-in-hdinsight"></a>Hdınsight'ta Linux tabanlı HBase kümeleriyle Apache Phoenix kullanın
Nasıl kullanacağınızı öğrenin [Apache Phoenix](http://phoenix.apache.org/) Azure Hdınsight ve SQLLine kullanma. Phoenix hakkında daha fazla bilgi için bkz: [Phoenix 15 dakika veya daha az](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Phoenix dilbilgisi için bkz: [Phoenix Dilbilgisi](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> Phoenix sürüm Hdınsight hakkında bilgi için [Hdınsight tarafından sağlanan Hadoop küme sürümlerindeki yenilikler](../hdinsight-component-versioning.md).
>
>

## <a name="use-sqlline"></a>SQLLine kullanın
[SQLLine](http://sqlline.sourceforge.net/) SQL yürütülecek bir komut satırı yardımcı programıdır.

### <a name="prerequisites"></a>Önkoşullar
SQLLine kullanmadan önce aşağıdaki öğeleri sahip olmanız gerekir:

* **Hdınsight'ta HBase kümesi**. Oluşturmak için bkz: [hdınsight'ta Apache HBase kullanmaya başlama](./apache-hbase-tutorial-get-started-linux.md).

Bir HBase kümesi bağlandığınızda, ZooKeeper VM'ler birine bağlanmanız gerekir. Her Hdınsight kümesi üç ZooKeeper VM'ler sahiptir.

**ZooKeeper ana bilgisayar adını almak için**

1. Açık Ambari göz atarak **https://\<küme adı\>. azurehdinsight.net**.
2. Oturum açmak için HTTP (küme) kullanıcı adını ve parolasını girin.
3. Soldaki menüde seçin **ZooKeeper**. Üç **ZooKeeper sunucusu** örnekleri listelenir.
4. Aşağıdakilerden birini seçin **ZooKeeper sunucusu** örnekleri. Üzerinde **Özet** bölmesinde Bul **ana bilgisayar adı**. İçin benzer *zk1 jdolehb.3lnng4rcvp5uzokyktxs4a5dhd.bx.internal.cloudapp.net*.

**SQLLine kullanmak için**

1. SSH kullanarak kümeye bağlanın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. SSH içinde SQLLine çalıştırmak için aşağıdaki komutları kullanın:

        cd /usr/hdp/2.2.9.1-7/phoenix/bin
        ./sqlline.py <ZOOKEEPER SERVER FQDN>:2181:/hbase-unsecure
3. Bir HBase tablosu oluşturmayı ve bazı verileri eklemek için aşağıdaki komutları çalıştırın:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

        !quit

Daha fazla bilgi için bkz: [SQLLine el ile](http://sqlline.sourceforge.net/#manual) ve [Phoenix Dilbilgisi](http://phoenix.apache.org/language/index.html).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Hdınsight'ta Apache Phoenix kullanma hakkında bilgi edindiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hdınsight Hbase'e genel bakış][hdinsight-hbase-overview].
  HBase, büyük miktarlarda yapılandırılmamış ve yarı yapılandırılmış veriler için rasgele erişim ve güçlü tutarlılık sağlayan hadoop'ta yerleşik bir Apache, açık kaynak, NoSQL veritabanıdır.
* [Azure Virtual Network HBase kümelerine sağlamak][hdinsight-hbase-provision-vnet].
  Uygulamalar HBase ile doğrudan iletişim kurabilmesi ile sanal ağ tümleştirme, uygulamalarınızı, aynı sanal ağ için HBase kümelerine dağıtılabilir.
* [Hdınsight'ta HBase çoğaltmayı yapılandırma](apache-hbase-replication.md). İki Azure veri merkezi arasında HBase çoğaltmayı ayarlama ayarlanacağını öğrenin.


[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]:apache-hbase-provision-vnet.md
[hdinsight-hbase-overview]:apache-hbase-overview.md


