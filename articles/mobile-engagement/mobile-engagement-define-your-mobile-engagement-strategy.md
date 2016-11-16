---
title: "Mobile Engagement stratejinizi tanımlama | Microsoft Belgeleri"
description: "Analizler ve anında iletme bildirimleri ile Mobile Engagement’ı eklemeyi ve iyileştirmeyi öğrenin."
services: mobile-engagement
documentationcenter: Mobile
author: piyushjo
manager: dwrede
editor: 
ms.assetid: 7533e318-81b9-4360-aace-b7be8225985b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/19/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 8cb91a8cdc6d16070034c79515731be7b820d389


---
# <a name="define-your-mobile-engagement-strategy"></a>Mobile Engagement stratejinizi tanımlama
*Uygulamanızı bir nedenle yazdınız: kullanıcılarınız kullansın diye!*

Kullanıcıların hayran kalacağı harika bir uygulama oluşturmak için çok çaba gösterdiğinize inanıyoruz. Büyük olasılıkla, kullanıcı kazanmak için ciddi bir pazarlama yatırımı da yaptınız. Ancak kullanıcı sayısının tavan yaptığı o ilk heyecan verici dönemin ardından kullanıcıların yavaş yavaş uygulamanızı kullanmayı bıraktığını görebilirsiniz. *Azure Mobile Engagement işte bu iş için burada!*: Kullanıcıların uygulamanızı kullanmaya devam etmelerini, böylece sizin de test edip öğrenme yoluyla uygulamayı sürekli geliştirmenizi sağlıyoruz.

Kullanıcıların elde tutulmasını ve kullanımı geliştirmeye yaklaşımımız, anında iletme bildirimleri ve Uygulama içi iletiler aracılığıyla uygulama kullanıcılarının katılımını sağlamaktan geçiyor. Ancak bu iletiler ve iletişim, her kullanıcının uygulamadaki davranışlarına göre kendisine özgü, çok özel bir şekilde tasarlanıyor. Amacımız, doğru hedef kitleyle, doğru zamanda ve doğru yerde iletişim kurmanızı sağlamaktır.

Ancak, bunun için işe *kullanıcılarınızı anlamakla* başlamanız gerekir. Ardından, yaptıklarına veya özelliklerine göre gruplar oluşturun (bunlara segment diyoruz) ve her segment için, o segmentle ilgili iletişimler oluşturun.

## <a name="mobile-engagement-serves-your-objectives"></a>Mobile Engagement hedeflerinize hizmet eder
*Elde tutma ve kullanım hakkında konuştuk, ancak amacımız nedir?*

Mobile Engagement stratejinizi oluştururken önce uygulamanızın hedeflerine ve ana performans göstergelerine (KPI) bakmak gerekir.

Hedefleri ve KPI'leri tanımlayarak işe başlamak, katılım sağlayan kullanım örneklerinizi doğru bakış açısından tanımlamanıza yardımcı olur.

Kullanım örnekleri, kullanıcılarınız ile iletişim kurmak üzere oluşturmak isteyeceğiniz basit bir kampanyalar listesidir. Basit bir karşılamadan, BT sisteminiz tarafından tetiklenen çok gelişmiş bir yardımcı program bildirimine kadar farklı kampanyalar içerebilir. İyi oluşturulmuş bir kullanım örneği, en azından *ne-kim-ne zaman* sorularının cevabını içermelidir:

1. Çok kısa bir ad (örneğin bir "Hoş geldiniz kampanyası").
2. **Ne**: Bir ileti örneği (örneğin, "Aramıza katıldığınıza çok sevindik! İlk ayınızı ücretsiz elde etmek için oturum açmayı unutmayın!"). Bu ileti son halinde değildir ve istediğiniz zaman değiştirilebilir, ancak ne söylemek istediğiniz hakkında düşünmeye başlamak çoğu zaman işinizi kolaylaştırır.
3. **Kim**: Bu iletiyi alacak segment (örneğin, "Uygulamayı ilk kez 3 gün önce başlatan, oturum açma sayfasını ziyaret etmiş ancak oturum açmamış olan tüm kullanıcılar").
   * Evet, böyle bir şeyi Azure Mobile Engagement ile kolayca yapabilirsiniz :)
   * Bu tanım da son halinde olmak zorunda değildir. Segmentlerinizi istediğiniz zaman tanımlayabilirsiniz, ancak doğru verilerin toplandığından emin olmak açısından segmentlere ayırmayı erken tanımlamak önemlidir.
