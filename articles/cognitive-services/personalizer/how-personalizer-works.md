---
title: Nasıl çalışır Personalizer - Personalizer
titleSuffix: Azure Cognitive Services
description: Personalizer bir bağlamda kullanmak için hangi eylemin bulmak için makine öğrenimini kullanıyor. Her öğrenme döngüsü, yalnızca kendisine derecesini gönderdiğiniz ve ödül çağıran veriler üzerine geliştirilen bir modeli vardır. Her öğrenme döngüsü birbirinden tamamen bağımsızdır.
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 06/07/2019
ms.author: edjez
ms.openlocfilehash: d0a0a6101d876493188426d19f2fa845d20ee274
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055476"
---
# <a name="how-personalizer-works"></a>Kişiselleştirme nasıl çalışır?

Personalizer bir bağlamda kullanmak için hangi eylemin bulmak için makine öğrenimini kullanıyor. Özel olarak kendisine gönderilen veriler üzerinde eğitilmiş bir modeli öğrenme döngüsü her sahip **derece** ve **ödül** çağırır. Her öğrenme döngüsü birbirinden tamamen bağımsızdır. Her bir bölümü veya kişiselleştirmek isteyip uygulamanızın davranışını bir öğrenme döngüsü oluşturun.

Her bir döngü için **ile derece API'ye çağrı** geçerli bağlam üzerinde alan:

* Olası eylemler listesi: içerik öğeleri, üst eylemi seçin.
* Listesi [bağlam özellikleri](concepts-features.md): kullanıcı, içerik ve bağlam gibi ilgili bağlamsal veriler.

**Derece** API karar ya da kullanılacak:

* _Exploit_: Geçmiş verilere dayanan en iyi eylem karar vermek için geçerli model.
* _Keşfedin_: Üst eylem yerine farklı bir eylem seçin.

**Ödül** API:

* Her bir derece görüşmesinin Ödül puanları ve özellikleri kaydederek modeli eğitmek için verileri toplar.
* Belirtilen ayarlara dayanan modeli güncelleştirmek için bu verileri kullanan _öğrenme ilke_.

## <a name="architecture"></a>Mimari

Aşağıdaki görüntüde sıralama ve ödül çağrıları çağırma mimari akışı gösterilmektedir:

![Alternatif metin](./media/how-personalizer-works/personalization-how-it-works.png "kişiselleştirme nasıl çalışır")

1. Personalizer eylemin boyut belirlemek için bir iç yapay ZEKA modeli kullanır.
1. Hizmet geçerli model yararlanma ya da yeni seçenekleri modeli için keşfetmek karar verir.  
1. Derecelendirme sonucu, Event Hubs'a gönderilir.
1. Personalizer ödül aldığında, Event Hubs'a ödül gönderilir. 
1. Boyut sayısı ve ödül ilişkilendirilir.
1. Yapay ZEKA modeli bağıntı sonuçlarına göre güncelleştirilir.
1. Çıkarımı altyapısının, yeni modeli ile güncelleştirilir. 

## <a name="research-behind-personalizer"></a>Personalizer arkasında araştırma

Personalizer, üstün Bilim ve araştırma alanında üzerinden hesaplanmıştır [pekiştirmeye dayalı öğrenme](concepts-reinforcement-learning.md) raporlar, araştırma etkinlikleri ve devam eden bir araştırma alanlarının Microsoft Research dahil olmak üzere.

## <a name="terminology"></a>Terminoloji

* **Döngü öğrenme**: Kişiselleştirme özelliğinden faydalanabilir, uygulamanızın her bir bölümü için öğrenme döngüsü oluşturabilirsiniz. Birden fazla deneyimini kişiselleştirmek için varsa, her biri için bir döngü oluşturun. 

* **Eylemler**: Ürün ve promosyonlar, aralarından seçim yapabileceğiniz gibi içerik öğeleri eylemlerdir. Personalizer olarak bilinen, kullanıcılarınıza göstermek için en iyi eylemi seçer _ödüllendirin eylem_, derece API aracılığıyla. Her eylem, özellikleri derece istekle beraber gönderilen olabilir.

* **Bağlam**: Daha doğru bir sıra sağlamak için örneğin, bağlamıyla ilgili bilgileri sağlayın:
    * Kullanıcı.
    * Cihaz öğrenebilir. 
    * Geçerli saat.
    * Diğer veriler geçerli durumu hakkında.
    * Kullanıcı veya bağlam ilgili geçmiş verileri.

    Özel uygulamanızın farklı bağlam bilgileri olabilir. 

