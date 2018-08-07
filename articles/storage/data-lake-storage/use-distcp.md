---
title: Azure Data Lake depolama Gen2 Distcp kullanma önizlemesi veri kopyalama | Microsoft Docs
description: Data Lake depolama Gen2 önizlemesi ve veri kopyalamak için Distcp aracını kullanın
services: storage
author: seguler
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: how-to
ms.date: 06/27/2018
ms.author: seguler
ms.openlocfilehash: 065c4c4315bda209484cc1b2449980e55d4ac798
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39522705"
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-storage-gen2-preview"></a>Azure depolama Blobları ile Data Lake depolama Gen2 önizlemesi arasında veri kopyalamak için Distcp kullanma

Azure Data Lake depolama Gen2 önizlemesi için erişimi olan bir HDInsight kümeniz varsa, Hadoop ekosistemi araçları gibi kullanabileceğiniz [Distcp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html) veri kopyalamak için **kitaplıklarından** HDInsight küme depolama (WASB) veri alanına Lake depolama Gen2 özellikli hesabı. Bu makalede, yönergeler Distcp aracını sağlar.

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Data Lake Storage (Önizleme) özelliği etkin bir Azure depolama hesabıyla**. Bir oluşturma hakkında yönergeler için bkz: [bir Azure Data Lake depolama Gen2 önizlemesi depolama hesabı oluşturma](quickstart-create-account.md)
* **Azure HDInsight kümesinde** bir Data Lake Store hesabına erişim. Bkz: [kullanımı Azure Data Lake depolama Gen2 Azure HDInsight ile kümeleri](use-hdi-cluster.md). Küme için Uzak Masaüstü etkinleştirdiğinizden emin olun.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>Bir HDInsight Linux kümesinden Distcp kullanma

Bir HDInsight kümesi, bir HDInsight kümesine farklı kaynaklardan gelen verileri kopyalamak için kullanılan Distcp yardımcı programı ile birlikte gelir. HDInsight kümesi, Azure Blob Depolama ve Azure Data Lake Storage birlikte kullanmak için yapılandırdıysanız, kullanılan,-arasında veri kopyalamak için hazır Distcp yardımcı olabilir. Bu bölümde, Distcp yardımcı programını kullanmak nasıl tümleştirildiği incelenmektedir.

1. Masaüstünüzde, kümeye bağlanmak için SSH kullanın. Bkz: [Linux tabanlı HDInsight kümesine bağlanma](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md). Komutları SSH isteminden çalıştırın.

2. Azure depolama BLOB'ları (WASB) erişim sağlayıp sağlayamadığınızı doğrulayın. Şu komutu çalıştırın:

        hdfs dfs –ls wasb://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/

    Çıktı, depolama BLOB içeriği listesini sağlamanız gerekir.

3. Benzer şekilde, kümeden Data Lake Store hesabına erişim sağlayıp sağlayamadığınızı doğrulayın. Şu komutu çalıştırın:

        hdfs dfs -ls abfs://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/

    Çıkış, Data Lake Storage hesabındaki dosyaların/klasörlerin listesini sağlamanız gerekir.

4. Bir Data Lake Storage hesabına WASB veri kopyalamak için Distcp kullanma.

        hadoop distcp wasb://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg abfs://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder

    Komut içeriğini kopyalar **/örnek/data/gutenberg/** Blob depolama alanına klasöründe **/myfolder** Data Lake Storage hesabında.

