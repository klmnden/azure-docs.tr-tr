---
title: Örnek utterances HALUK uygulamaları ekleme | Microsoft Docs
titleSuffix: Azure
description: Dil anlama (HALUK) uygulamalarında utterances eklemeyi öğrenin.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 74a4b77bd9823e5462eecd438cf4c1d863e79892
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36300647"
---
# <a name="add-example-utterances-and-label-with-entities"></a>Örnek utterances ve varlıklarla etiket ekleme

Örnek utterances metin kullanıcı sorular veya komutları gösterilebilir. Dil anlama (HALUK) öğretmeyi eklemeniz gerekir [örnek utterances](luis-concept-utterance.md) için bir [hedefi](luis-concept-intent.md).

Genellikle, bir örnek utterance amacına için ilk olarak ekleyin ve ardından hedefi sayfasında varlıkları ve etiket utterances oluşturun. Varlıkları ilk yerine oluşturacak olup [varlıkları ekleyin](luis-how-to-add-entities.md).

## <a name="add-an-utterance"></a>Bir utterance ekleyin
Bir hedefi sayfasında beklediğiniz, kullanıcılardan gibi ilgili örnek utterance girin `book 2 adult business tickets to Paris tomorrow on Air France` hedefi adı ve ENTER tuşuna basın altındaki metin kutusuna. 
 
>[!NOTE]
>HALUK tüm utterances küçük harflere dönüştürür.

![Vurgulanan utterance ile ekran görüntüsü, hedefleri Ayrıntılar sayfası](./media/luis-how-to-add-example-utterances/add-new-utterance-to-intent.png) 

Utterances geçerli amacı için utterances listesine eklenir. 

