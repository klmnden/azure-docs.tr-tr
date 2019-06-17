---
title: Azure Data Factory tarafından desteklenen ortam işlem | Microsoft Docs
description: Dönüştürme veya işlem verileri (örneğin, Azure HDInsight) Azure Data Factory işlem hatlarını kullanabileceğiniz işlem ortamları hakkında bilgi edinin.
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
ms.openlocfilehash: 0e0a249c53c90d3d8d03dcdb5fbb4f11f31c54df
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60565727"
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>Azure Data Factory tarafından desteklenen ortam işlem
> [!NOTE]
> Bu makale, Azure Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [işlem bağlı Hizmetleri,](../compute-linked-services.md).

Bu makalede, işlem veya dönüşüm veri kullanabileceğiniz işlem ortamlarını açıklanmaktadır. Ayrıca, Data Factory destekler, bu bağlantı bağlı hizmetler yapılandırdığınızda ortamları için bir Azure data factory işlem farklı yapılandırmaları (talep üzerine getirin kendi karşı) hakkındaki ayrıntılar da sağlar.

Aşağıdaki tabloda Data Factory ve bunlar üzerinde çalışan etkinlikler tarafından desteklenen işlem ortamlarının listesi sağlar. 

| İşlem ortamı                      | Etkinlikler                               |
| ---------------------------------------- | ---------------------------------------- |
| [İsteğe bağlı Azure HDInsight kümesi](#azure-hdinsight-on-demand-linked-service) veya [kendi HDInsight kümenizi](#azure-hdinsight-linked-service) | [DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop akış](data-factory-hadoop-streaming-activity.md) |
| [Azure Batch](#azure-batch-linked-service) | [DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) | [Machine Learning etkinlikleri: Toplu yürütme ve kaynak güncelleştirme](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics'i](#azure-data-lake-analytics-linked-service) | [Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Azure SQL](#azure-sql-linked-service), [Azure SQL veri ambarı](#azure-sql-data-warehouse-linked-service), [SQL Server](#sql-server-linked-service) | [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md) |

## <a name="supported-hdinsight-versions-in-azure-data-factory"></a>Data Factory'de desteklenen HDInsight sürümleri
Azure HDInsight, herhangi bir zamanda dağıtabilirsiniz birden çok Hadoop küme sürümleri destekler. Desteklenen her sürümü, dağıtım Hortonworks Data Platform (HDP) dağıtım belirli bir sürümünü ve eksiksiz bir kümesi oluşturur. 

Microsoft, en yeni Hadoop ekosistemi bileşenleri ve düzeltmeler ile desteklenen HDInsight sürümleri listesini güncelleştirir. Ayrıntılı bilgi için bkz. [HDInsight desteklenen sürümleri](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

> [!IMPORTANT]
> Linux tabanlı HDInsight sürüm 3.3 31 Temmuz 2017 devre dışı bırakılan. Data Factory sürüm 1 Hizmetleri müşterileri, 15 Aralık test etmek ve HDInsight'ın sonraki bir sürüme yükseltmek için 2017'ye kadar verilen isteğe bağlı HDInsight bağlı. Windows tabanlı HDInsight 31 Temmuz 2018'de kullanımdan kaldırılacaktır.
>
> 

### <a name="after-the-retirement-date"></a>O tarihten sonra 

15 Aralık 2017 tarihinden sonra:

- Linux tabanlı HDInsight sürüm 3.3 (veya önceki sürümler) artık oluşturabilirsiniz kümeleri kullanarak isteğe bağlı HDInsight bağlı hizmeti Data Factory sürüm 1. 
- Varsa [ **osType** ve **sürüm** özellikleri](https://docs.microsoft.com/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) açıkça mevcut bir Data Factory sürüm 1 isteğe bağlı HDInsight bağlı hizmeti için JSON tanımında belirtilmedi , varsayılan değeri değiştirildi **sürüm 3.1, osType = Windows =** için **sürümü =\<son HDI varsayılan sürümü\>(https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning), osType = Linux**.

31 Temmuz 2018 sonra:

- Artık, bir isteğe bağlı HDInsight bağlı hizmeti Data Factory sürüm 1 kullanılarak Windows tabanlı HDInsight kümeleri herhangi bir sürümünü oluşturabilirsiniz. 

### <a name="recommended-actions"></a>Önerilen Eylemler 

- En yeni Hadoop ekosistemi bileşenlerini ve düzeltmeleri kullandığınızdan emin olmak için güncelleştirme [ **osType** ve **sürüm** özellikleri](https://docs.microsoft.com/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) etkilenen Data Factory sürüm 1-isteğe bağlı, HDInsight bağlı hizmeti tanımları yeni Linux tabanlı HDInsight sürümlere (HDInsight 3.6). 
- Etkilenen bağlantılı hizmet başvurusu Data Factory sürüm 1 Hive, Pig, MapReduce ve Hadoop akış etkinlikleri 15 Aralık 2017'den önce test edin. Yeni ile uyumlu olduklarından emin olmak **osType** ve **sürüm** varsayılan değeri (**sürümü 3.6 =** , **osType = Linux**) veya açık bir HDInsight sürüm ve işletim sistemi yükseltme yapıyorsanız yazın. 
  Uyumluluk hakkında daha fazla bilgi için bkz: [Linux tabanlı bir küme için bir Windows tabanlı HDInsight kümesi'ten geçiş](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-from-windows-to-linux) ve [Hadoop bileşenleri ve sürümleri HDInsight ile kullanılabilen nelerdir?](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#hortonworks-release-notes-associated-with-hdinsight-versions). 
- Windows tabanlı HDInsight kümeleri oluşturmak için bir Data Factory sürüm 1 isteğe bağlı HDInsight bağlı hizmeti kullanmaya devam etmek için açıkça ayarlanmış **osType** için **Windows** 15 Aralık 2017'den önce. 31 Temmuz 2018'den önce Linux tabanlı HDInsight kümelerine geçirme öneririz. 
- Data Factory sürüm 1 yürütmek için bir isteğe bağlı HDInsight bağlı hizmeti kullanıyorsanız DotNet özel etkinliği, bunun yerine bir Azure Batch'i nasıl kullanacağınızı DotNet özel etkinlik JSON tanımı bağlı güncelleştirme hizmeti. Daha fazla bilgi için [bir Data Factory işlem hattında özel etkinlikler kullanma](https://docs.microsoft.com/azure/data-factory/v1/data-factory-use-custom-activities). 

> [!Note]
> Mevcut kullanırsanız, küme Getir kendi HDInsight bağlantılı cihaz Data Factory sürüm 1 veya Getir kendi ve isteğe bağlı HDInsight bağlı hizmeti Azure Data factory'deki herhangi bir eylemi gerekli değildir. Bu senaryolarda, HDInsight kümelerinin en son sürümü destek ilkesi zaten zorlanır. 
>
> 


## <a name="on-demand-compute-environment"></a>İsteğe bağlı işlem ortamı
İsteğe bağlı bir yapılandırmada, Data Factory işlem ortamını tam olarak yönetir. Veri işleme için bir iş gönderilmeden önce data Factory işlem ortamı otomatik olarak oluşturur. Data Factory, işi tamamlandığında işlem ortamını kaldırır. 

Bağlı hizmet için bir isteğe bağlı işlem ortamı oluşturabilirsiniz. Bağlı hizmet, işlem ortamını yapılandırmak ve iş yürütme, küme yönetimi ve eylemleri önyükleme ayrıntılı ayarlarını denetlemek için kullanın.

> [!NOTE]
> Şu anda isteğe bağlı yapılandırma yalnızca HDInsight kümeleri için desteklenir.
> 

## <a name="azure-hdinsight-on-demand-linked-service"></a>Azure HDInsight isteğe bağlı hizmeti
Veri fabrikası, veri işleme için bir Windows veya Linux tabanlı isteğe bağlı HDInsight kümesi otomatik olarak oluşturabilirsiniz. Küme, kümeyle ilişkili depolama hesabı ile aynı bölgede oluşturulur. JSON kullanın **linkedServiceName** kümeyi oluşturmak için özellik.

Aşağıdakilere dikkat edin *anahtar* noktaları hakkında isteğe bağlı HDInsight bağlı hizmeti:

* İsteğe bağlı HDInsight kümesi, Azure aboneliğinizde görünmez. Data Factory hizmetinin sizin adınıza isteğe bağlı HDInsight kümesi yönetir.
* Bir isteğe bağlı HDInsight kümesi üzerinde çalışan işler için günlüklere HDInsight kümesiyle ilişkili depolama hesabına kopyalanır. Azure portalında bu günlüklerine erişmek için Git **etkinlik çalışma ayrıntıları** bölmesi. Daha fazla bilgi için [izleme ve işlem hatlarını yönetmek](data-factory-monitor-manage-pipelines.md).
* Yalnızca HDInsight kümesi kurma zamanı ve çalışan işleri için ücretlendirilir.

> [!IMPORTANT]
> Genellikle sürer *20 dakika* ya da daha fazla isteğe bağlı bir HDInsight kümesi sağlayın.
>
> 

### <a name="example"></a>Örnek
Aşağıdaki JSON, Linux tabanlı bir isteğe bağlı HDInsight bağlı hizmeti tanımlar. Data Factory tarafından otomatik olarak oluşturulur bir *Linux tabanlı* veri dilimi işlediğinde, HDInsight kümesi. 

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
> HDInsight kümesi oluşturur bir *varsayılan kapsayıcı* JSON'da belirttiğiniz Azure Blob Depolama alanında **linkedServiceName** özelliği. Tasarım gereği, HDInsight, küme silindiğinde bu kapsayıcıyı silmez. Mevcut canlı bir küme olmadığı sürece bir dilim işlenmesi gereken her bir isteğe bağlı HDInsight bağlı hizmeti bir HDInsight kümesi oluşturulur (**timeToLive**). İşleme tamamlandığında, küme silinir. 
>
> Daha fazla dilim işlendikçe, Blob depolamanızda çok sayıda kapsayıcı görürsünüz. İşleri sorun giderme için kapsayıcıları gerekmiyorsa, depolama maliyetini azaltmak için kapsayıcıları silmek isteyebilirsiniz. Bu kapsayıcı adları bir düzene sahiptir: `adf<your Data Factory name>-<linked service name>-<date and time>`. Gibi bir araç kullanabilirsiniz [Microsoft Storage Gezgini](https://storageexplorer.com/) Blob Depolama kapsayıcıları silinemedi.
>
> 

### <a name="properties"></a>Özellikler
| Özellik                     | Açıklama                              | Gerekli |
| ---------------------------- | ---------------------------------------- | -------- |
| türü                         | Type özelliği ayarlanmış **Hdınsightondemand**. | Evet      |
| clusterSize                  | Kümedeki çalışan ve veri düğümleri sayısı. Bu özellik için belirttiğiniz çalışan düğümlerinin sayısını ek olarak 2 baş düğüm ile HDInsight kümesi oluşturulur. Düğümlerin boyutu 4 çekirdek olan işler için standart_d3, cinsindendir. 4-çalışan düğümü küme 24 çekirdek alır (4\*4 = 16 çekirdek çalışan düğümleri artı 2\*baş düğümleri için 4 = 8 çekirdek). İşler için standart_d3 katmanı hakkında daha fazla ayrıntı için bkz: [oluşturma Linux tabanlı Hadoop kümeleri HDInsight](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md). | Evet      |
| timeToLive                   | İsteğe bağlı HDInsight kümesi için izin verilen boşta kalma süresi. Bir etkinlik çalıştırma tamamlandığında kümesinde hiç bir etkin iş olduğunda ne kadar süreyle isteğe bağlı HDInsight kümesi Canlı kalmasını belirtir.<br /><br />Bir etkinlik çalıştırırsanız, örneğin, 6 dakika sürer ve **timeToLive** küme kalır Canlı etkinlik çalıştırma işleme 6 dakika sonra 5 dakika boyunca 5 dakikada ayarlanır. Başka bir etkinlik çalıştırması 6 dakikalık penceresinde yürütülürse, aynı küme tarafından işlenir.<br /><br />Bir isteğe bağlı HDInsight kümesi oluşturma (biraz sürebilir) pahalı bir işlemdir. Bu ayar, bir isteğe bağlı HDInsight kümesi yeniden kullanarak veri fabrikası performansını artırmak için gerektiği şekilde kullanın.<br /><br />Ayarlarsanız **timeToLive** değerini **0**, etkinlik çalıştırılmadan hemen sonra tamamlanmadan küme silinir. Bununla birlikte, yüksek bir değere ayarlarsanız, kümenin yüksek maliyetleri gereksiz yere kaynaklanan boşta kalabilir. Gereksinimlerinize göre uygun değeri ayarlamak önemlidir.<br /><br />Varsa **timeToLive** değeri uygun şekilde ayarlandığında, isteğe bağlı HDInsight kümesi örneğini birden fazla işlem hattını paylaşabilirsiniz. | Evet      |
| version                      | HDInsight küme sürümü. İzin verilen HDInsight sürümleri için bkz: [HDInsight desteklenen sürümleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#supported-hdinsight-versions). Bu değer belirtilmezse, [son HDI varsayılan sürümü](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning) kullanılır. | Hayır       |
| linkedServiceName            | Depolama ve veri işleme için isteğe bağlı küme tarafından kullanılacak Azure depolama bağlı hizmeti. HDInsight kümesi, bu depolama hesabı ile aynı bölgede oluşturulur.<p>Şu anda, Azure Data Lake Store depolama alanı olarak kullanan bir isteğe bağlı HDInsight kümesi oluşturulamıyor. HDInsight Data Lake Store işlem sonuç verileri depolamak istiyorsanız, Data Lake Store için Blob depolama alanından verileri kopyalamak için kopyalama etkinliğini kullanın. </p> | Evet      |
| additionalLinkedServiceNames | HDInsight bağlı hizmeti için ek depolama hesapları belirtir. Veri Fabrikası depolama hesaplarına adınıza kaydeder. Bu depolama hesapları HDInsight kümesi ile aynı bölgede olması gerekir. HDInsight kümesi tarafından belirtilen depolama hesabı ile aynı bölgede oluşturulur **linkedServiceName** özelliği. | Hayır       |
| osType                       | İşletim sistemi türü. İzin verilen değerler **Linux** ve **Windows**. Bu değer belirtilmezse, **Linux** kullanılır.  <br /><br />Linux tabanlı HDInsight kümeleri kullanarak öneririz. 31 Temmuz 2018'den Windows üzerinde HDInsight için kullanımdan kaldırma tarihtir. | Hayır       |
| hcatalogLinkedServiceName    | HCatalog veritabanına işaret eden bir Azure SQL bağlı hizmeti adı. SQL veritabanı meta veri deposu olarak kullanarak isteğe bağlı HDInsight kümesi oluşturulur. | Hayır       |

#### <a name="example-linkedservicenames-json"></a>Örnek: LinkedServiceNames JSON

```json
"additionalLinkedServiceNames": [
    "otherLinkedServiceName1",
    "otherLinkedServiceName2"
  ]
```

### <a name="advanced-properties"></a>Gelişmiş Özellikler
İsteğe bağlı HDInsight kümesi ayrıntılı yapılandırma için aşağıdaki özellikleri belirtebilirsiniz:

| Özellik               | Açıklama                              | Gerekli |
| :--------------------- | :--------------------------------------- | :------- |
| coreConfiguration      | Oluşturulacak HDInsight küme için çekirdek yapılandırma parametrelerini (core-site.xml) belirtir. | Hayır       |
| hBaseConfiguration     | HDInsight kümesi için HBase yapılandırma parametreleri (hbase-site.xml) belirtir. | Hayır       |
| hdfsConfiguration      | HDInsight küme için HDFS yapılandırma parametreleri (hdfs-site.xml) belirtir. | Hayır       |
| hiveConfiguration      | HDInsight kümesi için Hive yapılandırma parametreleri (hive-site.xml) belirtir. | Hayır       |
| mapReduceConfiguration | HDInsight kümesi için MapReduce yapılandırma parametreleri (mapred-site.xml) belirtir. | Hayır       |
| oozieConfiguration     | HDInsight kümesi için Oozie yapılandırma parametreleri (oozie-site.xml) belirtir. | Hayır       |
| stormConfiguration     | HDInsight kümesi (storm-site.xml) Storm yapılandırma parametreleri belirtir. | Hayır       |
| yarnConfiguration      | HDInsight kümesi için YARN yapılandırma parametreleri (yarn-site.xml) belirtir. | Hayır       |

#### <a name="example-on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>Örnek: Gelişmiş Özellikler ile isteğe bağlı HDInsight kümesi yapılandırma

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
Baş, veri ve ZooKeeper düğümleri boyutunu belirtmek için aşağıdaki özellikleri kullanın: 

| Özellik          | Açıklama                              | Gerekli |
| :---------------- | :--------------------------------------- | :------- |
| headNodeSize      | Baş düğüm boyutunu ayarlar. Varsayılan değer **işler için standart_d3**. Ayrıntılar için bkz [düğüm boyutunu belirlemek](#specify-node-sizes). | Hayır       |
| dataNodeSize      | Veri düğümü boyutunu ayarlar. Varsayılan değer **işler için standart_d3**. | Hayır       |
| zookeeperNodeSize | ZooKeeper düğümü boyutunu ayarlar. Varsayılan değer **işler için standart_d3**. | Hayır       |

#### <a name="specify-node-sizes"></a>Düğüm boyutlarını belirtin
Önceki bölümde açıklanan özellikler için belirtmelisiniz dize değerleri için bkz. [sanal makine boyutları](../../virtual-machines/linux/sizes.md). Değerleri cmdlet'lerinde uymalıdır ve API'leri başvuruda [sanal makine boyutları](../../virtual-machines/linux/sizes.md). (Varsayılan) büyük veri düğüm boyutu 7 GB bellek var. Bu senaryonuz için yeterli olmayabilir. 

D4 boyutu baş düğümü ve alt düğümü oluşturmak istiyorsanız, belirtin **işler için standart_d4** değeri olarak **headNodeSize** ve **dataNodeSize** özellikleri: 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Bu özellikler için yanlış bir değere ayarlarsanız, şu iletiyi görebilirsiniz:

  Küme oluşturulamadı. Özel durum: Küme oluşturma işlemi tamamlanamıyor. İşlem '400' koduyla başarısız oldu. Geride bırakma durumu küme: 'Error'. İleti: 'PreClusterCreationValidationFailure'. 
  
Bu iletiyi görürseniz cmdlet ve API adları tablosundan kullandığınızdan emin olun [sanal makine boyutları](../../virtual-machines/linux/sizes.md).  

> [!NOTE]
> Şu anda, Data Factory, HDInsight kümeleri, birincil depolama Data Lake Store kullanma desteklememektedir. Azure depolama, HDInsight kümeleri için birincil deposu olarak kullanırsınız. 
>
> 


## <a name="bring-your-own-compute-environment"></a>İşlem ortamı Getir kendi
Data Factory öğesinde bağlantılı hizmet olarak var olan bir işlem ortamı kaydedebilirsiniz. İşlem ortamı yönettiğiniz. Data Factory hizmetinin etkinlikleri yürütmek için işlem ortamı kullanır.

Bu tür bir yapılandırma için aşağıdaki bilgi işlem ortamları desteklenir:

* Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Azure SQL veritabanı, Azure SQL veri ambarı, SQL Server

## <a name="azure-hdinsight-linked-service"></a>Azure HDInsight bağlı hizmeti
Kendi HDInsight kümenizi Data Factory'ye kaydetmeniz için bir HDInsight bağlı hizmeti oluşturabilirsiniz.

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
| türü              | Type özelliği ayarlanmış **HDInsight**. | Evet      |
| Küme Uri'si        | HDInsight küme URİ'si.        | Evet      |
| username          | Mevcut bir HDInsight kümesine bağlanmak için kullanılacak kullanıcı hesabı adı. | Evet      |
| password          | Kullanıcı hesabının parolası.   | Evet      |
| linkedServiceName | HDInsight küme tarafından kullanılan Blob Depolama'ya başvuran depolama bağlı hizmetin adı. <p>Şu anda, Data Lake Store bağlı hizmeti için bu özelliği belirtilemez. HDInsight kümesi için Data Lake Store erişimi varsa, Hive veya Pig betiklerin Data Lake Store içinde verilere erişebilir. </p> | Evet      |

## <a name="azure-batch-linked-service"></a>Azure Batch bağlı hizmeti
Bir Batch havuzu sanal makineler (VM'ler) bir data factory'ye kaydetmek için bir toplu iş bağlı hizmeti oluşturabilirsiniz. Microsoft .NET özel etkinlikler, toplu veya HDInsight kullanarak çalıştırabilirsiniz.

Batch hizmeti kullanmaya yeni başladıysanız:

* Hakkında bilgi edinin [Azure Batch temel bilgileri](../../batch/batch-technical-overview.md).
* Hakkında bilgi edinin [yeni AzureBatchAccount](https://msdn.microsoft.com/library/mt125880.aspx) cmdlet'i. Bir Batch hesabı oluşturmak için bu cmdlet'i kullanın. Ya da kullanarak Batch hesabı oluşturabilirsiniz [Azure portalında](../../batch/batch-account-create-portal.md). Cmdlet kullanma hakkında ayrıntılı bilgi için bkz. [kullanarak bir Batch hesabını yönetmek için PowerShell](https://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx).
* Hakkında bilgi edinin [New-AzureBatchPool](https://msdn.microsoft.com/library/mt125936.aspx) cmdlet'i. Bir Batch havuzu oluşturmak için bu cmdlet'i kullanın.

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

İçin **accountName** özelliği ekleme **.\< bölge adı\>**  batch hesabınızın adı. Örneğin:

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
| türü              | Type özelliği ayarlanmış **AzureBatch**. | Evet      |
| accountName       | Batch hesabının adı.         | Evet      |
| accessKey         | Batch hesabı için erişim anahtarı.  | Evet      |
| poolName          | VM'lerin havuzunun adı.    | Evet      |
| linkedServiceName | Bu Batch ile ilişkili depolama bağlı hizmetinin adı bağlı hizmeti. Bu bağlı hizmeti, etkinliği çalıştırmak için ve Etkinlik yürütme günlüklerini depolamak için gereken hazırlık dosyalar için kullanılır. | Evet      |

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning bağlı hizmeti
Bir Machine Learning batch Puanlama uç noktası bir data factory'ye kaydetmeniz için bir Machine Learning bağlı hizmeti oluşturabilirsiniz.

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
| Tür       | Type özelliği ayarlanmış **AzureML**. | Evet      |
| mlEndpoint | Toplu işlem Puanlama URL'si.                   | Evet      |
| ApiKey     | Yayımlanan çalışma alanı modelinin API.     | Evet      |

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics bağlı hizmeti
Bir Data Lake Analytics işlem hizmetini bir Azure data factory'ye bağlamak için bir Data Lake Analytics bağlı hizmeti oluşturabilirsiniz. Data Lake Analytics U-SQL etkinliği işlem hattındaki bu bağlı hizmetini ifade eder. 

Aşağıdaki tabloda JSON tanımında kullanılan genel özellikleri açıklanmaktadır:

| Özellik                 | Açıklama                              | Gerekli                                 |
| ------------------------ | ---------------------------------------- | ---------------------------------------- |
| türü                 | Type özelliği ayarlanmış **AzureDataLakeAnalytics**. | Evet                                      |
| accountName          | Data Lake Analytics hesap adı.  | Evet                                      |
| dataLakeAnalyticsUri | The Data Lake Analytics URI.           | Hayır                                       |
| subscriptionId       | Azure abonelik kimliği                    | Hayır<br /><br />(Belirtilmezse, data factory aboneliği kullanılır.) |
| resourceGroupName    | Azure kaynak grubu adı.                | Hayır<br /><br /> (Belirtilmezse, data factory kaynak grubu kullanılır.) |

### <a name="authentication-options"></a>Kimlik doğrulaması seçenekleri
Data Lake Analytics bağlı hizmetinizin hizmet sorumlusu veya kullanıcı kimlik bilgilerini kullanarak kimlik doğrulaması arasından seçim yapabilirsiniz.

#### <a name="service-principal-authentication-recommended"></a>(Önerilen) hizmet sorumlusu kimlik doğrulaması
Hizmet sorumlusu kimlik doğrulaması kullanmak için Azure Active Directory (Azure AD) uygulama varlığın kaydedin. Ardından, Data Lake Store için Azure AD erişim. Ayrıntılı adımlar için bkz. [hizmetten hizmete kimlik doğrulaması](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:
* Uygulama Kimliği
* Uygulama anahtarı 
* Kiracı Kimliği

Hizmet sorumlusu kimlik doğrulaması, aşağıdaki özellikleri belirterek kullanın:

| Özellik                | Açıklama                              | Gerekli |
| :---------------------- | :--------------------------------------- | :------- |
| servicePrincipalId  | Uygulamanın istemci kimliği.     | Evet      |
| serviceprincipalkey değerleri | Uygulamanın anahtarı.           | Evet      |
| tenant              | Uygulamanızın bulunduğu Kiracı bilgileri (etki alanı adı veya Kiracı kimliği). Bu bilgileri almak için Azure portalının sağ üst köşedeki fareyi üzerine gelin. | Evet      |

**Örnek: Hizmet sorumlusu kimlik doğrulaması**
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
| authorization | Data Factory Düzenleyicisi'nde seçin **Authorize** düğmesi. Bu özellik için otomatik olarak oluşturulan yetkilendirme URL'si atar kimlik bilgilerini girin. | Evet      |
| oturum kimliği     | OAuth yetkilendirme oturumundan OAuth oturum kimliği. Her oturum kimliği benzersiz olup yalnızca bir kez kullanılabilir. Bu ayar, Data Factory Düzenleyici kullandığınızda otomatik olarak oluşturulur. | Evet      |

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
Seçerek oluşturulan yetkilendirme kodu **Authorize** düğme kümesi aralığından sonra süresi dolar. 

Kimlik Doğrulama belirtecinin süresi dolduğunda, aşağıdaki hata iletisini görebilirsiniz: 

  Kimlik bilgileri işlemi hatası: invalid_grant - AADSTS70002: Kimlik bilgileri doğrulanırken hata. AADSTS70008: Sağlanan erişim izni süresi doldu veya iptal edildi. İzleme kimliği: d18629e8-af88-43c5-88e3-d8419eb1fca1 bağıntı kimliği: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 zaman damgası: 2015-12-15 21:09:31Z

Aşağıdaki tablo, kullanıcı hesabı türü tarafından süre sonu gösterir: 

| Kullanıcı türü                                | Sürenin dolacağı tarih                            |
| :--------------------------------------- | :--------------------------------------- |
| Kullanıcı hesaplarının *değil* yönetilen Azure AD tarafından (Hotmail, Canlı vb.) | 12 saat.                                 |
| Kullanıcı hesapları *olan* Azure AD tarafından yönetilen | 14 gün sonra en son dilim çalıştırın. <br /><br />90 gün temel alan bir dilim bir OAuth tabanlı bağlı hizmeti 14 günde en az bir kez çalışır. |

Bu hatayı gidermek veya önlemek için seçerek yeniden yetkilendirin **Authorize** belirtecin süresi dolduğunda düğmesi. Ardından, bağlı hizmeti yeniden dağıtın. Değerleri için de oluşturabilirsiniz **SessionID** ve **yetkilendirme** aşağıdaki kodu kullanarak program aracılığıyla özellikleri:

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

Bu kod örneğinde kullanılan Data Factory sınıfları hakkında daha fazla ayrıntı için bkz:
* [AzureDataLakeStoreLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
* [AzureDataLakeAnalyticsLinkedService class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* [AuthorizationSessionGetResponse sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx)

Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll için bir başvuru ekleyin **WindowsFormsWebAuthenticationDialog** sınıfı. 

## <a name="azure-sql-linked-service"></a>Azure SQL bağlı hizmeti
Bir SQL bağlı hizmeti oluşturma ve kullanılmakta olan [saklı yordam etkinliğine](data-factory-stored-proc-activity.md) Data Factory işlem hattı bir saklı yordam çağırmak için. Daha fazla bilgi için [Azure SQL Bağlayıcısı](data-factory-azure-sql-connector.md#linked-service-properties).

## <a name="azure-sql-data-warehouse-linked-service"></a>Azure SQL veri ambarı bağlı hizmeti
SQL veri ambarı bağlı hizmetini oluşturmak ve kullanılmakta olan [saklı yordam etkinliğine](data-factory-stored-proc-activity.md) Data Factory işlem hattı bir saklı yordam çağırmak için. Daha fazla bilgi için [Azure SQL veri ambarı Bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties).

## <a name="sql-server-linked-service"></a>SQL Server bağlı hizmeti
SQL Server bağlı hizmeti oluşturma ve kullanılmakta olan [saklı yordam etkinliğine](data-factory-stored-proc-activity.md) Data Factory işlem hattı bir saklı yordam çağırmak için. Daha fazla bilgi için [SQL Server Bağlayıcısı](data-factory-sqlserver-connector.md#linked-service-properties).