5. Benzer şekilde, verileri Data Lake depolama hesabından Blob Storage (WASB) kopyalamak için Distcp kullanma.

        hadoop distcp abfs://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder wasb://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg

    Komut içeriğini kopyalar **/myfolder** için Data Lake Store hesabındaki **/örnek/data/gutenberg/** WASB klasöründe.

## <a name="performance-considerations-while-using-distcp"></a>DistCp kullanırken performans konuları

DistCp'ın en düşük ayrıntı düzeyi, tek bir dosya olduğundan, en fazla eş zamanlı kopyaların sayısı ayarını Data Lake Storage iyileştirmek için en önemli parametredir. Eş zamanlı kopyaların sayısı azaltıcının sayısını ayarlayarak denetlenir (**m**) komut satırı parametresi. Bu parametre, verileri kopyalamak için kullanılan azaltıcının en fazla sayısını belirtir. Varsayılan değer 20'dir.

**Örnek**

    hadoop distcp wasb://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg abfs://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder -m 100

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a>Kullanılacak azaltıcının sayısını nasıl belirlerim?

Aşağıda kullanabileceğiniz bazı yönergeler verilmiştir.

* **1. adım: toplam YARN bellek belirlemek** -DistCp işi çalıştırdığı kümenin YARN bellek belirlemek için ilk adımdır. Bu bilgiler, kümeyle ilişkili Ambari portalında kullanılabilir. YARN için gidin ve YARN bellek görmek için yapılandırmaları sekmesini görüntüleyin. Toplam YARN bellek elde etmek için kümenizdeki sahip YARN bellek düğüm düğüm sayısı ile çarpın.

* **2. adım: azaltıcının sayısını hesaplamak** -değerini **m** toplam YARN bellek YARN kapsayıcı boyutuna göre bölünen sayının eşittir. YARN kapsayıcı boyutu bilgileri de Ambari portalda kullanılabilir. YARN için gidin ve yapılandırmaları sekmesini görüntüleyin. YARN kapsayıcı boyutu Bu pencerede görüntülenir. Azaltıcının numaradan ulaşması için eşitlik (**m**) olan

        m = (number of nodes * YARN memory for each node) / YARN container size

**Örnek**

Bir 4 D14v2s düğüm kümedeki sahip ve 10 farklı klasörlerinden 10 TB veri aktarımı çalıştığınız varsayalım. Her değişken miktarda veri içerir ve her klasördeki dosya boyutları farklı.

* **YARN bellek toplam**: gelen Ambari portalı YARN bellek D14 düğümü için 96 GB olduğunu belirlemek için. Bu nedenle, dört düğümlü küme için toplam YARN bellek şöyledir: 

        YARN memory = 4 * 96GB = 384GB

* **Azaltıcının sayısı**: gelen Ambari portalı YARN kapsayıcı boyutu D14 küme düğümü için 3072 olduğunu belirlemek için. Bu nedenle, azaltıcının sayısıdır:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Ardından bellek kullanan diğer uygulamalar, yalnızca, kümenin YARN belleğin bir kısmını için DistCp kullanmayı da tercih edebilirsiniz.

### <a name="copying-large-datasets"></a>Büyük veri kümelerini kopyalama

Taşınacak veri kümesi boyutu olduğunda büyük (örneğin, > 1 TB) veya birçok farklı bir klasör varsa, birden çok DistCp iş kullanmayı düşünmeniz gerekir. Büyük olasılıkla herhangi bir performans kazancı yoktur, ancak herhangi bir işi başarısız olursa projenin tamamı yerine özel bir işi yeniden başlatmak yeterlidir, böylece işleri yayar.

### <a name="limitations"></a>Sınırlamalar

* DistCp performansına boyutunda benzer azaltıcının oluşturmaya çalışır. Azaltıcının sayısını artırmak her zaman performansı artırabilir değil.

* DistCp dosya başına yalnızca bir Eşleyici sınırlıdır. Bu nedenle, dosyaları olandan daha fazla azaltıcının olmamalıdır. DistCp bir dosyaya yalnızca bir Eşleyici atayabilirsiniz olduğundan, bu büyük dosyaları kopyalamak için kullanılan eşzamanlılık miktarını sınırlar.

* Büyük dosyaların küçük bir sayı varsa, bunları size daha fazla olası eşzamanlılık 256 MB dosya öbeklere bölünmesi. 
