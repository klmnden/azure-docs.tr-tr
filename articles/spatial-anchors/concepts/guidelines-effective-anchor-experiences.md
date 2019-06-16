---
title: Azure uzamsal bağlayıcılarını kullanın etkin bağlantı deneyimleri için yönergeler | Microsoft Docs
description: Yönergeler ve konuları oluşturun ve Azure uzamsal bağlayıcılarını kullanarak yer işaretleri etkili bir şekilde bulun.
author: mattwojo
manager: jken
services: azure-spatial-anchors
ms.author: mattwoj
ms.date: 02/24/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 9e77dcd96ffa0fbd57aa0ed1b4f857279ca768a7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60566046"
---
# <a name="create-an-effective-anchor-experience-by-using-azure-spatial-anchors"></a>Azure uzamsal bağlayıcılarını kullanarak bir etkin bağlantı deneyimi oluşturun

Bu makalede yönergeleri sağlar ve etkili bir şekilde yardımcı olacak hususlar oluşturup uzamsal bağlayıcılarını kullanarak yer işaretleri bulun.

## <a name="good-anchors"></a>İyi yer işaretleri

Uzamsal bağlayıcılarını iyi bağlayıcılarını oluşturmanıza yardımcı olur. Bilgilendirme ya da kılavuzluk kullanıcılar, kullanıcı deneyimi (UX) iyi yer işaretleri oluşturmanız için zaman ayırmanız önemlidir. Önden iyi bağlayıcılarını oluştururken yatırım tarafından bağlayıcılarını güvenilir bir şekilde bulmak için son kullanıcıların Yardım:

- Farklı cihazlar arasında.
- Çeşitli zamanlarda.
- Farklı ışık koşullarında.
- İstenen perspektiften alanı içinde.

## <a name="static-and-dynamic-locations"></a>Statik ve dinamik konumları

Bağlantı deneyimi tasarlamanın bölümü, konumları seçmektir. Konumları, statik ve tanımlanmış bir yönetici alanı olacak mı? Veya dinamik ve kullanıcı tarafından tanımlanan olur?

Bir perakende mağaza yöneticisi, kullanıcıların ziyaret etmesi ikna statik bir mağaza içi deneyimi isteyebilirsiniz. Ancak, bir karma gerçeklik masa oyunu geliştiricisi büyük olasılıkla yürütmek hedef konumu seçin kullanıcıların istersiniz.

Statik konumları için iyi bağlayıcılarını alanıyla puanlamalar vakit geçirmeyi Yöneticiler öğretebiliriz.

Dinamik konumları için nasıl öğrenebilir veya iyi yer işaretleri oluşturmanız için UX ile Kullanıcı Kılavuzu hakkında almalısınız.

## <a name="stable-visual-features"></a>Kararlı görsel özellikler

Karma gerçeklik üzerinde Visual izleme sistemlerinin kullanılacağını ve genişletilmiş gerçeklik cihazları ortamın görsel özelliklerini kullanır. En güvenilir deneyimi almak için:  

- *Yapmak* kararlı görsel özellikler (diğer bir deyişle, sıklıkla değişmeyen Özellikler) konumlarda yer işaretleri oluşturmanız.

- *Yoksa* yok edici özelliklere sahip boş yüzeyler üzerinde yer işaretleri oluşturmanız.

- *Yoksa* yüksek oranda yansıtıcı malzeme yer işaretleri oluşturmanız.

- *Yoksa* bağlantıları burada deseni yinelenen carpet veya duvar kağıdı gibi yüzeyleri oluşturmak.

![Yer işaretleri için iyi bir ortam ve yer işaretleri için hatalı bir ortamı örnekleri](./media/stable-visual.png)

## <a name="various-viewing-perspectives"></a>Çeşitli görüntüleme Perspektifleri

Bir bağlantı oluştururken, bağlayıcı bulmak için daha sonra deneyecek kişiler hakkında düşünün.

