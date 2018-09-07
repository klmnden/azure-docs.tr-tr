---
title: LUIS uygulamalarında örnek Konuşma ekleme
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUIS) uygulamalarında konuşma eklemeyi öğrenin.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: eb859dfd2dca37415d074e8a0398be37fcc0a6b8
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44051776"
---
# <a name="add-example-utterances-and-label-with-entities"></a>Örnek konuşma ve varlık etiketi Ekle

Örnek konuşma metin kullanıcı sorularınız ya da komutları örnekleridir. Language Understanding (LUIS) öğretmeyi eklemeniz gerekir [örnek konuşma](luis-concept-utterance.md) için bir [hedefi](luis-concept-intent.md).

Genellikle, bir amaç için önce bir örnek utterance ekleyin ve varlıkları ve etiket konuşma niyetini sayfada oluşturup. Varlıkları ilk yerine oluşturacak olup [varlık Ekle](luis-how-to-add-entities.md).

## <a name="add-an-utterance"></a>Bir utterance Ekle
Beklediğiniz kullanıcılarınızdan gelen gibi bir ilgili örnek utterance hedefi bir sayfaya girin `book 2 adult business tickets to Paris tomorrow on Air France` metin kutusuna aşağıdaki hedefi adını ve Enter tuşuna basın. 
 
>[!NOTE]
>LUIS tüm konuşma küçük harfe dönüştürür.

![Vurgulanan utterance ile ekran görüntüsü, hedefleri Ayrıntıları sayfası](./media/luis-how-to-add-example-utterances/add-new-utterance-to-intent.png) 

Konuşma geçerli amaç için konuşma listesine eklenir. 

