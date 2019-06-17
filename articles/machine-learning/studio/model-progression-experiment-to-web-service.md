---
title: Bir model bir web hizmeti nasıl olur?
titleSuffix: Azure Machine Learning Studio
description: Nasıl bir Web hizmetine bir geliştirme, Azure Machine Learning Studio'da model ilerler deneme mekanizması genel bakış.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-ms.author=yahajiza, previous-author=YasinMSFT
ms.date: 03/20/2017
ms.openlocfilehash: 28bb96099acb800d9095325b8c7b46a6b5124b4e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61066060"
---
# <a name="how-a-machine-learning-studio-model-progresses-from-an-experiment-to-a-web-service"></a>Machine Learning Studio'da model denemeden bir Web hizmetine nasıl ilerlediğini
Azure Machine Learning Studio geliştirin, çalıştırın, test etme ve yineleme olanak tanıyan etkileşimli bir tuvale sağlayan bir ***deneme*** Tahmine dayalı bir modeli temsil eden. Çok çeşitli için modüller vardır:

* Denemenizi giriş verileri
* Veri işleme
* Makine öğrenme algoritmalarını kullanarak bir model eğitip
* Modeli puanlama
* Sonuçları değerlendirin
* Son çıkış değerleri

Denemenizi ile memnun olduğunuzda olarak dağıtabileceğiniz bir ***Klasik Azure Machine Learning Web hizmetini*** veya ***yeni Azure Machine Learning Web hizmetini*** kullanıcıların yeni veriler göndererek ve geri alma Sonuç.

Bu makalede, nasıl bir geliştirme, Machine Learning modeli ilerler çalışır hale getirilen bir Web hizmeti için deneme mekanizması genel bir bakış sunuyoruz.

> [!NOTE]
> Geliştirmek ve makine öğrenimi modelleri dağıtmak için farklı yöntemleri vardır, ancak bu makalede, Machine Learning Studio'yu nasıl kullanacağınızı odaklanmıştır. Örneğin, R ile Tahmine dayalı bir Klasik Web hizmeti oluşturmak nasıl bir açıklamasını okumak için blog gönderisine bakın [derleme ve dağıtma Tahmine dayalı Web Apps kullanarak RStudio ve Azure Machine Learning studio](https://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx).
>
>

Azure Machine Learning Studio, geliştirmenize ve dağıtmanıza yardımcı olmak için tasarlandığından bir *Tahmine dayalı analiz modeli*, Tahmine dayalı analiz modeli içermeyen bir denemeyi geliştirmek için Studio kullanmak da mümkündür. Örneğin, bir denemeyi yalnızca giriş verileri işlemeden ve ardından sonuçları gönderir. Bir Tahmine dayalı analiz deneme gibi Tahmine dayalı olmayan bu deneme bir Web hizmeti olarak dağıtabilirsiniz, ancak deneme değil veya eğitim machine learning modeli Puanlama daha basit bir süreç olmasıdır. Tipik Studio bu şekilde kullanmak olmamasına karşın, Studio nasıl çalıştığına ilişkin ayrıntılı bir açıklama sunuyoruz, böylece biz bunu tartışmaya yer alacak.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Geliştirme ve Tahmine dayalı Web hizmeti dağıtma
Tipik bir çözüm geliştirin ve Machine Learning Studio kullanarak dağıtma gibi izleyen aşamaları şunlardır:

![Dağıtım akışı](./media/model-progression-experiment-to-web-service/model-stages-from-experiment-to-web-service.png)

*Şekil 1 - Tipik Tahmine dayalı analiz modeli aşamaları*

### <a name="the-training-experiment"></a>Eğitim denemesini
***Eğitim denemesini*** Geliştirme Web hizmetini Machine Learning Studio'da ilk aşamasıdır. Eğitim deneyde amaç, geliştirme, test, yineleme ve sonunda bir machine learning modeli eğitmek için bir yer vermektir. Sizi bile birden çok modeli eşzamanlı olarak en uygun çözümü arayın, ancak bitirdiğinizde, denemeler tek bir eğitim seçersiniz eğitebilirsiniz model ve denemeyi geri kalanından ortadan kaldırın. Tahmine dayalı bir analiz denemesi geliştirmek ilişkin bir örnek için bkz [bir Azure Machine Learning Studio'da kredi riski değerlendirmesi için Tahmine dayalı analiz çözümü geliştirme](tutorial-part1-credit-risk.md).

### <a name="the-predictive-experiment"></a>Tahmine dayalı denemeye
Eğitilen bir modelin eğitim denemenizi oluşturduktan sonra tıklayın **Web hizmetinin ayarı** seçip **Tahmine dayalı Web hizmeti** eğitim dönüştürme işlemini başlatmak için Machine Learning Studio'da için deneme bir ***Tahmine dayalı denemeye***. Tahmine dayalı deneyde amaç, sonunda bir Azure Web hizmeti olarak kullanıma hazır hale getirdiniz olma amacıyla yeni verileri puanlamak için eğitilen model kullanmaktır.

