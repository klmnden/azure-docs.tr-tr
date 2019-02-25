---
title: Etkin bağlantı deneyimlerinde Azure uzamsal yer işaretleri için yönergeler | Microsoft Docs
description: Yönergeler ve konuları oluşturma ve etkili bir şekilde Azure uzamsal yer işaretleri ile bağlayıcılarını bulma.
author: mattwojo
manager: jken
services: azure-spatial-anchors
ms.author: mattwoj
ms.date: 02/24/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 196958e4818251bd7f2ee78ca472e6f28292d908
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2019
ms.locfileid: "56753268"
---
# <a name="guidelines-for-an-effective-anchor-experience-with-azure-spatial-anchors"></a>Azure uzamsal bağlayıcılarını bir etkin bağlantı deneyimi için yönergeler

Bu makalede, yönergeleri ve oluşturma ve etkili bir şekilde Azure uzamsal yer işaretleri ile bağlayıcılarını bulma ile ilgili konuları sağlar.

## <a name="creating-anchors"></a>Bağlayıcıları oluşturuluyor

İyi bağlantıları oluşturma Azure uzamsal Çıpasıyla çalışmanın en önemli özelliklerinden biridir. Bilgilendirme veya kullanıcılar, UX iyi yer işaretleri oluşturmanız hakkında rehberlik sağlayan zaman ayırmanız önemlidir. Önden bağlantı oluşturmaya yatırım tarafından bağlayıcılarını güvenilir bir şekilde bulmak son kullanıcıların etkinleştir:

- Farklı cihazlarda
- Çeşitli zamanlarda
- Farklı ışık koşullara
- İstenen perspektiften alanı içinde
- Etc.

Önemli noktalar ve bağlantı oluşturma ve konum deneyiminizi tasarlamanıza yardımcı olacak yönergeler aşağıda verilmiştir.

## <a name="static-and-dynamic-locations"></a>Statik ve dinamik konumları

Bağlantı deneyimi tasarlama parçası katılan konumları seçme. Konumları, statik ve tanımlanmış bir yönetici alanı olacak mı? Veya dinamik ve kullanıcı tanımlı olacak?

Bir perakende mağaza yöneticisi, kullanıcıların ziyaret etmesi ikna statik bir mağaza içi deneyimi isteyebilirsiniz. Kullanıcıların yürütmek hedef konumu seçin, büyük olasılıkla bir karma gerçeklik masa oyunu Geliştirici istediği ise.

Statik konumları için iyi bağlayıcılarını alanıyla puanlamalar vakit geçirmeyi Yöneticiler eğitebilirsiniz.

Dinamik konumları için nasıl eğitmeniz veya iyi yer işaretleri oluşturmanız için UX ile Kullanıcı Kılavuzu hakkında almalısınız.

## <a name="stable-visual-features"></a>Kararlı görsel özellikler

Görsel izleme sistemleri tarafından kullanılan karma gerçeklik ve genişletilmiş gerçeklik cihazları ortamın görsel özelliklerini kullanır. En güvenilir deneyimi almak için:  

- Yer işaretleri konumlarda kararlı görsel özellikler (diğer bir deyişle, sıklıkla değişmeyen Özellikler) ile oluşturun.

- Yer işaretleri, yok edici özelliklere sahip boş yüzeyler üzerinde oluşturmayın.

- Yer işaretleri, yüksek oranda yansıtıcı malzeme oluşturmayın.

- Yer işaretleri, burada yalnızca desen, Halı duvar kağıdı gibi yinelenen veya yüzeylere üzerinde oluşturmayın.

![İyi hem kötü yanları ortamı gösteren örnek görüntüleri](./media/stable-visual.png)

## <a name="consider-various-viewing-perspectives"></a>Çeşitli görüntüleme Perspektifler göz önünde bulundurun.

Bir bağlantı oluştururken, daha sonra yer işareti bulunacak ziyaret kişileri düşünün.

