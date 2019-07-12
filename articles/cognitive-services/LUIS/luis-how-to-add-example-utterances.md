---
title: Örnek konuşmalar ekleme
titleSuffix: Language Understanding - Azure Cognitive Services
description: Örnek konuşma metin kullanıcı sorularınız ya da komutları örnekleridir. Language Understanding (LUIS) öğretmeyi bir amaç için örnek konuşma eklemeniz gerekir.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: af43697dc864019c6fd86b041e231a54d8aa8b40
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827486"
---
# <a name="add-an-entity-to-example-utterances"></a>Bir varlık için örnek Konuşma ekleme 

Örnek konuşma metin kullanıcı sorularınız ya da komutları örnekleridir. Language Understanding (LUIS) öğretmeyi eklemeniz gerekir [örnek konuşma](luis-concept-utterance.md) için bir [hedefi](luis-concept-intent.md).

Genellikle, hedefi bir ilk örnek utterance ekleyin ve ardından varlıkları oluşturun ve konuşma etiket **hedefleri** sayfası. Varlıkları ilk yerine oluşturacak olup [varlık Ekle](luis-how-to-add-entities.md).

## <a name="marking-entities-in-example-utterances"></a>Örnek konuşma varlıklarda işaretleme

Bir varlık için işaretlenecek örnek utterance metin seçtiğinizde, bir yerinde açılır menü görünür. Oluşturmak veya bir varlık seçmeniz için bu menüyü kullanın. 

Otomatik olarak etiketlendiğinden, önceden oluşturulmuş varlıklarla ve normal ifade varlıkları gibi belirli bir varlık türlerini örnek utterance etiketlenemez. 

## <a name="add-a-simple-entity"></a>Basit bir varlık ekleme

Aşağıdaki yordamda, oluşturmak ve özel varlık içinde aşağıdaki utterance etiketiyle **hedefleri** sayfası:

```text
Are there any SQL server jobs?
```

1. Seçin `SQL server` basit bir varlık olarak etiketlemek için utterance içinde. Görüntülenen varlık açılan kutusunda için var olan bir varlığa seçin veya yeni varlık ekleyin. Yeni bir varlık eklemek için adını yazın. `Job` metin kutusuna ve ardından **yeni varlık Oluştur**.

    ![Varlık adı girme ekran görüntüsü](./media/luis-how-to-add-example-utterances/create-simple-entity.png)

    > [!NOTE]
    > Etiket sözcükleri varlıklar olarak seçerken:
    > * Yalnızca tek bir sözcük, onu seçin. 
    > * İki veya daha fazla sözcük kümesi için ilk sözcük ve ardından son sözcüğü seçin.

1. İçinde **ne tür bir varlık oluşturmak istiyorsunuz?** açılır kutusunda, varlık adını doğrulayın ve seçin **basit** varlık türü ve ardından **Bitti**.

    A [tümcecik listesi](luis-concept-feature.md) genellikle bir varlığın sinyal artırmak için kullanılır.

## <a name="add-a-list-entity"></a>Bir liste varlık ekleme

Liste varlık sisteminizde ilgili sözcükleri tam metin eşleşmelerinin bir kümesini temsil eder. 

Bir şirketin bölüm listesi için değerleri normalleştirilmiş: `Accounting` ve `Human Resources`. Eş Anlamlılar her normalleştirilmiş bir adı vardır. Bir bölüm için tüm departman kısaltmalar, sayı veya argo bu eş anlamlıların içerebilir. Bir varlık oluşturduğunuzda tüm değerleri bilmeniz gerekmez. Daha fazla, eş anlamlı sözcüklerle gerçek kullanıcı konuşma gözden geçirdikten sonra ekleyebilirsiniz.

1. Bir örnek utterance içinde **hedefleri** sayfasında, bir sözcük veya tümcecik istediğiniz yeni listeden seçin. Varlık açılan görüntülendiğinde, ilk metin kutusuna yeni liste varlığı için bir ad girin ve ardından **yeni varlık Oluştur**.   

