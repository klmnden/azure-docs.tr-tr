---
title: Azure Data Lake depolama Gen2 Azure Blob dosya sistemi sürücüsü
description: ABFS Hadoop dosya sistemi sürücüsü
services: storage
author: normesta
ms.topic: conceptual
ms.author: normesta
ms.reviewer: jamesbak
ms.date: 12/06/2018
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: abe3f67141011c765f9de93bcf51998ddae002cb
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67696138"
---
# <a name="the-azure-blob-filesystem-driver-abfs-a-dedicated-azure-storage-driver-for-hadoop"></a>Azure Blob dosya sistemi sürücü (ABFS): Hadoop için adanmış bir Azure depolama sürücüsü

Azure Data Lake depolama Gen2 verileri için birincil erişim yöntemleri biri aracılığıyla [Hadoop dosya sistemi](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/index.html). Data Lake depolama Gen2, kullanıcıların yeni bir sürücü, Azure Blob dosya sistemi sürücüsü için Azure Blob Depolama erişim sağlar veya `ABFS`. ABFS Apache Hadoop bir parçasıdır ve hadoop ticari dağıtımlarının çoğunda dahil edilir. Bu sürücü kullanarak, birçok uygulama ve çerçeveler verileri Azure Blob depolama alanındaki açıkça Data Lake depolama Gen2'ye başvurma herhangi bir kodu erişebilir.

## <a name="prior-capability-the-windows-azure-storage-blob-driver"></a>Önceki özelliği: Windows Azure depolama blobu sürücü

Windows Azure depolama blobu sürücü veya [WASB sürücü](https://hadoop.apache.org/docs/current/hadoop-azure/index.html) Azure Blob Depolama için özgün destek sağlanır. Bu sürücü eşleme dosya sisteminin ilgili karmaşık görevleri, Azure Blob Depolama tarafından kullanıma sunulan stili arabirimi (gerektirdiği gibi Hadoop dosya sistemi arabirimi) semantiği, nesne depolamak gerçekleştirdi. Bu sürücü, Blob'larda depolanan verileri yüksek performanslı erişim sağlayan, bu model desteklemeye devam eder ancak önemli miktarda sürdürülmesi zor hale getirme, bu eşlemeyi gerçekleştiren kod içerir. Ayrıca, bazı işlemler gibi [FileSystem.rename()](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_renamePath_src_Path_d) ve [FileSystem.delete()](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/filesystem/filesystem.html#boolean_deletePath_p_boolean_recursive) dizinleri uygulandığında geniş birçok (nesne depoları yetersizliği nedeniyle işlemi gerçekleştirmek için bir sürücü gerektirir Destek dizinler için), genellikle azaltılmış performansa neden olur. ABFS sürücü WASB, devralınan eksiklikleri üstesinden gelmek için tasarlanmıştır.

## <a name="the-azure-blob-file-system-driver"></a>Azure Blob dosya sistemi sürücüsü

[Azure Data Lake depolama REST arabirimi](https://docs.microsoft.com/rest/api/storageservices/data-lake-storage-gen2) Azure Blob Depolama üzerinde dosya sistemi sematiğini desteklemek için tasarlanmıştır. Hadoop dosya sistemi ile aynı semantiğe desteklemek için de tasarlanmıştır koşuluyla, sürücü, karmaşık bir eşleme gereksinimi yoktur. Bu nedenle, Azure Blob dosya sistemi sürücüsü (veya ABFS), REST API'LERİNE ilişkin, yalnızca istemci dolgu değil.

Ancak, sürücü gerçekleştirmek gereken bazı işlevleri vardır:

### <a name="uri-scheme-to-reference-data"></a>Başvuru verileri için URI şeması

Kaynaklar (dizinler ve dosyalar) sonuçlanmaz ele alınması Hadoop içinde başka bir dosya sistemi uygulamaları ile tutarlı olmasını sağlamak, ABFS sürücü kendi URI şeması tanımlar. URI şeması belgelenen [Azure Data Lake depolama Gen2 URI'si kullanma](./data-lake-storage-introduction-abfs-uri.md). URI'ın yapıdır: `abfs[s]://file_system@account_name.dfs.core.windows.net/<path>/<path>/<file_name>`

Yukarıdaki URI biçimi kullanarak, standart Hadoop araçları ve çerçeveleri bu kaynakları başvurmak için kullanılabilir:

```bash
hdfs dfs -mkdir -p abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data 
hdfs dfs -put flight_delays.csv abfs://fileanalysis@myanalytics.dfs.core.windows.net/tutorials/flightdelays/data/ 
```

Dahili olarak ABFS sürücü dosyalara ve dizinlere URI belirtilen kaynaklar çevirir ve bu başvuruları ile Azure Data Lake depolama REST API çağrıları yapar.

### <a name="authentication"></a>Authentication

Hadoop application Data Lake depolama Gen2'ye yeteneğine sahip bir hesap içinde yer alan kaynaklara güvenle erişin, böylece iki tür kimlik doğrulaması ABFS sürücü destekler. Kullanılabilir kimlik doğrulama düzenleri tam ayrıntıları verilmiştir [Azure depolama Güvenlik Kılavuzu](../common/storage-security-guide.md). Bunlar:

- **Paylaşılan anahtar:** Bu kullanıcıların hesaptaki tüm kaynaklara erişim izni verir. Anahtar şifrelenir ve Hadoop yapılandırmasında depolanır.

- **Azure Active Directory OAuth taşıyıcı belirteci:** Azure AD taşıyıcı belirteçleri alınan ve son kullanıcı kimliğini veya yapılandırılmış bir hizmet sorumlusu kullanarak sürücü tarafından yenilenir. Bu kimlik doğrulama modeli kullanarak, çağrı başına temelinde atanan POSIX erişim denetimi listesi (ACL) karşı değerlendirilir ve sağlanan belirteçle ilişkili kimliği kullanarak tüm erişim yetkisi.

### <a name="configuration"></a>Yapılandırma

Tüm yapılandırma ABFS sürücü için depolanan <code>core-site.xml</code> yapılandırma dosyası. Hadoop dağıtımlarında özelliklerine sahip [Ambari](https://ambari.apache.org/), yapılandırma de Ambari REST API ve web portalı kullanılarak yönetilebilir.

Tüm desteklenen yapılandırma girdileri ayrıntılarını belirtilir [resmi Hadoop belgeleri](https://hadoop.apache.org/docs/current/hadoop-azure/index.html).

### <a name="hadoop-documentation"></a>Hadoop belgeleri

ABFS sürücü tam olarak belgelenen [resmi Hadoop belgeleri](https://github.com/apache/hadoop/blob/trunk/hadoop-tools/hadoop-azure/src/site/markdown/abfs.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Databricks kümesine oluşturma](./data-lake-storage-quickstart-create-databricks-account.md)
- [Azure Data Lake Storage 2. Nesil URI'sini kullanma](./data-lake-storage-introduction-abfs-uri.md)