## <a name="ignoring-words-and-punctuation"></a>Sözcük ve noktalama yoksayılıyor
Belirli bir sözcük veya örnek utterance noktalama yoksaymak istiyorsanız, kullanmak bir [düzeni](luis-concept-patterns.md#pattern-syntax) ile _Yoksay_ sözdizimi. 

## <a name="add-simple-entity-label"></a>Basit varlık etiketi ekleyin
Aşağıdaki yordamda oluşturun ve aşağıdaki utterance hedefi sayfasında özel varlıkları etiketi:

```
book me 2 adult business tickets to Paris tomorrow on Air France
```

1. "Hava Fransa" basit bir varlık olarak etiketlemek için utterance seçin.

    > [!NOTE]
    > Varlık etiketi kelimeleri seçerken:
    > * Yalnızca tek bir sözcük için bunu seçin. 
    > * İki veya daha fazla sözcük kümesi için başında, ardından kümesinin sonunda seçin.

2. Görüntülenen varlık açılan kutusunda için var olan bir varlık seçin veya yeni bir varlık ekleyin. Yeni bir varlık eklemek için metin kutusuna adını yazın ve ardından **yeni varlık oluşturma**. 
 
    ![Ekran görüntüsü, hedefleri Ayrıntıları sayfası, Basit varlık seçeneği vurgulanmış etiketleme](./media/luis-how-to-add-example-utterances/create-airline-simple-entity.png)

3. İçinde **varlık türünü oluşturmak istiyor musunuz?** açılan iletişim kutusunda, varlık adı doğrulayın ve Basit varlık türü seçin ve ardından **Bitti**.

    ![Onay iletişim kutusunun görüntüsü](./media/luis-how-to-add-example-utterances/create-simple-airline-entity.png)

    Bkz: [veri ayıklama](luis-concept-data-extraction.md#simple-entity-data) Basit varlık JSON sorgu yanıtı uç noktasından ayıklanıyor hakkında daha fazla bilgi edinmek için. Basit varlık deneyin [Hızlı Başlangıç](luis-quickstart-primary-and-secondary-data.md) basit bir varlık kullanma hakkında daha fazla bilgi için.


## <a name="add-list-entity-and-label"></a>Liste varlık ve etiket ekleme
Liste varlık sisteminizde (tam metinle eşleşen) bir sabit, kapalı ilgili sözcükler kümesini temsil eder. 

İçecekler listesi varlığı için iki normalleştirilmiş değerlere sahip olabilir: su ve soda pop. Eş anlamlıları her normalleştirilmiş bir adı vardır. Su için eş anlamlı sözcükleri H20, gaz, düz var. Soda pop için eş anlamlı sözcükleri ulaşılacak durumlardır, cola, ginger şunlardır. Varlık oluşturduğunuzda tüm değerleri bilmek zorunda değilsiniz. Eş anlamlıları ile gerçek kullanıcı utterances gözden geçirdikten sonra daha ekleyebilirsiniz.

|Normalleştirilmemiş bir ad|Eş anlamlılar|
|--|--|
|Su|H20, gaz, düz|
|Soda pop|Ulaşılacak durumlardır, cola, ginger|

Yeni bir liste varlık hedefi sayfasından oluştururken, belirgin olmayabilir iki şey yaparsınız. İlk olarak, ilk liste öğesi ekleyerek yeni bir liste oluşturuyorsunuz. İkinci olarak, ilk liste öğesi, sözcük veya tümcecik utterance seçili ile adlandırılır. Bu varlık sayfasından daha sonra değiştirebilirsiniz, ancak liste öğesinin adı için istediğiniz sözcüğü bulunan bir utterance seçmek için daha hızlı olabilir.

Örneğin, listesini oluşturmak istemeniz durumunda türleri içki ve, word seçili `h2o` varlık oluşturmak için utterance adı olduğu h20, tek bir öğe listesi olurdu. Daha genel bir ad istediyseniz, daha genel adı kullanan bir utterance seçmeniz gerekir. 

1. Utterance içinde listesindeki ilk öğedir word seçin, ardından listesinin adı metin kutusuna girin ve sonra seçin **yeni varlık oluşturma**.   

    ![Ekran görüntüsü, hedefleri Ayrıntıları sayfası, vurgulanmış yeni varlık oluştur](./media/luis-how-to-add-example-utterances/create-drink-list-entity.png)

2. İçinde **varlık türünü oluşturmak istiyor musunuz?** iletişim kutusunda, bu liste öğesinin anlamlıları ekleyin. İçki listesinde su öğe için ekleme `h20`, `perrier`, ve `waters`seçip **Bitti**. Liste eş anlamlıları belirteci düzeyinde uymadığı için "sularında" eklenir dikkat edin. Listede değilse İngilizce kültürün bu nedenle word düzeyinde düzeyidir "sularında" "su" Not matched. 

    ![İletişim kutusu oluşturmak istediğiniz ne varlık türünü işleminin ekran görüntüsü](./media/luis-how-to-add-example-utterances/drink-list-ddl.png)

    Yalnızca bir içki türünün, su İçecekler listesi vardır. Daha fazla içki türleri diğer utterances etiketleme veya varlıktan düzenleyerek ekleyebilirsiniz **varlıklar** sol gezinti bölmesinde. [Düzenleme](luis-how-to-add-entities.md#add-list-entities) varlıkları ek öğelere karşılık gelen eş anlamlıları ile girme seçenekleri sağlar ya da [alma](luis-how-to-add-entities.md#import-list-entity-values) bir listesi. 

    Bkz: [veri ayıklama](luis-concept-data-extraction.md#list-entity-data) listesi varlıklar JSON sorgu yanıtı uç noktasından ayıklanıyor hakkında daha fazla bilgi edinmek için. Deneyin [Hızlı Başlangıç](luis-quickstart-intent-and-list-entity.md) listesi varlığı kullanma hakkında daha fazla bilgi için.

## <a name="add-synonyms-to-the-list-entity"></a>Eş anlamlıları listesi varlık ekleme 
Bir eş anlamlı sözcük veya tümcecik utterance seçerek listesi varlık ekleyin. Listesinde varlık ve eklemek istediğiniz bir içki varsa `agua` eş su için adımları izleyin:

Utterance içinde eşanlamlı word gibi seçin `aqua` su için liste varlık adı açılan listede gibi seçip **bol**seçeneğini belirleyip **eş anlamlı ayarlamak**, sonra listeden seçin Bu gibi eşanlamlı öğesi **su**.

![Ekran görüntüsü, hedefleri Ayrıntıları sayfası, Create vurgulanmış yeni bir eş anlamlı](./media/luis-how-to-add-example-utterances/set-agua-as-synonym.png)

## <a name="create-new-item-for-list-entity"></a>Liste varlık için yeni öğe oluşturun
Sözcük veya tümcecik utterance seçerek var olan bir liste varlığı için yeni bir öğe oluşturun. Liste ve eklemek istediğiniz bir içki varsa `tea` yeni bir öğe adımları izleyin:

Utterance içinde yeni liste öğesi için word gibi seçin `tea`, liste varlık adı açılan listede gibi seçin **bol**seçeneğini belirleyip **yeni eşanlamlısı oluşturma**. 

![Yeni liste öğesi ekleme işleminin ekran görüntüsü](./media/luis-how-to-add-example-utterances/list-entity-create-new-item.png)

Word şimdi mavi renkte vurgulanır. Word getirirseniz, liste öğesi adı Çay gibi gösteren bir etiket görüntüler.

![Yeni liste öğesi etiketinin ekran görüntüsü](./media/luis-how-to-add-example-utterances/list-entity-item-name-tag.png)

## <a name="wrap-entities-in-composite-label"></a>Bileşik etiketinde varlıklar sarmalama
Bileşik varlıklar oluşturulur **varlıklar**. Hedefi sayfasından Bileşik varlık oluşturulamıyor. Bileşik varlık oluşturulduktan sonra bir utterance hedefi sayfasında içindeki varlıkların kayabilir. 

Utterance varsayılarak `book 2 tickets from Seattle to Cairo`, bir tek üst varlık bileşik utterance varlık bilgileri biletleri (2), kaynak (Seattle) ve hedef sayısı (Kahire) konumları döndürebilirsiniz. 

Aşağıdaki adımları [adımları](luis-how-to-add-entities.md#add-prebuilt-entity) eklemek için **numarası** önceden oluşturulmuş varlık. Varlık oluşturulduktan sonra `2` utterance etiketli bir varlık olduğunu gösteren mavi olduğunda. Önceden oluşturulmuş varlıklar HALUK göre etiketlenir. Ekleyemez veya önceden oluşturulmuş varlık etiketi tek utterance kaldırın. Yalnızca eklemek veya önceden oluşturulmuş tüm etiketleri ekleyerek veya önceden oluşturulmuş varlık uygulamadan kaldırarak kaldırın.

Aşağıdaki adımları [adımları](#add-hierarchical-entity-and-label) oluşturmak için bir **konumu** hiyerarşik varlık. Örnek utterance kaynak ve hedef konumların etiketleyin. 

Bileşik bir varlık varlıkları sarmalamadan önce tüm alt varlıkları vurgulanmıştır mavi, bunlar utterance etiketli anlamına emin olun.

1. Tek tek varlıklar bileşik sarmalamak için Bileşik varlık utterance ilk etiketli varlığı seçin. Örnek utterance içinde `book 2 tickets from Seattle to Cairo`, 2 sayısı ilk varlıktır. Bu seçim için seçenekleri gösteren açılan listesi görüntülenir.

    ![Seçilen numarası ve açılan seçenekleri vurgulanan ekran görüntüsü](./media/luis-how-to-add-example-utterances/wrap-1.png)

2. Seçin **sarmalamak Bileşik varlık** aşağı açılan listeden. 

    ![Bileşik varlık sarma Bileşik varlık vurgulanmış kaydırma için aşağı açılan seçeneklerinin ekran görüntüsü](./media/luis-how-to-add-example-utterances/wrap-2.png)

3. Bileşik varlık son sözcük seçin. Bu örnek utterance içinde "Location::Destination (Kahire temsil eder)" seçin. Yeşil bir çizgi şimdi tüm sözcükleri altında varlık olmayan sözcükler bileşik olan utterance dahil olmak üzere olduğunu.

    ![Vurgulanan numarasıyla BookFlight hedefi ekran sayfası](./media/luis-how-to-add-example-utterances/wrap-composite.png)

4. Bileşik varlık adı açılan listeden seçin. Bu örnekte, **TicketOrder**.

    ![Aşağı açılan listesinde vurgulanan Bileşik varlık adı ile Bileşik varlık, sözcük kaydırma işleminin ekran görüntüsü](./media/luis-how-to-add-example-utterances/wrap-4.png)

    Varlıkları doğru kaydırma yeşil bir çizgi deyimin tamamını altında olur.

    ![Vurgulanan bileşik varlığı ile utterance ekran görüntüsü](./media/luis-how-to-add-example-utterances/wrap-5.png)

    Bkz: [veri ayıklama](luis-concept-data-extraction.md#composite-entity-data) Bileşik varlık JSON sorgu yanıtı uç noktasından ayıklanıyor hakkında daha fazla bilgi edinmek için. Bileşik varlık deneyin [öğretici](luis-tutorial-composite-entity.md) Bileşik varlık kullanma hakkında daha fazla bilgi için.

## <a name="add-hierarchical-entity-and-label"></a>Hiyerarşik varlık ve etiket ekleme
Bir kategori bağlam öğrenmeli ve kavramsal olarak ilgili varlıkların buna hiyerarşik bir varlıktır. Aşağıdaki örnekte, kaynak ve hedef konumları varlık içerir. 

Utterance içinde `Book 2 tickets from Seattle to Cairo`, Seattle kaynak konumu ve Kahire hedef konumu. Her sözcük sırasını ve utterance word seçenek bağlam farklı ve öğrenilen konumdur.

1. Hedefi sayfasında utterance "Seattle" seçin ve sonra varlık adı girin ' konumu ve ardından **yeni varlık oluşturma**.

    ![İletişim kutusunun ekran görüntüsü, oluşturma hiyerarşik varlık etiketleme](./media/luis-how-to-add-example-utterances/create-hier-ent-1.png)

2. Açılan iletişim kutusunda için hiyerarşik seçin **varlık türü**, ardından ekleyin `Origin` ve `Destination` alt ve ardından olarak **Bitti**.

    ![Vurgulanan ToLocation varlığıyla ekran görüntüsü, hedefleri Ayrıntılar sayfası](./media/luis-how-to-add-example-utterances/create-location-hierarchical-entity.png)

3. Utterance Word'de ile üst hiyerarşik varlık etiketli. Bir alt varlık word atamanız gerekir. Utterance hedefi sayfaya dönün. Word ' ü seçin sonra oluşturduğunuz varlık adı açılan listeden seçin ve menü doğru alt varlık seçmeniz için sağa izleyin.

    ![Vurgulanan ToLocation varlığıyla ekran görüntüsü, hedefleri Ayrıntılar sayfası](./media/luis-how-to-add-example-utterances/label-tolocation.png)

    >[!CAUTION]
    >Tek bir uygulamada tüm varlıklar arasında alt varlık adlarının benzersiz olması gerekir. Alt varlıkları aynı ada sahip iki farklı hiyerarşik varlık içerebilir. 

    Bkz: [veri ayıklama](luis-concept-data-extraction.md#hierarchical-entity-data) hiyerarşik varlık JSON sorgu yanıtı uç noktasından ayıklanıyor hakkında daha fazla bilgi edinmek için. Hiyerarşik varlık deneyin [Hızlı Başlangıç](luis-quickstart-intent-and-hier-entity.md) hiyerarşik bir varlık kullanma hakkında daha fazla bilgi için.


## <a name="remove-entity-labels-from-utterances"></a>Varlık etiketleri utterances Kaldır
Bir utterance hedefi sayfasında makine öğrenilen varlık etiketleri kaldırabilirsiniz. Varlık makine öğrenilen değilse, bir utterance kaldırılamaz. Bir makine öğrenilen varlık utterance kaldırmanız gerekirse, tüm uygulama varlığı silmeniz gerekir. 

Bir utterance bir makine öğrenilen varlık etiketi kaldırmak için utterance varlığı seçin. Ardından **Etiketi Kaldır** varlık açılan kutusunda görüntülenir.

![Vurgulanan etiket kaldırma ile ekran görüntüsü, hedefleri Ayrıntılar sayfası](./media/luis-how-to-add-example-utterances/remove-label.png) 

## <a name="add-prebuilt-entity-label"></a>Önceden oluşturulmuş varlık etiketi ekleyin
Önceden oluşturulmuş varlıklar HALUK uygulamanıza eklerseniz, bu varlıklarla etiket utterances gerek yoktur. Önceden oluşturulmuş varlıkları ve bunları ekleme hakkında daha fazla bilgi için bkz: [varlıkları ekleyin](luis-how-to-add-entities.md#add-prebuilt-entity).

## <a name="add-regular-expression-entity-label"></a>Normal ifade varlık etiketi ekleyin
Normal ifade varlıkları HALUK uygulamanıza eklerseniz, bu varlıklarla etiket utterances gerek yoktur. Normal ifade varlıkları ve bunları ekleme hakkında daha fazla bilgi için bkz: [varlıkları ekleyin](luis-how-to-add-entities.md#add-regular-expression-entities).

## <a name="create-a-pattern-from-an-utterance"></a>Bir utterance bir düzeni oluşturma
Bkz: [Ekle düzeni hedefi veya varlık sayfasında mevcut utterance gelen](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="add-patternany-entity-label"></a>Pattern.Any varlık etiketi ekleyin
HALUK uygulamanıza pattern.any varlıklar eklerseniz, bu varlıklarla utterances etiketi olamaz. Bunlar yalnızca düzenleri geçerli olur. Pattern.any varlıkları ve bunları ekleme hakkında daha fazla bilgi için bkz: [varlıkları ekleyin](luis-how-to-add-entities.md#add-patternany-entities).

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
## <a name="train-your-app-after-changing-model-with-utterances"></a>Uygulamanızı utterances modeliyle değiştirdikten sonra eğitme
Eklemek, düzenlemek veya utterances, kaldırdıktan sonra [eğitmek](luis-how-to-train.md) ve [yayımlama](PublishApp.md) uygulamanız için uç nokta sorguları etkilemek yaptığınız değişiklikleri. 

## <a name="next-steps"></a>Sonraki adımlar

Şimdi oluşturmak, hedefleri utterances etiketleme sonra bir [Bileşik varlık](luis-how-to-add-entities.md).
