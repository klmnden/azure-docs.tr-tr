---
title: Distcp kullanma WASB Azure Data Lake depolama Gen1 içine gelen ve giden veri kopyalama | Microsoft Docs
description: Azure Data Lake depolama Gen1 ve Azure depolama bloblarından veri kopyalamak için Distcp aracını kullanın
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: ae2e9506-69dd-4b95-8759-4dadca37ea70
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: fbefe233ce0d2477982faf0a9f38a73062e0c7a1
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58884474"
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-azure-data-lake-storage-gen1"></a>Azure depolama BLOB'ları ile Azure Data Lake depolama Gen1 arasında veri kopyalamak için Distcp kullanma
> [!div class="op_single_selector"]
> * [DistCp’yi kullanma](data-lake-store-copy-data-wasb-distcp.md)
> * [AdlCopy’yi kullanma](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Azure Data Lake depolama Gen1 erişimi olan bir HDInsight kümesine varsa, verileri kopyalamak için Distcp gibi Hadoop ekosistemi araçları kullanabilirsiniz **kitaplıklarından** bir Data Lake depolama Gen1 hesabına bir HDInsight kümesi depolama (WASB). Bu makalede, yönergeler Distcp aracını sağlar.

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Data Lake depolama Gen1 hesap**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake depolama Gen1 ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* **Azure HDInsight kümesinde** bir Data Lake depolama Gen1 hesabına erişim. Bkz: [ile Data Lake depolama Gen1 bir HDInsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md). Küme için Uzak Masaüstü etkinleştirdiğinizden emin olun.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>Bir HDInsight Linux kümesinden Distcp kullanma

Bir HDInsight kümesi, bir HDInsight kümesine farklı kaynaklardan gelen verileri kopyalamak için kullanılan Distcp yardımcı programı ile birlikte gelir. HDInsight kümesi, ek depolama alanı olarak Data Lake depolama Gen1 kullanmak için yapılandırdıysanız, kullanılan,-de bir Data Lake depolama Gen1 hesabı ve veri kopyalamak için hazır Distcp yardımcı olabilir. Bu bölümde, Distcp yardımcı programını kullanmak nasıl tümleştirildiği incelenmektedir.

1. Masaüstünüzde, kümeye bağlanmak için SSH kullanın. Bkz: [Linux tabanlı HDInsight kümesine bağlanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md). Komutları SSH isteminden çalıştırın.

2. Azure depolama BLOB'ları (WASB) erişim sağlayıp sağlayamadığınızı doğrulayın. Şu komutu çalıştırın:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    Çıktı, depolama BLOB içeriği listesini sağlamanız gerekir.

3. Benzer şekilde, kümeden Data Lake depolama Gen1 hesap erişim sağlayıp sağlayamadığınızı doğrulayın. Şu komutu çalıştırın:

        hdfs dfs -ls adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/

    Çıkış, Data Lake depolama Gen1 hesabındaki dosyaların/klasörlerin listesini sağlamanız gerekir.

4. Bir Data Lake depolama Gen1 hesabına WASB veri kopyalamak için Distcp kullanma.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/myfolder

    Komut içeriğini kopyalar **/örnek/data/gutenberg/** WASB için klasöründe **/myfolder** Data Lake depolama Gen1 hesabı.

5. Benzer şekilde, WASB için Data Lake depolama Gen1 hesabından veri kopyalamak için Distcp kullanma.

        hadoop distcp adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Komut içeriğini kopyalar **/myfolder** için Data Lake depolama Gen1 hesabındaki **/örnek/data/gutenberg/** WASB klasöründe.

## <a name="performance-considerations-while-using-distcp"></a>DistCp kullanırken performans konuları

DistCp'ın en düşük ayrıntı düzeyi, tek bir dosya olduğundan, en fazla eş zamanlı kopyaların sayısı ayarını Data Lake depolama Gen1 iyileştirmek için en önemli parametredir. Eş zamanlı kopyaların sayısı azaltıcının sayısını ayarlayarak denetlenir ('M ') komut satırı parametresi. Bu parametre, verileri kopyalamak için kullanılan azaltıcının en fazla sayısını belirtir. Varsayılan değer 20'dir.

**Örnek**

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/myfolder -m 100

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a>Kullanılacak azaltıcının sayısını nasıl belirlerim?

Aşağıda kullanabileceğiniz bazı yönergeler verilmiştir.

* **1. adım: Toplam YARN bellek belirlemek** -DistCp işi çalıştırdığı kümenin YARN bellek belirlemek için ilk adımdır. Bu bilgiler, kümeyle ilişkili Ambari portalında kullanılabilir. YARN için gidin ve YARN bellek görmek için yapılandırmaları sekmesini görüntüleyin. Toplam YARN bellek elde etmek için kümenizdeki sahip YARN bellek düğüm düğüm sayısı ile çarpın.

* **2. adım: Azaltıcının sayısını hesaplamak** -değerini **m** toplam YARN bellek YARN kapsayıcı boyutuna göre bölünen sayının eşittir. YARN kapsayıcı boyutu bilgileri de Ambari portalda kullanılabilir. YARN için gidin ve yapılandırmaları sekmesini görüntüleyin. YARN kapsayıcı boyutu Bu pencerede görüntülenir. Azaltıcının numaradan ulaşması için eşitlik (**m**) olan

        m = (number of nodes * YARN memory for each node) / YARN container size

**Örnek**

Bir 4 D14v2s düğüm kümedeki sahip ve 10 farklı klasörlerinden 10 TB veri aktarımı çalıştığınız varsayalım. Her değişken miktarda veri içerir ve her klasördeki dosya boyutları farklı.

* Toplam YARN bellek - gelen Ambari portalı YARN bellek D14 düğümü için 96 GB olduğunu belirler. Bu nedenle, dört düğümlü küme için toplam YARN bellek şöyledir: 

        YARN memory = 4 * 96GB = 384GB

* Sayısı azaltıcının - gelen Ambari portalı YARN kapsayıcı boyutu D14 küme düğümü için 3072 olduğunu belirler. Bu nedenle, azaltıcının sayısıdır:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Ardından bellek kullanan diğer uygulamalar, yalnızca, kümenin YARN belleğin bir kısmını için DistCp kullanmayı da tercih edebilirsiniz.

### <a name="copying-large-datasets"></a>Büyük veri kümelerini kopyalama

Taşınacak veri kümesi boyutu olduğunda büyük (örneğin, > 1 TB) veya birçok farklı bir klasör varsa, birden çok DistCp iş kullanmayı düşünmeniz gerekir. Büyük olasılıkla herhangi bir performans kazancı yoktur, ancak herhangi bir işi başarısız olursa projenin tamamı yerine özel bir işi yeniden başlatmak yeterlidir, böylece işleri yayar.

### <a name="limitations"></a>Sınırlamalar

* DistCp performansına boyutunda benzer azaltıcının oluşturmaya çalışır. Azaltıcının sayısını artırmak her zaman performansı artırabilir değil.

* DistCp dosya başına yalnızca bir Eşleyici sınırlıdır. Bu nedenle, dosyaları olandan daha fazla azaltıcının olmamalıdır. DistCp bir dosyaya yalnızca bir Eşleyici atayabilirsiniz olduğundan, bu büyük dosyaları kopyalamak için kullanılan eşzamanlılık miktarını sınırlar.

* Büyük dosyaların küçük bir sayı varsa, bunları size daha fazla olası eşzamanlılık 256 MB dosya öbeklere bölünmesi. 
 
* Bir Azure Blob Depolama hesabından kopyalıyorsanız kopyalama işiniz blob depolama tarafında kısıtlanmış. Bu, kopyalama işinin performansı düşürür. Azure Blob Depolama sınırları hakkında daha fazla bilgi için Azure depolama sınırları görmek [Azure aboneliği ve hizmet sınırlamaları](../azure-subscription-service-limits.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Data Lake depolama Gen1 için Azure depolama Bloblarından veri kopyalama](data-lake-store-copy-data-azure-storage-blob.md)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake depolama Gen1 ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight ile Data Lake depolama Gen1 kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)
