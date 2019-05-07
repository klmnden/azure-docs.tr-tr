---
title: Varlık ekleme
titleSuffix: Language Understanding - Azure Cognitive Services
description: Language Understanding (LUIS) uygulamalarında kullanıcı konuşma anahtar verileri ayıklamak için varlıklar oluşturun.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 241e89ac7fa78184e7c55f9e8065e1534cea9143
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148722"
---
# <a name="create-entities-without-utterances"></a>Konuşma olmadan varlık oluşturma

Varlık, bir sözcük veya tümcecik ayıklanan istediğiniz utterance içinde temsil eder. Bir varlık (basamak, noktalar, kişiler, olayları veya kavramları) benzer nesnelerinin bir koleksiyonunu içeren bir sınıfı temsil eder. Bazen uygulamanızın görevini gerçekleştirmek gerekli olan ve bilgi ıntent'e ilgili varlıkları anlatmaktadır. (Önce veya sonra) bir utterance bir hedefi veya sonraya gelen eklediğinizde varlık oluşturabileceğiniz bir amaç için bir utterance ekleme.

Ekleme, düzenleme veya LUIS uygulamanızda varlıklarını silme **varlıkları listesi** üzerinde **varlıkları** sayfası. LUIS, iki ana tür varlıklar sunar: [önceden oluşturulmuş varlıklarla](luis-reference-prebuilt-entities.md)ve kendi [özel varlıklar](luis-concept-entity-types.md#types-of-entities).

Makine öğrenilen bir varlık oluşturulduktan sonra bu durumda tüm hedefleri tüm örnek utterance varlıkta işaretlemek gerekir.

<a name="add-prebuilt-entity"></a>

## <a name="add-a-prebuilt-entity-to-your-app"></a>Önceden oluşturulmuş bir varlık ekleme

Bir uygulamaya eklenen ortak önceden oluşturulmuş varlıklar *numarası* ve *datetimeV2*. 

1. Uygulamanızda, gelen **derleme** bölümünden **varlıkları** sol bölmesinde.
 
1. Üzerinde **varlıkları** sayfasında **önceden oluşturulmuş varlıklar ekleme**.

1. İçinde **önceden oluşturulmuş varlıklar ekleme** iletişim kutusunda **numarası** ve **datetimeV2** önceden oluşturulmuş varlıklar. Ardından **Bitti**'yi seçin.

    ![Önceden oluşturulmuş varlık iletişim kutusu Ekle ekran görüntüsü](./media/add-entities/list-of-prebuilt-entities.png)

<a name="add-simple-entities"></a>

## <a name="add-simple-entities-for-single-concepts"></a>Tek kavramlar için basit bir varlık ekleme

Bir varlığın tek bir kavramı açıklanmaktadır. Şirket bölüm adlarını gibi ayıklar bir varlık oluşturmak için aşağıdaki yordamı kullanın. *İnsan Kaynakları* veya *Operations*.   

1. Uygulamanızda seçin **derleme** bölümüne ve ardından **varlıkları** sol panelde, ardından **yeni varlık Oluştur**.

1. Açılan iletişim kutusuna `Location` içinde **varlık adı** kutusunda **basit** gelen **varlık türü** listeleyin ve ardından **Bitti**.

    Bu varlık oluşturulduktan sonra varlık içeren örnek konuşma sahip tüm hedefleri için gidin. Örnek utterance metni seçin ve metin varlık olarak işaretleyin. 

    A [tümcecik listesi](luis-concept-feature.md) genellikle bir varlığın sinyal artırmak için kullanılır.

<a name="add-regular-expression-entities"></a>

## <a name="add-regular-expression-entities-for-highly-structured-concepts"></a>Yüksek düzeyde yapılandırılmış kavramlar için normal ifade varlıkları ekleyin

Bir normal ifade varlık sağladığınız bir normal ifadeye göre utterance verileri dışarı çıkarmak için kullanılır. 

1. Uygulamanızda seçin **varlıkları** sol gezinti ve ardından **yeni varlık Oluştur**.

1. Açılan iletişim kutusuna `Human resources form name` içinde **varlık adı** kutusunda **normal ifade** gelen **varlık türü** listesinde, normal ifade girin`hrf-[0-9]{6}`ve ardından **Bitti**. 

    Bu normal ifade sabit karakterleriyle eşleşen `hrf-`, daha sonra 6 basamaklı bir formu temsil etmek için bir insan kaynakları form sayı.

<a name="add-composite-entities"></a>

## <a name="add-composite-entities-to-group-into-a-parent-child-relationship"></a>Bir üst-alt ilişkisi gruplandırmak için bileşik bir varlık ekleme

Bileşik bir varlık oluşturarak farklı türlerde varlıklar arasında ilişkiler tanımlayabilirsiniz. Aşağıdaki örnekte, varlık, normal bir ifade ve önceden oluşturulmuş bir varlık adını içerir.  

Utterance içinde `Send hrf-123456 to John Smith`, metin `hrf-123456` bir insan kaynakları için eşleşen [normal ifade](#add-regular-expression-entities) ve `John Smith` ile önceden oluşturulmuş varlık personName ayıklanır. Her varlık, daha büyük bir üst varlığa bir parçasıdır. 

1. Uygulamanızda seçin **varlıkları** sol gezinti bölmesinden **derleme** bölümüne ve ardından **önceden oluşturulmuş bir varlık ekleme**.

1. Önceden oluşturulmuş varlık ekleme **PersonName**. Yönergeler için [önceden oluşturulmuş varlıklar ekleme](#add-prebuilt-entity). 

1. Seçin **varlıkları** sol gezinti ve ardından **yeni varlık Oluştur**.

1. Açılan iletişim kutusuna `SendHrForm` içinde **varlık adı** kutusuna ve ardından **bileşik** gelen **varlık türü** listesi.

1. Seçin **alt öğe Ekle** yeni bir alt öğe eklemek için.

1. İçinde **alt #1**, varlığı seçin **numarası** listeden.

1. İçinde **alt 2**, varlığı seçin **İnsan Kaynakları form adı** listeden. 

1. **Done** (Bitti) öğesini seçin.

<a name="add-pattern-any-entities"></a>

## <a name="add-patternany-entities-to-capture-free-form-entities"></a>Serbest biçimli varlıkları yakalamak için Pattern.any varlık ekleme

[Pattern.Any](luis-concept-entity-types.md) varlıklardır yalnızca geçerli [desenleri](luis-how-to-model-intent-pattern.md), amacı değildir. Bu varlık türü, değişken uzunluğu ve sözcük seçimi varlıklarının sonuna Bul LUIS yardımcı olur. Bu varlık içindeki bir desenle kullanıldığından LUIS son varlık utterance şablonda olduğu bilir.

Bir uygulama varsa bir `FindHumanResourcesForm` hedefi, ayıklanan form başlığı ile hedefi tahmini da etkileyebilir. Formu başlığında sözcüklerdir açıklamak için bir desen içindeki bir Pattern.any kullanın. LUIS tahmin utterance ile başlar. İlk olarak, utterance işaretli ve varlıkları bulunduğunda deseni işaretli ve eşleşen varlıklar için eşleşen. 

Utterance içinde `Where is Request relocation from employee new to the company on the server?`, başlık sona ereceği ve utterance geri kalanını başladığı bağlamsal olarak açık olduğundan form başlığı olduğunu. Başlıklar, sözcük noktalama işaretleri ile karmaşık ifadeleri tek bir sözcük dahil olmak üzere herhangi bir sırada ve nonsensical sözcük sıralama olabilir. Bir desen, bir varlık oluşturmak, tam ve tam varlık ayıkladığınız sağlar. Başlık bulunduğunda `FindHumanResourcesForm` amacı, desenin amacı olduğundan tahmin.

1. Gelen **derleme** bölümünden **varlıkları** Sol paneli ve ardından **yeni varlık Oluştur**.

1. İçinde **varlık Ekle** iletişim kutusuna `HumanResourcesFormTitle` içinde **varlık adı** kutusunda ve seçin **Pattern.any** olarak **varlık türü**.

    Pattern.any varlık kullanmak için üzerinde bir desen Ekle **desenleri** sayfasında **uygulama performansını** doğru küme ayracı sözdizimi bölümündeki `Where is **{HumanResourcesFormTitle}** on the server?`.

    Pattern.any içerdiğinde deseninizin varlıkları yanlış ayıkladığını fark ederseniz bu sorunu gidermek için [açık liste](luis-concept-patterns.md#explicit-lists) kullanın. 

<a name="add-a-role-to-pattern-based-entity"></a>

## <a name="add-a-role-to-distinguish-different-contexts"></a>Farklı bağlamlardaki ayırt etmek için bir rol eklemek

Bağlama göre adlandırılmış alt rolüdür. Önceden oluşturulmuş ve makine öğrenilen varlıklar dahil olmak üzere tüm varlıkları kullanılabilir. 

Bir rol için söz dizimi **`{Entityname:Rolename}`** nerede varlık adının ardından bir iki nokta üst üste sonra rol adı. Örneğin, `Move {personName} from {LocationUsingRoles:Origin} to {LocationUsingRoles:Destination}`.

1. Gelen **derleme** bölümünden **varlıkları** sol bölmesinde.

1. **Create new entity** (Yeni varlık oluştur) öğesini seçin. Adını `LocationUsingRoles`. Tür seçin **basit** seçip **Bitti**. 

1. Seçin **varlıkları** sol panelden yeni varlık seçip **LocationUsingRoles** önceki adımda oluşturduğunuz.

1. İçinde **rol adı** metin kutusuna rol adını girin `Origin` girin. İkinci bir rol adını ekleyin `Destination`. 

    ![Kaynak rolü konumu varlığa ekleme işleminin ekran görüntüsü](./media/add-entities/roles-enter-role-name-text.png)

<a name="add-list-entities"></a>

## <a name="add-list-entities-for-exact-matches"></a>Tam eşleşme için liste varlık ekleme

Liste varlık ilgili sözcükler sabit, kapalı bir kümesini temsil eder. 

İnsan Kaynakları bir uygulama için tüm Departmanlar için eş anlamlı sözcükler yanı sıra tüm Departmanlar listesi olabilir. Bir varlık oluşturduğunuzda tüm değerleri bilmeniz gerekmez. Daha fazla, eş anlamlı sözcüklerle gerçek kullanıcı konuşma gözden geçirdikten sonra ekleyebilirsiniz.

1. Gelen **derleme** bölümünden **varlıkları** Sol paneli ve ardından **yeni varlık Oluştur**.

1. İçinde **varlık Ekle** iletişim kutusuna `Department` içinde **varlık adı** kutusunda ve seçin **listesi** olarak **varlık türü**. **Done** (Bitti) öğesini seçin.
  
1. Liste varlık sayfası normalleştirilmiş adları eklemenize olanak sağlar. İçinde **değerleri** metin gibi liste için bir bölüm adı girin `HumanResources` klavyedeki Enter tuşuna basın. 

1. Normalleştirilmiş değeri sağında her öğenin sonunda klavyedeki Enter'tuşuna basarak eş anlamlı sözcükler girin.

1. Daha fazla normalleştirilmiş öğeleri listesini istiyorsanız seçin **önerilir** seçenekleri görmek için [anlam sözlük](luis-glossary.md#semantic-dictionary).

    ![Seçenekleri görmek için önerilen özellik seçimi ekran görüntüsü](./media/add-entities/hr-list-2.png)


1. Normalleştirilmiş bir değer olarak ekleyin veya seçmek için önerilen listesinden bir öğe seçin **tüm eklemek** tüm öğeleri eklenecek. 
    Değerler şu JSON biçimini kullanarak var olan bir liste varlığa içeri aktarabilirsiniz:

    ```JSON
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

<a name="change-entity-type"></a>

## <a name="do-not-change-entity-type"></a>Varlık türü değiştirmeyin.

LUIS, ekleme veya kaldırma, varlık oluşturmak için gerekenler bilmediği varlık türünü değiştirmek izin vermez. Türü değiştirmek için biraz daha farklı bir adla doğru türde yeni bir varlık oluşturmak iyidir. Varlık oluşturulduktan sonra eski etiketli varlık adı her utterance içinde kaldırıp yeni varlık adı ekleyin. Tüm sesleri relabeled sonra eski varlığı silin. 

<a name="create-a-pattern-from-an-utterance"></a>

## <a name="create-a-pattern-from-an-example-utterance"></a>Bir örnek utterance bir düzen oluşturma

Bkz: [hedefi veya varlık sayfasında mevcut utterance Ekle deseni](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="train-your-app-after-changing-model-with-entities"></a>Varlıkları modeliyle değiştirdikten sonra uygulamanızı eğitin

Ekleme, düzenleme veya varlıkları Kaldır sonra [eğitme](luis-how-to-train.md) ve [yayımlama](luis-how-to-publish-app.md) uygulamanız için uç nokta sorguları etkilemek yaptığınız değişiklikleri. 

## <a name="next-steps"></a>Sonraki adımlar

Önceden oluşturulmuş varlıklarla ilgili daha fazla bilgi için bkz. [tanıyıcıları metin](https://github.com/Microsoft/Recognizers-Text) proje. 

Varlık JSON uç nokta sorgu yanıtına görüntülenme şeklini hakkında daha fazla bilgi için bkz: [veri ayıklama](luis-concept-data-extraction.md)

Hedefleri ve konuşma varlıklarını ekledikten sonra temel bir LUIS uygulaması sahip. Bilgi nasıl [eğitme](luis-how-to-train.md), [test](luis-interactive-test.md), ve [yayımlama](luis-how-to-publish-app.md) uygulamanızı.
 
