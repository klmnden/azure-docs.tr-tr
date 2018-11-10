---
title: Azure Machine Learning hizmeti nedir?
description: Bulutta makine öğrenimine ilişkin temel kavramları açıklar, bunu ne için kullanabileceğinizi anlatır ve makine öğrenimi terimlerini tanımlar. Uzman veri bilimcilerinin bulut ölçeğinde gelişmiş analiz uygulamaları geliştirmesini, üzerinde deneme yapmasını ve dağıtmasını sağlayan tümleşik, uçtan uca veri bilimi çözümü olan Azure Machine Learning’e genel bakış.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: garyericson
ms.author: garye
ms.date: 09/24/2018
ms.openlocfilehash: 1dac11b8ad71a936b33742b52c95ac998176baf7
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51244959"
---
# <a name="what-is-azure-machine-learning-service-preview"></a>Azure Machine Learning hizmeti (önizleme) nedir?

Azure Machine Learning hizmeti (Önizleme), makine öğrenimi modelleri geliştirmek ve dağıtmak için kullanabileceğiniz bir bulut hizmetidir. Azure Machine Learning hizmetini kullanarak, modellerinizi bulutun sağladığı geniş ölçekte derleme, eğitme, dağıtma ve yönetme sırasında izleyebilirsiniz.

## <a name="what-is-machine-learning"></a>Machine learning nedir?

Makine öğrenimi; bilgisayarların var olan verileri kullanarak gelecekteki davranışları, sonuçları ve eğilimleri öngörmelerini sağlayan bir veri bilimi tekniğidir. Bilgisayarlar, makine öğrenimini kullanarak açıkça programlamaya gerek kalmadan öğrenir.

Makine öğreniminin öngörüleri veya tahminleri, uygulama ve cihazları daha akıllı hale getirir. Örneğin, çevrimiçi alışveriş yaparken makine öğrenimi önceden satın aldıklarınızı temel alarak beğenebileceğiniz diğer ürünleri önermede yardımcı olur. Veya kredi kartınız makineden geçirildiğinde, makine öğrenimi işlemi bir işlem veritabanıyla karşılaştırır ve sahtekarlıkların saptanmasına yardımcı olur. Elektrikli süpürge robotunuz bir odayı temizlediğinde ise, makine öğrenimi robotunuzun işin tamamlanıp tamamlanmadığına karar vermesine yardımcı olur.

## <a name="what-is-azure-machine-learning-service"></a>Azure Machine Learning hizmeti nedir?

Azure Machine Learning hizmeti, makine öğrenmesi modellerini geliştirmek, eğitmek, test etmek, dağıtmak, yönetmek ve izlemek için kullanabileceğiniz bulut tabanlı bir ortam sağlar.

