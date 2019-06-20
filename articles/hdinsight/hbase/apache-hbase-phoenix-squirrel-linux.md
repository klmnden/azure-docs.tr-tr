---
title: 'Hızlı Başlangıç: Azure HDInsight - Apache Phoenix sorgu Apache HBase'
description: Bu hızlı başlangıçta, HDInsight Apache Phoenix kullanmayı öğrenin. Ayrıca, yükleme ve SQLLine HDInsight içinde HBase kümesi bağlanmak için bilgisayarınızda ayarlama konusunda bilgi edinin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: quickstart
ms.date: 06/12/2019
ms.author: hrasheed
ms.openlocfilehash: 20af6d32d03ae5d4fe37b1a37198ef1f2c50ec95
ms.sourcegitcommit: e5dcf12763af358f24e73b9f89ff4088ac63c6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67137415"
---
# <a name="quickstart-query-apache-hbase-in-azure-hdinsight-with-apache-phoenix"></a>Hızlı Başlangıç: Apache Phoenix ile Azure HDInsight, Apache HBase sorgu

Bu hızlı başlangıçta, Azure HDInsight içinde HBase sorguları çalıştırmak için Apache Phoenix kullanmayı öğrenin. Apache Phoenix, Apache HBase için bir SQL sorgusu altyapısıdır. Buna JDBC sürücüsü olarak erişilir ve bu SQL kullanarak HBase tablolarını sorgulamayı ve yönetmeyi sağlar. [SQLLine](http://sqlline.sourceforge.net/) SQL yürütmek için bir komut satırı yardımcı programıdır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Bir Apache HBase kümesi. Bkz: [küme oluştur](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) bir HDInsight kümesi oluşturma.  Seçtiğiniz olun **HBase** küme türü.

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="identify-a-zookeeper-node"></a>ZooKeeper düğümü belirle

Bir HBase kümesi için bağlandığınızda, Apache ZooKeeper düğümleri birine bağlanması gerekir. HDInsight kümesi her üç ZooKeeper düğümü vardır. Curl ZooKeeper düğümü hızlı bir şekilde tanımlamak için kullanılabilir. Değiştirerek aşağıdaki curl komutunu düzenleyin `PASSWORD` ve `CLUSTERNAME` ilgili değerleri içeren ve ardından bir komut istemi'nde komutu girin:

```cmd
curl -u admin:PASSWORD -sS -G https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER
```

Çıktı bir kısmını benzer şekilde görünür:

```output
    {
      "href" : "http://hn1-brim.432dc3rlshou3ocf251eycoapa.bx.internal.cloudapp.net:8080/api/v1/clusters/myCluster/hosts/zk0-brim.432dc3rlshou3ocf251eycoapa.bx.internal.cloudapp.net/host_components/ZOOKEEPER_SERVER",
      "HostRoles" : {
        "cluster_name" : "myCluster",
        "component_name" : "ZOOKEEPER_SERVER",
        "host_name" : "zk0-brim.432dc3rlshou3ocf251eycoapa.bx.internal.cloudapp.net"
      }
```

Değeri Not `host_name` daha sonra kullanmak için.

## <a name="create-a-table-and-manipulate-data"></a>Bir tablo oluşturun ve veri işleme

HBase kümelerine bağlanmak için SSH kullanın ve Apache Phoenix kullanarak HBase tabloları oluşturmak, eklemek ve verileri sorgulamak için.

1. Kullanım `ssh` HBase kümenize bağlanmak için komutu. Değiştirerek aşağıdaki komutu düzenleyin `CLUSTERNAME` , kümenizin adını ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Phoenix istemciye dizini değiştirin. Aşağıdaki komutu girin:

    ```bash
    cd /usr/hdp/current/phoenix-client/bin
    ```

3. Başlatma [SQLLine](http://sqlline.sourceforge.net/). Aşağıdaki komutta değiştirerek Düzenle `ZOOKEEPER` daha önce tanımlanan ZooKeeper düğümü ile sonra komutu girin:

    ```bash
    ./sqlline.py ZOOKEEPER:2181:/hbase-unsecure
    ```

4. Bir HBase tablosu oluşturun. Aşağıdaki komutu girin:

    ```sql
    CREATE TABLE Company (company_id INTEGER PRIMARY KEY, name VARCHAR(225));
    ```

5. SQLLine kullanma `!tables` HBase tüm tablolarda listelemek için komutu. Aşağıdaki komutu girin:

    ```sqlline
    !tables
    ```

6. Tablodaki değerleri ekleyin. Aşağıdaki komutu girin:

    ```sql
    UPSERT INTO Company VALUES(1, 'Microsoft');
    UPSERT INTO Company VALUES(2, 'Apache');
    ```

7. Tablo sorgusu. Aşağıdaki komutu girin:

    ```sql
    SELECT * FROM Company;
    ```

8. Bir kaydı silin. Aşağıdaki komutu girin:

    ```sql
    DELETE FROM Company WHERE COMPANY_ID=1;
    ```

9. Tabloyu bırakabilirsiniz. Aşağıdaki komutu girin:

    ```hbase
    DROP TABLE Company;
    ```

10. SQLLine kullanma `!quit` SQLLine çıkmak için komutu. Aşağıdaki komutu girin:

    ```sqlline
    !quit
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlangıcı tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

Bir kümeyi silmek için bkz: [tarayıcınız, PowerShell veya Azure CLI kullanarak bir HDInsight kümesi silme](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure HDInsight içinde HBase sorguları çalıştırmak için Apache Phoenix kullanmayı öğrendiniz. Apache Phoenix hakkında daha fazla bilgi edinmek için sonraki makaleye daha derin bir incelemesini sağlar.

> [!div class="nextstepaction"]
> [HDInsight üzerinde Apache Phoenix](../hdinsight-phoenix-in-hdinsight.md)
