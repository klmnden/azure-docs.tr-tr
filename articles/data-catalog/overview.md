---
title: Azure veri Kataloğu'na giriş
description: Bu makalede, Microsoft Azure Veri Kataloğu özelliklerinin ve giderdiği sorunların yanı sıra bu hizmete genel bir bakış sunulmaktadır. Veri Kataloğu, herhangi bir kullanıcının veri kaynaklarına kaydolmasına; bunları bulmasına, anlamasına ve tüketmesine imkan sağlar.
author: markingmyname
ms.author: maghan
ms.service: data-catalog
ms.topic: overview
ms.date: 04/05/2019
ms.openlocfilehash: cd20fc6ae71a0dd96a0006de8c81050bb0646905
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59287437"
---
# <a name="what-is-azure-data-catalog"></a>Azure Veri Kataloğu nedir?

Azure veri Kataloğu, bir tam olarak yönetilen bir bulut hizmetidir. Veri bulma olanağı bunlar gerekir ve veri kaynaklarını anlama kaynakları bulun. Bunun yanı sıra, Veri Kataloğu kuruluşların mevcut yatırımlarından daha fazla değer elde etmesine yardımcı olur.

Veri Kataloğu sayesinde, tüm kullanıcılar (analist, veri bilim insanı veya geliştirici) veri kaynaklarını bulabilir, anlayabilir ve tüketebilir. Veri Kataloğu, meta veriler ve ek açıklamalar için bir kitle kaynağı modelini içerir. Bir kuruluştaki tüm kullanıcıların bilgileriyle katkıda bulunması ve veri odaklı bir topluluk ve kültür oluşturması için tek, merkezi bir yerdir.

## <a name="discovery-challenges-for-data-consumers"></a>Veri tüketicileri için bulma zorlukları

Geleneksel olarak, kurumsal veri kaynaklarının bulunması grupsal bilgilere dayanan organik bir süreç olmuştur. Bu yaklaşım, bilgi varlıklarından en yüksek değeri elde etmek isteyen şirketler için çeşitli zorluklar teşkil etmektedir:

* Kullanıcılar, başka bir işlemin parçası olarak into contact with geldikleri sürece bir veri kaynağının mevcut etmeyebilirsiniz. Veri kaynaklarının kayıtlı olduğu merkezi bir konum yoktur.
* Kullanıcılar bir veri kaynağının konumunu bilmediği sürece bir istemci uygulaması kullanarak verilere bağlanamaz. Veri tüketimi deneyimleri, kullanıcıların bağlantı dizesini veya yolu bilmesini gerektirir.
* Kullanıcılar bir veri kaynağına ait belgelerin yerini bilmediği sürece bu verilerin hangi amaca yönelik olduğunu anlayamaz. Veri kaynakları ve belgeler çeşitli yerlerde bulunabilir ve çeşitli deneyimler aracılığıyla tüketilebilir.
* Kullanıcıların bir bilgi varlığıyla ilgili soruları varsa, verilerden sorumlu olan uzmanı veya ekibi bulması ve bunlarla çevrimdışı etkileşim kurması gerekir. Veri ve Perspektifler, kullanımda olan uzmanlar arasında açık bir bağlantı yoktur.
* Kullanıcılar veri kaynağına erişim isteme işlemini anlamadığı sürece, veri kaynağının ve belgelerin bulunması bile verilere erişime yardımcı olmaz.

## <a name="discovery-challenges-for-data-producers"></a>Veri üreticileri için bulma zorlukları

Veri tüketicileri daha önce sözü geçen sorunlarla karşılaşırken, bilgi varlıkları oluşturmaktan ve bunların bakımını yapmaktan sorumlu kullanıcılar da kendilerine özgü zorluklarla yüz yüze gelmektedir:

* Veri kaynaklarına açıklayıcı meta verilerle ek açıklama ekleme, genellikle sonuç vermeyen bir işlemdir. İstemci uygulamaları genellikle veri kaynağında depolanan açıklamaları yok sayar.
* Veri kaynakları için belge oluşturma genellikle sonuç vermeyen bir işlemdir. Belgeleri veri kaynaklarıyla eşitlenmiş kalmasını devam eden bir sorumluluğundadır. Kullanıcılar eski olduğu eski olduğunu belgelere güvenmez.
* Veri kaynaklarına yönelik belgelerin oluşturulması ve bakımının yapılması karmaşık ve zaman alan işlemlerdir. Bu belgelerin, veri kaynağını kullanan herkesin erişimine hazır hale getirilmesi ise daha da zordur.
* Veri kaynaklarına erişimin kısıtlanması ve veri tüketicilerinin nasıl erişim isteneceğini bilmesinin sağlanması sürekli karşılaşılan bir zorluktur.

