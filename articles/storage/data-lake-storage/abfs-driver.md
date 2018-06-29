---
title: Azure Data Lake Storage Gen2 Önizleme için Azure Blob dosya sistemi sürücüsü
description: ABFS Hadoop dosya sistemi sürücüsü
services: storage
keywords: ''
author: jamesbak
manager: jahogg
ms.topic: article
ms.author: jamesbak
ms.date: 06/27/2018
ms.service: storage
ms.component: data-lake-storage-gen2
ms.openlocfilehash: a726779e731be2534e457ba595d93fe51c023601
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036283"
---
# <a name="the-azure-blob-filesystem-driver-abfs-a-dedicated-azure-storage-driver-for-hadoop"></a>Azure Blob dosya sistemi sürücüsü (ABFS): Hadoop için adanmış bir Azure depolama sürücüsü

Azure Data Lake Storage Gen2 Önizleme verileri için birincil erişim yöntemleri biri aracılığıyla [Hadoop dosya sistemi](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/index.html). Azure Data Lake Storage Gen2 özellikleri Azure Blob dosya sistemi sürücüsü ilişkili bir sürücü veya `ABFS`. ABFS Apache Hadoop bir parçasıdır ve çoğu ticari dağıtımların hadoop eklenir. Bu sürücü kullanarak, pek çok uygulama ve çerçeveleri Data Lake Storage Gen2 verilerde açıkça Data Lake Storage Gen2 hizmet başvuran herhangi bir kod erişebilir.

## <a name="prior-capability-the-windows-azure-storage-blob-driver"></a>Önceki yetenek: Windows Azure depolama Blob sürücüsü

Windows Azure depolama Blob sürücü veya [WASB sürücü](https://hadoop.apache.org/docs/current/hadoop-azure/index.html) sağlanan Azure Storage Bloblarında özgün desteği. Bu sürücü eşleme dosya sisteminin karmaşık görev stili arabirim Azure Blob Storage tarafından kullanıma sunulan bir semantik (Hadoop dosya sistemi arabirimi tarafından gerektiği şekilde), nesne depolamak gerçekleştirilir. Bu sürücü BLOB'ları, depolanan verileri için yüksek performanslı erişim sağlayan, bu model desteklemeye devam eder ancak korumak zorlaşır Bu eşleme gerçekleştirme kodu önemli miktarda içerir. Ayrıca, bazı işlemler gibi [FileSystem.rename()](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_renamePath_src_Path_d) ve [FileSystem.delete()](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_deletePath_p_boolean_recursive) dizinleri uygulandığında büyük birkaç (nesne depoları eksikliği nedeniyle işlem gerçekleştirmek için sürücüsü gerektirir dizinler için destek) hangi sıklıkta performans için yol açar.

Bu nedenle, devralınmış tasarım eksiklikler WASB, gidermek için yeni Azure Data Lake Storage hizmeti yeni ABFS sürücüsünden desteğiyle uygulanmıştır.

## <a name="the-azure-blob-file-system-driver"></a>Azure Blob dosya sistemi sürücüsü

[Azure Data Lake Storage REST arabirimi](https://docs.microsoft.com/en-us/rest/api/datalakestorage/) Azure Blob Storage dosya sistemi sematiğini destekleyecek şekilde tasarlanmıştır. Hadoop dosya sistemi aynı zamanda aynı semantiğini desteklemek üzere tasarlanmıştır o sürücüsündeki karmaşık bir eşlemenin gereksinimi yoktur. Bu nedenle, yalnızca istemci dolgusu REST API için Azure Blob dosya sistemi sürücüsü (veya ABFS) değil.

Ancak, sürücü gerçekleştirmeniz gereken bazı işlevleri vardır:

### <a name="uri-scheme-to-reference-data"></a>Başvuru verileri için URI düzeni

Kaynakları (dizinleri ve dosyaları) ayrı olarak ele alınması için Hadoop içinde diğer dosya sistemi uygulamaları ile tutarlı ABFS sürücü kendi URI şeması tanımlar. URI şeması belgelenen [Azure Data Lake Storage Gen2 URI kullanmak](./introduction-abfs-uri.md). URI yapıdır: `abfs[s]://file_system@account_name.dfs.core.windows.net/<path>/<path>/<file_name>`

Yukarıdaki URI biçimi kullanarak, standart Hadoop araçları ve çerçeveleri bu kaynakları başvurmak için kullanılabilir:

```bash
hdfs dfs -mkdir -p abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data 
hdfs dfs -put flight_delays.csv abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data/ 
```

Dahili olarak, ABFS sürücü dosyaları ve dizinleri URI'si belirtilen kaynak(lar) dönüşür ve bu başvuruları ile Azure Data Lake Storage REST API'sini çağrılar.

### <a name="authentication"></a>Kimlik Doğrulaması

Hadoop uygulama güvenli bir şekilde Data Lake Storage Gen2 içinde bulunan kaynaklara erişebilmesi ABFS sürücüsü şu anda paylaşılan anahtar kimlik doğrulamasını destekler. Anahtar şifrelenir ve Hadoop yapılandırmasında depolanır.

### <a name="configuration"></a>Yapılandırma

ABFS sürücüsü için tüm yapılandırmayı depolanan <code>core-site.xml</code> yapılandırma dosyası. Hadoop dağıtımları özelliklerine sahip üzerinde [Ambari](http://ambari.apache.org/), yapılandırma aynı zamanda web portalı veya Ambari REST API kullanarak yönetiliyor olabilir.

Tüm desteklenen yapılandırma girişlerinin ayrıntılarını belirtilir [resmi Hadoop belgeleri](http://hadoop.apache.org/docs/current/hadoop-azure/index.html).

### <a name="hadoop-documentation"></a>Hadoop belgeleri

ABFS sürücü tam olarak belgelenen [resmi Hadoop belgeleri](http://hadoop.apache.org/docs/current/hadoop-azure/index.html)

## <a name="next-steps"></a>Sonraki adımlar

- [Kurulum Hdınsight kümeleri](./quickstart-create-connect-hdi-cluster.md)
- [Bir Azure Databricks kümesi oluşturma](./quickstart-create-databricks-account.md)
- [Azure Data Lake Storage Gen2 kullanmak URI](./introduction-abfs-uri.md)