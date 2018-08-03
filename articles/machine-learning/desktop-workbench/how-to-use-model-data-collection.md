---
title: Azure Machine Learning Workbench'te model veri koleksiyonu özelliğini kullanırsanız | Microsoft Docs
description: Bu makalede, Azure Machine Learning Workbench'te model veri toplama özelliğini kullanma hakkında konuşuyor
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: 5c1a884ebe6216c4e8099f2ada2182ccff68b63e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449788"
---
# <a name="collect-model-data-by-using-data-collection"></a>Veri koleksiyonu kullanarak model verileri toplama

Azure Machine Learning'de model veri toplama özelliği, model girişlerini ve tahminlerini bir web hizmetinden arşivlemek için kullanabilirsiniz.

## <a name="install-the-data-collection-package"></a>Veri toplama paketi yükleyin
Veri koleksiyonu kitaplığı, Linux ve Windows üzerinde yerel olarak yükleyebilirsiniz.

### <a name="windows"></a>Windows
Windows üzerinde veri toplayıcı Modülü aşağıdaki komutu kullanarak yükleyin:

    pip install azureml.datacollector

### <a name="linux"></a>Linux
Linux üzerinde ilk libxml ++ Kitaplığı yükleyin. Sudo altında verilmiş olması gerekir aşağıdaki komutu çalıştırın:

    sudo apt-get install libxml++2.6-2v5

Ardından aşağıdaki komutu çalıştırın:

    pip install azureml.datacollector

## <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

Model veri koleksiyonu, iki ortam değişkenlerini bağlıdır. AML_MODEL_DC_STORAGE_ENABLED ayarlanmalıdır **true** (tamamı küçük harf) ve AML_MODEL_DC_STORAGE ayarlanmalıdır Azure depolama hesabı için bağlantı dizesi verileri depolamak istediğiniz yeri.

Web hizmeti, azure'da bir kümede çalışırken zaten bu ortam değişkenleri ayarlanır. Yerel olarak çalıştırılırken bunları kendiniz ayarlamanız gerekir. Docker'ı kullanıyorsanız, ortam değişkenleri geçirmek için komutu çalıştırarak docker -e parametresini kullanın.

## <a name="collect-data"></a>Veri toplama

Model veri koleksiyonu kullanmak için Puanlama dosyanıza aşağıdaki değişiklikleri yapın:

1. Dosyasının en üstüne aşağıdaki kodu ekleyin:
   
    ```python
    from azureml.datacollector import ModelDataCollector
    ```

1. Aşağıdaki kod satırlarını ekleme `init()` işlevi:
    
    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
    prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
    ```

1. Aşağıdaki kod satırlarını ekleme `run(input_df)` işlevi:
    
    ```python
    global inputs_dc, prediction_dc
    inputs_dc.collect(input_df)
    prediction_dc.collect(pred)
    ```

    Emin olun değişkenleri `input_df` ve `pred` (tahmin değerinden `model.predict()`) çağırmadan önce başlatılır `collect()` bunlardaki işlevi.

1. Kullanım `az ml service create realtime` komutunu `--collect-model-data true` gerçek zamanlı web hizmeti oluşturmak için anahtar. Bu adım hizmet çalıştırdığınızda model verileri toplandığı emin olur.

     ```batch
    c:\temp\myIris> az ml service create realtime -f iris_score.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true 
    ```
    
1. Veri koleksiyonunu sınamak için çalıştırın `az ml service run realtime` komutu:

    ```
    C:\Temp\myIris> az ml service run realtime -i irisapp -d "ADD YOUR INPUT DATA HERE!!" 
    ``` 
    
## <a name="view-the-collected-data"></a>Toplanan verileri görüntüleme
Blob depolama alanında toplanan verileri görüntülemek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Seçin **tüm hizmetleri**.
1. Arama kutusuna **depolama hesapları** ve Enter tuşunu seçin.
1. Gelen **depolama hesapları** arama dikey penceresinde **depolama hesabı** kaynak. Depolama hesabınızı belirlemek için aşağıdaki adımları kullanın:

    a. Azure Machine Learning Workbench'i gidin, üzerinde çalıştığınız ve bir komut istemi açın projeyi seçin **dosya** menüsü.
    
    b. Girin `az ml env show -v` ve *storage_account* değeri. Bu değer, depolama hesabınızın adıdır.

1. Seçin **kapsayıcıları** kaynak dikey penceresini menü ve kapsayıcı adlı **modeldata**. Depolama hesabına yayma Başlat verileri görmek için ilk web hizmeti isteğinden sonra 10 dakikaya kadar beklemeniz gerekebilir. Veriler aşağıdaki kapsayıcı yoluyla bloblara akar:

    `/modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv`

Verileri Azure bloblarından kullanılabilir olarak çeşitli yollarla, hem Microsoft yazılımlarını hem de açık kaynak araçları aracılığıyla. İşte bazı örnekler:
- Azure Machine Learning Workbench: .csv dosyasını veri kaynağı olarak ekleyerek, Azure Machine Learning Workbench'te .csv dosyasını açın.
- Excel: günlük .csv dosyalarını çalışma sayfası olarak açın.
- [Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/): BLOB'lar .csv verilerinden çekilen verilerle grafik oluşturun.
- [Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-overview): .csv veri büyük bir kısmı bir veri çerçevesi oluşturun.
    ```python
    var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
    ```
- [Hive](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): yük .csv verilerini bir Hive tablosu ve doğrudan bloba SQL sorguları gerçekleştirme.

