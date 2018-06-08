---
title: Azure Machine Learning çalışma modeli veri koleksiyonu özelliğini kullanma | Microsoft Docs
description: Bu makalede Azure Machine Learning çalışma ekranı model verileri koleksiyonu özelliğini kullanma hakkında ettiği
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/12/2017
ms.openlocfilehash: 7a76322d70f6b54d65a4b751a7187425cb4be821
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34834551"
---
# <a name="collect-model-data-by-using-data-collection"></a>Veri toplama kullanarak model verileri toplama

Model girişleri ve bir web hizmetinden tahminleri arşivlemek için Azure Machine Learning modeli veri koleksiyonu özelliğini kullanabilirsiniz.

## <a name="install-the-data-collection-package"></a>Veri Toplama Paketi Yükle
Linux ve Windows, veri toplama Kitaplığı yerel olarak yükleyebilirsiniz.

### <a name="windows"></a>Windows
Windows üzerinde aşağıdaki komutu kullanarak veri toplayıcı modülü yükleyin:

    pip install azureml.datacollector

### <a name="linux"></a>Linux
Linux üzerinde ilk libxml ++ Kitaplığı yükleyin. Sudo altında verilmiş olması gerekir aşağıdaki komutu çalıştırın:

    sudo apt-get install libxml++2.6-2v5

Ardından aşağıdaki komutu çalıştırın:

    pip install azureml.datacollector

## <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

Model veri koleksiyonu, iki ortam değişkenlerini bağlıdır. AML_MODEL_DC_STORAGE_ENABLED ayarlanmalıdır **true** (tüm küçük harf) ve AML_MODEL_DC_STORAGE ayarlanmalıdır Azure depolama hesabı bağlantı dizesi verileri depolamak istediğiniz.

Web hizmeti azure'da bir kümede çalışırken zaten bu ortam değişkenleri ayarlanır. Yerel olarak çalıştırırken, kendiniz ayarlamanız gerekir. Docker kullanıyorsanız, ortam değişkenleri geçirmek için komutu Çalıştır docker -e parametresini kullanın.

## <a name="collect-data"></a>Veri toplama

Model veri koleksiyonu kullanmak için Puanlama dosyanıza aşağıdaki değişiklikleri yapın:

1. Dosyanın üst kısmında aşağıdaki kodu ekleyin:
   
    ```python
    from azureml.datacollector import ModelDataCollector
    ```

2. Kod aşağıdaki satırları ekleyin `init()` işlevi:
    
    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector('model.pkl',identifier="inputs")
    prediction_dc = ModelDataCollector('model.pkl', identifier="prediction")
    ```

3. Kod aşağıdaki satırları ekleyin `run(input_df)` işlevi:
    
    ```python
    global inputs_dc, prediction_dc
    inputs_dc.collect(input_df)
    prediction_dc.collect(pred)
    ```

    Olduğundan emin olun değişkenleri `input_df` ve `pred` (tahmin değerinden `model.predict()`) çağırmadan önce başlatılmış `collect()` bunlardaki işlevi.

4. Kullanım `az ml service create realtime` komutunu `--collect-model-data true` gerçek zamanlı web hizmeti oluşturmak için anahtar. Bu adım hizmetin çalıştırdığınızda model verileri toplanır emin olur.

     ```batch
    c:\temp\myIris> az ml service create realtime -f iris_score.py --model-file model.pkl -s service_schema.json -n irisapp -r python --collect-model-data true 
    ```
    
5. Veri koleksiyonunu sınamak için çalıştırın `az ml service run realtime` komutu:

    ```
    C:\Temp\myIris> az ml service run realtime -i irisapp -d "ADD YOUR INPUT DATA HERE!!" 
    ``` 
    
## <a name="view-the-collected-data"></a>Toplanan verileri görüntüleme
Blob depolama alanına toplanan verileri görüntülemek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri**.
3. Arama kutusuna **depolama hesapları** ve Enter tuşuna seçin.
4. Gelen **depolama hesapları** arama dikey penceresinde, select **depolama hesabı** kaynak. Depolama hesabınız belirlemek için aşağıdaki adımları kullanın:

    a. Azure Machine Learning çalışma ekranına gidin, proje üzerinde çalışıyorsanız ve bir komut isteminden açmak seçin **dosya** menüsü.
    
    b. Girin `az ml env show -v` ve denetleme *storage_account* değeri. Bu değer, depolama hesabınızın adıdır.

5. Seçin **kapsayıcıları** kaynak dikey menü ve kapsayıcı adlı **modeldata**. Depolama hesabı yayılıyor Başlat verileri görmek için ilk web hizmeti isteğine sonraki 10 dakika kadar beklemeniz gerekebilir. Veriler aşağıdaki kapsayıcı yoluyla bloblara akar:

    `/modeldata/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<day>/data.csv`

Verileri Azure bloblarından tüketilen çeşitli yollarla, Microsoft yazılım ve açık kaynaklı araçları aracılığıyla. İşte bazı örnekler:
- Azure Machine Learning çalışma ekranı:, .csv dosyası bir veri kaynağı olarak ekleyerek, Azure Machine Learning çalışma .csv dosyasını açın.
- Excel: günlük .csv dosyaları hesap çizelgesi olarak açın.
- [Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/): BLOB'lar .csv verilerden çekilen verilerle grafikleri oluşturun.
- [Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-overview): .csv veri büyük bölümünü bir veri çerçevesi oluşturun.
    ```python
    var df = spark.read.format("com.databricks.spark.csv").option("inferSchema","true").option("header","true").load("wasb://modeldata@<storageaccount>.blob.core.windows.net/<subscription_id>/<resource_group_name>/<model_management_account_name>/<webservice_name>/<model_id>-<model_name>-<model_version>/<identifier>/<year>/<month>/<date>/*")
    ```
- [Hive](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-tutorial-get-started): yük .csv verilerini bir Hive tablo ve SQL sorguları doğrudan blob gerçekleştirin.

