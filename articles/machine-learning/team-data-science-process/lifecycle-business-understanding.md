---
title: "İş anlama takım veri bilimi işlem yaşam döngüsü - Azure aşaması | Microsoft Docs"
description: "Hedefler, görevler ve veri bilimi projelerinizi iş anlama aşaması için teslim edilebilir."
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
ms.openlocfilehash: 2adce61f8185bd86a6a870bb5752fe936701b0af
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="business-understanding"></a>Kurumsal yaklaşım

Hedefler, görevler, bu konuda özetlenir ve sonuçlara ilişkili **iş anlama aşama** takım veri bilimi işleminin. Bu işlem, veri bilimi projelerinizi yapısı için kullanabileceğiniz önerilen bir yaşam döngüsü sunar. Yaşam döngüsü projeleri genellikle genellikle tekrarlayarak yürütme, önemli aşamaları ana hatlarıyla gösterilir:

* **İş anlama**
* **Veri alımı ve anlama**
* **Modelleme**
* **Dağıtım**
* **Müşteri kabulü**

Görsel gösterimi işte **takım veri bilimi işlemi yaşam döngüsü**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goals"></a>Hedefleri
* **Anahtar değişkenleri** olarak hizmet verecek olan belirtilen **model hedefleri** ve ilgili olan ölçümleri kullanılır projesi için başarı belirleyin.
* İlgili **veri kaynakları** iş erişimi veya almak gereken tanımlanır.

## <a name="how-to-do-it"></a>Bunu nasıl
Bu aşamada ele iki ana görevleri şunlardır: 

* **Hedefleri tanımlayın**: anlamak ve iş sorunlarını belirlemek için müşteri ve diğer Paydaşlar ile çalışır. İş hedefleri tanımlayın ve veri bilimi teknikleri hedefleyebilirsiniz soruları formüle.
* **Veri kaynaklarını tanımlama**: Bul yardımcı olan ilgili verilerin proje amaçlarını tanımlama soruları yanıtlayın.

### <a name="11-define-objectives"></a>1.1 hedefleri tanımlayın

1. Anahtar tanımlamak için bu adımın merkezi bir hedefi olan **iş değişkenleri** tahmin etmek analiz gereken. Bu değişkenler denir **model hedefleri** ve bunlarla ilişkilendirilmiş ölçümleri proje başarısını belirlemek için kullanılır. İki tür hedefleri satış tahmini veya sahte olan bir sırayı olasılığını gösterilebilir.

2. Tanımlamak **Proje hedefleri** soran ve ilgili ve belirli ve anlaşılır "diyez" soruları iyileştirme. Veri bilimi gibi soruları yanıtlamak için adlarını ve numaralarını kullanarak işlemidir. Sharp sorular sormak daha fazla bilgi için bkz: [veri bilimi nasıl](https://blogs.technet.microsoft.com/machinelearning/2016/03/28/how-to-do-data-science/) blogu. Veri bilimi / machine learning genellikle beş tür soruları yanıtlamak için kullanılır:
 
   * Ne kadar veya kaç tane? (regresyon)
   * Hangi kategori? (sınıflandırma)
   * Hangi Grup? (kümeleme)
   * Bu tuhaf mi? (anomali algılama)
   * Hangi seçeneği yapılması gerekir? (öneri)

    Hangi soruları istiyoruz ve nasıl yanıtlama, iş hedeflerinize başarır belirler.

3. Tanımlamak **takım projesi** roller ve sorumluluklar üyeleri belirterek. Daha fazla bilgi keşfedilen üzerinde yinelemek bir üst düzey kilometre taşı planı geliştirin.  

4. **Başarı ölçümlerini tanımlayın**. Örneğin: karmaşıklığını azaltmak için promosyon sunuyoruz böylece Itanium tabanlı sistemler için müşteri karmaşası tahmin doğruluğu, %x bu 3 aylık proje ucu tarafından elde etmek. Ölçümleri olmalıdır **akıllı**: 
   * **S**elirli 
   * **M**easurable
   * **A**chievable 
   * **R**elevant 
   * **T**IME bağlama 

### <a name="12-identify-data-sources"></a>1.2 veri kaynaklarını tanımlama
Sharp sorularınıza bilinen örnekleri içeren veri kaynakları belirleyin. Aşağıdaki veriler için bakın:

* Verileri **Relevant** soruya. Hedef ve hedef ilgili özelliklerin ölçüleri sahibiz?
* Verileri bir **doğru ölçü** bizim modeli hedef ve ilgi özellikleri.

Örneğin, varolan sistemler toplamak ve sorun gidermek ve proje hedeflerinize ulaşmak için veri ek türlerini oturum gerektiğini bulmak için genel olarak görülmez, değil. Bu durumda, dış veri kaynakları için konum veya yeni verileri toplamak için sistemlerinizi güncelleştirme isteyebilirsiniz.

## <a name="artifacts"></a>Yapıtlar
Bu aşamada teslim edilebilir öğeler şunlardır:

* [**Kurucu belge**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Charter.md): standart şablon TDSP Proje yapısı tanımında sağlanır. Bu proje boyunca yeni bulmaları yapılan ve iş gereksinimleri değiştikçe güncelleştirilen bir yaşam belgedir. Daha fazla ayrıntı, bulma işlemi ilerlemeyi olarak ekleme, bu belgedeki bağlı yinelemek için kullanılan anahtardır. Müşteri tutmak ve diğer Paydaşlar değişiklikler yaparken söz konusu ve bunları değişiklikler nedeniyle açık bir şekilde iletir.  
* [**Veri kaynakları**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#raw-data-sources): Bu **ham veri kaynakları** bölümünü **veri tanımlarını** TDSP projesinde bulunan rapor **veri raporu** klasör. Ham verileri için özgün ve hedef konumları belirtir. Sonraki aşamalarda, analitik ortamınız için verileri taşımak için komut dosyaları gibi ek ayrıntıları doldurun.  
* [**Veri sözlükler**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/DataDictionaries): Bu belgede istemci tarafından sağlanan veri açıklamalarını sağlar. Bu açıklamalar şemanın (veri türleri, bilgi doğrulama kuralları, eğer varsa) ve varlık ilişkisi diyagramları varsa hakkındaki bilgileri içerir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıda, her takım veri bilimi işlemi yaşam döngüsü adımda bağlantıları verilmiştir:

* [1. İş anlama](lifecycle-business-understanding.md)
* [2. Veri alımı ve anlama](lifecycle-data.md)
* [3. Modelleme](lifecycle-modeling.md)
* [4. Dağıtım](lifecycle-deployment.md)
* [5. Müşteri kabulü](lifecycle-acceptance.md)

Tam işlem için tüm adımları gösteren uçtan uca talimatlara **belirli senaryolar** de sağlanır. Listelenen ve küçük resim açıklamasında ile bağlantılı [örnek izlenecek yollar](walkthroughs.md) konu. Bunlar, bulut, şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl koruduğu gösterilmiştir. 

Azure Machine Learning Studio'da kullanmak adımları takım veri bilimi işlemde çalışan örnekler için bkz: [ile Azure ML](http://aka.ms/datascienceprocess) öğrenme yolu.