Bu dönüştürme, aşağıdaki adımları izleyerek gerçekleştirilir:

* Tek bir modüle eğitim için kullanılan modül kümesini dönüştürün ve eğitilen model olarak kaydedin
* Fazlalık modüllerin puanlamaya ilgili olmayan ortadan kaldırın
* Nihai Web hizmetini kullanacak olan giriş ve çıkış bağlantı noktaları ekleme

Daha fazla değişiklik deneyiminizi Tahmine dayalı Web hizmeti olarak dağıtmak hazır hale getirmek yapmak istediğiniz olabilir. Örneğin, yalnızca bir alt kümesini sonuçları çıktı olarak Web hizmeti istiyorsanız, bir filtre modülün çıkış bağlantı noktasına önce ekleyebilirsiniz.

Bu dönüştürme işleminde eğitim denemesini atılır değil. İşlem tamamlandığında iki sekme Studio'da vardır: biri eğitim denemesini, diğeri için Tahmine dayalı denemeye. Bu şekilde, Web hizmetini dağıtma ve Tahmine dayalı denemeye yeniden önce için eğitim denemesini değişiklikler yapabilirsiniz. Veya başka bir deneme satırın başlangıcına eğitim denemenin bir kopyasını kaydedebilirsiniz.

> [!NOTE]
> Tıkladığınızda **Tahmine dayalı Web hizmeti** , eğitim denemesini öngörücü bir denemeye dönüştürme için otomatik bir işlem başlatın ve bu da çoğu durumda çalışır. Eğitim denemenizi karmaşık ise (örneğin, birlikte katılın eğitim için birden çok yol varsa), bu dönüştürme el ile yapmak tercih edebilirsiniz. Daha fazla bilgi için [modelinizin Azure Machine Learning Studio'da dağıtımına hazırlamak nasıl](convert-training-experiment-to-scoring-experiment.md).
>
>

### <a name="the-web-service"></a>Web hizmeti
Memnun olduğunuzda, Tahmine dayalı denemeye hazır, hizmetiniz bir Klasik Web hizmeti olarak dağıtabilir veya yeni Web hizmeti üzerinde Azure Resource Manager tabanlı. Olarak dağıtarak modelinizi kullanıma hazır hale getirmek için bir *Klasik Machine Learning Web hizmetini*, tıklayın **Web hizmeti Dağıt** seçip **Web hizmeti dağıtma [Klasik]** . Olarak dağıtmak için *yeni Machine Learning Web hizmetini*, tıklayın **Web hizmeti Dağıt** seçip **Web hizmeti dağıtma [Yeni]** . Kullanıcılar artık Web hizmeti REST API kullanarak modelinize veri göndermek ve sonuçları geri alabilirsiniz. Daha fazla bilgi için bkz. [Azure Machine Learning web hizmetini kullanma](consume-web-services.md).

## <a name="the-non-typical-case-creating-a-non-predictive-web-service"></a>Normal olmayan durum: Tahmine dayalı olmayan bir Web hizmeti oluşturma
Denemenizi değil eğitimle Tahmine dayalı analiz modeli ve ardından bir eğitim denemesini hem bir Puanlama deneme oluşturmanız gerekmez - yalnızca bir deneme yok ve Web hizmeti olarak dağıtabilirsiniz. Machine Learning Studio, kullandığınız modülleri analiz ederek deneyiminizi Tahmine dayalı bir model içerip içermediğini algılar.

Denemenizi üzerinde çalışmalar ve onunla memnun olana sonra:

1. Tıklayın **Web hizmetinin ayarı** seçip **Web hizmetini yeniden eğitme** - giriş ve çıkış düğümleri otomatik olarak eklenir
2. Tıklayın **çalıştırın**
3. Tıklayın **Web hizmeti Dağıt** seçip **Web hizmeti dağıtma [Klasik]** veya **Web hizmeti dağıtma [Yeni]** dağıtmak istediğiniz ortamına bağlı olarak.

Web hizmetiniz artık dağıtılır ve erişebilir ve Tahmine dayalı Web hizmeti gibi yalnızca bir yönetin.

## <a name="updating-your-web-service"></a>Web hizmeti güncelleştiriliyor
Bir Web hizmeti olarak güncelleştirmek ihtiyacım olursa ne denemenizi dağıttığınıza göre?

Bu güncelleştirme için gerekenler üzerinde bağlıdır:

**Giriş veya çıkış değiştirmek istediğinizde ya da Web hizmeti verileri nasıl yönetir değiştirmek istediğiniz**

Model değiştirmiyorsanız, ancak yalnızca Web hizmeti verileri nasıl işlediğini değiştirme, Tahmine dayalı denemeyi düzenleyin ve ardından **Web hizmeti Dağıt** seçip **Web hizmeti dağıtma [Klasik]** veya **Web hizmeti dağıtma [Yeni]** yeniden. Web hizmeti durduruldu, güncelleştirilmiş Tahmine dayalı denemeye dağıtılır ve Web hizmeti yeniden başlatılır.

Örnek aşağıda verilmiştir: Tahmine dayalı denemenizi giriş verilerinin tahmin edilen sonucu ile tüm satırı döndürür varsayalım. Web hizmetinin yalnızca sonuç döndürmek için istediğinize karar verin. Ekleyebilirsiniz, böylece bir **proje sütunları** önce sonucu dışındaki sütunları dışlamak için şu çıkış bağlantı noktasına, Tahmine dayalı denemeye modülünde. Tıkladığınızda **Web hizmeti Dağıt** seçip **Web hizmeti dağıtma [Klasik]** veya **Web hizmeti dağıtma [Yeni]** Web hizmeti yeniden güncelleştirilir.

**Yeni veri modeli yeniden eğitme istiyorsunuz**

Machine learning modeli tutmak istiyor ancak yeni verilerle çağırma istiyorsanız iki seçeneğiniz vardır:

1. **Web hizmeti çalışırken modeli yeniden eğitme** -Tahmine dayalı Web hizmeti çalışırken modelinizi yeniden ayarlamak istiyorsanız, bunu yapmak için eğitim denemesini birkaç değişiklik yaparak bunu yapabilirsiniz bir ***yeniden eğitme Deneme***, ardından olarak dağıtabileceğiniz bir  ***yeniden eğitme web* hizmet**. Bunun nasıl yapılacağı hakkında yönergeler için bkz [yeniden eğitme Machine Learning modellerini programlama aracılığıyla](/azure/machine-learning/studio/retrain-machine-learning-model).
2. **Özgün eğitim denemesini için geri dönün ve farklı bir eğitim veri modelinizin geliştirilmesine kullanmasını** - deneyiminizi Tahmine dayalı Web hizmetine bağlıdır ancak eğitim denemesini doğrudan bu şekilde bağlı değil. Özgün eğitim denemesini değiştirmek ve'ı tıklatırsanız **Web hizmetinin ayarı**, oluşturacağı bir *yeni* Tahmine dayalı, denemeler dağıtıldığında, oluşturacak bir *yeni* Web hizmeti. Ayrıca, özgün Web hizmeti yalnızca güncelleştirmez.

   Eğitim denemesini değiştirmeniz gerekiyorsa, açın ve tıklayın **Kaydet** kopyalanmasına. Bu, özgün eğitim denemesini, Tahmine dayalı denemeye bırakmanız ve Web hizmeti. Bu gibi durumlarda, yeni bir Web hizmeti artık yaptığınız değişikliklerle oluşturabilirsiniz. Yeni bir Web hizmeti dağıtıldıktan sonra ardından önceki Web hizmetini durdurmak veya yeni bir tane çalışmasını sağlamak karar verebilirsiniz.

**Farklı bir model eğitip istiyorsunuz**

Çalışan farklı eğitim yöntemi vb. özgün, Tahmine dayalı denemeye, farklı bir makine öğrenme algoritmasına, seçme gibi değişiklikler yapmak istediğiniz sonra modelinizi yeniden eğitme için yukarıda açıklanan ikinci yordamı izlemeniz gerekir: açın deneme, eğitim tıklayın **Kaydet** bir kopyasını oluşturun ve ardından, model geliştirmenize, Tahmine dayalı denemeye oluşturma ve web Hizmeti'ni dağıtma yeni yolunu başlatmak için. Bu hizmet, hangi biri veya her ikisi de çalıştırmaya devam etmek için karar vermek için özgün bir - ilgisi olmayan bir yeni Web oluşturur.

## <a name="next-steps"></a>Sonraki adımlar
Geliştirme ve deneme işlemi hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* denemeyi - dönüştürme [modelinizin Azure Machine Learning Studio'da dağıtımı için hazırlama](convert-training-experiment-to-scoring-experiment.md)
* -Web Hizmeti'ni dağıtma [bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md)
* -modeli yeniden eğitme [yeniden eğitme Machine Learning modellerini programlama aracılığıyla](/azure/machine-learning/studio/retrain-machine-learning-model)

Bu işlem örnekleri için bkz:

* [Machine learning Öğreticisi: Azure Machine Learning Studio'da ilk denemenizi oluşturma](create-experiment.md)
* [İzlenecek yol: Bir Azure Machine learning'de kredi riski değerlendirmesi için Tahmine dayalı analiz çözümü geliştirin](tutorial-part1-credit-risk.md)