, Örneğin, bir yer işaretine sahip iki kapılar oda ortasında göz önünde bulundurun. Büyük olasılıkla ya da kapı odaya girmek kullanıcılara izin vermek istediğiniz. Bağlantı oluştururken, her iki açılan kapılar konumundan tarama gerekecektir. Kullanıcılar ya da kapı bağlayıcısından bulabilmesi için yer işareti etrafında ortam verilerini yakalamak için Perspektifler değiştirin.

Genel olarak, bir bağlantı oluştururken bulması dener kişilerin perspektiften tarama. Sanal içerik üzerinde dış bir mimariyle yerleştiriyorsanız, bu nedenle bağlantı oluştururken, tarama sırasında mimariyle geçici yol mantıklıdır. Oda üst köşesinde bulunan, yer işareti ise buradan yaklaşım yalnızca tek yönlü yoktur. Bu bağlantı oluştururken, yalnızca bu açısından tarayabilirsiniz.

## <a name="multiple-anchors"></a>Birden çok tutturucular

Aydınlatma uygulama algılar görsel özellikleri bir fark yaratabilir. Güçlü doğal ışığı oluşturulan bağlayıcılarını yapay açık ve tam tersi bulmak zor olabilir.  

Bu sorunu varsa, iki yer işaretleri oluşturmanız yardımcı olabilir. Aynı bölgeden yapay ışığı gün ışığından yararlanma ve başka bir bağlantı oluşturun. Uygulamanızı, ardından her iki yer işaretleri için sorgulayabilirsiniz. Her iki bağlantı bulunduğunda, uygulama bağlayıcı için bir poz olacaktır. 

Benzer şekilde, çoğu nesneyi taşımak için burada görsel özellikleri değiştirin ortamlarda birden çok bağlayıcılarını yardımcı olabilir. Bir bağlayıcı ortamındaki önemli değişiklikler nedeniyle bulmak çok zor hale geldiğinde, yeni bir bağlantı değiştirebilirsiniz. Örneğin, burada düzenini birkaç her ay yenilenir bir perakende satış mağazasındaki içinde bunu.

## <a name="targets-and-rooms"></a>Hedefleri ve odaları

Çoğu durumda, bir yer işareti, uygulamanızın deneyimi için bir giriş noktasıdır. Bu adım adım ilerleyin hızlıca isteyebilirsiniz ve güvenilir bir şekilde böylece kullanıcılar deneyiminizi girebilirsiniz. Kullanıcılar, yer işaretleri nasıl bulabilirsiniz harcadığınız zamanı bir önemli tasarım adımdır. İki geniş senaryolar açısından bağlayıcılarını bulma hakkında düşünmek faydalıdır: *hedefleri* ve *odaları*.

### <a name="targets"></a>Hedefler

Hedef senaryoda, bir yer işareti bilinen konumudur. Örneğin, bir kurgusal karma gerçeklik boyama uygulamasında, bir kullanıcı sanal tuval nu duvara yansıtabilir yerleştirir. Filiz, cihazlarını bağlantı bulup deneyiminize başlayın duvarında aynı yerde işaret edecek şekilde diğer kullanıcılar odadaki bildirir.  

Başka bir senaryoya örnek olarak bir hedef bir oturum açma, "Anlaşmaları tarayın." okuyan bir kahve Dükkanı olabilir Kahve Dükkanı Buraya bir yer işareti yerleştirdi. Kullanıcılar oturum tarama gibi bağlantı bulun ve anlaşmalar hakkında daha fazla kahve bulmak için genişletilmiş gerçeklik deneyimini girin.

Hedef senaryosunda, fotoğraflar yardımcı olabilir. Kullanıcıların cihazlarında hedef fotoğrafını gösterirse, bunlar hızlı bir şekilde gerçek dünyada tarama gerekenler tanımlayabilirsiniz. Örneğin, kullanıcılarınız GPS'ini kullanarak genel bir hedef alanı içinde geldiğinde yardımcı olabilir. Kullanıcı geldiğinde, uygulamanızı hedef bir fotoğraf gösterir. Kullanıcı boşluk görünüyor, hedef bulur ve bağlayıcısını tarar.

