---
title: Machine learning işlem hatlarını - Azure Machine Learning hizmeti oluşturma
description: Bu makalede, makine öğrenimi işlem hatları için Python ve işlem hatlarını kullanmanın avantajları Azure Machine Learning SDK ile birlikte derleme hakkında bilgi edinin. Machine learning (ML) işlem hatları oluşturmak, en iyi duruma getirmek ve makine öğrenimi iş akışları yönetmek için veri uzmanları tarafından kullanılır.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: sanpil
author: sanpil
ms.date: 11/07/2018
ms.openlocfilehash: 099b59cde4ee438f16b9d7e77bd81c004006cb71
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51684877"
---
# <a name="pipelines-and-azure-machine-learning"></a>İşlem hatları ve Azure Machine Learning

Bu makalede, makine öğrenimi işlem hatları için Python ve işlem hatlarını kullanmanın avantajları Azure Machine Learning SDK ile birlikte derleme hakkında bilgi edinin.

## <a name="what-are-machine-learning-pipelines"></a>Machine learning işlem hatlarını nelerdir?

Machine learning (ML) işlem hatları, veri bilimcileri, veri mühendisleri ve BT uzmanları kullanarak içinde yer alan adımların üzerinde işbirliği yapabilirsiniz:
+ Normalizations ve dönüştürmeler gibi veri hazırlama
+ Modeli eğitimi
+ Modeli değerlendirme
+ Dağıtım 

Aşağıdaki diyagramda bir örnek işlem hattı gösterilmektedir:

[ ![Makine öğrenimi işlem hatları Azure Machine Learning hizmetindeki](./media/concept-ml-pipelines/pipelines.png) ] (. / media/concept-ml-pipelines/machine-learning-pipelines-big.png#lightbox)

## <a name="why-build-pipelines-with-azure-machine-learning"></a>Neden Azure Machine Learning ile işlem hatları oluşturabilir?

[Python için Azure Machine Learning SDK](#the-python-sdk-for-pipelines) gönderin ve tek bir işlem hattı çalıştırmaları izlemek için de ML işlem hatları oluşturmak için kullanılabilir.

İşlem hattı ile akışınızı Basitlik, hız, taşınabilirlik ve yeniden iyileştirebilirsiniz. İşlem hattı Azure Machine Learning ile derleme yaparken, en iyi bilgilerinize odaklanabilirsiniz &mdash; makine öğrenimi &mdash; altyapı yerine.

Farklı adımları kullanarak yalnızca ihtiyacınız ince ve iş akışınızı test adımlarını yeniden çalıştıracaktır mümkün kılar. İşlem hattındaki bir hesaplama biriminde bir adımdır. Yukarıdaki diyagramda gösterildiği gibi veri hazırlama görevini de dahil olmak üzere birçok adım içerebilir, ancak normalleştirme, dönüştürme, doğrulama ve özellik kazandırma sayesinde için sınırlı değildir. Veri kaynakları ve Ara verilerin hangi kaydeder zamandan ve kaynaklardan işlem işlem hattının arasında yeniden kullanılır. 

İşlem hattı tasarlanmıştır sonra genellikle işlem hattının eğitim döngüsü daha ince ayar yoktur. Ne zaman bir işlem hattı, bir güncelleştirilmiş bir eğitim betiği gibi yeniden çalıştırmanız adımları çalışma atlar yeniden çalıştırın ve hangi değişmediğinden atlar. Aynı paradigma adımının yürütülmesi için kullanılan aynı komut için geçerlidir. 

Azure Machine Learning ile çeşitli araç setleri ve Microsoft Bilişsel Araç Seti veya TensorFlow gibi çerçeveleri her adım için işlem hattınızda kullanabilirsiniz. Çeşitli arasında Azure koordinatları [hedefleri işlem](concept-azure-machine-learning-architecture.md) aşağı akış işlem hedefleriyle Ara verilerinizi kolayca paylaşılabilir böylece kullanırsınız. 

Yapabilecekleriniz [Ölçümleriyle için işlem hattı denemelerinizi](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiments) doğrudan Azure portalında. 

## <a name="key-advantages"></a>Başlıca avantajları

Başlıca avantajları işlem hatları için makine öğrenimi iş akışları oluşturmak için aşağıdaki gibidir:

|Önemli bir avantajı|Açıklama|
|:-------:|-----------|
|**Katılımsız&nbsp;çalıştırır**|Paralel veya sıralı güvenilir ve katılımsız bir şekilde çalışması için birkaç adım zamanlayın. İşlem hattınızı çalışırken veri hazırlığı ve modelleme beri son gün veya hafta, artık diğer görevlere odaklanabilirsiniz. |
|**Karışık ve çok çeşitli bilgi işlem**|Heterojen ve ölçeklenebilir hesaplar ve depolamayı arasında güvenilir bir şekilde koordine edilen birden çok işlem hatlarını kullanın. Tek bir işlem hattı adımları hedefler, HDInsight GPU veri bilimi sanal makineleri ve gibi Databricks, verimli hale getirmek için kullanılabilir işlem seçenekleri kullanın, farklı işlem üzerinde çalıştırılabilir.|
|**Yeniden kullanılırlığı**|Yeniden eğitme ve toplu Puanlama gibi belirli senaryoları için işlem hatları şablonlaştırılacak.  Basit REST çağrılarını aracılığıyla dış sistemlerden tetiklenebilir.|
|**İzleme ve sürüm oluşturma**|Yineleme verileri ve sonuç el ile izlemek yerine yolları SDK işlem hatları açıkça adlandırmak için kullanın ve sürüm verilerinizi kaynakları, giriş ve çıkışları yanı sıra betikleri ve verileri üretkenliğinizi artırmak için ayrı olarak yönetme.|

## <a name="the-python-sdk-for-pipelines"></a>İşlem hatları için Python SDK'sı

ML işlem hatlarınızı oluşturmak için Python kullanın. Azure Machine Learning SDK'sı, sıralama ve hiçbir veri bağımlılık mevcut olduğunda işlem hatlarınızı adımları paralelleştirmek için zorunlu yapıları sunar. İle Jupyter not defterlerinde veya başka bir tercih edilen IDE'de etkileşim kurabilir. 

Bildirim temelli veriler bağımlılıkları kullanarak, görevlerinizi en iyi duruma getirebilirsiniz. Veri aktarımı ve modeli yayımlama gibi ortak görevler için önceden oluşturulmuş modüllerinin bir çerçeve SDK'sı içerir. Framework, işlem hatları üzerinde yeniden kullanılabilir özel adımları uygulayarak kendi kurallarınız modellemek için genişletilebilir. Ayrıca işlem hedefleri ve depolama kaynaklarını doğrudan SDK'dan yönetilebilir.

İşlem hatları, şablon olarak kaydedilebilir ve toplu Puanlama veya yeniden eğitme işleri zamanlamak için de bir REST uç noktasına dağıtılabilir.

Kullanıma [işlem hatları için Python SDK başvuru belgeleri](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) ve kendi yapı öğrenmek için sonraki bölümde Not.

## <a name="example-notebooks"></a>Örnek Not Defterleri
 
Azure Machine Learning işlem hatlarında aşağıdaki Not Defteri gösterir: [işlem hattı/işlem hattı-batch-scoring.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/pipeline/pipeline-batch-scoring.ipynb).
 
Bu not alın:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
