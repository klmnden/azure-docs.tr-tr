---
title: Nedir
titleSuffix: Azure Machine Learning service
description: -Bir geliştirmek, Uzman veri bilimcilerine yönelik tümleşik, uçtan uca veri bilimi çözümü olan Azure Machine Learning hizmetine genel bakış, denemeler yapın ve bulut ölçeğinde Gelişmiş analiz uygulamaları dağıtın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
author: j-martens
ms.author: jmartens
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 201ee251b195845e33ed3829be8540664811f2ab
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025309"
---
# <a name="what-is-azure-machine-learning-service"></a>Azure Machine Learning hizmeti nedir?

Azure Machine Learning hizmeti eğitmek, dağıtma, otomatikleştirin ve tüm bulut sunuyor Geniş ölçekte makine öğrenimi modelleri yönetmek için kullandığınız bir bulut hizmetidir.

## <a name="what-is-machine-learning"></a>Machine learning nedir?

Makine öğrenimi; bilgisayarların var olan verileri kullanarak gelecekteki davranışları, sonuçları ve eğilimleri öngörmelerini sağlayan bir veri bilimi tekniğidir. Machine learning kullanarak açıkça programlamaya gerek kalmadan bilgisayarlar öğrenin.

Makine öğreniminin öngörüleri veya tahminleri, uygulama ve cihazları daha akıllı hale getirir. Örneğin, çevrimiçi alışveriş yaparken makine öğrenimi, ne, satın aldığım üzerinde göre isteyebileceğiniz diğer ürünleri önermede yardımcı olur. Veya kredi kartınız makineden geçirildiğinde, makine öğrenimi işlemi bir işlem veritabanıyla karşılaştırır ve sahtekarlıkların saptanmasına yardımcı olur. Elektrikli süpürge robotunuz bir odayı temizlediğinde ise, makine öğrenimi robotunuzun işin tamamlanıp tamamlanmadığına karar vermesine yardımcı olur.

## <a name="what-is-azure-machine-learning-service"></a>Azure Machine Learning hizmeti nedir?

Azure Machine Learning hizmeti, veri hazırlama, eğitme, test, dağıtma, yönetme ve makine öğrenimi modelleri izlemek için kullanabileceğiniz bulut tabanlı bir ortam sağlar. Yerel makinenizde eğitim başlatın ve sonra ölçeği buluta genişletme. Hizmeti tamamen PyTorch, TensorFlow ve scikit gibi açık kaynak teknolojilerini destekler-bilgi edinin ve machine learning, derin öğrenme için Klasik ml ile her türlü kullanılabilir, denetimli ve Denetimsiz öğrenme. 

