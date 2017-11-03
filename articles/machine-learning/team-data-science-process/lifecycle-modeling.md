---
title: "Takım veri bilimi işlem yaşam döngüsü - Azure aşaması modelleme | Microsoft Docs"
description: "Hedefler, görevler ve veri bilimi projelerinizi modelleme aşaması için teslim edilebilir."
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/02/2017
ms.author: bradsev;
ms.openlocfilehash: 0c855faa96d7feb9f762ecaed7654669a5a8a5b7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="modeling"></a>Modelleme

Hedefler, görevler, bu konuda özetlenir ve sonuçlara ilişkili **aşama modelleme** takım veri bilimi işleminin. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sunar. Yaşam döngüsü projeleri genellikle genellikle tekrarlayarak yürütme, önemli aşamaları ana hatlarıyla gösterilir:

* **İş anlama**
* **Veri alımı ve anlama**
* **Modelleme**
* **Dağıtım**
* **Müşteri kabulü**

Görsel gösterimi işte **takım veri bilimi işlemi yaşam döngüsü**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Hedefleri
* Machine learning modelini en iyi veri özellikleri.
* Hedef en doğru tahmin bilgilendirici bir ML model.
* Üretim için uygun bir ML model.

## <a name="how-to-do-it"></a>Bunu nasıl
Bu aşamada ele üç ana görevleri şunlardır:

* **Özellik Mühendisliği**: model eğitim kolaylaştırmak için ham verileri veri özellikleri oluşturun.
* **Model Eğitimi**: kendi başarı ölçümlerini karşılaştırarak soru en doğru bir şekilde yanıtlar modeli bulunamadı.
* Modelinizi olup olmadığını belirlemek **üretim için uygun**.

### <a name="31-feature-engineering"></a>3.1 özellik Mühendisliği
Özellik Mühendisliği ekleme, toplama ve analiz kullanılan özellikler oluşturmak için ham değişkenleri dönüşümü içerir. Bir model yönlendiren içine Insight istiyorsanız, özellikleri birbirleriyle nasıl ilişkili olduğunu ve bu özellikleri kullanmak için machine learning algoritmaları şeklini anlamanız gerekir. Bu adım, etki alanı uzmanlık ve veri araştırması adımda elde edilen Öngörüler yaratıcı bir birleşimini gerektirir. Bu, bulma ve çok sayıda ilgisiz değişkenleri kaçınarak bilgilendirici değişkenleri dahil, bir dengeleme işidir. Bilgilendirici değişkenleri bizim sonucunu iyileştirmek; İlişkisiz değişkenleri gereksiz bir gürültü modeline tanıtır. Puanlama sırasında elde edilen yeni veriler için bu özellikleri oluşturmak gerekir. Bu nedenle bu özellikler nesil yalnızca Puanlama zamanında mevcut olan verileri üzerinde bağlı olabilir. Azure veri teknolojiler kullanılırken özellik Mühendisliği hakkında teknik yönergeler için bkz [özellik Mühendisliği veri bilimi işleminde](create-features.md). 

### <a name="32-model-training"></a>3.2 model eğitimi
Yanıt çalıştığınız sorusunu türüne bağlı olarak, kullanılabilir birçok modelleme algoritması vardır. Algoritmalar seçme ile ilgili yönergeler için bkz: [için Microsoft Azure Machine Learning algoritmaları seçme](../studio/algorithm-choice.md). Bu makale, Azure Machine Learning için yazılmıştır rağmen sağladığı Kılavuzu projeleri öğrenme herhangi bir makine için yararlıdır. 

İşlem modeli eğitim için aşağıdaki adımları içerir: 

* **Giriş verisi bölme** rastgele bir eğitim veri kümesi ve bir sınama veri kümesi modelleme için.
* **Modelleri oluşturabilir** eğitim veri kümesi kullanma.
* **Değerlendirme** (eğitim ve sınama veri kümesi) rakip machine learning algoritmaları çeşitli yanı sıra, bir dizi ilişkili geçerli verilerle ilgi soruyu yanıtlarken doğru sağlamıştır (parametre tarama da bilinir) parametreleri ayarlama.
* **"En iyi" çözümü belirlemenize** alternatif yöntemler arasında başarı ölçüm karşılaştırarak soruyu yanıtlamak için.

> [!NOTE]
> **Sızıntısını önlemek**: veri sızıntısını neden bir model veya unrealistically iyi tahminlerde için makine öğrenme algoritmasını sağlayan eğitim veri kümesi dışından verileri ekleme. Sızıntısını neden, Tahmine dayalı sonuçlar almak zaman bilimcilerine tedirgin veri doğru olması iyi göründüğü ortak bir nedeni. Bu bağımlılıklar algılamak zor olabilir. Genellikle sızıntısını önlemek için bir çözümleme veri kümesi oluşturma, bir model oluşturma ve doğruluk değerlendirme arasında yineleme gerektirir. 
> 
> 

Sağladığımız bir [otomatik modelleme ve Raporlama Aracı](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) birden çok algoritmaları ve parametre bir taban çizgisi modeli oluşturmak üzere dağılımlarında çalıştırabilir TDSP ile. Ayrıca, rapor değişken önem dahil olmak üzere her modeli ve parametre birleşimi performansını özetlemeye modelleme bir temel oluşturur. Bu işlem ayrıca, daha fazla özellik Mühendisliği sürücüsü olarak yinelemelidir. 

## <a name="artifacts"></a>Yapıtlar
Bu aşamada üretilen yapılar içerir:

* [**Özellik kümeleri**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#feature-sets): modelleme için geliştirilen özellikleri açıklanan **özellik kümeleri** bölümünü **veri tanımı** rapor. Özellikler ve özellik nasıl oluşturulan üzerinde açıklama oluşturmak için kodu işaretçiler içerir.
* [**Rapor modeli**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): denenir, her model için bir standart her deneme hakkında ayrıntılar sağlayan şablon tabanlı rapor üretilir.
* **Denetim noktası karar**: model üretim sisteme dağıtmak için yeterli düzgün çalışıp olup olmadığını değerlendirin. Sorulacak anahtar bazı sorular şunlardır:
  * Modeli belirtilen test veri yeterli güvenle soruyu yanıtlamak mu? 
  * Herhangi bir alternatif yaklaşımlar denemelisiniz: ek veri toplamak, daha fazla özellik Mühendisliği yapın ya da diğer algoritmalarıyla denemeler?

## <a name="next-steps"></a>Sonraki adımlar

Aşağıda, her takım veri bilimi işlemi yaşam döngüsü adımda bağlantıları verilmiştir:

* [1. İş anlama](lifecycle-business-understanding.md)
* [2. Veri alımı ve anlama](lifecycle-data.md)
* [3. Modelleme](lifecycle-modeling.md)
* [4. Dağıtım](lifecycle-deployment.md)
* [5. Müşteri kabulü](lifecycle-acceptance.md)

Tam işlem için tüm adımları gösteren uçtan uca talimatlara **belirli senaryolar** de sağlanır. Listelenen ve küçük resim açıklamasında ile bağlantılı [örnek izlenecek yollar](walkthroughs.md) konu. Bunlar, bulut, şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl koruduğu gösterilmiştir. 

Azure Machine Learning Studio'da kullanmak adımları takım veri bilimi işlemde çalışan örnekler için bkz: [ile Azure ML](http://aka.ms/datascienceprocess) öğrenme yolu. 