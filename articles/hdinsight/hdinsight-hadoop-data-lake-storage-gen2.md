---
title: Azure HDInsight, Apache Hadoop ile Azure Data Lake Storage (ADLS) 2. nesil kullanın
description: Azure Data Lake depolama Gen2'ye ve Azure HDInsight ile nasıl çalıştığı hakkında genel bir bakış sağlar.
services: hdinsight,storage
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/21/2018
ms.openlocfilehash: 2ae11afe1ecbe500a4851aab6d56e612fbe79ee6
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52975133"
---
# <a name="use-azure-data-lake-storage-gen2-with-apache-hadoop-in-azure-hdinsight"></a>Azure HDInsight, Apache Hadoop ile Azure Data Lake depolama Gen2'ı kullanma

Azure Data Lake depolama Gen1 özellikleri gibi bir Hadoop uyumlu bir dosya sistemi, Azure Active Directory, Azure Data Lake Storage (ADLS) 2. nesil alır çekirdek ve POSIX tabanlı ACL'ler ve bunları Azure Blob Depolama'ya tümleştirir. Bu birleşim de Blob Depolama'nın katmanlama ve veri Yaşam Döngüsü Yönetimi'ni kullanırken Azure Data Lake depolama Gen1 performansını yararlanmanızı sağlar.

## <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Azure Data Lake depolama Gen2'ın çekirdek işlevselliği

- Hadoop uyumlu erişim: Azure Data Lake depolama Gen2'ye ve bir Hadoop dağıtılmış dosya sistemi (HDFS) ile olduğu gibi veri erişimi yönetmesine olanak tanır. ABFS sürücü, Data Lake depolama 2. nesil'deki depolanan verilere erişmek için Azure HDInsight ve Azure Databricks dahil olmak üzere tüm Apache Hadoop ortamlarında kullanılabilir.

- POSIX izinleri kümesi: ACL ve POSIX izinlerle birlikte için Data Lake depolama Gen2'ye özel bazı ek ayrıntı Data Lake Gen2 güvenlik modelini destekler. Ayarları, Yönetim Araçları veya Apache Hive ve Apache Spark gibi çerçeveleri aracılığıyla yapılandırılabilir.

- Uygun maliyetli: Data Lake depolama Gen2, düşük maliyetli depolama kapasitesi ve işlem sunar. Azure Blob Depolama yaşam döngüsü gibi özellikler, veri yaşam döngüsü ile hareket ettikçe faturalandırma ücretleri ayarlayarak maliyetleri yardımcı olur.

- Blob Depolama Araçlar, çerçeveler ve uygulamalar ile çalışır: Data Lake depolama Gen2'ye bir çeşit Araçlar, çerçeveler ve Blob Depolama için bugün mevcut uygulamaları çalışmak devam eder.

- En iyi duruma getirilmiş sürücü: ABFS sürücü, özellikle büyük veri analizi için getirilmiştir. Karşılık gelen REST API'leri dfs uç noktası aracılığıyla çıkmış dfs.core.windows.net.

## <a name="whats-new-about-azure-data-lake-storage-gen-2"></a>Azure Data Lake depolama Gen 2 yenilikler

### <a name="managed-identities-for-secure-file-access"></a>Güvenli dosya erişimi için kimlikleri yönetilen

Azure HDInsight, Azure Data Lake depolama Gen2 dosyalarında küme erişimin güvenliğini sağlamak için yönetilen kimlikleri kullanır. Yönetilen kimlikleri, otomatik olarak yönetilen kimlik bilgileri kümesi ile Azure hizmetleri sağlayan Azure Active Directory özelliğidir. Bu kimlik bilgileri, AD kimlik doğrulamasını destekleyen herhangi bir hizmet için kimlik doğrulaması için kullanılabilir. Yönetilen kimliklerle kod veya yapılandırma dosyalarında kimlik bilgilerini saklamak amacıyla gerektirmez.

