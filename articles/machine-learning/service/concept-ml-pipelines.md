---
title: 'İşlem hatları: machine learning iş akışı en iyi duruma getirme'
titleSuffix: Azure Machine Learning service
description: Bu makalede, makine öğrenimi işlem hatları için Python ve işlem hatlarını kullanmanın avantajları Azure Machine Learning SDK ile birlikte derleme hakkında bilgi edinin. Makine öğrenimi (ML) işlem hatları, veri bilimciler tarafından makine öğrenimi iş akışlarını derlemek, iyileştirmek ve yönetmek için kullanılır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: sanpil
author: sanpil
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: f9049e22d15dacb91e86fd0ba623d69c9d17c789
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024818"
---
# <a name="build-machine-learning-pipelines-with-the-azure-machine-learning-service"></a>Machine learning işlem hatlarını Azure Machine Learning hizmeti ile derleme

Bu makalede, makine öğrenimi işlem hatları Python için Azure Machine Learning SDK ve işlem hatlarını kullanmanın avantajları oluşturabileceğinizi öğrenin.

## <a name="what-are-machine-learning-pipelines"></a>Machine learning işlem hatlarını nelerdir?

Machine learning (ML) işlem hatları, veri bilimcileri, veri mühendisleri ve BT uzmanları kullanarak içinde yer alan adımların üzerinde işbirliği yapabilirsiniz:
+ Normalizations ve dönüştürmeler gibi veri hazırlama
+ Modeli eğitimi
+ Modeli değerlendirme
+ Dağıtım 

Aşağıdaki diyagramda bir örnek işlem hattı gösterilmektedir:

![Machine learning hizmetinde Azure Machine Learning işlem hatları](./media/concept-ml-pipelines/pipelines.png)

### <a name="which-azure-pipeline-technology-should-i-use"></a>Hangi Azure işlem hattı teknoloji kullanmalıyım?

Azure bulut birkaç diğer işlem hatlarında, her biri farklı bir amaç sağlar. Aşağıdaki tabloda, farklı işlem hatları ve ne için kullanıldıkları listelenmektedir:

