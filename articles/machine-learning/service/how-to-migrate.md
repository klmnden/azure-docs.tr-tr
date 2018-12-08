---
title: Workbench uygulamasını geçirme
titleSuffix: Azure Machine Learning service
description: Yükseltme veya önceki bir sürümden en geç Azure Machine Learning hizmeti sürümüne geçirme konusunda bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: haining
author: haining
ms.date: 09/24/2018
ms.openlocfilehash: cc60fd6a9d5f154d26fc9c495f190296453a0db0
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53106660"
---
# <a name="migrate-from-workbench-to-the-latest-version-of-azure-machine-learning-service"></a>Azure Machine Learning hizmetinin en son sürüme Workbench'ten geçirme 

**Workbench uygulamasının yüklü olması ve/veya deneme ve model Yönetimi Önizleme hesapları, en son sürüme geçirmek için bu makaleyi kullanın.**  Önizleme Workbench yüklenmiş veya bir deneme ve/veya model Yönetimi hesabı yoksa, herhangi bir şey geçirme gerekmez.

## <a name="what-can-i-migrate"></a>Hangi geçirebilirim?
Azure Machine Learning hizmeti ilk önizleme sürümünde oluşturulan çoğu yapının kendi yerel depolanır veya Bulut depolama. Bu yapılar görünmez olur olmaz. Yapıtları geçirmek için yeniden güncelleştirilmiş Azure Machine Learning hizmeti ile kaydedin. 