4. **Ne Zaman**: Kampanyanızın zamanlaması Belirli bir tarihte veya bir tetikleyiciye bağlı olarak belirli bir eylemden sonra olabilir. Mobile Engagement, iletişiminizin doğru zamanlanmasını sağlamak amacıyla ciddi sayıda olasılık sunar.

Kullanım örnekleri ve segmentin tanımlanması, uygulama içinde toplanması gereken verileri tanımlamak için bir kılavuz sağlar. Bu rolü, *"etiket planı"* üstlenir. Etiket planı, veri toplama özelliğinin geliştiriciler için belirtildiğinden emin olmanıza olanak sağlar. Böylece geliştiriciler, kampanyalarınızda doğru verilerle çalışmanız için Mobile Engagement’a doğru kurulumu ekleyebilir. Tümleştirmenin doğru olduğundan ve ihtiyacınız olan verileri topladığından emin olmak için testler yürütmek de çok önemli olacaktır.

Tümleştirmeye dayalı şekilde, uygulamalar yayımlandıktan sonra bir pazarlamacı olarak analizlerinizi gerçek zamanlı görebilecek, hedef kitlenizi segmentlere ayırabilecek ve son kullanıcılar ile uygulama içinde veya dışında etkileşim kurmak için akıllı, hedefe yönelik anında iletme bildirimleri göndermeye başlayabileceksiniz.

### <a name="usecases-to-get-started"></a>Başlamak için kullanım örnekleri
1. Hoş geldiniz stratejisi: Uygulamanın başlatılmasındaki son kullanıcı davranışına dayalı olarak ilk oturumdan 2/5/10/15 gün sonra yeniden etkileşim kurmaya yönelik çeşitli anında iletme bildirimi kampanyaları oluşturun ve ilk kez kullananların elde tutulmasını artırın.
2. Son kullanıcı davranışından faydalanıp bilgileri yalnızca katılım olasılığı yüksek olan son kullanıcılara göndererek yeni içerik (özellik, makale/video veya ürün) tanıtın.
3. Uygulamaya not verme: Uygulamanıza mağazada 5 yıldız verme olasılığı en yüksek olan, kullanıcı tabanınızın yüzde 1’inden düşük bir kısmını hedefleyin.
4. Abonelik artırma: Değerli içerikleri bunları henüz görmemiş olan son kullanıcılara tanıtarak abonelik sayısını artırın.
5. Öğretici: Herkes için zorunlu öğreticilere son verin. Uygulama içi mükemmel öğreticiler oluşturup, yalnızca kullanıcı uygulamayı kullanmıyormuş gibi görünüyor veya bir özelliği kullanmakta zorluk çekiyorsa uygulama içi iletilerle bu öğreticileri tetiklemek daha iyi bir fikir değil mi?

## <a name="why-do-you-need-analytics-to-engage"></a>Katılım sağlamak için neden analize ihtiyaç var?
Bu noktada fark etmiş olabileceğiniz gibi yalnızca herkese giden bir anında iletme bildirimi kullanmak yeterli değildir. Mobile Engagement’ın temel kavramı, pazarlamacı ve geliştiricilerin doğru son kullanıcıyla, doğru zamanda ve doğru yerde etkileşmesine yardımcı olmaktır. Bu üç ana kavramı bilebilmek için, uygulamanızdan analizler toplayıp bunları kullanarak hedef kitlenizi segmentlere ayırmanız şarttır. Davranış segmentleri diğer veritabanınızdan veya CRM’den ya da çapraz bir kanaldan verilerle tamamlanırsa analiz daha da güç kazanır. Mobile Engagement, her kaynaktan veri almaya olanak sağlar ve bu verileri doğru hedef kitleye seslenmek için kullanır.

Hedef kitlenizle iletişimde konuya en uygun mesajı verebilmek için son kullanıcılarınızın davranışları hakkında bilgi sahibi olmanız ve durumlarını gerçek zamanlı olarak bilmeniz çok önemlidir. Veri toplanması, pazarlamacıların kullanım örneklerini yürütürken önemli noktalara odaklanmasına ve mobil katılım strateji hedeflerine ulaşmasına olanak tanır.  Önceden belirlenen hedeflere ulaşmak için en iyi yöntem, her önüne gelen analizi toplamak yerine, öğrenmek istediklerinize ve kullanım örneklerinize odaklanmanıza olanak tanıyan analizlere yönelmektir. Başlatma, deneme, çözümü test edip kullanmayı öğrenme, akıllı anında iletme bildirimi gönderme ve uygulamayı başarı öyküsü düzeyine taşıyacak şekilde kullanıcıların elde tutulmasını artırma şeklindeki sistemin en iyi yolu budur.

