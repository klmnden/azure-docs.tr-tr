---
title: '3. Öğretici: Kredi riski model dağıtma'
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio'da bir kredi riski değerlendirmesi için Tahmine dayalı analiz çözümü oluşturmayı gösteren ayrıntılı bir öğretici. Bu öğretici üç üç bölümlü öğretici serisinin bir parçasıdır. Bu model bir web hizmeti olarak dağıtma işlemi gösterilmektedir.
keywords: Kredi riski, Tahmine dayalı analiz çözümü, risk değerlendirmesi, dağıtma, web hizmeti
author: sdgilley
ms.author: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: tutorial
ms.date: 02/11/2019
ms.openlocfilehash: 6cdccd54546296c85864f1588b71109ed8b8f79f
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58620523"
---
# <a name="tutorial-3-deploy-credit-risk-model---azure-machine-learning-studio"></a>3. Öğretici: Kredi riski model - Azure Machine Learning Studio dağıtma

Bu öğreticide, bir Tahmine dayalı analiz çözümü geliştirme işleminin genişletilmiş bir göz atın. Machine Learning Studio'da basit bir model geliştirmektir.  Ardından bir Azure Machine Learning web hizmeti olarak modeli dağıtacağız.  Bu dağıtılmış modelin yeni verileri kullanarak tahmin yapabileceği. Bu öğretici **üç bölümlü öğretici bir serinin üçüncü bölümü**.

Bir kişinin kredi başvurusunda verdiği bilgilere dayanarak kredi riskini tahmin etmeniz gerektiğini varsayalım.  

Kredi riski değerlendirmesi karmaşık bir sorundur ancak bu öğreticide, bir bit basitleştirir. Microsoft Azure Machine Learning Studio'yu kullanarak Tahmine dayalı analiz çözümü nasıl oluşturabileceğinize dair bir örnek olarak kullanacaksınız. Azure Machine Learning Studio ve Machine Learning web hizmeti bu çözüm için kullanacaksınız. 

Üç bölümü olan Bu öğreticide, genel kullanıma açık kredi risk verileriyle başlayın.  Ardından geliştirin ve Tahmine dayalı bir model eğitip.  Son olarak modeli bir web hizmeti olarak dağıtalım.

İçinde [öğreticinin birinci kısmında](tutorial-part1-credit-risk.md), bir Machine Learning Studio çalışma alanı oluşturulduğunda, veri karşıya yüklendi ve bir deneme oluşturulur.

İçinde [öğreticinin İkinci bölüm](tutorial-part2-credit-risk-train.md), eğitim ve modelleri değerlendirilir.

Öğreticinin bu bölümünde:

> [!div class="checklist"]
> * Dağıtım için hazırlanma
> * Web hizmetini dağıtma
> * Web hizmetini test edin
> * Web hizmetini yönetme
> * Web hizmetine erişme

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="prerequisites"></a>Önkoşullar

Tam [öğreticinin İkinci bölüm](tutorial-part2-credit-risk-train.md).

## <a name="prepare-for-deployment"></a>Dağıtım için hazırlanma
Başkalarının Bu öğreticide geliştirdik Tahmine dayalı bir model kullanmak için bir fırsat vermek için bunu azure'da bir web hizmeti olarak dağıtabilirsiniz.

Bu noktaya kadar modelimizi eğitim ile denemeler. Ancak, dağıtılmış hizmet artık eğitim yapacak - kullanıcının girişi bizim modele dayalı Puanlama tarafından yeni tahminler üretmek gittiği. Biz bu denemeden dönüştürmek için bazı hazırlıklar yapacağız şekilde bir ***eğitim*** için deneme bir ***Tahmine dayalı*** denemeler yapın. 

Dağıtım için üç adımlık bir işlemdir:  

1. Modelleri birini kaldırın
1. Dönüştürme *eğitim denemesini* içinde oluşturduğunuz bir *Tahmine dayalı denemeye*
1. Tahmine dayalı denemeye bir web hizmeti olarak dağıtma

### <a name="remove-one-of-the-models"></a>Modelleri birini kaldırın

İlk olarak, bu deneme aşağı biraz trim gerekir. şu anda iki farklı modelleri deneme vardır, ancak yalnızca, bu web hizmeti olarak dağıttığınızda, bir model kullanmak istiyorsunuz.  

