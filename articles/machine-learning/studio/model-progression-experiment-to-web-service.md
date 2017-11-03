---
title: "Bir web hizmeti bir Azure Machine Learning modeli nasıl olur | Microsoft Docs"
description: "Bir geliştirme, Azure Machine Learning modeli ilerler kullanıma hazır hale getirilmiş bir Web hizmeti nasıl deneme mekanizması genel bakış."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 25e0c025-f8b0-44ab-beaf-d0f2d485eb91
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 383f0a466f92a230e49c3d1e96d306a0b7d67da2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-a-machine-learning-model-progresses-from-an-experiment-to-an-operationalized-web-service"></a>Nasıl bir Machine Learning modelini kullanıma hazır hale getirilmiş bir Web hizmeti deneme ilerler.
Azure Machine Learning Studio geliştirmek, çalıştırmak, test ve yinelemek izin veren bir etkileşimli tuvale sağlayan bir ***denemeler*** Tahmine dayalı bir modeli temsil eden. Çok çeşitli olabilir modülleri vardır:

* Denemenizi giriş verilerini
* Verileri işlemek
* Machine learning algoritmaları kullanarak bir model eğitme
* Modeli puanlama
* Sonuçları değerlendirin
* Çıkış son değerleri

İle denemenizi memnun kaldıktan sonra olarak dağıtabilirsiniz bir ***Klasik Azure Machine Learning Web hizmeti*** veya ***yeni Azure Machine Learning Web hizmeti*** böylece kullanıcılar yeni veri gönderme ve geri alma sonuçları.

Bu makalede, geliştirme, Machine Learning modeli ilerler kullanıma hazır hale getirilmiş bir Web hizmeti nasıl deneme mekanizması genel bir bakış sağlar.