1. İçinde **ne tür bir varlık oluşturmak istiyorsunuz?** açılır kutu, varlık adı ve select **listesi** türü olarak. Bu liste öğesinin eş anlamlılar eklemek ve ardından **Bitti**. 

    ![Liste varlık eş anlamlılar girme ekran görüntüsü](./media/luis-how-to-add-example-utterances/hr-create-list-2.png)

    Diğer konuşma etiketleme veya varlıktan düzenleyerek daha fazla liste öğelerini veya daha fazla öğe eş anlamlılar ekleyebilirsiniz **varlıkları** sol gezinti bölmesinde. [Düzenleme](luis-how-to-add-entities.md#add-list-entities) varlıkları girme ek öğeleri karşılık gelen, eş anlamlılar veya listesini alma seçeneği sunar. 

## <a name="add-a-composite-entity"></a>Bileşik bir varlık ekleme

Bileşik varlıklar, varolan oluşturulur **varlıkları** üst varlık oluşturmak için. 

Utterance varsayılarak `Does John Smith work in Seattle?`, bileşik bir utterance çalışan adını varlık bilgileri döndürebilir `John Smith`ve konumunu `Seattle` bileşik bir varlık içinde. Alt varlıklar uygulamada zaten bulunmalı ve Bileşik varlık oluşturmadan önce örnek utterance işaretlenmelidir.

1. Alt varlıklar bileşik bir varlığa kaydırmak için işaretleyin **ilk** (en soldaki) içinde utterance Bileşik varlık için varlık etiketli. Bu seçim seçenekleri göstermek için aşağı açılan listesi görüntülenir.

1. Seçin **kaydırma bileşik varlıkta** aşağı açılan listeden. 

1. Son sözcüğü Bileşik varlık (en sağ) seçin. Yeşil bir çizgi Bileşik varlık izleyen dikkat edin. Bu birleşik bir varlık için görsel gösterge ve en sağ alt varlık bileşik varlığa en soldaki alt varlıktaki tüm sözcüklerin altında olmalıdır.

1. Aşağı açılan listeden Bileşik varlık adı girin.

    Varlıkları doğru Kaydır yeşil bir çizgi deyimin tamamını altında olur.

1. Bileşik varlık ayrıntıları üzerinde doğrulama **ne tür bir varlık oluşturmak istiyorsunuz?** açılır kutusunu seçip **Bitti**.

    ![Açılan ekran görüntüsü, varlık ayrıntıları](./media/luis-how-to-add-example-utterances/hr-create-composite-3.png)

1. Bileşik varlık hem mavi noktalar tek varlıklar için ve tüm Bileşik varlık için yeşil bir çizgi görüntüler. 

    ![Bileşik varlığı ile ekran görüntüsü, hedefleri Ayrıntıları sayfası](./media/luis-how-to-add-example-utterances/hr-create-composite-4.png)

## <a name="add-entitys-role-to-utterance"></a>Utterance için varlığın rolünü ekleyin

Adlandırılmış alt utterance bağlamında tarafından belirlenen bir varlığın rolüdür. Bir varlık içindeki bir utterance varlık olarak işaretleyin ya da söz konusu varlık içinde bir rol seçin. Herhangi bir varlık makine öğrenilen özel varlıklar da dahil olmak üzere rolleri (Basit varlıkları ve bileşik varlıklar) sahip olabilir, makine öğrenilen (önceden oluşturulmuş varlıklar, normal ifade varlıkları listesi varlıklar) değildir. 

Bilgi [varlık rolleriyle bir utterance işaretlemek nasıl](tutorial-entity-roles.md) uygulamalı Öğreticisi. 

## <a name="entity-status-predictions"></a>Varlık durumu Öngörüler

LUIS Portalı'nda yeni utterance girdiğinizde, utterance varlık tahmin hataları olabilir. LUIS nasıl varlık belirlediğini ile karşılaştırıldığında nasıl bir varlık etiketli arasında bir fark tahmin hata var. 

Bu fark, utterance kırmızı alt çizgi ile LUIS Portalı'nda görsel olarak temsil edilir. Kırmızı alt çizgi, varlık köşeli ayraçlar içindeki ya da köşeli ayraç dışında görünebilir. 

![Ekran görüntüsü, varlık durumu tahmin uyuşmazlık](./media/luis-how-to-add-example-utterances/entity-prediction-error.png)

Utterance kırmızıyla altı çizili olan sözcükleri'ni seçin. 

Varlık kutu görüntüler **varlık durumu** tahmin tutarsızlık ise kırmızı ünlem işareti. Varlık durumu ile etiketlenmiş ve tahmin edilen varlıklar arasındaki fark hakkında daha fazla bilgi görmek için seçin **varlık durumu** sonra sağ öğeyi seçin.

![Ekran görüntüsü, varlık durumu seçimi](./media/luis-how-to-add-example-utterances/entity-prediction-error-correction.png)

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

Intents sayfasında bir utterance makine öğrenilen varlık etiketleri kaldırabilirsiniz. Varlık makine öğrenilen değilse, bir utterance kaldırılamaz. Utterance bir makine öğrenilen varlık kaldırmanız gerekirse, tüm uygulamadan varlığı silmek gerekir. 

Bir utterance makine öğrenilen varlık etiketi kaldırmak için utterance varlığı seçin. Ardından **Etiketi Kaldır** varlık açılan kutusunda görünür.

### <a name="add-a-prebuilt-entity-label"></a>Önceden oluşturulmuş varlık etiketi Ekle

Önceden oluşturulmuş varlıklarla LUIS uygulamanıza eklediğinizde, bu varlıklarla etiketi konuşma gerek yoktur. Önceden oluşturulmuş varlıklar ve bunları nasıl ekleyeceğinizi hakkında daha fazla bilgi için bkz: [varlık Ekle](luis-how-to-add-entities.md#add-a-prebuilt-entity-to-your-app).

### <a name="add-a-regular-expression-entity-label"></a>Bir normal ifade varlık etiketi Ekle

LUIS uygulamanızı normal ifade varlıkları eklerseniz, bu varlıklarla etiketi konuşma gerekmez. Normal ifade varlıkları ve bunları nasıl ekleyeceğinizi hakkında daha fazla bilgi için bkz: [varlık Ekle](luis-how-to-add-entities.md#add-regular-expression-entities-for-highly-structured-concepts).


### <a name="create-a-pattern-from-an-utterance"></a>Bir utterance bir düzen oluşturma

Bkz: [hedefi veya varlık sayfasında mevcut utterance Ekle deseni](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).


### <a name="add-a-patternany-entity"></a>Pattern.any varlık ekleme

LUIS uygulamanızı pattern.any varlıkları eklerseniz, bu varlıklarla konuşma etiketi olamaz. Bunlar yalnızca desenleri geçerli olur. Pattern.any varlıkları ve bunları nasıl ekleyeceğinizi hakkında daha fazla bilgi için bkz: [varlık Ekle](luis-how-to-add-entities.md#add-patternany-entities-to-capture-free-form-entities).

## <a name="train-your-app-after-changing-model-with-utterances"></a>Konuşma modeliyle değiştirdikten sonra uygulamanızı eğitin

Eklemek, düzenlemek ve kaldırmak, konuşma sonra [eğitme](luis-how-to-train.md) ve [yayımlama](luis-how-to-publish-app.md) uygulamanız için uç nokta sorguları etkilemek yaptığınız değişiklikleri. 

## <a name="next-steps"></a>Sonraki adımlar

Sonra konuşma etiketleme, **hedefleri**, artık oluşturabilirsiniz bir [Bileşik varlık](luis-how-to-add-entities.md).
