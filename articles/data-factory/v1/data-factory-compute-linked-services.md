---
title: Azure Data Factory ile desteklenen ortamlar işlem | Microsoft Docs
description: Dönüştürme veya işlem verileri (örneğin, Azure Hdınsight) Azure Data Factory işlem hatlarını kullanabileceğiniz bilgi işlem ortamları hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 6877a7e8-1a58-4cfb-bbd3-252ac72e4145
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 51a0f43587b9d34a3693eb4a2927d10c71bd95d1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34621760"
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>Azure Data Factory ile desteklenen ortamlar işlem
> [!NOTE]
> Bu makale, Azure Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [işlem bağlı Hizmetleri sürüm 2](../compute-linked-services.md).

Bu makalede kullanabileceğiniz bilgi işlem ortamları için işlem veya dönüştürme veri açıklanmaktadır. Ayrıca, bu bağlantı bağlı hizmetler yapılandırdığınızda, veri fabrikası destekler ortamları için bir Azure data factory işlem farklı yapılandırmaları (isteğe bağlı Getir kendi karşı) ilgili ayrıntıları da sağlar.

Aşağıdaki tabloda, veri fabrikası ve bunlar üzerinde çalışan etkinlikleri tarafından desteklenen bilgi işlem ortamları listesini sağlar. 

