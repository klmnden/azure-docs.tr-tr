---
title: 'Öğretici: Listelenen verilerle tam eşleşen metinleri almak için bir LUIS uygulaması oluşturma - Azure | Microsoft Docs'
description: Bu öğreticide veri ayıklama amacıyla amaçları ve liste varlıklarını kullanan basit bir LUIS uygulaması oluşturmayı öğrenerek hızlı bir başlangıç yapacaksınız.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 33394dff1091f27c79c74d8648a90724ba8d6698
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264836"
---
# <a name="tutorial-create-app-using-a-list-entity"></a>Öğretici: Liste varlığı kullanarak uygulama oluşturma
Bu öğreticide önceden tanımlanmış bir listeyle eşleşen verileri nasıl alacağınızı gösteren bir uygulama oluşturacaksınız. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Liste varlıklarını anlama 
> * OrderDrinks amacıyla içecek alanı için yeni bir LUIS uygulaması oluşturma
> * _None_ amacını ve örnek konuşmaları ekleme
> * Konuşmadan içecek öğelerini ayıklamak için liste varlığı ekleme
> * Uygulamayı eğitme ve yayımlama
> * LUIS JSON yanıtını görmek için uygulamanın uç noktasını sorgulama

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS][LUIS] hesabına ihtiyacınız olacak.

## <a name="purpose-of-the-list-entity"></a>Liste varlığının amacı
Bu uygulama `1 coke and 1 milk please` gibi içecek siparişlerini alır ve içecek türü gibi verileri döndürür. İçeceklerden oluşan bir **liste** varlığı, tam metin eşleşmelerini arar ve bu eşleşmeleri döndürür. 

Veri değerleri bilinen bir kümeden olduğunda liste varlığı bu veri türü için iyi bir seçimdir. İçeceklerin adları argo ve kısaltmalarla değişebilir ancak adlar genelde fazla değişiklik göstermez. 

