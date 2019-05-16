---
title: 'Hızlı Başlangıç: Sorgu Apache HBase, HBase Kabuğu olan Azure HDInsight-'
description: Apache HBase sorguları çalıştırmak için Apache HBase Kabuğu'nu kullanmayı öğrenin.
keywords: hdınsight, hadoop, HBase
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: quickstart
ms.date: 05/08/2019
ms.author: hrasheed
ms.openlocfilehash: 41b16e63522a02cc16eb4dac2cbcc8e6540aceaf
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65552089"
---
# <a name="quickstart-query-apache-hbase-in-azure-hdinsight-with-hbase-shell"></a>Hızlı Başlangıç: Azure HDInsight, sorgu Apache HBase ile HBase Kabuğu

Bu hızlı başlangıçta, bir HBase tablosu oluşturmayı, veri eklemek ve ardından tabloyu sorgulamak için Apache HBase Kabuğu'nu kullanmayı öğrenin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Bir Apache HBase kümesi. Bkz: [küme oluştur](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) bir HDInsight kümesi oluşturma.  Seçtiğiniz olun **HBase** küme türü.

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight (Apache Hadoop) bağlanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-a-table-and-manipulate-data"></a>Bir tablo oluşturun ve veri işleme

Çoğu kişi için veriler tablo biçiminde görünür:

![HDInsight HBase tablo verileri](./media/query-hbase-with-hbase-shell/hdinsight-hbase-contacts-tabular.png)

HBase içinde (uygulaması [bulut BigTable](https://cloud.google.com/bigtable/)), aynı veriler şu gibi görünür:

![HDInsight HBase BigTable verileri](./media/query-hbase-with-hbase-shell/hdinsight-hbase-contacts-bigtable.png)

HBase kümelerine bağlanmak ve Apache HBase Kabuğu kullanarak HBase tabloları oluşturmak, eklemek ve verileri sorgulamak için SSH kullanabilirsiniz.

1. Kullanım `ssh` HBase kümenize bağlanmak için komutu. Değiştirerek aşağıdaki komutu düzenleyin `CLUSTERNAME` , kümenizin adını ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Kullanım `hbase shell` HBase etkileşimli Kabuk başlatmak için komutu. SSH bağlantınızı aşağıdaki komutu girin:

    ```bash
    hbase shell
    ```

3. Kullanım `create` iki sütun ailesi ile bir HBase tablosu oluşturmak için komutu. Aşağıdaki komutu girin:

    ```hbase
    create 'Contacts', 'Personal', 'Office'
    ```

4. Kullanım `list` HBase tüm tablolarda listelemek için komutu. Aşağıdaki komutu girin:

    ```hbase
    list
    ```

5. Kullanım `put` değerleri belirtilen bir sütunda belirli bir tablodaki belirli bir satır eklemek için komutu. Aşağıdaki komutu girin:

    ```hbase
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    ```

6. Kullanım `scan` tarama ve döndürmek için komut `Contacts` tablo verileri. Aşağıdaki komutu girin:

    ```hbase
    scan 'Contacts'
    ```

7. Kullanım `get` komut satırının içeriği getirilemedi. Aşağıdaki komutu girin:

    ```hbase
    get 'Contacts', '1000'
    ```

    Kullanılarak benzer sonuçlar görürsünüz `scan` yalnızca bir satır olduğundan komutu.

8. Kullanım `delete` bir hücre değerini bir tablodaki silmek için komutu. Aşağıdaki komutu girin:

    ```hbase
    delete 'Contacts', '1000', 'Office:Address'
    ```

9. Kullanım `disable` tablo devre dışı bırakma komutu. Aşağıdaki komutu girin:

    ```hbase
    disable 'Contacts'
    ```

10. Kullanım `drop` HBase tablo drop komutu. Aşağıdaki komutu girin:

    ```hbase
    drop 'Contacts'
    ```

11. Kullanım `exit` HBase etkileşimli Kabuk durdurmak için komutu. Aşağıdaki komutu girin:

    ```hbase
    exit
    ```

HBase tablo şeması hakkında daha fazla bilgi için bkz. [Apache HBase şema tasarımına giriş](http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf). Daha fazla HBase komutları için bkz. [Apache HBase Başvuru Kılavuzu](https://hbase.apache.org/book.html#quickstart).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlangıcı tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır.

Bir kümeyi silmek için bkz: [tarayıcınız, PowerShell veya Azure CLI kullanarak bir HDInsight kümesi silme](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir HBase tablosu oluşturmayı, veri eklemek ve ardından tabloyu sorgulamak üzere Apache HBase Kabuğu kullanarak öğrendiniz. Hbase'de depolanan veriler hakkında daha fazla bilgi edinmek için sonraki makaleye, Apache Spark ile sorguları yürütüp yapmayı gösterir.

> [!div class="nextstepaction"]
> [Apache HBase veri okuma ve yazma için Apache Spark'ı kullanma](../hdinsight-using-spark-query-hbase.md)