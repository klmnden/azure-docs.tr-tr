---
title: "Azure Data Factory ile desteklenen ortamlar işlem | Microsoft Docs"
description: "Dönüştürme veya işlem verileri (örneğin, Azure Hdınsight) Azure Data Factory işlem hatlarını kullanabileceğiniz bilgi işlem ortamları hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 6877a7e8-1a58-4cfb-bbd3-252ac72e4145
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/01/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: 1547b5c3a5c629b85ff5fa9de6b39b25531d9ec9
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>Azure Data Factory ile desteklenen ortamlar işlem
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 bağlantılı Hizmetleri'nde işlem](../compute-linked-services.md).

Bu makalede, işlem veya dönüştürme veri için kullanabileceğiniz farklı bilgi işlem ortamları açıklanmaktadır. (İsteğe bağlı karşılaştırması Getir kendi) farklı yapılandırmaları hakkındaki ayrıntıları bu bağlama bağlı hizmetler yapılandırırken Data Factory ile desteklenen ortamlar için bir Azure data factory işlem sağlar.

Aşağıdaki tabloda, bilgi işlem ortamları Data Factory ve bunlar üzerinde çalışan etkinlikleri tarafından desteklenen bir listesini sağlar. 

| İşlem ortamı                      | etkinlikler                               |
| ---------------------------------------- | ---------------------------------------- |
| [İsteğe bağlı Hdınsight kümesi](#azure-hdinsight-on-demand-linked-service) veya [kendi Hdınsight kümenizi](#azure-hdinsight-linked-service) | [DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop akış](data-factory-hadoop-streaming-activity.md) |
| [Azure toplu işlem](#azure-batch-linked-service) | [DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) | [Machine Learning etkinlikleri: Toplu Yürütme ve Kaynak Güncelleştirme](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) | [Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Azure SQL](#azure-sql-linked-service), [Azure SQL veri ambarı](#azure-sql-data-warehouse-linked-service), [SQL Server](#sql-server-linked-service) | [Saklı Yordam](data-factory-stored-proc-activity.md) |

## <a name="supported-hdinsight-versions-in-azure-data-factory"></a>Azure veri fabrikası'nda desteklenen Hdınsight sürümleri
Azure Hdınsight herhangi bir zamanda dağıtılabilir birden çok Hadoop küme sürümlerindeki destekler. Her sürüm seçimi Hortonworks veri Platformu (HDP) dağıtım belirli bir sürümü ve o dağıtım içinde bulunan bileşenleri kümesi oluşturur. Microsoft hdınsight son Hadoop ekosistemi bileşenlerini ve düzeltmeleri sağlamak için Desteklenen sürümlerin listesi için güncelleştirme tutar. Ayrıntılı bilgi için bkz: [desteklenen Hdınsight sürümleri](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

> [!IMPORTANT]
> Linux tabanlı Hdınsight sürüm 3.3 31 Temmuz 2017 üzerinde devre dışı bırakılan. Veri Fabrikası v1 isteğe bağlı Hdınsight bağlı hizmetler müşteriler 15 Aralık, test ve Hdınsight sonraki bir sürüme yükseltmek için 2017 kadar verildi. Windows tabanlı Hdınsight 31 Temmuz 2018 kullanımdan kaldırılacaktır.
>
> 

**O tarihten sonra ne** 

15 Aralık 2017 sonra:

- Artık Linux tabanlı Hdınsight sürüm 3.3 (veya önceki sürümler) oluşturmak mümkün olmayacaktır isteğe bağlı Hdınsight bağlı hizmeti Azure Data Factory v1 kullanarak kümeleri. 

- Varsa [osType ve/veya Version özelliği](https://docs.microsoft.com/en-us/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) açıkça belirtilmediği var olan Azure Data Factory v1 isteğe bağlı Hdınsight bağlı hizmeti JSON tanımlarında varsayılan değer değiştirilmesi gelen **sürüm 3.1, osType = = Windows** için **sürüm 3.6, osType = Linux =**.

31 Temmuz 2018 sonra:

- Artık herhangi bir sürümünü isteğe bağlı Hdınsight bağlı hizmeti Azure Data Factory v1 kullanarak Windows tabanlı Hdınsight kümelerini oluşturmak mümkün olmayacaktır. 

 **Önerilen Eylemler** 

- Güncelleştirme [osType ve/veya Version özelliği](https://docs.microsoft.com/en-us/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) yeni Linux tabanlı Hdınsight için etkilenen Azure Data Factory v1 isteğe bağlı Hdınsight bağlı hizmeti tanımlarını sürümleri (Hdınsight emin olmak için 3.6) en son Hadoop kullanabilirsiniz ekosistemi bileşenlerini ve düzeltmeler. 
- 15 Aralık 2017 test önce Azure veri fabrikası V1 Hive, Pig, MapReduce ve Hadoop etkilenen bağlantılı emin olmak için hizmet başvuru etkinlikleri akış yeni ile uyumlu olduklarından *osType* ve/veya  *Sürüm* varsayılan değer (sürüm 3.6, osType = Linux =) veya açık Hdınsight sürüm ve de için yükselttiğiniz osType. Uyumluluğu hakkında daha fazla bilgi için lütfen gözden [Linux tabanlı bir kümeye bir Windows tabanlı Hdınsight kümeden geçiş](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-migrate-from-windows-to-linux) ve [Hadoop bileşenleri ve Hdınsight ile kullanılabilir sürümlerini nelerdir?](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-component-versioning#hortonworks-release-notes-associated-with-hdinsight-versions) belgeleri Web sayfaları. 
- Açıkça Windows tabanlı Hdınsight kümeleri oluşturmak için Azure Data Factory v1On isteğe bağlı Hdınsight bağlı hizmeti kullanmaya devam etmek istiyorsanız osType Windows 15 Aralık 2017 önce ayarlayın. Ancak, Linux tabanlı Hdınsight kümelerine 31 Temmuz 2018 önce geçirme hala öneririz. 
- Azure Data Factory v1DotNet özel etkinliği yürütmek için isteğe bağlı Hdınsight bağlı hizmeti kullanıyorsanız, bunun yerine, bir Azure Batch bağlı hizmeti kullanmak için DotNet özel etkinlik JSON tanımını güncelleştirin. Daha fazla bilgi almak [Azure DataFactory ardışık düzeninde özel etkinlikleri kullanmak](https://docs.microsoft.com/en-us/azure/data-factory/v1/data-factory-use-custom-activities) belgeleri Web sayfası. 

>[!Note]
>Varolan Getir bilgisayarınızı kendi küme (BYOC) Hdınsight bağlı hizmeti Azure Data Factory v1 ya da BYOC kullanan ve isteğe bağlı HDInsightLinked Azure Data Factory v2 hizmetinde kullanan müşteriler için en son sürümünü destekleyen ilke ofAzure Hdınsight kümeleri zaten zorlanan, bu nedenle, hiçbir eylem gerekli değildir. 
>
> 


## <a name="on-demand-compute-environment"></a>İsteğe bağlı bilgi işlem ortamı
Bu tür yapılandırmada bilgi işlem ortamı tam olarak Azure Data Factory hizmeti tarafından yönetilir. Bir işi veri işlemek için gönderildi ve iş tamamlandığında, kaldırılan önce Data Factory hizmeti tarafından otomatik olarak oluşturulur. İsteğe bağlı bilgi işlem ortamı için bağlı hizmet oluşturma yapılandırın ve iş yürütme, küme yönetimi ve eylemler önyükleme için ayrıntılı ayarları denetler.

> [!NOTE]
> İsteğe bağlı yapılandırma şu anda yalnızca Azure Hdınsight kümeleri için desteklenir.
>
> 

## <a name="azure-hdinsight-on-demand-linked-service"></a>Azure Hdınsight isteğe bağlı
Azure Data Factory hizmetinin otomatik olarak bir Windows/Linux tabanlı isteğe bağlı Hdınsight kümesi verileri işlemek için oluşturabilirsiniz. Küme kümeyle ilişkili depolama hesabı (JSON özelliğinde linkedServiceName) ile aynı bölgede oluşturulur.

Aşağıdakilere dikkat edin **önemli** noktaları hakkında isteğe bağlı Hdınsight bağlı hizmeti:

* Azure aboneliğinizde oluşturduğunuz isteğe bağlı Hdınsight kümesi görmezsiniz. Azure Data Factory hizmeti sizin adınıza isteğe bağlı Hdınsight kümesi yönetir.
* Bir isteğe bağlı Hdınsight kümesinde çalışan işleri için günlükleri Hdınsight kümesi ile ilişkilendirilmiş depolama hesabına kopyalanır. Bu günlükler Azure portalında erişebileceğiniz **etkinlik çalışma ayrıntıları** dikey. Bkz: [izleme ve yönetme ardışık düzen](data-factory-monitor-manage-pipelines.md) Ayrıntılar için makale.
* Yalnızca Hdınsight kümesi yukarı olduğunda zaman ve çalışan işleri için sizden ücret kesilir.

> [!IMPORTANT]
> Genellikle sürer **20 dakika** veya isteğe bağlı Azure Hdınsight kümesi sağlamak için daha fazla bilgi.
>
> 

### <a name="example"></a>Örnek
Aşağıdaki JSON Linux tabanlı isteğe bağlı Hdınsight bağlı hizmeti tanımlar. Data Factory hizmetinin otomatik olarak oluşturur bir **Linux tabanlı** veri dilimi işlerken Hdınsight kümesi. 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "AzureStorageLinkedService"
        }
    }
}
```

Bir Windows tabanlı Hdınsight kümesi kullanmak üzere ayarlanmış **osType** için **windows** veya varsayılan değer olarak özelliği kullanmayın: windows.  

> [!IMPORTANT]
> HDInsight kümesi JSON’da belirttiğiniz blob depolamada (**linkedServiceName**) bir **varsayılan kapsayıcı** oluşturur. HDInsight, küme silindiğinde bu kapsayıcıyı silmez. Bu davranış tasarım gereğidir. İsteğe bağlı HDInsight bağlı hizmetiyle, HDInsight kümesi her oluşturulduğunda, burada mevcut canlı bir küme (**timeToLive**) olmadıkça bir dilim gerekir ve işlem bittiğinde silinir. 
>
> Daha fazla dilim işlendikçe, Azure blob depolamanızda çok sayıda kapsayıcı görürsünüz. İşlerin sorunları giderilmesi için bunlara gerek yoksa, depolama maliyetini azaltmak için bunları silmek isteyebilirsiniz. Bu kapsayıcı adları bir düzene sahiptir: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. Azure blob depolamada kapsayıcı silmek için [Microsoft Storage Gezgini](http://storageexplorer.com/) gibi araçları kullanın.
>
> 

### <a name="properties"></a>Özellikler
| Özellik                     | Açıklama                              | Gerekli |
| ---------------------------- | ---------------------------------------- | -------- |
| type                         | Type özelliği ayarlanmalı **HDInsightOnDemand**. | Evet      |
| ClusterSize                  | Kümedeki çalışan/veri düğüm sayısı. Hdınsight kümesi için bu özelliği belirtmeniz çalışan düğüm sayısı ile birlikte 2 baş düğümler ile oluşturulur. Düğüm boyutu 4 çalışan düğümlü bir küme 24 çekirdek alır, böylece 4 çekirdeğe sahip Standard_D3 olduğundan (4\*4 = 16 çekirdek çalışan düğümleri artı 2\*4 = 8 çekirdek baş düğümler için). Bkz: [Hdınsight oluşturma Linux tabanlı Hadoop kümeleri](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) Standard_D3 katmanı hakkında ayrıntılı bilgi için. | Evet      |
| TimeToLive                   | İsteğe bağlı Hdınsight kümesi için izin verilen boşta kalma süresi. Ne kadar süreyle isteğe bağlı Hdınsight kümesi kümedeki diğer etkin iş yok varsa bir etkinlik tamamlandıktan sonra canlı kalır belirtir.<br/><br/>Örneğin, bir etkinlik Çalıştır 6 dakika sürer ve timetolive 5 dakika olarak ayarlanmıştır, küme 6 etkinlik işleme dakika çalıştırdıktan sonra 5 dakika boyunca etkin kalır. Başka bir etkinlik 6 dakika penceresiyle yürütülürse, aynı küme tarafından işlenir.<br/><br/>İsteğe bağlı Hdınsight kümesi oluşturma bir pahalı işlemi (işlem zaman alabilir), bunu kullanımı isteğe bağlı Hdınsight kümesi yeniden kullanarak bir veri fabrikası performansını artırmak için bu ayarı olarak gerekli olur.<br/><br/>Timetolive değeri 0 olarak ayarlarsanız, küme etkinlik Çalıştır tamamlandıktan hemen sonra silindi. Yüksek bir değer ayarlarsanız, kümenin yüksek maliyetlerini gereksiz yere kaynaklanan boşta kalmasını ancak. Bu nedenle, gereksinimlerinize göre uygun değere ayarlamak önemlidir.<br/><br/>Timetolive özellik değerini uygun şekilde ayarlarsanız, birden çok ardışık düzen isteğe bağlı Hdınsight kümesi örneğini paylaşabilirsiniz. | Evet      |
| Sürüm                      | Hdınsight küme sürümü. Varsayılan değer 3.1 Windows kümesi için ve 3.2 Linux kümesi için ' dir. | Hayır       |
| linkedServiceName            | Depolamak ve veri işleme için isteğe bağlı küme tarafından kullanılacak azure depolama bağlı hizmeti. Hdınsight kümesi, bu Azure depolama hesabı ile aynı bölgede oluşturulur.<p>Şu anda bir Azure Data Lake Store depolama alanı olarak kullanan bir isteğe bağlı Hdınsight kümesi oluşturulamıyor. Bir Azure Data Lake Store'da işleme Hdınsight sonuç verileri depolamak istiyorsanız, Azure Blob depolama alanından Azure Data Lake Store'a veri kopyalamak için kopyalama etkinliği kullanın. </p> | Evet      |
| additionalLinkedServiceNames | Data Factory hizmetinin bunları sizin adınıza kaydedebilirsiniz böylece Hdınsight için ek depolama hesapları hizmeti bağlı belirtir. Bu depolama hesaplarından linkedServiceName tarafından belirtilen depolama hesabı ile aynı bölgede oluşturulan Hdınsight kümesi ile aynı bölgede olması gerekir. | Hayır       |
| osType                       | İşletim sistemi türü. İzin verilen değerler: (varsayılan) Windows ve Linux | Hayır       |
| hcatalogLinkedServiceName    | Azure SQL adını HCatalog veritabanına işaret hizmeti bağlı. İsteğe bağlı Hdınsight kümesi meta depo Azure SQL veritabanı kullanılarak oluşturulur. | Hayır       |

#### <a name="additionallinkedservicenames-json-example"></a>additionalLinkedServiceNames JSON örneği

```json
"additionalLinkedServiceNames": [
    "otherLinkedServiceName1",
    "otherLinkedServiceName2"
  ]
```

### <a name="advanced-properties"></a>Gelişmiş Özellikler
Ayrıca, isteğe bağlı Hdınsight kümesinin ayrıntılı yapılandırma için aşağıdaki özellikleri belirtebilirsiniz.

| Özellik               | Açıklama                              | Gerekli |
| :--------------------- | :--------------------------------------- | :------- |
| coreConfiguration      | Çekirdek yapılandırma parametreleri (olduğu gibi core-site.xml) oluşturulacak Hdınsight kümesi için belirtir. | Hayır       |
| hBaseConfiguration     | Hdınsight kümesi için HBase yapılandırma parametreleri (hbase-site.xml) belirtir. | Hayır       |
| hdfsConfiguration      | Hdınsight kümesi için HDFS yapılandırma parametreleri (hdfs-site.xml) belirtir. | Hayır       |
| hiveConfiguration      | Hdınsight kümesi için hive yapılandırma parametreleri (hive-site.xml) belirtir. | Hayır       |
| mapReduceConfiguration | Hdınsight kümesi için MapReduce yapılandırma parametreleri (mapred-site.xml) belirtir. | Hayır       |
| oozieConfiguration     | Hdınsight kümesi için Oozie yapılandırma parametreleri (oozie-site.xml) belirtir. | Hayır       |
| stormConfiguration     | Hdınsight kümesi için Storm yapılandırma parametreleri (storm-site.xml) belirtir. | Hayır       |
| yarnConfiguration      | Hdınsight kümesi için Yarn yapılandırma parametreleri (yarn-site.xml) belirtir. | Hayır       |

#### <a name="example--on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>Örnek – Gelişmiş özellikleri ile isteğe bağlı Hdınsight küme yapılandırması

```json
{
  "name": " HDInsightOnDemandLinkedService",
  "properties": {
    "type": "HDInsightOnDemand",
    "typeProperties": {
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
Head, veriler ve aşağıdaki özellikleri kullanarak zookeeper düğümleri boyutunu belirtebilirsiniz: 

| Özellik          | Açıklama                              | Gerekli |
| :---------------- | :--------------------------------------- | :------- |
| headNodeSize      | Baş düğüm boyutunu belirtir. Varsayılan değer: Standard_D3. Bkz: **düğümü boyutları belirtme** ayrıntıları bölümü. | Hayır       |
| dataNodeSize      | Veri düğümü boyutunu belirtir. Varsayılan değer: Standard_D3. | Hayır       |
| zookeeperNodeSize | Zoo Keeper düğüm boyutunu belirtir. Varsayılan değer: Standard_D3. | Hayır       |

#### <a name="specifying-node-sizes"></a>Düğümü boyutları belirtme
Bkz: [sanal makine boyutları](../../virtual-machines/linux/sizes.md) makale dize değerleri önceki bölümde belirtildiği özellikleri için belirtmeniz gerekir. Değerleri uygun gerek **cmdlet'leri & API'leri** makalesinde başvurulan. Makalede görebileceğiniz gibi büyük (varsayılan) boyutu veri düğümünün senaryonuz için yeterince iyi olmayabilir 7 GB bellek vardır. 

Boyutta D4 baş düğümler ve çalışan düğümleri oluşturmak istiyorsanız, belirtin **Standard_D4** headNodeSize ve dataNodeSize özellikleri için değer olarak. 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Bu özellikler için yanlış bir değer belirtirseniz, aşağıdaki alabilirsiniz **hata:** küme oluşturma başarısız oldu. Özel durum: Küme oluşturma işlemi tamamlanamadı. İşlem '400' koduyla başarısız oldu. Küme geride bırakma durumu: 'Hata'. İleti: 'PreClusterCreationValidationFailure'. Bu hata iletisini zaman kullandığınızdan emin olun **CMDLET & API'leri** tablosundan ad [sanal makine boyutları](../../virtual-machines/linux/sizes.md) makalesi.  

> [!NOTE]
> Şu anda Azure Data Factory birincil deposu olarak Azure Data Lake Store kullanarak Hdınsight kümelerini desteklemiyor. Azure Storage Hdınsight kümeleri için birincil depolama alanı olarak kullanın. 
>
> 


## <a name="bring-your-own-compute-environment"></a>Kendi işlem ortamınızda Getir
Bu tür yapılandırma, kullanıcılar veri fabrikasında bağlı hizmet olarak zaten mevcut olan bir bilgi işlem ortamı kaydedin. Bilgi işlem ortamı kullanıcı tarafından yönetilir ve Data Factory hizmetinin etkinlikleri yürütmek için kullanır.

Bu tür bir yapılandırma için aşağıdaki bilgi işlem ortamları desteklenir:

* Azure Hdınsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Azure SQL DB, Azure SQL DW, SQL Server

## <a name="azure-hdinsight-linked-service"></a>Azure Hdınsight bağlı hizmeti
Kendi Hdınsight kümenizi Data Factory ile kaydetmek için bir Azure Hdınsight bağlı hizmeti oluşturabilirsiniz.

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
| type              | Type özelliği ayarlanmalı **Hdınsight**. | Evet      |
| clusterUri        | Hdınsight küme URI'si.        | Evet      |
| kullanıcı adı          | Var olan bir Hdınsight kümesine bağlanmak için kullanılacak kullanıcı adını belirtin. | Evet      |
| password          | Kullanıcı hesabı için parola belirtin.   | Evet      |
| linkedServiceName | Hdınsight küme tarafından kullanılan Azure blob depolama başvurduğu Azure Storage bağlı hizmetin adı. <p>Şu anda bu özellik için bir Azure Data Lake Store bağlı belirtemezsiniz. Hdınsight kümesi için Data Lake Store erişimi varsa, Azure Data Lake Store'da verileri Hive/Pig komut dosyalarından erişebilir. </p> | Evet      |

## <a name="azure-batch-linked-service"></a>Azure toplu işlem hizmeti bağlı
Batch havuzundaki sanal makineler (VM'ler) kaydetmek için bir Azure Batch bağlantılı hizmeti bir veri fabrikası oluşturabilirsiniz. .NET özel etkinlikler Azure Batch ya da Azure Hdınsight kullanarak çalıştırabilirsiniz.

Azure Batch hizmetine yeniyseniz, aşağıdaki konularda bakın:

* [Azure Batch Temelleri](../../batch/batch-technical-overview.md) Azure Batch hizmetinin genel bakış.
* [AzureBatchAccount yeni](https://msdn.microsoft.com/library/mt125880.aspx) bir Azure Batch hesabı oluşturmak için cmdlet'i (veya) [Azure portal](../../batch/batch-account-create-portal.md) Azure portalını kullanarak Azure Batch hesabı oluşturmak için. Bkz: [Azure Batch hesabını yönetmek için PowerShell kullanarak](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) konu cmdlet kullanma hakkında ayrıntılı yönergeler için.
* [Yeni-AzureBatchPool](https://msdn.microsoft.com/library/mt125936.aspx) bir Azure Batch havuzu oluşturmak için cmdlet'i.

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

Append "**.\< bölge adı\>**"batch hesabınızın adını **accountName** özelliği. Örnek:

```json
"accountName": "mybatchaccount.eastus"
```

Başka bir seçenek batchUri uç noktası aşağıdaki örnekte gösterildiği gibi sağlamaktır:

```json
"accountName": "adfteam",
"batchUri": "https://eastus.batch.azure.com",
```

### <a name="properties"></a>Özellikler
| Özellik          | Açıklama                              | Gerekli |
| ----------------- | ---------------------------------------- | -------- |
| type              | Type özelliği ayarlanmalı **AzureBatch**. | Evet      |
| accountName       | Azure toplu işlem hesabının adı.         | Evet      |
| accessKey         | Azure Batch hesabı için erişim anahtarı.  | Evet      |
| poolName          | Sanal makinelerin havuzunun adı.    | Evet      |
| linkedServiceName | Azure depolama adı hizmeti bağlı Azure Batch hizmeti ile ilişkili bağlı. Bu bağlı hizmetin etkinlik ve Etkinlik yürütme günlüklerini depolamak çalıştırmak için gerekli hazırlama dosyaları için kullanılır. | Evet      |

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning hizmeti bağlı
Bir veri fabrikası Puanlama uç noktası bir Machine Learning toplu kaydetmek için bir Azure Machine Learning bağlantılı hizmeti oluşturun.

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
| Tür       | Type özelliği ayarlanmalıdır: **AzureML**. | Evet      |
| mlEndpoint | Toplu Puanlama URL.                   | Evet      |
| apikey ile yapılan     | Yayımlanan çalışma alanı modelinin API.     | Evet      |

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics hizmeti bağlı
Oluşturduğunuz bir **Azure Data Lake Analytics** bir Azure Data Lake Analytics bağlamak için bağlantılı hizmeti bir Azure data factory hizmetine işlem. Data Lake Analytics U-SQL etkinliği ardışık düzeninde bu bağlı hizmetin başvuruyor. 

Aşağıdaki tabloda JSON tanımında kullanılan genel özellikleri için açıklamalar sağlanır. Daha fazla hizmet sorumlusu ve kullanıcı kimlik bilgileri doğrulaması arasında seçim yapabilirsiniz.

| Özellik                 | Açıklama                              | Gerekli                                 |
| ------------------------ | ---------------------------------------- | ---------------------------------------- |
| **türü**                 | Type özelliği ayarlanmalıdır: **AzureDataLakeAnalytics**. | Evet                                      |
| **accountName**          | Azure Data Lake Analytics hesap adı.  | Evet                                      |
| **dataLakeAnalyticsUri** | Azure Data Lake Analytics URI.           | Hayır                                       |
| **Subscriptionıd**       | Azure abonelik kimliği                    | Hayır (belirtilmezse, data Factory abonelik kullanılır). |
| **resourceGroupName**    | Azure kaynak grubu adı                | Hayır (belirtilmezse, kaynak grubu data Factory kullanılır). |

### <a name="service-principal-authentication-recommended"></a>(Önerilen) hizmet asıl kimlik doğrulaması
Hizmet asıl kimlik doğrulaması kullanmak için Azure Active Directory (Azure AD) bir uygulama varlığı kaydetmek ve Data Lake Store'a erişim izni. Ayrıntılı adımlar için bkz: [hizmeti için kimlik doğrulama](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Bağlantılı hizmet tanımlamak için kullandığınız aşağıdaki değerleri not edin:
* Uygulama Kimliği
* Uygulama anahtarı 
* Kiracı Kimliği

Hizmet asıl kimlik doğrulaması, aşağıdaki özellikleri belirterek kullanın:

| Özellik                | Açıklama                              | Gerekli |
| :---------------------- | :--------------------------------------- | :------- |
| **servicePrincipalId**  | Uygulamanın istemci kimliği belirtin.     | Evet      |
| **servicePrincipalKey** | Uygulamanın anahtarını belirtin.           | Evet      |
| **Kiracı**              | Uygulamanızın bulunduğu altında Kiracı bilgileri (etki alanı adı veya Kiracı kimliği) belirtin. Azure portalının sağ üst köşedeki fare gelerek alabilir. | Evet      |

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

### <a name="user-credential-authentication"></a>Kullanıcı kimlik bilgileri doğrulaması
Alternatif olarak, aşağıdaki özellikleri belirterek Data Lake Analytics için kullanıcı kimlik bilgilerinin kullanabilirsiniz:

| Özellik          | Açıklama                              | Gerekli |
| :---------------- | :--------------------------------------- | :------- |
| **Yetkilendirme** | Tıklatın **Authorize** Data Factory Düzenleyici'düğmesine tıklayın ve bu özelliği otomatik olarak oluşturulur yetkilendirme URL'si atar kimlik bilgilerinizi girin. | Evet      |
| **SessionID**     | OAuth yetkilendirme oturumundan OAuth oturum kimliği. Her oturum kimliği benzersiz olup yalnızca bir kez kullanılabilir. Data Factory Düzenleyici kullandığınızda bu ayarı otomatik olarak oluşturulur. | Evet      |

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
Oluşturulan kullanarak Yetkilendirme kodu **Authorize** düğmesi süre sonra süresi dolar. Farklı türlerdeki kullanıcı hesapları için zaman aşımı süresinin için aşağıdaki tabloya bakın. Aşağıdaki hatayı görebilirsiniz ne zaman ileti kimlik doğrulama **belirtecinin süresi dolmadan**: kimlik bilgisi işlemi hatası: invalid_grant - AADSTS70002: Kimlik doğrulanırken hata oluştu. AADSTS70008: Sağlanan erişim izninin süresi doldu veya iptal edildi. İzleme kimliği: d18629e8-af88-43c5-88e3-d8419eb1fca1 bağıntı kimliği: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 zaman damgası: 2015-12-15 21:09:31Z

| Kullanıcı türü                                | Kullanım süresi sonu                            |
| :--------------------------------------- | :--------------------------------------- |
| Azure Active Directory tarafından yönetilmeyen kullanıcı hesapları (@hotmail.com, @live.comvb..) | 12 saat                                 |
| Azure Active Directory (AAD tarafından) yönetilen kullanıcı hesapları | 14 gün sonra en son dilim çalıştırın. <br/><br/>90 OAuth tabanlı bağlantılı hizmette dayanan bir dilim 14 günde bir en az bir kez çalıştırılıyorsa, gün. |

Bu hatayı önlemek/çözmek için kullanarak yeniden yetkilendirin **Authorize** ne zaman düğmesini **belirtecinin süresi dolmadan** ve bağlantılı hizmeti yeniden dağıtın. Değerleri de oluşturabilirsiniz **SessionID** ve **yetkilendirme** kullanılarak programlı olarak kod şu şekilde özellikleri:

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

Bkz: [AzureDataLakeStoreLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), ve [AuthorizationSessionGetResponse sınıfı](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) konuları kod içinde kullanılan veri fabrikası sınıfları hakkında ayrıntılı bilgi için. Bir başvuru ekleyin: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll WindowsFormsWebAuthenticationDialog sınıfı için. 

## <a name="azure-sql-linked-service"></a>Azure SQL bağlı hizmeti
Azure SQL bağlı hizmeti oluşturma ve onunla kullanma [saklı yordam etkinliği](data-factory-stored-proc-activity.md) Data Factory işlem hattı bir saklı yordam çağırmak için. Bkz: [Azure SQL Bağlayıcısı](data-factory-azure-sql-connector.md#linked-service-properties) makale bu bağlı hizmetin hakkında ayrıntılı bilgi için.

## <a name="azure-sql-data-warehouse-linked-service"></a>Bağlı hizmetin Azure SQL veri ambarı
Bir Azure SQL Data Warehouse bağlı hizmet oluşturma ve onunla kullanma [saklı yordam etkinliği](data-factory-stored-proc-activity.md) Data Factory işlem hattı bir saklı yordam çağırmak için. Bkz: [Azure SQL Data Warehouse Bağlayıcısı](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) makale bu bağlı hizmetin hakkında ayrıntılı bilgi için.

## <a name="sql-server-linked-service"></a>SQL Server hizmeti bağlı
Bir SQL Server bağlantılı hizmet oluşturma ve onunla kullanma [saklı yordam etkinliği](data-factory-stored-proc-activity.md) Data Factory işlem hattı bir saklı yordam çağırmak için. Bkz: [SQL Server Bağlayıcısı](data-factory-sqlserver-connector.md#linked-service-properties) makale bu bağlı hizmetin hakkında ayrıntılı bilgi için.