Daha fazla bilgi için [Azure kaynakları için yönetilen kimlikleri nedir](../active-directory/managed-identities-azure-resources/overview.md).

### <a name="azure-blob-filesystem-abfs-driver"></a>Azure Blob dosya sistemi (ABFS) sürücü

Apache Hadoop uygulamaları yerel olarak okuma ve yerel disk depolama alanından verileri yazma beklenir. Bir Hadoop dosya sistemi sürücü ABFS gibi uygulama için normal Hadoop dosya sistemi işlemleri öykünen tarafından bulut depolama ile çalışmak Hadoop uygulamaları sağlar. Sürücü, ardından bu işlemleri gerçek bulut depolama platformu anlayan işlemlere dönüştürür.

Daha önce Hadoop dosya sistemi sürücü tüm dosya sistemi işlemleri Azure depolama REST API çağrıları istemci tarafında dönüştürün ve ardından REST API çağırma. Birden çok REST API ile sonuçlandı. Bu istemci-tarafı dönüştürme, ancak dosyayı yeniden adlandırma gibi bir tek bir dosya sistemi işlemi için çağırır. ABFS bazı Hadoop dosya sistemi mantığı istemci tarafı sunucu tarafı ve ADLS 2. nesil API için şimdi Blob API ile paralel çalışır taşınmıştır. Genel bir Hadoop dosya sistemi işlemleri artık yürütülebilir olduğundan bu geçiş, performansı geliştirir. bir REST API çağrısı ile.

Daha fazla bilgi için [Azure Blob dosya sistemi sürücü (ABFS): Hadoop için adanmış bir Azure depolama sürücüsü](../storage/data-lake-storage/abfs-driver.md).

### <a name="adls-gen-2-uri-scheme"></a>ADLS Gen 2 URI şeması

ADLS Gen2'ye HDInsight Azure depolamadaki dosyalara erişmek için yeni bir URI şeması kullanır:

`abfs[s]://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>`

SSL şifreli erişim URI düzeni sağlar (`abfss://` önek) ve şifrelenmemiş erişim (`abfs://` önek). Kullanmanızı öneririz `abfss` mümkün olduğunda, hatta azure'da aynı bölgede bulunan verilere erişirken.

`<FILE_SYSTEM_NAME>` Data Lake depolama Gen2'ye dosya sistemi yolunu tanımlar.

`<ACCOUNT_NAME>` Azure depolama hesabı adını tanımlar. Tam uygun etki alanı adı (FQDN) gereklidir.

`<PATH>` Dosya veya dizin HDFS yol adıdır.

Değilse değerleri `<FILE_SYSTEM_NAME>` ve `<ACCOUNT_NAME>` olmayan belirtilmezse, varsayılan dosya sistemi kullanılır. Varsayılan dosya sistemindeki dosyalar için göreli bir yol veya mutlak bir yol kullanabilirsiniz. Örneğin, `hadoop-mapreduce-examples.jar` HDInsight kümeleriyle gelen dosya başvurulabilir aşağıdaki yollardan birini kullanarak:

```
abfss://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
abfss:///example/jars/hadoop-mapreduce-examples.jar /example/jars/hadoop-mapreduce-examples.jar
```

> [!Note]
> Dosya adı `hadoop-examples.jar` HDInsight sürüm 2.1 ve 1.6 kümelerinde. HDInsight dışında dosyalarla çalışırken, yardımcı programların çoğu ABFS biçimlendirmek ve bunun yerine bir temel yol biçimi gibi bekler tanımaz `example/jars/hadoop-mapreduce-examples.jar`.

Daha fazla bilgi için [Azure Data Lake depolama Gen2 URI'si kullanma](../storage/data-lake-storage/introduction-abfs-uri.md).

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Data Lake depolama Gen2'ye Giriş](../storage/data-lake-storage/introduction.md)
- [Azure Data Lake depolama Gen2 önizlemesi Azure HDInsight kümeleri ile kullanma](../storage/data-lake-storage/use-hdi-cluster.md)