, Örneğin, iki kapılar sahip bir oda ortasında bir yer işareti düşünün. Büyük olasılıkla yer girin ya da kapı bağlantı bulmak için kullanıcılar izin vermek istediğiniz. Bağlantı oluştururken, her iki açılan kapılar konumundan tarama gerekecektir. Kullanıcılar ya da kapı bağlayıcısından bulabilmesi için bu iki perspektiften bağlantı etrafında ortam verilerini yakalar.

Genel olarak, bir bağlantı oluştururken, farklı yerlerde ya da onu bulmaya çalışırken duran kişilere beklediğiniz Perspektifler taranmalıdır.  

Sanal içerik üzerinde dış bir mimariyle yerleştiriyorsanız, bağlantı oluştururken hatayla göz atabilirsiniz mimariyle geçici yol mantıklıdır.  

Öte yandan, bir oda üst köşesinde bulunan, yer işareti ise buradan yaklaşım yalnızca tek yönlü yoktur. Bağlantı oluştururken, yalnızca bu açısından taranacak gerekir.

## <a name="multiple-anchors"></a>Birden çok tutturucular

Aydınlatma algılanan görsel özellikleri bir fark yaratabilir. Güçlü doğal ışığı oluşturulan bağlayıcılarını yapay aydınlatma altında koyu sonrasında bulmak zor olabilir ve bunun tersi de geçerlidir.  

Bu sorunla karşılaşırsanız, aynı nokta iki çıpasının – bir gün ışığından yararlanma ve diğer yapay aydınlatma altında – oluşturmak için yardımcı olabilir. Uygulamanızı, ardından her iki yer işaretleri için sorgulayabilirsiniz. Ya da bulunuyorsa, uygulama bağlayıcı için bir poz olacaktır. 

Benzer şekilde, çoğu nesneyi taşımak için burada görsel özellikleri değiştirin ortamlarda birden çok bağlayıcılarını yardımcı olabilir. Bir bağlayıcı ortamındaki önemli değişiklikler nedeniyle bulmak çok zor hale geldiğinde, yenisiyle değiştirin. Bu durumda, örneğin, burada düzenini birkaç her ay yenilenir bir perakende satış mağazasındaki içinde olabilir.

## <a name="targets-and-rooms"></a>Hedefleri ve odaları

Çoğu durumda, bir yer işareti bulma, uygulamanızın benzersiz deneyimi için giriş noktasını temsil eder. Bu adım adım ilerleyin hızlıca isteyebilirsiniz ve güvenilir bir şekilde böylece kullanıcılar deneyiminizi girebilirsiniz. Kullanıcılar, yer işaretleri nasıl bulabilirsiniz harcadığınız zamanı bir önemli tasarım adımdır. Bu konuda açısından iki geniş senaryoları düşünmek faydalıdır: **Hedefleri** ve **odaları**.

### <a name="targets"></a>Hedefler

![Düzenleme buradan başlayın](./media/start-here-edit.png)

Hedef senaryoda, bir yer işareti bilinen konumudur. Örneğin, bir kurgusal MR boyama uygulamasında, bir kullanıcı sanal tuval nu duvara yansıtabilir yerleştirir. Filiz, cihazlarını bağlantı bulup deneyiminize başlayın duvarında aynı yerde işaret edecek şekilde diğer kullanıcılar odadaki bildirir.  

Başka bir senaryoya örnek olarak bir hedef "bir kafede satırında beklenirken anlaşmalar için Tara" belirten bir göstergesi olabilir. Kahve Dükkanı daha önce Buraya bir yer işareti yerleştirdi. Kullanıcılar oturum tarama gibi bağlantı bulun ve AR deneyimi üzerinde kahve anlaşmalar için girin.

Hedef senaryosunda, fotoğraflar yardımcı olabilir. Kullanıcı bir fotoğraf hedef cihazlarında gösterebilir, bunlar hızlı bir şekilde gerçek dünyada tarama gerekenler tanımlayabilirsiniz. Örneğin, GPS kullanarak bir hedef genel yakın çevre içinde geldiğinde, kullanıcılarınızın yardımcı olabilir. Kullanıcı geldikten sonra uygulamanızı hedef bir fotoğraf gösterir. Kullanıcı görünen alanın etrafındaki boşluğu hedef bulur ve bağlayıcısını tarama devam eder.

