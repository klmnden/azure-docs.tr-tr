---
title: Üretimde - Azure Machine Learning modelleri için veri toplamayı etkinleştirin
description: Azure Blob depolama alanındaki Azure Machine Learning modeli giriş verilerini nasıl toplayacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 09/24/2018
ms.openlocfilehash: 4730003508463583d6620527e05bc330be599d80
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47032412"
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
# example: /modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myResGrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
```

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- Bir Azure Machine Learning çalışma alanı, yüklü Python için betikleri ve Azure Machine Learning SDK'sını içeren yerel bir dizin. Kullanarak şu önkoşul olarak gerekenleri edinin öğrenin [bir geliştirme ortamı yapılandırma](how-to-configure-environment.md) belge.

- Azure Kubernetes Service (AKS) dağıtılması için eğitilen makine öğrenme modeli. Yoksa, bkz. [görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md) öğretici.

- Bir [AKS kümesi](how-to-deploy-to-aks.md).

- Aşağıdaki bağımlılıklar ve yüklü modülün [ortamınızdaki](how-to-configure-environment.md):
  + Linux üzerinde:
    ```shell
    sudo apt-get install libxml++2.6-2v5
    pip install azureml-monitoring
    ```

  + Windows'da:
    ```shell
    pip install azureml-monitoring
    ```

## <a name="enable-data-collection"></a>Veri toplamayı etkinleştirme
Veri toplama, Azure Machine Learning hizmeti veya diğer araçları dağıtılan model bağımsız olarak etkinleştirilebilir. 

Bunu etkinleştirmek için şunları yapmanız:

1. Puanlama dosyası açın. 

1. Dosyasının en üstüne aşağıdaki kodu ekleyin:

   ```python 
   from azureml.monitoring import ModelDataCollector
   ```

2. Veri koleksiyonu değişkenlerinizi bildirin, `init()` işlevi:

    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector("best_model", identifier="inputs", feature_names=["feat1", "feat2", "feat3". "feat4", "feat5", "feat6"])
    prediction_dc = ModelDataCollector("best_model", identifier="predictions", feature_names=["prediction1", "prediction2"])
    ```

    *Correlationıd* isteğe bağlı bir parametre modelinizi gerektirmiyorsa ayarlamanız gerekmez. Yerinde bir bağıntı kimliği olan diğer verilerle daha kolay eşlemesi için yardımcı olur. (Örnekler: LoanNumber, CustomerID, vs.)
    
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

5. [Yeni görüntü oluşturup hizmetinize dağıtın.](how-to-deploy-to-aks.md) 


Yüklenen bağımlılıkları olan bir hizmet zaten varsa, **ortam dosyası** ve **Puanlama dosyası**, tarafından veri toplamayı etkinleştirin:

1. Git [Azure portalında](https://portal.azure.com).

1. Çalışma alanını açın.

1. Git **dağıtımları** -> **hizmet Seç** -> **Düzenle**.

   ![Hizmet Düzenle](media/how-to-enable-data-collection/EditService.png)

1. İçinde **Gelişmiş ayarlar**, seçimini **etkinleştirme Model veri koleksiyonu**. 

   ![Veri Toplama'seçeneğinin işaretini kaldırın](media/how-to-enable-data-collection/CheckDataCollection.png)

   Bu pencerede, "Appınsights tanılamasını etkinleştir" seçebilirsiniz, hizmet durumunu izlemek için.  

1. Seçin **güncelleştirme** değişikliği uygulamak için.


## <a name="disable-data-collection"></a>Veri toplamayı devre dışı
Dilediğiniz zaman veri toplamayı durdurabilirsiniz. Veri toplamayı devre dışı bırakmak için Python kodu ya da Azure portal'ı kullanın.

+ Seçenek 1 - Azure portalında devre dışı bırak: 
  1. [Azure portalda](https://portal.azure.com) oturum açın.

  1. Çalışma alanını açın.

  1. Git **dağıtımları** -> **hizmet Seç** -> **Düzenle**.

     ![Hizmet Düzenle](media/how-to-enable-data-collection/EditService.png)

  1. İçinde **Gelişmiş ayarlar**, seçimini **etkinleştirme Model veri koleksiyonu**. 

     ![Veri Toplama'seçeneğinin işaretini kaldırın](media/how-to-enable-data-collection/UncheckDataCollection.png) 

  1. Seçin **güncelleştirme** değişikliği uygulamak için.

* Seçenek 2 - veri toplama devre dışı bırakmak için Python kullanın:

  ```python 
  ## replace <service_name> with the name of the web service
  <service_name>.update(collect_model_data=False)
  ```

## <a name="example-notebook"></a>Örneğin not defteri

`00.Getting Started/12.enable-data-collection-for-models-in-aks.ipynb` Not Defteri, bu makaledeki kavramları göstermektedir.  

Bu not alın:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]