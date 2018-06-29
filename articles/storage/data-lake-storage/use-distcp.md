---
title: Azure Data Lake depolama Gen2 Distcp'yi kullanarak önizleme veri kopyalama | Microsoft Docs
description: Data Lake Storage Gen2 Önizleme gelen ve giden veri kopyalamak için Distcp'yi aracını kullanın
services: storage
documentationcenter: ''
author: seguler
manager: jahogg
editor: seguler
ms.component: data-lake-storage-gen2
ms.service: storage
ms.devlang: na
ms.topic: how-to
ms.date: 06/27/2018
ms.author: seguler
ms.openlocfilehash: 2a958ceb0b3a1db9d06d045a8161fa6cd3ef5aba
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059935"
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-storage-gen2-preview"></a>Azure Storage Bloblarında ve Data Lake Storage Gen2 Önizleme arasında veri kopyalamak için Distcp'yi kullanın

Azure Data Lake Storage Gen2 Önizleme erişimi olan bir Hdınsight kümesine varsa, veri kopyalamak için Distcp'yi gibi Hadoop ekosistemi araçları kullanabilirsiniz **ve ondan** bir Data Lake Storage Gen2 özellikli içine bir Hdınsight küme depolama (WASB) hesabı. Bu makale, yönergeler Distcp'yi aracını sağlar.

## <a name="prerequisites"></a>Önkoşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Data Lake Storage (Önizleme) özelliği etkin bir Azure depolama hesabıyla**. Bir oluşturma hakkında yönergeler için bkz: [Yapılacaklar](quickstart-create-account.md)
* **Azure Hdınsight kümesi** Data Lake Store hesabına erişimi. Bkz: [kullanım Azure Data Lake depolama Gen2 Azure Hdınsight ile kümeleri](use-hdi-cluster.md). Küme için Uzak Masaüstü etkinleştirdiğinizden emin olun.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>Bir Hdınsight Linux kümeden Distcp'yi kullanın

Hdınsight kümesi bir Hdınsight kümesine farklı kaynaklardan veri kopyalamak için kullanılan Distcp'yi yardımcı programı ile birlikte gelir. Hdınsight kümesi Azure Blob Storage ve Azure Data Lake Storage birlikte kullanmak için yapılandırdıysanız, kullanılan out-of--arasında veri kopyalamak için box Distcp'yi yardımcı olabilir. Bu bölümde, Distcp'yi yardımcı programını kullanmak üzere nasıl tümleştirildiği incelenmektedir.

1. Masaüstünüzdeki, SSH kümeye bağlanmak için kullanın. Bkz: [Linux tabanlı Hdınsight kümesine bağlanma](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md). Komutları SSH isteminden çalıştırın.

2. Azure Storage Blobları (WASB) erişim olup olmadığını doğrulayın. Şu komutu çalıştırın:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    Çıktı depolama blobu içeriğini listesini sağlamanız gerekir.

3. Benzer şekilde, Data Lake Store hesabına kümeden erişip erişemeyeceğini doğrulayın. Şu komutu çalıştırın:

        hdfs dfs -ls abfs://<filesystem_name>@<storage_account_name>.dfs.core.windows.net/

    Çıktı dosyaların/klasörlerin Data Lake Store hesabındaki listesini sağlamanız gerekir.

4. Verileri WASB bir Data Lake Store hesabına kopyalamak için Distcp'yi kullanın.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg abfs://<filesystem_name>@<storage_account_name>.dfs.core.windows.net/myfolder

    Komut içeriğini kopyalar **/örnek/data/gutenberg/** Blob depolama alanına klasöründe **/myfolder** Data Lake Store hesabındaki.

5. Benzer şekilde, Blob Storage (WASB) Data Lake Store hesabından veri kopyalamak için Distcp'yi kullanın.

        hadoop distcp abfs://<filesystem_name>@<storage_account_name>.dfs.core.windows.net/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    Komut içeriğini kopyalar **/myfolder** için Data Lake Store hesabındaki **/örnek/data/gutenberg/** WASB klasöründe.

## <a name="performance-considerations-while-using-distcp"></a>Distcp'yi kullanırken performans etkenleri

Distcp'yi'nin en düşük ayrıntı düzeyi tek bir dosya olduğundan, en fazla sayıda eşzamanlı kopyasını Data Lake Storage karşı iyileştirmek için en önemli parametre ayardır. Eşzamanlı kopya sayısını mappers sayısını ayarlayarak denetlenir ('M ') komut satırı parametresi. Bu parametre veri kopyalamak için kullanılan mappers üst sınırını belirtir. Varsayılan değer 20'dir.

**Örnek**

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg abfs://<filesystem_name>@<storage_account_name>.dfs.core.windows.net/myfolder -m 100

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