## <a name="ignoring-words-and-punctuation"></a>Sözcükleri ve noktalama işaretleri yoksayılıyor
Belirli sözcükleri ya da örnek utterance, noktalama işareti yok saymak istiyorsanız, kullanan bir [deseni](luis-concept-patterns.md#pattern-syntax) ile _Yoksay_ söz dizimi. 

## <a name="add-simple-entity-label"></a>Basit varlık etiketi Ekle
Aşağıdaki yordamda, oluşturun ve aşağıdaki utterance hedefi sayfasında özel varlıklarda etiketi:

```
book me 2 adult business tickets to Paris tomorrow on Air France
```

1. Basit bir varlık olarak etiketlemek için utterance "Hava Fransa" seçin.

    > [!NOTE]
    > Bunları varlıklar olarak etiketlemek için sözcükleri seçerken:
    > * Yalnızca tek bir sözcük, onu seçin. 
    > * İki veya daha fazla sözcük kümesi için başında ve sonunda kümesi, ardından seçin.

2. Görüntülenen varlık açılan kutusunda için var olan bir varlığa seçin veya yeni varlık ekleyin. Yeni bir varlık eklemek için metin kutusuna adını yazın ve ardından **yeni varlık Oluştur**. 
 
    ![Ekran görüntüsü, hedefleri Ayrıntıları sayfası, varlığın seçeneğinin vurgulandığı etiketleme](./media/luis-how-to-add-example-utterances/create-airline-simple-entity.png)

3. İçinde **ne tür bir varlık oluşturmak istiyorsunuz?** açılır iletişim kutusunda varlık adını doğrulayın ve basit bir varlık türü seçin ve ardından **Bitti**.

    ![Onay iletişim kutusunun görüntüsü](./media/luis-how-to-add-example-utterances/create-simple-airline-entity.png)

    Bkz: [veri ayıklama](luis-concept-data-extraction.md#simple-entity-data) JSON sorgu yanıtı uç noktasından Basit varlık ayıklama hakkında daha fazla bilgi edinmek için. Varlığın deneyin [hızlı](luis-quickstart-primary-and-secondary-data.md) tek bir varlığın kullanma hakkında daha fazla bilgi için.


## <a name="add-list-entity-and-label"></a>Liste varlığı ve etiket ekleme
Liste varlık (tam metni eşleşen) sabit, kapalı küme ilgili bir kelimelerin sisteminizde temsil eder. 

İçecekler listesi varlık için iki normalleştirilmiş değerlere sahip olabilir: su ve soda açılır. Eş Anlamlılar her normalleştirilmiş bir adı vardır. Su için H20, doğalgaz, düz eşanlamlıdır. Soda pop için eş anlamlı sözcükler Meyve, cola ginger olan. Bir varlık oluşturduğunuzda tüm değerleri bilmeniz gerekmez. Daha fazla, eş anlamlı sözcüklerle gerçek kullanıcı konuşma gözden geçirdikten sonra ekleyebilirsiniz.

|Normalleştirilmiş adı|Eş anlamlılar|
|--|--|
|Su|H20 gaz, düz|
|Soda pop|Meyve, cola ginger|

Yeni bir liste varlığı hedefi sayfasından oluştururken, belirgin olmayabilir iki şeyler yapıyor. İlk olarak, ilk liste öğesinin ekleyerek yeni bir liste oluşturuyorsunuz. İkinci olarak, ilk liste öğesinin bir sözcük veya tümcecik utterance seçtiğiniz ile adlandırılır. Varlık sayfasından daha sonra değiştirebilirsiniz, ancak liste öğesinin adını istediğiniz kelimesi olan bir utterance seçmek için daha hızlı olabilir.

Örneğin, içecek ve türleri listesini oluşturmak istiyorsanız sözcük seçili `h2o` utterance varlığı oluşturmak için h20 adı olan bir öğe listesine sahip. Daha genel bir ad istediyseniz, daha genel adı kullanan bir utterance seçmeniz gerekir. 

1. Utterance içinde listesindeki ilk öğedir word seçin, sonra listenin adı metin kutusuna girin ve sonra seçin **yeni varlık Oluştur**.   

    ![Ekran görüntüsü, hedefleri Ayrıntıları sayfası, vurgulanan yeni varlık oluştur](./media/luis-how-to-add-example-utterances/create-drink-list-entity.png)

2. İçinde **ne tür bir varlık oluşturmak istiyorsunuz?** iletişim kutusunda, bu liste öğesinin eş anlamlı sözcükler ekleyin. Su öğe için bir içecek liste Ekle `h20`, `perrier`, ve `waters`seçip **Bitti**. Liste eş anlamlılar belirteç düzeyinde sağlandığından, "sularında" eklenen dikkat edin. Listede değilse İngilizce kültürüne bu nedenle word düzeyinde düzeyidir "sularında" "su" için karşılaştırılamıyor. 

    ![İletişim kutusu oluşturmak istiyorsanız ne varlık türünü ekran görüntüsü](./media/luis-how-to-add-example-utterances/drink-list-ddl.png)

    İçecekler oluşan bu liste, su yalnızca bir içecek türü vardır. Daha fazla içecek türü diğer konuşma etiketleme veya varlıktan düzenleme ekleyebilirsiniz **varlıkları** sol gezinti bölmesinde. [Düzenleme](luis-how-to-add-entities.md#add-list-entities) varlıkları size ek öğeleriyle ilgili eş anlamlılar girme seçenekleri veya [alma](luis-how-to-add-entities.md#import-list-entity-values) bir liste. 

    Bkz: [veri ayıklama](luis-concept-data-extraction.md#list-entity-data) JSON sorgu yanıtı uç noktasından listesi varlık ayıklama hakkında daha fazla bilgi edinmek için. Deneyin [hızlı](luis-quickstart-intent-and-list-entity.md) liste varlığı kullanma hakkında daha fazla bilgi için.

## <a name="add-synonyms-to-the-list-entity"></a>Liste varlığı için eş anlamlı sözcükler ekleme 
Eşanlamlısı utterance içinde bir sözcük veya tümcecik seçerek için liste varlığı ekleyin. Varlık listesinde ve eklemek istediğiniz bir içecek varsa `agua` su eşanlamlısı, adımları izleyin:

Utterance eş anlamlı sözcük gibi seçin `aqua` su için liste varlık adı aşağı açılan listesinde, aşağıdaki gibi seçip **bol**, ardından **eş anlamlı ayarlamak**, listeyi seçin olduğu gibi ile eşanlamlıdır öğesi **su**.

![İle vurgulanmış yeni bir eş anlamlı Oluştur ekran görüntüsü, hedefleri Ayrıntıları sayfası](./media/luis-how-to-add-example-utterances/set-agua-as-synonym.png)

## <a name="create-new-item-for-list-entity"></a>Liste varlığı için yeni öğe oluşturun
Bir sözcük veya tümcecik içinde utterance seçerek var olan bir liste varlığı için yeni bir öğe oluşturun. Liste ve eklemek istediğiniz bir içecek varsa `tea` yeni bir öğe adımları izleyin:

Utterance içinde yeni bir liste öğesi için word gibi seçin `tea`, liste varlık adı aşağı açılan listesinde, aşağıdaki gibi seçin **bol**, ardından **yeni bir eş anlamlı oluşturma**. 

![Yeni liste öğesi ekleme işleminin ekran görüntüsü](./media/luis-how-to-add-example-utterances/list-entity-create-new-item.png)

Word artık mavi renkle vurgulanır. Word'ün gelin, liste öğesi adı Çay gibi gösteren bir etiket görüntüler.

![Yeni liste öğesi etiketi ekran görüntüsü](./media/luis-how-to-add-example-utterances/list-entity-item-name-tag.png)

## <a name="wrap-entities-in-composite-label"></a>Bileşik etiketinde varlıkları Kaydır
Bileşik varlıklar öğesinden oluşturulur **varlıkları**. Intent sayfasından bileşik bir varlık oluşturulamıyor. Bileşik varlık oluşturulduktan sonra bir utterance hedefi sayfasında, varlık sarabilirsiniz. 

Utterance varsayılarak `book 2 tickets from Seattle to Cairo`, tek üst varlıkta bir bileşik utterance varlık bilgilerini biletleri (2), (Seattle) kaynak ve hedef sayısı (Cairo) konumları döndürebilir. 

Aşağıdaki adımları [adımları](luis-how-to-add-entities.md#add-prebuilt-entity) eklemek için **numarası** önceden oluşturulmuş varlık. Varlık oluşturulduktan sonra `2` utterance etiketli bir varlık olduğunu gösteren, mavi olan. Önceden oluşturulmuş varlıklarla LUIS ile etiketlenir. Ekleyemez veya önceden oluşturulmuş varlık etiketi tek bir utterance kaldırın. Yalnızca ekleyebilir veya önceden oluşturulmuş tüm etiketleri ekleyerek veya uygulamayı önceden oluşturulmuş varlık kaldırarak kaldırın.

Aşağıdaki adımları [adımları](#add-hierarchical-entity-and-label) oluşturmak için bir **konumu** hiyerarşik varlık. Örnek utterance kaynak ve hedef konumların etiketleyin. 

Bileşik bir varlıkta varlıkları sarmalamadan önce tüm alt varlıklar vurgulanır bunlar utterance etiketlenmiş anlamına gelir, mavi emin olun.

1. Tek tek varlıklarla bileşik sarmak için bileşik bir varlık için utterance ilk etiketli varlık seçin. Örnek utterance içinde `book 2 tickets from Seattle to Cairo`, 2 sayısı ilk varlıktır. Bu seçim seçenekleri gösteren bir açılan listesi görüntülenir.

    ![Seçilen numarası ve açılan seçenekleri vurgulanmış ekran görüntüsü](./media/luis-how-to-add-example-utterances/wrap-1.png)

2. Seçin **kaydırma Bileşik varlık** aşağı açılan listeden. 

    ![Bileşik varlık kaydırma ile vurgulanmış bileşik varlıkta sarmalama için aşağı açılan seçeneklerinin ekran görüntüsü](./media/luis-how-to-add-example-utterances/wrap-2.png)

3. Son sözcüğü Bileşik varlık seçin. Bu örnekte utterance içinde "(Cairo temsil eden) Location::Destination" seçin. Yeşil çizginin artık tüm sözcükleri altında varlık olmayan sözcükler bileşik olan utterance kullanmaktadır.

    ![Vurgulanan numarasıyla BookFlight hedefi ekran sayfası](./media/luis-how-to-add-example-utterances/wrap-composite.png)

4. Bileşik varlık adı, aşağı açılan listeden seçin. Bu örnekte, olan **TicketOrder**.

    ![Aşağı açılan listede vurgulanmış Bileşik varlık adı ile bileşik varlıkla sözcük kaydırma işleminin ekran görüntüsü](./media/luis-how-to-add-example-utterances/wrap-4.png)

    Varlıkları doğru Kaydır yeşil bir çizgi deyimin tamamını altında olur.

    ![Utterance bileşik varlıkla vurgulanmış ekran görüntüsü](./media/luis-how-to-add-example-utterances/wrap-5.png)

    Bkz: [veri ayıklama](luis-concept-data-extraction.md#composite-entity-data) JSON sorgu yanıtı uç noktasından Bileşik varlık ayıklama hakkında daha fazla bilgi edinmek için. Bileşik varlık deneyin [öğretici](luis-tutorial-composite-entity.md) bileşik bir varlık kullanma hakkında daha fazla bilgi için.

## <a name="add-hierarchical-entity-and-label"></a>Hiyerarşik varlık ve etiket ekleme
Hiyerarşik bir varlık, bağlamsal öğrenilen ve kavramsal olarak ilişkili varlıkları kategorisidir. Aşağıdaki örnekte, kaynak ve hedef konumları varlık içerir. 

Utterance içinde `Book 2 tickets from Seattle to Cairo`, Seattle kaynak konumu ve Cairo hedef konumu. Her kelime sırasını ve sözcük seçenek utterance bağlamsal olarak farklı ve öğrenilen konumdur.

1. Hedefi sayfasında utterance "Seattle" seçin ve ardından varlık adını girin ' konum tıklayın ve ardından **yeni varlık Oluştur**.

    ![İletişim kutusunun ekran görüntüsü, oluşturma hiyerarşik varlık etiketleme](./media/luis-how-to-add-example-utterances/create-hier-ent-1.png)

2. Açılan iletişim kutusunda için hiyerarşik seçin **varlık türü**, eklersiniz `Origin` ve `Destination` alt tıklayın ve ardından olarak **Bitti**.

    ![Vurgulanan ToLocation varlıkla ekran görüntüsü, hedefleri Ayrıntıları sayfası](./media/luis-how-to-add-example-utterances/create-location-hierarchical-entity.png)

3. Utterance sözcüğü üst hiyerarşik varlıkla etiketlendi. Word'ün bir alt varlığına atamanız gerekir. Utterance için hedefi sayfaya dönün. Word ' ü seçin sonra oluşturduğunuz varlık adı aşağı açılan listeden seçin ve sağa doğru alt varlığı seçmeniz için menü izleyin.

    ![Vurgulanan ToLocation varlıkla ekran görüntüsü, hedefleri Ayrıntıları sayfası](./media/luis-how-to-add-example-utterances/label-tolocation.png)

    >[!CAUTION]
    >Tek bir uygulamada tüm varlıklar üzerinde alt varlık adlarının benzersiz olması gerekir. Alt varlıklar aynı ada sahip iki farklı hiyerarşik varlıklar içerebilir. 

    Bkz: [veri ayıklama](luis-concept-data-extraction.md#hierarchical-entity-data) JSON sorgu yanıtı uç noktasından hiyerarşik varlık ayıklama hakkında daha fazla bilgi edinmek için. Hiyerarşik varlık deneyin [hızlı](luis-quickstart-intent-and-hier-entity.md) hiyerarşik bir varlık kullanma hakkında daha fazla bilgi için.


## <a name="remove-entity-labels-from-utterances"></a>Varlık etiketleri konuşma kaldırın
Hedefi sayfasında bir utterance makine öğrenilen varlık etiketleri kaldırabilirsiniz. Varlık makine öğrenilen değilse, bir utterance kaldırılamaz. Utterance bir makine öğrenilen varlık kaldırmanız gerekirse, tüm uygulamadan varlığı silmek gerekir. 

Bir utterance makine öğrenilen varlık etiketi kaldırmak için utterance varlığı seçin. Ardından **Etiketi Kaldır** varlık açılan kutusunda görünür.

![Ekran görüntüsü, hedefleri Ayrıntıları sayfası, vurgulanan Etiketi Kaldır](./media/luis-how-to-add-example-utterances/remove-label.png) 

## <a name="add-prebuilt-entity-label"></a>Önceden oluşturulmuş varlık etiketi Ekle
LUIS uygulamanızı önceden oluşturulmuş varlıklarla eklerseniz, bu varlıklarla etiket konuşma gerekmez. Önceden oluşturulmuş varlıklar ve bunları nasıl ekleyeceğinizi hakkında daha fazla bilgi için bkz: [varlık Ekle](luis-how-to-add-entities.md#add-prebuilt-entity).

## <a name="add-regular-expression-entity-label"></a>Normal ifade varlık etiketi Ekle
LUIS uygulamanızı normal ifade varlıkları eklerseniz, bu varlıklarla etiket konuşma gerekmez. Normal ifade varlıkları ve bunları nasıl ekleyeceğinizi hakkında daha fazla bilgi için bkz: [varlık Ekle](luis-how-to-add-entities.md#add-regular-expression-entities).

## <a name="create-a-pattern-from-an-utterance"></a>Bir utterance bir düzen oluşturma
Bkz: [hedefi veya varlık sayfasında mevcut utterance Ekle deseni](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="add-patternany-entity-label"></a>Pattern.Any varlık etiketi Ekle
LUIS uygulamanızı pattern.any varlıkları eklerseniz, bu varlıklarla konuşma etiketi olamaz. Bunlar yalnızca desenleri geçerli olur. Pattern.any varlıkları ve bunları nasıl ekleyeceğinizi hakkında daha fazla bilgi için bkz: [varlık Ekle](luis-how-to-add-entities.md#add-patternany-entities).

<!--
Fix this - moved to luis-how-to-add-intents.md - how ?

## Search in utterances
## Prediction discrepancy errors
## Filter by intent prediction discrepancy errors
## Filter by entity type
## Switch to token view
## Delete utterances
## Edit an utterance
## Reassign utterances

-->
## <a name="train-your-app-after-changing-model-with-utterances"></a>Konuşma modeliyle değiştirdikten sonra uygulamanızı eğitin
Eklemek, düzenlemek ve kaldırmak, konuşma sonra [eğitme](luis-how-to-train.md) ve [yayımlama](luis-how-to-publish-app.md) uygulamanız için uç nokta sorguları etkilemek yaptığınız değişiklikleri. 

## <a name="next-steps"></a>Sonraki adımlar

Konuşma, hedefleri olarak etiketleme sonra artık oluşturabileceğiniz bir [Bileşik varlık](luis-how-to-add-entities.md).
