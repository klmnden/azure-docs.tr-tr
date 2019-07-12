---
title: AKS dağıtımlarda, (Önizleme) veri değişikliklerini Algıla
titleSuffix: Azure Machine Learning service
description: Azure Kubernetes hizmeti veri değişikliklerini zamanlı şekilde modeller Azure Machine Learning hizmetinde dağıtılan algılayın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: copeters
author: cody-dkdc
ms.date: 07/08/2019
ms.openlocfilehash: 3b8152bde8b7e44dde1b0b9c82216333778f83da
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67806016"
---
# <a name="detect-data-drift-preview-on-models-deployed-to-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) için dağıtılan modellerinde veri değişikliklerini (Önizleme) algılama

Bu makalede, dağıtılmış bir modelinin veri çıkarımı ve eğitim veri kümesi arasında veri değişikliklerini izlemek nasıl öğrenin. Machine learning bağlamında, eğitilen makine öğrenimi modellerini düşürülmüş tahmini Performans nedeniyle kayması karşılaşabilirsiniz. Azure Machine Learning hizmeti ile veri değişikliklerini izleyebilirsiniz ve kaymaları algılandığında hizmet bir e-posta uyarısı size gönderebilir.

## <a name="what-is-data-drift"></a>Veri değişikliklerini nedir?

Veri değişikliklerini bir Modeli'ne üretimde sunulan veriler modeli eğitmek için kullanılan verileri farklı olduğunda gerçekleşir. Başlıca nedenlerdendir burada doğruluğu zamanla, düşüyor biridir kayması modeli performans sorunlarını algılamaya yardımcı olur böylece izleme verileri. 

## <a name="what-can-i-monitor"></a>Ne izleyebilirim?

Azure Machine Learning hizmeti ile AKS'de dağıtılmış bir modelinin girişleri izleyebilir ve bu veri modeli için eğitim kümesine karşılaştırın. Düzenli aralıklarla çıkarımı verilerdir [anlık görüntü ve profili](how-to-explore-prepare-data.md), ardından da temel veri kümesinde veri değişikliklerini çözümleme üretmek için hesaplanan: 

+ Kayması katsayısı adlı, veri değişikliklerini büyüklüğünü ölçer.
+ Ölçüler, hangi özelliklerin veri değişikliklerini neden bildiren özelliğiyle katkı verileri kayma.
+ Ölçüler ölçümleri uzaklığı. Şu anda Wasserstein ve enerji uzaklık hesaplanır.
+ Dağıtımları özelliklerinin ölçer. Şu anda çekirdek yoğunluğu tahmini ve çubuk.
+ Veri uyarılarını kayma e-posta ile gönderin.

> [!Note]
> Bu hizmeti (Önizleme) olduğu ve yapılandırma seçenekleri sınırlıdır. Lütfen bkz. bizim [API belgeleri](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/?view=azure-ml-py) ve [sürüm notları](azure-machine-learning-release-notes.md) ayrıntıları ve güncelleştirmeleri. 

### <a name="how-data-drift-is-monitored-in-azure-machine-learning-service"></a>Azure Machine Learning hizmeti veri değişikliklerini nasıl izlenir