Bu zorluklar bir araya geldiğinde, kurumsal verilerin kullanımını ve anlaşılmasını teşvik etmek ve desteklemek isteyen şirketler için ciddi bir engel oluşturmaktadır.

## <a name="azure-data-catalog-can-help"></a>Azure Veri Kataloğu bu konuda yardımcı olabilir

Veri Kataloğu, bu sorunları gidermek ve kuruluşların mevcut bilgi varlıklarından en yüksek değeri elde etmesine yardımcı olmak üzere tasarlanmıştır. Veri Kataloğu, veri kaynaklarının verileri yöneten kullanıcılar tarafından kolayca bulunabilmesini ve anlaşılır olmasını sağlar.

Veri Kataloğu, veri kaynağının kaydedilebileceği bulut tabanlı bir hizmet sağlar. Veriler mevcut konumunda kalırken, bunların meta verileri ve veri kaynağı konumuna yönelik bir başvuru Veri Kataloğu'na eklenir. Bu meta veriler ayrıca her bir veri kaynağının arama ile kolayca bulunabilmesini ve bunları bulan kullanıcılar tarafından anlaşılabilmesini sağlamak üzere dizine alınır.

Bir veri kaynağı kaydedildikten sonra meta verilerini ardından zenginleştirilebilir. Meta veriler, kayıtlı kullanıcı veya kuruluştaki diğer kullanıcılar tarafından eklenebilir. Herhangi bir kullanıcı, açıklama, etiket veya veri kaynağı erişimi istemeye yönelik belge ve işlemler gibi diğer meta verileri sağlayarak bir veri kaynağına açıklama ekleyebilir. Bu tanımlayıcı meta veriler, veri kaynağından kaydedilen yapısal meta verilere (sütun adları ve veri türleri gibi) ek niteliğindedir.

Veri kaynaklarını ve bunların kullanımını bulup anlamak, kaynakların kaydedilmesindeki birincil amaçtır. Kurumsal kullanıcıların iş zekası, uygulama geliştirme, veri bilimi veya doğru verilerin gerektiği başka herhangi bir görevde verilere ihtiyacı olabilir. Bu kullanıcılar, ihtiyaçlarıyla eşleşen verileri hızla bulmak, verileri anlayarak bunların amaca uygunluğunu değerlendirmek ve veri kaynağını kendi tercih ettikleri araçta açarak verileri tüketmek için Veri Kataloğu’nun bulma deneyimini kullanabilir. 

Diğer yandan, kullanıcılar zaten kaydettikleri veri kaynakları için etiketler, belgeler ve ek açıklamalar ekleyerek kataloğa katkıda bulunabilir. Bu kullanıcılar tarafından yeni veri kaynakları da kaydedilebilir ve bu kaynaklar katalog kullanıcıları topluluğu tarafından bulunabilir, anlaşılabilir ve tüketilebilir.

![Veri Kataloğu özellikleri](./media/data-catalog-what-is-data-catalog/data-catalog-capabilities.png)

## <a name="learn-more-about-data-catalog"></a>Veri Kataloğu hakkında daha fazla bilgi edinin

Veri Kataloğu'nun özellikleri hakkında daha fazla bilgi edinmek için bkz.

* [Veri kaynaklarını kaydetme](data-catalog-how-to-register.md)
* [Veri kaynaklarını bulma](data-catalog-how-to-discover.md)
* [Veri kaynaklarına açıklama ekleme](data-catalog-how-to-annotate.md)
* [Veri kaynaklarını belgeleme](data-catalog-how-to-documentation.md)
* [Veri kaynaklarına bağlanma](data-catalog-how-to-connect.md)
* [Büyük verilerle çalışma](data-catalog-how-to-big-data.md)
* [Veri varlıklarını yönetme](data-catalog-how-to-manage.md)
* [İş Sözlüğünü ayarlama](data-catalog-how-to-business-glossary.md)
* [Sık sorulan sorular](data-catalog-frequently-asked-questions.md)

## <a name="next-steps"></a>Sonraki adımlar

Veri Kataloğu ile çalışmaya başlamak için:

* [Hızlı Başlangıç: Bir Azure veri Kataloğu oluşturma](data-catalog-get-started.md)
* [Azure veri kataloğunuz açın](https://www.azuredatacatalog.com)