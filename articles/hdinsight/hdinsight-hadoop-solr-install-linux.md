---
title: Linux tabanlı HDInsight üzerinde - Azure Solr yüklemek üzere betik eylemi kullanın
description: Betik eylemlerini kullanarak Linux tabanlı HDInsight Hadoop kümelerinde Solr yüklemeyi öğrenin.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: 3500a29c1cdd8b1997f67a3cf1918090dc4ca812
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53383604"
---
# <a name="install-and-use-apache-solr-on-hdinsight-hadoop-clusters"></a>Yükleme ve HDInsight Hadoop kümeleri üzerinde Apache Solr kullanma

Betik eylemi kullanarak Azure HDInsight üzerinde Apache Solr yüklemeyi öğrenin. Solr Hadoop tarafından yönetilen veri çubuğunda Kurumsal düzeyde arama özellikleri sağlar ve bir güçlü arama platformudur.

> [!IMPORTANT]  
> Bu belgedeki adımlar, Linux kullanan bir HDInsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

> [!IMPORTANT]  
> Bu belgede kullanılan örnek betik, belirli bir yapılandırmayla Solr 4.9 yükler. Farklı koleksiyonlar, parçalar, şemalar, çoğaltmalar, vb. ile Solr kümesini yapılandırmak, Solr ikili dosyaları ve komut dosyasını değiştirmeniz gerekir.

## <a name="whatis"></a>Solr nedir

