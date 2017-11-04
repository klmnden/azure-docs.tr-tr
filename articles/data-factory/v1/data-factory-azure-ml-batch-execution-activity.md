---
title: "Azure Data Factory kullanarak Tahmine dayalı veri ardışık düzen oluşturun | Microsoft Docs"
description: "Nasıl oluşturulacağını açıklar Azure Data Factory ve Azure Machine Learning kullanarak Tahmine dayalı ardışık düzen oluşturun"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 4fad8445-4e96-4ce0-aa23-9b88e5ec1965
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: 3169584bc884107ccd34b01264683d8c73c0fecb
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a>Azure Machine Learning ve Azure Data Factory kullanarak Tahmine dayalı ardışık düzen oluşturun

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive etkinliği](data-factory-hive-activity.md) 
> * [Pig etkinliği](data-factory-pig-activity.md)
> * [MapReduce etkinliği](data-factory-map-reduce.md)
> * [Hadoop akış etkinliği](data-factory-hadoop-streaming-activity.md)
> * [Spark etkinliği](data-factory-spark.md)
> * [Machine Learning Batch Yürütme Etkinliği](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Kaynak Güncelleştirme Etkinliği](data-factory-azure-ml-update-resource-activity.md)
> * [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL Etkinliği](data-factory-usql-activity.md)
> * [.NET özel etkinlik](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Giriş
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [dönüştürme Data Factory sürüm 2 machine learning kullanarak verileri](../transform-data-using-machine-learning.md).


### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) derleme, test ve Tahmine dayalı analiz çözümlerini dağıtma olanak tanır. Bir üst düzey açısından bakıldığında, üç adımda gerçekleştirilir:

1. **Eğitim denemenizi oluşturma**. Azure ML Studio kullanarak bu adımı uygulayın. ML studio eğitmek ve eğitim verileri kullanarak bir Tahmine dayalı bir analiz modeli test etmek için kullandığınız bir görsel işbirlikçi geliştirme ortamıdır.
2. **Tahmine dayalı bir deneme Dönüştür**. Modelinizi var olan verilerle eğitilmiş ve yeni verilerinizi puanlamada için kullanıma hazır sonra hazırlamak ve puanlama için denemenizi kolaylaştırır.
3. **Web hizmeti olarak dağıtabilir**. Puanlama denemenizi bir Azure web hizmeti olarak yayımlayabilirsiniz. Bu web hizmeti uç noktası aracılığıyla modelde veri gönderebilir ve sonuç tahminleri model Excel'den alabilirsiniz.  

### <a name="azure-data-factory"></a>Azure Data Factory
Data Factory, verilerin **taşınmasını** ve **dönüştürülmesini** düzenleyen ve otomatikleştiren bulut tabanlı bir veri tümleştirme hizmetidir. Çeşitli veri depolarına verilerden alma, veri dönüştürme/işlemi ve sonuç verileri veri depoları yayımlama Azure Data Factory kullanarak veri tümleştirme çözümleri oluşturabilirsiniz.

Data Factory hizmeti verileri taşıyıp dönüştüren ve ardından işlem hattını belirli bir zamanlamaya (saatlik, günlük, haftalık, vb.) göre çalıştıran veri işlem hatları oluşturmanıza imkan tanır. Ayrıca, veri işlem hatlarınız arasındaki çizgileri ve bağımlılıkları gösteren ve sorunları kolayca saptamak ve izleme uyarılarını ayarlamak üzere tek bir birleşik görünümden tüm veri işlem hatlarınızı izleyen zengin görsel öğeler sağlar.

Bkz: [Azure Data Factory'ye giriş](data-factory-introduction.md) ve [ilk işlem hattınızı oluşturma](data-factory-build-your-first-pipeline.md) makaleler Azure Data Factory hizmetiyle hızlıca başlamak için.

### <a name="data-factory-and-machine-learning-together"></a>Veri Fabrikası ve Machine Learning birlikte
Azure Data Factory bir yayımlanan kullanmak ardışık düzen kolayca oluşturmanıza olanak sağlar [Azure Machine Learning] [ azure-machine-learning] web hizmeti Tahmine dayalı analiz için. Kullanarak **toplu iş yürütme etkinliği** bir Azure Data Factory işlem hattı verileri toplu tahminleri yapmak için bir Azure ML web hizmeti çağırabilirsiniz. Bkz: [toplu iş yürütme etkinliği kullanarak bir Azure ML web Hizmeti'ni çağırmadan](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) ayrıntıları bölümü.

Zaman içinde denemeler Puanlama Azure ML Tahmine dayalı modelleri yeni giriş veri kümeleri kullanarak retrained gerekir. Aşağıdakileri yaparak bir Data Factory işlem hattı Azure ML modelden yeniden eğitme:

1. Eğitim denemenizi (değil Tahmine dayalı denemeye) bir web hizmeti olarak yayımlayın. Tahmine dayalı denemeye önceki senaryoda bir web hizmeti olarak kullanıma sunmak için yaptığınız gibi Azure ML Studio'da bu adımı uygulayın.
2. Eğitim denemenizi web hizmetini çağırmak için Azure ML toplu iş yürütme etkinliği kullanın. Temel olarak, eğitim web hizmeti ve puanlama web hizmetini çağırmak için Azure ML toplu iş yürütme etkinliği kullanın.

Yeniden eğitme ile tamamladıktan sonra Puanlama web hizmeti (web hizmeti olarak sunulan Tahmine dayalı denemeye) ile yeni eğitilen modelini kullanarak güncelleştirme **Azure ML güncelleştirme kaynak etkinliği**. Bkz: [kaynak güncelleştirme etkinliği kullanarak modelleri güncelleştirme](data-factory-azure-ml-update-resource-activity.md) Ayrıntılar için makale.

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Toplu iş yürütme etkinliği kullanarak bir web hizmeti çağırma
Azure Data Factory veri hareketlerini ve işleme düzenlemek için kullanın ve sonra da toplu iş yürütme Azure Machine Learning kullanarak gerçekleştirin. Üst düzey adımlar şunlardır:

1. Bir Azure Machine Learning bağlantılı hizmeti oluşturun. Aşağıdaki değerleri gerekir:

   1. **İstek URI'si** toplu iş yürütme API. İstek URI'Sİ'i tıklatarak bulabilirsiniz **toplu iş yürütme** web Hizmetleri sayfasında bağlantı.
   2. **API anahtarı** için yayımlanan Azure Machine Learning web hizmeti. API anahtarını yayımladığınız web hizmeti tıklatarak bulabilirsiniz.
   3. Kullanım **AzureMLBatchExecution** etkinlik.

      ![Machine Learning Panosu](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

      ![Toplu URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)

### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Senaryo: Web hizmeti girişleri/verileri Azure Blob Depolama başvuran çıkışları kullanarak denemelerini
Bu senaryoda, Azure Machine Learning Web hizmeti bir Azure blob depolama alanındaki bir dosyadan veri kullanarak tahminleri yapar ve tahmin sonuçlarını blob depolama alanında depolar. Aşağıdaki JSON Data Factory işlem hattı AzureMLBatchExecution etkinliği ile tanımlar. Veri kümesini etkinlik sahip **DecisionTreeInputBlob** giriş olarak ve **DecisionTreeResultBlob** çıktı olarak. **DecisionTreeInputBlob** kullanarak geçirilen web hizmeti tarafından bir girdi olarak **WebServiceInput etkinliğine** JSON özelliği. **DecisionTreeResultBlob** çıkış olarak Web hizmeti tarafından kullanılarak geçirilir **webServiceOutputs** JSON özelliği.  

> [!IMPORTANT]
> Web hizmeti birden fazla girdi aldığı durumlarda kullanmak **webServiceInputs** kullanmak yerine özelliği **WebServiceInput etkinliğine**. Bkz: [Web hizmeti birden çok girişi gerektiren](#web-service-requires-multiple-inputs) webServiceInputs özelliğini kullanarak bir örnek için bölüm.
>
> Tarafından başvurulan veri kümeleri **WebServiceInput etkinliğine**/**webServiceInputs** ve **webServiceOutputs** özellikleri (içinde  **typeProperties**) de dahil etkinliğin **girişleri** ve **çıkarır**.
>
> Azure ML denemenizde web hizmeti giriş ve çıkış bağlantı noktaları ve genel parametreleri özelleştirebileceğiniz varsayılan adları ("input1", "input2") sahip. WebServiceInputs, webServiceOutputs ve globalParameters ayarları için kullandığınız adlarının denemeler adlarında tam olarak eşleşmelidir. Örnek istek yükü, beklenen eşleme doğrulamak Azure ML uç noktanız için toplu iş yürütme Yardım sayfasında görüntüleyebilirsiniz.
>
>

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchExecution",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "DecisionTreeInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "DecisionTreeResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "typeProperties":
        {
            "webServiceInput": "DecisionTreeInputBlob",
            "webServiceOutputs": {
                "output1": "DecisionTreeResultBlob"
            }                
        },
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```
> [!NOTE]
> Yalnızca girişleri ve çıkışları AzureMLBatchExecution etkinliğin Web hizmeti parametreleri olarak geçirilebilir. Örneğin, yukarıdaki JSON parçacığında, bir giriş WebServiceInput etkinliğine parametresi Web hizmeti için bir girdi olarak geçirilen AzureMLBatchExecution etkinliğine DecisionTreeInputBlob olabilir.   
>
>

### <a name="example"></a>Örnek
Bu örnek, girdi ve çıktı verilerini saklamak için Azure Storage kullanır.

Gitmenizi öneririz [Data Factory ile ilk işlem hattınızı oluşturma] [ adf-build-1st-pipeline] Bu örnek geçmeden önce Öğreticisi. Bu örnekte, veri fabrikası yapıları (bağlı hizmetler, veri kümelerini, ardışık düzen) oluşturmak için Data Factory düzenleyici kullanın.   

1. Oluşturma bir **bağlantılı hizmeti** için **Azure Storage**. Girdi ve çıktı dosyası farklı depolama hesapları yoksa, iki bağlı hizmet gerekir. Bir JSON örneği aşağıdadır:

    ```JSON
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
        }
      }
    }
    ```
2. Oluşturma **giriş** Azure Data Factory **dataset**. Bazı diğer Data Factory veri kümeleri farklı olarak, bu veri kümeleri her ikisini de içermelidir **folderPath** ve **fileName** değerleri. İşlem veya benzersiz giriş ve çıkış dosyaları her toplu iş yürütme (her veri dilimi) için bölümlendirme kullanabilirsiniz. Giriş CSV dosya biçimine dönüştürmek ve her dilim için depolama hesabındaki yerleştirmek için bazı Yukarı Akış etkinliği eklemeniz gerekebilir. Bu durumda, dahil **dış** ve **externalData** aşağıdaki örnekte ve, DecisionTreeInputBlob gösterilen ayarları çıkış veri kümesi farklı bir etkinliğe olacaktır.

    ```JSON
    {
      "name": "DecisionTreeInputBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/input",
          "fileName": "in.csv",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }
    ```

    Giriş csv dosyanızda sütun başlık satırı olmalıdır. Kullanıyorsanız **kopyalama etkinliği** csv blob depolama alanına oluşturun/taşıma için havuz özelliğini ayarlamalıdır **blobWriterAddHeader** için **doğru**. Örneğin:

    ```JSON
    sink:
    {
        "type": "BlobSink",     
        "blobWriterAddHeader": true
    }
    ```

    Csv dosyası üstbilgi satırındaki yoksa, aşağıdaki hatayı görebilirsiniz: **etkinliğinde hata: dize okunurken hata oluştu. Beklenmeyen bir belirteç: StartObject. Yol '', satır 1, 1, konum**.
3. Oluşturma **çıkış** Azure Data Factory **dataset**. Bu örnekte, her dilim yürütme için bir benzersiz çıkış yolu oluşturmak için bölümleme kullanır. Bölümleme olmadan, etkinliği dosyanın üzerine.

    ```JSON
    {
      "name": "DecisionTreeResultBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/scored/{folderpart}/",
          "fileName": "{filepart}result.csv",
          "partitionedBy": [
            {
              "name": "folderpart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyyMMdd"
              }
            },
            {
              "name": "filepart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HHmmss"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 15
        }
      }
    }
    ```
4. Oluşturma bir **bağlantılı hizmeti** türü: **AzureMLLinkedService**, API anahtarı sağlayarak ve toplu yürütme URL'si model.

    ```JSON
    {
      "name": "MyAzureMLLinkedService",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
          "mlEndpoint": "https://[batch execution endpoint]/jobs",
          "apiKey": "[apikey]"
        }
      }
    }
    ```
5. Son olarak, içeren bir ardışık düzen Yazar bir **AzureMLBatchExecution** etkinlik. Çalışma zamanında, ardışık düzen aşağıdaki adımları gerçekleştirir:

   1. Giriş dosyası konumunu giriş kümeleriniz alır.
   2. Azure Machine Learning toplu iş yürütme API çağırır
   3. Toplu iş yürütme çıktısını çıkış veri kümesinde bulunan verilen blob kopyalar.

      > [!NOTE]
      > AzureMLBatchExecution etkinlik sıfır veya daha fazla girişleri ve çıkışları bir veya daha fazla olabilir.
      >
      >

    ```JSON
    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [
            {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                {
                    "name": "DecisionTreeInputBlob"
                }
                ],
                "outputs": [
                {
                    "name": "DecisionTreeResultBlob"
                }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }
    ```

      Her ikisi de **Başlat** ve **son** tarih/saat olmalıdır [ISO biçiminde](http://en.wikipedia.org/wiki/ISO_8601). Örneğin: 2014-10-14T16:32:41Z. **Son** zaman isteğe bağlı. İçin değer belirtmezseniz, **son** özelliği olarak hesaplanır "**start + 48 hours.**" İşlem hattını süresiz olarak çalıştırmak için **end** özelliği değerini **9999-09-09** olarak ayarlayın. JSON özellikleri hakkında ayrıntılı bilgi için bkz. [JSON Betik Oluşturma Başvurusu](https://msdn.microsoft.com/library/dn835050.aspx).

      > [!NOTE]
      > AzureMLBatchExecution için giriş belirtme etkinliği isteğe bağlıdır.
      >
      >

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Senaryo: Denemelerini çeşitli depolarını verilerde başvurmak için Okuyucu/Yazıcı modüllerini kullanma
Azure ML denemeler oluştururken, başka bir yaygın bir senaryo, okuyucu ve yazıcı modülleri kullanmaktır. Okuyucu modülü bir deneme veri yüklemek için kullanılır ve yazıcı modülü denemelerinizden verileri kaydetmek için. Okuyucu ve yazıcı modüller hakkında daha fazla ayrıntı için bkz: [okuyucu](https://msdn.microsoft.com/library/azure/dn905997.aspx) ve [yazan](https://msdn.microsoft.com/library/azure/dn905984.aspx) MSDN Kitaplığı konularda.     

Okuyucu ve yazıcı modülleri kullanırken, bu Okuyucu/Yazıcı modülleri her bir özellik için bir Web hizmeti parametresi kullanmak iyi bir uygulamadır. Bu web parametreleri değerlerini çalışma zamanı sırasında yapılandırmanıza olanak sağlar. Örneğin, bir Azure SQL veritabanı kullanan bir okuyucu modülü ile bir deneme oluşturabilirsiniz: XXX.database.windows.net. Web hizmeti dağıtıldıktan sonra YYY.database.windows.net adlı başka bir Azure SQL Server belirtmek web hizmeti sağlamak istiyorsunuz. Yapılandırılması için bu değeri izin vermek için Web hizmeti parametresini kullanabilirsiniz.

> [!NOTE]
> Web hizmeti giriş ve çıkış Web hizmeti parametrelerinden farklıdır. İlk senaryoda, bir giriş ve çıkış için bir Azure ML Web hizmeti nasıl belirtilebilir gördünüz. Bu senaryoda, Okuyucu/Yazıcı modülleri özelliklerine karşılık gelen bir Web hizmeti parametreleri geçirin.
>
>

Web hizmeti parametreleri kullanarak bir senaryo bakalım. Verileri Azure Machine Learning tarafından desteklenen veri kaynaklarından biri okunacak okuyucu modülü kullanan bir dağıtılan Azure Machine Learning web hizmetine sahip (örneğin: Azure SQL veritabanı). Toplu yürütme işlemi yapıldıktan sonra sonuçları yazıcı Modülü (Azure SQL veritabanı) kullanılarak yazılır.  Hiçbir web hizmeti girişleri ve çıkışları denemeler tanımlanır. Bu durumda, okuyucu ve yazıcı modülleri için ilgili web hizmeti parametreleri yapılandırmanızı öneririz. Bu yapılandırma Okuyucu/Yazıcı AzureMLBatchExecution etkinlik kullanırken yapılandırılması için modül sağlar. Web hizmeti parametreleri belirtin **globalParameters** JSON etkinliğinde gibi bölüm.

```JSON
"typeProperties": {
    "globalParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```

Aynı zamanda [veri fabrikası işlevleri](data-factory-functions-variables.md) Web değerleri geçirme içinde parametreleri aşağıdaki örnekte gösterildiği gibi hizmet:

```JSON
"typeProperties": {
    "globalParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> Web hizmeti parametreleri büyük/küçük harfe duyarlıdır, bu nedenle etkinliğin belirttiğiniz adları JSON Web hizmeti tarafından sunulan olanları eşleşmesini.
>
>

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a>Verileri Azure Blob içinde birden çok dosya okuma için bir okuyucu modülü kullanma
Pig gibi etkinlikleri ile büyük veri ardışık düzenleri ve Hive bir oluşturabilir veya hiçbir uzantı ile daha fazla çıkış dosyaları. Örneğin, bir dış Hive tablo belirttiğinizde, dış Hive tablosu için veri aşağıdaki adı 000000_0 ile Azure blob storage depolanabilir. Birden çok dosya okumak için bir deneme okuyucu modülü kullanın ve bunları tahminleri için kullanın.

Okuyucu modülü bir Azure Machine Learning deneme kullanırken, Azure Blob girdi olarak belirtebilirsiniz. Azure blob depolama alanındaki dosyalar çıktı dosyaları olabilir (örnek: 000000_0) Hdınsight üzerinde çalışan bir Pig ve Hive komut dosyası tarafından üretilen. Okuyucu modülü yapılandırarak (hiçbir uzantılı) dosyaları okumasına izin verir **kapsayıcısına yol dizin/blob**. **Kapsayıcı yoluna** işaret kapsayıcıya ve **dizin/blob** aşağıdaki görüntüde gösterildiği gibi dosyaları içeren klasör işaret eder. Yıldız işareti diğer bir deyişle, \*) **belirtir kapsayıcı/klasöründeki tüm dosyalar (diğer bir deyişle, aggregateddata/data/yıl ay/2014-6 = /\*)** deneme bir parçası olarak okuyun.

![Azure Blob özellikleri](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)

### <a name="example"></a>Örnek
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Web hizmeti parametreleri ile AzureMLBatchExecution aktivitesiyle kanalı

```JSON
{
  "name": "MLWithSqlReaderSqlWriter",
  "properties": {
    "description": "Azure ML model with sql azure reader/writer",
    "activities": [
      {
        "name": "MLSqlReaderSqlWriterActivity",
        "type": "AzureMLBatchExecution",
        "description": "test",
        "inputs": [
          {
            "name": "MLSqlInput"
          }
        ],
        "outputs": [
          {
            "name": "MLSqlOutput"
          }
        ],
        "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
        "typeProperties":
        {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
                "output1": "MLSqlOutput"
            }
              "globalParameters": {
                "Database server name": "<myserver>.database.windows.net",
                "Database name": "<database>",
                "Server user account name": "<user name>",
                "Server user account password": "<password>"
              }              
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        },
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

Yukarıdaki JSON örnekte:

* Dağıtılan Azure Machine Learning Web hizmeti, başlangıç/bitiş bir Azure SQL veritabanı veri okuma/yazma için bir okuyucu ve yazıcı modülü kullanır. Aşağıdaki dört parametre bu Web hizmetini sunar: veritabanı sunucu adı, veritabanı adı, sunucu kullanıcı hesabı adı ve sunucu kullanıcı hesabı parolası.  
* Her ikisi de **Başlat** ve **son** tarih/saat olmalıdır [ISO biçiminde](http://en.wikipedia.org/wiki/ISO_8601). Örneğin: 2014-10-14T16:32:41Z. **Son** zaman isteğe bağlı. İçin değer belirtmezseniz, **son** özelliği olarak hesaplanır "**start + 48 hours.**" İşlem hattını süresiz olarak çalıştırmak için **end** özelliği değerini **9999-09-09** olarak ayarlayın. JSON özellikleri hakkında ayrıntılı bilgi için bkz. [JSON Betik Oluşturma Başvurusu](https://msdn.microsoft.com/library/dn835050.aspx).

### <a name="other-scenarios"></a>Diğer senaryolar
#### <a name="web-service-requires-multiple-inputs"></a>Web hizmeti birden çok giriş gerektiriyor
Web hizmeti birden fazla girdi aldığı durumlarda kullanmak **webServiceInputs** kullanmak yerine özelliği **WebServiceInput etkinliğine**. Tarafından başvurulan veri kümeleri **webServiceInputs** de dahil etkinliğin **girişleri**.

Azure ML denemenizde web hizmeti giriş ve çıkış bağlantı noktaları ve genel parametreleri özelleştirebileceğiniz varsayılan adları ("input1", "input2") sahip. WebServiceInputs, webServiceOutputs ve globalParameters ayarları için kullandığınız adlarının denemeler adlarında tam olarak eşleşmelidir. Örnek istek yükü, beklenen eşleme doğrulamak Azure ML uç noktanız için toplu iş yürütme Yardım sayfasında görüntüleyebilirsiniz.

```JSON
{
    "name": "PredictivePipeline",
    "properties": {
        "description": "use AzureML model",
        "activities": [{
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [{
                "name": "inputDataset1"
            }, {
                "name": "inputDataset2"
            }],
            "outputs": [{
                "name": "outputDataset"
            }],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties": {
                "webServiceInputs": {
                    "input1": "inputDataset1",
                    "input2": "inputDataset2"
                },
                "webServiceOutputs": {
                    "output1": "outputDataset"
                }
            },
            "policy": {
                "concurrency": 3,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "02:00:00"
            }
        }],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
    }
}
```

#### <a name="web-service-does-not-require-an-input"></a>Web hizmeti bir giriş gerektirmez
Azure ML toplu iş yürütme web Hizmetleri, tüm girişleri gerektirmeyebilir örnek R veya Python komut dosyası için tüm iş akışlarını çalıştırmak için kullanılabilir. Ya da deneme herhangi GlobalParameters kullanıma sunmuyor okuyucu modülü ile yapılandırılabilir. Bu durumda, AzureMLBatchExecution etkinlik şu şekilde yapılandırılması:

```JSON
{
    "name": "scoring service",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "myBlob"
        }
    ],
    "typeProperties": {
        "webServiceOutputs": {
            "output1": "myBlob"
        }              
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-does-not-require-an-inputoutput"></a>Web hizmeti bir giriş/çıkış gerektirmez
Azure ML toplu iş yürütme web hizmeti yapılandırılmış herhangi bir Web hizmeti çıktı sahip olmayabilir. Bu örnekte, Web hizmeti girişi veya çıkışı yok veya tüm GlobalParameters yapılandırılır. Hala faaliyete yapılandırılmış bir çıktı yoktur, ancak bir webServiceOutput verilmedi.

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "placeholderOutputDataset"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a>Web hizmeti kullanan okuyucular ve yazarlar ve diğer etkinlikler yalnızca başarılı olduğunda etkinlik çalışması
Azure ML web hizmeti okuyucu ve yazıcı modülleri ile veya olmadan herhangi GlobalParameters çalıştırmak için yapılandırılabilir. Ancak, yalnızca bir Yukarı Akış işlem tamamlandığında hizmetini çağırmak için veri kümesi bağımlılıkları kullanan bir ardışık düzen hizmeti çağrıları katıştırmak isteyebilirsiniz. Bu yaklaşımı kullanarak toplu iş yürütme tamamlandıktan sonra başka bir eylemi de tetikleyebilirsiniz. Bu durumda, etkinlik girişleri ve çıkışları, bunlardan herhangi birinin Web hizmeti girişleri veya çıkışları adlandırma olmadan kullanarak bağımlılıkları hızlı.

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "inputs": [
        {
            "name": "upstreamData1"
        },
        {
            "name": "upstreamData2"
        }
    ],
    "outputs": [
        {
            "name": "downstreamData"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

**Paketler** şunlardır:

* Deneme uç noktanızı bir WebServiceInput etkinliğine kullanıyorsa: blob veri kümesi temsil edilir ve etkinlik girişlerinde ve WebServiceInput etkinliğine özelliği dahil edilmiştir. Aksi takdirde WebServiceInput etkinliğine özelliği atlanmıştır.
* Deneme uç noktanızı webServiceOutput(s) kullanıyorsa: blob veri kümeleri ile temsil edilir ve etkinlik çıkışları ve webServiceOutputs özelliğinde dahil edilir. Etkinlik çıkarır ve webServiceOutputs her çıktı denemede ada göre eşleştirilir. Aksi takdirde, webServiceOutputs özelliği atlanmıştır.
* GlobalParameter(s), deneme uç noktasını kullanıma sunar, bunlar etkinlik globalParameters özelliğinde anahtar, değer çiftleri olarak verilir. Aksi takdirde, globalParameters özelliği atlanmıştır. Anahtarlar büyük/küçük harfe duyarlıdır. [Azure Data Factory işlevleri](data-factory-functions-variables.md) değerleri kullanılabilir.
* Ek veri kümeleri içinde etkinlik typeProperties başvurulan olmadan etkinlik girişleri ve çıkışları özelliklerinde eklenebilir. Bu veri kümeleri dilim bağımlılıkları kullanılarak yürütme yöneten ancak aksi AzureMLBatchExecution etkinlik tarafından göz ardı edilir.


## <a name="updating-models-using-update-resource-activity"></a>Kaynak güncelleştirme etkinliği kullanarak modelleri güncelleştiriliyor
Yeniden eğitme ile tamamladıktan sonra Puanlama web hizmeti (web hizmeti olarak sunulan Tahmine dayalı denemeye) ile yeni eğitilen modelini kullanarak güncelleştirme **Azure ML güncelleştirme kaynak etkinliği**. Bkz: [kaynak güncelleştirme etkinliği kullanarak modelleri güncelleştirme](data-factory-azure-ml-update-resource-activity.md) Ayrıntılar için makale.

### <a name="reader-and-writer-modules"></a>Okuyucu ve yazıcı modülleri
Web hizmeti parametreleri kullanarak için yaygın bir senaryo Azure SQL okuyucuları ve yazıcıları kullanılmasıdır. Okuyucu modülü bir denemeyi Azure Machine Learning Studio dışında veri management hizmetlerinden veri yüklemek için kullanılır. Veri Yönetimi Hizmetleri Azure Machine Learning Studio dışında içine denemelerinizden verileri kaydetmek için yazıcı modülüdür.  

Azure Blob/Azure SQL Okuyucu/Yazıcı hakkında daha fazla ayrıntı için bkz: [okuyucu](https://msdn.microsoft.com/library/azure/dn905997.aspx) ve [yazan](https://msdn.microsoft.com/library/azure/dn905984.aspx) MSDN Kitaplığı konularda. Önceki bölümdeki örnek, Azure Blob okuyucu ve Azure Blob yazıcı kullanılır. Bu bölümde, Azure SQL okuyucu ve Azure SQL yazıcı kullanarak anlatılmaktadır.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
**S:** my büyük veri ardışık düzen tarafından üretilen birden çok dosya vardır. Tüm dosyalar üzerinde çalışmak için AzureMLBatchExecution etkinliği kullanabilir miyim?

**Y:** Evet. Bkz: **verileri Azure Blob içinde birden çok dosya okuma için bir okuyucu modülü kullanılarak** ayrıntıları bölümü.

## <a name="azure-ml-batch-scoring-activity"></a>Azure ML toplu iş Puanlama etkinliği
Kullanıyorsanız **AzureMLBatchScoring** Azure Machine Learning ile tümleştirmek için etkinlik en son kullanmanızı öneririz **AzureMLBatchExecution** etkinlik.

AzureMLBatchExecution etkinlik sunulan Azure PowerShell ve Azure SDK'sını Ağustos 2015 sürümünde.

AzureMLBatchScoring etkinlik kullanmaya devam etmek istiyorsanız, bu bölümde okuma devam edin.  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Giriş/çıkış için Azure Storage kullanarak azure ML toplu Puanlama etkinliği

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchScoring",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "ScoringInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "ScoringResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

### <a name="web-service-parameters"></a>Web hizmeti parametreleri
Web hizmeti parametreleri için değerleri belirlemek için ekleyin bir **typeProperties** için bölüm **AzureMLBatchScoringActivty** aşağıdaki örnekte gösterildiği gibi JSON ardışık düzeninde bölümünde:

```JSON
"typeProperties": {
    "webServiceParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```
Aynı zamanda [veri fabrikası işlevleri](data-factory-functions-variables.md) Web değerleri geçirme içinde parametreleri aşağıdaki örnekte gösterildiği gibi hizmet:

```JSON
"typeProperties": {
    "webServiceParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> Web hizmeti parametreleri büyük/küçük harfe duyarlıdır, bu nedenle etkinliğin belirttiğiniz adları JSON Web hizmeti tarafından sunulan olanları eşleşmesini.
>
>

## <a name="see-also"></a>Ayrıca Bkz.
* [Azure blog gönderisi: Azure Machine Learning ve Azure Data Factory ile çalışmaya başlama](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)

[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/