Keşfedin ve veri hazırlamasını, eğitin ve test modelleri ve bunları dağıtma gibi zengin araçlar kullanarak:
+ A [görsel arabirim](ui-quickstart-run-experiment.md) , denemelerinizi oluşturmak ve ardından dağıtmak için sürükle ve bırak modüllerini yapabilecekleriniz de modeller
+ [Jupyter not defterleri](https://jupyter.org) hangi kullanma [SDK'ları](https://docs.microsoft.com/azure/machine-learning/service/#reference) gibi kendi kodunuzu yazma için [Bu örnek Not Defterleri](https://aka.ms/aml-notebooks)
+ [Visual Studio Code uzantısı](how-to-vscode-tools.md)

## <a name="what-can-i-do-with-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile neler yapabilirim?

Kullanarak <a href="https://aka.ms/aml-sdk" target="_blank">Azure Machine Learning Python SDK</a> açık kaynak Python paketlerini veya kullanarak [görsel arabirim (Önizleme)](ui-quickstart-run-experiment.md), derleme ve yüksek oranda doğru makine öğrenimi ve derin öğrenme eğitin kendiniz bir Azure Machine Learning hizmetinde çalışma alanı modeller.

Açık kaynak Python paketlerini kullanılabilen birçok makine öğrenme bileşenler gibi seçebileceğiniz <a href="https://scikit-learn.org/stable/" target="_blank">Scikit-öğrenme</a>, <a href="https://www.tensorflow.org" target="_blank">Tensorflow</a>, <a href="https://pytorch.org" target="_blank">PyTorch</a>ve <a href="https://mxnet.io" target="_blank">MXNet</a>.

Kod yazın veya görsel arabirimini kullanın yanı sıra en iyi çözüm bulmak için dağıtılan modelleri Yönet denerken birden çok çalıştırma izleyebilirsiniz.

### <a name="code-first-experience"></a>Kod öncelikli deneyimi

Eğitimini kullanarak, yerel makinede Başlat <a href="https://aka.ms/aml-sdk" target="_blank">Azure Machine Learning Python SDK'sı</a> ve sonra ölçeği buluta genişletme. Kullanılabilir birçok [hedefleri işlem](how-to-set-up-training-targets.md), ister Azure Machine Learning işlem ve [Azure Databricks](/azure/azure-databricks/what-is-azure-databricks)ile [Hizmetleri ayarlama hiper parametre Gelişmiş](how-to-tune-hyperparameters.md), oluşturabileceğinizi bulutun gücünü kullanarak daha hızlı bir şekilde daha iyi modelleri.

Ayrıca [model eğitiminin ve ayarlama otomatikleştirmek](tutorial-auto-train-models.md) SDK'sını kullanarak.

### <a name="code-free--low-code-experience"></a>Kodsuz / düşük kod deneyimi

Kod gerektirmeyen eğitimi deneyin:

+ N-sürükle ve bırak denemek ve dağıtım için görsel arabirim
    
    ![Azure Machine Learning hizmeti için görsel arabirim](media/overview-what-is-azure-ml/visual-interface.png)

+ Azure portal seçeneği otomatik ML denemeleri

### <a name="operationalization-mlops"></a>Kullanıma hazır hale getirme (MLOps)

Doğru modeli kullandığınız zaman kolayca bunu IOT cihazında veya Power bı'dan bir web hizmeti kullanabilirsiniz. Daha fazla bilgi için makaleye bakın [nasıl dağıtılacağı ve nerede](how-to-deploy-and-where.md). 

Dağıtılan Modellerinizi kullanarak yönetebileceğiniz sonra [Python için Azure Machine Learning SDK](https://aka.ms/aml-sdk) veya [Azure portalında](https://portal.azure.com/). 

Bu modeller tüketilebilir ve dönüş içinde Öngörüler [gerçek zamanlı](how-to-consume-web-service.md) veya [zaman uyumsuz olarak](how-to-run-batch-predictions.md) büyük miktarlarda veri çubuğunda.

İle Gelişmiş [makine öğrenimi işlem hatları](concept-ml-pipelines.md), veri hazırlama, model eğitiminin ve değerlendirme, dağıtım yoluyla her adımında işbirliği yapabilir.

Azure Machine Learning hizmeti ile çalışmaya başlamak için bkz: [sonraki adımlar](#next-steps).

## <a name="how-does-azure-machine-learning-service-differ-from-studio"></a>Azure Machine Learning hizmeti Studio'dan farkı nedir?

[Machine Learning Studio](../studio/what-is-ml-studio.md) Burada, oluşturabilir, test ve kod yazmaya gerek kalmadan, makine öğrenimi çözümleri dağıtma bir işbirliğine dayalı, sürükle ve bırak visual çalışma alanıdır. Önceden oluşturulmuş ve önceden yapılandırılmış bir makine öğrenme algoritmalarını kullanır ve bir özel yanı sıra veri işleme modülleri platform işlem.

Azure Machine Learning hizmeti sağlar hem SDK'ları **- ve -** hızla veri hazırlama için bir görsel interface(preview) eğitme ve makine öğrenimi modelleri dağıtın. Bu görsel bir arabirim (Önizleme) Studio'ya benzer bir Sürükle ve bırak deneyimi sağlar. Ancak, özel işlem platform Studio, farklı görsel arabirim kendi işlem kaynakları kullanır ve Azure Machine Learning hizmetinde tam olarak tümleşiktir.

Hızlı bir karşılaştırması aşağıdadır.

|| Machine Learning Studio | Azure Machine Learning hizmeti:<br/>Görsel arabirim|
|---| --- | --- |
|| Genel kullanıma (GA) | Önizleme aşamasında|
|Modüller için arabirimi| Many | İlk dizi popüler modülleri|
|Eğitim işlem hedefleri| Özel işlem hedefi, yalnızca CPU desteği| Azure Machine Learning işlem, GPU veya CPU destekler.<br/>(Desteklenen SDK'yı diğer hesaplar)|
|Dağıtım işlem hedefleri| Özel web hizmeti biçimi, özelleştirilemeyen | Kurumsal güvenlik seçenekleri ve Azure Kubernetes hizmeti. <br/>([Diğer hesaplar](how-to-deploy-and-where.md) desteklenen SDK'sı) |
|Otomatik model eğitiminin ve hiper parametre ayarı | Hayır | Henüz visual arabiriminde. <br/> (SDK ve Azure Portalı'nda desteklenmiyor.) | 

Görsel arabirim (Önizleme) ile denemeye [hızlı başlangıç: Hazırlama ve kod yazmaya gerek kalmadan verileri görselleştirin](ui-quickstart-run-experiment.md)

> [!NOTE]
> Modelleri Studio'da oluşturulan dağıtılmış veya Azure Machine Learning hizmeti tarafından yönetilir. Ancak, oluşturulan ve dağıtılan hizmet visual arabiriminde modelleri Azure Machine Learning hizmeti çalışma yönetilebilir.

## <a name="free-trial"></a>Ücretsiz deneme sürümü

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

Azure hizmetlerinde harcayabileceğiniz krediler alırsınız. Krediler bittikten sonra hesabı tutabilir ve [ücretsiz Azure hizmetlerini](https://azure.microsoft.com/free/) kullanabilirsiniz. Açıkça ayarlarınızı değiştirip ücretlendirme istemediğiniz sürece kredi kartınız asla ücretlendirilmez. Veya [MSDN abone Avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F), ücretli Azure Hizmetleri, size, için kullanabildiğiniz her ay krediler sunar.

## <a name="next-steps"></a>Sonraki adımlar

- [Machine Learning hizmeti çalışma alanı oluşturma](setup-create-workspace.md) kullanmaya başlamak için.

- Eksiksiz öğreticileri izleyin: 
  + [Bir Azure Machine Learning hizmeti ile görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md) 
  + [Veri hazırlama ve otomatik-bir regresyon modeli eğitimi için otomatik makine öğrenimi kullanıyor](tutorial-data-prep.md)

- Kullanım [Azure Machine Learning veri hazırlığı SDK'sı](https://aka.ms/data-prep-sdk) verilerinizi hazırlamak için.

- Makine öğrenmesi senaryolarınızı derlemek, iyileştirmek ve yönetmek için [makine öğrenmesi işlem hatları](/azure/machine-learning/service/concept-ml-pipelines) hakkında bilgi edinin.

- Ayrıntılı okuma [Azure Machine Learning hizmeti mimarisi ve kavramları](concept-azure-machine-learning-architecture.md) makalesi.

- Daha fazla bilgi için [diğer makine öğrenimi ürünlerini Microsoft gelen](/azure/architecture/data-guide/technology-choices/data-science-and-machine-learning).