| İşlem hattı | Ne yapar? | Kurallı kanal |
| ---- | ---- | ---- |
| Azure Machine Learning işlem hatları | Yeniden kullanılabilir makine öğrenme makineniz senaryoları öğrenmek için bir şablon olarak kullanılabilecek iş akışlarını tanımlar. | Veri modeli -> |
| [Azure Data Factory işlem hatları](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) | Grupları veri taşıma ve dönüştürme gibi aynı denetim etkinlikleri bir görevi gerçekleştirmek gerekli.  | Verileri veri -> |
| [Azure işlem hatları](https://azure.microsoft.com/services/devops/pipelines/) | Sürekli tümleştirme ve teslim uygulamanızın herhangi bir platform/any bulut  | Kod -> uygulama/hizmet |

## <a name="why-build-pipelines-with-azure-machine-learning"></a>Neden Azure Machine Learning ile işlem hatları oluşturabilir?

Kullanabileceğiniz [Python için Azure Machine Learning SDK](#the-python-sdk-for-pipelines) gönderin ve tek bir işlem hattı çalıştırmaları izlemek için farklı ML işlem hatları, de oluşturmak için.

İşlem hattı ile akışınızı Basitlik, hız, taşınabilirlik ve yeniden iyileştirebilirsiniz. İşlem hattı Azure Machine Learning ile derleme yaparken, makine öğrenimi, uzmanlığınızı yerine altyapısı üzerinde odaklanabilirsiniz.

Farklı adımları kullanarak, yalnızca ince ve test iş akışınızı, gereken adımlarını yeniden çalıştıracaktır mümkün kılar. İşlem hattındaki bir hesaplama biriminde bir adımdır. Yukarıdaki şemada gösterildiği gibi veri hazırlama görevini birçok adımı gerektirebilir. Aşağıdaki adımları içerir ancak normalleştirme, dönüştürme, doğrulama ve özellik kazandırma sayesinde için sınırlı değildir. Veri kaynakları ve Ara verilerin hangi kaydeder zamandan ve kaynaklardan işlem işlem hattının arasında yeniden kullanılır. 

İşlem hattı tasarlandıktan sonra genellikle işlem hattının eğitim döngüsü daha ince ayar yoktur. Ne zaman bir işlem hattı, bir güncelleştirilmiş bir eğitim betiği gibi yeniden çalıştırmanız adımları çalışma atlar yeniden çalıştırın ve hangi değişmediğinden atlar. Aynı paradigma adımının yürütülmesi için kullanılan aynı komut için geçerlidir. 

Azure Machine Learning sayesinde, işlem hattındaki her adım için çeşitli araç setleri ve PyTorch veya TensorFlow, gibi çerçeveleri kullanabilirsiniz. Çeşitli arasında Azure koordinatları [hedefleri işlem](concept-azure-machine-learning-architecture.md) aşağı akış işlem hedefleriyle Ara verilerinizi kolayca paylaşılabilir böylece, kullanın. 

Yapabilecekleriniz [Ölçümleriyle için işlem hattı denemelerinizi](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiments) doğrudan Azure portalında. 

## <a name="key-advantages"></a>Başlıca avantajları

Makine öğrenimi iş akışları için işlem hatları oluşturmaya önemli avantajlar şunlardır:

|Önemli bir avantajı|Açıklama|
|:-------:|-----------|
|**Katılımsız&nbsp;çalıştırır**|Birkaç adımı güvenilir bir şekilde ve katılımsız olarak paralel ya da sırayla çalışacak şekilde zamanlayın. Çünkü veri hazırlığı ve model için son günler veya haftalar, işlem hattınızı çalışırken diğer görevlere artık odaklanabilirsiniz. |
|**Karışık ve çok çeşitli bilgi işlem**|Heterojen ve ölçeklenebilir hesaplar ve depolamayı arasında güvenilir bir şekilde koordine edilen birden çok işlem hatlarını kullanın. HDInsight, GPU veri bilimi Vm'lerini ve Databricks gibi farklı işlem hedeflerde tek bir işlem hattı adımları çalıştırabilirsiniz. Bu, kullanılabilir işlem seçenekleri verimli bir şekilde kullanılmasını sağlar.|
|**Yeniden kullanılırlığı**|Yeniden eğitme ve toplu Puanlama gibi belirli senaryoları için işlem hatları templatize. Basit REST çağrılarını aracılığıyla dış sistemlerden bunları tetikler.|
|**İzleme ve sürüm oluşturma**|Yineleme verileri ve sonuç el ile izlemek yerine yolları SDK işlem hatları açıkça adlandırmak için kullanın ve sürüm verilerinizi kaynakları, giriş ve çıkışları. Ayrıca, betikleri ve verileri üretkenliğinizi artırmak için ayrı olarak yönetebilir.|

## <a name="the-python-sdk-for-pipelines"></a>İşlem hatları için Python SDK'sı

ML işlem hatlarınızı oluşturmak için Python kullanın. Azure Machine Learning SDK'sı, sıralama ve hiçbir veri bağımlılık mevcut olduğunda işlem hatlarınızı adımları paralelleştirmek için zorunlu yapıları sunar. İle Jupyter not defterlerinde veya başka bir tercih ettiğiniz tümleşik geliştirme ortamında etkileşim kurabilir. 

Bildirim temelli veriler bağımlılıkları kullanarak, görevlerinizi en iyi duruma getirebilirsiniz. Veri aktarımı ve modeli yayımlama gibi ortak görevler için önceden oluşturulmuş modüllerinin bir çerçeve SDK'sı içerir. İşlem hatları üzerinde yeniden kullanılabilir özel adımları uygulayarak kendi kurallarınız model için framework genişletebilirsiniz. Ayrıca doğrudan SDK'dan işlem hedefleri ve depolama kaynaklarını yönetebilirsiniz.

İşlem hatları şablon olarak kaydedin ve toplu Puanlama veya yeniden eğitme işleri zamanlamak için de bunları bir REST uç noktasına dağıtın.

Kendi yapı öğrenmek için [işlem hatları için Python SDK başvuru belgeleri](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) ve sonraki bölümde Not.

## <a name="example-notebooks"></a>Örnek Not Defterleri
 
Azure Machine Learning işlem hatlarında aşağıdaki not defterlerini göstermektedir: [how-to-use-azureml/machine-learning-pipelines](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines).
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [ilk işlem hattınızı oluşturma](how-to-create-your-first-pipeline.md).
