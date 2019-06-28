---
title: AKS dağıtımlarda veri değişikliklerini (Önizleme) tespit etme
titleSuffix: Azure Machine Learning service
description: Azure Kubernetes hizmeti veri değişikliklerini zamanlı şekilde modeller Azure Machine Learning hizmetinde dağıtılan algılama hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: copeters
author: cody-dkdc
ms.date: 06/20/2019
ms.openlocfilehash: e4deeab28fb643ff32624ba9dd16574e621f508c
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67333268"
---
# <a name="how-to-detect-data-drift-preview-on-models-deployed-to-azure-kubernetes-service"></a>Veri değişikliklerini (Önizleme) için Azure Kubernetes Service tarafından dağıtılan modellerinde tespit etme
Bu makalede, için izleme hakkında bilgi edinin [veri değişikliklerini](concept-data-drift.md) dağıtılmış bir modelinin bir eğitim veri kümesi ve çıkarım verileri arasında. 

Veri değişikliklerini başlıca nedenlerdendir burada doğruluğu zamanla düşüyor biridir. Üretimde bir Modeli'ne sunulan veriler modeli eğitmek için kullanılan verileri farklı olduğunda ortaya çıkar. Azure Machine Learning hizmeti veri değişikliklerini algılayıcısı kullanarak veri değişikliklerini izleyebilirsiniz. Kayması algılanırsa, hizmet için bir uyarı gönder.  

> [!Note]
> Bu hizmeti (Önizleme) olduğu ve yapılandırma seçenekleri sınırlıdır. Lütfen bkz. bizim [API belgeleri](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/?view=azure-ml-py) ve [sürüm notları](azure-machine-learning-release-notes.md) ayrıntıları ve güncelleştirmeleri. 

Azure Machine Learning hizmeti ile AKS'de dağıtılmış bir modelinin girişleri izleyebilir ve bu veri modeli için eğitim kümesine karşılaştırın. Düzenli aralıklarla çıkarımı verilerdir [anlık görüntü ve profili](how-to-explore-prepare-data.md), ardından da temel veri kümesinde veri değişikliklerini çözümleme üretmek için hesaplanan: 

+ Kayması katsayısı adlı, veri değişikliklerini büyüklüğünü ölçer.
+ Ölçüler, hangi özelliklerin veri değişikliklerini neden bildiren özelliğiyle katkı verileri kayma.
+ Ölçüler ölçümleri uzaklığı. Şu anda Wasserstein ve enerji uzaklık hesaplanır.
+ Dağıtımları özelliklerinin ölçer. Şu anda çekirdek yoğunluğu tahmini ve çubuk.
+ Veri uyarılarını kayma e-posta ile gönderin.

Bu ölçümleri nasıl hesaplanır hakkında daha fazla bilgi için bkz [veri değişikliklerini kavramı](concept-data-drift.md) makalesi.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

- Bir Azure Machine Learning hizmeti çalışma alanında ve yüklü Python için Azure Machine Learning SDK'sı. Kullanarak şu önkoşul olarak gerekenleri edinin öğrenin [bir geliştirme ortamı yapılandırma](how-to-configure-environment.md) belge.

- [Ortamınızı ayarlama](how-to-configure-environment.md)ve ardından veri değişikliklerini SDK'sı aşağıdaki komutu kullanarak yükleyin:

    ```
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

- Ayarlanan [model veri toplayıcı](how-to-enable-data-collection.md) modelin AKS dağıtımdan verileri toplamak ve verilerini onaylamayı toplanır `modeldata` blob kapsayıcısı.

## <a name="import-dependencies"></a>İçeri aktarma bağımlılıkları 
Bu kılavuzda kullanılan bağımlılıkları içeri aktarın:

```python
# Azure ML service packages 
from azureml.core import Experiment, Run, RunDetails
from azureml.contrib.datadrift import DataDriftDetector, AlertConfiguration
``` 

## <a name="configure-data-drift"></a>Veri değişikliklerini yapılandırın 

Yapılandırma aşağıdaki Python örnek gösterir `DataDriftDetector` nesnesi:

```python
# if email address is specified, setup AlertConfiguration
alert_config = AlertConfiguration('your_email@contoso.com')

# create a new DatadriftDetector object
datadrift = DataDriftDetector.create(ws, model.name, model.version, services, frequency="Day", alert_config=alert_config)
    
