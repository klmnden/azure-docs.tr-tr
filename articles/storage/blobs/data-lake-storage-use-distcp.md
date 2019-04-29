---
title: Azure Data Lake depolama Gen2 DistCp kullanarak verileri kopyalama | Microsoft Docs
description: Data Lake depolama Gen2'ye ve veri kopyalamak için DistCp aracını kullanın
services: storage
author: seguler
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: seguler
ms.openlocfilehash: 3b58dc8dabc55ba428ce6e35091a6947e5f4a824
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61481811"
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-azure-data-lake-storage-gen2"></a>Azure depolama BLOB'ları ile Azure Data Lake depolama Gen2 arasında veri kopyalamak için DistCp kullanma

Kullanabileceğiniz [DistCp](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html) etkin hiyerarşik ad alanı ile bir genel amaçlı V2 depolama hesabı ve genel amaçlı V2 depolama hesabı arasında veri kopyalamak için. Bu makalede, yönergeler DistCp aracını sağlar.

Komut satırı parametreleri çeşitli DistCp sağlar ve uygulamanızın kullanımını iyileştirmek için bu makaleyi okuyun için önemle öneririz. Bu makale, veri etkin hiyerarşik ad alanı hesabına kopyalamak için kullanımına odaklanarak temel işlevlerini gösterir.

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Data Lake depolama Gen2 özellikleri (hiyerarşik ad alanı) etkin olmayan mevcut bir Azure depolama hesabı**.
* **Data Lake depolama Gen2 özelliği etkin bir Azure depolama hesabıyla**. Bir oluşturma hakkında yönergeler için bkz: [bir Azure Data Lake depolama Gen2'ye depolama hesabı oluşturma](data-lake-storage-quickstart-create-account.md)
* **Bir dosya sistemi** , oluşturuldu depolama hesabında etkin hiyerarşik ad alanı.
* **Azure HDInsight kümesinde** Data Lake depolama Gen2'ye etkin olan bir depolama hesabına erişim. Bkz: [kullanımı Azure Data Lake depolama Gen2 Azure HDInsight ile kümeleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json). Küme için Uzak Masaüstü etkinleştirdiğinizden emin olun.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>Bir HDInsight Linux kümesinden DistCp kullanma

Bir HDInsight kümesi, bir HDInsight kümesine farklı kaynaklardan gelen verileri kopyalamak için kullanılan DistCp yardımcı programı ile birlikte gelir. HDInsight kümesi, Azure Blob Depolama ve Azure Data Lake Storage birlikte kullanmak için yapılandırdıysanız, kullanılan,-arasında veri kopyalamak için hazır DistCp yardımcı olabilir. Bu bölümde, DistCp yardımcı programını kullanmak nasıl tümleştirildiği incelenmektedir.

1. HDI kümenize bir SSH oturumu oluşturun. Bkz: [Linux tabanlı HDInsight kümesine bağlanma](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

2. (Etkin hiyerarşik ad alanı), mevcut genel amaçlı V2 hesabına erişim sağlayıp sağlayamadığınızı doğrulayın.

        hdfs dfs –ls wasbs://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/

    Çıktı, kapsayıcı içeriğini listesini sağlamanız gerekir.

3. Benzer şekilde, kümeden etkin hiyerarşik ad alanı depolama hesabına erişebilir olup olmadığını doğrulayın. Şu komutu çalıştırın:

        hdfs dfs -ls abfss://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/

    Çıkış, Data Lake Storage hesabındaki dosyaların/klasörlerin listesini sağlamanız gerekir.

4. Bir Data Lake Storage hesabına WASB veri kopyalamak için DistCp kullanma.

        hadoop distcp wasbs://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg abfss://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder

    Komut içeriğini kopyalar **/örnek/data/gutenberg/** Blob depolama alanına klasöründe **/myfolder** Data Lake Storage hesabında.

5. Benzer şekilde, verileri Data Lake depolama hesabından Blob Storage (WASB) kopyalamak için DistCp kullanma.

        hadoop distcp abfss://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder wasbs://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg

    Komut içeriğini kopyalar **/myfolder** için Data Lake Store hesabındaki **/örnek/data/gutenberg/** WASB klasöründe.

## <a name="performance-considerations-while-using-distcp"></a>DistCp kullanırken performans konuları

DistCp'ın en düşük ayrıntı düzeyi, tek bir dosya olduğundan, en fazla eş zamanlı kopyaların sayısı ayarını Data Lake Storage iyileştirmek için en önemli parametredir. Eş zamanlı kopyaların sayısı azaltıcının sayısı eşittir (**m**) komut satırı parametresi. Bu parametre, verileri kopyalamak için kullanılan azaltıcının en fazla sayısını belirtir. Varsayılan değer 20'dir.

**Örnek**

    hadoop distcp wasbs://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg abfss://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder -m 100

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a>Kullanılacak azaltıcının sayısını nasıl belirlerim?

Aşağıda kullanabileceğiniz bazı yönergeler verilmiştir.

* **1. adım: 'Default' YARN uygulama kuyruğa kullanılabilir toplam bellek belirlemek** -ilk adımı 'default' YARN uygulama kuyruğa kullanılabilir bellek belirlemektir. Bu bilgiler, kümeyle ilişkili Ambari portalında kullanılabilir. YARN için gidin ve 'default' uygulama kuyruğa YARN bellek görmek için yapılandırmaları sekmesini görüntüleyin. (Aslında bir MapReduce işi olan) DistCp işiniz için toplam kullanılabilir belleğin budur.

* **2. adım: Azaltıcının sayısını hesaplamak** -değerini **m** toplam YARN bellek YARN kapsayıcı boyutuna göre bölünen sayının eşittir. YARN kapsayıcı boyutu bilgileri de Ambari portalda kullanılabilir. YARN için gidin ve yapılandırmaları sekmesini görüntüleyin. YARN kapsayıcı boyutu Bu pencerede görüntülenir. Azaltıcının numaradan ulaşması için eşitlik (**m**) olan

        m = (number of nodes * YARN memory for each node) / YARN container size

**Örnek**

Bir 4 x D14v2s kümeniz varsa ve 10 farklı klasörlerinden 10 TB veri aktarımı çalıştığınız varsayalım. Her değişken miktarda veri içerir ve her klasördeki dosya boyutları farklı.

* **YARN bellek toplam**: Ambari portaldan YARN belleğin bir D14 düğümü için 96 GB olup olmadığını belirler. Bu nedenle, dört düğümlü küme için toplam YARN bellek şöyledir: 

        YARN memory = 4 * 96GB = 384GB

* **Azaltıcının sayısı**: Ambari portaldan YARN kapsayıcı boyutu D14 küme düğümü için 3.072 MB olduğunu belirleyin. Bu nedenle, azaltıcının sayısıdır:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Ardından bellek kullanan diğer uygulamalar, yalnızca, kümenin YARN belleğin bir kısmını için DistCp kullanmayı da tercih edebilirsiniz.

### <a name="copying-large-datasets"></a>Büyük veri kümelerini kopyalama

Taşınacak veri kümesi boyutu olduğunda büyük (örneğin, > 1 TB) veya birçok farklı bir klasör varsa, birden çok DistCp iş kullanmayı düşünmeniz gerekir. Büyük olasılıkla herhangi bir performans kazancı yoktur, ancak herhangi bir işi başarısız olursa projenin tamamı yerine özel bir işi yeniden başlatmak yeterlidir, böylece işleri yayar.

### <a name="limitations"></a>Sınırlamalar

* DistCp performansına boyutunda benzer azaltıcının oluşturmaya çalışır. Azaltıcının sayısını artırmak her zaman performansı artırabilir değil.

* DistCp dosya başına yalnızca bir Eşleyici sınırlıdır. Bu nedenle, dosyaları olandan daha fazla azaltıcının olmamalıdır. DistCp bir dosyaya yalnızca bir Eşleyici atayabilirsiniz olduğundan, bu büyük dosyaları kopyalamak için kullanılan eşzamanlılık miktarını sınırlar.

* Büyük dosyaların küçük bir sayı varsa, bunları size daha fazla olası eşzamanlılık 256 MB dosya öbeklere bölünmesi.
