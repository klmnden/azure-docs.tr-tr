---
title: Azure Machine Learning hizmeti nedir?
description: --Bir geliştirmek, Uzman veri bilimcilerine yönelik tümleşik, uçtan uca veri bilimi çözümü olan Azure Machine Learning hizmetine genel bakış, denemeler yapın ve bulut ölçeğinde Gelişmiş analiz uygulamaları dağıtın.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: garyericson
ms.author: garye
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: c3c0697af739c151f9aa7cbaed65283a365be7a7
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53077231"
---
# <a name="what-is-azure-machine-learning-service"></a>Azure Machine Learning hizmeti nedir?

Azure Machine Learning hizmeti eğitmek, dağıtma, otomatikleştirin ve tüm bulut sunuyor Geniş ölçekte makine öğrenimi modelleri yönetmek için kullanabileceğiniz bir bulut hizmetidir.

## <a name="what-is-machine-learning"></a>Machine learning nedir?

Makine öğrenimi; bilgisayarların var olan verileri kullanarak gelecekteki davranışları, sonuçları ve eğilimleri öngörmelerini sağlayan bir veri bilimi tekniğidir. Bilgisayarlar, makine öğrenimini kullanarak açıkça programlamaya gerek kalmadan öğrenir.

Makine öğreniminin öngörüleri veya tahminleri, uygulama ve cihazları daha akıllı hale getirir. Örneğin, çevrimiçi alışveriş yaparken makine öğrenimi önceden satın aldıklarınızı temel alarak beğenebileceğiniz diğer ürünleri önermede yardımcı olur. Veya kredi kartınız makineden geçirildiğinde, makine öğrenimi işlemi bir işlem veritabanıyla karşılaştırır ve sahtekarlıkların saptanmasına yardımcı olur. Elektrikli süpürge robotunuz bir odayı temizlediğinde ise, makine öğrenimi robotunuzun işin tamamlanıp tamamlanmadığına karar vermesine yardımcı olur.

## <a name="what-is-azure-machine-learning-service"></a>Azure Machine Learning hizmeti nedir?

Azure Machine Learning hizmeti, makine öğrenmesi modellerini geliştirmek, eğitmek, test etmek, dağıtmak, yönetmek ve izlemek için kullanabileceğiniz bulut tabanlı bir ortam sağlar.