### <a name="rooms"></a>odaları

![Oda tarama](./media/scan-room.png)

Oda senaryoda, kullanıcılar yalnızca var olan Buraya bir yer işareti yere bilmek bir boşluk girin. Kullanıcılar, cihazını alanı tarama ve yer işaretine hızlıca bulun.

Genellikle bu deneyim elde gerektirir anlatıldığı gibi iyi seçkin bağlantıları oluşturma [çeşitli görüntüleme Perspektifler göz önünde bulundurun](#consider-various-viewing-perspectives) daha önce. Oda bağlantı oluştururken çok perspektiften tarandı, kullanıcıların neredeyse her yerden, onu bulmaya çalışırken tarayabilirsiniz.

Esas olarak, bağlantı oluşturan kişi hızla alanı daha sonra gelen kişiler tarayabilir ve yer işaretine bulmak için tarama daha fazla zaman harcadığını. Bu, deneyiminiz için göz önünde bulundurmanız gerekecektir önemli bir dengedir.

Daha önce açıklanan MR Boyama uygulama odası senaryosu olarak uygun olmayan bir örnektir. Burada yer işaretine yerleştirme kullanıcı deneyimini hızla katılmak için diğer istiyor. Kullanıcıların deneyimini başlatmak için iyi taranmış bir yer kadar beklemek istiyor musunuz. Tüm kullanıcıların cihazlarını bağlantıları bulmak için işaret edecek şekilde tam olarak nerede olduğunu bildiğimizden bu senaryoyu daha iyi bir hedef senaryosu olarak ele alınır.

## <a name="effectively-locating-anchors"></a>Etkili bir şekilde bağlayıcılarını bulma

Görsel izleme sistemleri işlevi için bir ortamın görsel özellikleri kullanır. Daha fazla görsel özellikleri bir tarama kapsamındaki bir yer işareti bulma olasılığı daha.

Ortamın kullanışlı bir tarama teşvik eden bir kullanıcı deneyimi oluşturmak için izlemeniz gereken bazı genel yönergeleri vardır.

İlk olarak, kullanıcı bir yer işareti birkaç saniye içinde yerini değil, uygulama kullanıcıların cihaz daha fazla perspektif yakalanan şekilde taşımak için teşvik etmelidir.  Uygulama ayrıca Perspektifler daha fazla bağlayıcısından taranırken bir ortam taşımak için kullanıcıların kendilerini teşvik edebilir. Daha fazla perspektif, aynı noktaları daha iyi cihaz görür.

Hedef senaryoları, tüm farklı perspektiften görüntülerken hedef taşımak için kullanıcı isteyin. Diğer bir deyişle, bağlayıcı bulunana kadar yeni açılardan hedef yakalamak için kendi ayak taşımak için kullanıcıya sor.

Oda senaryoları için yavaş oda tarama kullanıcıdan. Örneğin, 180 veya hatta 360 derece yer yakalamak için döndürmek için kullanıcıya sor. Veya yeni bir perspektif odadaki taşımak için kullanıcıya sor. Tarama en anlamlı arasında yer taramak için bir yöntemdir. Bu, yakındaki bir duvar tarama say ortamın daha fazla görsel özelliği yakalar. Yakındaki bir duvar tarama ortam çok yararlı görsel özelliklerini yakalamaz.

Cihaz için yan tekrar tekrar için bir yer işareti ararken taşımak yararlı değildir. Bu, yalnızca aynı açısından aynı noktalarını saptayacaktır.

## <a name="testing-the-experience"></a>Deneyimi test etme

Yukarıdaki genel yönergelerdir. Azure uzamsal Çıpasıyla gerçek dünyayla etkileşimde bulunan uygulamaları yazıyorsunuz. Bu nedenle, uygulamanızın bağlantı senaryoları gerçek ortamlarda test etme için zaman ayırmak önemlidir. Bu ortamlar için özellikle doğrudur, uygulamayı kullanmak için kullanıcılarınızın nerede beklediğiniz temsili.