---
title: "Azure Data Factory ile desteklenen ortamlar işlem | Microsoft Docs"
description: "Dönüştürme veya işlem verileri (örneğin, Azure Hdınsight) Azure Data Factory işlem hatlarını kullanabileceğiniz bilgi işlem ortamları hakkında bilgi edinin."
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/10/2017
ms.author: shengc
ms.openlocfilehash: a530b08c276596ddbffafc21e6cffdd9e0e9e3fa
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>Azure Data Factory ile desteklenen ortamlar işlem
Bu makalede, işlem veya dönüştürme veri için kullanabileceğiniz farklı bilgi işlem ortamları açıklanmaktadır. (İsteğe bağlı karşılaştırması Getir kendi) farklı yapılandırmaları hakkındaki ayrıntıları bu bağlama bağlı hizmetler yapılandırırken Data Factory ile desteklenen ortamlar için bir Azure data factory işlem sağlar.

Aşağıdaki tabloda, bilgi işlem ortamları Data Factory ve bunlar üzerinde çalışan etkinlikleri tarafından desteklenen bir listesini sağlar. 

| İşlem ortamı                      | etkinlikler                               |
| ---------------------------------------- | ---------------------------------------- |
| [İsteğe bağlı Hdınsight kümesi](#azure-hdinsight-on-demand-linked-service) veya [kendi Hdınsight kümenizi](#azure-hdinsight-linked-service) | [Hive](transform-data-using-hadoop-hive.md), [Pig](transform-data-using-hadoop-pig.md), [Spark](transform-data-using-spark.md), [MapReduce](transform-data-using-hadoop-map-reduce.md), [Hadoop akış](transform-data-using-hadoop-streaming.md) |
| [Azure toplu işlem](#azure-batch-linked-service) | [Özel](transform-data-using-dotnet-custom-activity.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) | [Machine Learning etkinlikleri: Toplu Yürütme ve Kaynak Güncelleştirme](transform-data-using-machine-learning.md) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) | [Data Lake Analytics U-SQL](transform-data-using-data-lake-analytics.md) |
| [Azure SQL](#azure-sql-linked-service), [Azure SQL veri ambarı](#azure-sql-data-warehouse-linked-service), [SQL Server](#sql-server-linked-service) | [Saklı Yordam](transform-data-using-stored-procedure.md) |

>  

## <a name="on-demand-compute-environment"></a>İsteğe bağlı bilgi işlem ortamı
Bu tür yapılandırmada bilgi işlem ortamı tam olarak Azure Data Factory hizmeti tarafından yönetilir. Bir işi veri işlemek için gönderildi ve iş tamamlandığında, kaldırılan önce Data Factory hizmeti tarafından otomatik olarak oluşturulur. İsteğe bağlı bilgi işlem ortamı için bağlı hizmet oluşturma yapılandırın ve iş yürütme, küme yönetimi ve eylemler önyükleme için ayrıntılı ayarları denetler.

> [!NOTE]
> İsteğe bağlı yapılandırma şu anda yalnızca Azure Hdınsight kümeleri için desteklenir.
>
> 

## <a name="azure-hdinsight-on-demand-linked-service"></a>Azure Hdınsight isteğe bağlı hizmeti
Azure Data Factory hizmetinin otomatik olarak verileri işlemek için isteğe bağlı Hdınsight kümesi oluşturabilirsiniz. Küme kümeyle ilişkili depolama hesabı (JSON özelliğinde linkedServiceName) ile aynı bölgede oluşturulur. Depolama hesabı, genel amaçlı standart Azure depolama hesabınızın olması gerekir. 

Aşağıdakilere dikkat edin **önemli** noktaları hakkında isteğe bağlı Hdınsight bağlı hizmeti:

* İsteğe bağlı Hdınsight kümesi, Azure aboneliğinizin altında oluşturulur. Görmeyebilir küme yukarı olduğunda, Azure portalında küme ve çalışır durumda. 
* Bir isteğe bağlı Hdınsight kümesinde çalışan işleri için günlükleri Hdınsight kümesi ile ilişkilendirilmiş depolama hesabına kopyalanır. ClusterUserName, clusterPassword clusterSshUserName, bağlantılı hizmet tanımı'nda tanımlanan clusterSshPassword küme yaşam döngüsü sırasında geniş kapsamlı sorun giderme için küme oturum açmak için kullanılır. 
* Yalnızca Hdınsight kümesi yukarı olduğunda zaman ve çalışan işleri için sizden ücret kesilir.

> [!IMPORTANT]
> Genellikle sürer **20 dakika** veya isteğe bağlı Azure Hdınsight kümesi sağlamak için daha fazla bilgi.
>
> 

### <a name="example"></a>Örnek
Aşağıdaki JSON Linux tabanlı isteğe bağlı Hdınsight bağlı hizmeti tanımlar. Data Factory hizmetinin otomatik olarak oluşturur bir **Linux tabanlı** gerekli etkinliği işlemek için Hdınsight kümesi. 

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
> Daha fazla etkinlik çalışırken, çok sayıda kapsayıcıları, Azure blob depolama alanına bakın. İşlerin sorunları giderilmesi için bunlara gerek yoksa, depolama maliyetini azaltmak için bunları silmek isteyebilirsiniz. Bu kapsayıcı adları bir düzene sahiptir: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. Azure blob depolamada kapsayıcı silmek için [Microsoft Storage Gezgini](http://storageexplorer.com/) gibi araçları kullanın.
>
> 

### <a name="properties"></a>Özellikler
| Özellik                     | Açıklama                              | Gerekli |
| ---------------------------- | ---------------------------------------- | -------- |
| type                         | Type özelliği ayarlanmalı **HDInsightOnDemand**. | Evet      |
| ClusterSize                  | Kümedeki çalışan/veri düğüm sayısı. Hdınsight kümesi için bu özelliği belirtmeniz çalışan düğüm sayısı ile birlikte 2 baş düğümler ile oluşturulur. Düğüm boyutu 4 çalışan düğümlü bir küme 24 çekirdek alır, böylece 4 çekirdeğe sahip Standard_D3 olduğundan (4\*4 = 16 çekirdek çalışan düğümleri artı 2\*4 = 8 çekirdek baş düğümler için). Bkz: [Hdınsight Hadoop, Spark, Kafka ve daha fazla ile kümelerde ayarlama](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) Ayrıntılar için. | Evet      |
| linkedServiceName            | Depolamak ve veri işleme için isteğe bağlı küme tarafından kullanılacak azure depolama bağlı hizmeti. Hdınsight kümesi, bu Azure depolama hesabı ile aynı bölgede oluşturulur. Azure HDInsight, desteklediği her bir Azure bölgesinde kullanabileceğiniz toplam çekirdek sayısıyla ilgili sınırlamaya sahiptir. Bu Azure bölgesinde gerekli clusterSize karşılamak için yeterli çekirdek kotası olduğundan emin olun. Ayrıntılar için başvurmak [Hdınsight Hadoop, Spark, Kafka ve daha fazla ile kümelerde ayarlama](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md)<p>Şu anda bir Azure Data Lake Store depolama alanı olarak kullanan bir isteğe bağlı Hdınsight kümesi oluşturulamıyor. Bir Azure Data Lake Store'da işleme Hdınsight sonuç verileri depolamak istiyorsanız, Azure Blob depolama alanından Azure Data Lake Store'a veri kopyalamak için kopyalama etkinliği kullanın. </p> | Evet      |
| clusterResourceGroup         | Hdınsight kümesi bu kaynak grubunda oluşturulur. | Evet      |
| TimeToLive                   | İsteğe bağlı Hdınsight kümesi için izin verilen boşta kalma süresi. Ne kadar süreyle isteğe bağlı Hdınsight kümesi kümedeki diğer etkin iş yok varsa bir etkinlik tamamlandıktan sonra canlı kalır belirtir. Değer izin verilen en az 5 dakikadır (00: 05:00).<br/><br/>Örneğin, bir etkinlik Çalıştır 6 dakika sürer ve timetolive 5 dakika olarak ayarlanmıştır, küme 6 etkinlik işleme dakika çalıştırdıktan sonra 5 dakika boyunca etkin kalır. Başka bir etkinlik 6 dakika penceresiyle yürütülürse, aynı küme tarafından işlenir.<br/><br/>İsteğe bağlı Hdınsight kümesi oluşturma bir pahalı işlemi (işlem zaman alabilir), bunu kullanımı isteğe bağlı Hdınsight kümesi yeniden kullanarak bir veri fabrikası performansını artırmak için bu ayarı olarak gerekli olur.<br/><br/>Timetolive değeri 0 olarak ayarlarsanız, küme etkinlik Çalıştır tamamlandıktan hemen sonra silindi. Yüksek bir değer ayarlarsanız, küme, bazı sorun giderme için oturum açmak boşta kalır ancak amacı, ancak yüksek maliyetleri de neden olabilir. Bu nedenle, gereksinimlerinize göre uygun değere ayarlamak önemlidir.<br/><br/>Timetolive özellik değerini uygun şekilde ayarlarsanız, birden çok ardışık düzen isteğe bağlı Hdınsight kümesi örneğini paylaşabilirsiniz. | Evet      |
| clusterType                  | Oluşturulacak Hdınsight kümesi türü. İzin verilen değerler "hadoop" ve "spark" tür. Belirtilmezse, varsayılan değer hadoop olur. | Hayır       |
| Sürüm                      | Hdınsight küme sürümü. Belirtilmezse, geçerli Hdınsight tanımlanan varsayılan sürümü kullanıyor. | Hayır       |
| hostSubscriptionId           | Hdınsight kümesi oluşturmak için kullanılan Azure abonelik kimliği. Belirtilmezse, Azure oturum açma içeriğiniz abonelik Kimliğini kullanır. | Hayır       |
| clusterNamePrefix           | HDI küme adı, bir zaman damgası önek küme adının sonunda otomatik olarak eklenir| Hayır       |
| sparkVersion                 | Küme türü "Spark" ise spark sürümü | Hayır       |
| additionalLinkedServiceNames | Data Factory hizmetinin bunları sizin adınıza kaydedebilirsiniz böylece Hdınsight için ek depolama hesapları hizmeti bağlı belirtir. Bu depolama hesaplarından linkedServiceName tarafından belirtilen depolama hesabı ile aynı bölgede oluşturulan Hdınsight kümesi ile aynı bölgede olması gerekir. | Hayır       |
| osType                       | İşletim sistemi türü. İzin verilen değerler: Linux ve Windows (için yalnızca Hdınsight 3.3). Linux varsayılandır. | Hayır       |
| hcatalogLinkedServiceName    | Azure SQL adını HCatalog veritabanına işaret hizmeti bağlı. İsteğe bağlı Hdınsight kümesi meta depo Azure SQL veritabanı kullanılarak oluşturulur. | Hayır       |
| connectVia                   | Bu Hdınsight bağlı hizmeti etkinlikleri gönderilmesi için kullanılacak tümleştirme çalışma. İsteğe bağlı Hdınsight bağlı hizmeti için Azure tümleştirmesi çalışma zamanı yalnızca destekler. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. | Hayır       |
| clusterUserName                   | Kümeye erişmek için kullanıcı adı. | Hayır       |
| clusterPassword                   | Parola kümeye erişmek için güvenli dize türünde. | Hayır       |
| clusterSshUserName         | SSH kullanıcı adına (Linux için) uzaktan kümenin düğüm bağlayın. | Hayır       |
| clusterSshPassword         | SSH için güvenli bir dize türü parolayı (Linux için) uzaktan kümenin düğüm bağlayın. | Hayır       |


> [!IMPORTANT]
> Hdınsight dağıtılabilir birden çok Hadoop küme sürümlerindeki destekler. Her sürüm seçimi Hortonworks veri Platformu (HDP) dağıtım belirli bir sürümü ve o dağıtım içinde bulunan bileşenleri kümesi oluşturur. Desteklenen sürümlerin listesi için Hdınsight son Hadoop ekosistemi bileşenlerini ve düzeltmeleri sağlamak için güncelleştirilmesini engeller. En son bilgileri her zaman başvurun emin olun [desteklenen Hdınsight sürümü ve işletim sistemi türü](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) Hdınsight'ın desteklenen sürümünü kullandığınızdan emin olmak için. 
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

İsteğe bağlı Hdınsight bağlı hizmeti sizin adınıza Hdınsight kümeleri oluşturmak için bir hizmet asıl kimlik doğrulaması gerektirir. Hizmet asıl kimlik doğrulaması kullanmak için Azure Active Directory (Azure AD) bir uygulama varlığı kaydetmek ve onu vermek için **katkıda bulunan** rol abonelik veya kaynak grubu Hdınsight kümesi oluşturulur. Ayrıntılı adımlar için bkz: [bir Azure Active Directory kaynaklara erişebilir uygulama ve hizmet sorumlusu oluşturmak için kullanım portal](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal). Bağlantılı hizmet tanımlamak için kullandığınız aşağıdaki değerleri not edin:

- Uygulama Kimliği
- Uygulama anahtarı 
- Kiracı Kimliği

Hizmet asıl kimlik doğrulaması, aşağıdaki özellikleri belirterek kullanın:

| Özellik                | Açıklama                              | Gerekli |
| :---------------------- | :--------------------------------------- | :------- |
| **servicePrincipalId**  | Uygulamanın istemci kimliği belirtin.     | Evet      |
| **servicePrincipalKey** | Uygulamanın anahtarını belirtin.           | Evet      |
| **Kiracı**              | Uygulamanızın bulunduğu altında Kiracı bilgileri (etki alanı adı veya Kiracı kimliği) belirtin. Azure portalının sağ üst köşedeki fare gelerek alabilir. | Evet      |

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
Head, veriler ve aşağıdaki özellikleri kullanarak zookeeper düğümleri boyutunu belirtebilirsiniz: 

| Özellik          | Açıklama                              | Gerekli |
| :---------------- | :--------------------------------------- | :------- |
| headNodeSize      | Baş düğüm boyutunu belirtir. Varsayılan değer: Standard_D3. Bkz: **düğümü boyutları belirtme** ayrıntıları bölümü. | Hayır       |
| dataNodeSize      | Veri düğümü boyutunu belirtir. Varsayılan değer: Standard_D3. | Hayır       |
| zookeeperNodeSize | Zoo Keeper düğüm boyutunu belirtir. Varsayılan değer: Standard_D3. | Hayır       |

#### <a name="specifying-node-sizes"></a>Düğümü boyutları belirtme
Bkz: [sanal makine boyutları](../virtual-machines/linux/sizes.md) makale dize değerleri önceki bölümde belirtildiği özellikleri için belirtmeniz gerekir. Değerleri uygun gerek **cmdlet'leri & API'leri** makalesinde başvurulan. Makalede görebileceğiniz gibi büyük (varsayılan) boyutu veri düğümünün senaryonuz için yeterince iyi olmayabilir 7 GB bellek vardır. 

Boyutta D4 baş düğümler ve çalışan düğümleri oluşturmak istiyorsanız, belirtin **Standard_D4** headNodeSize ve dataNodeSize özellikleri için değer olarak. 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Bu özellikler için yanlış bir değer belirtirseniz, aşağıdaki alabilirsiniz **hata:** küme oluşturma başarısız oldu. Özel durum: Küme oluşturma işlemi tamamlanamadı. İşlem '400' koduyla başarısız oldu. Küme geride bırakma durumu: 'Hata'. İleti: 'PreClusterCreationValidationFailure'. Bu hata iletisini zaman kullandığınızdan emin olun **CMDLET & API'leri** tablosundan ad [sanal makine boyutları](../virtual-machines/linux/sizes.md) makalesi.        

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
| Özellik          | Açıklama                              | Gerekli |
| ----------------- | ---------------------------------------- | -------- |
| type              | Type özelliği ayarlanmalı **Hdınsight**. | Evet      |
| clusterUri        | Hdınsight küme URI'si.        | Evet      |
| kullanıcı adı          | Var olan bir Hdınsight kümesine bağlanmak için kullanılacak kullanıcı adını belirtin. | Evet      |
| password          | Kullanıcı hesabı için parola belirtin.   | Evet      |
| linkedServiceName | Hdınsight küme tarafından kullanılan Azure blob depolama başvurduğu Azure Storage bağlı hizmetin adı. <p>Şu anda bu özellik için bir Azure Data Lake Store bağlı belirtemezsiniz. Hdınsight kümesi için Data Lake Store erişimi varsa, Azure Data Lake Store'da verileri Hive/Pig komut dosyalarından erişebilir. </p> | Evet      |
| connectVia        | Bu bağlı hizmetin etkinlikleri gönderilmesi için kullanılacak tümleştirme çalışma. Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. | Hayır       |

> [!IMPORTANT]
> Hdınsight dağıtılabilir birden çok Hadoop küme sürümlerindeki destekler. Her sürüm seçimi Hortonworks veri Platformu (HDP) dağıtım belirli bir sürümü ve o dağıtım içinde bulunan bileşenleri kümesi oluşturur. Desteklenen sürümlerin listesi için Hdınsight son Hadoop ekosistemi bileşenlerini ve düzeltmeleri sağlamak için güncelleştirilmesini engeller. En son bilgileri her zaman başvurun emin olun [desteklenen Hdınsight sürümü ve işletim sistemi türü](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) Hdınsight'ın desteklenen sürümünü kullandığınızdan emin olmak için. 
>

## <a name="azure-batch-linked-service"></a>Azure Batch bağlı hizmeti

Batch havuzundaki sanal makineler (VM'ler) kaydetmek için bir Azure Batch bağlantılı hizmeti bir veri fabrikası oluşturabilirsiniz. Azure Batch kullanarak özel etkinliği çalıştırabilirsiniz.

Azure Batch hizmetine yeniyseniz, aşağıdaki konularda bakın:

* [Azure Batch Temelleri](../batch/batch-technical-overview.md) Azure Batch hizmetinin genel bakış.
* [New-AzureRmBatchAccount](/powershell/module/azurerm.batch/New-AzureRmBatchAccount?view=azurermps-4.3.1) bir Azure Batch hesabı oluşturmak için cmdlet'i (veya) [Azure portal](../batch/batch-account-create-portal.md) Azure portalını kullanarak Azure Batch hesabı oluşturmak için. Bkz: [Azure Batch hesabını yönetmek için PowerShell kullanarak](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) konu cmdlet kullanma hakkında ayrıntılı yönergeler için.
* [Yeni-AzureBatchPool](/powershell/module/azurerm.batch/New-AzureBatchPool?view=azurermps-4.3.1) bir Azure Batch havuzu oluşturmak için cmdlet'i.

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
| type              | Type özelliği ayarlanmalı **AzureBatch**. | Evet      |
| accountName       | Azure toplu işlem hesabının adı.         | Evet      |
| accessKey         | Azure Batch hesabı için erişim anahtarı.  | Evet      |
| batchUri          | URL https:// biçiminde Azure Batch hesabınıza*batchaccountname.region*. batch.azure.com. | Evet      |
| poolName          | Sanal makinelerin havuzunun adı.    | Evet      |
| linkedServiceName | Azure depolama adı hizmeti bağlı Azure Batch hizmeti ile ilişkili bağlı. Bu bağlı hizmetin etkinliği çalıştırmak için gereken dosyaları hazırlama için kullanılır. | Evet      |
| connectVia        | Bu bağlı hizmetin etkinlikleri gönderilmesi için kullanılacak tümleştirme çalışma. Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. | Hayır       |

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning bağlı hizmeti
Bir veri fabrikası Puanlama uç noktası bir Machine Learning toplu kaydetmek için bir Azure Machine Learning bağlantılı hizmeti oluşturun.

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
| Tür                   | Type özelliği ayarlanmalıdır: **AzureML**. | Evet                                      |
| mlEndpoint             | Toplu Puanlama URL.                   | Evet                                      |
| apikey ile yapılan                 | Yayımlanan çalışma alanı modelinin API.     | Evet                                      |
| updateResourceEndpoint | Tahmine dayalı Web hizmeti ile eğitilen model dosyasını güncelleştirmek için kullanılan Azure ML Web Hizmeti uç noktası için güncelleştirme kaynak URL | Hayır                                       |
| servicePrincipalId     | Uygulamanın istemci kimliği belirtin.     | UpdateResourceEndpoint belirttiyseniz gereklidir |
| servicePrincipalKey    | Uygulamanın anahtarını belirtin.           | UpdateResourceEndpoint belirttiyseniz gereklidir |
| Kiracı                 | Uygulamanızın bulunduğu altında Kiracı bilgileri (etki alanı adı veya Kiracı kimliği) belirtin. Azure portalının sağ üst köşedeki fare gelerek alabilir. | UpdateResourceEndpoint belirttiyseniz gereklidir |
| connectVia             | Bu bağlı hizmetin etkinlikleri gönderilmesi için kullanılacak tümleştirme çalışma. Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. | Hayır                                       |

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics bağlı hizmeti
Oluşturduğunuz bir **Azure Data Lake Analytics** bir Azure Data Lake Analytics bağlamak için bağlantılı hizmeti bir Azure data factory hizmetine işlem. Data Lake Analytics U-SQL etkinliği ardışık düzeninde bu bağlı hizmetin başvuruyor. 

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
| subscriptionId       | Azure abonelik kimliği                    | Hayır (belirtilmezse, data Factory abonelik kullanılır). |
| resourceGroupName    | Azure kaynak grubu adı                | Hayır (belirtilmezse, kaynak grubu data Factory kullanılır). |
| servicePrincipalId   | Uygulamanın istemci kimliği belirtin.     | Evet                                      |
| servicePrincipalKey  | Uygulamanın anahtarını belirtin.           | Evet                                      |
| Kiracı               | Uygulamanızın bulunduğu altında Kiracı bilgileri (etki alanı adı veya Kiracı kimliği) belirtin. Azure portalının sağ üst köşedeki fare gelerek alabilir. | Evet                                      |
| connectVia           | Bu bağlı hizmetin etkinlikleri gönderilmesi için kullanılacak tümleştirme çalışma. Azure tümleştirmesi çalışma zamanı veya Self-hosted tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. | Hayır                                       |



## <a name="azure-sql-database-linked-service"></a>Azure SQL Veritabanı bağlı hizmeti
Azure SQL bağlı hizmeti oluşturma ve onunla kullanma [saklı yordam etkinliği](transform-data-using-stored-procedure.md) Data Factory işlem hattı bir saklı yordam çağırmak için. Bkz: [Azure SQL Bağlayıcısı](connector-azure-sql-database.md#linked-service-properties) makale bu bağlı hizmetin hakkında ayrıntılı bilgi için.

## <a name="azure-sql-data-warehouse-linked-service"></a>Azure SQL Data Warehouse bağlı hizmeti
Bir Azure SQL Data Warehouse bağlı hizmet oluşturma ve onunla kullanma [saklı yordam etkinliği](transform-data-using-stored-procedure.md) Data Factory işlem hattı bir saklı yordam çağırmak için. Bkz: [Azure SQL Data Warehouse Bağlayıcısı](connector-azure-sql-data-warehouse.md#linked-service-properties) makale bu bağlı hizmetin hakkında ayrıntılı bilgi için.

## <a name="sql-server-linked-service"></a>SQL Server bağlantılı hizmeti
Bir SQL Server bağlantılı hizmet oluşturma ve onunla kullanma [saklı yordam etkinliği](transform-data-using-stored-procedure.md) Data Factory işlem hattı bir saklı yordam çağırmak için. Bkz: [SQL Server Bağlayıcısı](connector-sql-server.md#linked-service-properties) makale bu bağlı hizmetin hakkında ayrıntılı bilgi için.

## <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - adlandırma kuralları
Aşağıdaki tabloda için Data Factory yapıtlarının adlandırma kuralları sağlar.

| Ad                             | Ad benzersizliği                          | Doğrulama denetimleri                        |
| :------------------------------- | :--------------------------------------- | :--------------------------------------- |
| Data Factory                     | Microsoft Azure arasında benzersiz. Adları büyük küçük harf duyarsız, diğer bir deyişle, `MyDF` ve `mydf` aynı veri fabrikası bakın. | <ul><li>Her veri fabrikası tam olarak bir Azure aboneliğine bağlıdır.</li><li>Nesne adları bir harf veya sayı ile başlamalı ve yalnızca harf, rakam ve tire (-) karakterini içerebilir.</li><li>Her tire (-) karakterinden hemen önünde ve bir harf veya sayı gelmelidir gerekir. Kapsayıcı adlarında art arda tirelere izin verilmez.</li><li>Adı 3-63 karakter uzunluğunda olabilir.</li></ul> |
| Bağlı hizmetler/tablolar/işlem hatları | Veri Fabrikası'nda ile benzersiz. Adları büyük/küçük harfe duyarsızdır. | <ul><li>Bir tablo adı karakter sayısı: 260.</li><li>Nesne adları bir harf, sayı veya alt çizgi (_) ile başlamalıdır.</li><li>Şu karakterler kullanılamaz: ".", "+","?", "/", "<", ">","*", "%", "&", ":","\\"</li></ul> |
| Kaynak Grubu                   | Microsoft Azure arasında benzersiz. Adları büyük/küçük harfe duyarsızdır. | <ul><li>En fazla karakter sayısı: 1000.</li><li>Ad harf, rakam ve şu karakterleri içerebilir: "-", "_",","ve"."</li></ul> |

## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory ile desteklenen dönüştürme etkinliklerinin listesi için bkz: [verileri](transform-data.md).
