---
title: Azure Data Factory kullanarak Tahmine dayalı veri işlem hatları oluşturun | Microsoft Docs
description: Nasıl oluşturulacağını açıklar Azure Data Factory ve Azure Machine Learning kullanarak öngörülebilir komut zincirleri oluşturma
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 4fad8445-4e96-4ce0-aa23-9b88e5ec1965
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 4093febd19d71512e3c80704e88f9d5cf669d7d9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60567440"
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a>Azure Machine Learning ve Azure Data Factory kullanarak öngörülebilir komut zincirleri oluşturma

> [!div class="op_single_selector" title1="Dönüştürme etkinlikleri"]
> * [Hive etkinliği](data-factory-hive-activity.md)
> * [Pig etkinliği](data-factory-pig-activity.md)
> * [MapReduce etkinliği](data-factory-map-reduce.md)
> * [Hadoop akış etkinliğinde](data-factory-hadoop-streaming-activity.md)
> * [Spark etkinliği](data-factory-spark.md)
> * [Machine Learning Batch Yürütme Etkinliği](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Kaynak Güncelleştirme Etkinliği](data-factory-azure-ml-update-resource-activity.md)
> * [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL Etkinliği](data-factory-usql-activity.md)
> * [.NET özel etkinliği](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Giriş
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [Data Factory'de machine learning kullanarak verileri dönüştürme](../transform-data-using-machine-learning.md).


### <a name="azure-machine-learning"></a>Azure Machine Learning
[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) derleme, test ve Tahmine dayalı analiz çözümlerini dağıtmanıza olanak sağlar. Bir üst düzey açısından bakıldığında, bu üç adımda gerçekleştirilir:

1. **Eğitim denemesini oluşturma**. Azure Machine Learning Studio'yu kullanarak, bu adımı uygulayın. Azure Machine Learning studio, eğitim ve eğitim verilerini kullanarak bir Tahmine dayalı analiz modeli test etmek için kullandığınız bir görsel işbirliğine dayalı geliştirme ortamıdır.
2. **Öngörücü bir denemeye dönüştürme**. Modelinizi mevcut verilerle eğitim almış ve yeni verileri puanlamak için kullanıma hazır sonra hazırlama ve puanlama için deneyiminizi kolaylaştırın.
3. **Bir web hizmeti olarak dağıtalım**. Bir Azure web hizmeti olarak Puanlama denemenizi yayımlayabilirsiniz. Bu web hizmeti uç noktası aracılığıyla modelinizi veri göndermek ve sonuç Öngörüler model Excel'den alırsınız.

### <a name="azure-data-factory"></a>Azure Data Factory
Data Factory, verilerin **taşınmasını** ve **dönüştürülmesini** düzenleyen ve otomatikleştiren bulut tabanlı bir veri tümleştirme hizmetidir. Çeşitli veri depolarından veri alabilen, dönüştürebilen / veri ve sonuç verilerini veri depolarında yayımlayabilen Azure Data Factory kullanarak veri tümleştirme çözümleri oluşturabilirsiniz.

Data Factory hizmeti verileri taşıyıp dönüştüren ve ardından işlem hattını belirli bir zamanlamaya (saatlik, günlük, haftalık, vb.) göre çalıştıran veri işlem hatları oluşturmanıza imkan tanır. Ayrıca, veri işlem hatlarınız arasındaki çizgileri ve bağımlılıkları gösteren ve sorunları kolayca saptamak ve izleme uyarılarını ayarlamak üzere tek bir birleşik görünümden tüm veri işlem hatlarınızı izleyen zengin görsel öğeler sağlar.

Bkz: [Azure Data Factory'ye giriş](data-factory-introduction.md) ve [ilk işlem hattınızı oluşturma](data-factory-build-your-first-pipeline.md) makaleler Azure Data Factory hizmetiyle hızlıca başlamak için.

### <a name="data-factory-and-machine-learning-together"></a>Data Factory ve Machine Learning ile birlikte
Azure Data Factory bir yayımlanan kullanan işlem hatları kolayca oluşturmanıza olanak sağlar [Azure Machine Learning] [ azure-machine-learning] web için Tahmine dayalı analiz hizmetidir. Kullanarak **Batch yürütme etkinliği** toplu verilerde tahmin yapmayı sağlayan bir Azure Machine Learning studio web hizmeti bir Azure Data Factory işlem hattında çağırabilirsiniz. Ayrıntılar için Batch yürütme etkinliği bölümünü kullanarak bir Azure Machine Learning studio web hizmeti çağırma bakın.

Zaman içinde yeni bir giriş veri kümeleri kullanarak eğitilebileceği denemeleri Puanlama Azure Machine Learning Studio'da Tahmine dayalı modelleri gerekir. Aşağıdaki adımları uygulayarak bir Data Factory işlem hattı Azure Machine Learning studio modelden yeniden eğitebilir:

1. Eğitim denemesini (değil Tahmine dayalı denemeye) bir web hizmeti olarak yayımlayın. Tahmine dayalı denemeye önceki senaryoda bir web hizmeti olarak kullanıma sunmak için yaptığınız gibi Azure Machine Learning Studio'da bu adımı uygulayın.
2. Batch yürütme etkinliği Azure Machine Learning studio web hizmeti için eğitim denemesini çağırmak için kullanın. Temel olarak, hem eğitim web hizmeti hem de Puanlama web hizmeti çağırmak için Azure Machine Learning studio Batch yürütme etkinliği kullanabilirsiniz.

Yeniden eğitme ile işiniz bittiğinde, Puanlama web hizmeti (bir web hizmeti olarak kullanıma sunulan Tahmine dayalı denemeye) ile yeni eğitim modeli kullanarak güncelleştirme **Azure Machine Learning studio güncelleştirmek kaynak etkinliği**. Bkz: [kaynak güncelleştirme etkinliği kullanarak modelleri güncelleştirme](data-factory-azure-ml-update-resource-activity.md) makale Ayrıntılar için.

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Batch yürütme etkinliği kullanan bir web hizmetini çağırmak
Veri taşıma ve işleme düzenlemek için Azure veri fabrikasını kullanın ve sonra da kullanarak Azure Machine Learning batch yürütme gerçekleştirin. Üst düzey adımlar şunlardır:

1. Bir Azure Machine Learning bağlı hizmeti oluşturursunuz. Aşağıdaki değerleri ihtiyacınız vardır:

   1. **İstek URI'si** toplu yürütme API. İstek URI'si tıklayarak bulabilirsiniz **toplu iş yürütme** web Hizmetleri sayfasında bağlantı.
   2. **API anahtarı** için yayımlanan Azure Machine Learning web hizmeti. API anahtarı, yayımlanan web hizmeti tıklayarak bulabilirsiniz.
   3. Kullanım **AzureMLBatchExecution** etkinlik.

      ![Machine Learning Panosu](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

      ![Batch URI'si](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)

### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Senaryo: Web hizmeti girdiler/Azure Blob depolama alanındaki verilere başvuran çıktılar kullanarak denemeler
Bu senaryoda, Azure Machine Learning Web hizmeti bir Azure blob depolamadaki bir dosyadan verileri kullanarak Öngörüler sağlar ve blob depolama alanında tahmin sonuçlarını depolar. Aşağıdaki JSON ile AzureMLBatchExecution etkinliği bir Data Factory işlem hattı tanımlar. Veri kümesi etkinliğinde **DecisionTreeInputBlob** giriş olarak ve **DecisionTreeResultBlob** çıktı olarak. **DecisionTreeInputBlob** kullanılarak geçirilen bir web hizmeti tarafından giriş olarak **WebServiceInput** JSON özelliği. **DecisionTreeResultBlob** çıkış olarak Web hizmeti tarafından kullanılarak geçirilir **webServiceOutputs** JSON özelliği.

> [!IMPORTANT]
> Web hizmetini birden fazla giriş aldığı durumlarda kullanmak **Webserviceınputs** kullanmak yerine özellik **WebServiceInput**. Bkz: [Web hizmeti, birden çok giriş gerektirir](#web-service-requires-multiple-inputs) Webserviceınputs özelliğini kullanarak bir örnek için bölüm.
>
> Tarafından başvurulan veri kümeleri **WebServiceInput**/**Webserviceınputs** ve **webServiceOutputs** özellikleri (içinde  **typeProperties**) de dahil etkinliğinde **girişleri** ve **çıkarır**.
>
> Azure Machine Learning studio denemesi web hizmeti giriş ve çıkış bağlantı noktaları ve genel parametrelerini özelleştirebileceğiniz varsayılan adları ("input1", "input2") sahip. Webserviceınputs webServiceOutputs ve globalParameters ayarları için kullandığınız adları denemeleri adlarında tam olarak eşleşmelidir. Örnek istek yükü beklenen eşleme doğrulamak Azure Machine Learning studio uç noktanız için toplu işlem yürütme Yardım sayfasında görüntüleyebilirsiniz.
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
> Yalnızca giriş ve çıkışları AzureMLBatchExecution etkinliğin Web hizmetine parametre olarak geçirilebilir. Örneğin, yukarıdaki JSON parçacığında, bir Web hizmeti giriş olarak WebServiceInput parametresi geçirilen AzureMLBatchExecution etkinliği bir girdi DecisionTreeInputBlob olur.
>
>

### <a name="example"></a>Örnek
Bu örnek, girdi ve çıktı verilerini tutmak için Azure depolama kullanır.

Gitmenizi öneririz [Data Factory ile ilk işlem hattınızı oluşturma] [ adf-build-1st-pipeline] Bu örnek geçmeden önce öğretici. Bu örnekte (bağlı hizmetler, veri kümeleri, işlem hattı) Data Factory yapıtlarının oluşturmak için Data Factory Düzenleyicisi'ni kullanın.

1. Oluşturma bir **bağlı hizmet** için **Azure depolama**. Giriş ve çıkış dosyalarının farklı depolama hesaplarında, iki bağlı hizmet gerekir. Bir JSON örneği aşağıda verilmiştir:

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
2. Oluşturma **giriş** Azure Data Factory **veri kümesi**. Bazı diğer Data Factory veri kümeleri farklı olarak, bu veri kümeleri hem de içermelidir **folderPath** ve **fileName** değerleri. Her toplu iş yürütme (her veri dilimi) işlemek veya benzersiz giriş ve çıkış dosyaları neden bölümleme kullanabilirsiniz. CSV dosyası biçiminde giriş dönüştürme ve her dilim için depolama hesabındaki yerleştirme bazı Yukarı Akış etkinliği eklemeniz gerekebilir. Bu durumda, dahil **dış** ve **externalData** aşağıdaki örnek ve, DecisionTreeInputBlob gösterilen ayarları, farklı bir etkinliğin çıkış veri kümesi olur.

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

    Sütun üst bilgi satırı giriş csv dosyanız olmalıdır. Kullanıyorsanız **kopyalama etkinliği** csv blob depolama alanına oluşturma/taşıma için havuz özelliği ayarlayın **blobWriterAddHeader** için **true**. Örneğin:

    ```JSON
    sink:
    {
        "type": "BlobSink",
        "blobWriterAddHeader": true
    }
    ```

    Csv dosyası üst bilgi satırı sahip değilse, aşağıdaki hatayı görebilirsiniz: **Etkinlikte hata: Hata okuma dizesi. Beklenmeyen belirteç: StartObject. Yol '', satır 1, 1 konumlandırma**.
3. Oluşturma **çıkış** Azure Data Factory **veri kümesi**. Bu örnek, her bir dilim yürütme için bir benzersiz çıkış yolu oluşturmak için bölümleme kullanır. Etkinlik, bölümleme olmadan, dosyanın üzerine yazacak.

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
4. Oluşturma bir **bağlı hizmet** türü: **AzureMLLinkedService**, API anahtarı sağlayan ve toplu yürütme URL'si model.

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
5. Son olarak, bir işlem hattı içeren Yazar bir **AzureMLBatchExecution** etkinlik. Çalışma zamanında işlem hattı, aşağıdaki adımları gerçekleştirir:

   1. Giriş veri kümelerinizi giriş dosyasının konumunu alır.
   2. Azure Machine Learning batch yürütme API çağırır
   3. Toplu işlem yürütme çıktısı, çıktı veri kümesi içinde belirtilen blob kopyalar.

      > [!NOTE]
      > AzureMLBatchExecution etkinliğin sıfır veya daha fazla giriş ve çıkışları bir veya daha fazla olabilir.
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

      Her ikisi de **Başlat** ve **son** tarih/saat olmalıdır [ISO biçimi](https://en.wikipedia.org/wiki/ISO_8601). Örneğin: 2014-10-14T16:32:41Z. **Son** zaman isteğe bağlıdır. İçin değer belirtmezseniz **son** özelliği olarak hesaplanır "**start + 48 hours.** " İşlem hattını süresiz olarak çalıştırmak için **end** özelliği değerini **9999-09-09** olarak ayarlayın. JSON özellikleri hakkında ayrıntılı bilgi için bkz. [JSON Betik Oluşturma Başvurusu](https://msdn.microsoft.com/library/dn835050.aspx).

      > [!NOTE]
      > AzureMLBatchExecution giriş belirterek etkinliği isteğe bağlıdır.
      >
      >

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Senaryo: Çeşitli depoları verilerde başvurmak için Okuyucu/Yazıcı modülleri'ni kullanarak deneme
Azure Machine Learning studio denemeleri oluştururken bir diğer yaygın senaryo, okuyucu ve yazıcı modülleri kullanmaktır. Okuyucu modülü bir denemenin verileri yüklemek için kullanılır ve yazıcı modülü denemelerinizden veri kaydetmektir. Okuyucu ve yazıcı modüller hakkında daha fazla ayrıntı için bkz: [okuyucu](https://msdn.microsoft.com/library/azure/dn905997.aspx) ve [yazıcı](https://msdn.microsoft.com/library/azure/dn905984.aspx) konularıyla ilgili MSDN Kitaplığı.

Okuyucu ve yazıcı modülleri kullanırken, bu Okuyucu/Yazıcı modülleri her bir özellik için bir Web hizmeti parametresi kullanmak iyi bir uygulamadır. Bu web parametreleri çalışma zamanı sırasında değerleri yapılandırmanıza olanak sağlar. Örneğin, bir Azure SQL veritabanı kullanan bir okuyucu modülü ile bir deneme oluşturabilirsiniz: XXX.database.windows.net. Web hizmeti dağıtıldıktan sonra web hizmeti tüketicilerinin YYY.database.windows.net adlı başka bir Azure SQL Server'ı belirtmek etkinleştirmek istediğiniz. Bir Web hizmeti parametresi yapılandırılması için bu değeri izin vermek için kullanabilirsiniz.

> [!NOTE]
> Web hizmeti giriş ve çıkış, Web hizmeti parametreleri farklıdır. Bu senaryoda, bir giriş ve çıkış bir Azure Machine Learning studio Web hizmeti için nasıl belirtilebilir gördünüz. Bu senaryoda, Okuyucu/Yazıcı modülleri özelliklerine karşılık gelen bir Web hizmeti parametrelerini geçirin.
>
>

Web hizmeti parametrelerini kullanarak bir senaryo bakalım. Bir Azure Machine Learning tarafından desteklenen veri kaynakları verileri okumak için bir okuyucu modülü kullanan dağıtılmış bir Azure Machine Learning web hizmeti sahip (örneğin: Azure SQL veritabanı). Toplu yürütme işlemi gerçekleştirildikten sonra sonuçları bir yazıcı Modülü (Azure SQL veritabanı) kullanılarak yazılır.  Hiçbir web hizmeti giriş ve çıkışları denemeleri içinde tanımlanır. Bu durumda, okuyucu ve yazıcı modüller için ilgili web hizmeti parametreleri yapılandırmanızı öneririz. Bu yapılandırma Okuyucu/Yazıcı AzureMLBatchExecution etkinlik kullanırken yapılandırılması için modül sağlar. Web hizmeti parametreleri olarak belirttiğiniz **globalParameters** JSON etkinliğinde gibi bölümü.

```JSON
"typeProperties": {
    "globalParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```

Ayrıca [Data Factory işlevleri](data-factory-functions-variables.md) Web değerler geçirerek parametreleri aşağıdaki örnekte gösterildiği gibi hizmet:

```JSON
"typeProperties": {
    "globalParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> Web hizmeti parametreleri büyük/küçük harfe duyarlıdır, dolayısıyla etkinliğinde belirttiğiniz adları JSON Web hizmeti tarafından kullanıma sunulan olanları eşleşmesini.
>
>

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a>Azure Blob içinde birden çok dosyadan veri okumak için bir okuyucu modülü kullanma
Pig gibi etkinlikler ile büyük veri işlem hatları ve Hive birini oluşturabilir veya daha fazla çıkış dosyalarını bir uzantı yok. Örneğin, dış bir Hive tablosu belirttiğinizde, dış Hive tablosu için verileri aşağıdaki adı 000000_0 ile Azure blob depolama alanında depolanabilir. Birden çok dosyayı okumak için bir deneme okuyucu modülü kullanın ve bunları tahminler elde etmek için kullanın.

Okuyucu modülü kullanarak bir Azure Machine Learning deneme, Azure Blob girdi olarak belirtebilirsiniz. Çıkış dosyalarını Azure blob depolamada dosyalar olabilir (örnek: 000000_0) HDInsight üzerinde çalışan bir Pig ve Hive betiği tarafından oluşturulur. Okuyucu Modülü (hiçbir uzantılı) dosyaları yapılandırarak okumanıza izin verir **kapsayıcı yolu dizin/blob**. **Kapsayıcı yolu** kapsayıcıya işaret ve **dizin/blob** aşağıdaki görüntüde gösterildiği gibi dosyaları içeren klasör işaret eder. Yıldız işareti diğer bir deyişle, \*) **belirten kapsayıcı/klasöründeki tüm dosyalar (diğer bir deyişle, aggregateddata/data/yıl ay/2014-6 = /\*)** denemeyi bir parçası olarak okunur.

![Azure Blob özellikleri](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)

### <a name="example"></a>Örnek
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>Web hizmeti parametreleri ile AzureMLBatchExecution etkinliği ile işlem hattı

```JSON
{
  "name": "MLWithSqlReaderSqlWriter",
  "properties": {
    "description": "Azure Machine Learning studio model with sql azure reader/writer",
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

Yukarıdaki JSON örneği:

* Dağıtılan Azure Machine Learning Web hizmetini Azure SQL veritabanı ' / için veri okuma/yazma için bir okuyucu ve yazıcı modülü kullanır. Bu Web hizmetini aşağıdaki dört parametre sunar:  Veritabanı sunucusu adı, veritabanı adı, sunucu kullanıcı hesabı adını ve Server kullanıcı hesabı parolası.
* Her ikisi de **Başlat** ve **son** tarih/saat olmalıdır [ISO biçimi](https://en.wikipedia.org/wiki/ISO_8601). Örneğin: 2014-10-14T16:32:41Z. **Son** zaman isteğe bağlıdır. İçin değer belirtmezseniz **son** özelliği olarak hesaplanır "**start + 48 hours.** " İşlem hattını süresiz olarak çalıştırmak için **end** özelliği değerini **9999-09-09** olarak ayarlayın. JSON özellikleri hakkında ayrıntılı bilgi için bkz. [JSON Betik Oluşturma Başvurusu](https://msdn.microsoft.com/library/dn835050.aspx).

### <a name="other-scenarios"></a>Diğer senaryolar
#### <a name="web-service-requires-multiple-inputs"></a>Web hizmeti birden çok giriş gerektiriyor
Web hizmetini birden fazla giriş aldığı durumlarda kullanmak **Webserviceınputs** kullanmak yerine özellik **WebServiceInput**. Tarafından başvurulan veri kümeleri **Webserviceınputs** etkinliğinde eklenmelidir **girişleri**.

Azure Machine Learning studio denemesi web hizmeti giriş ve çıkış bağlantı noktaları ve genel parametrelerini özelleştirebileceğiniz varsayılan adları ("input1", "input2") sahip. Webserviceınputs webServiceOutputs ve globalParameters ayarları için kullandığınız adları denemeleri adlarında tam olarak eşleşmelidir. Örnek istek yükü beklenen eşleme doğrulamak Azure Machine Learning studio uç noktanız için toplu işlem yürütme Yardım sayfasında görüntüleyebilirsiniz.

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

#### <a name="web-service-does-not-require-an-input"></a>Web hizmeti giriş gerektirmez
Azure Machine Learning studio toplu yürütme web Hizmetleri, hiç giriş gerektirmeyecek örnek R veya Python komut dosyası için tüm iş akışlarını çalıştırmak için kullanılabilir. Veya denemeyi hiçbir GlobalParameters kullanıma sunmuyor okuyucu modülü ile yapılandırılmış olabilir. Bu durumda, AzureMLBatchExecution etkinlik aşağıdaki gibi yapılandırılır:

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

#### <a name="web-service-does-not-require-an-inputoutput"></a>Web hizmeti giriş/çıkış gerektirmez
Azure Machine Learning studio toplu yürütme web hizmeti, yapılandırılmış herhangi bir Web hizmeti çıktı olmayabilir. Bu örnekte, Web hizmeti giriş veya çıkış yok ya da tüm GlobalParameters yapılandırılır. Etkinlik üzerinde yapılandırılmış bir çıktı hala yoktur ancak bir webServiceOutput verilmedi.

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

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a>Web hizmeti kullandığı okuyucular ve Yazıcılar ve diğer etkinlikler yalnızca başarılı olduğunda, etkinlik çalıştırmaları
Azure Machine Learning studio web hizmeti okuyucu ve yazıcı modülleri ile veya olmadan tüm GlobalParameters çalıştırmak için yapılandırılmış olabilir. Ancak, bazı Yukarı Akış işleme tamamlandığında hizmetini çağırmak için veri kümesi bağımlılıkları kullanan bir işlem hattında hizmet çağrıları eklemek isteyebilirsiniz. Bu yaklaşımı kullanarak batch yürütme tamamlandığında da başka bir eylem tetikleyebilirsiniz. Bu durumda, bunların hiçbirine Web hizmeti giriş veya çıkış olarak adlandırma olmadan etkinlik girişler ve çıkışlar,'ı kullanarak bağımlılıkları ifade edebilirsiniz.

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

* Deneme bitiş bir WebServiceInput kullanıyorsa: bir blob veri kümesi tarafından temsil edilir ve etkinlik girişlerinde ve WebServiceInput özelliği de yer almaktadır. Aksi takdirde, hem WebServiceInput özelliği atlanmış.
* Deneme bitiş webServiceOutput(s) kullanıyorsa: blob veri kümeleri tarafından temsil edilir ve etkinlik çıktıları ve webServiceOutputs özelliğinde dahil edilir. Etkinlik çıkışı yapar ve webServiceOutputs denemede her çıkış adına göre eşleştirilir. Aksi takdirde, webServiceOutputs özelliği atlanmış.
* Deneme bitiş globalParameter(s) sunarsa, etkinlik globalParameters özelliğinde anahtar, değer çiftleri olarak verilir. Aksi takdirde, globalParameters özelliği atlanmış. Anahtarlar büyük/küçük harfe duyarlıdır. [Azure Data Factory işlevleri](data-factory-functions-variables.md) değerleri kullanılabilir.
* Ek veri kümeleri etkinlik typeProperties başvurulan olmadan etkinlik giriş ve çıkışları özelliklerinde eklenebilir. Bu veri kümelerini yürütme dilimi bağımlılıkları yönetir ancak Aksi halde AzureMLBatchExecution etkinlik tarafından göz ardı edilir.


## <a name="updating-models-using-update-resource-activity"></a>Kaynak güncelleştirme etkinliği kullanarak modelleri güncelleştiriliyor
Yeniden eğitme ile işiniz bittiğinde, Puanlama web hizmeti (bir web hizmeti olarak kullanıma sunulan Tahmine dayalı denemeye) ile yeni eğitim modeli kullanarak güncelleştirme **Azure Machine Learning studio güncelleştirmek kaynak etkinliği**. Bkz: [kaynak güncelleştirme etkinliği kullanarak modelleri güncelleştirme](data-factory-azure-ml-update-resource-activity.md) makale Ayrıntılar için.

### <a name="reader-and-writer-modules"></a>Okuyucu ve yazıcı modülleri
Azure SQL okuyucular ve yazıcılar kullanımını Web hizmeti parametrelerini kullanma yaygın bir senaryodur. Okuyucu modülü, veri yönetimi hizmetlerinden dışında Azure Machine Learning Studio'da bir denemeyi verileri yüklemek için kullanılır. Azure Machine Learning Studio dışında veri yönetim hizmetlerinin içine denemelerinizden verileri kaydetmek için yazıcı modülüdür.

Azure Blob/Azure SQL Okuyucu/Yazıcı hakkında daha fazla ayrıntı için bkz: [okuyucu](https://msdn.microsoft.com/library/azure/dn905997.aspx) ve [yazıcı](https://msdn.microsoft.com/library/azure/dn905984.aspx) konularıyla ilgili MSDN Kitaplığı. Önceki bölümdeki örnek, Azure Blob okuyucu ve Azure Blob yazıcı kullanılır. Bu bölümde, Azure SQL okuyucu ve Azure SQL yazıcı kullanımını açıklar.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
**S:** My büyük veri ardışık düzen tarafından üretilen birden fazla dosya var. Tüm dosyalar üzerinde çalışmaya AzureMLBatchExecution etkinliği kullanabilir miyim?

**C:** Evet. Bkz: **Azure Blob içinde birden çok dosyadan veri okumak için bir okuyucu modülü kullanarak** ayrıntıları bölümü.

## <a name="azure-machine-learning-studio-batch-scoring-activity"></a>Azure Machine Learning studio toplu iş Puanlama etkinliği
Kullanıyorsanız **AzureMLBatchScoring** etkinliği, Azure Machine Learning ile tümleştirmek için en son kullanmanızı öneririz **AzureMLBatchExecution** etkinlik.

AzureMLBatchExecution etkinlik sunulan Azure SDK'sını ve Azure PowerShell Ağustos 2015 sürümünde.

AzureMLBatchScoring etkinlik kullanmaya devam etmek istiyorsanız, bu bölümde okuma devam edin.

### <a name="azure-machine-learning-studio-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Azure Machine Learning studio giriş/çıkış için Azure depolama kullanarak toplu Puanlama etkinliği

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
Web hizmeti parametreleri için değerleri belirtmek için bir **typeProperties** bölümünü **AzureMLBatchScoringActivity** aşağıdaki örnekte gösterildiği gibi JSON işlem hattında bölümünde:

```JSON
"typeProperties": {
    "webServiceParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```
Ayrıca [Data Factory işlevleri](data-factory-functions-variables.md) Web değerler geçirerek parametreleri aşağıdaki örnekte gösterildiği gibi hizmet:

```JSON
"typeProperties": {
    "webServiceParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> Web hizmeti parametreleri büyük/küçük harfe duyarlıdır, dolayısıyla etkinliğinde belirttiğiniz adları JSON Web hizmeti tarafından kullanıma sunulan olanları eşleşmesini.
>
>

## <a name="see-also"></a>Ayrıca Bkz.
* [Azure Web günlüğü gönderisi: Azure Data Factory ve Azure Machine Learning ile çalışmaya başlama](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)

[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: https://azure.microsoft.com/services/machine-learning/