## <a name="app-intents"></a>Uygulama amaçları
Amaçlar, kullanıcı isteklerinin kategorileridir. Bu uygulamada iki amaç vardır: OrderDrink ve None. [None](luis-concept-intent.md#none-intent-is-fallback-for-app) amacı, uygulama haricindeki bir durumu belirtmek için kullanılır.  

## <a name="list-entity-is-an-exact-text-match"></a>Liste varlığı, tam metin eşleşmesidir
Varlığın amacı, konuşma içindeki metnin bölümlerini bulmak ve kategorilere ayırmaktır. [Liste](luis-concept-entity-types.md) varlığında, tam eşleşen sözcükler veya tümcecikler kullanılabilir.  

LUIS, bu içecek uygulaması için içecek siparişini standart sipariş oluşturacak ve gerekli alanları dolduracak şekilde ayıklar. LUIS konuşmalarda farklı sözcükler, kısaltmalar ve argo kullanılmasına izin verir. 

Kullanıcıların konuşmalarına basit örnekler şunlardır:

```
2 glasses of milk
3 bottles of water
2 cokes
```

Konuşmaların kısaltılmış veya argo sürümleri şunlardır:

```
5 milk
3 h2o
1 pop
```
 
Liste varlığı, `h2o` ile suyu ve `pop` ile alkolsüz içeceği eşleştirir.  

## <a name="what-luis-does"></a>LUIS tarafından gerçekleştirilen işlemler
Konuşmadaki amaç ve varlıklar tanımlandıktan, [ayıklandıktan](luis-concept-data-extraction.md#list-entity-data) ve [uç noktadan](https://aka.ms/luis-endpoint-apis) JSON biçiminde döndürüldükten sonra LUIS görevini tamamlamış olur. Arama uygulaması veya sohbet botu bu JSON yanıtını alıp tasarlanma amacına uygun olarak isteği gerçekleştirir. 

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma
1. [LUIS][LUIS] web sitesinde oturum açın. LUIS uç noktalarının yayımlanmasını istediğiniz [bölgede][LUIS-regions] oturum açtığınızdan emin olun.

2. [LUIS][LUIS] web sitesinde **Create new app** (Yeni uygulama oluştur) öğesini seçin.  

    ![Yeni uygulama oluşturma](./media/luis-quickstart-intent-and-list-entity/app-list.png)

3. Açılan iletişim kutusuna `MyDrinklist` adını girin. 

    ![Uygulamaya MyDrinkList adını verin](./media/luis-quickstart-intent-and-list-entity/create-app-dialog.png)

4. İşlem tamamlandıktan sonra uygulamalar **Intents** (Amaçlar) sayfasını ve **None** (Yok) amacını gösterir. 

    [![](media/luis-quickstart-intent-and-list-entity/intents-page-none-only.png "Intents (Amaçlar) sayfasının ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/intents-page-none-only.png#lightbox)

## <a name="create-a-new-intent"></a>Yeni amaç oluşturma

1. **Intents** (Amaçlar) sayfasında **Create new intent** (Yeni amaç oluştur) öğesini seçin. 

    [![](media/luis-quickstart-intent-and-list-entity/create-new-intent.png "Create new intent (Yeni amaç oluştur) düğmesi vurgulanmış Intents (Amaçlar) sayfasının ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/create-new-intent.png#lightbox)

2. Yeni amaç adı olarak `OrderDrinks` girin. Bu amacın kullanıcı içecek siparişi vermek istediğinde seçilmesi gerekir.

    Bu amacı oluşturarak tanımlamak istediğiniz birincil bilgi kategorisini oluşturmuş olursunuz. Bu kategoriye bir ad verdiğinizde LUIS sorgu sonuçlarını kullanan başka uygulamalar da uygun bir yanıt bulmak veya ilgili eylemi gerçekleştirmek için bu kategorinin adını kullanabilir. LUIS bu soruları yanıtlamaz, yalnızca doğal dilde sorulan bilgi türünü tanımlar. 

    [![](media/luis-quickstart-intent-and-list-entity/intent-create-dialog-order-drinks.png "Yeni OrderDrings amacını oluşturma ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/intent-create-dialog-order-drinks.png#lightbox)

3. `OrderDrinks` amacına kullanıcının sormasını beklediğiniz birkaç konuşma girin; örneğin:

    | Örnek konuşmalar|
    |--|
    |Please send 2 cokes and a bottle of water to my room (Lütfen odama 2 kola ve bir şişe su gönderin)|
    |2 perriers with a twist of lime (Limon dilimli 2 Perrier)|
    |h20|

    [![](media/luis-quickstart-intent-and-list-entity/intent-order-drinks-utterance.png "OrderDrinks amaç sayfasında konuşma girme bölümünün gösterildiği ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/intent-order-drinks-utterance.png#lightbox)

## <a name="add-utterances-to-none-intent"></a>None amacına konuşma ekleme

LUIS uygulamasının **None** (Yok) amacında şu an için bir konuşma mevcut değildir. **None** amacında uygulanın yanıtlamasını istemediğiniz konuşmaların bulunması gerekir. Bu bölümü boş bırakmayın. 

1. Sol panelden **Intents** (Amaçlar) öğesini seçin. 

    [![](media/luis-quickstart-intent-and-list-entity/left-panel-intents.png "Sol panelden Intents (Amaçlar) bağlantısının seçilmesini gösteren ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/left-panel-intents.png#lightbox)

2. **None** (Yok) amacını seçin. Kullanıcının girebileceği ancak uygulamanızla ilgili olmayan üç konuşma girin:

    | Örnek konuşmalar|
    |--|
    |Cancel! (İptal!)|
    |Good bye (Güle güle)|
    |What is going on? (Neler oluyor?)|

## <a name="when-the-utterance-is-predicted-for-the-none-intent"></a>Konuşmanın None (Yok) amacına uygun olduğu tahmin edildiğinde
LUIS çağrı uygulamanızda (sohbet botu gibi) LUIS, bir konuşma için **None** (Yok) amacını döndürdüğünde kullanıcının konuşmayı sonlandırmak isteyip istemediğini sorabilir. Kullanıcının konuşmayı sonlandırmak istememesi durumunda bot devam etme talimatları verebilir. 

Varlıklar **None** (Yok) amacı içinde çalışır. En yüksek puanlı amacın **None** (Yok) olması ancak sohbet botunuz için anlamlı olan bir varlık ayıklanması halinde sohbet botunuz müşterinin amacına odaklanan bir soru yöneltebilir. 

## <a name="create-a-menu-entity-from-the-intent-page"></a>Intent (Amaç) sayfasından bir menü varlığı oluşturma
İki amaçta da konuşma bulunduğuna göre artık LUIS uygulamasının içeceğin ne olduğunu anlaması gerekiyor. `OrderDrinks` amacına dönün ve aşağıdaki adımları izleyerek konuşma içindeki içecekleri etiketleyin (işaretleyin):

1. Sol panelde **Intents** (Amaçlar) öğesini seçerek `OrderDrinks` amacına dönün.

2. Amaç listesinden `OrderDrinks` öğesini seçin.

3. `Please send 2 cokes and a bottle of water to my room` konuşmasının içinde `water` sözcüğünü seçin. En üstünde yeni varlık oluşturmak kullanabileceğiniz bir metin kutusu bulunan açılan menü görüntülenir. `Drink` metin kutusuna varlık adını girip açılan menüden **Create new entity** (Yeni varlık oluştur) öğesini seçin. 

    [![](media/luis-quickstart-intent-and-list-entity/intent-label-h2o-in-utterance.png "Konuşmada sözcük seçerek yeni varlık oluşturma işleminin ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/intent-label-h2o-in-utterance.png#lightbox)

4. Açılır pencerede **List** (Liste) varlık türünü seçin. Eş anlamlı olarak `h20` girişini ekleyin. Her eş anlamlıdan sonra Enter tuşuna basın. Eş anlamlı listesine `perrier` öğesini eklemeyin. Bu bir sonraki adımda örnek olarak eklenir. **Done** (Bitti) öğesini seçin.

    [![](media/luis-quickstart-intent-and-list-entity/create-list-ddl.png "Yeni varlık yapılandırma ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/create-list-ddl.png#lightbox)

5. Varlığı oluşturdunuz, şimdi water (su) girişinin eş anlamlısını seçip diğer eş anlamlıları etiketleyin ve ardından açılan listeden `Drink` öğesini seçin. Sağdaki menüden `Set as synonym` ve ardından `water` öğesini seçin.

    [![](media/luis-quickstart-intent-and-list-entity/intent-label-perriers.png "Konuşmayı var olan varlıkla etiketleme işleminin ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/intent-label-perriers.png#lightbox)

## <a name="modify-the-list-entity-from-the-entity-page"></a>Entity (Varlık) sayfasından liste varlığını değiştirme
İçecek listesi varlığı oluşturuldu ancak çok fazla öğe ve eş anlamlı içermiyor. Terimlerin, kısaltmaların ve argo ifadelerin bazılarını biliyorsanız **Entity** (Varlık) sayfasındaki listeyi daha hızlı bir şekilde doldurabilirsiniz. 

1. Sol panelden **Entities** (Varlıklar) öğesini seçin.

    [![](media/luis-quickstart-intent-and-list-entity/intent-select-entities.png "Sol panelden Entities (Varlıklar) bağlantısının seçilmesini gösteren ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/intent-select-entities.png#lightbox)

2. Varlık listesinden `Drink` öğesini seçin.

    [![](media/luis-quickstart-intent-and-list-entity/entities-select-drink-entity.png "Varlıklar listesinden Drink (İçecek) varlığının seçilmesini gösteren ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/entities-select-drink-entity.png#lightbox)

3. Metin kutusuna `Soda pop` yazıp Enter tuşuna basın. Bu, gazlı içecekler için sık kullanılan bir terimdir. Her kültürde bu içecek türü için kullanılan farklı bir ad veya argo terim vardır.

    [![](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-canonical-name.png "Kurallı ad girme işleminin ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-canonical-name.png#lightbox)

4. `Soda pop` ile aynı sıraya şuna benzer eş anlamlılar girin: 

    ```
    coke
    cokes
    coca-cola
    coca-colas
    ```

    Tümcecikler, noktalama işaretleri, iyelik öğeleri ve çoğul ifadeler eş anlamlı olarak eklenebilir. Liste varlığı tam metin eşleşmesi (büyük/küçük harf durumu hariç) olduğundan eş anlamlıların tüm kullanımları kapsaması gerekir. Sorgu günlükleri veya uç nokta isabetlerini gözden geçirdiğinizde daha fazla kullanım öğrenirseniz listeyi genişletebilirsiniz. 

    Bu makalede örneği kısa tutmak için birkaç eş anlamlı kullanılmıştır. Üretim düzeyindeki bir LUIS uygulamasında birçok eş anlamlı bulunacak ve düzenli olarak gözden geçirilip genişletilecektir. 

    [![](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-synonyms.png "Eş anlamlı ekleme işleminin ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-synonyms.png#lightbox)

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme
LUIS uygulaması eğitilene kadar amaçlar ve varlıklar (model) üzerinde yapılan değişiklikleri bilemez. 

1. LUIS web sitesinin sağ üst kısmından **Train** (Eğitim) düğmesini seçin.

    ![Uygulamayı eğitme](./media/luis-quickstart-intent-and-list-entity/train-button.png)

2. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde eğitim tamamlanmış olur.

    ![Eğitim başarılı oldu](./media/luis-quickstart-intent-and-list-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Uç nokta URL'sini almak için uygulamayı yayımlama
Sohbet botunda veya başka bir uygulamada LUIS tahmini almak için uygulamayı yayımlamanız gerekir. 

1. LUIS web sitesinin sağ üst kısmından **Publish** (Yayımla) düğmesini seçin. 

    [![](media/luis-quickstart-intent-and-list-entity/publish.png "Publish (Yayımla) düğmesinin seçilmesini gösteren ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/publish.png#lightbox)

2. Production (Üretim) yuvasını ve ardından **Publish** (Yayımla) düğmesini seçin. 

    [![](media/luis-quickstart-intent-and-list-entity/publish-to-production.png "Publish to production slot (Üretim yuvasına yayımla) düğmesinin seçilmesini gösteren ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/publish-to-production.png#lightbox)

3. Web sitesinin üst kısmında işlemin başarılı olduğunu belirten yeşil durum çubuğunu gördüğünüzde yayımlama işlemi tamamlanmış olur.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Uç noktayı farklı bir konuşmayla sorgulama
1. **Publish** (Yayımla) sayfasının en altında bulunan **endpoint** (uç nokta) bağlantısını seçin. Bu eylem adres çubuğunda uç nokta URL'sinin bulunduğu başka bir tarayıcı penceresi açar. 

    [![](media/luis-quickstart-intent-and-list-entity/publish-select-endpoint.png "Publish (Yayımla) sayfasındaki uç nokta URL'sinin ekran görüntüsü")](media/luis-quickstart-intent-and-list-entity/publish-select-endpoint.png#lightbox)

2. Adres çubuğundaki URL'nin sonuna gidip `2 cokes and 3 waters` yazın. Son sorgu dizesi parametresi konuşma **s**orgusu olan `q` öğesidir. Bu konuşma, etiketlenmiş olan konuşmalarla aynı olmadığından iyi bir testtir ve `OrderDrinks` amacını `cokes` ile `waters` olmak üzere iki içecek türünü döndürmelidir.

```
{
  "query": "2 cokes and 3 waters",
  "topScoringIntent": {
    "intent": "OrderDrinks",
    "score": 0.999998569
  },
  "intents": [
    {
      "intent": "OrderDrinks",
      "score": 0.999998569
    },
    {
      "intent": "None",
      "score": 0.23884207
    }
  ],
  "entities": [
    {
      "entity": "cokes",
      "type": "Drink",
      "startIndex": 2,
      "endIndex": 6,
      "resolution": {
        "values": [
          "Soda pop"
        ]
      }
    },
    {
      "entity": "waters",
      "type": "Drink",
      "startIndex": 14,
      "endIndex": 19,
      "resolution": {
        "values": [
          "h20"
        ]
      }
    }
  ]
}
```

## <a name="where-is-the-natural-language-processing-in-the-list-entity"></a>Doğal dil işleme, List (Liste) varlığının hangi bölümünde gerçekleştirilir? 
Liste varlığı tam metin eşleşmesi olduğundan dil işleme (veya makine öğrenmesi) kullanmaz. LUIS, doğru en yüksek puanlı amacı seçmek için doğal dil işleme (veya makine öğrenmesi) süreçlerini kullanır. Ayrıca bir konuşma birden fazla varlığın karışımı veya birden fazla varlık türü olabilir. Her konuşma **Simple** (Basit) varlığı gibi doğal dil işleme (veya makine öğrenmesi) dahil olmak üzere uygulamadaki tüm varlıklar için işlenir.

## <a name="what-has-this-luis-app-accomplished"></a>Bu LUIS uygulaması hangi işlemleri gerçekleştirdi?
Yalnızca iki amaca ve bir liste varlığına sahip olan bu uygulama, doğal dil sorgu varlığını tanımladı ve ayıklanan verileri döndürdü. 

Sohbet botunuz artık `OrderDrinks` birincil eylemini ve Drink (İçecek) listesi varlığından sipariş verilen içecek türlerini belirlemek için yeterli bilgiye sahip. 

## <a name="where-is-this-luis-data-used"></a>Bu LUIS verileri nerede kullanılır? 
LUIS uygulamasının bu istek üzerinde gerçekleştirebileceği işlemler bu kadardır. Sohbet botu gibi bir çağrı uygulaması topScoringIntent sonucunu ve varlık verilerini alarak bir sonraki adımı gerçekleştirebilir. LUIS, bot veya çağrı uygulaması için programlama işini gerçekleştirmez. LUIS yalnızca kullanıcının amacını belirler. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS uygulamasını silebilirsiniz. Bunu yapmak için uygulama listesinde uygulama adının yanındaki üç nokta menüsüne (...) tıklayıp **Delete** (Sil) öğesini seçin. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Normal ifade eklemeyi öğrenin](luis-quickstart-intents-regex-entity.md)

Sayıları ayıklamak için **number** [önceden oluşturulmuş varlığını](luis-how-to-add-entities.md#add-prebuilt-entity) ekleyin. 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
