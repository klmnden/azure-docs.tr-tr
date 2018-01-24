---
title: "Azure Data Factory kullanarak Tahmine dayalı veri ardışık düzen oluşturun | Microsoft Docs"
description: "Azure Machine Learning - toplu iş yürütme etkinliği Azure Data factory'de kullanarak Tahmine dayalı bir ardışık düzen oluşturmayı öğrenin."
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: shengc
ms.openlocfilehash: fa493a6d7b4cf775f64b87c1d5cc21ff4a138609
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a>Azure Machine Learning ve Azure Data Factory kullanarak Tahmine dayalı ardışık düzen oluşturun
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-azure-ml-batch-execution-activity.md)
> * [Sürüm 2 - Önizleme](transform-data-using-machine-learning.md)

[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) derleme, test ve Tahmine dayalı analiz çözümlerini dağıtma olanak tanır. Bir üst düzey açısından bakıldığında, üç adımda gerçekleştirilir:

1. **Eğitim denemenizi oluşturma**. Azure ML Studio kullanarak bu adımı uygulayın. ML studio eğitmek ve eğitim verileri kullanarak bir Tahmine dayalı bir analiz modeli test etmek için kullandığınız bir görsel işbirlikçi geliştirme ortamıdır.
2. **Tahmine dayalı bir deneme Dönüştür**. Modelinizi var olan verilerle eğitilmiş ve yeni verilerinizi puanlamada için kullanıma hazır sonra hazırlamak ve puanlama için denemenizi kolaylaştırır.
3. **Web hizmeti olarak dağıtabilir**. Puanlama denemenizi bir Azure web hizmeti olarak yayımlayabilirsiniz. Bu web hizmeti uç noktası aracılığıyla modelde veri gönderebilir ve sonuç tahminleri model Excel'den alabilirsiniz.  

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [makine öğrenme toplu iş yürütme etkinliği V1](v1/data-factory-azure-ml-batch-execution-activity.md).


### <a name="data-factory-and-machine-learning-together"></a>Veri Fabrikası ve Machine Learning birlikte
Azure Data Factory, yayımlanan [Azure Machine Learning] [azure machine learning] web hizmeti Tahmine dayalı analiz kullanmak ardışık düzen kolayca oluşturmanıza olanak sağlar. Kullanarak **toplu iş yürütme etkinliği** bir Azure Data Factory işlem hattı verileri toplu tahminleri yapmak için bir Azure ML web hizmeti çağırabilirsiniz. 

Zaman içinde denemeler Puanlama Azure ML Tahmine dayalı modelleri yeni giriş veri kümeleri kullanarak retrained gerekir. Aşağıdakileri yaparak bir Data Factory işlem hattı Azure ML modelden yeniden eğitme:

1. Eğitim denemenizi (değil Tahmine dayalı denemeye) bir web hizmeti olarak yayımlayın. Tahmine dayalı denemeye önceki senaryoda bir web hizmeti olarak kullanıma sunmak için yaptığınız gibi Azure ML Studio'da bu adımı uygulayın.
2. Eğitim denemenizi web hizmetini çağırmak için Azure ML toplu iş yürütme etkinliği kullanın. Temel olarak, eğitim web hizmeti ve puanlama web hizmetini çağırmak için Azure ML toplu iş yürütme etkinliği kullanın.

Yeniden eğitme ile tamamladıktan sonra Puanlama web hizmeti (web hizmeti olarak sunulan Tahmine dayalı denemeye) ile yeni eğitilen modelini kullanarak güncelleştirme **Azure ML güncelleştirme kaynak etkinliği**. Bkz: [kaynak güncelleştirme etkinliği kullanarak modelleri güncelleştirme](update-machine-learning-models.md) Ayrıntılar için makale.

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning bağlı hizmeti

Oluşturduğunuz bir **Azure Machine Learning** bağlantılı hizmeti bir Azure Machine Learning Web hizmeti bir Azure data factory'ye bağlamak için. Bağlı hizmetin Azure Machine Learning toplu iş yürütme etkinliği tarafından kullanılır ve [kaynak güncelleştirme etkinliği](update-machine-learning-models.md). 