Artırmalı ağaç modeli SVM modelin daha iyi gerçekleştirilen verdiyseniz varsayalım. Yapılacak ilk şey kaldırmak, bu nedenle [iki sınıflı destekli vektör makinesi] [ two-class-support-vector-machine] modülü ve eğitim için kullanılan modül. Tıklayarak denemenin bir kopyasını önce yapmak isteyebileceğiniz **Kaydet** deneme tuvalinin altındaki.

Aşağıdaki modüller Sil yapmanız gerekir:  

* [İki sınıflı destekli vektör makinesi][two-class-support-vector-machine]
* [Modeli eğitme] [ train-model] ve [Score Model] [ score-model] ona bağlı modüller
* [Veri Normalleştir] [ normalize-data] (her ikisi)
* [Modeli değerlendirme] [ evaluate-model] (biz tamamlanmış değerlendirme modelleri için)

Her modülü seçin ve Delete tuşuna basın veya modül sağ tıklayıp **Sil**. 

![Destekli vektör makinesi modeli kaldırılacağı silmek için hangi modülü vurgular](./media/tutorial-part3-credit-risk-deploy/publish3a.png)

Modelimiz şöyle görünmelidir:

![Destekli vektör makinesi modeli silindiğinde elde edilen deneme](./media/tutorial-part3-credit-risk-deploy/publish3.png)

Bu modeli kullanarak dağıtmak hazırız artık [iki sınıflı artırılmış karar ağacı][two-class-boosted-decision-tree].

### <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Eğitim denemesini öngörücü bir denemeye dönüştürme

Bu model dağıtım için hazır hale getirmek için bu eğitim denemesini öngörücü bir denemeye dönüştürme gerekir. Bu üç adımdan oluşur:

1. Modeli eğittiğimize ve ardından bizim eğitim modüllerinin yerine geçer kaydedin
1. Eğitim için yalnızca gerekli olan modülleri kaldırmak için denemeyi Kırp
1. Burada, web hizmeti giriş kabul eder ve burada Bu çıktıyı oluşturur tanımlayın

Bunu el ile yapabilirsiniz, ancak tıklayarak üç adımı Neyse gerçekleştirilebilir **Web hizmetinin ayarı** deneme tuvalinin altındaki (ve seçerek **Tahmine dayalı Web hizmeti** seçeneği).