> [!NOTE]
> Unutmayın: Çok fazla veri, veriye zarar verir!
> 
> 

### <a name="usecases-and-best-practices"></a>Kullanım örnekleri ve en iyi yöntemler
Aşağıdaki bölümlerde, başlamanıza yardımcı olması için müşterilerimiz aracılığıyla karşılaştığımız bazı önemli kullanım örneklerini kısaca ele alacağız.

#### <a name="media"></a>Medya
Son kullanıcı tarafından tüketilen içerik türünü toplayın ve hedef kitleyi bu davranışa göre segmentlere ayırarak belirli içerik türlerini bu içerikle ilgilenme olasılığı en yüksek olan kitleye hedefleyin. Tüm kullanıcı tabanına istenmeyen posta gönderilmesini önler ve elde tutmayı artırır.

#### <a name="mcommerce"></a>M-ticaret
Uygulamada en sık ziyaret edilen ürün kategorilerini toplayın ve indirim veya yeni ürün tanıtırken bu kategoride ürün satın alma olasılığı daha yüksek olan son kullanıcıları hedefleyin. Gelirleri artırmayı hedefleyin. Burada da hedefimiz istenmeyen posta göndermemek!

#### <a name="gaming"></a>Oyun
Son kullanıcının oyundaki seviyesini ve belirli bir dönemde harcadığı zamanı toplayarak takılıp kalmış olabilecek ve sonraki seviyeye atlamak için bir ödül teklifini kabul etme olasılığı daha yüksek olan kitleyi hedefleyin.

Bir süredir oynamamış kullanıcılar için bir teşvik içeren belirli olaylar konusunda iletişim kurarak böyle kullanıcıları geri dönmeye özendirin.

#### <a name="retail"></a>Perakende
Hedef kitlenin sevdiği ürünlere veya davranışlarına dayalı olarak tüketme olasılığı yüksek olan ürünleri ya da markaları toplayın ve kitleyi mağazanıza yönlendirerek satın alma gelirlerini artırın.

#### <a name="banking"></a>Bankacılık
Uygulamanın ilk başlatılması sırasında bir hesap oluşturan son kullanıcılara ait verileri toplayın. Hedefe yönelik bir anında iletme bildirimi ile bir hoş geldiniz stratejisi kullanıp hesap aboneliklerinin sayısını artırmayı hedefleyin.

### <a name="how-to-create-a-great-tag-plan"></a>Harika bir etiket planı nasıl oluşturulur?
Etiket planı, kullanıcı davranışlarını anlamak ve kullanıcı tabanını düzgün bir şekilde segmentlere ayırmak için yeterli analize sahip olmak açısından toplanması gereken tüm etiketleri (veri) sağlayan bir çeşit kullanıcı yolu açıklaması veya uygulamanın iş akışı gibi olmalıdır. Bu teknik bir işlem değildir. Bu nedenle, pazarlamacılar Mobile Engagement stratejilerine göre toplamak istedikleri verileri belirtebilir.

Yapılacak minimum işlem, bir uygulamanın en azından tüm ekranlarını (Mobile Engagement’ta *etkinlikler* olarak bilinir) etiketlemektir. Bu, kullanıcı yolunun belirlenmesine yardımcı olur.

Etkinlik, bir düğmeye tıklanması gibi eylem bilgilerini toplayan *olaylar* ekleyebilir. Böylece uygulama içerisindeki etkileşimler toplanabilir. Bu sayede pazarlamacılar, kullanıcıların ziyaret ettiği ekranları ve neler yaptıklarını bilebilirler.

`Jobs`süreli eylemlerdir. Bir kullanıcının hesap oluşturmasının veya oturum açmasının ne kadar sürdüğünü pazarlamacının anlayabilmesi için çok kullanışlıdır. Bir web hizmetini çağırmanın ne kadar sürdüğünü izleyebilmeleri için geliştiricilere de faydalı olabilir.

`Errors`kullanıcıların uygulamanızda sorunlar yaşayıp yaşamadığını öğrenmek için de izlenebilir. Örneğin, sık sık bağlantı sorunlarıyla karşılaşılması.

