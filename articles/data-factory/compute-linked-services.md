---
title: Azure Data Factory tarafından desteklenen ortam işlem | Microsoft Docs
description: Dönüştürme veya işlem verileri (örneğin, Azure HDInsight) Azure Data Factory işlem hatlarını kullanabileceğiniz işlem ortamları hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/15/2019
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: b4078303a0fabf70fe8bda82875dd312714f73de
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66155256"
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>Azure Data Factory tarafından desteklenen ortam işlem
Bu makalede, işlem veya dönüşüm veri için kullanabileceğiniz farklı işlem ortamlarında açıklanmaktadır. (İsteğe bağlı ve Getir kendi) farklı yapılandırmalar hakkında ayrıntılar bu bağlama bağlı hizmetler yapılandırırken Data Factory tarafından desteklenen bir Azure data factory'ye ortamları işlem sağlar.

Aşağıdaki tabloda Data Factory ve bunlar üzerinde çalışan etkinlikler tarafından desteklenen işlem ortamlarının listesi sağlar. 

| İşlem ortamı                                          | etkinlikler                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [İsteğe bağlı HDInsight kümesi](#azure-hdinsight-on-demand-linked-service) veya [kendi HDInsight kümenizi](#azure-hdinsight-linked-service) | [Hive](transform-data-using-hadoop-hive.md), [Pig](transform-data-using-hadoop-pig.md), [Spark](transform-data-using-spark.md), [MapReduce](transform-data-using-hadoop-map-reduce.md), [Hadoop akış](transform-data-using-hadoop-streaming.md) |
| [Azure Batch](#azure-batch-linked-service)                   | [Özel](transform-data-using-dotnet-custom-activity.md)     |
| [Azure Machine Learning](#azure-machine-learning-linked-service) | [Machine Learning etkinlikleri: Toplu yürütme ve kaynak güncelleştirme](transform-data-using-machine-learning.md) |
| [Azure Data Lake Analytics'i](#azure-data-lake-analytics-linked-service) | [Data Lake Analytics U-SQL](transform-data-using-data-lake-analytics.md) |
| [Azure SQL](#azure-sql-database-linked-service), [Azure SQL veri ambarı](#azure-sql-data-warehouse-linked-service), [SQL Server](#sql-server-linked-service) | [Saklı Yordam](transform-data-using-stored-procedure.md) |
| [Azure Databricks](#azure-databricks-linked-service)         | [Not Defteri](transform-data-databricks-notebook.md), [Jar](transform-data-databricks-jar.md), [Python](transform-data-databricks-python.md) |

>  

## <a name="on-demand-hdinsight-compute-environment"></a>İsteğe bağlı HDInsight işlem ortamı
Bu tür yapılandırma, bilgi işlem ortamınız Azure Data Factory hizmeti tarafından tamamen yönetilir. Bir iş verileri işlemek için gönderilen ve iş tamamlandığında kaldırıldı önce Data Factory hizmeti tarafından otomatik olarak oluşturulur. İsteğe bağlı işlem ortamı için bağlı hizmet oluşturun, yapılandırın ve iş yürütme, küme yönetimi ve eylemleri önyükleme için ayrıntılı ayarları denetler.

> [!NOTE]
> İsteğe bağlı yapılandırma şu anda yalnızca Azure HDInsight kümeleri için desteklenir. Azure Databricks işi kümelerini kullanarak isteğe bağlı işleri de destekler, başvurmak [Azure databricks bağlı hizmeti](#azure-databricks-linked-service) daha fazla ayrıntı için.

## <a name="azure-hdinsight-on-demand-linked-service"></a>Azure HDInsight isteğe bağlı hizmeti
Azure Data Factory hizmetinin otomatik olarak verileri işlemek için bir isteğe bağlı HDInsight kümesi oluşturabilirsiniz. Küme, kümeyle ilişkili depolama hesabı (JSON özelliğinde linkedServiceName) ile aynı bölgede oluşturulur. Depolama hesabı genel amaçlı standart Azure depolama hesabı olmalıdır. 

Aşağıdakilere dikkat edin **önemli** noktaları hakkında isteğe bağlı HDInsight bağlı hizmeti:

* İsteğe bağlı HDInsight kümesi, Azure aboneliği altında oluşturulur. Azure portal'ınızın kümedeki küme yukarı olduğunda görmek mümkün ve çalışan olursunuz. 
* Bir isteğe bağlı HDInsight kümesi üzerinde çalışan işler için günlükleri, HDInsight kümesi ile ilişkili depolama hesabına kopyalanır. ClusterUserName, clusterPassword clusterSshUserName, bağlı hizmet tanımında clusterSshPassword küme yaşam döngüsü boyunca ayrıntılı sorun giderme için küme oturum açmak için kullanılır. 
* Yalnızca HDInsight kümesi yukarı olduğunda zaman ve çalışan işleri için ücretlendirilir.
* Kullanabileceğiniz bir **betik eylemi** bağlı hizmeti Azure HDInsight ile isteğe bağlı.  

> [!IMPORTANT]
> Genellikle sürer **20 dakika** ya da daha fazla isteğe bağlı bir Azure HDInsight kümesi sağlayın.

### <a name="example"></a>Örnek
Aşağıdaki JSON, Linux tabanlı bir isteğe bağlı HDInsight bağlı hizmeti tanımlar. Data Factory hizmetinin otomatik olarak oluşturur bir **Linux tabanlı** gerekli etkinliğini işlemek için HDInsight kümesi. 

```json
{
  "name": "HDInsightOnDemandLinkedService",
  "properties": {
    "type": "HDInsightOnDemand",
    "typeProperties": {
      "clusterType": "hadoop",
      "clusterSize": 1,
      "timeToLive": "00:15:00",
      "hostSubscriptionId": "<subscription ID>",
      "servicePrincipalId": "<service principal ID>",
      "servicePrincipalKey": {
        "value": "<service principal key>",
        "type": "SecureString"
      },
      "tenant": "<tenent id>",
      "clusterResourceGroup": "<resource group name>",
      "version": "3.6",
      "osType": "Linux",
      "linkedServiceName": {
        "referenceName": "AzureStorageLinkedService",
        "type": "LinkedServiceReference"
      }
    },
    "connectVia": {
      "referenceName": "<name of Integration Runtime>",
      "type": "IntegrationRuntimeReference"
    }
  }
}
```

> [!IMPORTANT]
> HDInsight kümesi JSON’da belirttiğiniz blob depolamada (**linkedServiceName**) bir **varsayılan kapsayıcı** oluşturur. HDInsight, küme silindiğinde bu kapsayıcıyı silmez. Bu davranış tasarım gereğidir. İsteğe bağlı HDInsight bağlı hizmetiyle, HDInsight kümesi her oluşturulduğunda, burada mevcut canlı bir küme (**timeToLive**) olmadıkça bir dilim gerekir ve işlem bittiğinde silinir. 
>
> Daha fazla etkinlik çalıştırmalarını gibi Azure blob depolamanızda çok sayıda kapsayıcı görürsünüz. İşlerin sorunları giderilmesi için bunlara gerek yoksa, depolama maliyetini azaltmak için bunları silmek isteyebilirsiniz. Bu kapsayıcı adları bir düzene sahiptir: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. Azure blob depolamada kapsayıcı silmek için [Microsoft Storage Gezgini](https://storageexplorer.com/) gibi araçları kullanın.
>
> 

### <a name="properties"></a>Özellikler
| Özellik                     | Açıklama                              | Gerekli |
| ---------------------------- | ---------------------------------------- | -------- |
| type                         | Type özelliği ayarlanmalıdır **Hdınsightondemand**. | Evet      |
| clusterSize                  | Kümedeki çalışan/veri düğümü sayısı. HDInsight kümesi için bu özelliği belirtmeniz çalışan düğümleri sayısını 2 baş düğüm ile oluşturulur. 4 çekirdek, 4 çalışan düğümü küme 24 çekirdek sürer olan işler için standart_d3 boyutunu düğümlerdir (4\*4 = 16 çekirdek çalışan düğümleri artı 2\*baş düğümleri için 4 = 8 çekirdek). Bkz: [HDInsight Hadoop, Spark, Kafka ve daha fazlası ile kümelerini ayarlama](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) Ayrıntılar için. | Evet      |
| linkedServiceName            | Depolama ve veri işleme için isteğe bağlı küme tarafından kullanılacak azure depolama bağlı hizmeti. HDInsight kümesi bu Azure depolama hesabı ile aynı bölgede oluşturulur. Azure HDInsight, desteklediği her bir Azure bölgesinde kullanabileceğiniz toplam çekirdek sayısıyla ilgili sınırlamaya sahiptir. Bu Azure bölgesinde gerekli clusterSize karşılamak için yeterli çekirdek kotası olduğundan emin olun. Ayrıntılar için başvurmak [HDInsight Hadoop, Spark, Kafka ve daha fazlası ile kümelerini ayarlama](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md)<p>Şu anda, bir Azure Data Lake Store depolama alanı olarak kullanan bir isteğe bağlı HDInsight kümesi oluşturulamıyor. HDInsight bir Azure Data Lake Store içinde işleme sonuç verileri depolamak istiyorsanız, Azure Data Lake Store için Azure Blob depolama alanından verileri kopyalamak için kopyalama etkinliğini kullanın. </p> | Evet      |
| clusterResourceGroup         | HDInsight kümesi bu kaynak grubunda oluşturulur. | Evet      |
| TimeToLive                   | İsteğe bağlı HDInsight kümesi için izin verilen boşta kalma süresi. Ne kadar süreyle isteğe bağlı HDInsight kümesi kümede hiç bir etkin iş olduğunda çalıştırmak bir etkinlik tamamlandıktan sonra canlı kalmasını belirtir. Değer izin verilen en az 5 dakikadır (00: 05:00).<br/><br/>Örneğin, bir etkinlik çalıştırması 6 dakika sürer ve timetolive 5 dakika olarak ayarlanmıştır, küme etkinlik işleme 6 dakika çalıştırdıktan sonra 5 dakika boyunca Canlı kalır. Başka bir etkinlik çalıştırması 6 dakika penceresiyle yürütülürse, aynı küme tarafından işlenir.<br/><br/>Bir isteğe bağlı HDInsight kümesi oluşturma bir yeniden bir isteğe bağlı HDInsight kümesi kullanarak veri fabrikası performansını artırmak için bu ayarı olarak gerekli bunu kullanmak, pahalı işlem (biraz sürebilir) bağlıdır.<br/><br/>Timetolive değeri 0 olarak ayarlarsanız etkinlik çalıştırma tamamlandıktan hemen sonra küme silinir. Yüksek bir değere ayarlarsanız, kümenin bazı sorun giderme için oturum açmanız için boşta kalır ancak amaçlı ancak içinde yüksek maliyetlere neden olabilir. Bu nedenle, ihtiyaçlarınıza uygun değeri ayarlamak önemlidir.<br/><br/>Timetolive özellik değeri uygun şekilde ayarlarsanız, birden fazla işlem hattını isteğe bağlı HDInsight kümesi örneğini paylaşabilirsiniz. | Evet      |
| clusterType                  | Oluşturulacak HDInsight küme türü. İzin verilen değerler şunlardır: "hadoop" ve "spark". Belirtilmezse, varsayılan değer hadoop olur. Kurumsal güvenlik paketi etkin küme oluşturulamıyor isteğe bağlı, bunun yerine kullanmak bir [mevcut küme / kendi işlem Getir](#azure-hdinsight-linked-service). | Hayır       |
| version                      | HDInsight küme sürümü. Belirtilmezse, geçerli HDInsight tanımlanan varsayılan sürümü kullanıyor. | Hayır       |
| hostSubscriptionId           | HDInsight kümesi oluşturmak için kullanılan Azure abonelik kimliği. Belirtilmezse, abonelik kimliği, Azure oturum açma bağlamını kullanır. | Hayır       |
| clusterNamePrefix           | HDI küme adı, bir zaman damgası öneki, küme adının sonunda otomatik olarak eklenir| Hayır       |
| sparkVersion                 | Küme türü "Spark" ise spark sürümü | Hayır       |
| additionalLinkedServiceNames | Ek depolama hesapları için HDInsight bağlı hizmeti Data Factory hizmetinin sizin adınıza kaydedebilmesi belirtir. Bu depolama hesaplarından linkedServiceName tarafından belirtilen depolama hesabı ile aynı bölgede oluşturulan HDInsight kümesi ile aynı bölgede olması gerekir. | Hayır       |
| osType                       | İşletim sistemi türü. İzin verilen değerler şunlardır: Linux ve Windows (için HDInsight 3.3 yalnızca). Linux varsayılandır. | Hayır       |
| hcatalogLinkedServiceName    | Azure SQL adını bağlı HCatalog veritabanına işaret eden hizmeti. Meta veri deposu olarak Azure SQL veritabanı kullanarak isteğe bağlı HDInsight kümesi oluşturulur. | Hayır       |
| connectVia                   | Bu HDInsight bağlı hizmeti için etkinlikler gönderilmesi için kullanılacak Integration Runtime. İsteğe bağlı HDInsight bağlı hizmeti için yalnızca Azure Integration Runtime'ı destekler. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Hayır       |
| clusterUserName                   | Kümeye erişmek için kullanıcı adı. | Hayır       |
| clusterPassword                   | Parola kümeye erişmek için güvenli dize türünde. | Hayır       |
| clusterSshUserName         | SSH kullanıcı adı (Linux için) uzaktan küme düğümüne bağlanın. | Hayır       |
| clusterSshPassword         | SSH için güvenli dize türünde parola (Linux için) uzaktan küme düğümüne bağlanın. | Hayır       |
| scriptActions | Komut dosyası için belirtmek [HDInsight küme özelleştirmeleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux) talep üzerine küme oluşturma sırasında. <br />Şu anda, Azure Data Factory'nin kullanıcı arabirimi geliştirme aracı belirten yalnızca 1 betik eylemi destekler, ancak bu sınırlama, JSON aracılığıyla elde edebilirsiniz (birden çok betik eylemleri, JSON'da belirtin). | Hayır |


> [!IMPORTANT]
> HDInsight dağıtılabilir birden çok Hadoop küme sürümleri destekler. Her sürüm seçimi, Hortonworks Data Platform (HDP) dağıtım belirli bir sürümünü ve dağıtımı içinde bulunan bileşenler kümesi oluşturur. En yeni Hadoop ekosistemi bileşenleri ve düzeltmeleri sağlamak üzere güncelleştiriliyor desteklenen HDInsight sürümleri listesini tutar. En son bilgileri her zaman başvuran emin [desteklenen HDInsight sürüm ve işletim sistemi türü](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) HDInsight'ın desteklenen sürümünü kullandığınızdan emin olmak için. 
>
> [!IMPORTANT]
> HDInsight bağlı Hizmetleri şu anda, HBase, etkileşimli sorgu (LLAP) Hive, Storm desteklemez. 
>
> 

#### <a name="additionallinkedservicenames-json-example"></a>additionalLinkedServiceNames JSON örneği

```json
"additionalLinkedServiceNames": [{
    "referenceName": "MyStorageLinkedService2",
    "type": "LinkedServiceReference"          
}]
```

### <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

İsteğe bağlı HDInsight bağlı hizmeti, sizin adınıza HDInsight kümeleri oluşturmak için bir hizmet sorumlusu kimlik doğrulaması gerektirir. Hizmet sorumlusu kimlik doğrulaması kullanmak, Azure Active Directory (Azure AD) uygulama varlığın kaydedin ve bu izni **katkıda bulunan** rol abonelik veya kaynak grubu HDInsight kümesi oluşturulur. Ayrıntılı adımlar için bkz. [Azure Active Directory kaynaklarına erişmek uygulama ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal). Bağlı hizmetini tanımlamak için kullandığınız şu değerleri not edin:

- Uygulama Kimliği
- Uygulama anahtarı 
- Kiracı Kimliği

Hizmet sorumlusu kimlik doğrulaması, aşağıdaki özellikleri belirterek kullanın:

| Özellik                | Açıklama                              | Gerekli |
| :---------------------- | :--------------------------------------- | :------- |
| **servicePrincipalId**  | Uygulamanın istemci kimliği belirtin.     | Evet      |
| **serviceprincipalkey değerleri** | Uygulama anahtarını belirtin.           | Evet      |
| **Kiracı**              | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Azure portalının sağ üst köşedeki fare getirerek geri alabilirsiniz. | Evet      |

### <a name="advanced-properties"></a>Gelişmiş Özellikler

İsteğe bağlı HDInsight kümesinin ayrıntılı yapılandırma için aşağıdaki özellikleri belirtebilirsiniz.

| Özellik               | Açıklama                              | Gerekli |
| :--------------------- | :--------------------------------------- | :------- |
| coreConfiguration      | Oluşturulacak HDInsight küme için çekirdek yapılandırma parametrelerini (olduğu gibi core-site.xml) belirtir. | Hayır       |
| hBaseConfiguration     | HDInsight kümesi için HBase yapılandırma parametreleri (hbase-site.xml) belirtir. | Hayır       |
| hdfsConfiguration      | HDInsight küme için HDFS yapılandırma parametreleri (hdfs-site.xml) belirtir. | Hayır       |
| hiveConfiguration      | HDInsight kümesi için hive yapılandırma parametreleri (hive-site.xml) belirtir. | Hayır       |
| mapReduceConfiguration | HDInsight kümesi için MapReduce yapılandırma parametreleri (mapred-site.xml) belirtir. | Hayır       |
| oozieConfiguration     | HDInsight kümesi için Oozie yapılandırma parametreleri (oozie-site.xml) belirtir. | Hayır       |
| stormConfiguration     | HDInsight kümesi (storm-site.xml) Storm yapılandırma parametreleri belirtir. | Hayır       |
| yarnConfiguration      | HDInsight kümesi için Yarn yapılandırma parametreleri (yarn-site.xml) belirtir. | Hayır       |

#### <a name="example--on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>Örneğin, gelişmiş özelliklere sahip isteğe bağlı HDInsight kümesi yapılandırma

```json
{
    "name": " HDInsightOnDemandLinkedService",
    "properties": {
      "type": "HDInsightOnDemand",
      "typeProperties": {
          "clusterSize": 16,
          "timeToLive": "01:30:00",
          "hostSubscriptionId": "<subscription ID>",
          "servicePrincipalId": "<service principal ID>",
          "servicePrincipalKey": {
            "value": "<service principal key>",
            "type": "SecureString"
          },
          "tenant": "<tenent id>",
          "clusterResourceGroup": "<resource group name>",
          "version": "3.6",
          "osType": "Linux",
          "linkedServiceName": {
              "referenceName": "AzureStorageLinkedService",
              "type": "LinkedServiceReference"
            },
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
            "additionalLinkedServiceNames": [{
                "referenceName": "MyStorageLinkedService2",
                "type": "LinkedServiceReference"          
            }]
        }
    },
      "connectVia": {
      "referenceName": "<name of Integration Runtime>",
      "type": "IntegrationRuntimeReference"
    }
}
```

### <a name="node-sizes"></a>Düğümü boyutları
Baş, veri ve zookeeper düğümleri aşağıdaki özellikleri kullanarak boyutunu belirtebilirsiniz: 

| Özellik          | Açıklama                              | Gerekli |
| :---------------- | :--------------------------------------- | :------- |
| headNodeSize      | Baş düğüm boyutunu belirtir. Varsayılan değerdir: Standard_D3. Bkz: **düğümü boyutları belirtme** ayrıntıları bölümü. | Hayır       |
| dataNodeSize      | Veri düğümü boyutunu belirtir. Varsayılan değerdir: Standard_D3. | Hayır       |
| zookeeperNodeSize | Zoo Keeper düğüm boyutunu belirtir. Varsayılan değerdir: Standard_D3. | Hayır       |

#### <a name="specifying-node-sizes"></a>Düğümü boyutları belirtme
Bkz: [sanal makinelerinin boyutları](../virtual-machines/linux/sizes.md) makale için dize değerlerini önceki bölümde belirtilen özellikleri belirtmeniz gerekir. Değerleri uygun gerek **cmdlet'leri ve API'leri** makalede bahsedilen. Makalesinde görebileceğiniz gibi 7 GB bellek, senaryonuz için yeterince iyi olmayabilir (varsayılan) büyük boyuttaki veri düğümü vardır. 

Boyutlandırılmış D4 baş düğümü ve alt düğümü oluşturmak istiyorsanız, belirtin **işler için standart_d4** headNodeSize ve dataNodeSize özellikleri için değer olarak. 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Bu özellikler için yanlış bir değer belirtirseniz, aşağıdaki alabileceğiniz **hata:** Küme oluşturulamadı. Özel durum: Küme oluşturma işlemi tamamlanamıyor. İşlem '400' koduyla başarısız oldu. Geride bırakma durumu küme: 'Error'. Mesaj: 'PreClusterCreationValidationFailure'. Bu hata iletisini aldığınızda, kullandığınızdan emin olun **CMDLET'i ve API'leri** tablosundan ad [sanal makinelerinin boyutları](../virtual-machines/linux/sizes.md) makalesi.        

## <a name="bring-your-own-compute-environment"></a>Kendi işlem ortamı getirin
Bu tür yapılandırma, kullanıcılar, Data Factory öğesinde bağlantılı hizmet olarak zaten mevcut olan bir bilgi işlem ortamı kaydedebilirsiniz. Bilgi işlem ortamınız, kullanıcı tarafından yönetilir ve Data Factory hizmetinin etkinlikleri yürütmek için kullanır.

Bu tür bir yapılandırma için aşağıdaki bilgi işlem ortamları desteklenir:

* Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Azure SQL DB'ye, Azure SQL DW, SQL Server

## <a name="azure-hdinsight-linked-service"></a>Azure HDInsight bağlı hizmeti
Kendi HDInsight kümenizi Data Factory'ye kaydetmeniz için bir Azure HDInsight bağlı hizmeti oluşturabilirsiniz.

### <a name="example"></a>Örnek

```json
{
    "name": "HDInsightLinkedService",
    "properties": {
      "type": "HDInsight",
      "typeProperties": {
        "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
        "userName": "username",
        "password": {
            "value": "passwordvalue",
            "type": "SecureString"
          },
        "linkedServiceName": {
              "referenceName": "AzureStorageLinkedService",
              "type": "LinkedServiceReference"
        }
      },
      "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
      }
    }
  }
```

### <a name="properties"></a>Özellikler
| Özellik          | Açıklama                                                  | Gerekli |
| ----------------- | ------------------------------------------------------------ | -------- |
| type              | Type özelliği ayarlanmalıdır **HDInsight**.            | Evet      |
| Küme Uri'si        | HDInsight küme URİ'si.                            | Evet      |
| kullanıcı adı          | Mevcut bir HDInsight kümesine bağlanmak için kullanılacak kullanıcı adını belirtin. | Evet      |
| password          | Kullanıcı hesabı için parola belirtin.                       | Evet      |
| linkedServiceName | HDInsight küme tarafından kullanılan Azure blob Depolama'ya başvuran Azure depolama bağlı hizmetin adı. <p>Şu anda bu özellik için bir Azure Data Lake Store bağlı belirtemezsiniz. HDInsight kümesi için Data Lake Store erişimi varsa, Azure Data Lake Store verileri Hive/Pig betiklerin erişebilir. </p> | Evet      |
| isEspEnabled      | Belirtin '*true*' HDInsight küme ise [Kurumsal güvenlik paketi](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-introduction) etkin. Varsayılan '*false*'. | Hayır       |
| connectVia        | Bu bağlı hizmeti için etkinlikler gönderilmesi için kullanılacak Integration Runtime. Azure tümleştirme çalışma zamanı veya şirket içinde barındırılan Integration Runtime'ı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. <br />Kurumsal güvenlik paketi (ESP) HDInsight küme kullanımı görebilmesi kümesine sahip bir şirket içinde barındırılan tümleştirme çalışma zamanı etkin veya ESP HDInsight kümesi olarak aynı sanal ağ içindeki dağıtılmalıdır için. | Hayır       |

> [!IMPORTANT]
> HDInsight dağıtılabilir birden çok Hadoop küme sürümleri destekler. Her sürüm seçimi, Hortonworks Data Platform (HDP) dağıtım belirli bir sürümünü ve dağıtımı içinde bulunan bileşenler kümesi oluşturur. En yeni Hadoop ekosistemi bileşenleri ve düzeltmeleri sağlamak üzere güncelleştiriliyor desteklenen HDInsight sürümleri listesini tutar. En son bilgileri her zaman başvuran emin [desteklenen HDInsight sürüm ve işletim sistemi türü](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) HDInsight'ın desteklenen sürümünü kullandığınızdan emin olmak için. 
>
> [!IMPORTANT]
> HDInsight bağlı Hizmetleri şu anda, HBase, etkileşimli sorgu (LLAP) Hive, Storm desteklemez. 
>
> 

## <a name="azure-batch-linked-service"></a>Azure Batch bağlı hizmeti

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Veri Fabrikası için bir Batch havuzu sanal makineler (VM'ler) kaydetmek için bir Azure Batch bağlı hizmeti oluşturabilirsiniz. Azure Batch kullanarak özel etkinliği çalıştırabilirsiniz.

Azure Batch hizmetine yeni başladıysanız, aşağıdaki konularda bakın:

* [Azure Batch temel bilgileri](../batch/batch-technical-overview.md) için Azure Batch hizmetine genel bakış.
* [Yeni AzBatchAccount](/powershell/module/az.batch/New-azBatchAccount) Azure Batch hesabı oluşturmak için cmdlet'i (veya) [Azure portalında](../batch/batch-account-create-portal.md) Azure portalını kullanarak Azure Batch hesabı oluşturmak için. Bkz: [Azure Batch hesabını yönetmek için PowerShell kullanma](https://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) konu cmdlet kullanma hakkında ayrıntılı yönergeler için.
* [Yeni AzBatchPool](/powershell/module/az.batch/New-AzBatchPool) bir Azure Batch havuzu oluşturmak için cmdlet'i.

### <a name="example"></a>Örnek

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
      "type": "AzureBatch",
      "typeProperties": {
        "accountName": "batchaccount",
        "accessKey": {
          "type": "SecureString",
          "value": "access key"
        },
        "batchUri": "https://batchaccount.region.batch.azure.com",
        "poolName": "poolname",
        "linkedServiceName": {
          "referenceName": "StorageLinkedService",
          "type": "LinkedServiceReference"
        }
      },
      "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
      }
    }
  }
```


### <a name="properties"></a>Özellikler
| Özellik          | Açıklama                              | Gerekli |
| ----------------- | ---------------------------------------- | -------- |
| type              | Type özelliği ayarlanmalıdır **AzureBatch**. | Evet      |
| accountName       | Azure Batch hesabı adı.         | Evet      |
| accessKey         | Azure Batch hesabı için erişim anahtarı.  | Evet      |
| batchUri          | URL https:// biçiminde Azure Batch hesabınıza*batchaccountname.region*. batch.azure.com. | Evet      |
| poolName          | Sanal makine havuzu adı.    | Evet      |
| linkedServiceName | Ad Azure depolama bağlı hizmeti bu bağlı Azure Batch hizmeti ile ilişkilendirilmiş. Bu bağlı hizmeti, etkinliği çalıştırmak için gerekli dosyaları hazırlama için kullanılır. | Evet      |
| connectVia        | Bu bağlı hizmeti için etkinlikler gönderilmesi için kullanılacak Integration Runtime. Azure tümleştirme çalışma zamanı veya şirket içinde barındırılan Integration Runtime'ı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Hayır       |

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning bağlı hizmeti
Bir Machine Learning batch Puanlama uç noktası bir data factory'ye kaydetmeniz için bir Azure Machine Learning bağlı hizmet oluşturursunuz.

### <a name="example"></a>Örnek

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
      "type": "AzureML",
      "typeProperties": {
        "mlEndpoint": "https://[batch scoring endpoint]/jobs",
        "apiKey": {
            "type": "SecureString",
            "value": "access key"
        }
     },
     "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
      }
    }
}
```

### <a name="properties"></a>Özellikler
| Özellik               | Açıklama                              | Gerekli                                 |
| ---------------------- | ---------------------------------------- | ---------------------------------------- |
| Type                   | Type özelliği ayarlanmalıdır: **AzureML**. | Evet                                      |
| mlEndpoint             | Toplu işlem Puanlama URL'si.                   | Evet                                      |
| ApiKey                 | Yayımlanan çalışma alanı modelinin API.     | Evet                                      |
| updateResourceEndpoint | Tahmine dayalı Web hizmeti ile eğitilmiş model dosyasını güncelleştirmek için kullanılan bir Azure ML Web Hizmeti uç noktası güncelleştirme kaynak URL'si | Hayır                                       |
| servicePrincipalId     | Uygulamanın istemci kimliği belirtin.     | UpdateResourceEndpoint belirttiyseniz gereklidir |
| servicePrincipalKey    | Uygulama anahtarını belirtin.           | UpdateResourceEndpoint belirttiyseniz gereklidir |
| tek                 | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Azure portalının sağ üst köşedeki fare getirerek geri alabilirsiniz. | UpdateResourceEndpoint belirttiyseniz gereklidir |
| connectVia             | Bu bağlı hizmeti için etkinlikler gönderilmesi için kullanılacak Integration Runtime. Azure tümleştirme çalışma zamanı veya şirket içinde barındırılan Integration Runtime'ı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Hayır                                       |

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics bağlı hizmeti
Oluşturduğunuz bir **Azure Data Lake Analytics** bir Azure Data Lake Analytics bağlamak için bağlı hizmet bir Azure data factory hizmetine işlem. Data Lake Analytics U-SQL etkinliği işlem hattındaki bu bağlı hizmetini ifade eder. 

### <a name="example"></a>Örnek

```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics URI",
            "servicePrincipalId": "service principal id",
            "servicePrincipalKey": {
                "value": "service principal key",
                "type": "SecureString"
            },
            "tenant": "tenant ID",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="properties"></a>Özellikler

| Özellik             | Açıklama                              | Gerekli                                 |
| -------------------- | ---------------------------------------- | ---------------------------------------- |
| type                 | Type özelliği ayarlanmalıdır: **AzureDataLakeAnalytics**. | Evet                                      |
| accountName          | Azure Data Lake Analytics hesap adı.  | Evet                                      |
| dataLakeAnalyticsUri | Azure Data Lake Analytics URI.           | Hayır                                       |
| subscriptionId       | Azure abonelik kimliği                    | Hayır                                       |
| resourceGroupName    | Azure kaynak grubu adı                | Hayır                                       |
| servicePrincipalId   | Uygulamanın istemci kimliği belirtin.     | Evet                                      |
| servicePrincipalKey  | Uygulama anahtarını belirtin.           | Evet                                      |
| tek               | Kiracı bilgileri (etki alanı adı veya Kiracı kimliği), uygulamanızın bulunduğu altında belirtin. Azure portalının sağ üst köşedeki fare getirerek geri alabilirsiniz. | Evet                                      |
| connectVia           | Bu bağlı hizmeti için etkinlikler gönderilmesi için kullanılacak Integration Runtime. Azure tümleştirme çalışma zamanı veya şirket içinde barındırılan Integration Runtime'ı kullanabilirsiniz. Belirtilmezse, varsayılan Azure Integration Runtime kullanır. | Hayır                                       |



## <a name="azure-databricks-linked-service"></a>Azure Databricks bağlı hizmeti
Oluşturabileceğiniz **Azure Databricks bağlı hizmeti** Databricks workloads(notebooks) çalıştırmak için kullanacağınız bir Databricks çalışma alanı kaydetmek için.

### <a name="example---using-new-job-cluster-in-databricks"></a>Örnek - yeni proje kümesi Databricks'te kullanarak

```json
{
    "name": "AzureDatabricks_LS",
    "properties": {
        "type": "AzureDatabricks",
        "typeProperties": {
            "domain": "https://eastus.azuredatabricks.net",
            "newClusterNodeType": "Standard_D3_v2",
            "newClusterNumOfWorker": "1:10",
            "newClusterVersion": "4.0.x-scala2.11",
            "accessToken": {
                "type": "SecureString",
                "value": "dapif33c9c721144c3a790b35000b57f7124f"
            }
        }
    }
}

```

### <a name="example---using-existing-interactive-cluster-in-databricks"></a>Örnek - Databricks'te mevcut etkileşimli kümesi kullanma

```json
{
    "name": " AzureDataBricksLinedService",
    "properties": {
      "type": " AzureDatabricks",
      "typeProperties": {
        "domain": "https://westeurope.azuredatabricks.net",
        "accessToken": {
            "type": "SecureString", 
            "value": "dapif33c9c72344c3a790b35000b57f7124f"
          },
        "existingClusterId": "{clusterId}"
        }
}

```

### <a name="properties"></a>Özellikler

| Özellik             | Açıklama                              | Gerekli                                 |
| -------------------- | ---------------------------------------- | ---------------------------------------- |
| name                 | Bağlı hizmetin adı               | Evet   |
| tür                 | Type özelliği ayarlanmalıdır: **AzureDatabricks**. | Evet                                      |
| etki alanı               | Buna göre bir Databricks çalışma alanı, bölgeye göre Azure bölgesi belirtin. Örnek: https://eastus.azuredatabricks.net | Evet                                 |
| accessToken          | Data factory'nin Azure Databricks için kimlik doğrulaması erişim belirteci gereklidir. Erişim belirteci databricks çalışma alanından oluşturulması gerekir. Erişim belirteci bulunabilir bulmak için aşağıdaki adımları ayrıntılı [burada](https://docs.azuredatabricks.net/api/latest/authentication.html#generate-token)  | Evet                                       |
| existingClusterId    | Bu tüm işleri çalıştırmak için var olan bir kümenin küme kimliği. Bu, zaten oluşturulmuş bir etkileşimli kümede olmalıdır. Yanıt vermeyi durdurursa kümeyi el ile yeniden başlatmanız gerekebilir. Daha fazla güvenilirlik için yeni kümelerinde çalışan işlerin Databricks önerin. Küme Kimliği bulabilirsiniz kümeler, etkileşimli bir çalışma alanı -> Databricks kümesinde etkileşimli küme adı -> yapılandırma -> etiketleri ->. [Daha fazla ayrıntı](https://docs.databricks.com/user-guide/clusters/tags.html) | Hayır 
| newClusterVersion    | Spark kümesi sürümü. Databricks içinde bir proje kümesi oluşturun. | Hayır  |
| newClusterNumOfWorker| Bu küme olması gereken alt düğüm sayısı. Bir kümenin bir Spark sürücüsü ve num_workers yürütücüler num_workers + 1 Spark düğümleri toplam vardır. Int32 biçimlendirilmiş bir dize, "1" anlamına gelir numOfWorker gibi 1 veya "1:10" otomatik ölçeklendirme, en az 1 ve en yüksek olarak 10 anlamına gelir.  | Hayır                |
| newClusterNodeType   | Bu alan, bu kümesinde Spark düğümlerinin her biri için kullanılabilir kaynakları tek bir değer olarak kodlar. Örneğin, düğümlerin sağlanabilir ve bellek veya işlem gücü kullanımlı iş yükleri için iyileştirilmiş Spark Bu alan yeni küme için gereklidir                | Hayır               |
| newClusterSparkConf  | İsteğe bağlı, kullanıcı tarafından belirtilen Spark yapılandırma anahtar-değer çiftleri kümesi. Kullanıcılar ayrıca ek JVM seçenekleri bir dizede Yürütücü ve sürücü ile spark.driver.extraJavaOptions ve spark.executor.extraJavaOptions sırasıyla geçirebilirsiniz. | Hayır  |


## <a name="azure-sql-database-linked-service"></a>Azure SQL Veritabanı bağlı hizmeti
Bir Azure SQL bağlı hizmeti oluşturma ve kullanılmakta olan [saklı yordam etkinliğine](transform-data-using-stored-procedure.md) Data Factory işlem hattı bir saklı yordam çağırmak için. Bkz: [Azure SQL Bağlayıcısı](connector-azure-sql-database.md#linked-service-properties) bu bağlı hizmeti hakkında bilgi için makalenin.

## <a name="azure-sql-data-warehouse-linked-service"></a>Azure SQL veri ambarı bağlı hizmeti
Bir Azure SQL veri ambarı bağlı hizmetini oluşturmak ve kullanılmakta olan [saklı yordam etkinliğine](transform-data-using-stored-procedure.md) Data Factory işlem hattı bir saklı yordam çağırmak için. Bkz: [Azure SQL veri ambarı Bağlayıcısı](connector-azure-sql-data-warehouse.md#linked-service-properties) bu bağlı hizmeti hakkında bilgi için makalenin.

## <a name="sql-server-linked-service"></a>SQL Server bağlı hizmeti
SQL Server bağlı hizmeti oluşturma ve kullanılmakta olan [saklı yordam etkinliğine](transform-data-using-stored-procedure.md) Data Factory işlem hattı bir saklı yordam çağırmak için. Bkz: [SQL Server Bağlayıcısı](connector-sql-server.md#linked-service-properties) bu bağlı hizmeti hakkında bilgi için makalenin.

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory tarafından desteklenen dönüştürme etkinliklerinin listesi için bkz. [verileri dönüştürme](transform-data.md).
