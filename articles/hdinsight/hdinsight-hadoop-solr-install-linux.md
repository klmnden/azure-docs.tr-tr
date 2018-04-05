---
title: Linux tabanlı Hdınsight üzerinde - Azure Solr yüklemek için betik eylemi kullanın | Microsoft Docs
description: Betik eylemleri kullanarak Linux tabanlı Hdınsight Hadoop kümeleri üzerinde Solr yüklemeyi öğrenin.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cc93ed5c-a358-456a-91a4-f179185c0e98
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: larryfr
ms.openlocfilehash: f642a1f8060f566ec95b23995d0f82191b0c5315
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a>Yükleme ve Solr Hdınsight Hadoop kümeleri kullanma

Betik eylemi kullanarak Azure Hdınsight'ta Solr yüklemeyi öğrenin. Solr güçlü arama platformudur ve Hadoop tarafından yönetilen veri kuruluş düzeyinde arama yetenekleri sağlar.

> [!IMPORTANT]
    > Bu belgede yer alan adımlar Linux kullanan bir Hdınsight kümesi gerektirir. Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [Windows'da HDInsight'ın kullanımdan kaldırılması](hdinsight-component-versioning.md#hdinsight-windows-retirement).

> [!IMPORTANT]
> Bu belgede kullanılan örnek betik, belirli bir yapılandırmayla Solr 4.9 yükler. Farklı Koleksiyonlara, parça, şemalar, çoğaltmalar, vb. ile Solr küme yapılandırmak istiyorsanız, Solr ikili dosyaları ve komut dosyasını değiştirmeniz gerekir.

## <a name="whatis"></a>Solr nedir

[Apache Solr](http://lucene.apache.org/solr/features.html) güçlü tam metin araması verileri sağlayan bir kurumsal arama platformudur. Hadoop depolamak ve çok büyük miktarda veri yönetme olanak sağlarken, Apache Solr hızlı bir şekilde veri almak için arama yetenekleri sağlar.

> [!WARNING]
> Hdınsight kümesi ile sağlanan bileşenler tam olarak Microsoft tarafından desteklenir.
>
> Özel bileşenleri, Solr, örneğin, daha fazla sorun gidermenize yardımcı olması için ticari koşulların elverdiği oranda makul destek alırsınız. Microsoft destek özel bileşenlerle sorunları gidermek mümkün olmayabilir. Yardım için açık kaynak toplulukları devreye gerekebilir. Örneğin, olduğu gibi kullanılabilecek birçok topluluk siteleri vardır: [Hdınsight için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Apache projeleri proje siteleri de [ http://apache.org ](http://apache.org), örneğin: [Hadoop](http://hadoop.apache.org/).

## <a name="what-the-script-does"></a>Betiğin yaptığı

Bu komut, Hdınsight kümesine aşağıdaki değişiklikleri yapar:

* Solr 4.9 içine yükler `/usr/hdp/current/solr`
* Bir kullanıcı oluşturur **solrusr**, Solr hizmetini çalıştırmak için kullanılan
* Ayarlar **solruser** sahibi olarak `/usr/hdp/current/solr`
* Ekler bir [Upstart](http://upstart.ubuntu.com/) Solr otomatik olarak başlar yapılandırma.

## <a name="install"></a>Betik eylemleri kullanılarak Solr yükleyin

Solr bir Hdınsight kümesine yüklemek için örnek komut dosyası şu konumda kullanılabilir:

    https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh

Solr yüklü olan bir küme oluşturmak için adımlarda kullanın [Hdınsight kümeleri oluşturma](hdinsight-hadoop-create-linux-clusters-portal.md) belge. Oluşturma işlemi sırasında Solr yüklemek için aşağıdaki adımları kullanın:

1. Gelen __küme Özet__ bölümü, select__Advanced settings__, ardından __betik eylemleri__. Form doldurmak için aşağıdaki bilgileri kullanın:

   * **AD**: betik eylemi için kolay bir ad girin.
   * **BETİK URI'Sİ**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh
   * **HEAD**: Bu seçeneği işaretleyin.
   * **ÇALIŞAN**: Bu seçeneği işaretleyin.
   * **ZOOKEEPER**: Zookeeper düğümüne yüklemek için bu seçeneği işaretleyin.
   * **PARAMETRELERİ**: Bu alanı boş bırakın

2. Ekranın alt kısmındaki **betik eylemleri** bölümünde, kullanmak **seçin** yapılandırmayı kaydetmek için düğmesi. Son olarak, **sonraki** geri dönmek için düğmesini __küme özeti__

3. Gelen __küme Özet__ sayfasında, __oluşturma__ küme oluşturmak için.

## <a name="usesolr"></a>Hdınsight'ta nasıl Solr kullanıyor

> [!IMPORTANT]
> Bu bölümdeki adımları temel Solr işlevleri göstermektedir. Solr kullanma hakkında daha fazla bilgi için bkz: [Apache Solr site](http://lucene.apache.org/solr/).

### <a name="index-data"></a>Dizin verileri

Solr için örnek veri eklemek için aşağıdaki adımları kullanın ve ardından sorgu:

1. SSH kullanarak HDInsight kümesine bağlanma:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

     > [!IMPORTANT]
     > Adımları bu belgenin sonraki bölümlerinde SSL tüneli Solr web kullanıcı Arabirimine bağlamak için kullanın. Bu adımları kullanabilmek için bir SSL tünel oluşturmak ve daha sonra kullanmak için tarayıcınızı yapılandırmanız gerekir.
     >
     > Daha fazla bilgi için bkz: [kullanım SSH tünel Hdınsight ile](hdinsight-linux-ambari-ssh-tunnel.md) belge.

2. Solr dizin örnek veri sağlamak için aşağıdaki komutları kullanın:

    ```bash
    cd /usr/hdp/current/solr/example/exampledocs
    java -jar post.jar solr.xml monitor.xml
    ```

    Aşağıdaki çıktıyı konsola verilir:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    `post.jar` Yardımcı programı ekler **solr.xml** ve **monitor.xml** dizine belgeleri.
  
3. Solr REST API sorgulamak için aşağıdaki komutu kullanın:

    ```bash
    curl "http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true"
    ```

    Bu komut arar **collection1** eşleşen herhangi bir belgeniz için **\*:\*** (olarak kodlanmış \*% 3A\* sorgu dizesinde). Aşağıdaki JSON belgesi, yanıt örneğidir:

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

Solr Pano web tarayıcınız üzerinden Solr ile çalışmanıza olanak sağlayan kullanıcı Arabirimi bir web uygulamasıdır. Solr Pano doğrudan Internet'ten, Hdınsight kümesi üzerinde açık değil. SSH tüneli erişmek için kullanabilirsiniz. SSH tüneli kullanma hakkında daha fazla bilgi için bkz: [kullanım SSH tünel Hdınsight ile](hdinsight-linux-ambari-ssh-tunnel.md) belge.

SSH tüneli kurduktan sonra Solr panoyu kullanmak için aşağıdaki adımları kullanın:

1. Birincil headnode ana bilgisayar adını belirleyin:

   1. SSH küme baş düğümüne bağlanmak için kullanın. Örneğin, `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.

       SSH kullanma hakkında daha fazla bilgi için bkz: [Hdınsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

   2. Tam ana bilgisayar adı almak için aşağıdaki komutu kullanın:

        ```bash
        hostname -f
        ```

        Bu komutu aşağıdaki ana bilgisayar adı için benzer bir değer döndürür:

            hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

        Daha sonra kullanılmak üzere döndürdü, değer kaydedin.

2. Tarayıcınızda, bağlanmak **http://HOSTNAME:8983/solr/#/**, burada **ana bilgisayar adı** önceki adımlarda belirlenen adıdır.

    İstek, kümenizde Solr Web Arabirimine SSH tüneli üzerinden yönlendirilir. Sayfa aşağıdaki görüntüye benzer görünür:

    ![Solr Pano görüntüsü](./media/hdinsight-hadoop-solr-install-linux/solrdashboard.png)

3. Sol bölmeden kullanmak **çekirdek Seçici** seçmek için açılır **collection1**. Birden çok girişi bunları altında görünmelidir **collection1**.

4. Girişlerden **collection1**seçin **sorgu**. Arama sayfası doldurmak için aşağıdaki değerleri kullanın:

   * İçinde **q** metin kutusuna  **\*:**\*. Bu sorgu Solr dizini tüm belgeleri döndürür. Belirli bir dizeyi belgelerde arama yapmak istiyorsanız, o dizeyi buraya girebilirsiniz.
   * İçinde **wt** metin kutusunda, çıktı biçimi seçin. Varsayılan değer **json**.

     Son olarak, seçin **sorgu yürütme** arama pate sonundaki düğmesi.

     ![Bir küme özelleştirmek için betik eylemi kullanın](./media/hdinsight-hadoop-solr-install-linux/hdi-solr-dashboard-query.png)

     Çıktı dizini daha önce eklediğimiz iki belge döndürür. Çıktı aşağıdaki JSON belgesi benzer:

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

El ile durdurun ve Solr başlatmak için aşağıdaki komutları kullanın:

```bash
sudo stop solr
sudo start solr
```

## <a name="backup-indexed-data"></a>Veri yedekleme dizini

Kümeniz için varsayılan depolama Solr verileri yedeklemek için aşağıdaki adımları kullanın:

1. SSH kullanarak kümeye bağlanın, ardından baş düğüm için ana bilgisayar adı almak için aşağıdaki komutu kullanın:

    ```bash
    hostname -f
    ```

2. Dizinli verilerin bir anlık görüntüsünü oluşturmak için aşağıdaki komutu kullanın. Değiştir **ana bilgisayar adı** önceki komuttan döndürülen adı ile:

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

3. Değiştirme dizinleri `/usr/hdp/current/solr/example/solr`. Bir alt burada her koleksiyon için yoktur. Her koleksiyon dizini içeren bir `data` koleksiyonu için anlık görüntü içeren dizini.

4. Sıkıştırılmış bir anlık görüntü klasörü arşiv oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    tar -zcf snapshot.20150806185338855.tgz snapshot.20150806185338855
    ```

    Değiştir `snapshot.20150806185338855` koleksiyonunuz için anlık görüntü adı değerlerle.

    Bu komut, adlandırılmış bir arşiv oluşturur **snapshot.20150806185338855.tgz**, içeriğini içeren **snapshot.20150806185338855** dizini.

5. Ardından aşağıdaki komutu kullanarak kümenin birincil depolama arşive depolayabilirsiniz:

    ```bash
    hdfs dfs -put snapshot.20150806185338855.tgz /example/data
    ```

Solr yedekleme ve geri yükleme ile çalışma hakkında daha fazla bilgi için bkz: [ https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups ](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups).

## <a name="next-steps"></a>Sonraki adımlar

* [Hdınsight kümelerinde Giraph yükleme](hdinsight-hadoop-giraph-install-linux.md). Küme özelleştirme Giraph Hdınsight Hadoop kümelerine yüklemek için kullanın. Giraph Hadoop kullanarak işleme grafik işlemleri yapmanıza olanak tanır ve Azure Hdınsight ile kullanılabilir.

* [Hdınsight kümelerinde Hue yüklemek](hdinsight-hadoop-hue-linux.md). Küme özelleştirme Hdınsight Hadoop kümeleri üzerinde Hue yüklemek için kullanın. Hue Hadoop kümeyle etkileşim kurmak için kullanılan Web uygulamaları kümesidir.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
