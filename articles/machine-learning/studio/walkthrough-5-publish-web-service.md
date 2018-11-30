---
title: '5. adım: Machine Learning Studio web hizmeti dağıtma | Microsoft Docs'
description: "5. adımı geliştirme Tahmine dayalı çözüm Kılavuzu: Tahmine dayalı bir denemeyi Machine Learning Studio'da bir web hizmeti olarak dağıtın."
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=yahajiza, author=YasinMSFT)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 3fca74a3-c44b-4583-a218-c14c46ee5338
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.openlocfilehash: 33965270c2be6f70614def79a49f1c4aa1a8fbbc
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52309938"
---
# <a name="walkthrough-step-5-deploy-the-azure-machine-learning-studio-web-service"></a>Kılavuz adımı 5: Azure Machine Learning Studio web hizmeti dağıtma
Bu kılavuz, beşinci adımdır [bir Azure Machine learning'de Tahmine dayalı analiz çözümü geliştirin](walkthrough-develop-predictive-solution.md)

1. [Bir Machine Learning çalışma alanı oluşturma](walkthrough-1-create-ml-workspace.md)
2. [Mevcut verileri yükleme](walkthrough-2-upload-data.md)
3. [Yeni bir deneme oluşturma](walkthrough-3-create-new-experiment.md)
4. [Modelleri eğitme ve değerlendirme](walkthrough-4-train-and-evaluate-models.md)
5. **Web hizmetini dağıtma**
6. [Web hizmetine erişme](walkthrough-6-access-web-service.md)

- - -
Başkalarının bu izlenecek yolda geliştirdik Tahmine dayalı model kullanmak için bir şans verin, biz, azure'da bir web hizmeti olarak dağıtabilirsiniz.

Bu noktaya kadar biz modelimizi eğitim ile denemeler. Ancak, dağıtılmış hizmet artık eğitim yapacak - kullanıcının girişi bizim modele dayalı Puanlama tarafından yeni tahminler üretmek gittiği. Biz bu denemeden dönüştürmek için bazı hazırlıklar yapacağız şekilde bir ***eğitim*** için deneme bir ***Tahmine dayalı*** denemeler yapın. 

Bu üç adımlık bir işlemdir.  

1. Modelleri birini kaldırın
2. Dönüştürme *eğitim denemesini* içine oluşturduk bir *Tahmine dayalı denemeye*
3. Tahmine dayalı denemeye bir web hizmeti olarak dağıtma

## <a name="remove-one-of-the-models"></a>Modelleri birini kaldırın

İlk olarak, bu deneme aşağı biraz kırpılacak ihtiyacımız var. Şu anda iki farklı modelleri deneme sahibiz ancak biz yalnızca Biz bu bir web hizmeti olarak dağıtalım bir modeli kullanmak istiyorsunuz.  

Artırmalı ağaç modeli SVM modelin daha iyi gerçekleştirilen biz karar varsayalım. Yapılacak ilk şey kaldırmak, bu nedenle [iki sınıflı destekli vektör makinesi] [ two-class-support-vector-machine] modülü ve eğitim için kullanılan modül. Tıklayarak denemenin bir kopyasını önce yapmak isteyebileceğiniz **Kaydet** deneme tuvalinin altındaki.

Aşağıdaki modüller silmek gerekir:  

* [İki sınıflı destekli vektör makinesi][two-class-support-vector-machine]
* [Modeli eğitme] [ train-model] ve [Score Model] [ score-model] ona bağlı modüller
* [Veri Normalleştir] [ normalize-data] (her ikisi)
* [Modeli değerlendirme] [ evaluate-model] (biz tamamlanmış değerlendirme modelleri için)

Her modülü seçin ve Delete tuşuna basın veya modül sağ tıklayıp **Sil**. 

![SVM modeli kaldırıldı][3a]

Modelimiz şöyle görünmelidir:

![SVM modeli kaldırıldı][3]

Bu modeli kullanarak dağıtmak hazırız artık [iki sınıflı artırılmış karar ağacı][two-class-boosted-decision-tree].

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Eğitim denemesini öngörücü bir denemeye dönüştürme

Bu model dağıtım için hazır hale getirmek için bu eğitim denemesini öngörücü bir denemeye dönüştürme ihtiyacımız var. Bu üç adımdan oluşur:

1. Modeli eğittiğimize ve ardından bizim eğitim modüllerinin yerine geçer kaydedin
2. Eğitim için yalnızca gerekli olan modülleri kaldırmak için denemeyi Kırp
3. Burada, web hizmeti giriş kabul eder ve burada Bu çıktıyı oluşturur tanımlayın

El ile yapabileceğimiz ancak tıklayarak üç adımı Neyse gerçekleştirilebilir **Web hizmetinin ayarı** deneme tuvalinin altındaki (ve seçerek **Tahmine dayalı Web hizmeti** seçeneği).

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
> Denemeyi deneme tuvalinin üst kısmında eklenen sekmeleri altında iki parça kaydedildiğini görebilirsiniz. Özgün eğitim denemesini sekmesi altında olduğunu **eğitim denemesini**, ve yeni oluşturulan Tahmine dayalı denemeye altındadır **Tahmine dayalı denemeye**. Tahmine dayalı denemeye sizi bir web hizmeti olarak dağıtacaksınız olur.

Biz bu belirli deneme ile ek bir adım uygulamanız gerekir.
İki ekledik [R betiği yürütme] [ execute-r-script] verileri için bir ağırlık işlevi sağlamak için modüller. Bu modüllerdeki son modelin kullanıma olabilmesi için eğitim ve test için ihtiyacımız bir kandırma oluştu.
Machine Learning Studio'da bir kaldırıldı [R betiği yürütme] [ execute-r-script] kaldırdığınızda, modül [bölünmüş] [ split] modülü. Şimdi biz diğer kaldırın ve connect [meta verileri Düzenleyicisi] [ metadata-editor] doğrudan [Score Model][score-model].    

Bizim deneme gibi görünmelidir:  

![Eğitim modeli Puanlama][4]  

> [!NOTE]
> Biz Tahmine dayalı denemeye UCI Almanca kredi kartı verileri veri kümesine neden sol merak ediyor olabilirsiniz. Hizmet değil özgün veri kümesinden kullanıcı verilerini puanlamak için neden özgün veri kümesinden modelde geliştirmediğinden geçiyor?
> 
> Hizmet asıl kredi kartı verileriyle gerektirmeyeceği geçerlidir. Ancak, şema vardır, kaç sütun gibi bilgileri içerir ve hangi sütunların sayısal bu veriler için gerekir. Bu şema bilgileri, kullanıcının verileri yorumlamak gereklidir. Biz bu bileşenlerin hizmet çalışırken, veri kümesi şemasını Puanlama modülü sahip olacak şekilde bağlı bırakın. Verileri kullanılmaz, yalnızca şema.  
> 
>Dikkat edilecek önemli bir unsur, özgün veri etiketi içeriyorsa, ardından web girdisinden beklenen şema ayrıca etiketli bir sütun beklediğiniz olan! Etiket ve diğer verileri, eğitim kümesinde olan ancak web girişlerinde web giriş ve eğitim veri kümesi, ortak bir modüle bağlanmadan önce olmaz, bu kullanacağınızı kaldırmaktır. 
> 

Son bir kez denemeyi çalıştırın (tıklayın **çalıştırma**.) Modelin hala çalıştığından emin olmak istiyorsanız, çıktısını tıklayın [Score Model] [ score-model] modülü ve select **sonuçlarını görüntüle**. Özgün veriler, kredi riski değeri ("Puanlanmış etiketler") ve puanlama olasılık değeri ("Puanlanmış olasılıklar".) ile birlikte görüntülendiğini görebilirsiniz. 

## <a name="deploy-the-web-service"></a>Web hizmetini dağıtma
Denemeyi ya da bir Klasik web hizmeti olarak veya Azure Resource Manager'a bağlı yeni bir web hizmeti olarak dağıtabilirsiniz.

### <a name="deploy-as-a-classic-web-service"></a>Klasik web hizmeti olarak dağıtma
Bizim denemeden türetilmiş bir Klasik web hizmetini dağıtmak için **Web hizmeti Dağıt** seçin ve tuval aşağıda **Web hizmeti dağıtma [Klasik]**. Machine Learning Studio web hizmeti olarak denemeyi dağıtır ve bu web hizmeti için panoya alır. Bu sayfadan deney için döndürebilir (**anlık görüntüsünü görüntüle** veya **görüntülemek son**) ve web hizmeti basit bir test çalıştırın (bkz **web hizmetini Test** aşağıda). (Daha fazla bilgi, bu kılavuzun sonraki adımda) web hizmetine erişebilen uygulamaları oluşturmak için buradaki bilgiler de mevcuttur.

![Web hizmet Panosu][6]

Tıklayarak hizmeti yapılandırabilirsiniz **yapılandırma** sekmesi. (Bunu verilen deney adı varsayılan olarak) hizmet adını burada değiştirebilirsiniz ve bir açıklama girin. Daha fazla kolay etiketler için girdi ve çıktı verilerini de tanıyabilirsiniz.  

![Web hizmetini yapılandır][5]  

### <a name="deploy-as-a-new-web-service"></a>Yeni bir web hizmeti dağıtma

> [!NOTE] 
> Yeni bir web hizmetini dağıtmak için web hizmetini dağıttığınız aboneliğe yeterli izinleri olmalıdır. Daha fazla bilgi için [Azure Machine Learning Web Hizmetleri portalını kullanarak bir web hizmetini yönetme](manage-new-webservice.md). 

Yeni bir web hizmetini dağıtmak için sunduğumuz denemeden türetilmiş:

1. Tıklayın **Web hizmeti Dağıt** seçin ve tuval aşağıda **Web hizmeti dağıtma [Yeni]**. Machine Learning Studio, Azure Machine Learning web hizmetlerini aktarır **dağıtma deneme** sayfası.

2. Web hizmeti için bir ad girin. 

3. İçin **fiyat planı**, bir devrenin fiyatlandırma planını seçin veya "Yeni Oluştur" seçebilir ve yeni plan bir ad verin ve aylık plan seçeneğini belirleyin. Hizmetinizi varsayılan bölgeden ve web hizmetiniz için planlar plan katmanları varsayılan bu bölgeye dağıtılır.

4. Tıklayın **dağıtma**.

Birkaç dakika sonra **hızlı** web hizmetiniz için sayfası açılır.

Tıklayarak hizmeti yapılandırabilirsiniz **yapılandırma** sekmesi. Hizmet burada değiştirebilirsiniz başlığı ve bir açıklama girin. 

Web hizmetini test etmek için **Test** sekme (bkz **web hizmetini Test** aşağıda). Web hizmetine erişebilen uygulamaları oluşturma hakkında daha fazla bilgi için tıklatın **Tüket** (Bu kılavuzda bir sonraki adımda, daha fazla ayrıntıya yazılacak) sekmesi.

> [!TIP]
> Bunu dağıttıktan sonra web hizmeti güncelleştirebilirsiniz. Örneğin, eğitim denemesini düzenleyebilir, sonra da, modelinizde değiştirmek istiyorsanız, model parametrelerinin ince ve tıklayın **Web hizmeti Dağıt**u seçerek **Web hizmeti dağıtma [Klasik]** veya **[Yeni] Web hizmetini dağıtma**. Denemeyi yeniden dağıttığınızda, artık güncelleştirilmiş model kullanarak web hizmeti değiştirir.  
> 
> 

## <a name="test-the-web-service"></a>Web hizmetini test edin

Web hizmeti erişim sağlandığında aracılığıyla kullanıcının veri girer **Web hizmeti giriş** burada da geçirilir Modülü [Score Model] [ score-model] modülü ve puanlanmış. Tahmine dayalı denemeye ayarladığımız şekilde, özgün kredi riski veri kümesi ile aynı biçimde veri modeli bekliyor.
Sonuçları kullanıcıya web hizmeti aracılığıyla döndürülen **Web hizmeti çıkış** modülü.

> [!TIP]
> Tüm yapılandırılmış, Tahmine dayalı denemeye sahibiz şekilde oluşur [Score Model] [ score-model] modülü döndürülür. Bu, tüm giriş verilerini ayrıca kredi riski değeri ve puanlama olasılık içerir. Ancak isterseniz farklı bir şey döndürebilir; Örneğin, yalnızca kredi riski değer döndürebilir. Bunu yapmak için INSERT bir [proje sütunları] [ project-columns] arasında Modülü [Score Model] [ score-model] ve **Web hizmeti çıkış**döndürmek için web hizmeti istemediğiniz sütunları ortadan kaldırmak için. 
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

2. Bir veri kümesini girin ve ardından **Tamam**. 

#### <a name="test-in-the-machine-learning-web-services-portal"></a>Machine Learning Web Hizmetleri portalında test etme

1. Üzerinde **PANO** sayfasında web hizmeti, **Test Önizleme** altında bağlantı **varsayılan uç nokta**. Azure Machine Learning Web Hizmetleri portalında web hizmeti uç noktası için test sayfası açılır ve hizmet için bir giriş verisi ister. Özgün kredi riski kümesinde görünen aynı sütunları şunlardır.

2. Tıklayın **Test istek-yanıt**. 

### <a name="test-a-new-web-service"></a>Yeni bir web hizmetini test edin

Yalnızca Machine Learning Web Hizmetleri portalında yeni bir web hizmetini test edebilirsiniz.

1. İçinde [Azure Machine Learning Web Hizmetleri](https://services.azureml.net/quickstart) portal'ı tıklatın **Test** sayfanın üstünde. **Test** sayfası açılır ve hizmet için veri girebilirsiniz. Görüntülenen giriş alanları, özgün kredi riski kümesinde görünen sütunlara karşılık gelir. 

2. Bir veri kümesini girin ve ardından **Test istek-yanıt**.

Test sonuçlarını, çıkış sütununun sayfanın sağ tarafında görüntülenir. 


## <a name="manage-the-web-service"></a>Web hizmetini yönetme

Web hizmetinizi Klasik veya yeni dağıttıktan sonra buradan yönetebilirsiniz [Microsoft Azure Machine Learning Web Hizmetleri](https://services.azureml.net/quickstart) portalı.

Web hizmetinizin performansını izlemek için:

1. Oturum [Microsoft Azure Machine Learning Web Hizmetleri](https://services.azureml.net/quickstart) portalı
2. Tıklayın **Web Hizmetleri**
3. Web hizmeti
4. Tıklayın **Panosu**

- - -
**Sonraki: [web hizmetine erişme](walkthrough-6-access-web-service.md)**

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
[project-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
