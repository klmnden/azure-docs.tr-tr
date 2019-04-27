---
title: Üretim Modellerinizi verileri toplama
titleSuffix: Azure Machine Learning service
description: Azure Blob depolama alanındaki Azure Machine Learning modeli giriş verilerini nasıl toplayacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 12/3/2018
ms.custom: seodec18
ms.openlocfilehash: a127a211157edb0b26d0495bc2ed05dd79323111
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60820434"
---
# <a name="collect-data-for-models-in-production"></a>Üretimde modelleri için veri toplama

Bu makalede, giriş model verileri, dağıttınız Azure Machine Learning hizmetlerinden Azure Kubernetes kümesi (AKS) ile bir Azure Blob depolama alanına toplamak nasıl öğrenebilirsiniz. 

Etkinleştirildikten sonra bu verileri, Topla, yardımcı olur:
* Üretim veri modelinizi girdikçe veri drifts izleme

* Daha iyi kararlar üzerinde ne zaman yeniden eğitme veya modelinizi iyileştirin

* Topladığınız verilerle, modeli yeniden eğitme

## <a name="what-is-collected-and-where-does-it-go"></a>Ne toplanır ve nereye?

Aşağıdaki veriler toplanabilir:
* Model **giriş** Azure Kubernetes kümesi (AKS) dağıtılan web Hizmetleri verilerini (ses, görüntü ve video **değil** toplanan) 
  
* Üretim giriş verileri kullanarak model tahmin.

> [!Note]
> Hizmetin bir parçası önceden toplayarak veya bu verileri önceden hesaplamaları şu anda değildir.   

Çıkış, bir Azure Blob üzerinde kaydedilmiş. Bir Azure Blob veri eklendikten sonra analizi çalıştırmak için en sevdiğiniz aracı seçebilirsiniz. 

Çıktı verilerini BLOB yolunu bu söz dizimi aşağıdaki gibidir:

```
/modeldata/<subscriptionid>/<resourcegroup>/<workspace>/<webservice>/<model>/<version>/<identifier>/<year>/<month>/<day>/data.csv
# example: /modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
```

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

- Bir Azure Machine Learning hizmeti çalışma alanında, yüklü Python için betikleri ve Azure Machine Learning SDK'sını içeren yerel bir dizin. Kullanarak şu önkoşul olarak gerekenleri edinin öğrenin [bir geliştirme ortamı yapılandırma](how-to-configure-environment.md) belge.

- Azure Kubernetes Service (AKS) dağıtılması için eğitilen makine öğrenme modeli. Yoksa, bkz. [görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md) öğretici.

- Azure Kubernetes hizmeti kümesi. Oluşturma ve bir dağıtma hakkında daha fazla bilgi için bkz. [nasıl dağıtılacağı ve nerede](how-to-deploy-and-where.md) belge.