Azure Machine Learning hizmetini kullanarak, veri değişikliklerini veri kümeleri veya dağıtımları ile izlenir. İçin veri değişikliklerini izlemek için bir temel veri kümesi - genellikle eğitim veri kümesi için bir model - belirtilir. İkinci bir veri kümesi - genellikle bir dağıtımdan - model giriş verileri toplanan temel veri kümesinde sınanır. Her iki veri kümelerinin olduğunu [profili](how-to-explore-prepare-data.md#explore-with-summary-statistics) ve giriş verileri için farklı izleme hizmeti. İki veri kümesi arasındaki farkları algılamak için makine öğrenme modeli eğitilir. Modelin performans değişikliklerini katsayısı, iki veri kümesi arasında kayması büyüklüğünü ölçer dönüştürülür. Kullanarak [model interpretability](machine-learning-interpretability-explainability.md), kayması katsayısı için katkıda bulunan özellikler hesaplanır. Veri kümesi profilinden her özellik hakkında istatistiksel bilgi izlenir. 

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Yoksa, başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

- Bir Azure Machine Learning hizmeti çalışma alanında ve yüklü Python için Azure Machine Learning SDK'sı. Bölümündeki yönergeleri kullanın [bir Azure Machine Learning hizmeti çalışma alanı oluşturma](setup-create-workspace.md#sdk) aşağıdakileri yapmak için:

    - Miniconda ortamı oluşturma
    - Azure Machine için Python SDK'sı Learning yükleme
    - Çalışma alanı oluşturma
    - Bir çalışma alanı yapılandırma dosyası (aml_config/config.json) yazın.

- Veri değişikliklerini SDK'sı aşağıdaki komutu kullanarak yükleyin:

    ```shell
    pip install azureml-contrib-datadrift
    ```

- Oluşturma bir [veri kümesi](how-to-create-register-datasets.md) modelinizin eğitim verileri.

- Eğitim veri kümesi belirtmek, [kaydetme](concept-model-management-and-deployment.md) modeli. Aşağıdaki örneği kullanarak göstermektedir `datasets` parametresi eğitim veri kümesi belirtmek için:

    ```python
    model = Model.register(model_path=model_file,
                        model_name=model_name,
                        workspace=ws,
                        datasets=datasets)

    print(model_name, image_name, service_name, model)
    ```

- [Model verisi toplamayı etkinleştir](how-to-enable-data-collection.md) modelin AKS dağıtımdan verileri toplamak ve verilerini onaylamayı toplanır `modeldata` blob kapsayıcısı.

## <a name="configure-data-drift"></a>Veri değişikliklerini yapılandırın
Denemeniz için veri değişikliklerini yapılandırmak için bağımlılıklar aşağıdaki Python örnekte görüldüğü gibi içeri aktarın. 

Bu örnek yapılandırma gösterir [ `DataDriftDetector` ](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift.datadriftdetector.datadriftdetector?view=azure-ml-py) nesnesi:

```python
# Import Azure ML packages
from azureml.core import Experiment, Run, RunDetails
from azureml.contrib.datadrift import DataDriftDetector, AlertConfiguration

# if email address is specified, setup AlertConfiguration
alert_config = AlertConfiguration('your_email@contoso.com')

# create a new DatadriftDetector object
datadrift = DataDriftDetector.create(ws, model.name, model.version, services, frequency="Day", alert_config=alert_config)
    
print('Details of Datadrift Object:\n{}'.format(datadrift))
```

## <a name="submit-a-datadriftdetector-run"></a>DataDriftDetector çalıştırma gönderin

İle `DataDriftDetector` nesne, yapılandırılmış gönderebildiği bir [çalıştırma veri değişikliklerini](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift.datadriftdetector%28class%29?view=azure-ml-py#run-target-date--services--compute-target-name-none--create-compute-target-false--feature-list-none--drift-threshold-none-) modeli için belirli bir tarihte. Çalıştırma işleminin bir parçası olarak ayarlayarak DataDriftDetector uyarıları etkinleştirin `drift_threshold` parametresi. Varsa [datadrift_coefficient](#metrics) üzerinde belirli `drift_threshold`, e-posta gönderilir.

```python
# adhoc run today
target_date = datetime.today()

# create a new compute - creates datadrift-server
run = datadrift.run(target_date, services, feature_list=feature_list, create_compute_target=True)

# or specify existing compute cluster
run = datadrift.run(target_date, services, feature_list=feature_list, compute_target='cpu-cluster')

# show details of the data drift run
exp = Experiment(ws, datadrift._id)
dd_run = Run(experiment=exp, run_id=run)
RunDetails(dd_run).show()
```

## <a name="visualize-drift-metrics"></a>Kayması ölçümleri görselleştirin

<a name="metrics"></a>

Çalıştırın, DataDriftDetector gönderdikten sonra veri değişikliklerini görev çalıştırma her yinelemede kaydedilen kayması ölçümlerini görebilirsiniz:


|Ölçüm|Açıklama|
--|--|
wasserstein_distance|Tek boyutlu sayısal dağıtım için tanımlanan istatistiksel uzaklığı.|
energy_distance|Tek boyutlu sayısal dağıtım için tanımlanan istatistiksel uzaklığı.|
datadrift_coefficient|Benzer şekilde Matthew'ın korelasyon katsayısı olarak hesaplanan, ancak bu çıkış gerçek 0 ile 1 arasında bir sayı. Kayması bağlamında, 0 hiçbir kayması gösterir ve 1, en fazla kayması gösterir.|
datadrift_contribution|Katkıda bulunan farklı özellikler önemini özelliği.|

Kayması ölçümleri görüntülemek için birden çok yolu vardır:

* Jupyter pencere öğesi kullanın.
* Kullanım [ `get_metrics()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py#get-metrics-name-none--recursive-false--run-type-none--populate-false-) herhangi işlevi `datadrift` nesne çalıştırın.
* Modelinizde Azure portalında ölçümleri görüntüleyin

Aşağıdaki Python örnek ilgili verileri kayması ölçümleri çizim gösterilmektedir. Döndürülen ölçümler, özel görselleştirmeler oluşturmak için kullanabilirsiniz:

```python
# start and end are datetime objects 
drift_metrics = datadrift.get_output(start_time=start, end_time=end)

# Show all data drift result figures, one per serivice.
# If setting with_details is False (by default), only the data drift magnitude will be shown; if it's True, all details will be shown.
drift_figures = datadrift.show(with_details=True)
```

![Azure Machine Learning tarafından algılanan veri değişikliklerini bakın](media/how-to-monitor-data-drift/drift_show.png)


## <a name="schedule-data-drift-scans"></a>Zamanlama veri değişikliklerini taramalar 

Veri değişikliklerini algılama etkinleştirdiğinizde belirtilen, zamanlanmış sıklığında bir DataDriftDetector çalıştırılır. Datadrift_coefficient ulaşırsa verilen `drift_threshold`, zamanlanmış her çalıştırmasıyla bir e-posta gönderilir. 

```python
datadrift.enable_schedule()
datadrift.disable_schedule()
```

Veri değişikliklerini algılayıcısı yapılandırmasını model Ayrıntıları sayfasında Azure Portalı'nda görülebilir.

![Azure portalında veri değişikliklerini yapılandırma](media/how-to-monitor-data-drift/drift_config.png)

## <a name="view-results-in-azure-ml-workspace-ui"></a>Azure ML çalışma alanı arabiriminde sonuçlarını görüntüle

Azure ML çalışma alanı Arabiriminde sonuçlarını görüntülemek için model sayfasına gidin. Modelin Ayrıntılar sekmesinde, veri değişikliklerini yapılandırması gösterilir. 'Veri değişikliklerini (Önizleme)' sekmesi, veri değişikliklerini ölçümlerin görselleştirildiği artık kullanılabilir. 

![Azure portalında veri kayması](media/how-to-monitor-data-drift/drift_ui.png)

## <a name="receiving-drift-alerts"></a>Alıcı kayması uyarıları

Uyarı verme eşiği ve bir e-posta adresi sağlayarak kayması katsayısı ayarlayarak bir [Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/overview) e-posta uyarısı değişikliklerini katsayısı eşiğin üstünde kaldığında otomatik olarak gönderilir. 

Tüm veri değişikliklerini ölçümleri depolanır, özel uyarıları ve eylemleri ayarlamak sırayla [Application Insights](how-to-enable-app-insights.md) yanı sıra Azure Machine Learning hizmeti çalışma oluşturulduğu kaynak. Bağlantıyı e-posta uyarısı için Application Insights sorgu izleyebilirsiniz.

![Veri değişikliklerini e-posta uyarısı](media/how-to-monitor-data-drift/drift_email.png)

## <a name="retrain-your-model-after-drift"></a>Kayması sonra modelinizi yeniden eğitme

Veri değişikliklerini dağıtılan model performansını olumsuz etkiler, bu modeli yeniden eğitme zamanı geldi. Aşağıdaki [ `diff()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#diff-rhs-dataset--compute-target-none--columns-none-
) yöntemi eski ve yeni bir eğitim veri kümesi arasındaki değişiklikler ilk bir fikir verir. 

```python
from azureml.core import Dataset

old_training_dataset.diff(new_training_dataset)
```

Önceki kodun çıktısı alarak, modelinizi yeniden eğitme isteyebilirsiniz. Bunu yapmak için aşağıdaki adımlarla devam edin.

* Toplanan verileri araştırmak ve yeni modeli eğitmek için verileri hazırlama.
* Bu, eğitin ve test verileri bölün.
* Modelin yeni verileri kullanarak yeniden eğitin.
* Yeni oluşturulan model performansını değerlendirin.
* Üretim modelinden daha iyi performans ise yeni model dağıtma.

## <a name="next-steps"></a>Sonraki adımlar

* Veri değişikliklerini kullanarak bir tam örnek için bkz: [Azure ML veri kayma not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/data-drift/azure-ml-datadrift.ipynb). Bu Jupyter not defteri kullanmayı gösterir. bir [Azure açık veri kümesi](https://docs.microsoft.com/azure/open-datasets/overview-what-are-open-datasets) hava durumunu tahmin etmek için bir modeli eğitmek için AKS'ye dağıtma ve izleme için veri değişikliklerini. 

* Veri değişikliklerini doğru genel kullanılabilirlik hareket ettikçe, soru, yorum veya öneriniz büyük ölçüde bildiriminizden. Aşağıdaki ürün geri bildirim düğmesini kullanın! 