Bu tür verilerin tümü parametrelerle genişletilerek (Mobile Engagement’ta *ek bilgiler*) uygulamadan dinamik veri toplamanıza olanak sağlanabilir. Bu ayrıntılı segmentlere ayırma için önemlidir. Örneğin, pazarlamacılar kullanıcıyı tükettiği içerik türüne göre bir segmente yerleştirebilir. İçerik türü, etkinlik veya olaya ilişkin dinamik bilgi olacaktır.

*Uygulama bilgileri*, uygulamanın veya kullanıcının durumunu gerçek zamanlı olarak doğrulamanıza olanak tanıyan verilerdir. Bu veriler, aynı zamanda hedef kitle tabanını kategorilere ayırıp hızlı bir şekilde hedeflemeye yardımcı olur. Örneğin, kullanıcının oturum açıp açmadığına ilişkin bir doğru/yanlış durumunu veya kullanıcının abonelik sona erme tarihini kullanabilir.

#### <a name="example-of-tags"></a>Etiket örneği
*Kullanım örneği: Doğru son kullanıcıyı, doğru anında iletme bildirimi içeriğiyle hedeflemek için hedef kitle davranışlarını segmentlere ayırın.*

1. Bir ürün kategorisini tanıtmak üzere anında iletme bildirimi gönderin: Hedef kitleyi belirli bir dönemde x kez ziyaret ettikleri ürün kategorisine veya sepetlerine ekledikleri belirli bir ürüne göre segmentlere ayırmak için davranış verileri toplayın. Toplanan veriler, segmentlere ayırıp doğru hedef kitleye bir anında iletme bildirimi göndermenize olanak tanıyacaktır.
2. Uygulamaya not ver: Bir sosyal ağ üzerindeki hedef kitle tarafından paylaşılan içeriğe dayalı veri toplayın. Uygulamanızın *elçilerini* belirleyerek hedef kitleyi segmentlere ayırmayı amaçlar. Elçiler, uygulama içi bir anında iletme bildirimi kullanarak uygulamanıza mağazada 5 yıldız vermeleri istenebilecek en uygun hedef kitledir.
   
   ![][1]

*Kullanım örneği: Bildirim temelli veriler*

1. Uyarı haberleri segmenti oluşturun: Bildirim temelli veriler toplayarak hedef kitleyi tercihlerine göre segmentlere ayır. Belirli bir kitleyi gerçekten ilgilendiren belirli bir konuda anında iletme bildirimi gönderilmesine olanak tanır.
2. Oturum açma durumuna göre hedef kitleyi segmentlere ayırın. Kullanıcının bağlı olup olmadığını veya hesap oluşturup oluşturmadığını öğrenmek için veri toplayın. Henüz oturum açmamış olan son kullanıcıların hedeflenmesine yardımcı olur ve son kullanıcıyı aranıza katılmaya teşvik eden bir anında iletme bildirimi gönderir.
   ![][2]

### <a name="next-steps"></a>Sonraki adımlar
* Temel Mobile Engagement kavramları hakkında daha fazla bilgi edinmek için [Mobile Engagement Kavramları] makalesini ziyaret edin.
* Azure’da yeni bir Mobile Engagement Uygulama Koleksiyonu oluşturmak ve Mobile Engagement portalıyla uygulamalarınızı yönetmeye başlamak için [Mobile Engagement Uygulaması Oluşturma](mobile-engagement-create.md) makalesini ziyaret edin.
* Ayrıntılı bilgiler için [Best practices](mobile-engagement-getting-started-best-practices.md) (En iyi uygulamalar) makalesini ziyaret edin.
* Örnek bir oyun uygulaması ile Mobile Engagement’ı uygulama hakkında bilgi edinmek için [Oyun Uygulaması senaryosu](mobile-engagement-gaming-scenario.md) makalesini ziyaret edin. 
* Örnek bir medya uygulaması ile Mobile Engagement’ı uygulama hakkında bilgi edinmek için [Medya Uygulaması senaryosu](mobile-engagement-media-scenario.md) makalesini ziyaret edin. 
* Uygulama hakkında daha fazla bilgi edinmek için [Öğreticiler] makalesini ziyaret edin.

<!-- Images. -->
[1]: ./media/mobile-engagement-define-your-mobile-engagement-strategy/use-case1.png
[2]: ./media/mobile-engagement-define-your-mobile-engagement-strategy/use-case2.png

<!-- URLs. -->
[Mobile Engagement Kavramları]: http://azure.microsoft.com/documentation/articles/mobile-engagement-concepts/
[Öğreticiler]: http://azure.microsoft.com/documentation/articles/mobile-engagement-ios-get-started/




<!--HONumber=Nov16_HO2-->


