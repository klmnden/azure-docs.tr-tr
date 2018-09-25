---
title: LUIS uygulamalarında varlık ekleme
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUIS) uygulamalarında varlıklar (anahtar, uygulamanızın etki alanı veri) ekleyin.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: e82955da24e127e5536c2e40ad2cccf07c5fa173
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47032013"
---
# <a name="manage-entities"></a>Varlıkları yönetme
Uygulamanızın tanımladıktan sonra [hedefleri](luis-concept-intent.md), yapmanız [etiket örnek konuşma](luis-concept-utterance.md) ile [varlıkları](luis-concept-entity-types.md). Varlıklar, önemli bir komut veya soru parçalarıdır ve istemci uygulamanızı kendi görevi gerçekleştirmek gerekli olabilir. 

Ekleme, düzenleme veya uygulamanızdaki varlıklarını silme **varlıkları listesi** üzerinde **varlıkları** sayfası. LUIS, iki ana tür varlıklar sunar: [önceden oluşturulmuş varlıklarla](luis-reference-prebuilt-entities.md)ve kendi özel varlıklarınızı.

Aşağıdaki bölümlerde yalnızca bir LUIS uygulaması içinde kullanılabilir gelen **derleme** bölümü. **Derleme** üst gezinti çubuğunda bir bağlantıdır. Bir kez iç **derleme** bölümünden **varlıkları** sol gezinti menüsünde. Varlık makine öğrenilen ise bir varlık uygulamaya eklendikten sonra yapabilecekleriniz [varlık etiketi](luis-how-to-add-example-utterances.md) utterance içinde. Uygulama eğitim ve yayımlanan sonra varlık verilerini alabilecek [ayıklanan](luis-concept-data-extraction.md) tahmin öğesinden. 

## <a name="add-prebuilt-entity"></a>Önceden oluşturulmuş bir varlık ekleme
Önceden oluşturulmuş varlıklarla tanımlanmış [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text) proje. Bir uygulamaya eklenen ortak önceden oluşturulmuş varlıklar *numarası* ve *datetimeV2*. 

1. Uygulamanızda, gelen **derleme** bölümüne ve ardından **varlıkları** sol bölmesinde.
 
2. Üzerinde **varlıkları** sayfasında **önceden oluşturulmuş varlıklarla yönetme**.

