---
title: Senaryo değerlendirme - Personalizer
titleSuffix: Azure Cognitive Services
description: Personalizer doğru öğesi, eylem veya görüntülenecek - deneyimini iyileştirin, daha iyi iş sonuçları elde etmek veya verimliliği artırmak için ürün, uygulamanızın nerede seçebilirsiniz herhangi bir durumda uygulanabilir.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: e51ef9afd0e49b690a4f9cab09fdfbd3e86eee66
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65441044"
---
# <a name="where-can-you-use-personalizer"></a>Kişiselleştirme’yi nerelerde kullanabilirsiniz?

Kullanım Personalizer doğru öğesi, eylem veya - deneyimini iyileştirin için görüntülenecek bir ürün seçmek için uygulamanız gereken yere herhangi bir durumda, daha iyi iş sonuçları elde etmek veya üretkenliği artırın. 

Personalizer kullanıcıya göstermek için hangi eylemin seçmek için makine öğrenimini kullanıyor. Seçimi miktarı, kalitesini ve dağıtımını Hizmeti'ne gönderilen veri bağlı olarak önemli ölçüde farklılık gösterebilir.

### <a name="checklist-for-applying-personalizer"></a>Personalizer uygulamak için Denetim listesi


Durumlarda Personalizer uygulayabilirsiniz burada:

* Uygulamanız için bir işletmeyi ya da kullanılabilirlik hedef vardır.
* Uygulamanızda ne göstermek bağlamsal bir karar kullanıcılara sağlama bu hedefe burada artıracak bir yer var.
* En iyi seçim yapabilir ve toplu kullanıcı davranışı ve toplam ödül puan öğrendiniz.
* Kişiselleştirme için makine öğrenimi kullanımını izleyen [sorumlu kullanım yönergeleri](ethics-responsible-use.md) ve seçtiğiniz Seçenekler.
* Bağlamsal bir karar seçenekleri sınırlı kümesinden en iyi seçenek (işlem) sıralaması olarak ifade edilebilir.
* Uygulamanız için çalışan dereceli seçimi kullanıcı davranışı bazı yönlerini ölçüm ve içinde bir ifade tarafından ne kadar iyi belirlenebilir bir _puanı ödüllendirin_. Bu, -1 ile 1 arasında bir sayı olabilir.
* Ödül puan çok fazla karışıklık veya dış etkenler getirmek değil. Deneme süresi hala ilgili olsa ödül puan hesaplanabilir kadar düşüktür.
* Boyut bağlamı en az 5 listesi olarak express [özellikleri](concepts-features.md) olduğunu düşündüğünüz doğru seçim yapmanıza yardımcı ve bu kişisel bilgi içermez. (PII).
* Sahip olduğunuz her içerik seçeneği hakkında bilgi _eylem_, en az 5 listesi olarak [özellikleri](concepts-features.md) Yardım Personalizer yapacak doğru seçim düşünün.
* Uygulamanız için uzun veri tutabilirsiniz en az 100.000 etkileşimleri geçmişini ulaşıncaya kadar yeterli.

## <a name="machine-learning-considerations-for-applying-personalizer"></a>Machine learning Personalizer uygulamak için dikkat edilmesi gerekenler

Personalizer öğrenme, diğer bir deyişle, machine Learning bir yaklaşım, size geri bildirim tarafından verilen pekiştirmeye üzerinde temel alır. 

Personalizer bilgi edinin durumlarda en iyi yeri:

* Sorun (örneğin, haber veya biçimde tercihlerinde) zaman içinde drifts varsa üzerine en iyi kişiselleştirme kalmak için yeterli olayları yoktur. Gerçek dünyada sürekli değişikliği personalizer Uyarlanır ancak sonuçları yok ise yeterli olayları ve verileri yeni eğilimlere keşfetmek uzmanlardan en uygun olmayacaktır. Sıklıkta gerçekleşen bir kullanım örneği seçmeniz gerekir. En az 500 kereden günde gerçekleşen kullanım örnekleri aranıyor göz önünde bulundurun.
* Bağlam ve eylemleri olan yeterli [özellikleri](concepts-features.md) öğrenme süreçlerini kolaylaştırmasına için.
* Çağrı başına derece için 50'den az eylemler vardır.
* Veri saklama ayarlarınıza Personalizer çevrimdışı değerlendirmeleri ve ilke iyileştirme gerçekleştirmek için yeterli veri toplamaya izin verin. 50. 000'en az bir veri noktaları genellikle budur.

## <a name="monitor-effectiveness-of-personalizer"></a>Personalizer etkinliğini izleme

Gerçekleştirerek Personalizer etkisini düzenli aralıklarla izleyebilirsiniz [çevrimdışı değerlendirmeleri](concepts-offline-evaluation.md).

## <a name="use-personalizer-with-recommendation-engines"></a>Öneri altyapıları ile Personalizer kullanın

Birçok şirket, müşterilerin büyük bir katalog ürünlerden önermek için öneri altyapıları, pazarlama ve campaigning araçları, İzleyici segmentlere ayırma ve kümeleme, ortak filtreleme ve başka bir yolla kullanın.

[Microsoft Recommenders GitHub deposundan](https://github.com/Microsoft/Recommenders) önerisi sistemleri, Jupyter not defterleri sağlanan oluşturmaya yönelik örnekler ve en iyi yöntemler sağlar. Veri hazırlama, modelleri oluşturmaya, değerlendirme, ayarlama ve xDeepFM, dahil olmak üzere birçok ortak yaklaşım ÖİB, ALS, RBM, DKN için öneri altyapıları faaliyete geçirmeye yönelik çalışma örnekleri sağlar.

Mevcut olduğunda personalizer bir öneri altyapısı ile çalışabilirsiniz.

* Öneri altyapıları, büyük miktarlarda öğeleri (örneğin, 500.000) alın ve yüzlerce veya binlerce seçeneğe (örneğin, ilk 20) bir alt önerilir.
* Personalizer, bunlarla ilgili bilgileri çok sayıda eylemlerle az sayıda alır ve çoğu öneri altyapıları yalnızca kullanıcılar, ürünleri ve bunların etkileşimleri hakkında birkaç öznitelik kullanırken gerçek zamanlı olarak verilen zengin bir bağlam için bunları derecelendirir.
* Burada içerik değişmesine hızlı bir şekilde, haber gibi canlı olayları daha iyi sonuçlar verecek her zaman kullanıcı tercihlerini otonom olarak keşfetmek için tasarlanan personalizer Canlı topluluk içeriği, içerik günlük güncelleştirmeleriyle veya dönemsel içeriği.

Yaygın bir öneri altyapısı (örneğin, belirli bir müşteri için ilk 20 ürünü) çıktısını alın ve bu, Personalizer için giriş eylem olarak kullanmaktır.

## <a name="next-steps"></a>Sonraki adımlar

[Etik & sorumlu kullanım](ethics-responsible-use.md).