print('Details of Datadrift Object:\n{}'.format(datadrift))
```

Daha fazla bilgi için [DataDrift](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/?view=azure-ml-py) başvuru.

## <a name="submit-a-datadriftdetector-run"></a>DataDriftDetector çalıştırma gönderin

Yapılandırılmış DataDriftDetector ile gönderdiğiniz bir [çalıştırma veri değişikliklerini](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift.datadriftdetector%28class%29?view=azure-ml-py#run-target-date--services--compute-target-name-none--create-compute-target-false--feature-list-none--drift-threshold-none-) modeli için belirli bir tarihte. 

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

## <a name="get-data-drift-analysis-results"></a>Veri değişikliklerini analiz sonuçlarını Al

Aşağıdaki Python örnek ilgili verileri kayması ölçümleri çizim gösterilmektedir. Döndürülen ölçümler, özel görselleştirmeler oluşturmak için kullanabilirsiniz:

```python
# start and end are datetime objects 
drift_metrics = datadrift.get_output(start_time=start, end_time=end)

# Show all data drift result figures, one per serivice.
# If setting with_details is False (by default), only the data drift magnitude will be shown; if it's True, all details will be shown.
drift_figures = datadrift.show(with_details=True)
```

![Veri değişikliklerini göster](media/how-to-monitor-data-drift/drift_show.png)

Hesaplanan ölçümleri hakkında daha fazla bilgi için bkz. [veri değişikliklerini kavramı](concept-data-drift.md) makalesi.

## <a name="schedule-data-drift-detection"></a>Zamanlama veri değişikliklerini algılama 

Veri değişikliklerini zamanlamasının etkinleştirilmesi, belirtilen sıklıkta çalıştırmak bir DataDriftDetector gerçekleştirir. Belirtilen eşiğin üstünde kayması katsayısı ise bir e-posta gönderilir. 

```python
datadrift.enable_schedule()
datadrift.disable_schedule()
```

Veri değişikliklerini algılayıcısı yapılandırmasını model Ayrıntıları sayfasında Azure Portalı'nda görülebilir.

![Azure portalında veri değişikliklerini yapılandırma](media/how-to-monitor-data-drift/drift_config.png)

## <a name="view-results-in-azure-ml-workspace-ui"></a>Azure ML çalışma alanı arabiriminde sonuçlarını görüntüle

Azure ML çalışma alanı Arabiriminde sonuçlarını görüntülemek için model sayfasına gidin. Modelin Ayrıntılar sekmesinde, veri değişikliklerini yapılandırması gösterilir. 'Veri değişikliklerini (Önizleme)' sekmesi, veri değişikliklerini ölçümlerin görselleştirildiği artık kullanılabilir. 

![Azure portalında veri kayması](media/how-to-monitor-data-drift/drift_ui.png)

## <a name="setting-up-alerts"></a>Uyarıları Ayarlama 

Uyarı verme eşiği ve bir e-posta adresi vererek kayması katsayısı ayarlayarak bir [Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/overview) kayması katsayısı eşiğin üzerindeyse, e-posta uyarı gönderilir. Tüm veri değişikliklerini ölçümleri özel uyarı veya Eylemler ayarlamak, Azure Machine Learning hizmeti çalışma alanıyla ilişkili app ınsights kaynağı depolanır. App ınsights sorgu, e-posta uyarısı bağlantıyı takip edebilirsiniz.

![Veri değişikliklerini e-posta uyarısı](media/how-to-monitor-data-drift/drift_email.png)

## <a name="next-steps"></a>Sonraki adımlar

* Veri değişikliklerini kullanarak bir tam örnek için bkz: [Azure ML veri kayma not defteri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/data-drift/azure-ml-datadrift.ipynb). Bu Jupyter not defteri kullanmayı gösterir. bir [Azure açık veri kümesi](https://docs.microsoft.com/azure/open-datasets/overview-what-are-open-datasets) hava durumunu tahmin etmek için bir modeli eğitmek için AKS'ye dağıtma ve izleme için veri değişikliklerini. 

* Veri değişikliklerini doğru genel kullanılabilirlik hareket ettikçe, soru, yorum veya öneriniz büyük ölçüde bildiriminizden. Aşağıdaki ürün geri bildirim düğmesini kullanın! 