3. İçinde **Ekle veya önceden oluşturulmuş varlıklarla kaldırma** iletişim kutusunda **numarası** ve **datetimeV2** önceden oluşturulmuş varlıklar. Ardından **Bitti**'yi seçin.

    ![Önceden oluşturulmuş varlık iletişim kutusu Ekle ekran görüntüsü](./media/add-entities/list-of-prebuilt-entities.png)

    Bkz: [veri ayıklama](luis-concept-data-extraction.md#prebuilt-entity-data) JSON sorgu yanıtı uç noktasından oluşturulmuş varlık ayıklama hakkında daha fazla bilgi edinmek için.

## <a name="add-simple-entities"></a>Basit bir varlık ekleme
Bir varlığın tek bir kavram açıklayan genel bir varlıktır. 

1. Uygulamanızda, gelen **derleme** bölümüne ve ardından **varlıkları** Sol paneli ve ardından **yeni varlık Oluştur**.

2. Açılan iletişim kutusuna `Airline` içinde **varlık adı** kutusunda **basit** gelen **varlık türü** listeleyin ve ardından **Bitti**.

    ![Havayolu Basit varlık oluşturma, iletişim kutusunun ekran görüntüsü](./media/add-entities/create-simple-airline-entity.png)

    Bkz: [veri ayıklama](luis-concept-data-extraction.md#simple-entity-data) JSON sorgu yanıtı uç noktasından Basit varlık ayıklama hakkında daha fazla bilgi edinmek için. Varlığın deneyin [hızlı](luis-quickstart-primary-and-secondary-data.md) tek bir varlığın kullanma hakkında daha fazla bilgi için.

## <a name="add-regular-expression-entities"></a>Normal ifade varlıkları ekleyin
Bir normal ifade varlık sağladığınız bir normal ifadeye göre utterance verileri dışarı çıkarmak için kullanılır. 

1. Uygulamanızda seçin **varlıkları** sol gezinti ve ardından **yeni varlık Oluştur**.

2. Açılan iletişim kutusuna `AirFrance Flight` içinde **varlık adı** kutusunda **normal ifade** gelen **varlık türü** listesinde, normal ifade girin`AFR[0-9]{3,4}`ve ardından **Bitti**. 

    Bu AirFrance uçuş normal ifade üç karakter, gerçek anlamda bekliyor `AFR`, ardından 3 veya 4 rakamı. Sayı 0 ile 9 arasında herhangi bir sayı olabilir. AirFrance uçuş numaraları gibi normal ifadeyle eşleşen: "AFR101", "ARF1302" ve "AFR5006". Bkz: [veri ayıklama](luis-concept-data-extraction.md) JSON sorgu yanıtı uç noktasından varlık ayıklama hakkında daha fazla bilgi edinmek için.

    ![Normal ifade varlık oluşturmak için iletişim kutusunun görüntüsü](./media/add-entities/regex-entity-create-dialog.png)

    Bkz: [veri ayıklama](luis-concept-data-extraction.md#regular-expression-entity-data) JSON sorgu yanıtı uç noktasından normal ifade varlık ayıklama hakkında daha fazla bilgi edinmek için. Normal ifade varlık deneyin [hızlı](luis-quickstart-intents-regex-entity.md) bir normal ifade varlık kullanma hakkında daha fazla bilgi için.

## <a name="add-hierarchical-entities"></a>Hiyerarşik bir varlık ekleme
Hiyerarşik bir varlık, bağlamsal öğrenilen ve kavramsal olarak ilişkili varlıkları kategorisidir. Aşağıdaki örnekte, kaynak ve hedef konumları varlık içerir. 

Utterance içinde `Book 2 tickets from Seattle to Cairo`, Seattle kaynak konumu ve Cairo hedef konumu. Her kelime sırasını ve sözcük seçenek utterance bağlamsal olarak farklı ve öğrenilen konumdur.

Hiyerarşik bir varlık eklemek için aşağıdaki adımları tamamlayın: 

1. Uygulamanızda seçin **varlıkları** sol gezinti ve ardından **yeni varlık Oluştur**.

2. Açılan iletişim kutusuna `Location` içinde **varlık adı** kutusuna ve ardından **hiyerarşik** gelen **varlık türü** listesi.

    ![Hiyerarşik varlık ekleme](./media/add-entities/hier-location-entity-creation.png)

3. Seçin **alt öğe Ekle**, ve "Başlangıç" yazın **alt #1** kutusu. 

4. Seçin **alt öğe Ekle**, ve "Hedef" yazın **alt 2** kutusu. **Done** (Bitti) öğesini seçin.
5. 
    >[!NOTE]
    >Bir alt silmek için yanındaki Çöp Kutusu simgesini seçin.

    >[!CAUTION]
    >Tek bir uygulamada tüm varlıklar üzerinde alt varlık adlarının benzersiz olması gerekir. Alt varlıklar aynı ada sahip iki farklı hiyerarşik varlıklar içerebilir. 

    Bkz: [veri ayıklama](luis-concept-data-extraction.md#hierarchical-entity-data) JSON sorgu yanıtı uç noktasından hiyerarşik varlık ayıklama hakkında daha fazla bilgi edinmek için. Hiyerarşik varlık deneyin [hızlı](luis-quickstart-intent-and-hier-entity.md) hiyerarşik bir varlık kullanma hakkında daha fazla bilgi için.

## <a name="add-composite-entities"></a>Bileşik varlık ekleme
Bileşik bir varlık oluşturarak birkaç mevcut varlıklar arasında ilişkiler tanımlayabilirsiniz. Aşağıdaki örnekte, biletleri, kaynak ve hedef konumların sayısı varlık içerir. 

Utterance içinde `Book 2 tickets from Seattle to Cairo`, 2 sayısı önceden oluşturulmuş bir varlığa eşleştirilir, Seattle kaynak konumu ve Cairo hedef konumu. Bileşik bir varlık oluşturduktan sonra her varlık daha büyük bir üst varlığa bir parçasıdır.

1. Uygulamanızda, önceden oluşturulmuş varlık Ekle **numarası**. Yönergeler için [önceden oluşturulmuş varlıklar ekleme](#add-prebuilt-entity). 

2. Hiyerarşik varlık ekleme `Location`, alt türleri dahil olmak üzere: `origin`, `destination`. Daha fazla bilgi için bkz. [hiyerarşik varlık Ekle](#add-hierarchical-entities). 

3. Seçin **varlıkları** sol gezinti ve ardından **yeni varlık Oluştur**.

4. Açılan iletişim kutusuna `TicketsOrder` içinde **varlık adı** kutusuna ve ardından **bileşik** gelen **varlık türü** listesi.

5. Seçin **alt öğe Ekle** yeni bir alt öğe eklemek için.

6. İçinde **alt #1**, varlığı seçin **numarası** listeden.

7. İçinde **alt 2**, varlığı seçin **Location::Origin** listeden. 

8. İçinde **alt 3**, varlığı seçin **Location::Destination** listeden. 

9. **Done** (Bitti) öğesini seçin.

    ![Bileşik bir varlık oluşturmak için iletişim kutusunun görüntüsü](./media/add-entities/ticketsorder-composite-entity.png)

    >[!NOTE]
    >Alt silmek için çöp kutusu düğmesine yanında seçin.

    Bkz: [veri ayıklama](luis-concept-data-extraction.md#composite-entity-data) JSON sorgu yanıtı uç noktasından Bileşik varlık ayıklama hakkında daha fazla bilgi edinmek için. Bileşik varlık deneyin [öğretici](luis-tutorial-composite-entity.md) bileşik bir varlık kullanma hakkında daha fazla bilgi için.


## <a name="add-patternany-entities"></a>Pattern.Any varlık ekleme
[Pattern.Any](luis-concept-entity-types.md) varlıklardır yalnızca geçerli [desenleri](luis-how-to-model-intent-pattern.md). Bu varlık, değişken uzunluğu ve sözcük seçimi varlıklarının sonuna Bul LUIS yardımcı olur. Bu varlık içindeki bir desenle kullanıldığından LUIS son varlık içinde utterance olduğu bilir.

Bir uygulama varsa bir `FindBookInfo` hedefi, defteri başlığı ile hedefi tahmini da etkileyebilir. Kitap başlığında sözcüklerdir açıklamak için bir desen içindeki bir Pattern.any kullanın. LUIS tahmin utterance ile başlar. İlk olarak, utterance işaretli ve varlıkları bulunduğunda deseni işaretli ve eşleşen varlıklar için eşleşen. 

Utterance içinde `Who wrote the book Ask and when was it published?`, başlık sona ereceği ve utterance geri kalanını başladığı bağlamsal olarak açık olduğundan Ask kitap başlığı olduğunu. Kitap başlıklarının herhangi bir sırada tek bir sözcük dahil olmak üzere sözcük noktalama işaretleri ile karmaşık ifadeleri ve nonsensical sözcük sıralama olabilir. Bir desen, bir varlık oluşturmak, tam ve tam varlık ayıkladığınız sağlar. Kitap başlığı bulunduğunda `FindBookInfo` amacı, desenin amacı olduğundan tahmin.

1. Uygulamanızda, gelen **derleme** bölümüne ve ardından **varlıkları** Sol paneli ve ardından **yeni varlık Oluştur**.

2. İçinde **varlık Ekle** iletişim kutusuna `BookTitle` içinde **varlık adı** kutusunda ve seçin **Pattern.any** olarak **varlık türü**.
 
    ![Pattern.any varlık oluşturma işleminin ekran görüntüsü](./media/add-entities/create-pattern-any-entity.png)

    Pattern.any varlık kullanmak için üzerinde bir desen Ekle **desenleri** sayfa (altında **uygulama performansını** bölümü) doğru küme ayracı sözdizimi ile gibi "için **{BookTitle}** Yazar kimdir? ".

    Bkz: [veri ayıklama](luis-concept-data-extraction.md#patternany-entity-data) JSON sorgu yanıtı uç noktasından Pattern.any varlık ayıklama hakkında daha fazla bilgi edinmek için. Deneyin [deseni](luis-tutorial-pattern.md) Pattern.any varlık kullanma hakkında daha fazla bilgi edinmek için öğretici.

Bir Pattern.any içerdiğinde, deseni bulabilirsiniz, ayıklar varlıkları yanlış kullanmak bir [açık listesi](luis-concept-patterns.md#explicit-lists) bu sorunu düzeltmek için. 

## <a name="add-role-to-pattern-based-entity"></a>Rolü için desen tabanlı varlık Ekle
Adlandırılmış alt bağlamını temel alan bir varlığın rolüdür. İçin karşılaştırılabilir bir [hiyerarşik](#add-hierarchical-entities) varlık ancak rolleri yalnızca kullanıldığı [desenleri](luis-how-to-model-intent-pattern.md). 

Örneğin, uçak bileti sahip bir *kaynak şehri* ve *hedef şehri*, ancak her ikisini de şehir. LUIS, hem de Şehir olduğundan ve sözcük sırasını ve sözcük seçimi, bağlama göre kaynak ve hedef şehirler belirleyebilirsiniz belirler. 

Bir rol için söz dizimi **{varlık adı: rol adı}** burada varlık adının ardından bir iki nokta üst üste sonra rol adı. Örneğin, "kitap bilet {konumu: Origin} öğesinden {konumu: hedef}".

1. Uygulamanızda, gelen **derleme** bölümüne ve ardından **varlıkları** sol bölmesinde.

2. **Create new entity** (Yeni varlık oluştur) öğesini seçin. Adını `Location`. Tür seçin **basit** seçip **bitti**

3. Seçin **varlıkları** sol panelden yeni varlık seçip **konumu** 2. adımda oluşturduğunuz.

4. İçinde **rol adı** metin kutusuna rol adını girin `Origin` girin. İkinci bir rol adını ekleyin `Destination`. Örneğin, uçak yolculuğu bir kaynak ve hedef şehri olabilir. İki "Başlangıç" ve "Hedef" rolleridir.

    ![Kaynak rolü konumu varlığa ekleme işleminin ekran görüntüsü](./media/add-entities/roles-enter-role-name-text.png)

    Bkz: [veri ayıklama](luis-concept-data-extraction.md) rolleri JSON sorgu yanıtı uç noktasından ayıklama hakkında daha fazla bilgi edinmek için. Pattern.any varlık kullanma hakkında daha fazla bilgi için desen öğreticisini deneyin.

## <a name="add-list-entities"></a>Liste varlık ekleme
Liste varlık sisteminizde ilgili sözcükler sabit, kapalı bir kümesini temsil eder. 

İçecekler listesi varlık için iki normalleştirilmiş değerlere sahip olabilir: su ve soda açılır. Eş Anlamlılar her normalleştirilmiş bir adı vardır. Su için H20, doğalgaz, düz eşanlamlıdır. Soda pop için eş anlamlı sözcükler Meyve, cola ginger olan. Bir varlık oluşturduğunuzda tüm değerleri bilmeniz gerekmez. Daha fazla, eş anlamlı sözcüklerle gerçek kullanıcı konuşma gözden geçirdikten sonra ekleyebilirsiniz.

|Normalleştirilmiş adı|Eş anlamlılar|
|--|--|
|Su|H20 gaz, düz|
|Soda pop|Meyve, cola ginger|

1. Uygulamanızda, gelen **derleme** bölümüne ve ardından **varlıkları** Sol paneli ve ardından **yeni varlık Oluştur**.

2. İçinde **varlık Ekle** iletişim kutusuna `Drinks` içinde **varlık adı** kutusunda ve seçin **listesi** olarak **varlık türü**. **Done** (Bitti) öğesini seçin.
 
    ![İçecekler liste varlığı oluşturma iletişim kutusunun görüntüsü](./media/add-entities/menu-list-dialog.png)
  
3.  Liste varlık sayfası normalleştirilmiş adları eklemenize olanak sağlar. İçinde **değerleri** metin kutusu, bir öğe listesi için aşağıdaki gibi girin `Water` için İçecekler listesinden sonra klavyedeki Enter tuşuna basın. 

    ![Ekran görüntüsü, İçecekler liste varlığı ile su normalleştirilmiş metin kutusundaki değeri](./media/add-entities/entity-list-normalized-name.png)

4. Normalleştirilmiş değeri sağındaki **su**, eş anlamlı sözcükler girin `h20`, `flat`, ve `gas`, her öğenin sonunda klavyedeki Enter tuşuna basın.

    ![Ekran görüntüsü, İçecekler liste varlığı ile vurgulanmış su için eş anlamlı öğeleri](./media/add-entities/menu-list-synonyms.png)

5. Daha fazla normalleştirilmiş öğeleri listesini istiyorsanız seçin **önerilir** seçenekleri görmek için [anlam sözlük](luis-glossary.md#semantic-dictionary).

    ![Ekran görüntüsü, bol liste varlığı ile vurgulanmış önerilen öğeler](./media/add-entities/entity-list-recommended-list.png)

6. Normalleştirilmiş bir değer olarak ekleyin veya seçmek için önerilen listesinden bir öğe seçin **tüm eklemek** tüm öğeleri eklenecek. 

    ![Ekran görüntüsü, bol Bira önerilen öğesi ile varlık listesinde ve tüm düğmesi ekleme](./media/add-entities/list-entity-add-suggestions.png)

    Bkz: [veri ayıklama](luis-concept-data-extraction.md#list-entity-data) JSON sorgu yanıtı uç noktasından listesi varlık ayıklama hakkında daha fazla bilgi edinmek için. Deneyin [hızlı](luis-quickstart-intent-and-list-entity.md) liste varlığı kullanma hakkında daha fazla bilgi için.

## <a name="import-list-entity-values"></a>Liste varlık değerlerini Al
Var olan bir liste varlığa değerleri içeri aktarabilirsiniz.

 1. Liste varlık sayfasında **alma listeler**.

 2. İçinde **alma yeni girişler** iletişim kutusunda **Dosya Seç** ve liste içeren bir JSON dosyasını seçin.

    ![Ekran görüntüsü, içeri aktarma listesi varlık değerlerini açılır iletişim kutusunda](./media/add-entities/menu-list-import-json-dialog-with-file.png)

    >[!NOTE]
    >LUIS ile uzantısı ".json" yalnızca dosyalarını içe aktarır.

 3. Json'da desteklenen liste sözdizimi hakkında daha fazla bilgi edinmek için seçin **desteklenenler listesinde söz dizimi hakkında bilgi edinin** iletişim genişletin ve izin verilen sözdizimi örneği görüntülemek için. İletişim Daralt ve söz dizimi gizlemek için bağlantı başlığı seçin.

 4. **Done** (Bitti) öğesini seçin.

    Geçerli bir json örneği bir **renkleri** liste varlığı, aşağıdaki JSON ile biçimlendirilmiş kodda gösterilmiştir:

    ```
    [
        {
            "canonicalForm": "Blue",
            "list": [
                "navy",
                "royal",
                "baby"
            ]
        },
        {
            "canonicalForm": "Green",
            "list": [
                "kelly",
                "forest",
                "avacado"
            ]
        }
    ]  
    ```

## <a name="edit-entity-name"></a>Varlık adı Düzenle
1. Üzerinde **varlıkları** liste sayfası, varlık listesinden seçin. Bu eylem açılır **varlık** sayfası.

2. Üzerinde **varlık** sayfasında, varlık adı düzenleyin varlık adının yanındaki Düzenle simgesini seçerek. Varlık türü düzenlenebilir değil. 

## <a name="delete-entity"></a>Varlığı Sil

Üzerinde **varlık** sayfasında **Sil varlık** düğmesi. Ardından, **Tamam** silme işlemini onaylamak için onay iletisi.
 
![Varlığı Sil düğmesi vurgulanmış ekran, konum varlık sayfası](./media/add-entities/entity-delete.png)

>[!NOTE]
>* Hiyerarşik bir varlığı silmek, tüm alt varlıklar siler.
>* Bileşik bir varlığı silme yalnızca Birleşik siler ve bileşik ilişkisini keser, ancak onu oluşturan varlıkları silmez.

## <a name="changing-entity-type"></a>Varlık türünü değiştirme
LUIS, ekleme veya kaldırma, varlık oluşturmak için gerekenler bilmediği varlık türünü değiştirmek izin vermez. Türü değiştirmek için biraz daha farklı bir adla doğru türde yeni bir varlık oluşturmak iyidir. Varlık oluşturulduktan sonra eski etiketli varlık adı her utterance içinde kaldırıp yeni varlık adı ekleyin. Tüm sesleri relabeled sonra eski varlığı silin. 

## <a name="create-a-pattern-from-an-utterance"></a>Bir utterance bir düzen oluşturma
Bkz: [hedefi veya varlık sayfasında mevcut utterance Ekle deseni](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="search-utterances"></a>Arama konuşma
Arama ve filtreleme ile araç çubuğundaki Büyüteç simgesinin konuşma. 

## <a name="train-your-app-after-changing-model-with-entities"></a>Varlıkları modeliyle değiştirdikten sonra uygulamanızı eğitin
Ekleme, düzenleme veya varlıkları Kaldır sonra [eğitme](luis-how-to-train.md) ve [yayımlama](luis-how-to-publish-app.md) uygulamanız için uç nokta sorguları etkilemek yaptığınız değişiklikleri. 

## <a name="next-steps"></a>Sonraki adımlar
Hedefleri ve konuşma varlıklarını ekledikten sonra temel bir LUIS uygulaması sahip. Bilgi nasıl [eğitme](luis-how-to-train.md), [test](luis-interactive-test.md), ve [yayımlama](luis-how-to-publish-app.md) uygulamanızı.
 