![Bir kullanıcının mobil cihazında hedef fotoğrafını gösteren bir yer işareti gösterimi](./media/start-here-edit.png)

### <a name="rooms"></a>odaları

Oda senaryoda, kullanıcılar yalnızca var olan Buraya bir yer işareti yere bilmek bir boşluk girin. Kullanıcı, cihazını alanı tarama ve hızlı bir şekilde bağlantı bulun.

Bu deneyim genellikle iyi seçkin yer işaretleri oluşturmanız size çeşitli görüntüleme açılardan açıklandığı gibi gerektirir. Yer işareti oluşturduğunuzda, birden çok perspektif odasından taradığınız ise bulmak çalıştıklarında kullanıcıların neredeyse her yerden tarayabilirsiniz.

![Bir kullanıcı bir bağlayıcı bulmayı oda nasıl tarayabilir çizimi](./media/scan-room.png)

Aslında, daha sonra kullanıcıların tarama ve yer işaretine hızlıca bulun, yer işareti oluşturduğunuzda alanı tarama daha fazla zaman ayırıyor. Deneyiminizi oluşturmak gibi bu önemli dengelemeyi göz önünde bulundurmanız gerekir.

Daha önce ele almıştık karma gerçeklik boyama uygulaması örneği, iyi bir oda senaryosu olarak çalışmaz. Burada tutturucusu yerleştirir kullanıcının diğerlerinin deneyimini hızla katılmak istiyor. Kullanıcıların deneyimini oda iyi taranıncaya kadar başlatmak için beklenecek istemezsiniz. Bu örnek, tüm kullanıcıların cihazlarını bağlantıları bulmak için işaret edecek şekilde tam olarak nerede bildiğiniz olduğundan, bir hedef senaryo olarak daha iyi çalışır.

## <a name="anchor-location"></a>Bağlantı konumu

Görsel izleme sistemleri bir ortamdaki görsel özellikleri kullanır. Bir tarama içeren daha görsel özellikleri, bir yer işareti bulma olasılığı daha.

Ortamın kullanışlı bir tarama teşvik eden bir UX oluşturmak için bu bölümdeki yönergeleri izleyin.

İlk olarak, kullanıcı, birkaç saniye içinde bir yer işareti yerini değil, uygulama daha fazla perspektif yakalamak için cihaz taşımak için kullanıcıların teşvik. Uygulamayı, ayrıca kullanıcıların kendilerini Perspektifler daha fazla bağlayıcısından taramak için ortamı etrafında taşımak için teşvik edebilir. Cihaz gördüğü daha fazla özellik Perspektifler, daha iyi.

Hedef senaryoları, farklı perspektiften görüntülemek için hedef taşımak için kullanıcı isteyin. Diğer bir deyişle, bağlayıcı bulunana kadar yeni açılardan hedef yakalamak için kullanıcıya sor.

Oda senaryoları için yavaş oda tarama kullanıcıdan. Örneğin, 180 derece veya hatta 360 derece yer yakalamak için açmak için kullanıcıya sor. Veya yeni bir perspektif odasından görüntülemesini isteyin. 

En anlamlı arasında yer tarama yöntemdir. Odanın bir tarama, yakındaki bir duvar taranmasını ortamın daha fazla görsel özelliği örneğin yakalar. Yakındaki bir duvar taranmasını ortamın visual gibi birçok yararlı özellik yakalama olmaz.

Sürekli cihaz için bir yer işareti ararken yan yana taşımak yararlı değildir. Bu, yalnızca aynı açısından aynı noktalarını yakalar.

## <a name="experience-tests"></a>Testleri deneyimi

Bu makalede, genel yönergeleri ele almıştık. Uzamsal Çıpasıyla gerçek dünyayla etkileşimde bulunan uygulamaları yazıyorsanız. Bu nedenle, zaman, uygulamanızın bağlantı senaryoları gerçek ortamlarda test etme için ayırmak. Bu uygulamayı kullanmak için kullanıcılarınızın nerede beklediğiniz temsil eden ortamlar için özellikle doğrudur.