> [!TIP]
> Daha ayrıntılı bilgi isterseniz ne olur eğitim denemesini öngörücü bir denemeye dönüştürme yaptığınızda, bkz: [modelinizin Azure Machine Learning Studio'da dağıtımına hazırlamak nasıl](convert-training-experiment-to-scoring-experiment.md).

Tıkladığınızda **Web hizmetinin ayarı**, olacaklar:

* Tek bir eğitim modeli dönüştürülür **eğitilen Model** modülü ve deneme tuvaline solundaki modül paletindeki saklı (altında bulabilirsiniz **eğitilen modelleri**)
* Eğitim için kullanılan modülleri kaldırılır; özellikle:
  * [İki sınıflı Artırmalı karar ağacı][two-class-boosted-decision-tree]
  * [Modeli eğitme][train-model]
  * [Verileri bölme][split]
  * İkinci [R betiği yürütme] [ execute-r-script] test verileri için kullanılan modül
* Kaydedilmiş eğitilen modeli yeniden denemenin eklenir
* **Web hizmeti giriş** ve **Web hizmeti çıkış** modülleri eklendi (bunlar burada kullanıcının veri modeli girer ve web hizmeti erişim sağlandığında hangi verilerin döndürülen tanımlar)

> [!NOTE]
> Denemeyi deneme tuvalinin üst kısmında eklenen sekmeleri altında iki parça kaydedildiğini görebilirsiniz. Özgün eğitim denemesini sekmesi altında olduğunu **eğitim denemesini**, ve yeni oluşturulan Tahmine dayalı denemeye altındadır **Tahmine dayalı denemeye**. Tahmine dayalı denemeye bir web hizmeti olarak dağıtacaksınız olur.

Bu belirli deneme ile bir ek adımı gerçekleştirmeniz gerekir.
iki eklediğiniz [R betiği yürütme] [ execute-r-script] verileri için bir ağırlık işlevi sağlamak için modüller. Bu modüllerdeki son modelin kullanıma olabilmesi için eğitim ve sınama için gerekli bir kandırma oluştu.
Machine Learning Studio'da bir kaldırıldı [R betiği yürütme] [ execute-r-script] kaldırdığınızda, modül [bölünmüş] [ split] modülü. Şimdi diğer kaldırın ve connect [meta verileri Düzenleyicisi] [ metadata-editor] doğrudan [Score Model][score-model].    

Bizim deneme gibi görünmelidir:  

![Eğitim modeli Puanlama](./media/tutorial-part3-credit-risk-deploy/publish4.png)


> [!NOTE]
> UCI Almanca kredi kartı verileri veri kümesi Tahmine dayalı denemeye neden bıraktığınız merak ediyor olabilirsiniz. Hizmet değil özgün veri kümesinden kullanıcı verilerini puanlamak için neden özgün veri kümesinden modelde geliştirmediğinden geçiyor?
> 
> Hizmet asıl kredi kartı verileriyle gerektirmeyeceği geçerlidir. Ancak, şema vardır, kaç sütun gibi bilgileri içerir ve hangi sütunların sayısal bu veriler için gerekir. Bu şema bilgileri, kullanıcının verileri yorumlamak gereklidir. hizmet çalışırken, veri kümesi şemasını Puanlama modülü sahip olacak şekilde bağlı bu bileşenlerin bırakır. Verileri kullanılmaz, yalnızca şema.  
> 
>Dikkat edilecek önemli bir unsur, özgün veri etiketi içeriyorsa, ardından web girdisinden beklenen şema ayrıca etiketli bir sütun beklediğiniz olan! Etiket ve diğer verileri, eğitim kümesinde olan ancak web girişlerinde web giriş ve eğitim veri kümesi, ortak bir modüle bağlanmadan önce olmaz, bu kullanacağınızı kaldırmaktır. 
> 

Son bir kez denemeyi çalıştırın (tıklayın **çalıştırma**.) Modelin hala çalıştığından emin olmak istiyorsanız, çıktısını tıklayın [Score Model] [ score-model] modülü ve select **sonuçlarını görüntüle**. Özgün veriler, kredi riski değeri ("Puanlanmış etiketler") ve puanlama olasılık değeri ("Puanlanmış olasılıklar".) ile birlikte görüntülendiğini görebilirsiniz. 

## <a name="deploy-the-web-service"></a>Web hizmetini dağıtma
Denemeyi ya da bir Klasik web hizmeti olarak veya Azure Resource Manager'a bağlı yeni bir web hizmeti olarak dağıtabilirsiniz.

### <a name="deploy-as-a-classic-web-service"></a>Klasik web hizmeti olarak dağıtma
Bizim denemeden türetilmiş bir Klasik web hizmetini dağıtmak için **Web hizmeti Dağıt** seçin ve tuval aşağıda **Web hizmeti dağıtma [Klasik]**. Machine Learning Studio web hizmeti olarak denemeyi dağıtır ve bu web hizmeti için panoya alır. Bu sayfadan deney için döndürebilir (**anlık görüntüsünü görüntüle** veya **görüntülemek son**) ve web hizmeti basit bir test çalıştırın (bkz **web hizmetini Test** aşağıda). (Daha fazla bilgi, bu öğreticinin sonraki adımda) web hizmetine erişebilen uygulamaları oluşturmak için buradaki bilgiler de mevcuttur.

![Web hizmet Panosu](./media/tutorial-part3-credit-risk-deploy/publish6.png)


Tıklayarak hizmeti yapılandırabilirsiniz **yapılandırma** sekmesi. (Bunu verilen deney adı varsayılan olarak) hizmet adını burada değiştirebilirsiniz ve bir açıklama girin. Daha fazla kolay etiketler için girdi ve çıktı verilerini de tanıyabilirsiniz.  

![Web hizmetini yapılandır](./media/tutorial-part3-credit-risk-deploy/publish5.png)


### <a name="deploy-as-a-new-web-service"></a>Yeni bir web hizmeti dağıtma

> [!NOTE] 
> Yeni bir web hizmetini dağıtmak için web hizmetini dağıttığınız aboneliğe yeterli izinleri olmalıdır. Daha fazla bilgi için [Azure Machine Learning Web Hizmetleri portalını kullanarak bir web hizmetini yönetme](manage-new-webservice.md). 

Yeni bir web hizmetini dağıtmak için sunduğumuz denemeden türetilmiş:

1. Tıklayın **Web hizmeti Dağıt** seçin ve tuval aşağıda **Web hizmeti dağıtma [Yeni]**. Machine Learning Studio, Azure Machine Learning web hizmetlerini aktarır **dağıtma deneme** sayfası.

1. Web hizmeti için bir ad girin. 

1. İçin **fiyat planı**, bir devrenin fiyatlandırma planını seçin veya "Yeni Oluştur" seçebilir ve yeni plan bir ad verin ve aylık plan seçeneğini belirleyin. Hizmetinizi varsayılan bölgeden ve web hizmetiniz için planlar plan katmanları varsayılan bu bölgeye dağıtılır.

1. Tıklayın **dağıtma**.

Birkaç dakika sonra **hızlı** web hizmetiniz için sayfası açılır.

Tıklayarak hizmeti yapılandırabilirsiniz **yapılandırma** sekmesi. Hizmet burada değiştirebilirsiniz başlığı ve bir açıklama girin. 

Web hizmetini test etmek için **Test** sekme (bkz **web hizmetini Test** aşağıda). Web hizmetine erişebilen uygulamaları oluşturma hakkında daha fazla bilgi için tıklatın **Tüket** (Bu öğreticinin sonraki adımında, daha fazla ayrıntıya yazılacak) sekmesi.

> [!TIP]
> Bunu dağıttıktan sonra web hizmeti güncelleştirebilirsiniz. Örneğin, eğitim denemesini düzenleyebilir, sonra da, modelinizde değiştirmek istiyorsanız, model parametrelerinin ince ve tıklayın **Web hizmeti Dağıt**u seçerek **Web hizmeti dağıtma [Klasik]** veya **[Yeni] Web hizmetini dağıtma**. Denemeyi yeniden dağıttığınızda, artık güncelleştirilmiş model kullanarak web hizmeti değiştirir.  
> 
> 

## <a name="test-the-web-service"></a>Web hizmetini test edin

Web hizmeti erişim sağlandığında aracılığıyla kullanıcının veri girer **Web hizmeti giriş** burada da geçirilir Modülü [Score Model] [ score-model] modülü ve puanlanmış. Tahmine dayalı denemeye belirlediğiniz şekilde, özgün kredi riski veri kümesi ile aynı biçimde veri modeli bekliyor.
Sonuçları kullanıcıya web hizmeti aracılığıyla döndürülen **Web hizmeti çıkış** modülü.

> [!TIP]
> Tüm yapılandırılmış, Tahmine dayalı denemeye sahip olduğunuz şekilde oluşur [Score Model] [ score-model] modülü döndürülür. Bu, tüm giriş verilerini ayrıca kredi riski değeri ve puanlama olasılık içerir. Ancak isterseniz farklı bir şey döndürebilir; Örneğin, yalnızca kredi riski değer döndürebilir. Bunu yapmak için INSERT bir [Sütunları Seç] [ select-columns] arasında Modülü [Score Model] [ score-model] ve **Web hizmeti çıkış**döndürmek için web hizmeti istemediğiniz sütunları ortadan kaldırmak için. 
> 
> 

Klasik web test edebilirsiniz ya da hizmet **Machine Learning Studio** veya **Azure Machine Learning Web Hizmetleri** portalı.
Yeni bir web test edebilirsiniz yalnızca hizmet **Machine Learning Web Hizmetleri** portalı.

> [!TIP]
> Azure Machine Learning Web Hizmetleri portalında test ederken, istek-yanıt hizmeti test etmek için kullanabileceğiniz örnek veriler oluşturma portalı olabilir. Üzerinde **yapılandırma** sayfasında, "Evet" seçeneğini **örnek veriler etkin?**. Üzerinde istek-yanıt sekmesini açtığınızda **Test** sayfasında, portal özgün kredi riski veri kümesinden alınan örnek veriler doldurur.

### <a name="test-a-classic-web-service"></a>Klasik web hizmetini test edin

Machine Learning Studio'da veya Machine Learning Web Hizmetleri portalında bir Klasik web hizmetini test edebilirsiniz. 

#### <a name="test-in-machine-learning-studio"></a>Machine Learning Studio'da test

1. Üzerinde **PANO** sayfasında web hizmeti, **Test** düğmesini **varsayılan uç nokta**. Bir iletişim kutusu açılır ve hizmet için bir giriş verisi sorar. Özgün kredi riski kümesinde görünen aynı sütunları şunlardır.  

1. Bir veri kümesini girin ve ardından **Tamam**. 

#### <a name="test-in-the-machine-learning-web-services-portal"></a>Machine Learning Web Hizmetleri portalında test etme

1. Üzerinde **PANO** sayfasında web hizmeti, **Test Önizleme** altında bağlantı **varsayılan uç nokta**. Azure Machine Learning Web Hizmetleri portalında web hizmeti uç noktası için test sayfası açılır ve hizmet için bir giriş verisi ister. Özgün kredi riski kümesinde görünen aynı sütunları şunlardır.

2. Tıklayın **Test istek-yanıt**. 

### <a name="test-a-new-web-service"></a>Yeni bir web hizmetini test edin

Yalnızca Machine Learning Web Hizmetleri portalında yeni bir web hizmetini test edebilirsiniz.

1. İçinde [Azure Machine Learning Web Hizmetleri](https://services.azureml.net/quickstart) portal'ı tıklatın **Test** sayfanın üstünde. **Test** sayfası açılır ve hizmet için veri girebilirsiniz. Görüntülenen giriş alanları, özgün kredi riski kümesinde görünen sütunlara karşılık gelir. 

1. Bir veri kümesini girin ve ardından **Test istek-yanıt**.

Test sonuçlarını, çıkış sütununun sayfanın sağ tarafında görüntülenir. 


## <a name="manage-the-web-service"></a>Web hizmetini yönetme

Web hizmetinizi Klasik veya yeni dağıttıktan sonra buradan yönetebilirsiniz [Microsoft Azure Machine Learning Web Hizmetleri](https://services.azureml.net/quickstart) portalı.

Web hizmetinizin performansını izlemek için:

1. Oturum [Microsoft Azure Machine Learning Web Hizmetleri](https://services.azureml.net/quickstart) portalı
1. Tıklayın **Web Hizmetleri**
1. Web hizmeti
1. Tıklayın **Panosu**

## <a name="access-the-web-service"></a>Web hizmetine erişme

Bu öğreticide önceki adımda, kredi riskini tahmin modeli kullanan bir web hizmetini dağıttığınız. Artık kullanıcıların veri göndermek ve sonuçları almak kullanabilirsiniz. 

Web hizmeti, alabilir ve dönüş verileri iki yöntemden biriyle REST API'lerini kullanarak bir Azure web hizmetidir:  

* **İstek/yanıt** - kullanıcı bir HTTP protokolünü kullanarak bir veya daha fazla kredi veri satırlarını hizmetine gönderir ve hizmet yanıt veren bir veya daha fazla sonuç kümeleri.
* **Toplu yürütme** - kullanıcı depolar veya daha fazla satır kredi verileri bir Azure blob ve sonra blob konum hizmetine gönderir. Hizmet giriş blob veri satırı puanlar, başka bir blobun sonuçlarını depolar ve bu kapsayıcı URL'sini döndürür.  

Klasik web hizmetine erişmek için hızlı ve en kolay yollarından biri sayesinde [Azure ML istek-yanıt hizmeti Web uygulaması](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) veya [Azure ML toplu iş yürütme hizmeti Web uygulaması şablonunu](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).

Bu web uygulaması şablonları, web hizmetinizin girdi verilerini ve hangi döndüreceği bildiği bir özel web uygulaması oluşturabilirsiniz. Tek yapmak için ihtiyacınız olan web hizmeti ve veri erişim sağlamak ve şablon geri kalanını yapar.

Web uygulaması şablonları kullanma hakkında daha fazla bilgi için bkz. [web uygulaması şablonu ile bir Azure Machine Learning Web hizmetini kullanma](/azure/machine-learning/studio/consume-web-services).



## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [machine-learning-studio-clean-up](../../../includes/machine-learning-studio-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bu adımlar tamamlandı:

> [!div class="checklist"]
> * Dağıtım için hazırlanma
> * Web hizmetini dağıtma
> * Web hizmetini test edin
> * Web hizmetini yönetme
> * Web hizmetine erişme

Ayrıca, R, sağlanan başlangıç kodunu kullanarak web hizmetine erişmek için özel bir uygulama geliştirebilirsiniz C#, ve Python programlama dilleri arasındadır.

> [!div class="nextstepaction"]
> [Bir Azure Machine Learning Web hizmetini kullanma](consume-web-services.md)

<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