Aşağıdaki tablo ve makale, mevcut varlıklarınızı ve kaynakları önce veya sonra Azure Machine Learning hizmetinin en son sürüme taşınmadan yapabilecekleriniz açıklanmaktadır. Önceki sürümü ve varlıklarınızı süre kullanmaya devam edebilirsiniz ([destek zaman çizelgesi geçiş bkz](overview-what-happened-to-workbench.md#timeline)).

|Varlık veya eski sürümü kaynak|Geçişini sağlayabilir miyim?|Eylemler|
|-----------------|:-------------:|-------------|
|Makine öğrenimi modellerini (olarak yerel dosyaları)|Evet|Yok. Önceki gibi çalışır.|
|Model bağımlılıkları & şemaları (olarak yerel dosyaları)|Evet|Yok. Önceki gibi çalışır.|
|Projeler|Evet|[Yerel klasör eklemek için yeni bir çalışma alanı](#projects).|
|Çalıştırma geçmişleri|Hayır|[İndirilebilir](#history) bir süredir.|
|Veri dosyaları hazırlama|Hayır|[Herhangi bir boyut veri kümesi hazırlama](#dataprep) modelleme için yeni Azure Machine Learning Data Prep SDK'sını kullanarak veya Azure Databricks kullanın.|
|Hedef işlem|Hayır|Bunları yeni bir çalışma alanında kaydedin.|
|Kayıtlı modelleri|Hayır|Yeni bir çalışma alanı altında modeli yeniden kaydedin.|
|Kayıtlı bildirimleri|Hayır|Yok. Bildirimler artık yeni bir çalışma alanında bir kavram olarak.|
|Kayıtlı görüntüleri|Hayır|Yeni bir çalışma alanı altında dağıtım Docker görüntüsünü yeniden oluştur.|
|Dağıtılan web Hizmetleri|Hayır|Yok. Bunlar olarak çalışmaya devam-olduğu <br/>veya [en son sürümünü kullanarak yeniden dağıtmadan](#services).|
|Deneme ve <br/>Model Yönetimi hesapları|Hayır|[Çalışma alanı oluşturma](#resources) yerine.|
|Makine CLI & SDK öğrenme|Hayır|Yeni [CLI](reference-azure-machine-learning-cli.md) ve [SDK](https://aka.ms/aml-sdk) yeni çalışma.|


Daha fazla bilgi edinin [bu sürümdeki değişiklikler](overview-what-happened-to-workbench.md)?

>[!Warning]
>Bu makalede, Azure Machine Learning Studio kullanıcılar için değil. Workbench (Önizleme) uygulamasının yüklü olması ve/veya deneme ve model Yönetimi Önizleme hesapları Azure Machine Learning hizmeti müşteriler için var.

<a name="resources"></a>

## <a name="azure-resources"></a>Azure kaynakları

İşlem kaynakları, deneme hesapları, model yönetim hesapları ve machine learning gibi ortamları üzerinden Azure Machine Learning hizmetinin en son sürüme geçirilemez. Bkz: [zaman çizelgesi](overview-what-happened-to-workbench.md#timeline) üzerinde ne kadar süreyle varlıklarınızı çalışmaya devam eder.

En son sürüm ile bir Azure Machine Learning hizmeti çalışma alanında oluşturmaya başlama [Azure portalında](quickstart-get-started.md). Portalın çalışma alanı panosu yalnızca Edge, Chrome ve Firefox tarayıcılarında desteklenir.

Bu yeni çalışma alanı, üst düzey hizmet kaynak ve Azure Machine Learning hizmetinin en son özelliklerin tümünü kullanmanıza olanak tanır. Bu konu hakkında daha fazla bilgi edinin [çalışma ve mimari](concept-azure-machine-learning-architecture.md).

<a name="projects"></a>

## <a name="projects"></a>Projeler

Projelerinizi bulutta bir çalışma alanında olması yerine projeleri artık en son sürümü yerel makinenizde dizinlerdir. Bir diyagramı bkz [son mimarisi](concept-azure-machine-learning-architecture.md). 

Dosyaları ve komut dosyalarını içeren yerel dizine kullanmaya devam etmek için dizinin adını belirtin. ['experiment.submit'](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) Python komutunu ya da 'az ml proje ekleme' CLI komutunu kullanarak.

Örneğin:
```python
run = exp.submit(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
```

<a name="services"></a>

## <a name="deployed-web-services"></a>Dağıtılan web Hizmetleri

Web hizmetlerini geçirmek için yeni dağıtım hedefleri yeni CLI veya SDK'sını kullanarak Modellerinizi yeniden dağıtın. Özgün Puanlama dosyası, model dosyası bağımlılıkları dosyaları, ortam dosyası ve şema dosyalarını değiştirmenize gerek yoktur. 

En son sürümde modelleri web hizmetleri için Azure Container Instances'a (ACI) veya Azure Kubernetes Service (AKS) kümesi olarak dağıtılır. 

Bu makalelerde daha fazla bilgi edinin:
+ [Nasıl dağıtılacağı ve nerede](how-to-deploy-and-where.md)
+ [Öğretici: Azure Machine Learning hizmeti ile modelleri dağıtma](tutorial-deploy-models-with-aml.md)

Zaman [önceki CLI uçlar için destek](overview-what-happened-to-workbench.md#timeline), Model Yönetimi hesabınız ile ilk olarak dağıtılan web hizmetleri yönetmek mümkün olmayacaktır. Ancak, bu web hizmetleri için Azure Container Service (ACS) hala destekleniyor sürece çalışmaya devam eder.

<a name="history"></a>

## <a name="run-history-records"></a>Çalıştırma geçmişi kayıtları

Eski çalışma alanı altında mevcut çalıştırma geçmişleri eklemek devam edemiyor, ancak önceki CLI kullanarak sahip geçmişleri dışarı aktarabilirsiniz. Zaman [önceki CLI uçlar için destek](overview-what-happened-to-workbench.md#timeline), artık bu çalıştırma geçmişleri dışarı aktarmak mümkün olmayacaktır.

Modellerinizde eğitim ve yeni CLI ve SDK'sını kullanarak çalıştırma geçmişleri izleme başlatın. Şunları öğrenebilirsiniz nasıl [öğretici: Azure Machine Learning hizmeti ile modellerinin eğitilmesi](tutorial-train-models-with-aml.md).

Önceki CLI ile çalıştırma geçmişini dışarı aktarmak için:

```azurecli
#list all runs
az ml history list

#get details about a particular run
az ml history info

# download all artifacts of a run
az ml history download
```

<a name="dataprep"></a>

## <a name="data-preparation-files"></a>Veri hazırlama dosyası
Veri hazırlama dosyası Workbench taşınabilir değildir. Ancak yine de yeni Azure Machine Learning veri hazırlığı SDK'sını kullanarak modellemek için herhangi bir boyut veri kümesi hazırlama veya büyük veri kümeleri için Azure Databricks kullanabilirsiniz.  [Veri hazırlığı SDK almayı öğrenme](how-to-data-prep.md). 

## <a name="next-steps"></a>Sonraki adımlar

Çalışma alanı oluşturma, bir proje oluşturun, bir komut dosyasını çalıştırın ve betiğin çalıştırma geçmişini Azure Machine Learning hizmetinin en son sürümle keşfedin nasıl gösteren Hızlı Başlangıç için deneyin [AzureMachineLearninghizmetiileçalışmayabaşlama](quickstart-get-started.md).

Bu iş daha ayrıntılı bir deneyim için eğitim ve Azure Machine Learning hizmeti ile modelleri dağıtmak için ayrıntılı adımları içeren eksiksiz öğreticide izleyin. 

> [!div class="nextstepaction"]
> [Öğretici: Eğitme ve modelleri dağıtma](tutorial-train-models-with-aml.md)