[Apache Solr](http://lucene.apache.org/solr/features.html) veriler üzerinde güçlü tam metin araması sağlayan bir kurumsal arama platformudur. Hadoop depolamak ve yönetmek çok büyük miktarda veri etkinleştirse bile, Apache Solr hızlıca veri almak için arama özellikleri sağlar.

> [!WARNING]   
> HDInsight kümesi ile sağlanan bileşenler tamamen Microsoft tarafından desteklenir.
>
> Özel bileşenler, Solr gibi daha fazla sorun giderme konusunda yardımcı olması için ticari açıdan makul destek alırsınız. Microsoft Destek, özel bileşenlerle sorunlarını çözmek mümkün olmayabilir. Yardım almak için açık kaynak topluluklar ilgisini gerekebilir. Örneğin, gibi kullanılan birçok topluluk siteleri vardır: [HDInsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Apache projeleri proje siteleri de [ http://apache.org ](http://apache.org), örneğin: [Hadoop](http://hadoop.apache.org/).

## <a name="what-the-script-does"></a>Betik yapar

Bu betik, HDInsight kümesi için aşağıdaki değişiklikleri yapar:

* Solr 4.9 uygulamasına yükler `/usr/hdp/current/solr`
* Bir kullanıcının oluşturduğu **solrusr**, Solr hizmetini çalıştırmak için kullanılır
* Kümeleri **solruser** sahibi olarak `/usr/hdp/current/solr`
* Ekler bir [Upstart](http://upstart.ubuntu.com/) Solr otomatik olarak başlatan yapılandırma.

## <a name="install"></a>Betik eylemleri kullanılarak Solr yükleme

Solr'ın bir HDInsight kümesine yüklemek için örnek betik, aşağıdaki konumda kullanılabilir:

    https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh

Solr yüklü olan bir küme oluşturmak için adımları kullanın. [oluşturma HDInsight kümeleri](hdinsight-hadoop-create-linux-clusters-portal.md) belge. Oluşturma işlemi sırasında Solr yüklemek için aşağıdaki adımları kullanın:

1. Gelen __küme özeti__ bölümü, ardından select__Advanced settings__ __betik eylemlerini__. Formun doldurulması için aşağıdaki bilgileri kullanın:

   * **AD**: Betik eylemi için bir kolay ad girin.
   * **BETİK URI'Sİ**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh
   * **HEAD**: Bu seçeneği işaretleyin
   * **ÇALIŞAN**: Bu seçeneği işaretleyin
   * **ZOOKEEPER**: Zookeeper düğümüne yüklemek için bu seçeneği işaretleyin
   * **PARAMETRELERİ**: Bu alanı boş bırakın

2. Sayfanın alt kısmında **betik eylemlerini** bölümündeki **seçin** yapılandırmayı kaydetmek için düğme. Son olarak, **sonraki** dönmek için düğme __küme özeti__

3. Gelen __küme özeti__ sayfasında __Oluştur__ kümeyi oluşturun.

## <a name="usesolr"></a>HDInsight Solr nasıl kullanabilirim

> [!IMPORTANT]  
> Bu bölümdeki adımları temel Solr işlevlerini göstermektedir. Solr kullanma hakkında daha fazla bilgi için bkz. [Apache Solr site](http://lucene.apache.org/solr/).

### <a name="index-data"></a>Verilerin dizinini oluşturma

Solr için örnek verileri eklemek için aşağıdaki adımları kullanın ve ardından sorgulayın:

1. SSH kullanarak HDInsight kümesine bağlanma:

    > [!NOTE]  
    > Değiştirin `sshuser` SSH kullanıcısı ile küme için. Değiştirin `clustername` ile kümesinin adı.

    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

     > [!IMPORTANT]  
     > Daha sonra bu belgedeki adımlarda bir SSH tüneli Solr web kullanıcı Arabirimine bağlamak için kullanın. Bu adımları kullanabilmek için bir SSH tüneli ve daha sonra kullanmak için tarayıcınızı yapılandırmanız gerekir.
     >
     > Daha fazla bilgi için [SSH tünel oluşturmayı kullanma HDInsight ile](hdinsight-linux-ambari-ssh-tunnel.md) belge.

2. Solr dizin örnek verileriniz için aşağıdaki komutları kullanın:

    ```bash
    cd /usr/hdp/current/solr/example/exampledocs
    java -jar post.jar solr.xml monitor.xml
    ```

    Aşağıdaki çıktıyı konsola döndürülür:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    `post.jar` Yardımcı programı ekler **solr.xml** ve **monitor.xml** belgelere dizin.
  
3. Solr REST API sorgulamak için aşağıdaki komutu kullanın:

    ```bash
    curl "http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true"
    ```

    Bu komut arar **collection1** eşleşen herhangi bir belge için **\*:\*** (olarak kodlanmış \*% 3A\* sorgu dizesinde). Aşağıdaki JSON belgesini yanıt örneğidir:

            "response": {
                "numFound": 2,
                "start": 0,
                "maxScore": 1,
                "docs": [
                  {
                    "id": "SOLR1000",
                    "name": "Solr, the Enterprise Search Server",
                    "manu": "Apache Software Foundation",
                    "cat": [
                      "software",
                      "search"
                    ],
                    "features": [
                      "Advanced Full-Text Search Capabilities using Lucene",
                      "Optimized for High Volume Web Traffic",
                      "Standards Based Open Interfaces - XML and HTTP",
                      "Comprehensive HTML Administration Interfaces",
                      "Scalability - Efficient Replication to other Solr Search Servers",
                      "Flexible and Adaptable with XML configuration and Schema",
                      "Good unicode support: héllo (hello with an accent over the e)"
                    ],
                    "price": 0,
                    "price_c": "0,USD",
                    "popularity": 10,
                    "inStock": true,
                    "incubationdate_dt": "2006-01-17T00:00:00Z",
                    "_version_": 1486960636996878300
                  },
                  {
                    "id": "3007WFP",
                    "name": "Dell Widescreen UltraSharp 3007WFP",
                    "manu": "Dell, Inc.",
                    "manu_id_s": "dell",
                    "cat": [
                      "electronics and computer1"
                    ],
                    "features": [
                      "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                    ],
                    "includes": "USB cable",
                    "weight": 401.6,
                    "price": 2199,
                    "price_c": "2199,USD",
                    "popularity": 6,
                    "inStock": true,
                    "store": "43.17614,-90.57341",
                    "_version_": 1486960637584081000
                  }
                ]
              }

### <a name="using-the-solr-dashboard"></a>Solr panosunu kullanma

Solr, bir web tarayıcınız Solr ile çalışmanıza olanak sağlayan kullanıcı Arabirimi panodur. Doğrudan Internet'ten HDInsight kümeniz üzerinde Solr Pano gösterilmez. SSH tüneli erişmek için kullanabilirsiniz. SSH tüneli kullanma hakkında daha fazla bilgi için bkz. [SSH tünel oluşturmayı kullanma HDInsight ile](hdinsight-linux-ambari-ssh-tunnel.md) belge.

SSH tüneli sağladıktan sonra Solr panoyu kullanmak için aşağıdaki adımları kullanın:

1. Birincil baş için ana bilgisayar adı belirler:

   1. Küme baş düğümüne bağlanmak için SSH kullanın. Örneğin, `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.

       SSH kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

   2. Tam ana bilgisayar adını almak için aşağıdaki komutu kullanın:

        ```bash
        hostname -f
        ```

        Bu komut, aşağıdaki ana bilgisayar adı için benzer bir değer döndürür:

            hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

        Döndürülen değer, daha sonra kullanılmak üzere kaydedin.

2. Tarayıcınızda bağlanma **http://HOSTNAME:8983/solr/#/** burada **HOSTNAME** önceki adımlarda belirlenen adıdır.

    İstek, kümenizdeki Solr web kullanıcı Arabirimi SSH tüneli üzerinden yönlendirilir. Aşağıdaki görüntüye benzer bir sayfa görünür:

    ![Solr panosunun görüntüsü](./media/hdinsight-hadoop-solr-install-linux/solrdashboard.png)

3. Sol bölmede, kullanmak **çekirdek Seçici** seçmek için açılan **collection1**. Birden çok girişi bunları altında görünmelidir **collection1**.

4. Girişlerinden **collection1**seçin **sorgu**. Arama sayfasını doldurmak için aşağıdaki değerleri kullanın:

   * İçinde **q** metin kutusuna  **\*:**\*. Bu sorgu, Solr dizine alınmış tüm belgelerin döndürür. Belirli bir dizeyi belgelerde arama yapmak istiyorsanız, buraya o dizenin girebilirsiniz.
   * İçinde **wt** metin kutusunda, çıkış biçimini seçin. Varsayılan değer **json**.

     Son olarak, seçin **sorguyu Yürüt** arama pate alt kısmındaki düğmesi.

     ![Bir küme özelleştirmek üzere betik eylemi kullanın](./media/hdinsight-hadoop-solr-install-linux/hdi-solr-dashboard-query.png)

     Çıkış dizinine daha önce eklediğimiz iki belgeleri döndürür. Aşağıdaki JSON belgesini için benzer bir çıkış:

           "response": {
               "numFound": 2,
               "start": 0,
               "maxScore": 1,
               "docs": [
                 {
                   "id": "SOLR1000",
                   "name": "Solr, the Enterprise Search Server",
                   "manu": "Apache Software Foundation",
                   "cat": [
                     "software",
                     "search"
                   ],
                   "features": [
                     "Advanced Full-Text Search Capabilities using Lucene",
                     "Optimized for High Volume Web Traffic",
                     "Standards Based Open Interfaces - XML and HTTP",
                     "Comprehensive HTML Administration Interfaces",
                     "Scalability - Efficient Replication to other Solr Search Servers",
                     "Flexible and Adaptable with XML configuration and Schema",
                     "Good unicode support: héllo (hello with an accent over the e)"
                   ],
                   "price": 0,
                   "price_c": "0,USD",
                   "popularity": 10,
                   "inStock": true,
                   "incubationdate_dt": "2006-01-17T00:00:00Z",
                   "_version_": 1486960636996878300
                 },
                 {
                   "id": "3007WFP",
                   "name": "Dell Widescreen UltraSharp 3007WFP",
                   "manu": "Dell, Inc.",
                   "manu_id_s": "dell",
                   "cat": [
                     "electronics and computer1"
                   ],
                   "features": [
                     "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                   ],
                   "includes": "USB cable",
                   "weight": 401.6,
                   "price": 2199,
                   "price_c": "2199,USD",
                   "popularity": 6,
                   "inStock": true,
                   "store": "43.17614,-90.57341",
                   "_version_": 1486960637584081000
                 }
               ]
             }

### <a name="starting-and-stopping-solr"></a>Başlatma ve durdurma Solr

El ile durdurun ve Solr'ı başlatmak için aşağıdaki komutları kullanın:

```bash
sudo stop solr
sudo start solr
```

## <a name="backup-indexed-data"></a>Veri yedekleme dizini

Varsayılan depolama kümenizin Solr verileri yedeklemek için aşağıdaki adımları kullanın:

1. SSH kullanarak kümeye bağlanın ve ardından baş düğümü için konak adını almak için aşağıdaki komutu kullanın:

    ```bash
    hostname -f
    ```

2. Dizini oluşturulan verilerin bir anlık görüntüsünü oluşturmak için aşağıdaki komutu kullanın. Değiştirin **HOSTNAME** önceki komuttan döndürülen ada sahip:

    ```bash
    curl http://HOSTNAME:8983/solr/replication?command=backup
    ```

    Yanıt aşağıdaki XML'e benzer:

        <?xml version="1.0" encoding="UTF-8"?>
        <response>
          <lst name="responseHeader">
            <int name="status">0</int>
            <int name="QTime">9</int>
          </lst>
          <str name="status">OK</str>
        </response>

3. Dizinleri `/usr/hdp/current/solr/example/solr`. Bir alt burada her koleksiyon için yoktur. Her koleksiyon dizinini içeren bir `data` koleksiyonu için anlık görüntü içeren dizin.

4. Anlık görüntü klasörü, sıkıştırılmış bir arşiv oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    tar -zcf snapshot.20150806185338855.tgz snapshot.20150806185338855
    ```

    Değiştirin `snapshot.20150806185338855` koleksiyonunuz için anlık görüntü adıyla değerleri.

    Bu komut, adlandırılmış bir arşivini oluşturur **snapshot.20150806185338855.tgz**, içeriğini içeren **snapshot.20150806185338855** dizin.

5. Ardından aşağıdaki komutu kullanarak kümenin birincil depolama arşivi depolayabilir:

    ```bash
    hdfs dfs -put snapshot.20150806185338855.tgz /example/data
    ```

Apache Solr yedekleme ve geri yükleme ile çalışma hakkında daha fazla bilgi için bkz. [ https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups ](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups).

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight kümeleri üzerinde Apache Giraph yükleme](hdinsight-hadoop-giraph-install-linux.md). Küme özelleştirmesi, HDInsight Hadoop kümelerinde Giraph'ı yüklemek için kullanın. Giraph grafik Hadoop kullanarak işleme yapmanıza olanak tanır ve Azure HDInsight ile kullanılabilir.

* [HDInsight kümelerinde Hue yüklemek](hdinsight-hadoop-hue-linux.md). Küme özelleştirmesi, HDInsight Hadoop kümeleri üzerinde Hue yüklemek için kullanın. Hue Web uygulamalarının bir Hadoop kümesi ile etkileşim kurmak için kullanılan bir kümesidir.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
