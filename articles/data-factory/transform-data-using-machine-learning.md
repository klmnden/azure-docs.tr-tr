---
title: Azure Data Factory kullanarak Tahmine dayalı veri işlem hatları oluşturun | Microsoft Docs
description: Azure Machine Learning - Batch yürütme etkinliği Azure Data factory'de kullanarak Tahmine dayalı bir işlem hattı oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/20/2019
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: aaf1d72a0c9c56e7d140fb615caf014507ebf263
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60928088"
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a>Azure Machine Learning ve Azure Data Factory kullanarak öngörülebilir komut zincirleri oluşturma
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1](v1/data-factory-azure-ml-batch-execution-activity.md)
> * [Geçerli sürüm](transform-data-using-machine-learning.md)

[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) derleme, test ve Tahmine dayalı analiz çözümlerini dağıtmanıza olanak sağlar. Bir üst düzey açısından bakıldığında, bu üç adımda gerçekleştirilir:

1. **Eğitim denemesini oluşturma**. Azure Machine Learning Studio'yu kullanarak, bu adımı uygulayın. Azure Machine Learning studio, eğitim ve eğitim verilerini kullanarak bir Tahmine dayalı analiz modeli test etmek için kullandığınız bir görsel işbirliğine dayalı geliştirme ortamıdır.
2. **Öngörücü bir denemeye dönüştürme**. Modelinizi mevcut verilerle eğitim almış ve yeni verileri puanlamak için kullanıma hazır sonra hazırlama ve puanlama için deneyiminizi kolaylaştırın.
3. **Bir web hizmeti olarak dağıtalım**. Bir Azure web hizmeti olarak Puanlama denemenizi yayımlayabilirsiniz. Bu web hizmeti uç noktası aracılığıyla modelinizi veri göndermek ve modelden sonucu Öngörüler alırsınız.

### <a name="data-factory-and-machine-learning-together"></a>Data Factory ve Machine Learning ile birlikte
Azure Data Factory bir yayımlanan kullanan işlem hatları kolayca oluşturmanıza olanak sağlar [Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning) web için Tahmine dayalı analiz hizmetidir. Kullanarak **Batch yürütme etkinliği** toplu verilerde tahmin yapmayı sağlayan bir Azure Machine Learning studio web hizmeti bir Azure Data Factory işlem hattında çağırabilirsiniz.

Zaman içinde yeni bir giriş veri kümeleri kullanarak eğitilebileceği denemeleri Puanlama Azure Machine Learning Studio'da Tahmine dayalı modelleri gerekir. Aşağıdaki adımları uygulayarak bir Data Factory işlem hattı modelden yeniden eğitebilir:

1. Eğitim denemesini (değil Tahmine dayalı denemeye) bir web hizmeti olarak yayımlayın. Tahmine dayalı denemeye önceki senaryoda bir web hizmeti olarak kullanıma sunmak için yaptığınız gibi Azure Machine Learning Studio'da bu adımı uygulayın.
2. Batch yürütme etkinliği Azure Machine Learning studio web hizmeti için eğitim denemesini çağırmak için kullanın. Temel olarak, hem eğitim web hizmeti hem de Puanlama web hizmeti çağırmak için Azure Machine Learning studio Batch yürütme etkinliği kullanabilirsiniz.

Yeniden eğitme ile işiniz bittiğinde, Puanlama web hizmeti (bir web hizmeti olarak kullanıma sunulan Tahmine dayalı denemeye) ile yeni eğitim modeli kullanarak güncelleştirme **Azure Machine Learning studio güncelleştirmek kaynak etkinliği**. Bkz: [kaynak güncelleştirme etkinliği kullanarak modelleri güncelleştirme](update-machine-learning-models.md) makale Ayrıntılar için.

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning bağlı hizmeti

Oluşturduğunuz bir **Azure Machine Learning** bağlı hizmet bir Azure Machine Learning Web hizmeti bir Azure veri fabrikasına bağlamak için. Bağlı hizmeti Azure Machine Learning Batch yürütme etkinliği tarafından kullanılır ve [kaynak güncelleştirme etkinliği](update-machine-learning-models.md).