> [!NOTE]
> Geliştirmek ve makine öğrenimi modellerini dağıtmak için diğer yolları vardır, ancak bu makale, Machine Learning Studio nasıl kullandığınıza odaklanmıştır. Örneğin, Tahmine dayalı bir Klasik Web hizmeti ile R oluşturma açıklamasını okumak için blog gönderisine bakın [yapı & dağıtmak Tahmine dayalı Web uygulamaları kullanarak Rstudio'dan ve Azure ML](http://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx).
> 
> 

Azure Machine Learning Studio, geliştirmek ve dağıtmanıza yardımcı olmak için tasarlanmıştır ancak bir *Tahmine dayalı analiz modeli*, Tahmine dayalı bir modeli içermeyen bir denemeyi geliştirmek için Studio kullanmak da mümkündür. Örneğin, bir deneme yalnızca veri girişi işlemeden ve sonuçları çıktı. Bir Tahmine dayalı analiz deneme gibi Tahmine dayalı olmayan bu deneme bir Web hizmeti olarak dağıtabilirsiniz, ancak deneme değil veya eğitim machine learning modeli Puanlama olduğundan daha basit bir işlem değil. Tipik Studio bu şekilde kullanmak için olmamasına karşın, Studio nasıl çalıştığı hakkında ile ilgili eksiksiz bir açıklama sunuyoruz Biz bu tartışmada içermesi.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Geliştirme ve Tahmine dayalı bir Web hizmetini dağıtma
Geliştirmek ve Machine Learning Studio kullanarak dağıtmak gibi tipik bir çözüm izleyen aşamaları şunlardır:

![Dağıtım akışı](./media/model-progression-experiment-to-web-service/model-stages-from-experiment-to-web-service.png)

*Şekil 1 - Tipik Tahmine dayalı bir modeli aşamaları*

### <a name="the-training-experiment"></a>Eğitim denemenizi
***Eğitim denemenizi*** Web hizmetiniz Machine Learning Studio'da geliştirmenin ilk aşamasıdır. Geliştirme, test, yineleme ve sonunda bir machine learning modelini eğitmek için bir yer vermek için eğitim denemenizi amacı budur. Bile birden fazla modeli aynı anda en iyi çözüm arayın, ancak işiniz bittiğinde, denemeler eğitilmiş tek bir seçersiniz eğitmek model ve kalan deneme kısmından ortadan kaldırmak. Bir Tahmine dayalı analiz denemesi geliştirmek ilişkin bir örnek için bkz: [Azure Machine Learning kredi riski değerlendirmesi için Tahmine dayalı analiz çözümü geliştirme](walkthrough-develop-predictive-solution.md).

### <a name="the-predictive-experiment"></a>Tahmine dayalı denemeye
Eğitim denemenizi bir modeli eğittikten sonra tıklatın **Web hizmetinin ayarı** seçip **Tahmine dayalı Web hizmeti** Machine Learning Studio'da eğitim dönüştürme işlemini başlatmak için için deneme bir ***Tahmine dayalı denemeye***. Tahmine dayalı denemeye amacı sonunda bir Azure Web hizmeti olarak kullanıma hazır hale getirilmiş olma amacı ile yeni verilerinizi puanlamada için eğitilen model kullanmaktır.

Bu dönüştürme, aşağıdaki adımlarda size gerçekleştirilir:

* Tek bir modüle eğitim için kullanılan modül kümesini dönüştürmek ve eğitilen model olarak Kaydet
* Puanlama için ilgili olmayan yabancı modülleriniz ortadan kaldırma
* Nihai Web hizmetini kullanacak olan giriş ve çıkış bağlantı noktaları ekleme

Daha fazla değişiklik Tahmine dayalı denemenizi bir Web hizmeti olarak dağıtmak hazır hale getirmek yapmak istediğiniz olabilir. Örneğin, yalnızca bir alt sonuçlarını çıkarmak için Web hizmeti istiyorsanız, çıkış bağlantı noktasına önce filtreleme modülü ekleyebilirsiniz.

Bu dönüştürme işlemi eğitim denemenizi atılan değil. İşlem tamamlandığında, Studio'da iki sekme vardır: biri eğitim denemenizi, diğeri tahmini deneme için. Bu şekilde, Web hizmetini dağıtma ve Tahmine dayalı denemeye yeniden önce eğitim denemenizi değişiklikler yapabilirsiniz. Veya deneme başka bir satırın başlangıcına eğitim denemenizi bir kopyasını kaydedebilirsiniz.

> [!NOTE]
> Tıkladığınızda **Tahmine dayalı Web hizmeti** eğitim denemenizi Tahmine dayalı bir deneme dönüştürmek için otomatik bir işlem başlatın ve bu da çoğu durumda çalışır. Eğitim denemenizi karmaşık ise (örneğin, birlikte katılma eğitim için birden fazla yol varsa), bu dönüştürme el ile yapmak tercih edebilirsiniz. Daha fazla bilgi için bkz: [modelinizi Azure Machine Learning Studio'da dağıtımına hazırlamak nasıl](convert-training-experiment-to-scoring-experiment.md).
> 
> 

### <a name="the-web-service"></a>Web hizmeti
Memnun kaldıktan sonra Tahmine dayalı denemeye hazır, hizmetiniz bir Klasik Web hizmeti olarak dağıtabilir veya yeni Web hizmeti tabanlı Azure Resource Manager. Modelinizi olarak dağıtılarak faaliyete geçirmek için bir *Klasik Machine Learning Web hizmeti*, tıklatın **Web hizmeti Dağıt** seçip **Web hizmeti Dağıt [Klasik]**. Olarak dağıtmak için *yeni Machine Learning Web hizmeti*, tıklatın **Web hizmeti Dağıt** seçip **Web hizmeti dağıtma [Yeni]**. Kullanıcılar artık Web hizmeti REST API kullanarak modelinizi veri göndermek ve sonuçları geri alabilirsiniz. Daha fazla bilgi için bkz. [Azure Machine Learning web hizmetini kullanma](consume-web-services.md).

## <a name="the-non-typical-case-creating-a-non-predictive-web-service"></a>Normal olmayan durum: Tahmine dayalı olmayan bir Web hizmeti oluşturma
Denemenizi eğitmek değil, Tahmine dayalı bir modeli olduktan sonra eğitim denemenizi ve puanlama deneme oluşturmanız gerekmez - yalnızca bir deneme yoktur ve bir Web hizmeti olarak dağıtabilirsiniz. Machine Learning Studio kullanmış olduğunuz modülleri çözümleyerek denemenizi Tahmine dayalı bir model içerip içermediğini algılar.

Denemenizi üzerinde yinelendiğinde ve onunla memnun sonra:

1. ' I tıklatın **Web hizmetinin ayarı** seçip **Web hizmeti yeniden eğitme** - giriş ve çıkış düğümlerini otomatik olarak eklenir
2. Tıklatın **çalıştırın**
3. Tıklatın **Web hizmeti Dağıt** seçip **Web hizmeti Dağıt [Klasik]** veya **Web hizmeti dağıtma [Yeni]** dağıtmak istediğiniz ortamına bağlı olarak.

Web hizmeti artık dağıtılır ve erişmek ve yalnızca bir Tahmine dayalı Web hizmeti gibi yönetebilirsiniz.

## <a name="updating-your-web-service"></a>Web hizmetiniz güncelleştirme
Ne denemenizi bir Web hizmeti olarak dağıttıktan sonra bunu güncelleştirmeniz gerekiyor?

Bu güncelleştirme için gerekenler üzerinde bağlıdır:

**Giriş veya çıkış değiştirmek istediğinizde ya da Web hizmeti verileri nasıl düzenler değiştirmek istediğiniz**

Model değiştirmiyorsanız, ancak yalnızca Web hizmetinin veri işleme biçimini değiştirme, Tahmine dayalı denemeye düzenleyin ve ardından **Web hizmeti Dağıt** seçip **Web hizmeti Dağıt [Klasik]** veya **Web hizmeti dağıtma [Yeni]** yeniden. Web hizmeti durdurulursa, güncelleştirilmiş tahmini deneme dağıtılır ve Web hizmeti yeniden başlatılır.

Örnek aşağıda verilmiştir: Tahmine dayalı denemenizi tüm satır tahmin edilen sonuç ile giriş verilerini döndürür varsayalım. Web hizmeti tarafından yalnızca bir sonuç istediğiniz karar verebilirsiniz. Ekleyebileceğiniz şekilde bir **proje sütunları** sonucu dışındaki sütunları dışlamak için şu çıkış bağlantı noktasına önce Tahmine dayalı denemeye modülünde. Tıkladığınızda **Web hizmeti Dağıt** seçip **Web hizmeti Dağıt [Klasik]** veya **Web hizmeti dağıtma [Yeni]** Web hizmeti yeniden güncelleştirilir.

**Yeni veri modeliyle yeniden eğitme istiyor**

Model öğrenme Makinenizde tutmak istediğiniz, ancak yeni verilerle yeniden eğitme istediğiniz, iki seçeneğiniz vardır:

1. **Web hizmetinin çalıştığı sırada model yeniden eğitme** -Tahmine dayalı Web hizmeti çalışırken modelinizi yeniden eğitme istiyorsanız, bunu yapmak için eğitim denemenizi birkaç değişiklik yaparak yapabilirsiniz bir ***yeniden eğitme denemeler***, olarak dağıtabilirsiniz sonra bir  ***yeniden eğitme web* hizmet**. Bunun nasıl yapılacağı hakkında yönergeler için bkz [yeniden eğitme Machine Learning modellerini programlama aracılığıyla](retrain-models-programmatically.md).
2. **Özgün eğitim denemenizi geri dönün ve farklı eğitim veri modelinizi geliştirmek için kullanma** - Tahmine dayalı denemenizi Web hizmetine bağlanır ancak eğitim denemenizi doğrudan bu şekilde bağlı değil. Özgün eğitim denemenizi değiştirip tıklatın **Web hizmetinin ayarı**, oluşturacağı bir *yeni* Tahmine dayalı, denemeler dağıtıldığında, oluşturacak bir *yeni* Web hizmet. Yalnızca özgün Web hizmeti de güncelleştirmez.
   
   Eğitim denemenizi değiştirmeniz gerekiyorsa, bunu açıp tıklatın **Kaydet** bir kopya yapmak için. Bu, özgün eğitim deneme, Tahmine dayalı denemeye bırakmanız ve Web hizmeti. Bu gibi durumlarda, yeni bir Web hizmeti artık yaptığınız değişikliklerle oluşturabilirsiniz. Yeni Web hizmeti dağıttıktan sonra bir kez daha sonra önceki Web hizmetini durdurmak veya yeni bir çalışmasını sağlamak karar verebilirsiniz.

**Farklı bir modeli eğitmek istediğiniz**

Çalışırken farklı eğitim yöntemi, vb. gibi farklı bir makine öğrenme algoritmasını seçmek özgün Tahmine dayalı denemenizi, değişiklik yapmak istediğiniz ardından modelinizin yeniden eğitme için yukarıda açıklanan ikinci yordamı izlemeniz gerekir: açın deneme, eğitim tıklatın **Kaydet** bir kopya yapmak ve sonra modeli geliştirmek, Tahmine dayalı deneme oluşturma ve web hizmetini dağıtma yeni yol başlatın. Bu hizmet, hangi biri veya her ikisi de çalıştırmaya devam etmesine karar verebilirsiniz özgün birine - ilgisi olmayan yeni bir Web oluşturur.

## <a name="next-steps"></a>Sonraki Adımlar
Geliştirme ve deneme işlemi hakkında daha fazla ayrıntı için aşağıdaki makalelere bakın:

* -Deneme dönüştürme [modelinizi Azure Machine Learning Studio'daki dağıtımı için hazırlama](convert-training-experiment-to-scoring-experiment.md)
* Web hizmeti - dağıtma [bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md)
* model - yeniden eğitme [yeniden eğitme Machine Learning modellerini programlama aracılığıyla](retrain-models-programmatically.md)

Tüm işlem örnekler için bkz:

* [Machine learning Öğreticisi: Azure Machine Learning Studio'da ilk denemenizi oluşturma](create-experiment.md)
* [İzlenecek yol: Azure Machine Learning kredi riski değerlendirmesi için Tahmine dayalı analiz çözümü geliştirme](walkthrough-develop-predictive-solution.md)

