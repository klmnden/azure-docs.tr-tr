---
title: HDInsight - Azure üzerinde HBase örneğiyle çalışmaya başlama
description: HDInsight'ta hadoop kullanmaya başlamak için bu Apache HBase örneğini izleyin. HBase kabuğundan tablolar oluşturun ve Hive kullanarak bunları sorgulayın.
keywords: hbase komutu,hbase örneği
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: hrasheed
ms.openlocfilehash: c1c582c60a7c91de40983daa07bdec1e8d748f8a
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64718866"
---
# <a name="get-started-with-an-apache-hbase-example-in-hdinsight"></a>HDInsight'ta Apache HBase örneğiyle çalışmaya başlama

Oluşturmayı bir [Apache HBase](https://hbase.apache.org/) küme HDInsight, HBase tabloları oluşturmak ve tabloları kullanarak sorgu [Apache Hive](https://hive.apache.org/).  Genel HBase bilgileri için bkz. [HDInsight HBase’e genel bakış][hdinsight-hbase-overview].

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu HBase örneğini denemeye başlamadan önce aşağıdakilere sahip olmanız gerekir:

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Secure Shell(SSH)](../hdinsight-hadoop-linux-use-ssh-unix.md). 
* [curl](https://curl.haxx.se/download.html).

## <a name="create-apache-hbase-cluster"></a>Apache HBase kümesi oluşturma
Aşağıdaki yordamda HBase kümesi ve bağlı varsayılan Azure Depolama hesabı oluşturmak için Azure Resource Manager şablonu kullanılmaktadır. Yordamda ve diğer küme oluşturma yöntemlerinde kullanılan parametreleri anlamak için bkz. [HDInsight’ta Linux tabanlı Hadoop kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md). Data Lake depolama Gen2 kullanma hakkında daha fazla bilgi için bkz. [hızlı başlangıç: HDInsight kümelerinde ayarlama](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).

1. Azure Portal'da bir şablonu açmak için aşağıdaki görüntüye tıklayın. Şablonuna [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/).
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. **Özel dağıtım** dikey penceresine şu değerleri girin:
   
   * **Abonelik**: Kümeyi oluşturmak için kullanılan Azure aboneliğinizi seçin.
   * **Kaynak grubu**: Bir Azure Resource management grubu oluşturun veya var olanı kullanın.
   * **Konum**: Kaynak grubu konumunu belirtin. 
   * **ClusterName**: HBase kümesi için bir ad girin.
   * **Küme oturum açma adı ve parola**: Varsayılan oturum açma adı **admin**’dir.
   * **SSH kullanıcı adı ve parola**: Varsayılan kullanıcı adı **sshuser** şeklindedir.  Bunu yeniden adlandırabilirsiniz.
     
     Diğer parametreler isteğe bağlıdır.  
     
     Her kümenin bir Azure Depolama hesabı bağımlılığı vardır. Bir küme silindikten sonra veriler depolama hesabında saklanır. Kümenin varsayılan depolama hesabı adı, "depo" ifadesi eklenmiş küme adıdır. Şablon değişkenleri bölümüne sabit kodlanır.
3. **Yukarıdaki hüküm ve koşulları kabul ediyorum**’u seçip **Satın al**’a tıklayın. Bir küme oluşturmak yaklaşık 20 dakika sürer.

> [!NOTE]  
> Bir HBase kümesi silindikten sonra aynı varsayılan blob kapsayıcısını kullanarak başka bir HBase kümesi oluşturabilirsiniz. Yeni küme özgün kümede oluşturduğunuz HBase tablolarını seçer. Tutarsızlıkları önlemek için kümeyi silmeden önce HBase tablolarını devre dışı bırakmanız önerilir.
> 
> 

## <a name="create-tables-and-insert-data"></a>Tablo oluşturma ve veri ekleme
HBase kümelerine bağlanmak ve daha sonra kullanmak için SSH kullanabilirsiniz [Apache HBase Kabuğu](https://hbase.apache.org/0.94/book/shell.html) HBase tabloları oluşturmak için veri ve sorgu veri ekleyin. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

Çoğu kişi için veriler tablo biçiminde görünür:

![HDInsight HBase tablo verileri][img-hbase-sample-data-tabular]

HBase içinde (uygulaması [bulut BigTable](https://cloud.google.com/bigtable/)), aynı veriler şu gibi görünür:

![HDInsight HBase BigTable verileri][img-hbase-sample-data-bigtable]


**HBase kabuğunu kullanmak için**

1. SSH'den aşağıdaki HBase komutu çalıştırın:
   
    ```bash
    hbase shell
    ```

2. İki sütun ailesi ile bir HBase oluşturun:

    ```hbaseshell   
    create 'Contacts', 'Personal', 'Office'
    list
    ```
3. Bazı verileri ekleyin:
    
    ```hbaseshell   
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    scan 'Contacts'
    ```
   
    ![HDInsight Hadoop HBase kabuğu][img-hbase-shell]
4. Tek bir satır alın
   
    ```hbaseshell
    get 'Contacts', '1000'
    ```
   
    Yalnızca bir satır olduğundan tarama komutunu kullanmanızla aynı sonuçları görürsünüz.
   
    HBase tablo şeması hakkında daha fazla bilgi için bkz. [Apache HBase şema tasarımına giriş][hbase-schema]. HBase komutları hakkında daha fazla bilgi için bkz. [Apache HBase başvuru kılavuzu][hbase-quick-start].
5. Kabuktan çıkış yapma
   
    ```hbaseshell
    exit
    ```

**Verileri kişi HBase tablosuna toplu olarak yüklemek için**

HBase’de verileri tablolara yüklemek için bazı yöntemler vardır.  Daha fazla bilgi için bkz. [Toplu yükleme](https://hbase.apache.org/book.html#arch.bulk.load).

Örnek veri dosyası, bir ortak blob kapsayıcısında bulunabilir *wasb://hbasecontacts\@hditutorialdata.blob.core.windows.net/contacts.txt*.  Veri dosyasının içeriği şudur:

    8396    Calvin Raji      230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu         646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie         508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson     674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller    397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile     592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee        870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes       599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander  670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander    998-555-0171    230-555-0200    771 Northridge Drive

İsterseniz, bir metin dosyası oluşturabilir ve dosyayı kendi depolama hesabınıza yükleyebilirsiniz. Yönergeler için bkz. [HDInsight Apache Hadoop işleri için verileri karşıya yükleme][hdinsight-upload-data].

> [!NOTE]  
> Bu yordam son yordamda oluşturduğunuz Kişiler HBase tablosunu kullanır.

1. SSH’de, veri dosyalarını StoreFiles’a dönüştürmek ve Dimporttsv.bulk.output tarafından belirtilen göreli bir yola depolamak için aşağıdaki komutu çalıştırın.  HBase Kabuğu'ndan çıkış yapmak için çıkış komutunu kullanın.

    ```bash   
    hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
    ```

2. Verileri /example/data/storeDataFileOutput konumundan HBase tablosuna yüklemek için aşağıdaki komutu çalıştırın:
   
    ```bash
    hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
    ```

3. HBase kabuğunu açabilir ve tarama komutunu kullanarak tablo içeriğini listeleyebilirsiniz.

## <a name="use-apache-hive-to-query-apache-hbase"></a>Apache Hive, Apache HBase sorgulama kullanın

Kullanarak HBase tablolarındaki verileri sorgulayabilirsiniz [Apache Hive](https://hive.apache.org/). Bu bölümde HBase tablosuyla eşlenen bir Hive tablosu oluşturur ve HBase tablosunda verileri sorgulamak için kullanırsınız.

1. **PuTTY** uygulamasını açın ve kümeye bağlanın.  Önceki yordamda bulunan yönergelere bakın.
2. Beeline’ı başlatmak için SSH oturumunda aşağıdaki komutu kullanın:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    Beeline hakkında daha fazla bilgi için bkz. [Beeline ile HDInsight’ta Hadoop ile Hive kullanma](../hadoop/apache-hadoop-use-hive-beeline.md).
       
3. Aşağıdaki komutu çalıştırın [HiveQL](https://cwiki.apache.org/confluence/display/Hive/LanguageManual) HBase tablosuyla eşlenen bir Hive tablosu oluşturmak için komut dosyası. Bu deyimi çalıştırmadan önce HBase kabuğunu kullanarak bu öğreticinin daha önceki bölümlerinde başvurulan örnek tablosunu oluşturduğunuzdan emin olun.

    ```hiveql   
    CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
    STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
    WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
    TBLPROPERTIES ('hbase.table.name' = 'Contacts');
    ```

4. HBase tablosundaki verileri sorgulamak için aşağıdaki HiveQL betiğini çalıştırın:

    ```hiveql   
    SELECT count(rowkey) FROM hbasecontacts;
    ```

## <a name="use-hbase-rest-apis-using-curl"></a>Curl kullanarak HBase REST API’lerini kullanma

REST API’sinin güvenliği [temel kimlik doğrulaması](https://en.wikipedia.org/wiki/Basic_access_authentication) ile sağlanır. Kimlik bilgilerinizin sunucuya güvenli bir şekilde gönderilmesi için istekleri her zaman Güvenli HTTP (HTTPS) kullanarak yapmalısınız.

1. Mevcut HBase tablolarını listelemek için şu komutu kullanın:

    ```bash
    curl -u <UserName>:<Password> \
    -G https://<ClusterName>.azurehdinsight.net/hbaserest/
    ```

1. İki sütunlu aileler içeren yeni bir HBase tablosu oluşturmak için aşağıdaki komutu kullanın:

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
    -v
    ```

    Şema JSon biçiminde sağlanır.
1. Bazı verileri eklemek için aşağıdaki komutu kullanın:

    ```bash   
    curl -u <UserName>:<Password> \
    -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d "{\"Row\":[{\"key\":\"MTAwMA==\",\"Cell\": [{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}]}]}" \
    -v
    ```
   
    -d anahtarında belirtilen değerleri base64 ile kodlamanız gerekir. Örnekte:
   
   * MTAwMA==: 1000
   * UGVyc29uYWw6TmFtZQ==: Kişisel: adı
   * Sm9obiBEb2xl: John Dole
     
     [false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) birden fazla (toplu) değer eklemenizi sağlar.
1. Bir satır almak için aşağıdaki komutu kullanın:
   
    ```bash 
    curl -u <UserName>:<Password> \
    -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
    -H "Accept: application/json" \
    -v
    ```

HBase Rest hakkında daha fazla bilgi için bkz. [Apache HBase Başvuru Kılavuzu](https://hbase.apache.org/book.html#_rest).

> [!NOTE]  
> Thrift, HDInsight’ta HBase tarafından desteklenmez.
>
> Curl’ü veya WebHCat ile başka bir REST iletişimini kullanırken HDInsight küme yöneticisinin kullanıcı adı ve parolasını sağlayarak isteklerin kimliğini doğrulamanız gerekir. Ayrıca, sunucuya istek göndermek için kullanılan Tekdüzen Kaynak Tanımlayıcısı’nın (URI) bir parçası olarak küme adını kullanmanız gerekir:
> 
>   
>        curl -u <UserName>:<Password> \
>        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status
>   
>    Aşağıdakine benzer bir yanıt almanız gerekir:
>   
>        {"status":"ok","version":"v1"}
   


## <a name="check-cluster-status"></a>Küme durumunu denetleme
HDInsight içinde HBase, kümelerin izlenmesi için bir Web Kullanıcı Arabirimi ile birlikte gönderilir. Web Kullanıcı Arabirimini kullanarak istatistikler veya bölgeler hakkında bilgi isteyebilirsiniz.

**HBase Master Kullanıcı Arabirimi’ne erişmek için**

1. Ambari Web kullanıcı Arabirimi bir oturum sırasında https://&lt;Clustername >. azurehdinsight.net.
2. Soldaki menüden **HBase**’e tıklayın.
3. Sayfanın üstündeki **Hızlı bağlantılar**’a tıklayın, etkin Zookeeper düğümü bağlantısının üzerine gelin ve **HBase Master Kullanıcı Arabirimi**’ne tıklayın.  Kullanıcı arabirimi başka bir tarayıcı sekmesinde açılır:

   ![HDInsight HBase HMaster Kullanıcı Arabirimi](./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-hmaster-ui.png)

   HBase Master Kullanıcı Arabirimi aşağıdaki bölümleri içerir:

   - Bölge sunucuları
   - Yedekleme yöneticileri
   - Tablolar
   - Görevler
   - Yazılım öznitelikleri

## <a name="delete-the-cluster"></a>Küme silme
Tutarsızlıkları önlemek için kümeyi silmeden önce HBase tablolarını devre dışı bırakmanız önerilir.

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Sorun giderme

HDInsight kümeleri oluştururken sorun yaşarsanız bkz. [erişim denetimi gereksinimleri](../hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir Apache HBase kümesi oluşturma ve tabloları oluşturma ve verileri tabloların HBase kabuğundan görüntülemeyi öğrendiniz. Ayrıca HBase tablolarındaki veriler üzerinde bir Hive sorgusu kullanmayı, HBase C# REST API’lerini kullanarak bir HBase tablosu oluşturmayı ve tablodan veri almayı öğrendiniz.

Daha fazla bilgi için bkz:

* [HDInsight Hbase'e genel bakış][hdinsight-hbase-overview]: Apache HBase, büyük miktarlarda yapılandırmamış ve yarı yapılandırılmış veri için rastgele erişim ve güçlü tutarlılık sağlayan, Apache Hadoop üzerinde kurulu bir Apache, açık kaynaklı NoSQL veritabanıdır.

[hdinsight-upload-data]: ../hdinsight-upload-data.md
[hbase-reference]: https://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: https://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]:apache-hbase-overview.md
[hdinsight-hbase-provision-vnet]:apache-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: https://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hbase-shell]: ./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/apache-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png
