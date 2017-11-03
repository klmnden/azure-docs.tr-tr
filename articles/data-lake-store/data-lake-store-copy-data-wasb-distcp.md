---
title: Veri WASB gelen ve giden Distcp'yi kullanarak Data Lake Store kopyalama | Microsoft Docs
description: "Azure Storage Bloblarında gelen ve Data Lake Store'a veri kopyalamak için Distcp'yi aracını kullanın"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: ae2e9506-69dd-4b95-8759-4dadca37ea70
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/03/2017
ms.author: nitinme
ms.openlocfilehash: 1c9e100b4a0e7781f0782a49835d50492895ded1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-store"></a>Azure Depolama Blobları ile Data Lake Store arasında veri kopyalamak için Distcp kullanma
> [!div class="op_single_selector"]
> * [DistCp’yi kullanma](data-lake-store-copy-data-wasb-distcp.md)
> * [AdlCopy’yi kullanma](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Data Lake Store'a erişimi olan bir Hdınsight kümeniz varsa, veri kopyalamak için Distcp'yi gibi Hadoop ekosistemi araçları kullanabilirsiniz **ve ondan** Data Lake Store hesabında bir Hdınsight küme depolama (WASB). Bu makale, yönergeler Distcp'yi aracını sağlar.

## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Data Lake Store hesabı**. Bir oluşturma hakkında yönergeler için bkz: [Azure Data Lake Store ile çalışmaya başlama](data-lake-store-get-started-portal.md)
* **Azure Hdınsight kümesi** bir Data Lake Store hesabına erişim. Bkz: [Data Lake Store ile bir Hdınsight kümesi oluşturmayı](data-lake-store-hdinsight-hadoop-use-portal.md). Küme için Uzak Masaüstü etkinleştirdiğinizden emin olun.

## <a name="do-you-learn-fast-with-videos"></a>Videolarla daha hızlı mı öğreniyorsunuz?
[Bu videoyu izleyin](https://mix.office.com/watch/1liuojvdx6sie) Azure Storage Bloblarında ve Distcp'yi kullanarak Data Lake Store arasında veri kopyalama hakkında.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>Bir Hdınsight Linux kümeden Distcp'yi kullanın

Hdınsight kümesi bir Hdınsight kümesine farklı kaynaklardan veri kopyalamak için kullanılan Distcp'yi yardımcı programı ile birlikte gelir. Hdınsight kümesi Data Lake Store ek depolama alanı olarak kullanmak için yapılandırdıysanız, kullanılan out-of-- ve da Data Lake Store hesabından veri kopyalamak için box Distcp'yi yardımcı olabilir. Bu bölümde, Distcp'yi yardımcı programını kullanmak üzere nasıl tümleştirildiği incelenmektedir.

1. Masaüstünüzdeki, SSH kümeye bağlanmak için kullanın. Bkz: [Linux tabanlı Hdınsight kümesine bağlanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md). Komutları SSH isteminden çalıştırın.

2. Azure Storage Blobları (WASB) erişim olup olmadığını doğrulayın. Şu komutu çalıştırın:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    Çıktı depolama blobu içeriğini listesini sağlamanız gerekir.

3. Benzer şekilde, Data Lake Store hesabı kümeden erişip erişemeyeceğini doğrulayın. Şu komutu çalıştırın:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    Çıktı dosyaların/klasörlerin Data Lake Store hesabındaki listesini sağlamanız gerekir.

4. Verileri WASB bir Data Lake Store hesabına kopyalamak için Distcp'yi kullanın.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    Komut içeriğini kopyalar **/örnek/data/gutenberg/** için WASB klasöründe **/myfolder** Data Lake Store hesabındaki.

5. Benzer şekilde, WASB için Data Lake Store hesabından veri kopyalamak için Distcp'yi kullanın.

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Komut içeriğini kopyalar **/myfolder** için Data Lake Store hesabındaki **/örnek/data/gutenberg/** WASB klasöründe.

## <a name="performance-considerations-while-using-distcp"></a>Distcp'yi kullanırken performans etkenleri

Distcp'yi'nin en düşük ayrıntı düzeyi tek bir dosya olduğundan, en fazla sayıda eşzamanlı kopyasını Data Lake Store karşı iyileştirmek için en önemli parametre ayardır. Eşzamanlı kopya sayısını mappers sayısını ayarlayarak denetlenir ('M ') komut satırı parametresi. Bu parametre veri kopyalamak için kullanılan mappers üst sınırını belirtir. Varsayılan değer 20'dir.

**Örnek**

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder -m 100

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a>Kullanılacak mappers sayısını nasıl belirlerim?

Aşağıda kullanabileceğiniz bazı yönergeler verilmiştir.

* **1. adım: toplam YARN bellek belirlemek** -Distcp'yi iş çalıştırdığı küme için kullanılabilir YARN bellek belirlemek için ilk adımdır. Bu bilgiler, kümeyle ilişkili ambarı portalında kullanılabilir. YARN için gidin ve YARN bellek görmek için yapılandırmalar sekmesini görüntüleyin. Toplam YARN bellek almak için kümedeki sahip YARN bellek düğüm sayısı ile düğümü başına çarpın.

* **2. adım: mappers sayısını hesaplayın** -değerini **m** bölü YARN kapsayıcı boyutundan toplam YARN bellek sayının eşittir. YARN kapsayıcı boyutu bilgileri Ambari Portalı'nda da kullanılabilir. YARN için gidin ve yapılandırmalar sekmesini görüntüleyin. YARN kapsayıcı boyutu Bu pencerede görüntülenir. Mappers numaradan gelmesi denklemi (**m**) değil

        m = (number of nodes * YARN memory for each node) / YARN container size

**Örnek**

Kümede 4 D14v2s düğüm varsa ve 10 farklı klasörlerden 10 TB'lık veriyi aktarmaya çalıştığınız varsayalım. Klasörlerinin her biri değişen miktarda veri içerir ve her klasördeki dosya boyutları farklıdır.

* Toplam YARN bellek - gelen Ambari portal YARN bellek D14 düğümü için 96 GB olduğunu belirler. Bu nedenle, dört düğümlü küme için toplam YARN bellek şöyledir: 

        YARN memory = 4 * 96GB = 384GB

* Sayısı mappers - gelen Ambari portal YARN kapsayıcı boyutu D14 küme düğümü için 3072 olduğunu belirler. Bu nedenle, mappers sayısıdır:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Daha sonra diğer uygulamaları bellek kullanıyorsanız, yalnızca kümenizin YARN belleğin bir kısmını için Distcp'yi kullanmayı da tercih edebilirsiniz.

### <a name="copying-large-datasets"></a>Büyük veri kümelerini kopyalama

Taşınacak veri kümesi boyutunu olduğunda büyük (örneğin, > 1 TB) veya birçok farklı klasörleri varsa, birden çok Distcp'yi işleri kullanmayı düşünmeniz gerekir. Büyük olasılıkla bir performans kazancı yoktur, ancak herhangi bir işi başarısız olursa, yalnızca tüm iş yerine özel bir işi yeniden başlatmanız gerekir böylece işleri yayar.

### <a name="limitations"></a>Sınırlamalar

* Distcp'yi boyutunun performansını iyileştirmek için benzer mappers oluşturmayı dener. Mappers sayısını artırmak her zaman performansı artırabilir değil.

* Dosya başına yalnızca bir Eşleyici Distcp'yi sınırlıdır. Bu nedenle, dosyaları olandan daha fazla mappers olmamalıdır. Distcp'yi yalnızca bir dosyaya bir Eşleyici atayabilirsiniz olduğundan, bu büyük dosyaları kopyalamak için kullanılan eşzamanlılık miktarını sınırlar.

* Az sayıda büyük dosyalar varsa, bunları daha fazla olası eşzamanlılık vermek için 256 MB dosya parçalara bölünmesi. 
 
* Bir Azure Blob Depolama hesabından kopyalıyorsanız kopyalama işini blob depolama tarafında kısıtlanan. Bu, kopyalama işinin performansı düşürür. Azure Blob Depolama sınırları hakkında daha fazla bilgi için bkz: Azure Storage sınırlarını adresindeki [Azure aboneliği ve hizmet sınırları](../azure-subscription-service-limits.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Storage Bloblarından Data Lake Store'a veri kopyalama](data-lake-store-copy-data-azure-storage-blob.md)
* [Data Lake Store'da verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics'i Data Lake Store ile kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight'ı Data Lake Store ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