* **[Özellikleri](concepts-features.md)** : Bir içerik öğesi veya kullanıcı bağlamı hakkında bilgi birimidir.

* **Ödül**: Kullanıcı sıralama API'sine nasıl yanıt verdiğini ölçü eylemi, 0 ile 1 arasında bir puan döndürdü. 0-1 değeri, kişiselleştirme iş hedeflerinize ulaşmak istediğiniz nasıl Yardım üzerinde temel iş mantığınızı tarafından ayarlanır. 

* **Araştırma**: En iyi eylem döndürmek yerine, bu kullanıcı için farklı bir eylem seçtiğinde Personalizer hizmet araştırmaktadır. Personalizer hizmet değişikliklerini, stagnation, önler ve devam eden kullanıcı davranışlarına yönelik inceleyerek uyarlayabilirsiniz. 

* **Deneme süresi**: Personalizer hizmeti bir ödül için bekleyeceği süreyi, derece çağrı andan itibaren başlayarak, olay için oluştu.

* **Etkin olmayan olaylar**: Etkin olmayan bir olayın nerede derece çağrıldı, ancak kullanıcı hiç sonuç, istemci uygulama kararları nedeniyle görürsünüz emin değilseniz olan biridir. Etkin olmayan olaylar oluşturma ve kişiselleştirme sonuçlarını depolamak için izin verin ve sonra bunları daha sonra machine learning modeli etkilemeden atmak mı karar verin.

* **Model**: Personalizer modeli, tüm verileri sıralama ve ödül çağrıları ve öğrenme İlkesi tarafından belirlenen bir eğitim davranışı gönderdiğiniz bağımsız değişkenler birleşimi eğitim verilerini almaya kullanıcı davranışı hakkında bilgi edindiniz yakalar. 

## <a name="example-use-cases-for-personalizer"></a>Örnek kullanım örnekleri için Personalizer

* Hedefi açıklama & Kesinleştirme: kullanıcılarınız kullansın daha iyi bir deneyim bunların amacı olduğunda Yardım işaretini her kullanıcı için kişiselleştirilmiş bir seçenek sağlayarak.
* Varsayılan menüler ve seçenekleri için öneriler: büyük olasılıkla öğesi bir impersonal menü veya alternatifleri listesi sunmak yerine bir ilk adım olarak kişiselleştirilmiş bir biçimde Öner bot sahip.
* Bot nitelikleri & sesi: kişiselleştirilmiş bir yolla bu nitelikleri değişen ton, ayrıntı ve yazma stili değişebilir botlar için göz önünde bulundurun.
* & Uyarı bildirim içeriği: kullanıcıların daha etkileşim kurmak için kullanılacak metni uyarıları karar verin.
* & Uyarı zamanlama bildirim: daha fazla bunları etkileşim kurmak amacıyla kullanıcılara bildirimler gönderme zamanı öğrenilmesine kişiselleştirilmiş.

## <a name="checklist-for-applying-personalizer"></a>Personalizer uygulamak için Denetim listesi

Durumlarda Personalizer uygulayabilirsiniz burada:

* Uygulamanız için bir işletmeyi ya da kullanılabilirlik hedef vardır.
* Uygulamanızda ne göstermek bağlamsal bir karar kullanıcılara sağlama bu hedefe burada artıracak bir yer var.
* En iyi seçim yapabilir ve toplu kullanıcı davranışı ve toplam ödül puan öğrendiniz.
* Kişiselleştirme için makine öğrenimi kullanımını izleyen [sorumlu kullanım yönergeleri](ethics-responsible-use.md) ve ekibiniz için Seçenekler.
* Karar en iyi seçenek sıralaması olarak ifade edilebilir ([eylem](concepts-features.md#actions-represent-a-list-of-options) seçenekleri sınırlı bir dizi.
* Bu seçimi ne kadar iyi çalışan, iş mantığı tarafından kullanıcı davranışı bazı yönlerini ölçme ve -1 ile 1 arasında bir sayı olarak ifade hesaplanabilir.
* Ödül puan çok fazla karışıklık veya dış etkenler getirmek değil, özellikle deneme süresi hala ilgili olsa ödül puan hesaplanabilir kadar düşük olur.
* Boyut bağlamı düşündüğünüz en az 5 özellikler sözlüğü doğru seçim yapmanıza yardımcı ve bu kişisel bilgi içermez ifade edebilirsiniz.
* Her eylem bir sözlük olarak en az 5 öznitelikleri hakkında bilgi varsa veya düşündüğünüz özellikleri Personalizer doğru seçim yapmanıza yardımcı olur.
* Verileri uzun süre tutmak en az 100.000 etkileşimleri geçmişini ulaşıncaya kadar yeterli.

## <a name="machine-learning-considerations-for-applying-personalizer"></a>Machine learning Personalizer uygulamak için dikkat edilmesi gerekenler

Personalizer öğrenme, bunu size geri bildirim tarafından verilen, makine öğrenimine bir yaklaşım pekiştirmeye üzerinde temel alır. 

Personalizer bilgi edinin durumlarda en iyi yeri:
* Sorun (örneğin, haber veya biçimde tercihlerinde) zaman içinde drifts varsa üzerine en iyi kişiselleştirme kalmak için yeterli olayları yoktur. Gerçek dünyada sürekli değişikliği personalizer Uyarlanır ancak sonuçları yok ise yeterli olayları ve verileri yeni eğilimlere keşfetmek uzmanlardan en uygun olmayacaktır. Sıklıkta gerçekleşen bir kullanım örneği seçmeniz gerekir. En az 500 kereden günde gerçekleşen kullanım örnekleri aranıyor göz önünde bulundurun.
* Bağlam ve Eylemler öğrenme süreçlerini kolaylaştırmasına için yeterli özellikleri vardır.
* Çağrı başına derece için 50'den az eylemler vardır.
* Veri saklama ayarlarınıza Personalizer çevrimdışı değerlendirmeleri ve ilke iyileştirme gerçekleştirmek için yeterli veri toplamaya izin verin. 50\. 000'en az bir veri noktaları genellikle budur.

## <a name="how-to-use-personalizer-in-a-web-application"></a>Bir web uygulamasında Personalizer kullanma

Bir web uygulaması için bir döngü ekleme içerir:

* Hangi kişiselleştirmek için deneyimi belirlemek, hangi eylemleri ve kullandığınız özellikler, hangi bağlam özelliklerini kullanma ve hangi ödül ayarlarsınız.
* Uygulamanızda kişiselleştirme SDK'sına bir başvuru ekleyin.
* Kişiselleştirmek hazır olduğunuzda derece API çağrısı.
* EventID Store. Daha sonra ödül API'si ile bir ödül gönderdiğiniz.
1. Etkinleştirme kullanıcı kişisel sayfanıza yakaladı emin olduğunuzda olayı için çağırın.
1. Kullanıcı Seçimi dereceli içerik için bekleyin. 
1. Ödül ne kadar iyi sıralama API çıkışını yaptığınız belirtmek için API çağrısı.

## <a name="how-to-use-personalizer-with-a-chat-bot"></a>Personalizer bir sohbet bot ile kullanma

Bu örnekte, her kullanıcının bir dizi menü veya seçenekler aşağı göndermek yerine varsayılan öneride bulunmak için kişiselleştirme kullanmayı görürsünüz.

* Alma [kod](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/tree/master/samples/ChatbotExample) Bu örnek için.
* Bot çözümünüzü ayarlayın. LUIS uygulamanızı yayımlamak emin olun. 
* Boyut sayısı ve ödül API çağrıları için robot yönetin.
    * LUIS yaklaşım işlemesine yönetmek için kod ekleyin. Varsa **hiçbiri** döndürülen hedefleri listesinde Personalizer derece için için ıntents üst amaç veya üst amaç 's puanı, iş mantığı eşiğin altında olduğu gibi gönderin.
    * Amaç listesi kullanıcıya derece API yanıtından en iyi hedefi olan ilk amacıyla seçilebilir bağlantılar olarak gösterir.
    * Kullanıcının seçiminin yakalayın ve bunu ödül API çağrısında gönderin. 

### <a name="recommended-bot-patterns"></a>Önerilen bot desenleri

* Her kullanıcı için sonuçları önbelleğe alma aksine bir Kesinleştirme gereklidir her zaman Personalizer derece API çağrıları yapabilir. Hedefi belirsizliğini sonucu tek bir kişi için zaman içinde değişebilir ve varyanslar keşfetmek sıralama API izin vererek genel öğrenme hızlandırır.
* Çok sayıda kullanıcı cihazını yaygındır ve böylece kişiselleştirmek için yeterli veri sahip bir etkileşim'i seçin. Örneğin, tanıtım soruları daha iyi uygun daha küçük açıklamalar olabilir derin yalnızca bazı kullanıcılar için alabilirsiniz konuşma grafiğinde.
* "İlk öneri doğru" etkinleştirmek için sıralama API çağrılarını kullanmak konuşmaları, kullanıcı burada sorulan "İster X?" ya da "X kastettiniz?" ve kullanıcı yalnızca doğrulayabilirsiniz. Seçenekler burada menüden seçmelisiniz kullanıcıya sunan aksine. Örneğin, kullanıcı: "Bir kahve sipariş istiyorum" Bot: "çift espresso ister misiniz?". Bir öneri için doğrudan ilgili olarak bu şekilde ödül de güçlü sinyaldir.

## <a name="how-to-use-personalizer-with-a-recommendation-solution"></a>Bir öneri Çözümle Personalizer kullanma

Büyük bir katalog derece API'ye gönderilen 30 olası eylemler olarak sunulabilir birkaç öğelerine aşağı filtrelemek için öneri Altyapısı'nı kullanın.

Öneri altyapıları ile Personalizer kullanabilirsiniz:

* Ayarlanan [öneri çözüm](https://github.com/Microsoft/Recommenders/). 
* Bir sayfa görüntüleme önerileri kısa listesini almak için öneri modeli çağırın.
* Öneri çözümünün çıktı derecelendirmek için kişiselleştirme çağırın.
* Kullanıcı eyleminizi ödül API çağrısı ile ilgili geri bildirim gönderin.


## <a name="pitfalls-to-avoid"></a>Kaçınılacak Tuzaklar

* Burada kişiselleştirilmiş davranışı tüm kullanıcılar arasında bulunan bir şey ancak bunun yerine bir şey belirli kullanıcılar için anımsanacak değilse veya bir kullanıcıya özgü seçenekleri listesinden gelen Personalizer kullanmayın. Örneğin, bir ilk 20 olası menü öğeleri listesi pizza siparişi yararlıdır, ancak kullanıcıların çağırmak için ilgili kişi kişi listesi childcare (örneğin, "büyükanne") ile ilgili Yardım gerektiren kişiselleştirilebilir arasında bir şey olmadığında önermek Personalizer kullanma Kullanıcı tabanınızı.


## <a name="adding-content-safeguards-to-your-application"></a>İçerik koruma uygulamanıza ekleme

Uygulamanız için kullanıcıların ve bazıları için gösterilen içeriği güvenli olmayan veya bazı kullanıcılar için uygun olabilir, emin olmak önceden planlamanız gerekir içerik büyük sapmalar izin veriyorsa doğru korumaları görmesini, kullanıcılarınızın kabul edilemez önlemek içindir içeriği. Koruma uygulamak için en iyi modelidir: Koruma uygulamak için en iyi modelidir:
    * Sıralama için Eylemler listesini edinin.
    * Hedef kitle için uygun olmayan olanları filtreleyin.
    * Yalnızca bu uygulanabilir Eylemler Sırala.
    * Kullanıcı eylemine sıralanmış üst görüntüler.

Bazı mimarilerde yukarıdaki dizisi uygulamak zor olabilir. Bu durumda, derecelendirme sonra koruma uygulamak için alternatif bir yaklaşım yoktur, ancak provizyon koruma dışında kalan Eylemler Personalizer modeli eğitmek için kullanılmayan şekilde yapılması gerekir.

* Devre dışı öğrenmeye sıralaması için Eylemler listesi alın.
* Derece eylemler.
* Üst eylem uygulanabilir olup olmadığını denetleyin.
    * Üst işlem kurtarılabilir ise, bu dereceyi için öğrenme etkinleştirin ve ardından kullanıcıya göstermek.
    * Üst eylem uygulanabilir değilse değil Bu sıralama için öğrenme etkinleştirmek ve kendi mantığı ya da alternatif yaklaşımlar karar kullanıcıya göstermek. İkinci en iyi kişilerinin sıralı seçeneği kullansanız bile, bu sıralama için öğrenme etkinleştirmeyin.

## <a name="verifying-adequate-effectiveness-of-personalizer"></a>Yeterli verimliliğini Personalizer doğrulanıyor

Gerçekleştirerek Personalizer etkisini düzenli aralıklarla izleyebilirsiniz [çevrimdışı değerlendirmeleri](how-to-offline-evaluation.md)

## <a name="next-steps"></a>Sonraki adımlar

Anlamak [Personalizer kullanabileceğiniz](where-can-you-use-personalizer.md).