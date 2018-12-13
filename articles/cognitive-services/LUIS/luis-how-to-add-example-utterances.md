---
title: Örnek konuşmalar ekleme
titleSuffix: Language Understanding - Azure Cognitive Services
description: Örnek konuşma metin kullanıcı sorularınız ya da komutları örnekleridir. Language Understanding (LUIS) öğretmeyi bir amaç için örnek konuşma eklemeniz gerekir.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 33c941f84952faca1961bb65687b4098b837a2fd
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53139185"
---
# <a name="add-an-entity-to-example-utterances"></a>Bir varlık için örnek Konuşma ekleme 

Örnek konuşma metin kullanıcı sorularınız ya da komutları örnekleridir. Language Understanding (LUIS) öğretmeyi eklemeniz gerekir [örnek konuşma](luis-concept-utterance.md) için bir [hedefi](luis-concept-intent.md).

Genellikle, bir örnek utterance bir amaç için ilk olarak ekleyin ve varlıkları ve etiket konuşma niyetini sayfada oluşturup. Varlıkları ilk yerine oluşturacak olup [varlık Ekle](luis-how-to-add-entities.md).

## <a name="marking-entities-in-example-utterances"></a>Örnek konuşma varlıklarda işaretleme

Bir varlık için işaretlenecek örnek utterance metin seçtiğinizde, bir yerinde açılır menü görünür. Oluşturmak veya bir varlık seçmeniz için bu menüyü kullanın. 

Otomatik olarak etiketlendiğinden, önceden oluşturulmuş varlıklarla ve normal ifade varlıkları gibi belirli bir varlık türlerini örnek utterance etiketlenemez. 

## <a name="add-a-simple-entity"></a>Basit bir varlık ekleme

Aşağıdaki yordamda, oluşturun ve hedefi sayfasında aşağıdaki utterance içinde özel bir varlık etiketi:

```text
Does John Smith work in Seattle?
```

1. Seçin `Seattle` basit bir varlık olarak etiketlemek için utterance içinde.

    [![Basit bir varlık için utterance metin seçme işleminin ekran görüntüsü](./media/luis-how-to-add-example-utterances/hr-create-simple-1.png)](./media/luis-how-to-add-example-utterances/hr-create-simple-1.png)

    > [!NOTE]
    > Etiket sözcükleri varlıklar olarak seçerken:
    > * Yalnızca tek bir sözcük, onu seçin. 
    > * İki veya daha fazla sözcük kümesi için başında ve sonunda kümesi, ardından seçin.

1. Görüntülenen varlık açılan kutusunda için var olan bir varlığa seçin veya yeni varlık ekleyin. Yeni bir varlık eklemek için metin kutusuna adını yazın ve ardından **yeni varlık Oluştur**. 

    ![Varlık adı girme ekran görüntüsü](./media/luis-how-to-add-example-utterances/hr-create-simple-2.png)

1. İçinde **ne tür bir varlık oluşturmak istiyorsunuz?** açılır kutusunda, varlık adını doğrulayın ve seçin **basit** varlık türü ve ardından **Bitti**.

    A [tümcecik listesi](luis-concept-feature.md) genellikle bir varlığın sinyal artırmak için kullanılır.

## <a name="add-a-list-entity"></a>Bir liste varlık ekleme

Liste varlık (tam metni eşleşen) sabit, kapalı küme ilgili bir kelimelerin sisteminizde temsil eder. 

Bir şirketin bölüm listesi için değerleri normalleştirilmiş: `Accounting` ve `Human Resources`. Eş Anlamlılar her normalleştirilmiş bir adı vardır. Bir bölüm için tüm departman kısaltmalar, sayı veya argo bu eş anlamlıların içerebilir. Bir varlık oluşturduğunuzda tüm değerleri bilmeniz gerekmez. Daha fazla, eş anlamlı sözcüklerle gerçek kullanıcı konuşma gözden geçirdikten sonra ekleyebilirsiniz.

