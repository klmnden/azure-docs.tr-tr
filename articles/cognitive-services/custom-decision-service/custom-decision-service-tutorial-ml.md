---
title: Bilişsel hizmetler Azure Machine Learning - | Microsoft Docs
description: Machine learning hizmetindeki Azure özel kararı, bağlamsal karar vermek için bir bulut tabanlı API için Öğreticisi.
services: cognitive-services
author: slivkins
manager: slivkins
ms.service: cognitive-services
ms.topic: article
ms.date: 05/08/2018
ms.author: slivkins;marcozo;alekh
ms.openlocfilehash: 50814d67ee39c6657954610358462d877843416e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354598"
---
# <a name="machine-learning"></a>Makine öğrenimi

Bu öğretici, Gelişmiş makine öğrenimi özel karar hizmetinin işlevleri giderir. Öğretici iki bölümden oluşur: [featurization](#featurization-concepts-and-implementation) ve [özellik belirtimi](#feature-specification-format-and-apis). Featurization verilerinizi "Özellikler" olarak temsil eden için machine learning için başvuruyor. Özellik belirtimi JSON biçimi ve Özellikler belirtmek için bulunabilecek API'leri kapsar.

Varsayılan olarak, machine learning özel karar hizmetinde müşteriye saydamdır. Özellikler içeriğinizi otomatik olarak ayıklanır ve standart öğrenmeyi öğrenme algoritmasını kullanılır. Özellik ayıklama birkaç diğer Azure Bilişsel hizmetler yararlanır: [varlık bağlama](../entitylinking/home.md), [metin analizi](../text-analytics/overview.md), [duygu](../emotion/home.md), ve [bilgisayar görme](../computer-vision/home.md). Bu öğretici, varsayılan işlevsellik kullanılan yalnızca atlanabilir.

## <a name="featurization-concepts-and-implementation"></a>Featurization: kavramlar ve uygulama

Özel karar hizmet tek tek kararlarını verir. Her bir kararı birkaç alternatifleri arasında paketini, Eylemler seçme içerir. Uygulamaya bağlı olarak, tek bir eylem veya Eylemler (kısa) sıralı bir listesini kararı tercih edebilirsiniz.

Örneğin, bir Web sitesi ön sayfasında makalelerinin seçimini kişiselleştirme. Burada, karşılık gelen eylemleri aşağıdaki makalelere ve belirli bir kullanıcıya göstermek için hangi makaleleri her bir karardır.

Her eylem henceforth adlı özelliklerinin vektörü ile temsil edilen *özellikleri*. Otomatik olarak ayıklanan özelliklerin yanı sıra yeni özellikleri belirtebilirsiniz. Ayrıca, bazı özellikler oturum ancak machine learning için yoksaymak için özel karar hizmet söyleyebilirsiniz.

### <a name="native-vs-internal-features"></a>Yerel iç özellikleri karşılaştırması

Özellikler, uygulamanız için bir en iyi biçiminde belirtin, bir sayı, bir dize veya dizi olması. Bu özellikler "yerel özellik." olarak adlandırılır Özel karar hizmet her yerel özelliği "İç özellikleri." adlı bir veya daha fazla sayısal özellikleri çevirir.

İç özelliklerine çevirisi aşağıdaki gibidir:

- sayısal özellikleri aynı kalır.
- Dizideki her öğe için bir tane çeşitli sayısal özellikler için sayısal bir dizi çevirir.
- bir dize değerli özellik `"Name":"Value"` ada sahip bir özellik erişimcisine varsayılan olarak açıktır `"NameValue"` ve 1 değeri.
- İsteğe bağlı olarak bir dize olarak temsil edilebilir [sözcükler paketi](https://en.wikipedia.org/wiki/Bag-of-words_model). Ardından bir iç özelliği değeri bu word oluşumları sayısıdır. Bu dize her sözcüğün için oluşturulur.
- Sıfır değerli iç özelliklerini göz ardı edilir.

### <a name="shared-vs-action-dependent-features"></a>Paylaşılan eylem bağımlı Özellikler

Bazı özellikler tüm karar bakın ve tüm eylemler için aynıdır. Bunları diyoruz *Paylaşılan Özellikler*. Bazı diğer özellikler için belirli bir eylem özgüdür. Bunları diyoruz *eylem bağımlı Özellikler* (ADFs).

Çalışan örnekte, kullanıcı ve/veya world durumunu paylaşılan özellikler açıklanmıştır. Coğrafi konuma, geçerlilik süresi ve kullanıcının cinsiyeti ve şu anda hangi ana olayların gerçekleştiği gibi özellikler. Eylem bağımlı özellikler tarafından bu makalede ele alınan konulardan gibi belirli bir makaleye özelliklerini açıklayan.

### <a name="interacting-features"></a>Etkileşen özellikleri

Özellikleri genellikle "etkileşim": biri etkisini bazılarında bağlıdır. Örneğin, X kullanıcı Spor içinde olup olmadığını ilgilendiği özelliğidir. Y belirli bir makaleye hakkında Spor olup özelliğidir. Daha sonra özelliğin etkisi, Y özelliği X yüksek oranda bağlıdır.

X ve Y özellikleri arasındaki etkileşim için hesap için oluşturma bir *ikinci derece* özellik değeri olan X\*Y. (Ayrıca, "çapraz" dediğimiz X ve Y.) Hangi özellikleri çiftlerini çapraz seçebilirsiniz.

> [!TIP]
> Paylaşılan bir özelliği, kendi derece etkilemek için eylem bağımlı özelliklerle çapraz. Bir eylem bağımlı özelliği, kişiselleştirme için katkıda için paylaşılan özelliklerle çapraz.

Diğer bir deyişle, tüm ADFs ile çapraz olmayan paylaşılan bir özellik aynı şekilde her eylem etkiler. ADF ile herhangi bir paylaşılan özellik çapraz değil, her bir kararı çok etkiler. Bu tür özelliklerin ödül tahminleri varyansını azaltabilir.

### <a name="implementation-via-namespaces"></a>Ad alanları aracılığıyla uygulama

Portalda "VW komut satırı" çapraz özellikleri (yanı sıra diğer featurization kavramlar) uygulayabilirsiniz. Sözdizimi dayanır [Vowpal Wabbit](http://hunch.net/~vw/) komut satırı.

Kavramı kullanımla Merkezi *ad alanı*: adlandırılmış özelliklerinin bir alt kümesi. Her bir özellik tam olarak bir ad alanına ait. Ad alanı, açıkça özel karar hizmetine sağlanan özellik değeri belirtilebilir. Bir özellik VW komut satırında başvurmak için tek yoludur.

"Paylaşılan" veya "bağımlı eylem" bir ad alanı: yalnızca paylaşılan özelliklerini oluşur ya da yalnızca aynı eylemin eylem bağımlı özellikler oluşur.

> [!TIP]
> Açıkça belirtilen ad alanlarında özellikleri sarmalamak için iyi bir uygulamadır. Aynı ad alanında ilgili özellikleri grup.

Ad alanı sağlanmazsa, bu özellik varsayılan ad alanına otomatik olarak atanır.

> [!IMPORTANT]
> Özellikler ve ad alanları eylemler arasında tutarlı olması gerekmez. Özellikle, bir ad alanı farklı eylemler için farklı özellikler olabilir. Ayrıca, belirli bir ad diğerlerinin değildir ve bazı eylemler için tanımlanabilir.

Aynı dize değerli yerel özelliğinden gelen birden çok iç özellikleri aynı ad alanına gruplandırılır. Aynı özellik adına sahip olsa bile farklı ad alanlarında bulunan iki yerel özellikler ayrı olarak kabul edilir.

> [!IMPORTANT]
> Uzun ve açıklayıcı bir ad alanı kimlikleri ortak olsa da, VW komut satırı kimliğine aynı harfle başlayan ad alanları arasında ayrım yapmaz. Hangi aşağıdaki, ad alanı tek harfler gibi ID'lerini `x` ve `y`.

Uygulama Ayrıntıları aşağıdaki gibidir:

- Ad alanları geçilecek `x` ve `y`, yazma `-q xy` veya `--quadratic xy`. Her özellik sonra `x` her özelliğiyle çapraz `y`. Kullanım `-q x:` geçilecek `x` her ad alanı ve `-q ::` tüm ad alanlarını çiftlerini geçilecek.

- Ad alanındaki tüm özellikleri yoksaymak için `x`, yazma `--ignore x`.

Ad alanları tanımlanan her bu komutları ayrı ayrı her eylem uygulanır.

### <a name="estimated-average-as-a-feature"></a>Tahmini ortalama bir özellik olarak

Tüm kararları için tercih bir düşünce deney ne belirli bir eylem, ortalama ödül mu? Bu tür ortalama ödül kalitesi "Genel" Bu eylem bir ölçü olarak kullanılabilir. Tam olarak bilinmiyor zaman diğer eylemler seçtiniz yerine bazı kararlarında. Ancak, bu teknikler öğrenme öğrenmeyi tahmin edilebilir. Bu tahmin kalitesini genellikle zamanla artırır.

Bu "ortalama ödül tahmini" belirli bir eylemi için bir özellik olarak dahil etmeyi seçebilirsiniz. Yeni veri ulaştığında özel karar hizmet bu tahmin otomatik olarak güncelleştirecektir. Bu özellik adında *Marjinal özelliği* Bu eylem. Marjinal özellikleri, machine learning için ve denetim için kullanılabilir.

Marjinal özellikleri eklemek için yazma `--marginal <namespace>` VW komut satırında. Tanımlamak `<namespace>` şekilde JSON içinde:

```json
{<namespace>: {"mf_name":1 "action_id":1}
```

Belirli bir eylemi diğer eylem bağımlı özelliklerinin yanı sıra bu ad alanı ekleyin. Aynı kullanarak her bir kararı için bu bir tanımını sağlamak `mf_name` ve `action_id` tüm kararları için.

Marjinal özelliği her bir eylemde için eklenen `<namespace>`. `action_id` Eylemi benzersiz olarak tanımlayan herhangi bir özellik adı olabilir. Özellik adı ayarlamak `mf_name`. Özellikle, margınal farklı özellikleri `mf_name` farklı özellikler davranılır — her biri için farklı bir ağırlık öğrenilen `mf_name`.

Olan varsayılan kullanım `mf_name` tüm eylemler için aynıdır. Ardından bir ağırlık Marjinal tüm özellikler için öğrenilen.

Birden çok Marjinal özellikleri aynı eylemi için aynı değerlere ancak farklı özellik adları ile de belirtebilirsiniz.

```json
{<namespace>: {"mf_name1":1 "action_id":1 "mf_name2":1 "action_id":1}}
```

### <a name="1-hot-encoding"></a>1 hot kodlama

Burada her bit olası değerler aralığına karşılık gelen bit vektörüne olarak bazı özellikleri göstermek seçebilirsiniz. Bu özellik bu aralıkta arasındadır ve yalnızca, bu biti 1 olarak ayarlandı. Bu nedenle, 1 olarak ayarlanmış bir "etkin" bit yoktur ve diğerleri 0 olarak ayarlayın. Bu gösterim yaygın olarak bilinen *1 hot kodlama*.

1 hot kodlama kendiliğinden anlamlı bir sayısal gösterim olmayan kategorik özellikleri için "coğrafi bölge" gibi genel bir durumdur. Ödül olan etkileyebileceği büyük olasılıkla doğrusal olmayan sayısal özellikler için tavsiye edilir. Örneğin, belirli bir makaleye ilgili belirli bir yaş gruba ve ilgisiz herkes daha eski veya küçük olabilir.

Herhangi bir dize değerli özellik varsayılan olarak 1 hot kodlanır: ayrı bir iç özelliği için olası her değer oluşturulur. Otomatik 1 hot sayısal özellikleri ve/veya özelleştirilmiş aralıklarıyla kodlama şu anda sağlanmadı.

> [!TIP]
> Machine learning algoritmaları belirli bir iç özelliği, tüm olası değerler tek bir yolla kabul: aracılığıyla ortak bir "ağırlığı." 1 hot kodlama her değer aralığı için ayrı "ağırlık" sağlar. Yeterli verileri toplanır, ancak daha iyi ödül yakınsaması için gereken veri miktarı artırabilir sonra aralıkları küçülterek daha iyi ödül yol açar.

## <a name="feature-specification-format-and-apis"></a>Özellik belirtimi: biçimi ve API'leri

Bulunabilecek API'leri aracılığıyla özellikleri belirtebilirsiniz. Tüm API'leri ortak bir JSON biçimini kullanın. API'ler ve kavramsal bir düzeyde biçimi aşağıda verilmiştir. Belirtimi Swagger şeması tarafından tamamlanır.

Özellik belirtimi için temel JSON şablonunu aşağıdaki gibidir:

```json
{
"<name>":<value>, "<name>":<value>, ... ,
"namespace1": {"<name>":<value>, ... },
"namespace2": {"<name>":<value>, ... },
...
}
```

Burada `<name>` ve `<value>` özellik adı ve özellik değeri için sırasıyla bekleyin. `<value>` bir dize, tamsayı, float, bir Boole değeri veya dizi olabilir. Bir ad alanına Sarmalanan olmayan bir özellik, varsayılan ad alanına otomatik olarak atanır.

Dize paketi-in-sözcükler olarak göstermek için özel bir sözdizimi kullanın `"_text":"string"` yerine `"<name>":<value>`. Etkili bir şekilde ayrı bir iç özellik dizesindeki her sözcüğün için oluşturulur. Değeri bu word oluşumları sayısıdır.

Varsa `<name>` "_" ile başlar (ve `"_text"`), bu özellik yoksayılır.

> [!TIP]
> Bazen birden fazla JSON kaynaktan özellikleri birleştirin. Kolaylık sağlamak için bunları aşağıdaki gibi gösterebilir:
>
> ```json
> {
> "source1":<features>,
> "source2":<features>,
> ...
> }
> ```

Burada `<features>` önceden tanımlı temel özellik belirtimi başvuruyor. Daha derin "iç içe geçme" düzeyde çok kullanılabilir. Özel karar hizmeti otomatik olarak yorumlanan "derin" JSON nesnelerini bulur `<features>`.

#### <a name="feature-set-api"></a>Özellik kümesi API

Özellik API kümesini daha önce açıklanan JSON biçiminde özelliklerin listesini döndürür. Birkaç özellik kümesi API uç noktaları kullanabilirsiniz. Her uç nokta özellik kümesi kimliği ve bir URL tarafından tanımlanır. Özellik arasında eşleme kimlikleri ve URL'leri portalda ayarlayın.

Özellik API kümesini JSON uygun yerinde karşılık gelen özellik kümesi kimliği ekleyerek çağırın. Eylem bağımlı özellikler için çağrı otomatik olarak eylem kimliğine göre parametreli. Birkaç belirtebilirsiniz özelliği için aynı eylemi kimlikleri ayarlayın.

#### <a name="action-set-api-json-version"></a>Eylem API kümesini (JSON sürüm)

Eylem API kümesini eylemleri ve özelliklerini JSON'de belirtilir bir sürüme sahip. Özellikler, açıkça ve/veya özellik kümesi API'leri aracılığıyla belirtilebilir. Paylaşılan Özellikler tüm eylemler için bir kez belirtilebilir.

#### <a name="ranking-api-http-post-call"></a>API (HTTP POST çağrısı) sıralaması

API sıralaması HTTP POST çağrısına kullanan bir sürüme sahip. Bu çağrı gövdesi, Eylemler ve esnek bir JSON söz dizimi aracılığıyla özellikleri belirtir.

Eylemler açıkça belirtilen ve/veya eylem kimliklerini ayarlayın. Bir eylem kümesi kimliği karşılaşıldığında, karşılık gelen eylemi ayarlamak API uç noktası çağrı yürütülür.

Eylem ayarlama API için olduğu gibi açıkça ve/veya özellik kümesi API'leri aracılığıyla özellikler belirtilebilir. Paylaşılan Özellikler tüm eylemler için bir kez belirtilebilir.