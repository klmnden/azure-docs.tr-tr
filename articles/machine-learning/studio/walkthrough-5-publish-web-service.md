---
title: '5. adım: Machine Learning web hizmetini dağıtma | Microsoft Docs'
description: "5. adımını geliştirme Tahmine dayalı bir çözüm izlenecek yol: Tahmine dayalı bir denemeyi Machine Learning Studio'da bir web hizmeti olarak dağıtın."
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: 3fca74a3-c44b-4583-a218-c14c46ee5338
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.openlocfilehash: 416a6d1c151fae01a00d00282ab9118e7391fbb7
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="walkthrough-step-5-deploy-the-azure-machine-learning-web-service"></a>Kılavuz Adımı 5: Azure Machine Learning web hizmetini dağıtma
Bu izlenecek yol beşinci adımdır [Azure Machine learning'de Tahmine dayalı analiz çözümü geliştirme](walkthrough-develop-predictive-solution.md)

1. [Bir Machine Learning çalışma alanı oluşturma](walkthrough-1-create-ml-workspace.md)
2. [Mevcut verileri yükleme](walkthrough-2-upload-data.md)
3. [Yeni bir deneme oluşturma](walkthrough-3-create-new-experiment.md)
4. [Modelleri eğitme ve değerlendirme](walkthrough-4-train-and-evaluate-models.md)
5. **Web hizmetini dağıtma**
6. [Web hizmetine erişme](walkthrough-6-access-web-service.md)

- - -
Başkalarının Biz bu kılavuzda geliştirdik Tahmine dayalı bir model kullanma olanağı vermek için biz bunu Azure üzerinde bir web hizmeti olarak dağıtabilirsiniz.

Bu noktaya kadar biz modelimizi eğitim ile denemeler. Ancak artık dağıtılmış hizmet eğitim yapacaksınız - bizim modelini temel alan kullanıcı girişi Puanlama tarafından yeni tahminleri oluşturmak için geçiyor. Bu deneme gelen dönüştürmek için bazı hazırlıklar yapmanız oluşturacağız şekilde bir ***eğitim*** için deneme bir ***Tahmine dayalı*** deneyin. 

Bu üç adımlık bir işlemdir.  

1. Modellerin kaldırın
2. Dönüştürme *eğitim denemenizi* içine oluşturduk bir *tahmini deneme*
3. Tahmine dayalı denemeye bir web hizmeti olarak dağıtma

## <a name="remove-one-of-the-models"></a>Modellerin kaldırın

İlk olarak, biz bu deneme aşağı biraz kırpma gerekir. Şu anda iki farklı modelleri denemesinde sahip olduğumuz ancak yalnızca bir model Biz bu web hizmeti olarak dağıtırken kullanılacak istiyoruz.  

Biz boosted ağacı modeli SVM modeli daha iyi gerçekleştirilen karar varsayalım. Yapılacak ilk şey kaldırmak nedenle [iki sınıflı destek vektör makinesi] [ two-class-support-vector-machine] modülü ve eğitim için kullanılan modül. Bir denemeyi ilk tıklayarak kopyasını isteyebilirsiniz **Kaydet** deneme tuvalinin altındaki.

Aşağıdaki modüller silmek ihtiyacımız var:  

* [İki sınıflı destekli vektör makinesi][two-class-support-vector-machine]
* [Modeli eğitmek] [ train-model] ve [Score Model] [ score-model] ona bağlı modüller
* [Veri normalleştirin] [ normalize-data] (her ikisi de)
* [Modeli değerlendirin] [ evaluate-model] (biz modelleri değerlendirme tamamlanmış olduğundan)

Her modülü seçin ve Delete tuşuna basın veya modülü sağ tıklatın ve seçin **silmek**. 

![SVM modeli kaldırıldı][3a]

Modelimizi bu gibi görünmelidir:

![SVM modeli kaldırıldı][3]

Bu modeli kullanarak dağıtmaya hazır değiliz artık [iki-Class Boosted karar ağacı][two-class-boosted-decision-tree].

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Tahmine dayalı bir deneme eğitim denemenizi Dönüştür

Bu model dağıtım için hazır hale getirmek için Biz bu eğitim denemenizi Tahmine dayalı bir deneme dönüştürmeniz gerekir. Bu üç adımdan oluşur:

1. Modeli eğittiğimize ve bizim eğitim modüllerini Değiştir kaydedin
2. Eğitim için yalnızca gerekli olan modülleri kaldırmak için deneme kırpma
3. Burada web hizmeti girişi kabul eder ve burada bir çıktı üretir tanımlayın

El ile yapabileceğimiz ancak tıklayarak üç adımı Neyse gerçekleştirilebilir **Web hizmetinin ayarı** deneme tuvalinin altındaki (ve seçerek **Tahmine dayalı Web hizmeti** seçeneği).

> [!TIP]
> Daha fazla bilgi isterseniz ne olur Tahmine dayalı bir deneme eğitim denemenizi dönüştürürken bkz [modelinizi Azure Machine Learning Studio'da dağıtımına hazırlamak nasıl](convert-training-experiment-to-scoring-experiment.md).

Tıkladığınızda **Web hizmetinin ayarı**, olacaklar:

* Tek bir eğitim modeli dönüştürülür **eğitilen Model** modülü ve deneme tuvaline solundaki modül paletindeki depolanan (altında bulabilirsiniz **eğitilmiş modeller**)
* Eğitim için kullanılan modülleri kaldırılır; özellikle:
  * [İki sınıflı artırılmış karar ağacı][two-class-boosted-decision-tree]
  * [Train Model][train-model]
  * [Veri bölme][split]
  * İkinci [R betiği yürütün] [ execute-r-script] test verileri için kullanılan Modülü
* Kaydedilen eğitilen model yeniden deneme eklenir
* **Web hizmeti girişi** ve **Web hizmeti çıkış** modülleri eklenir (Bu kullanıcının veri modeli burada girer ve web hizmeti erişildiğinde hangi verilerin döndürülen belirleme)

> [!NOTE]
> Deneme tuvalinin üstünde eklenen sekmeleri altında iki bölümden deneme kaydedilir görebilirsiniz. Özgün eğitim denemenizi sekmesi altında olan **eğitim denemenizi**, ve yeni oluşturulan Tahmine dayalı denemeye altında **Tahmine dayalı denemeye**. Tahmine dayalı denemeye, biz web hizmeti olarak dağıtacaksınız adrestir.

Biz bu belirli deneme ile ek bir adım gerçekleştirmeniz gerekir.
İki eklediğimiz [R betiği yürütün] [ execute-r-script] ağırlıklı işlevi veri sağlamak için modül. Bu modüller son modelindeki çıkışı olabilmesi için biz eğitim ve test için gerekli bir eli oluştu.
Machine Learning Studio kaldırılmış bir [R betiği yürütün] [ execute-r-script] onu kaldırıldığında Modülü [bölünmüş] [ split] modülü. Biz diğer kaldırın ve connect artık [meta verileri Düzenleyicisi] [ metadata-editor] doğrudan [Score Model][score-model].    

Bizim deneme gibi görünmelidir:  

![Eğitim modeli Puanlama][4]  

> [!NOTE]
> Biz Tahmine dayalı denemeye UCI Almanca kredi kartı veri kümesi neden sol merak ediyor olabilirsiniz. Hizmet geçiyor kullanıcının verileri, özgün dataset puan, bu nedenle özgün veri kümesi modelde neden bırakın?
> 
> Hizmet asıl kredi kartı veri gerekmez geçerlidir. Ancak, şemayı vardır kaç sütunlar gibi bilgileri içerir ve hangi sütunların sayısal bu verileri, gerekir. Bu şema bilgileri, kullanıcının verileri yorumlamak gereklidir. Biz bu bileşenlerin hizmet çalışırken Puanlama modülü dataset şeması sahip olması bağlı bırakın. Veri kullanılmaz, yalnızca şema.  
> 
> 

Son bir kez denemeyi çalıştırın (tıklatın **çalıştırmak**.) Modelin hala çalıştığından emin olmak istiyorsanız, çıktısını tıklatın [Score Model] [ score-model] modülü ve Seç **sonuçları görüntüleme**. Özgün veriler, kredi riski değeri ("skoru etiketleri") ve puanlama olasılık değeri ("skoru olasılıklar".) ile birlikte görüntülendiğini görebilirsiniz 

## <a name="deploy-the-web-service"></a>Web hizmetini dağıtma
Deneme ya da Klasik web hizmeti olarak ya da Azure Resource Manager tabanlı yeni bir web hizmeti olarak dağıtabilirsiniz.

### <a name="deploy-as-a-classic-web-service"></a>Klasik web hizmeti olarak dağıtma
Bizim deneme türetilmiş bir Klasik web hizmeti dağıtmak için **Web hizmeti Dağıt** seçin ve tuvalin altındaki **Web hizmeti Dağıt [Klasik]**. Machine Learning Studio'da deneme bir web hizmeti olarak dağıtır ve bu web hizmeti panoya alır. Bu sayfadan denemede döndürebilirsiniz (**görünüm anlık görüntü** veya **görüntülemek son**) ve basit bir sınama web hizmetinin çalıştırın (bkz **web hizmetini sınama** aşağıda). (Daha ayrıntılı bu kılavuzun sonraki adımda) web hizmeti erişebileceği uygulamaları oluşturmak için buradaki bilgiler bulunmaktadır.

![Web hizmeti Panosu][6]

Tıklatarak hizmeti yapılandırabilirsiniz **yapılandırma** sekmesi. (Bunu verilen deneme adını varsayılan olarak) hizmet adı burada değiştirebilirsiniz ve bir açıklama girin. Daha fazla kolay etiketleri için girdi ve çıktı verilerini de verebilirsiniz.  

![Web hizmetini yapılandır][5]  

### <a name="deploy-as-a-new-web-service"></a>Yeni bir web hizmeti olarak dağıtma

> [!NOTE] 
> Yeni bir web hizmeti dağıtmak için web hizmetini dağıtma abonelikte yeterli izniniz olması gerekir. Daha fazla bilgi için bkz: [Azure Machine Learning Web Hizmetleri Portalı'nı kullanarak bir web hizmetini yönetmek](manage-new-webservice.md). 

Yeni bir web hizmeti dağıtmak için bizim deneme türetilen:

1. Tıklatın **Web hizmeti Dağıt** seçin ve tuvalin altındaki **Web hizmeti dağıtma [Yeni]**. Machine Learning Studio aktarır, Azure Machine Learning web hizmetleri için **dağıtmak deneme** sayfası.

2. Web hizmeti için bir ad girin. 

3. İçin **fiyat planı**, var olan bir fiyatlandırma planı seçebilir ya da "Yeni Oluştur" seçeneğini belirleyin ve yeni plan bir ad verin ve aylık plan seçeneği belirleyin. Varsayılan bölgeniz ve web hizmetiniz için planlara planı katmanları varsayılan bu bölgeye dağıtılır.

4. Tıklatın **dağıtmak**.

Birkaç dakika sonra **Hızlı Başlangıç** web hizmetiniz için sayfası açılır.

Tıklatarak hizmeti yapılandırabilirsiniz **yapılandırma** sekmesi. Hizmet burada değiştirebilirsiniz başlığı ve bir açıklama girin. 

Web hizmeti sınamak için **Test** sekmesinde (bkz **web hizmetini sınama** aşağıda). Web hizmeti erişebileceği uygulamaları oluşturma hakkında daha fazla bilgi için tıklatın **Tüket** sekmesinde (Bu kılavuzda bir sonraki adım, daha fazla ayrıntıya çıkar).

> [!TIP]
> Bunu dağıttıktan sonra web hizmeti güncelleştirebilirsiniz. Örneğin, eğitim denemenizi düzenleyin ve sonra da, modelinizde değiştirmek istiyorsanız, model parametreleri ince ayar ve tıklatın **Web hizmeti Dağıt**, seçme **Web hizmeti Dağıt [Klasik]** veya **[Yeni] Web hizmetini dağıtma**. Denemeyi yeniden dağıttığınızda, artık güncelleştirilmiş model kullanarak web hizmeti değiştirir.  
> 
> 

## <a name="test-the-web-service"></a>Web hizmetini sınama

Web hizmeti erişildiğinde aracılığıyla kullanıcının verileri girer **Web hizmeti girişi** burada da iletilir Modülü [Score Model] [ score-model] modülü ve skoru. Tahmine dayalı denemeye ayarlarız şekilde, özgün kredi riski dataset aynı biçimde veri modeli bekliyor.
Web hizmetinden döndürülen sonuçları kullanıcıya **Web hizmeti çıkış** modülü.

> [!TIP]
> Tüm yapılandırılmış, Tahmine dayalı denemeye sahibiz şekilde oluşur [Score Model] [ score-model] modülü döndürülür. Bu, tüm giriş verilerini artı kredi riski değeri ve puanlama olasılık içerir. Ancak isterseniz, farklı bir şey dönebilirsiniz - Örneğin, yalnızca kredi riski değer döndürebilirsiniz. Bunu yapmak için INSERT bir [proje sütunları] [ project-columns] modülü arasında [Score Model] [ score-model] ve **Web hizmeti çıkış**sütunları ortadan kaldırmak için geri dönmek için web hizmeti istemediğiniz. 
> 
> 

Klasik web test edebilirsiniz ya da hizmet **Machine Learning Studio** veya **Azure Machine Learning Web Hizmetleri** portal.
Yeni bir web testi yalnızca hizmet **Machine Learning Web Hizmetleri** portal.

> [!TIP]
> Azure Machine Learning Web Hizmetleri Portalı'nda test edilirken istek-yanıt hizmeti test etmek için kullanabileceğiniz örnek veri oluşturma portal olabilir. Üzerinde **yapılandırma** sayfasında, için "Evet" seçeneğini **örnek veriler etkin?**. Üzerinde istek-yanıt sekmesini açtığınızda **Test** sayfasında, portal özgün kredi riski kümesinden alınan örnek verileri doldurur.

### <a name="test-a-classic-web-service"></a>Klasik web hizmetini sınama

Machine Learning Studio'da veya Machine Learning Web Hizmetleri portalında bir Klasik web hizmeti test edebilirsiniz. 

#### <a name="test-in-machine-learning-studio"></a>Machine Learning Studio'da test

1. Üzerinde **PANO** sayfasında web hizmeti için **Test** altında düğmesini **varsayılan uç nokta**. Bir iletişim kutusu açılır ve hizmet için bir giriş verisi sorar. Özgün kredi riski kümesinde görünen aynı sütunlara bunlar.  

2. Bir veri kümesi girin ve ardından **Tamam**. 

#### <a name="test-in-the-machine-learning-web-services-portal"></a>Machine Learning Web Hizmetleri Portalı'nda test

1. Üzerinde **PANO** sayfasında web hizmeti için **Test Önizleme** altında bağlantı **varsayılan uç nokta**. Web hizmeti uç noktası için Azure Machine Learning Web Hizmetleri portalında test sayfası açılır ve hizmet için bir giriş verisi sorar. Özgün kredi riski kümesinde görünen aynı sütunlara bunlar.

2. Tıklatın **Test istek-yanıt**. 

### <a name="test-a-new-web-service"></a>Yeni bir web hizmeti test etme

Yalnızca Machine Learning Web Hizmetleri portalında yeni bir web hizmeti test edebilirsiniz.

1. İçinde [Azure Machine Learning Web Hizmetleri](https://services.azureml.net/quickstart) portal tıklatın **Test** sayfanın üst kısmındaki. **Test** sayfası açılır ve veri hizmeti için girebilirsiniz. Görüntülenen giriş alanları, özgün kredi riski kümesinde görünen sütunlara karşılık gelir. 

2. Bir veri kümesi girin ve ardından **Test istek-yanıt**.

Test sonuçlarını çıkış sütununda sayfasının sağ tarafında görüntülenir. 


## <a name="manage-the-web-service"></a>Web hizmeti yönetme

Klasik ya da yeni web hizmetinizi dağıttıktan sonra sonra buradan yönetebilirsiniz [Microsoft Azure Machine Learning Web Hizmetleri](https://services.azureml.net/quickstart) portal.

Web hizmetinizin performansını izlemek için:

1. Oturum [Microsoft Azure Machine Learning Web Hizmetleri](https://services.azureml.net/quickstart) portalı
2. Tıklatın **Web Hizmetleri**
3. Web hizmeti
4. Tıklatın **Panosu**

- - -
**Sonraki: [web hizmetine erişim](walkthrough-6-access-web-service.md)**

[3]: ./media/walkthrough-5-publish-web-service/publish3.png
[3a]: ./media/walkthrough-5-publish-web-service/publish3a.png
[4]: ./media/walkthrough-5-publish-web-service/publish4.png
[5]: ./media/walkthrough-5-publish-web-service/publish5.png
[6]: ./media/walkthrough-5-publish-web-service/publish6.png


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
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