1. Belirli bir utterance için örnek utterance listesinde bir sözcük veya tümcecik yeni listesinde istediğiniz'ı seçin. Sonra üstteki metin kutusunda listenin adını girin ve ardından **yeni varlık Oluştur**.   

    ![Liste varlık adı girme ekran görüntüsü](./media/luis-how-to-add-example-utterances/hr-create-list-1.png)


1. İçinde **ne tür bir varlık oluşturmak istiyorsunuz?** açılır kutu, varlık adı ve select **listesi** türü olarak. Bu liste öğesinin eş anlamlılar eklemek ve ardından **Bitti**. 

    ![Liste varlık eş anlamlılar girme ekran görüntüsü](./media/luis-how-to-add-example-utterances/hr-create-list-2.png)

    Diğer konuşma etiketleme veya varlıktan düzenleyerek daha fazla liste öğelerini veya daha fazla öğe eş anlamlılar ekleyebilirsiniz **varlıkları** sol gezinti bölmesinde. [Düzenleme](luis-how-to-add-entities.md#add-list-entities) varlıkları girme ek öğeleri karşılık gelen, eş anlamlılar veya listesini alma seçeneği sunar. 

## <a name="add-composite-entity"></a>Bileşik varlık ekleme

Bileşik varlıklar, varolan oluşturulur **varlıkları** üst varlık içinde. 

Utterance varsayılarak `Does John Smith work in Seattle?`, bileşik bir utterance tek üst nesnenin altında çalışan adını ve konumunu varlık bilgileri döndürebilir. 

Çalışan, John Smith'in, önceden oluşturulmuş bir addır [personName](luis-reference-prebuilt-person.md) varlık. Konum, Seattle, özel bir basit varlıktır. Bu iki varlık oluşturulur ve bir örnek utterance etiketlenmiş sonra bu varlıkların bileşik bir varlıkta sarmalanabilir. 

1. Tek tek varlıklar bir bileşik kaydırmak için işaretleyin **ilk** (en soldaki) içinde utterance Bileşik varlık için varlık etiketli. Bu seçim seçenekleri gösteren bir açılan listesi görüntülenir.

1. Seçin **kaydırma Bileşik varlık** aşağı açılan listeden. 

    ![Ekran seçin "Bileşik varlığındaki kaydırma"](./media/luis-how-to-add-example-utterances/hr-create-composite-1.png)

1. Son sözcüğü Bileşik varlık (en sağ) seçin. Yeşil bir çizgi Bileşik varlık izleyen dikkat edin.

1. Aşağı açılan listeden Bileşik varlık adı girin.

    ![Ekran görüntüsü, aşağı açılan listeden Bileşik varlık adı girin](./media/luis-how-to-add-example-utterances/hr-create-composite-2.png)

    Varlıkları doğru Kaydır yeşil bir çizgi deyimin tamamını altında olur.

1. Bileşik varlık ayrıntıları üzerinde doğrulama **ne tür bir varlık oluşturmak istiyorsunuz?** açılır kutusunu seçip **Bitti**.

    ![Açılan ekran görüntüsü, varlık ayrıntıları](./media/luis-how-to-add-example-utterances/hr-create-composite-3.png)

1. Bileşik varlık hem mavi noktalar tek varlıklar için ve tüm Bileşik varlık için yeşil bir çizgi görüntüler. 

    ![Bileşik varlığı ile ekran görüntüsü, hedefleri Ayrıntıları sayfası](./media/luis-how-to-add-example-utterances/hr-create-composite-4.png)

## <a name="add-hierarchical-entity"></a>Hiyerarşik varlık ekleme

Hiyerarşik bir varlık, bağlamsal öğrenilen ve kavramsal olarak ilişkili varlıkları kategorisidir. Aşağıdaki örnekte, kaynak ve hedef konumları varlık içerir. 

Utterance içinde `Move John Smith from Seattle to Cairo`, Seattle kaynak konumu ve Cairo hedef konumu. Her kelime sırasını ve sözcük seçenek utterance bağlamsal olarak farklı ve öğrenilen konumdur.

1. Hedefi sayfasında utterance seçin `Seattle`, varlık adı enter `Location`ve ardından klavyedeki Enter'ı seçin.

    ![İletişim kutusunun ekran görüntüsü, oluşturma hiyerarşik varlık etiketleme](./media/luis-how-to-add-example-utterances/hr-hier-1.png)

1. İçinde **ne tür bir varlık oluşturmak istiyorsunuz?** açılır kutusunda _hiyerarşik_ için **varlık türü**, eklersiniz `Origin` ve `Destination` alt olarak ve ardından **Bitti**.

    ![Vurgulanan ToLocation varlıkla ekran görüntüsü, hedefleri Ayrıntıları sayfası](./media/luis-how-to-add-example-utterances/create-location-hierarchical-entity.png)

1. Utterance sözcüğü üst hiyerarşik varlıkla etiketlendi. Word'ün bir alt varlığına atamanız gerekir. Utterance için hedefi ayrıntı sayfasında dönün. Word ' ü seçin sonra oluşturduğunuz varlık adı aşağı açılan listeden seçin ve sağa doğru alt varlığı seçmeniz için menü izleyin.

    ![Word'ün bir alt varlık için Ata gerek duyduğunuz senaryolara ekran görüntüsü, hedefleri Ayrıntıları sayfası](./media/luis-how-to-add-example-utterances/hr-hier-3.png)

    >[!CAUTION]
    >Tek bir uygulamada tüm varlıklar üzerinde alt varlık adlarının benzersiz olması gerekir. Alt varlıklar aynı ada sahip iki farklı hiyerarşik varlıklar içerebilir. 

## <a name="entity-status-predictions"></a>Varlık durumu Öngörüler

LUIS Portalı'nda yeni utterance girdiğinizde, utterance varlık tahmin hataları olabilir. LUIS nasıl varlık belirlediğini ile karşılaştırıldığında nasıl bir varlık etiketli arasında bir fark tahmin hata var. 

Bu fark, utterance kırmızı alt çizgi ile LUIS Portalı'nda görsel olarak temsil edilir. Kırmızı alt çizgi, varlık köşeli ayraçlar içindeki ya da köşeli ayraç dışında görünebilir. 

![Ekran görüntüsü, varlık durumu tahmin uyuşmazlık](./media/luis-how-to-add-example-utterances/entity-prediction-error.png)

Utterance kırmızıyla altı çizili olan sözcükleri'ni seçin. 

Varlık kutu görüntüler **varlık durumu** tahmin tutarsızlık ise kırmızı ünlem işareti. Varlık durumu ile etiketlenmiş ve tahmin edilen varlıklar arasındaki fark hakkında daha fazla bilgi görmek için seçin **varlık durumu** sonra sağ öğeyi seçin.

![Öğe tahmin tutarsızlık düzeltmek doğru seçme işleminin ekran görüntüsü](./media/luis-how-to-add-example-utterances/entity-status.png)

Kırmızı çizgi aşağıdaki durumlarda hiçbirini görünebilir:

   * Varlık etiketli bir utterance girildiğinde, ancak önce
   * Varlık etiketi uygulandığında
   * Varlık etiketi, kaldırılır
   * Birden fazla varlık etiketi bu metni ne tahmini elde edildiğinde 

Aşağıdaki çözümlerden varlık tahmin tutarsızlık gidermek:

|Varlık|Görsel gösterge|Tahmin|Çözüm|
|--|--|--|--|
|Girilen utterance, henüz varlık etiketli değil.|kırmızı alt çizgi|Tahmin doğrudur.|Tahmin edilen değer ile varlık etiketi.|
|Etiketlenmemiş metin|kırmızı alt çizgi|Yanlış tahmin|Yanlış bu varlığı kullanan geçerli konuşma tüm amaçlarını gözden geçirilmesi gerekir. Geçerli konuşma LUIS, bu metin tahmin ettiği varlık olduğunu mistaught.
|Etiketli metin doğru|Mavi varlık vurgulayın, kırmızı alt çizgi|Yanlış tahmin|Daha fazla konuşma yerler ve kullanımları çeşitli doğru etiketli varlıkla sağlar. Geçerli konuşma bu LUIS öğretmeyi yeterli olduğu varlık ya da benzer varlıkları aynı bağlam içinde görünür olan. LUIS kafanız bu nedenle tek bir varlığa benzer varlık birleştirilmelidir. Başka bir çözüm bir kelimelerin önemi artırmak için bir ifade listesi eklemektir. |
|Yanlış etiketli metin|Mavi varlık vurgulayın, kırmızı alt çizgi|Doğru tahmin| Daha fazla konuşma yerler ve kullanımları çeşitli doğru etiketli varlıkla sağlar. 

## <a name="other-actions"></a>Diğer eylemler

Seçilen grubu veya ayrı bir öğe olarak örnek konuşma eylemleri gerçekleştirebilirsiniz. Seçili örnek konuşma grupları bağlamsal menü listesinin üst değiştirin. Tek öğeleri listesi ve bağlamsal tek nokta yukarıda bağlamsal menüyü her utterance satırının sonundaki kullanabilir. 

### <a name="remove-entity-labels-from-utterances"></a>Varlık etiketleri konuşma kaldırın

Hedefi sayfasında bir utterance makine öğrenilen varlık etiketleri kaldırabilirsiniz. Varlık makine öğrenilen değilse, bir utterance kaldırılamaz. Utterance bir makine öğrenilen varlık kaldırmanız gerekirse, tüm uygulamadan varlığı silmek gerekir. 

Bir utterance makine öğrenilen varlık etiketi kaldırmak için utterance varlığı seçin. Ardından **Etiketi Kaldır** varlık açılan kutusunda görünür.

![Ekran görüntüsü, hedefleri Ayrıntıları sayfası, vurgulanan Etiketi Kaldır](./media/luis-how-to-add-example-utterances/remove-label.png) 

### <a name="add-prebuilt-entity-label"></a>Önceden oluşturulmuş varlık etiketi Ekle

Önceden oluşturulmuş varlıklarla LUIS uygulamanıza eklediğinizde, bu varlıklarla etiketi konuşma gerek yoktur. Önceden oluşturulmuş varlıklar ve bunları nasıl ekleyeceğinizi hakkında daha fazla bilgi için bkz: [varlık Ekle](luis-how-to-add-entities.md#add-prebuilt-entity).

### <a name="add-regular-expression-entity-label"></a>Normal ifade varlık etiketi Ekle

LUIS uygulamanızı normal ifade varlıkları eklerseniz, bu varlıklarla etiketi konuşma gerekmez. Normal ifade varlıkları ve bunları nasıl ekleyeceğinizi hakkında daha fazla bilgi için bkz: [varlık Ekle](luis-how-to-add-entities.md#add-regular-expression-entities).


### <a name="create-a-pattern-from-an-utterance"></a>Bir utterance bir düzen oluşturma

Bkz: [hedefi veya varlık sayfasında mevcut utterance Ekle deseni](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).


### <a name="add-patternany-entity"></a>Pattern.Any varlık ekleme

LUIS uygulamanızı pattern.any varlıkları eklerseniz, bu varlıklarla konuşma etiketi olamaz. Bunlar yalnızca desenleri geçerli olur. Pattern.any varlıkları ve bunları nasıl ekleyeceğinizi hakkında daha fazla bilgi için bkz: [varlık Ekle](luis-how-to-add-entities.md#add-patternany-entities).

## <a name="train-your-app-after-changing-model-with-utterances"></a>Konuşma modeliyle değiştirdikten sonra uygulamanızı eğitin

Eklemek, düzenlemek ve kaldırmak, konuşma sonra [eğitme](luis-how-to-train.md) ve [yayımlama](luis-how-to-publish-app.md) uygulamanız için uç nokta sorguları etkilemek yaptığınız değişiklikleri. 

## <a name="next-steps"></a>Sonraki adımlar

Konuşma, hedefleri olarak etiketleme sonra artık oluşturabileceğiniz bir [Bileşik varlık](luis-how-to-add-entities.md).