```JSON
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "URL to Azure ML Predictive Web Service",
            "apiKey": {
                "type": "SecureString",
                "value": "api key"
            }
        }
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Bkz: [işlem bağlı Hizmetleri](compute-linked-services.md) makale açıklamaları özellikleri JSON tanımında hakkında. 

Azure Machine Learning Klasik Web Hizmetleri ve Tahmine dayalı denemeniz için yeni Web Hizmetleri destekler. Veri Fabrikası'ndan kullanmak için doğru birini seçebilirsiniz. Azure Machine Learning bağlı hizmeti oluşturmak için gerekli bilgileri almak için tüm Web Hizmetleri (yeni) ve Klasik Web Hizmetleri listelendiği https://services.azureml.net için gidin. İstediğiniz erişmek ve tıklayın Web hizmeti **Tüket** sayfası. Kopya **birincil anahtar** için **apikey ile yapılan** özelliği ve **toplu istekleri** için **mlEndpoint** özelliği. 

![Azure Machine Learning Web Hizmetleri](./media/transform-data-using-machine-learning/web-services.png)

##<a name="azure-machine-learning-batch-execution-activity"></a>Azure Machine Learning toplu iş yürütme etkinliği

Aşağıdaki JSON parçacığında, bir Azure Machine Learning toplu iş yürütme etkinliği tanımlar. Etkinlik tanımı daha önce oluşturduğunuz Azure Machine Learning bağlantılı hizmeti bir başvuru içeriyor. 

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
| ad              | İşlem hattında etkinlik adı     | Evet      |
| açıklama       | Etkinlik yaptığı açıklayan metin.  | Hayır       |
| type              | Data Lake Analytics U-SQL etkinliği için etkinlik türüdür **AzureMLBatchExecution**. | Evet      |
| linkedServiceName | Azure Machine Learning için bağlantılı Hizmetleri hizmeti bağlı. Bu bağlantılı hizmeti hakkında bilgi edinmek için [işlem bağlı Hizmetleri](compute-linked-services.md) makalesi. | Evet      |
| webServiceInputs  | Azure Machine Learning Web hizmeti girişleri eşlenmesi anahtar, değer çiftleri. Anahtarı yayımlanan Azure Machine Learning Web hizmeti tanımlanan giriş parametreleri eşleşmelidir. Giriş Blob konumları belirten bir Azure depolama bağlantılı Hizmetleri ve FilePath özellikleri çifti değerdir. | Hayır       |
| webServiceOutputs | Azure Machine Learning Web hizmeti çıkışları eşlenmesi anahtar, değer çiftleri. Anahtarı yayımlanan Azure Machine Learning Web hizmeti içinde tanımlanan çıkış parametreleri eşleşmelidir. Değer bir Azure depolama bağlı hizmetleri ve FilePath çıkış belirten özellikleri çifti Blob konumları. | Hayır       |
| globalParameters  | Azure ML toplu yürütme Hizmeti uç noktasına geçirilecek anahtar, değer çiftleri. Anahtarlar yayımlanan Azure ML web hizmeti tanımlı web hizmeti parametreleri adları eşleşmelidir. Azure ML toplu iş yürütme isteği GlobalParameters özelliğinde değerleri geçirilir | Hayır       |

### <a name="scenario-1-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a>Senaryo 1: Web hizmeti girişleri/verileri Azure Blob Depolama başvuran çıkışları kullanarak denemelerini

Bu senaryoda, Azure Machine Learning Web hizmeti bir Azure blob depolama alanındaki bir dosyadan veri kullanarak tahminleri yapar ve tahmin sonuçlarını blob depolama alanında depolar. Aşağıdaki JSON Data Factory işlem hattı AzureMLBatchExecution etkinliği ile tanımlar. Girdi ve çıktı verilerini Azure blogu depolama LinkedName ve FilePath çifti kullanarak başvuruluyor. Aşağıdaki örnekte girişleri ve çıkışları bağlı hizmetin farklı, doğru dosyaları seçin ve Azure ML Web hizmetine göndermek için farklı bağlantılı hizmetler için her giriş/çıkış veri fabrikası için kullanabilirsiniz. 

> [!IMPORTANT]
> Azure ML deneme, web hizmeti giriş ve çıkış bağlantı noktaları ve genel parametreleri özelleştirebileceğiniz varsayılan adları ("input1", "input2") sahip. WebServiceInputs, webServiceOutputs ve globalParameters ayarları için kullandığınız adlarının denemeler adlarında tam olarak eşleşmelidir. Örnek istek yükü, beklenen eşleme doğrulamak Azure ML uç noktanız için toplu iş yürütme Yardım sayfasında görüntüleyebilirsiniz.
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
### <a name="scenario-2-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a>Senaryo 2: Denemelerini çeşitli depolarını verilerde başvurmak için Okuyucu/Yazıcı modüllerini kullanma
Azure ML denemeler oluştururken, başka bir yaygın senaryo veri içeri aktarma ve çıktı verilerini modülleri kullanmaktır. Veri içeri aktarma modül verileri bir deneme yüklemek için kullanılır ve çıktı verilerini denemelerinizden verileri kaydetmek için modülüdür. Veri içeri aktarma ve çıktı verilerini modüller hakkında daha fazla ayrıntı için bkz: [veri içeri aktar](https://msdn.microsoft.com/library/azure/dn905997.aspx) ve [çıktı verilerini](https://msdn.microsoft.com/library/azure/dn905984.aspx) MSDN Kitaplığı konularda.     

Veri içeri aktarma ve çıktı verilerini modülleri kullanırken, bu modüllerin her bir özellik için bir Web hizmeti parametresi kullanmak iyi bir uygulamadır. Bu web parametreleri değerlerini çalışma zamanı sırasında yapılandırmanıza olanak sağlar. Örneğin, bir Azure SQL veritabanı kullanan bir veri içeri aktarma modülü ile bir deneme oluşturabilirsiniz: XXX.database.windows.net. Web hizmeti tüketicileri adlı başka bir Azure SQL Server belirtmek etkinleştirmek istediğiniz web hizmeti dağıtıldıktan sonra `YYY.database.windows.net`. Yapılandırılması için bu değeri izin vermek için Web hizmeti parametresini kullanabilirsiniz.

> [!NOTE]
> Web hizmeti giriş ve çıkış Web hizmeti parametrelerinden farklıdır. İlk senaryoda, bir giriş ve çıkış için bir Azure ML Web hizmeti nasıl belirtilebilir gördünüz. Bu senaryoda, içeri aktarma verileri/çıktı verilerini modülleri özelliklerine karşılık gelen bir Web hizmeti parametreleri geçirin.
>
> 

Web hizmeti parametreleri kullanarak bir senaryo bakalım. Verileri Azure Machine Learning tarafından desteklenen veri kaynaklarından biri okunacak okuyucu modülü kullanan bir dağıtılan Azure Machine Learning web hizmetine sahip (örneğin: Azure SQL veritabanı). Toplu yürütme işlemi yapıldıktan sonra sonuçları yazıcı Modülü (Azure SQL veritabanı) kullanılarak yazılır.  Hiçbir web hizmeti girişleri ve çıkışları denemeler tanımlanır. Bu durumda, okuyucu ve yazıcı modülleri için ilgili web hizmeti parametreleri yapılandırmanızı öneririz. Bu yapılandırma Okuyucu/Yazıcı AzureMLBatchExecution etkinlik kullanırken yapılandırılması için modül sağlar. Web hizmeti parametreleri belirtin **globalParameters** JSON etkinliğinde gibi bölüm.

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
> Web hizmeti parametreleri büyük/küçük harfe duyarlıdır, bu nedenle etkinliğin belirttiğiniz adları JSON Web hizmeti tarafından sunulan olanları eşleşmesini.
>

Yeniden eğitme ile tamamladıktan sonra Puanlama web hizmeti (web hizmeti olarak sunulan Tahmine dayalı denemeye) ile yeni eğitilen modelini kullanarak güncelleştirme **Azure ML güncelleştirme kaynak etkinliği**. Bkz: [kaynak güncelleştirme etkinliği kullanarak modelleri güncelleştirme](update-machine-learning-models.md) Ayrıntılar için makale.



## <a name="next-steps"></a>Sonraki adımlar
Diğer yollarla verileri dönüştürmek açıklanmaktadır aşağıdaki makalelere bakın: 

* [U-SQL etkinliği](transform-data-using-data-lake-analytics.md)
* [Hive etkinliği](transform-data-using-hadoop-hive.md)
* [Pig etkinliği](transform-data-using-hadoop-pig.md)
* [MapReduce activity](transform-data-using-hadoop-map-reduce.md)
* [Hadoop akış etkinliği](transform-data-using-hadoop-streaming.md)
* [Spark etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Saklı yordam etkinliği](transform-data-using-stored-procedure.md)