[ ![Azure Machine Learning hizmeti iş akışı](./media/overview-what-is-azure-ml/aml.png) ] (./media/overview-what-is-azure-ml/aml.png#lightbox)

Azure Machine Learning hizmeti açık kaynak teknolojileri tam olarak destekler, bu nedenle TensorFlow ve scikit-learn gibi makine öğrenimi bileşenleri ile birlikte on binlerce açık kaynak Python paketi kullanabilirsiniz.
[Jupyter not defterleri](http://jupyter.org) veya [Visual Studio Code Tools for AI](https://visualstudio.microsoft.com/downloads/ai-tools-vscode/) gibi zengin araçlar, verileri etkileşimli olarak keşfetmeyi, dönüştürmeyi ve sonra geliştirip modelleri test etmeyi kolaylaştırır.
Azure Machine Learning hizmeti ayrıca kolaylık, verimlilik ve doğrulukla modeller oluşturmanıza yardımcı olan [model oluşturma ve ayarlama işlemini otomatik hale getiren](tutorial-auto-train-models.md) özellikler içerir.

Azure Machine Learning hizmeti, yerel makinenizde eğitimi başlatmanıza ve sonra ölçeği buluta genişletmenize olanak tanır. [Azure Batch AI](https://azure.microsoft.com/services/batch-ai/) yerel desteği ve [gelişmiş hiper parametre ayarlama hizmetleri](how-to-tune-hyperparameters.md) ile bulutun gücünü kullanarak daha iyi modelleri daha hızlı bir şekilde derleyebilirsiniz. 

Doğru modeli kullandığınızda, Docker gibi bir kapsayıcıya kolayca dağıtabilirsiniz. Bunun anlamı, [Azure Container Instances](how-to-deploy-to-aci.md) veya [Azure Kubernetes Service](how-to-deploy-to-aks.md)’e dağıtımın kolay olması veya kapsayıcıyı şirket içi veya bulut olmasına bakılmaksızın kendi dağıtımlarınızda kullanabilmenizdir.
Dağıtılan modelleri yönetebilir ve en uygun çözümü bulmak için denemeler yaparken birden fazla çalıştırmayı izleyebilirsiniz.

[!INCLUDE [aml-preview-note](../../../includes/aml-preview-note.md)]

## <a name="what-can-i-do-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile neler yapabilirim?

Azure Machine Learning hizmeti sizin için bir modeli otomatik olarak oluşturup otomatik olarak ayarlayabilir.
Bir örneği için bkz. [Öğretici: Azure Automated Machine Learning ile bir sınıflandırma modelini otomatik olarak eğitme](tutorial-auto-train-models.md).

Veya Python için Azure Machine Learning <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>’sını açık kaynaklı Python paketleri ile birlikte kullanarak, bir Azure Machine Learning hizmeti çalışma alanında yüksek oranda doğru makine öğrenimi ve derin öğrenme modellerini kendi kendinize derleyip eğitebilirsiniz.
Açık kaynaklı Python paketlerinde mevcut olan aşağıdaki gibi çok sayıda makine öğrenimi bileşeni arasından seçim yapabilirsiniz:

- <a href="http://scikit-learn.org/stable/" target="_blank">Scikit-learn</a>
- <a href="https://www.tensorflow.org" target="_blank">Tensorflow</a>
- <a href="https://pytorch.org" target="_blank">PyTorch</a>
- <a href="https://www.microsoft.com/en-us/cognitive-toolkit/" target="_blank">CNTK</a>
- <a href="http://mxnet.io" target="_blank">MXNet</a>

Bir model oluşturduktan sonra, test için yerel olarak dağıtılabilen bir kapsayıcı (Docker gibi) oluşturmak için ve sonra [Azure Container Instances](how-to-deploy-to-aci.md) veya [Azure Kubernetes Service](how-to-deploy-to-aks.md) içinde bir üretim web hizmeti olarak kullanabilirsiniz.

Daha sonra [Azure portal](https://portal.azure.com/) veya [Azure Machine Learning CLI uzantısını](reference-azure-machine-learning-cli.md) kullanarak dağıtılmış modellerinizi yönetebilirsiniz.
Model ölçümlerini değerlendirebilir, modelin yeni sürümlerini yeniden eğitip yeniden dağıtabilir, tüm bunları yaparken modelin denemelerini izleyebilirsiniz.

Azure Machine Learning hizmeti ile çalışmaya başlamak için aşağıdaki [Sonraki adımlara](#next-steps) bakın.

## <a name="how-is-azure-machine-learning-service-different-from-studio"></a>Azure Machine Learning hizmetinin Studio'dan farkı nedir?

Azure Machine Learning Studio, kod yazmaya gerek olmadan makine öğrenimi çözümlerini derleyebileceğiniz, test edebileceğiniz ve dağıtabileceğiniz, iş birliğine dayalı bir sürükle bırak görsel çalışma alanıdır. Önceden derlenmiş ve önceden yapılandırılmış makine öğrenimi algoritmaları ile veri işleme modüllerini kullanır.

Makine öğrenimi modelleri ile hızlı ve kolay bir şekilde deneme yapmak istediğinizde ve yerleşik makine öğrenimi algoritmaları çözümleriniz için yeterli olduğunda Machine Learning Studio’yu kullanın.

Bir Python ortamında çalışıyorsanız, makine öğrenimi algoritmalarınız üzerinde daha fazla denetime sahip olmak istiyorsanız veya açık kaynaklı makine öğrenimi kitaplıkları kullanmak istiyorsanız, Machine Learning hizmetini kullanın.

> [!NOTE]
> Azure Machine Learning Studio'da oluşturulan modeller, Azure Machine Learning hizmeti tarafından dağıtılamaz veya yönetilemez.

## <a name="free-trial"></a>Ücretsiz deneme sürümü
Abone değilseniz, [ücretsiz olarak bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). Azure hizmetlerinde harcayabileceğiniz krediler alırsınız. Krediler bittikten sonra hesabı tutabilir ve [ücretsiz Azure hizmetlerini](https://azure.microsoft.com/free/) kullanabilirsiniz. Açıkça ayarlarınızı değiştirip ücretlendirme istemediğiniz sürece kredi kartınız asla ücretlendirilmez. Alternatif olarak [MSDN abone avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): MSDN aboneliğiniz, ücretli Azure hizmetlerinizi kullanabildiğiniz her ay size kredi verir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalı kullanarak başlama](quickstart-get-started.md) makalesinden yararlanarak bir makine öğrenimi hizmeti çalışma alanı oluşturma
 
- Azure Machine Learning hizmeti ile modelleri eğitme ve dağıtma hakkında bilgi almak için [Azure Machine Learning ile bir görüntü sınıflandırma modelini eğitme](tutorial-train-models-with-aml.md) adlı tam öğreticiyi izleyin

- Azure Machine Learning’in modelinizi otomatik olarak oluşturup ayarlamasına izin verme hakkında bilgi için bkz. [Öğretici: Azure Automated Machine Learning ile bir sınıflandırma modelini otomatik olarak eğitme](tutorial-auto-train-models.md)

- Makine öğrenmesi senaryolarınızı derlemek, iyileştirmek ve yönetmek için [makine öğrenmesi işlem hatları](/azure/machine-learning/service/concept-ml-pipelines) hakkında bilgi edinin.

- Hizmete teknik ve ayrıntılı bir bakış için bkz. [Azure Machine Learning hizmeti mimarisi ve kavramları](concept-azure-machine-learning-architecture.md)

- Microsoft’un diğer makine öğrenimi ürünleri hakkında daha fazla bilgi için bkz. [Microsoft’un diğer makine öğrenimi ürünleri](./overview-more-machine-learning.md)


<!-- 

An intro to AML or an end-to-end quickstart video could go here.

In this 9-minute video, learn how you can benefit your app. You'll learn about key features and what a typical workflow looks like. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0-3 minutes covers key features and use-cases.
+ 3-4 minutes covers service provisioning. 
+ 4-6 minutes covers Import Data wizard used to create an index using the built-in real estate dataset.

-->