- [Ortamınızı ayarlama](how-to-configure-environment.md) yükleyip [izleme SDK](https://aka.ms/aml-monitoring-sdk).

## <a name="enable-data-collection"></a>Veri toplamayı etkinleştirme
Veri toplama, Azure Machine Learning hizmeti veya diğer araçları dağıtılan model bağımsız olarak etkinleştirilebilir. 

Bunu etkinleştirmek için şunları yapmanız:

1. Puanlama dosyası açın. 

1. Ekleme [koddan](https://aka.ms/aml-monitoring-sdk) dosyanın üst:

   ```python 
   from azureml.monitoring import ModelDataCollector
   ```

2. Veri koleksiyonu değişkenlerinizi bildirin, `init()` işlevi:

    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector("best_model", identifier="inputs", feature_names=["feat1", "feat2", "feat3". "feat4", "feat5", "feat6"])
    prediction_dc = ModelDataCollector("best_model", identifier="predictions", feature_names=["prediction1", "prediction2"])
    ```

    *Correlationıd* isteğe bağlı bir parametre modelinizi gerektirmiyorsa ayarlamanız gerekmez. Yerinde bir bağıntı kimliği olan diğer verilerle daha kolay eşlemesi için yardımcı olur. (Örnekler: LoanNumber CustomerID, vs.)
    
    *Tanımlayıcı* daha sonra BLOB klasör yapısını oluşturmak için kullanılan, onu "işlenen" yerine "ham" verileri bölme için kullanılabilir.

3.  Aşağıdaki kod satırlarını ekleme `run(input_df)` işlevi:

    ```python
    data = np.array(data)
    result = model.predict(data)
    inputs_dc.collect(data) #this call is saving our input data into Azure Blob
    prediction_dc.collect(result) #this call is saving our input data into Azure Blob
    ```

4. Veri koleksiyonu **değil** otomatik olarak ayarlanmış **true** , AKS içinde bir hizmet dağıttığınızda, bu nedenle güncelleştirmeniz gerekir yapılandırma dosyanız aşağıdaki gibi: 

    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True)
    ```
    İzleme hizmeti için Appınsights bu yapılandırmayı değiştirerek de açılabilir:
    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True, enable_app_insights=True)
    ``` 

5. Yeni bir görüntü oluşturmak ve hizmeti dağıtmak için bkz: [nasıl dağıtılacağı ve nerede](how-to-deploy-and-where.md) belge.


Yüklenen bağımlılıkları olan bir hizmet zaten varsa, **ortam dosyası** ve **Puanlama dosyası**, tarafından veri toplamayı etkinleştirin:

1. Git [Azure portalında](https://portal.azure.com).

1. Çalışma alanını açın.

1. Git **dağıtımları** -> **hizmet Seç** -> **Düzenle**.

   ![Hizmet Düzenle](media/how-to-enable-data-collection/EditService.PNG)

1. İçinde **Gelişmiş ayarlar**, seçimini **etkinleştirme Model veri koleksiyonu**. 

    [![Veri toplamayı kontrol edin](media/how-to-enable-data-collection/CheckDataCollection.png)](./media/how-to-enable-data-collection/CheckDataCollection.png#lightbox)

   Bu pencerede, "Appınsights tanılamasını etkinleştir" seçebilirsiniz, hizmet durumunu izlemek için.  

1. Seçin **güncelleştirme** değişikliği uygulamak için.


## <a name="disable-data-collection"></a>Veri toplamayı devre dışı
Dilediğiniz zaman veri toplamayı durdurabilirsiniz. Veri toplamayı devre dışı bırakmak için Python kodu ya da Azure portal'ı kullanın.

+ Seçenek 1 - Azure portalında devre dışı bırak: 
  1. [Azure portalda](https://portal.azure.com) oturum açın.

  1. Çalışma alanını açın.

  1. Git **dağıtımları** -> **hizmet Seç** -> **Düzenle**.

     [![Düzenle seçeneği](media/how-to-enable-data-collection/EditService.PNG)](./media/how-to-enable-data-collection/EditService.PNG#lightbox)

  1. İçinde **Gelişmiş ayarlar**, seçimini **etkinleştirme Model veri koleksiyonu**. 

     [![Veri Toplama'seçeneğinin işaretini kaldırın](media/how-to-enable-data-collection/UncheckDataCollection.png)](./media/how-to-enable-data-collection/UncheckDataCollection.png#lightbox)

  1. Seçin **güncelleştirme** değişikliği uygulamak için.

+ Seçenek 2 - veri toplama devre dışı bırakmak için Python kullanın:

  ```python 
  ## replace <service_name> with the name of the web service
  <service_name>.update(collect_model_data=False)
  ```

## <a name="validate-your-data-and-analyze-it"></a>Verilerinizi doğrulamak ve analiz edin
Herhangi bir aracı, Azure Blob içinde toplanan verileri çözümlemek için tercihinizi seçebilirsiniz. 

Verileri, BLOB'dan hızlıca erişmek için:
1. [Azure portalda](https://portal.azure.com) oturum açın.

1. Çalışma alanını açın.
1. Tıklayarak **depolama**.

    [![Depolama](media/how-to-enable-data-collection/StorageLocation.png)](./media/how-to-enable-data-collection/StorageLocation.png#lightbox)

1. Bu söz dizimi ile BLOB çıktı veri yolunu izleyin:

```
/modeldata/<subscriptionid>/<resourcegroup>/<workspace>/<webservice>/<model>/<version>/<identifier>/<year>/<month>/<day>/data.csv
# example: /modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
```


### <a name="analyzing-model-data-through-power-bi"></a>Power BI aracılığıyla model verileri analiz etme

1. İndir ve Aç [Power BI Desktop](https://www.powerbi.com)

1. Seçin **Veri Al** tıklayın [ **Azure Blob Depolama**](https://docs.microsoft.com/power-bi/desktop-data-sources).

    [![PBI Blob Kurulumu](media/how-to-enable-data-collection/PBIBlob.png)](./media/how-to-enable-data-collection/PBIBlob.png#lightbox)


1. Depolama hesabınızın adını ekleyin ve depolama anahtarınızı girin. Bu bilgiler, blob'un içinde bulabilirsiniz **ayarları** >> erişim anahtarları. 

1. Kapsayıcıyı seçin **modeldata** tıklayın **Düzenle**. 

    [![PBI Gezgini](media/how-to-enable-data-collection/pbiNavigator.png)](./media/how-to-enable-data-collection/pbiNavigator.png#lightbox)

1. Sorgu Düzenleyicisi altındaki "Name" sütun tıklayın ve depolama hesabınızı 1 ekleyin. Filtre modeli yolu. Not: yalnızca belirli bir yıl ya da aylık dosyalarına aramak istiyorsanız, yalnızca filtre yolunu genişletin. Örneğin, yalnızca Mart verileri arayın: / modeldata/subscriptionıd > / resourcegroupname > / workspacename > / webservicename > / modelname > / modelversion > / tanımlayıcısı > / Yıl > / 3

1. Temel alarak ilgili verileri filtreleme **adı**. Depolanmış durumunda **Öngörüler** ve **girişleri** her bir sorgu oluşturmanız gerekir.

1. CPU'nun çift oku **içerik** dosyaları birleştirmek için sütun. 

    [![PBI içeriği](media/how-to-enable-data-collection/pbiContent.png)](./media/how-to-enable-data-collection/pbiContent.png#lightbox)

1. Tamam'ı tıklatın ve verileri dağıtılacak.

    [![pbiCombine](media/how-to-enable-data-collection/pbiCombine.png)](./media/how-to-enable-data-collection/pbiCombine.png#lightbox)

1. Artık tıklayabilirsiniz **Kapat ve Uygula** .

1.  Tarafından girişlerini ve tahminlerini tablolarınızı olacak otomatik olarak eklendiyse bağıntısını **RequestId**.

1. Model verileriniz üzerinde özel raporlar oluşturmaya başlayın.


### <a name="analyzing-model-data-using-databricks"></a>Databricks kullanarak model verileri analiz etme

1. Oluşturma bir [Databricks çalışma alanı](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal). 

1. Databricks çalışma alanınıza gidin. 

1. İçinde databricks çalışma alanı seçin **karşıya veri**.

    [![DB karşıya yükleme](media/how-to-enable-data-collection/dbupload.png)](./media/how-to-enable-data-collection/dbupload.png#lightbox)

1. Yeni bir tablo oluşturun ve seçin **diğer veri kaynakları** Azure Blob Depolama alanı -> Create Table not defterinde ->.

    [![DB tablosu](media/how-to-enable-data-collection/dbtable.PNG)](./media/how-to-enable-data-collection/dbtable.PNG#lightbox)

1. Verilerinizi konumunu güncelleştirin. Örnek aşağıda verilmiştir:

    ```
    file_location = "wasbs://mycontainer@storageaccountname.blob.core.windows.net/modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/*/*/data.csv" 
    file_type = "csv"
    ```
 
    [![DBsetup](media/how-to-enable-data-collection/dbsetup.png)](./media/how-to-enable-data-collection/dbsetup.png#lightbox)

1. Şablon görüntülemek ve çözümlemek için adımları izleyin. 

## <a name="example-notebook"></a>Örneğin not defteri

[How-to-use-azureml/deployment/enable-data-collection-for-models-in-aks/enable-data-collection-for-models-in-aks.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/enable-data-collection-for-models-in-aks/enable-data-collection-for-models-in-aks.ipynb) Not Defteri, bu makaledeki kavramları göstermektedir.  

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