```JSON
{
    "type" : "linkedServices",
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "URL to Azure ML Predictive Web Service",
            "apiKey": {
                "type": "SecureString",
                "value": "api key"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Bkz: [işlem bağlı Hizmetleri](compute-linked-services.md) açıklamaları JSON tanımındaki özellikler hakkında daha fazla makale.

Azure Machine Learning Klasik Web hizmetleri hem de yeni Web Hizmetleri, Tahmine dayalı denemeye için destek. Data Factory tarafından kullanılacak doğru olanı seçebilirsiniz. Azure Machine Learning bağlı hizmetini oluşturmak için gereken bilgileri almak için Git https://services.azureml.net, tüm Web Hizmetleri (yeni) ve Klasik Web Hizmetleri burada listelenir. Web erişimi ve istediğiniz hizmeti **Tüket** sayfası. Kopyalama **birincil anahtar** için **apiKey** özelliği ve **toplu istekleri** için **mlEndpoint** özelliği.

![Azure Machine Learning Web Hizmetleri](./media/transform-data-using-machine-learning/web-services.png)

## <a name="azure-machine-learning-batch-execution-activity"></a>Azure Machine Learning Batch Execution etkinliği

Aşağıdaki JSON kod parçacığında, bir Azure Machine Learning Batch Execution etkinliği tanımlar. Etkinlik tanımı, daha önce oluşturduğunuz bağlı Azure Machine Learning hizmetine başvuru içeriyor.

```JSON
{
    "name": "AzureMLExecutionActivityTemplate",
    "description": "description",
    "type": "AzureMLBatchExecution",
    "linkedServiceName": {
        "referenceName": "AzureMLLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "webServiceInputs": {
            "<web service input name 1>": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService1",
                    "type": "LinkedServiceReference"
                },
                "FilePath":"path1"
            },
            "<web service input name 2>": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService1",
                    "type": "LinkedServiceReference"
                },
                "FilePath":"path2"
            }
        },
        "webServiceOutputs": {
            "<web service output name 1>": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService2",
                    "type": "LinkedServiceReference"
                },
                "FilePath":"path3"
            },
            "<web service output name 2>": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService2",
                    "type": "LinkedServiceReference"
                },
                "FilePath":"path4"
            }
        },
        "globalParameters": {
            "<Parameter 1 Name>": "<parameter value>",
            "<parameter 2 name>": "<parameter 2 value>"
        }
    }
}
```

| Özellik          | Açıklama                              | Gerekli |
| :---------------- | :--------------------------------------- | :------- |
| ad              | İşlem hattındaki etkinliğin adı     | Evet      |
| açıklama       | Etkinliğin ne yaptığını açıklayan metin.  | Hayır       |
| type              | Data Lake Analytics U-SQL etkinliği için etkinlik türdür **AzureMLBatchExecution**. | Evet      |
| linkedServiceName | Bağlı hizmeti Azure Machine learning'e bağlı hizmetler. Bu bağlı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi. | Evet      |
| Webserviceınputs  | Azure Machine Learning Web hizmeti girişleri adlarını eşleme anahtar, değer çiftleri. Anahtarı, yayımlanan Azure Machine Learning Web hizmeti içinde tanımlanan giriş parametreleri eşleşmelidir. Giriş Blob konumları belirten bir Azure depolama bağlı hizmetleri ve FilePath özellikleri çifti değerdir. | Hayır       |
| webServiceOutputs | Azure Machine Learning Web hizmeti çıkışları adlarını eşleme anahtar, değer çiftleri. Anahtarı, yayımlanan Azure Machine Learning Web hizmeti içinde tanımlanan çıkış parametreleri eşleşmelidir. Değer, bir Azure depolama bağlı hizmetleri ve dosya yolu: çıkış belirten özellikleri çifti Blob konumları. | Hayır       |
| globalParameters  | Azure Machine Learning studio toplu yürütme Hizmeti uç noktası için geçirilecek anahtar, değer çiftleri. Anahtarları yayımlanan Azure Machine Learning studio web hizmeti tanımlı web hizmeti parametrelerini adları eşleşmelidir. Azure Machine Learning studio toplu yürütme isteği GlobalParameters özelliğinde değerleri geçirilir | Hayır       |

### <a name="scenario-1-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Senaryo 1: Web hizmeti girdiler/Azure Blob depolama alanındaki verilere başvuran çıktılar kullanarak denemeler

Bu senaryoda, Azure Machine Learning Web hizmeti bir Azure blob depolamadaki bir dosyadan verileri kullanarak Öngörüler sağlar ve blob depolama alanında tahmin sonuçlarını depolar. Aşağıdaki JSON ile AzureMLBatchExecution etkinliği bir Data Factory işlem hattı tanımlar. Girdi ve çıktı verilerini Azure Blog depolama alanındaki bir LinkedName ve FilePath çifti kullanarak başvuruluyor. Örnekte bağlı hizmetin adı girişler ve çıkışlar farklı, doğru dosyaları seçin ve Azure Machine Learning studio Web hizmeti için göndermek için farklı bir bağlı hizmetler her girdiler/çıktılar Data Factory için kullanabilirsiniz.

> [!IMPORTANT]
> Azure Machine Learning studio denemesi, web hizmeti giriş ve çıkış bağlantı noktaları ve genel parametrelerini özelleştirebileceğiniz varsayılan adları ("input1", "input2") sahiptir. Webserviceınputs webServiceOutputs ve globalParameters ayarları için kullandığınız adları denemeleri adlarında tam olarak eşleşmelidir. Örnek istek yükü beklenen eşleme doğrulamak Azure Machine Learning studio uç noktanız için toplu işlem yürütme Yardım sayfasında görüntüleyebilirsiniz.
>
>

```JSON
{
    "name": "AzureMLExecutionActivityTemplate",
    "description": "description",
    "type": "AzureMLBatchExecution",
    "linkedServiceName": {
        "referenceName": "AzureMLLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "webServiceInputs": {
            "input1": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService1",
                    "type": "LinkedServiceReference"
                },
                "FilePath":"amltest/input/in1.csv"
            },
            "input2": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService1",
                    "type": "LinkedServiceReference"
                },
                "FilePath":"amltest/input/in2.csv"
            }
        },
        "webServiceOutputs": {
            "outputName1": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService2",
                    "type": "LinkedServiceReference"
                },
                "FilePath":"amltest2/output/out1.csv"
            },
            "outputName2": {
                "LinkedServiceName":{
                    "referenceName": "AzureStorageLinkedService2",
                    "type": "LinkedServiceReference"
                },
                "FilePath":"amltest2/output/out2.csv"
            }
        }
    }
}
```
### <a name="scenario-2-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Senaryo 2: Çeşitli depoları verilerde başvurmak için Okuyucu/Yazıcı modülleri'ni kullanarak deneme
Azure Machine Learning studio denemeleri oluştururken bir diğer yaygın senaryo, verileri içeri aktarma ve çıktı verilerini modülleri kullanmaktır. Verileri içeri aktarma modülü bir denemenin verileri yüklemek için kullanılır ve çıkış veri modülü denemelerinizden veri kaydetmektir. Verileri içeri aktarma ve çıktı verilerini modüller hakkında daha fazla ayrıntı için bkz: [verileri içeri aktarma](https://msdn.microsoft.com/library/azure/dn905997.aspx) ve [çıktı verilerini](https://msdn.microsoft.com/library/azure/dn905984.aspx) konularıyla ilgili MSDN Kitaplığı.

Verileri içeri aktarma ve çıktı verilerini modülleri kullanırken, bu modüllerin her bir özellik için bir Web hizmeti parametresi kullanmak iyi bir uygulamadır. Bu web parametreleri çalışma zamanı sırasında değerleri yapılandırmanıza olanak sağlar. Örneğin, bir Azure SQL veritabanı kullanan bir verileri içeri aktarma modülü ile bir deneme oluşturabilirsiniz: XXX.database.windows.net. Web hizmeti dağıtıldıktan sonra web hizmeti tüketicilerinin adlı başka bir Azure SQL Server'ı belirtmek etkinleştirmek istediğiniz `YYY.database.windows.net`. Bir Web hizmeti parametresi yapılandırılması için bu değeri izin vermek için kullanabilirsiniz.

> [!NOTE]
> Web hizmeti giriş ve çıkış, Web hizmeti parametreleri farklıdır. Bu senaryoda, bir giriş ve çıkış bir Azure Machine Learning studio Web hizmeti için nasıl belirtilebilir gördünüz. Bu senaryoda, veri/çıkış verilerini içeri aktar modülleri özelliklerine karşılık gelen bir Web hizmeti parametrelerini geçirin.
>
>

Web hizmeti parametrelerini kullanarak bir senaryo bakalım. Bir Azure Machine Learning tarafından desteklenen veri kaynakları verileri okumak için bir okuyucu modülü kullanan dağıtılmış bir Azure Machine Learning web hizmeti sahip (örneğin: Azure SQL veritabanı). Toplu yürütme işlemi gerçekleştirildikten sonra sonuçları bir yazıcı Modülü (Azure SQL veritabanı) kullanılarak yazılır.  Hiçbir web hizmeti giriş ve çıkışları denemeleri içinde tanımlanır. Bu durumda, okuyucu ve yazıcı modüller için ilgili web hizmeti parametreleri yapılandırmanızı öneririz. Bu yapılandırma Okuyucu/Yazıcı AzureMLBatchExecution etkinlik kullanırken yapılandırılması için modül sağlar. Web hizmeti parametreleri olarak belirttiğiniz **globalParameters** JSON etkinliğinde gibi bölümü.

```JSON
"typeProperties": {
    "globalParameters": {
        "Database server name": "<myserver>.database.windows.net",
        "Database name": "<database>",
        "Server user account name": "<user name>",
        "Server user account password": "<password>"
    }
}
```

> [!NOTE]
> Web hizmeti parametreleri büyük/küçük harfe duyarlıdır, dolayısıyla etkinliğinde belirttiğiniz adları JSON Web hizmeti tarafından kullanıma sunulan olanları eşleşmesini.
>

Yeniden eğitme ile işiniz bittiğinde, Puanlama web hizmeti (bir web hizmeti olarak kullanıma sunulan Tahmine dayalı denemeye) ile yeni eğitim modeli kullanarak güncelleştirme **Azure Machine Learning studio güncelleştirmek kaynak etkinliği**. Bkz: [kaynak güncelleştirme etkinliği kullanarak modelleri güncelleştirme](update-machine-learning-models.md) makale Ayrıntılar için.

## <a name="next-steps"></a>Sonraki adımlar
Anlatan farklı yollarla verileri dönüştürmek aşağıdaki makalelere bakın:

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akış etkinliğinde](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