| İşlem ortamı                      | Etkinlikler                               |
| ---------------------------------------- | ---------------------------------------- |
| [İsteğe bağlı Azure Hdınsight kümesi](#azure-hdinsight-on-demand-linked-service) veya [kendi Hdınsight kümenizi](#azure-hdinsight-linked-service) | [DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop akış](data-factory-hadoop-streaming-activity.md) |
| [Azure Batch](#azure-batch-linked-service) | [DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) | [Machine Learning etkinlikleri: Toplu Yürütme ve Kaynak Güncelleştirme](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) | [Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Azure SQL](#azure-sql-linked-service), [Azure SQL veri ambarı](#azure-sql-data-warehouse-linked-service), [SQL Server](#sql-server-linked-service) | [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md) |

## <a name="supported-hdinsight-versions-in-azure-data-factory"></a>Veri fabrikasında desteklenen Hdınsight sürümleri
Azure Hdınsight herhangi bir zamanda dağıtabilirsiniz birden çok Hadoop küme sürümlerindeki destekler. Desteklenen her sürümü dağıtımlarında Hortonworks veri Platformu (HDP) dağıtım belirli bir sürümü ve bir bileşen kümesi oluşturur. 

Microsoft en son Hadoop ekosistemi bileşenlerini ve düzeltmeler ile Desteklenen sürümlerin listesi için Hdınsight güncelleştirir. Ayrıntılı bilgi için bkz: [desteklenen Hdınsight sürümleri](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

> [!IMPORTANT]
> Linux tabanlı Hdınsight sürüm 3.3 31 Temmuz 2017 devre dışı bırakılan. Veri Fabrikası sürüm 1 müşteriler 15 Aralık, test ve Hdınsight sonraki bir sürüme yükseltmek için 2017 kadar verildi Hizmetleri isteğe bağlı Hdınsight bağlı. Windows tabanlı Hdınsight 31 Temmuz 2018 kullanımdan kaldırılacaktır.
>
> 

### <a name="after-the-retirement-date"></a>O tarihten sonra 

15 Aralık 2017 sonra:

- Linux tabanlı Hdınsight sürüm 3.3 (veya önceki sürümler) artık oluşturabilirsiniz kümeleri kullanarak bir isteğe bağlı Hdınsight bağlı hizmeti veri fabrikasında sürüm 1. 
- Varsa [ **osType** ve **sürüm** özellikleri](https://docs.microsoft.com/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) varolan bir veri fabrikası sürüm 1 isteğe bağlı Hdınsight bağlı hizmeti için JSON tanımında açıkça belirtilmedi , varsayılan değer değiştirilirse **sürüm 3.1, osType = Windows =** için **sürümü =\<son HDI varsayılan sürümü\>(https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#hadoop-components-available-with-different-hdinsight-versions), osType Linux =**.

31 Temmuz 2018 sonra:

- Veri Fabrikası sürüm 1 isteğe bağlı Hdınsight bağlı hizmeti kullanarak artık Windows tabanlı Hdınsight kümeleri herhangi bir sürümünü oluşturabilirsiniz. 

### <a name="recommended-actions"></a>Önerilen eylemler 

- En son düzeltmeler ve Hadoop ekosistemi bileşenlerini kullandığınızdan emin olmak için güncelleştirme [ **osType** ve **sürüm** özellikleri](https://docs.microsoft.com/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) etkilenen veri fabrikası sürüm 1 isteğe bağlı olarak Hdınsight bağlı hizmeti tanımları yeni Linux tabanlı Hdınsight sürümleri (Hdınsight 3.6). 
- 15 Aralık 2017 önce etkilenen bağlantılı hizmet başvurusu Data Factory sürüm 1 Hive, Pig, MapReduce ve Hadoop akış etkinlikleri sınayın. Yeni ile uyumlu olduklarından emin olmak **osType** ve **sürüm** varsayılan değeri (**sürüm 3.6 =**, **osType Linux =**) veya Açık Hdınsight sürümü ve işletim sistemi yükseltme yapıyorsanız yazın. 
  Uyumluluğu hakkında daha fazla bilgi için bkz: [Linux tabanlı bir kümeye bir Windows tabanlı Hdınsight kümeden geçiş](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-from-windows-to-linux) ve [Hadoop bileşenleri ve Hdınsight ile kullanılabilir sürümlerini nelerdir?](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#hortonworks-release-notes-associated-with-hdinsight-versions). 
- Windows tabanlı Hdınsight kümeleri oluşturmak için bir veri fabrikası sürüm 1 isteğe bağlı Hdınsight bağlı hizmeti kullanmaya devam etmek için açık olarak ayarlanıp **osType** için **Windows** 15 Aralık 2017 önce. Linux tabanlı Hdınsight kümelerine 31 Temmuz 2018 önce geçirmek öneririz. 
- Veri Fabrikası sürüm 1 yürütmek için bir isteğe bağlı Hdınsight bağlı hizmeti kullanıyorsanız DotNet özel etkinlik, bunun yerine bir Azure Batch kullanılacak DotNet özel etkinlik JSON tanımını bağlantılı güncelleştirme hizmeti. Daha fazla bilgi için bkz: [bir Data Factory işlem hattı özel etkinlikleri kullanmak](https://docs.microsoft.com/azure/data-factory/v1/data-factory-use-custom-activities). 

> [!Note]
> Var olan kullanırsanız, veri fabrikası sürüm 1 aygıtı Getir kendi küme Hdınsight bağlantılı veya Getir kendi ve isteğe bağlı Hdınsight bağlı hizmeti Azure Data factory'de sürüm 2, hiçbir eylem gerekli değildir. Bu senaryolarda, Hdınsight kümeleri, en son sürüm destek ilkesi zaten uygulanır. 
>
> 


## <a name="on-demand-compute-environment"></a>İsteğe bağlı bilgi işlem ortamı
Bir isteğe bağlı yapılandırma Data Factory işlem ortamı tam olarak yönetir. Bir iş verilerini işlemek için gönderilmeden önce data Factory işlem ortamı otomatik olarak oluşturur. İş tamamlandığında, veri fabrikası bilgi işlem ortamı kaldırır. 

İsteğe bağlı bilgi işlem ortamı için bağlı hizmet oluşturabilirsiniz. Bağlantılı hizmeti, bilgi işlem ortamı yapılandırmak ve iş yürütme, küme yönetim ve eylemler önyükleme ayrıntılı ayarlarını denetlemek için kullanın.

> [!NOTE]
> Şu anda, isteğe bağlı yapılandırma yalnızca Hdınsight kümeleri için desteklenir.
> 

## <a name="azure-hdinsight-on-demand-linked-service"></a>Azure Hdınsight isteğe bağlı hizmeti
Veri Fabrikası otomatik olarak verileri işlemek için bir Windows veya Linux tabanlı isteğe bağlı Hdınsight kümesi oluşturabilirsiniz. Küme kümeyle ilişkili depolama hesabı ile aynı bölgede oluşturulur. JSON kullanmak **linkedServiceName** küme oluşturmak için özellik.

Aşağıdakilere dikkat edin *anahtar* noktaları hakkında isteğe bağlı Hdınsight bağlı hizmeti:

* İsteğe bağlı Hdınsight kümesi Azure aboneliğinizde görünmüyor. Data Factory hizmetinin sizin adınıza isteğe bağlı Hdınsight kümesi yönetir.
* Bir isteğe bağlı Hdınsight kümesinde çalışan işleri için günlükleri, Hdınsight kümesi ile ilişkilendirilmiş depolama hesabına kopyalanır. Azure portalında Bu günlükler erişmek için Git **etkinlik çalışma ayrıntıları** bölmesi. Daha fazla bilgi için bkz: [İzleyici ve ardışık düzen yönetmek](data-factory-monitor-manage-pipelines.md).
* Yalnızca Hdınsight kümesi çalışıyor zaman ve çalışan işleri için sizden ücret kesilir.

> [!IMPORTANT]
> Genellikle sürer *20 dakika* veya isteğe bağlı Hdınsight kümesi sağlamak için daha fazla bilgi.
>
> 

### <a name="example"></a>Örnek
Aşağıdaki JSON Linux tabanlı isteğe bağlı Hdınsight bağlı hizmeti tanımlar. Veri Fabrikası otomatik olarak oluşturur bir *Linux tabanlı* veri dilimi işlediğinde, Hdınsight kümesi. 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.6",
            "osType": "Linux",
            "clusterSize": 1,
            "timeToLive": "00:05:00",            
            "linkedServiceName": "AzureStorageLinkedService"
        }
    }
}
```

> [!IMPORTANT]
> Hdınsight kümesi oluşturur bir *varsayılan kapsayıcı* JSON'da belirttiğiniz Azure Blob depolamada **linkedServiceName** özelliği. Tasarım gereği, küme silindiğinde bu kapsayıcı Hdınsight silmez. Mevcut canlı bir küme olmadıkça bir dilim işlenmesi gereken her zaman bir isteğe bağlı Hdınsight bağlı hizmeti bir Hdınsight kümesi oluşturulur (**timeToLive**). Küme, işlem bittiğinde silinir. 
>
> Daha fazla dilim işlendikçe, çok sayıda kapsayıcı, Blob depolama alanına bakın. İşlerini sorun giderme için kapsayıcıları ihtiyaç duymuyorsanız depolama maliyetini azaltmak için kapsayıcılara silmek isteyebilirsiniz. Bu kapsayıcı adları bir düzene sahiptir: `adf<your Data Factory name>-<linked service name>-<date and time>`. Gibi bir araç kullanabilirsiniz [Microsoft Storage Gezgini](http://storageexplorer.com/) Blob depolamada kapsayıcı silmek için.
>
> 

### <a name="properties"></a>Özellikler
| Özellik                     | Açıklama                              | Gerekli |
| ---------------------------- | ---------------------------------------- | -------- |
| type                         | Type özelliği ayarlamak **HDInsightOnDemand**. | Evet      |
| clusterSize                  | Kümedeki çalışan ve veri düğüm sayısı. Hdınsight kümesi için bu özelliği belirtmeniz çalışan düğüm sayısı ek olarak 2 baş düğümler ile oluşturulur. Düğümlerin boyutu 4 çekirdeğe sahip Standard_D3 ' dir. Çalışan 4 düğümlü bir küme 24 çekirdek alır (4\*4 = 16 çekirdek çalışan düğümleri artı 2\*4 = 8 çekirdek baş düğümler için). Standart D3 katmanı hakkında daha fazla ayrıntı için bkz: [Hdınsight oluşturma Linux tabanlı Hadoop kümeleri](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md). | Evet      |
| timeToLive                   | İsteğe bağlı Hdınsight kümesi için izin verilen boşta kalma süresi. Bir etkinlik Çalıştır bittiğinde, kümedeki herhangi bir etkin işler varsa ne kadar süreyle isteğe bağlı Hdınsight kümesi Canlı kalır belirtir.<br /><br />Örneğin, bir etkinlik çalıştırırsanız 6 dakika sürer ve **timeToLive** 5 dakika, 5 dakika 6 etkinliğin çalışma işleme dakika sonra canlı küme kalır ayarlanır. Başka bir etkinlik 6 dakika penceresinde yürütülürse, aynı küme tarafından işlenir.<br /><br />İsteğe bağlı Hdınsight kümesi oluşturma (Bu işlem biraz zaman alabilir) pahalı bir işlemdir. İsteğe bağlı Hdınsight kümesi yeniden kullanarak bir veri fabrikası performansını artırmak için gerektiği gibi bu ayarı kullanın.<br /><br />Ayarlarsanız **timeToLive** değeri **0**, küme sonlandığında etkinliği çalıştırmak hemen silinir. Ancak, yüksek bir değer ayarlarsanız, kümenin yüksek maliyetlerini gereksiz yere kaynaklanan boşta kalabilir. Gereksinimlerinize göre uygun değere ayarlanmış olması önemlidir.<br /><br />Varsa **timeToLive** değerini uygun şekilde ayarlayın, birden çok ardışık düzen isteğe bağlı Hdınsight kümesi örneğini paylaşabilirsiniz. | Evet      |
| sürüm                      | Hdınsight küme sürümü. İzin verilen Hdınsight sürümleri için bkz: [desteklenen Hdınsight sürümleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#supported-hdinsight-versions). Bu değer belirtilmezse, [son HDI varsayılan sürümü](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#hadoop-components-available-with-different-hdinsight-versions) kullanılır. | Hayır       |
| linkedServiceName            | Depolamak ve veri işleme için isteğe bağlı küme tarafından kullanılacak Azure Storage bağlı hizmeti. Hdınsight kümesi, bu depolama hesabı ile aynı bölgede oluşturulur.<p>Şu anda, depolama alanı olarak Azure Data Lake Store kullanan bir isteğe bağlı Hdınsight kümesi oluşturulamıyor. Data Lake Store'da işleme Hdınsight sonuç verileri depolamak Blob depolama alanından Data Lake Store'a veri kopyalamak için kopyalama etkinliği kullanın. </p> | Evet      |
| additionalLinkedServiceNames | Hdınsight bağlı hizmeti için ek depolama hesapları belirtir. Veri Fabrikası depolama hesapları, sizin adınıza kaydeder. Bu depolama hesapları, Hdınsight kümesi ile aynı bölgede olması gerekir. Hdınsight kümesi tarafından belirtilen depolama hesabı ile aynı bölgede oluşturulan **linkedServiceName** özelliği. | Hayır       |
| osType                       | İşletim sistemi türü. İzin verilen değerler **Linux** ve **Windows**. Bu değer belirtilmezse, **Linux** kullanılır.  <br /><br />Linux tabanlı Hdınsight kümeleri kullanarak öneririz. Windows'da Hdınsight için devre dışı bırakma 31 Temmuz 2018 tarihidir. | Hayır       |
| hcatalogLinkedServiceName    | HCatalog veritabanına işaret eden Azure SQL bağlı hizmetin adı. İsteğe bağlı Hdınsight kümesi meta depo SQL veritabanı kullanılarak oluşturulur. | Hayır       |

#### <a name="example-linkedservicenames-json"></a>Örnek: LinkedServiceNames JSON

```json
"additionalLinkedServiceNames": [
    "otherLinkedServiceName1",
    "otherLinkedServiceName2"
  ]
```

### <a name="advanced-properties"></a>Gelişmiş Özellikler
İsteğe bağlı Hdınsight kümesinin ayrıntılı yapılandırma için aşağıdaki özellikleri belirtebilirsiniz:

| Özellik               | Açıklama                              | Gerekli |
| :--------------------- | :--------------------------------------- | :------- |
| coreConfiguration      | Çekirdek yapılandırma parametreleri (core-site.xml) oluşturulacak Hdınsight kümesi için belirtir. | Hayır       |
| hBaseConfiguration     | Hdınsight kümesi için HBase yapılandırma parametreleri (hbase-site.xml) belirtir. | Hayır       |
| hdfsConfiguration      | Hdınsight kümesi için HDFS yapılandırma parametreleri (hdfs-site.xml) belirtir. | Hayır       |
| hiveConfiguration      | Hdınsight kümesi için Hive yapılandırma parametreleri (hive-site.xml) belirtir. | Hayır       |
| mapReduceConfiguration | Hdınsight kümesi için MapReduce yapılandırma parametreleri (mapred-site.xml) belirtir. | Hayır       |
| oozieConfiguration     | Hdınsight kümesi için Oozie yapılandırma parametreleri (oozie-site.xml) belirtir. | Hayır       |
| stormConfiguration     | Hdınsight kümesi için Storm yapılandırma parametreleri (storm-site.xml) belirtir. | Hayır       |
| yarnConfiguration      | Hdınsight kümesi için YARN yapılandırma parametreleri (yarn-site.xml) belirtir. | Hayır       |

#### <a name="example-on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>Örnek: İsteğe bağlı Hdınsight Küme Yapılandırması Gelişmiş özellikleri

```json
{
  "name": " HDInsightOnDemandLinkedService",
  "properties": {
    "type": "HDInsightOnDemand",
    "typeProperties": {
      "version": "3.6",
      "osType": "Linux",
      "clusterSize": 16,
      "timeToLive": "01:30:00",
      "linkedServiceName": "adfods1",
      "coreConfiguration": {
        "templeton.mapper.memory.mb": "5000"
      },
      "hiveConfiguration": {
        "templeton.mapper.memory.mb": "5000"
      },
      "mapReduceConfiguration": {
        "mapreduce.reduce.java.opts": "-Xmx4000m",
        "mapreduce.map.java.opts": "-Xmx4000m",
        "mapreduce.map.memory.mb": "5000",
        "mapreduce.reduce.memory.mb": "5000",
        "mapreduce.job.reduce.slowstart.completedmaps": "0.8"
      },
      "yarnConfiguration": {
        "yarn.app.mapreduce.am.resource.mb": "5000",
        "mapreduce.map.memory.mb": "5000"
      },
      "additionalLinkedServiceNames": [
        "datafeeds",
        "adobedatafeed"
      ]
    }
  }
}
```

### <a name="node-sizes"></a>Düğümü boyutları
Head, veri ve ZooKeeper düğümleri boyutunu belirtmek için aşağıdaki özellikleri kullanın: 

| Özellik          | Açıklama                              | Gerekli |
| :---------------- | :--------------------------------------- | :------- |
| headNodeSize      | Baş düğüm boyutunu ayarlar. Varsayılan değer **Standard_D3**. Ayrıntılar için bkz [düğümü boyutları belirtin](#specify-node-sizes). | Hayır       |
| dataNodeSize      | Veri düğümü boyutunu ayarlar. Varsayılan değer **Standard_D3**. | Hayır       |
| zookeeperNodeSize | ZooKeeper düğüm boyutunu ayarlar. Varsayılan değer **Standard_D3**. | Hayır       |

#### <a name="specify-node-sizes"></a>Düğümü boyutları belirtin
Önceki bölümde açıklanan özellikler için belirtmelisiniz dize değerleri için bkz: [sanal makine boyutlarını](../../virtual-machines/linux/sizes.md). Değerleri cmdlet'leri için uygun olmalıdır ve API başvurulan [sanal makine boyutlarını](../../virtual-machines/linux/sizes.md). (Varsayılan) büyük veri düğüm boyutu 7 GB bellek var. Bu senaryo için yeterli olmayabilir. 

D4 boyutu baş düğümler ve çalışan düğümleri oluşturmak istiyorsanız, belirtin **Standard_D4** değeri olarak **headNodeSize** ve **dataNodeSize** özellikleri: 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Bu özellikler için yanlış bir değere ayarlarsanız, şu iletiyi görebilirsiniz:

  Küme oluşturma başarısız oldu. Özel durum: Küme oluşturma işlemi tamamlanamadı. İşlem '400' koduyla başarısız oldu. Küme geride bırakma durumu: 'Hata'. İleti: 'PreClusterCreationValidationFailure'. 
  
Bu iletiyi görürseniz, cmdlet ve API adları tablosundan kullandığınızdan emin olun [sanal makine boyutlarını](../../virtual-machines/linux/sizes.md).  

> [!NOTE]
> Şu anda, veri fabrikası Data Lake Store birincil deposu olarak kullanmak Hdınsight kümeleri desteklemiyor. Azure Storage Hdınsight kümeleri için birincil depolama alanı olarak kullanın. 
>
> 


## <a name="bring-your-own-compute-environment"></a>Getir kendi bilgi işlem ortamı
Var olan bir bilgi işlem ortamı veri fabrikasında bağlı hizmet olarak kaydedebilirsiniz. Bilgi işlem ortamı yönetin. Data Factory hizmetinin işlem ortamı etkinlikleri yürütmek için kullanır.

Bu tür bir yapılandırma için aşağıdaki bilgi işlem ortamları desteklenir:

* Azure Hdınsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Azure SQL veritabanı, Azure SQL Data Warehouse, SQL Server

## <a name="azure-hdinsight-linked-service"></a>Azure Hdınsight bağlı hizmeti
Kendi Hdınsight kümenizi Data Factory ile kaydetmek için bir Hdınsight bağlı hizmeti oluşturabilirsiniz.

### <a name="example"></a>Örnek

```json
{
  "name": "HDInsightLinkedService",
  "properties": {
    "type": "HDInsight",
    "typeProperties": {
      "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
      "userName": "admin",
      "password": "<password>",
      "linkedServiceName": "MyHDInsightStoragelinkedService"
    }
  }
}
```

### <a name="properties"></a>Özellikler
| Özellik          | Açıklama                              | Gerekli |
| ----------------- | ---------------------------------------- | -------- |
| type              | Type özelliği ayarlamak **Hdınsight**. | Evet      |
| clusterUri        | Hdınsight küme URI'si.        | Evet      |
| kullanıcı adı          | Olan bir Hdınsight kümesine bağlanmak için kullanılacak kullanıcı hesabının adı. | Evet      |
| password          | Kullanıcı hesabının parolası.   | Evet      |
| linkedServiceName | Hdınsight küme tarafından kullanılan Blob Depolama başvurduğu depolama bağlı hizmetin adı. <p>Şu anda bu özellik için bir Data Lake Store bağlı belirtemezsiniz. Hdınsight kümesi için Data Lake Store erişimi varsa, Data Lake Store'da verilerin Hive veya Pig komut dosyalarından erişebilir. </p> | Evet      |

## <a name="azure-batch-linked-service"></a>Azure Batch bağlı hizmeti
Batch havuzundaki bir veri fabrikası için sanal makineleri (VM'ler) kaydetmek için bağlantılı Batch hizmeti oluşturabilirsiniz. Microsoft .NET özel etkinlikler toplu veya Hdınsight kullanarak çalıştırabilirsiniz.

Batch hizmeti kullanmaya yeni başladıysanız:

* Hakkında bilgi edinin [Azure Batch temel bilgileri](../../batch/batch-technical-overview.md).
* Hakkında bilgi edinin [yeni AzureBatchAccount](https://msdn.microsoft.com/library/mt125880.aspx) cmdlet'i. Batch hesabı oluşturmak için bu cmdlet'i kullanın. Ya da toplu işlem hesabı kullanarak oluşturabileceğiniz [Azure portal](../../batch/batch-account-create-portal.md). Cmdlet kullanma hakkında ayrıntılı bilgi için bkz: [Batch hesabını yönetmek için PowerShell kullanarak](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx).
* Hakkında bilgi edinin [New-AzureBatchPool](https://msdn.microsoft.com/library/mt125936.aspx) cmdlet'i. Bir toplu işlem havuzu oluşturmak için bu cmdlet'i kullanın.

### <a name="example"></a>Örnek

```json
{
  "name": "AzureBatchLinkedService",
  "properties": {
    "type": "AzureBatch",
    "typeProperties": {
      "accountName": "<Azure Batch account name>",
      "accessKey": "<Azure Batch account key>",
      "poolName": "<Azure Batch pool name>",
      "linkedServiceName": "<Specify associated storage linked service reference here>"
    }
  }
}
```

İçin **accountName** özelliği, append **.\< bölge adı\>**  batch hesabınızın adını. Örneğin:

```json
"accountName": "mybatchaccount.eastus"
```

Başka bir seçenek sağlamaktır **batchUri** uç noktası. Örneğin:

```json
"accountName": "adfteam",
"batchUri": "https://eastus.batch.azure.com",
```

### <a name="properties"></a>Özellikler
| Özellik          | Açıklama                              | Gerekli |
| ----------------- | ---------------------------------------- | -------- |
| type              | Type özelliği ayarlamak **AzureBatch**. | Evet      |
| accountName       | Toplu işlem hesabının adı.         | Evet      |
| accessKey         | Toplu işlem hesabı için erişim anahtarı.  | Evet      |
| poolName          | VM'nin havuzunun adı.    | Evet      |
| linkedServiceName | Bağlantılı hizmetinin bu toplu işlemle ilişkili depolama bağlı hizmetin adı. Bu bağlı hizmetin etkinliği çalıştırmak ve Etkinlik yürütme günlüklerini depolamak için gerekli olan hazırlama dosyalar için kullanılır. | Evet      |

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning bağlı hizmeti
Bir veri fabrikası Puanlama uç noktası bir Machine Learning toplu kaydetmek için Machine Learning bağlantılı hizmeti oluşturabilirsiniz.

### <a name="example"></a>Örnek

```json
{
  "name": "AzureMLLinkedService",
  "properties": {
    "type": "AzureML",
    "typeProperties": {
      "mlEndpoint": "https://[batch scoring endpoint]/jobs",
      "apiKey": "<apikey>"
    }
  }
}
```

### <a name="properties"></a>Özellikler
| Özellik   | Açıklama                              | Gerekli |
| ---------- | ---------------------------------------- | -------- |
| Tür       | Type özelliği ayarlamak **AzureML**. | Evet      |
| mlEndpoint | Toplu Puanlama URL.                   | Evet      |
| apikey ile yapılan     | Yayımlanan çalışma alanı modelinin API.     | Evet      |

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics bağlı hizmeti
Data Lake Analytics işlem hizmeti bir Azure data factory'ye bağlamak için Data Lake Analytics bağlantılı hizmeti oluşturabilirsiniz. Data Lake Analytics U-SQL etkinliği ardışık düzeninde bu bağlı hizmetin başvuruyor. 

Aşağıdaki tabloda JSON tanımında kullanılan genel özelliklerini açıklamaktadır:

| Özellik                 | Açıklama                              | Gerekli                                 |
| ------------------------ | ---------------------------------------- | ---------------------------------------- |
| type                 | Type özelliği ayarlamak **AzureDataLakeAnalytics**. | Evet                                      |
| accountName          | Data Lake Analytics hesap adı.  | Evet                                      |
| dataLakeAnalyticsUri | Data Lake Analytics URI.           | Hayır                                       |
| subscriptionId       | Azure abonelik kimliği                    | Hayır<br /><br />(Belirtilmezse, veri fabrikası abonelik kullanılır.) |
| resourceGroupName    | Azure kaynak grubu adı.                | Hayır<br /><br /> (Belirtilmezse, veri fabrikası kaynak grubu kullanılır.) |

### <a name="authentication-options"></a>Kimlik doğrulaması seçenekleri
Data Lake Analytics bağlı hizmetiniz için bir hizmet sorumlusu veya kullanıcı kimlik bilgileri kullanılarak kimlik doğrulaması arasından seçim yapabilirsiniz.

#### <a name="service-principal-authentication-recommended"></a>(Önerilen) hizmet asıl kimlik doğrulaması
Hizmet asıl kimlik doğrulaması kullanmak için Azure Active Directory (Azure AD) bir uygulama varlığı kaydedin. Ardından, Data Lake Store için Azure AD erişim. Ayrıntılı adımlar için bkz: [hizmeti için kimlik doğrulama](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Bağlantılı hizmet tanımlamak için kullandığınız aşağıdaki değerleri not edin:
* Uygulama Kimliği
* Uygulama anahtarı 
* Kiracı Kimliği

Hizmet asıl kimlik doğrulaması, aşağıdaki özellikleri belirterek kullanın:

| Özellik                | Açıklama                              | Gerekli |
| :---------------------- | :--------------------------------------- | :------- |
| servicePrincipalId  | Uygulamanın istemci kimliği     | Evet      |
| servicePrincipalKey | Uygulamanın anahtar.           | Evet      |
| kiracı              | Uygulamanızın bulunduğu Kiracı bilgileri (etki alanı adı veya Kiracı kimliği). Bu bilgileri almak için Azure portalının sağ üst köşesinde farenizi gelin. | Evet      |

**Örnek: Hizmet asıl kimlik doğrulaması**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="user-credential-authentication"></a>Kullanıcı kimlik bilgileri doğrulaması
Data Lake Analytics için kullanıcı kimlik bilgileri doğrulaması için aşağıdaki özellikleri belirtin:

| Özellik          | Açıklama                              | Gerekli |
| :---------------- | :--------------------------------------- | :------- |
| Yetkilendirme | Veri Fabrikası Düzenleyicisi'nde seçin **Authorize** düğmesi. Bu özelliği otomatik olarak oluşturulur yetkilendirme URL'si atar kimlik bilgisi girin. | Evet      |
| SessionID     | OAuth yetkilendirme oturumundan OAuth oturum kimliği. Her oturum kimliği benzersiz olup yalnızca bir kez kullanılabilir. Data Factory Düzenleyici kullandığınızda bu ayarı otomatik olarak oluşturulur. | Evet      |

**Örnek: Kullanıcı kimlik bilgileri doğrulaması**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a>Belirteç süre sonu
Seçerek oluşturulan yetkilendirme kodu **Authorize** düğmesi kümesi aralığından sonra süresi dolar. 

Kimlik doğrulama belirteci sona erdiğinde, aşağıdaki hata iletisini görebilirsiniz: 

  Kimlik bilgisi işlemi hatası: invalid_grant - AADSTS70002: Kimlik doğrulanırken hata oluştu. AADSTS70008: Sağlanan erişim izninin süresi doldu veya iptal edildi. İzleme kimliği: d18629e8-af88-43c5-88e3-d8419eb1fca1 bağıntı kimliği: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 zaman damgası: 2015-12-15 21:09:31Z

Aşağıdaki tabloda kullanıcı hesap türüne göre süre sonu gösterilmektedir: 

| Kullanıcı türü                                | Kullanım süresi sonu                            |
| :--------------------------------------- | :--------------------------------------- |
| Olan kullanıcı hesaplarını *değil* Azure AD tarafından (Hotmail, canlı ve benzeri) yönetilen | 12 saat.                                 |
| Kullanıcı hesapları *olan* Azure AD tarafından yönetilen | 14 gün sonra en son dilim çalıştırın. <br /><br />90 gün temel alan bir dilim OAuth tabanlı bağlantılı hizmet 14 günde bir en az bir kez çalıştırır. |

Önlemenize veya bu hatayı gidermek için seçerek yeniden yetkilendirin **Authorize** belirtecinin süresi dolduğunda düğmesine tıklayın. Ardından, bağlantılı hizmeti yeniden dağıtın. Değerleri de oluşturabilirsiniz **SessionID** ve **yetkilendirme** aşağıdaki kodu kullanarak program aracılığıyla özellikleri:

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

Bu kod örneğinde kullanılan veri fabrikası sınıfları hakkında daha fazla bilgi için bkz:
* [AzureDataLakeStoreLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
* [AzureDataLakeAnalyticsLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* [AuthorizationSessionGetResponse sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx)

Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll için bir başvuru ekleyin **WindowsFormsWebAuthenticationDialog** sınıfı. 

## <a name="azure-sql-linked-service"></a>Azure SQL bağlı hizmeti
SQL bağlı hizmeti oluşturma ve onunla kullanmak [saklı yordam etkinliği](data-factory-stored-proc-activity.md) Data Factory işlem hattı bir saklı yordam çağırmak için. Daha fazla bilgi için bkz: [Azure SQL bağlayıcı](data-factory-azure-sql-connector.md#linked-service-properties).

## <a name="azure-sql-data-warehouse-linked-service"></a>Azure SQL Data Warehouse bağlı hizmeti
Bir SQL Data Warehouse bağlı hizmet oluşturma ve onunla kullanın [saklı yordam etkinliği](data-factory-stored-proc-activity.md) Data Factory işlem hattı bir saklı yordam çağırmak için. Daha fazla bilgi için bkz: [Azure SQL Data Warehouse Bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties).

## <a name="sql-server-linked-service"></a>SQL Server bağlantılı hizmeti
Bir SQL Server bağlantılı hizmet oluşturun ve onunla kullanmak [saklı yordam etkinliği](data-factory-stored-proc-activity.md) Data Factory işlem hattı bir saklı yordam çağırmak için. Daha fazla bilgi için bkz: [SQL Server Bağlayıcısı](data-factory-sqlserver-connector.md#linked-service-properties).