[ ![Azure Machine Learning hizmeti iş akışı](./media/overview-what-is-azure-ml/aml.png) ] (./media/overview-what-is-azure-ml/aml.png#lightbox)

Azure Machine Learning hizmeti açık kaynak teknolojileri tam olarak destekler, bu nedenle TensorFlow ve scikit-learn gibi makine öğrenimi bileşenleri ile birlikte on binlerce açık kaynak Python paketi kullanabilirsiniz.
İçin zengin Araçlar, gibi destekleyen [Jupyter not defterleri](http://jupyter.org) veya [Visual Studio Code için Azure Machine Learning](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai#overview) uzantısı, etkileşimli olarak verileri araştırmak, dönüştürme ve ardından geliştirme ve test kolaylaştırır modeller.
Azure Machine Learning hizmeti ayrıca kolaylık, verimlilik ve doğrulukla modeller oluşturmanıza yardımcı olan [model oluşturma ve ayarlama işlemini otomatik hale getiren](tutorial-auto-train-models.md) özellikler içerir.

Azure Machine Learning hizmeti, yerel makinenizde eğitimi başlatmanıza ve sonra ölçeği buluta genişletmenize olanak tanır. Kullanılabilir birçok [hedefleri işlem](how-to-set-up-training-targets.md) Azure Machine Learning işlem gibi ve [Azure Databricks](/azure/azure-databricks/what-is-azure-databricks)ile [Hizmetleri ayarlama hiper parametre Gelişmiş](how-to-tune-hyperparameters.md), oluşturabileceğinizi modelleri, bulutun gücünü kullanarak daha hızlı, daha iyi.

Doğru modeli kullandığınızda, Docker gibi bir kapsayıcıya kolayca dağıtabilirsiniz. Bu, Azure Container Instances veya Azure Kubernetes hizmeti dağıtmak basit veya kendi dağıtımlarda, şirket içinde veya bulutta kapsayıcı kullanabileceğiniz anlamına gelir. Daha fazla bilgi için [nasıl dağıtılacağı ve nerede](how-to-deploy-and-where.md) belge.
Dağıtılan modelleri yönetebilir ve en uygun çözümü bulmak için denemeler yaparken birden fazla çalıştırmayı izleyebilirsiniz.
Modeliniz dağıtıldıktan sonra elde edilmesi de döndürebilir [gerçek zamanlı](how-to-consume-web-service.md), veya [zaman uyumsuz olarak](how-to-run-batch-predictions.md) büyük miktarlarda veri çubuğunda.

İle Gelişmiş [makine öğrenimi işlem hatları](concept-ml-pipelines.md), veri hazırlama, model eğitiminin ve değerlendirme ve dağıtım tüm adımları üzerinde işbirliği yapabilirsiniz.

## <a name="what-can-i-do-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile neler yapabilirim?

Azure Machine Learning hizmeti otomatik-bir modeli eğitme ve otomatik-bunu sizin için ayarlama.
Bir örneği için bkz. [Öğretici: Azure Automated Machine Learning ile bir sınıflandırma modelini otomatik olarak eğitme](tutorial-auto-train-models.md).

Azure Machine Learning kullanarak <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> Python, açık kaynak Python paketlerini yanı sıra yapı ve yüksek oranda doğru machine learning eğitimi ve derin öğrenme modelleri kendiniz bir Azure Machine Learning hizmeti çalışma alanında.
Açık kaynaklı Python paketlerinde mevcut olan aşağıdaki gibi çok sayıda makine öğrenimi bileşeni arasından seçim yapabilirsiniz:

- <a href="https://scikit-learn.org/stable/" target="_blank">Scikit-learn</a>
- <a href="https://www.tensorflow.org" target="_blank">Tensorflow</a>
- <a href="https://pytorch.org" target="_blank">PyTorch</a>
- <a href="https://www.microsoft.com/en-us/cognitive-toolkit/" target="_blank">CNTK</a>
- <a href="http://mxnet.io" target="_blank">MXNet</a>

Bir modeli oluşturduktan sonra yerel olarak test etmek için dağıtılabilen bir kapsayıcı (örneğin, Docker) oluşturmak için kullanın. Test tamamlandıktan sonra modeli bir üretim web hizmetine veya Azure Container Instances, hem de Azure Kubernetes hizmeti olarak dağıtılabilir. Daha fazla bilgi için [nasıl dağıtılacağı ve nerede](how-to-deploy-and-where.md) belge.

Kullanarak, dağıtılan modelleri ardından yönetebileceğiniz [Python için Azure Machine Learning SDK](https://aka.ms/aml-sdk) veya [Azure portalında](https://portal.azure.com/).
Modelin denemeleri tümünü havada modeli ölçümler, retrain ve yeniden dağıtma modeli yeni sürümlerini değerlendirebilirsiniz.

Azure Machine Learning hizmeti ile çalışmaya başlamak için aşağıdaki [Sonraki adımlara](#next-steps) bakın.

## <a name="how-is-azure-machine-learning-service-different-from-studio"></a>Azure Machine Learning hizmetinin Studio'dan farkı nedir?

Azure Machine Learning Studio, kod yazmaya gerek olmadan makine öğrenimi çözümlerini derleyebileceğiniz, test edebileceğiniz ve dağıtabileceğiniz, iş birliğine dayalı bir sürükle bırak görsel çalışma alanıdır. Önceden derlenmiş ve önceden yapılandırılmış makine öğrenimi algoritmaları ile veri işleme modüllerini kullanır.

Makine öğrenimi modelleri ile hızlı ve kolay bir şekilde deneme yapmak istediğinizde ve yerleşik makine öğrenimi algoritmaları çözümleriniz için yeterli olduğunda Machine Learning Studio’yu kullanın.

Bir Python ortamında çalışıyorsanız, makine öğrenimi algoritmalarınız üzerinde daha fazla denetime sahip olmak istiyorsanız veya açık kaynaklı makine öğrenimi kitaplıkları kullanmak istiyorsanız, Machine Learning hizmetini kullanın.

> [!NOTE]
> Azure Machine Learning Studio'da oluşturulan modeller, Azure Machine Learning hizmeti tarafından dağıtılamaz veya yönetilemez.

## <a name="free-trial"></a>Ücretsiz deneme sürümü
Abone değilseniz, [ücretsiz olarak bir Azure hesabı açabilirsiniz](https://aka.ms/amlfree). Azure hizmetlerinde harcayabileceğiniz krediler alırsınız. Krediler bittikten sonra hesabı tutabilir ve [ücretsiz Azure hizmetlerini](https://azure.microsoft.com/free/) kullanabilirsiniz. Açıkça ayarlarınızı değiştirip ücretlendirme istemediğiniz sürece kredi kartınız asla ücretlendirilmez. Alternatif olarak, [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F), ücretli Azure Hizmetleri, size için kullanabileceğiniz her ay kredi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Machine Learning hizmeti kullanmaya başlamak için çalışma alanı oluşturma [Azure portalını kullanarak](quickstart-get-started.md) veya [python'da](quickstart-create-workspace-with-python.md).

- Eksiksiz öğreticide izleyin [eğitme ve bir Azure Machine Learning ile görüntü sınıflandırma modeli dağıtma](tutorial-train-models-with-aml.md).

- [Otomatik olarak oluşturmak için Azure Machine Learning ve Otomatik Ayarla bir model](tutorial-auto-train-models.md).

- Makine öğrenmesi senaryolarınızı derlemek, iyileştirmek ve yönetmek için [makine öğrenmesi işlem hatları](/azure/machine-learning/service/concept-ml-pipelines) hakkında bilgi edinin.

- Ayrıntılı okuma [Azure Machine Learning hizmeti mimarisi ve kavramları](concept-azure-machine-learning-architecture.md) makalesi.

- Diğer Microsoft machine learning ürünleri hakkında daha fazla bilgi için bkz. [diğer makine öğrenimi ürünlerini Microsoft gelen](./overview-more-machine-learning.md).


<!-- 

An intro to AML or an end-to-end quickstart video could go here.

In this 9-minute video, learn how you can benefit your app. You'll learn about key features and what a typical workflow looks like. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0-3 minutes covers key features and use-cases.
+ 3-4 minutes covers service provisioning. 
+ 4-6 minutes covers Import Data wizard used to create an index using the built-in real estate dataset.

-->
