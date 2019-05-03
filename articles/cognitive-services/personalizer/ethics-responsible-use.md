---
title: Etik kurallara ve sorumlu kullanın - Personalizer
titleSuffix: Azure Cognitive Services
description: Bu yönergeler, kişiselleştirme şirket ve hizmet oluşturmanıza yardımcı olan bir şekilde uygulamak için yardımcı en amaçlanmıştır. Araştırma için Duraklat, öğrenin ve insanların hayatını kişiselleştirmesini etkisini üzerinde deliberate emin olun. Şüpheye düştüğünüzde kılavuzunu isteyin.
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 7b1e972b5516aa79d1754e32e487e17c9e68ac1d
ms.sourcegitcommit: eea74d11a6d6ea6d187e90e368e70e46b76cd2aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2019
ms.locfileid: "65035429"
---
# <a name="guidelines-for-responsible-implementation-of-personalizer"></a>Personalizer sorumlu uygulaması için yönergeler

Kişiler ve society AI tam potansiyelini ortaya çıkarmak, uygulamaları bunlar güveninizi Bu yapay ZEKA uygulamalarına ekleme ve uygulama kullanıcılarının yapay ZEKA ile oluşturulmuş bir şekilde tasarlanmış olması gerekir. Bu yönergeleri Personalizer şirket ve hizmet oluşturmanıza yardımcı olan bir şekilde uygulamak için yardımcı adresindeki amaçlanmıştır. Araştırma için Duraklat, öğrenin ve insanların hayatını kişiselleştirmesini etkisini üzerinde deliberate emin olun. Şüpheye düştüğünüzde kılavuzunu isteyin.

Bu yönergeleri yasal öneri amaçlanmamıştır ve hızlı ve bol demo'lu gelişmeler ile uygulamanızı yasaların bu alandaki ve, kesimdeki uyumlu ayrı olarak emin olun.

Ayrıca, Personalizer kullanarak uygulamanızı tasarlarken, çok sayıda ethics, gizlilik, güvenlik, güvenilirlik, ekleme, saydamlık ve Sorumluluk dahil olmak üzere tüm veri merkezli yapay ZEKA sistemi geliştirirken sahip sorumlulukları düşünmelisiniz. Daha fazla bilgiyi bu hakkında [önerilen okumalar](#recommended-reading) bölümü.

Aşağıdaki içeriğe başlangıç denetim listesi kullanın ve özelleştirebilir ve senaryonuz için İyileştir. Bu belge, iki ana bölümü vardır: İlk senaryo, özellikler ve ödüller için Personalizer seçerken sorumlu kullanım konuları vurgulama için ayrılır. İkinci sınav zamanı, Microsoft düşündüğü bir dizi yapay ZEKA sistemleri oluşturmayı göz önünde bulundurulması ve eyleme dönüştürülebilir öneriler ve riskleri Personalizer kullanımınız bunları nasıl etkileyeceğini üzerinde sağlar. 


## <a name="your-responsibility"></a>Sizin sorumluluğunuzdadır

Geliştiriciler ve Personalizer kullanan işletmeler sorumlu ve bu algoritmalar society kullanmanın etkileri sorumlu foundation sorumlu uygulama için tüm kuralları oluşturun. Kuruluşunuzda dağıtacağınız bir uygulama geliştiriyorsanız rol ve sorumluluk, işlemi ve kişiler etkilemesi için tanıması gerekir. Dağıtılacak bir uygulama tasarlarken, üçüncü bir şahıs, uygulamanın davranışını sorumlu kim, kendileriyle paylaşılan bir anlayış için gelen ve bu anlama belgeleyin.

Güven karşılanan taahhütleri kavramına yerleşik olarak bulunur - kullanıcılarınızın, society ve sahip olabilir, açık ve örtük taahhütleri tanımlamak için uygulamalarınızı çalışır, yasal çerçeve göz önünde bulundurun.

Microsoft, araçları ve aşağıdaki sorumluluklara göre hareket yardımcı olacak belgeler çaba sürekli olarak koyuyor. [Microsoft'a geri bildirimde](mailto:cogsvcs-RL-feedback@microsoft.com?subject%3DPersonalizer%20Responsible%20Use%20Feedback&body%3D%5BPlease%20share%20any%20question%2C%20idea%20or%20concern%5D) ek araçlar düşünüyorsanız, ürün özellikleri ve belgeleri Personalizer kullanmak için aşağıdaki yönergeleri uygulamak yardımcı.


## <a name="factors-for-responsibly-implementing-personalizer"></a>Faktörleri depoladığımız Personalizer uygulamak için

Personalizer uygulama kullanıcılarınızın ve işletmenizin için mükemmel bir değer olabilir. Aşağıdaki yönergeleri göz önünde bulundurularak Personalizer depoladığımız uygulamak için başlangıç zaman:

* Kişiselleştirme uygulamak için kullanım örnekleri seçme.
* Derleme [ödüllendirin işlevleri](https://github.com/Azure/personalization-rl/blob/master/docs/concepts-rewards.md).
* Seçme [özellikleri](https://github.com/Azure/personalization-rl/blob/master/docs/concepts-features.md) kişiselleştirme için kullanacağınız bağlamını ve olası eylemler hakkında.


## <a name="choosing-use-cases-for-personalizer"></a>Kullanım örnekleri için Personalizer seçme

İçeriği ve kullanıcı arabirimleri kişiselleştirmek için öğrenir bir hizmeti kullanarak yararlı olur. Yol kişiselleştirme negatif yan etkileri gerçek dünyada, kullanıcılar içerik kişiselleştirme farkında değilseniz, dahil olmak üzere oluşturması halinde, ayrıca misapplied. 

Yazımı olası ile Personalizer kullanımları örnekleri negatif yan etkileri ya da saydamlık eksikliği eklemek için burada "ödül" bağlıdır üzerinde çok uzun vadeli karmaşık senaryoları Etkenler, zaman içine aşırı Basitleştirilmiş hemen bir ödül unfavorable olabilir kişiler için sonuçları. Bu "neticede oluşan" seçenekleri ya da zarar riskini ilgili seçenekleri değerlendirilmesi eğilimindedir. Örneğin: 


* **Finans**: Kişiselleştirme kredisi Finans ve sigorta ürünler, burada risk faktörleri verilerine dayalı kişilerin bilgi sahibi olmadığınız, elde edemez veya dispute olamaz sunar. 
* **Eğitim**: Okul kurslar ve eğitim kurumları önerileri burada ve sapmaları yaymak diğer seçenekleri kullanıcıların farkındalık azaltmak için sıralamalara sahip kişiselleştirme.
* **Demokrasi ve Vatandaşlık katılım**: Neticede oluşan ve manipulative konularında fikirlerini amacı sahip olan kullanıcıların içerik kişiselleştirme.
* **Üçüncü taraf ödül değerlendirme**: Ödül üzerinde ikinci bir 3 temel burada öğeleri kişiselleştirme kullanıcının kendi davranış tarafından oluşturulan bir ödül yerine kullanıcı taraf değerlendirmesi.
* **Araştırma için intolerance**: Tüm durumlarda zarar Personalizer araştırma davranışını burada neden olabilir.

Kullanım örnekleri için Personalizer seçerken:

* Kişiselleştirme kullanıcılarınıza nasıl yardımcı olduğunu düşünüyor tasarım işlemi başlatın.
* Bazı öğeler kişiselleştirme desenleri veya araştırma nedeniyle kullanıcıların derecelendirdiyseniz değil gerçek dünyada olumsuz etkileri göz önünde bulundurun.
* Kendi kendine Paskalya prophecy döngüler göz önünde bulundurun. Daha sonra başka işlemler, böylece kişiselleştirme ödül bir modeli eğitir durumunda bu sorun demografik grubu ilgili içeriklere erişmekten hariç. Örneğin, çoğu kişi low-income Komşuları içinde premium sigorta teklif elde yok ve yavaş Komşuları içinde hiç kimse hiç teklifi görüntülemek üzere eğilimlidir.
* Personalizer gelecekte yeniden oluşturmak için gerekli olması durumunda modelleri ve öğrenme ilkeleri bir kopyasını kaydedin. Düzenli aralıklarla veya her model yenileme süresi bunu yapabilirsiniz.
* Araştırma düzeyini yeterli alan ve "Yankı odasına" etkilerini hafifletmek için bir araç olarak kullanmak için göz önünde bulundurun.


## <a name="selecting-features-for-personalizer"></a>Personalizer için özellikler seçme

İçeriğin kişiselleştirilmesini içeriği ve kullanıcı hakkında yararlı bilgiler bağlıdır. Bazı uygulamalar ve sektörlerde, özelliklerin doğrudan veya dolaylı olarak discriminatory ve potansiyel olarak geçersiz olarak kabul edilebilir bazı kullanıcı unutmayın.

Bu özellikler etkisini göz önünde bulundurun:

* **Kullanıcı demografisi**: Seks, cinsiyet, yaş, yarış, Din ilgili özellikler: Bu özellik bazı uygulamalarda Yasal nedenlerle verilmez ve kişiselleştirme genelleştirmesi ve sapma yayar çünkü etrafında kişiselleştirmek etik olmayabilir. Bu fark yayma elderly veya cinsiyet tabanlı kitlelere gösterilen değil Mühendisliği için gönderen bir iş örneğidir.
* **Yerel ayar bilgilerinin**: Dünyanın birçok yerde konum bilgilerini (örneğin, bir posta kodu, posta kodu veya Komşuları adı) yüksek oranda gelir, yarış ve Din ile ilişkili olabilir.
* **Kullanıcı algısı eşitliği**: Burada, uygulamanızın doğru kararlar yapıyor durumlarda bile kullanıcıların uygulama değişikliklerinizi discriminatory olan özelliklerle ilişkili olabilir görünmesine şekilde görüntülenen içeriğin perceiving etkisini göz önünde bulundurun.
* **Özellikler istenmeyen sapması**:  Yalnızca bir alt kümesini bir popülasyon etkileyen özellikleri kullanarak sunulan sapmaları tür vardır. Özellikleri algorithmically oluşturuluyor, bu çok dikkat gerektirir, ne zaman kullanarak görüntü gibi analiz ayıklamak için bir resim veya metin metin varlıkları bulmak için analiz öğeleri. Kendinizi bu özellikler oluşturmak için kullandığınız hizmetlerin özelliklerini farkında olun.

Bağlamları ve Eylemler için Personalizer göndermek için özellikler seçerken aşağıdaki yöntemleri uygulayın:

* Bazı uygulamalar için belirli özellikleri kullanarak, etik kurallara ve yasal göz önünde bulundurun ve zararsız görünümlü özellikleri diğerleri için proxy'leri olabilir, istediğiniz veya kaçınmanız gerekir,
* Kullanıcılar için saydam olarak göreceği seçenekler kişiselleştirmek için algoritmalar ve veri analizi kullanıldığını.
* Kendinize sorun: Kullanıcılarımın dikkat edin ve miyim içeriği kişiselleştirmek için bu bilgileri kullandıysanız mutlu? Ben kararı belirli öğeleri gizlemek veya vurgulamak için nasıl yapıldı gösteren hissedene?
* Diğer özelliklerine göre sınıflandırma veya Segment veri yerine davranış kullanın. Demografik bilgiler geleneksel olarak kullanılan Perakendeciler tarafından – demografik öznitelikler görünen toplamak ve dijital Çağ önce üzerinde işlem yapmak basit - geçmiş nedeniyle ancak soru ne gerçek etkileşim olduğunda ilgili demografik bilgileri kullanıcıların kimliğini ve tercihleri daha yakından ilgili bağlamsal ve geçmiş verileri.
* 'Kandırılan gelen' özellikleri engelleme Personalizer kullanılamıyor.%n%nÇözüm kesintiye, utandırmaktan ve kullanıcıların, belirli sınıfların taciz yolları yanıltıcı de eğitim için çok sayıda kullanılırsa açabilir kötü amaçlı kullanıcılar tarafından göz önünde bulundurun. 
* Uygun ve uygun olduğunda, kullanıcılarınızın kabul et veya kullanılan kişisel belirli özelliklere sahip dışında iyileştirilmiş sağlamak için uygulamanızı tasarlayın. Bu, "Konum bilgileri gibi", "Cihaz bilgileri", "Geçmiş satın alma geçmişi" vb. gruplandırılır.


## <a name="computing-rewards-for-personalizer"></a>Ödül Personalizer için bilgi işlem

Uygulamanızın iş mantığı tarafından sağlanan ödül puan göre ödül için hangi eylemin seçimi geliştirmek personalizer içindedir.

İyi oluşturulmuş ödül puanı, bir kuruluşun görev için bağlı olmayan bir iş hedef kısa süreli bir proxy olarak görür.

Örneğin, ne tıklandığında, dikkat dağıtıcı ya da bir iş sonucu için bağlı olsa bile tıklar ödüllendiriyoruz başka her şey çoğaltamaz Personalizer hizmet arama tıklama hale getirir.

Karşıt bir örnek olarak, bir haber site tıkladığında, gibi çok daha anlamlı bağlı ödül "kullanıcı içeriğini okumak için yeterli zaman ayırın muydunuz?" ayarlamak isteyebilirsiniz "Bunların ilgili makaleler veya başvurular üzerinde tıklayın?". Personalizer ile yakından ödül ölçümlerini bağlamak kolaydır. Ancak iyi sonuçlar ile kısa vadeli kullanıcı etkileşimini confound değil dikkatli olun.

### <a name="unintended-consequences-from-reward-scores"></a>Ödül puanları gelen istenmeyen sonuçları
Ödül puanları amaçları en iyi şekilde oluşturulmuş, ancak hala beklenmeyen sonuçlarıyla veya istenmeyen sonuçları Personalizer içeriği nasıl sıralayan oluşturabilirsiniz. 

Aşağıdaki örnekleri dikkate alın:

* İzlenen video uzunluğu yüzdesine video içerik kişiselleştirme ödüllendiriyoruz derece kısa videolar için büyük olasılıkla eğilimli.
* Yaklaşım analizi nasıl paylaşılacağını veya içeriğin kendisini olmadan, sosyal medya paylaşımları ödüllendiriyoruz "katılım" çok fazla yummaz eğilimindedir, ancak küçük değer kazandıran rahatsız edici, yönetilmeyen veya tahrik edici içerik derecelendirme için neden olabilir.
* Tahmin edilebilirliğini burada düğmeleri yayımladım konumu veya bu belirli daha zor hale uyarmadan amaçlı değiştirmekte olduğunuz kullanıcı arabirimi ve kullanılabilirlik ile değiştirmek için kullanıcıların beklemiyoruz kullanıcı arabirimi öğeleri eylemini ödüllendiriyoruz etkileyebilir üretken kalabilmek için kullanıcı gruplarına.

Bu en iyi yöntemleri uygulayın:

* Çevrimdışı denemeleri sisteminizle etkisi ve yan etkileri anlamak için farklı bir ödül yaklaşımları kullanarak çalıştırın.
* Ödül işlevlerinizi değerlendirin ve kendinize sorun nasıl bir son derece naïve kişi yorumlanması Kıvrım ve onunla istenmeyen sonuçlar ulaşın.


## <a name="responsible-design-considerations"></a>Sorumlu tasarım konuları

Sorumlu yapay ZEKA uygulamaları için tasarım alanları şunlardır: Bu framework sonlandıran daha fazla bilgi edinin [gelecek dönemlere](https://news.microsoft.com/futurecomputed/).

![Hesaplanan gelecekteki değerlerinden yapay ZEKA](media/ethics-and-responsible-use/ai-values-future-computed.png)

### <a name="accountability"></a>Sorumluluk
*Tasarım ve yapay ZEKA sistemlerini dağıtma kişiler sistemlerini nasıl çalışması için sorumlu olmalıdır*. 

* İç oluşturma Personalizer, gerçekleştirme yönergeleri belge ve takım, Yöneticiler ve tedarikçileri iletişim.
* Ödül puanları nasıl hesaplanır, düzenli aralıklarla gözden geçirmeleri gerçekleştirmek, hangi özellikler Personalizer etkileşimimiz görmek için çevrimdışı değerlendirme gerçekleştirmek ve sonuçları gereksiz ve gereksiz özelliklerini ortadan kaldırmak için kullanın.
* Açıkça kullanıcılarınıza nasıl Personalizer, hangi amacı ve hangi verilerin ile kullanılan iletişim kurar.
* Bilgi ve varlıklar - modelleri ve öğrenme ilkeleri Personalizer çalışması için sonuçları yeniden kullanabilmek için kullandığı diğer veriler - gibi arşivleyebilirsiniz.

### <a name="transparency"></a>Saydamlık
*Yapay ZEKA sistemleri olması Understandable*. Personalizer ile

• Kullanıcıların içeriği nasıl kişiselleştirilmiş hakkında bilgi verin. Örneğin, kullanıcılarınız etiketli bir düğme gösterebilirsiniz "neden bu önerileri?" kullanıcı eylemleri ve en iyi hangi özellikler bir rol Personalizer sonuçlarında yürütülen gösteriliyor.
• Kullanım oluşturma deneyimini kişiselleştirmek için kullanıcılar ve davranışları hakkında bilgi kullanacağınız bahsetmek koşullarınızın emin olun.


* *Kullanıcılara nasıl içerik kişiselleştirilmiş hakkında bilgi verin.* Örneğin, kullanıcılarınız etiketli bir düğme gösterebilirsiniz `Why These Suggestions?` kullanıcı eylemleri ve en iyi hangi özellikler kişiselleştirmeyi rol yürütülen gösteriliyor.
* Kullanıcılar hakkında bilgileri deneyimini kişiselleştirmek için kullanacağınız kullanım koşullarınızı bahsetmek emin olun.

### <a name="fairness"></a>Eşitliği
* AI sistemleri tüm kişileri oldukça değerlendirmeniz gerekir.

* Personalizer olduğu sonuçlarını uzun vadeli, arızi ya da gerçek zarar ilgili kullanım durumları için kullanmayın.
* İçerikle kişiselleştirmek uygun olmayan veya istenmeyen sapmaları yaymak yarayabilir özellikler kullanmayın. Örneğin, finansal benzer koşullarda kimseyle finansal ürünler için aynı kişiselleştirilmiş öneriler görmeniz gerekir.
* Düzenleyiciler, algoritmik araçları veya kullanıcılar kendileri kaynaklanan özellikleri bulunabilir sapmaları anlayın.

### <a name="reliability-and-safety"></a>Güvenilirlik ve güvenliği
*Yapay ZEKA sistemleri, güvenilir ve güvenli bir şekilde yükleyebildiğiniz*. Personalizer için:

* *Eylemler, seçilen olmamalıdır Personalizer sağlamayan*. Örneğin, uygunsuz bir şekilde filmler için anonim bir öneri veya eksik yaş kullanıcı yapma, kişiselleştirmek için Eylemler dışında filtrelenmelidir.
* *Bir iş varlığı Personalizer modelinizi yönetme*.  Aksi takdirde bir önemli iş varlığı kabul et ve sıklıkla kaydedin ve model ve ilkeleri Personalizer döngünüzü arkasında öğrenme yedeklemek nasıl düşünün. Sonuçları yeniden Self denetim ve ölçüm geliştirme için önemlidir.
* *Kullanıcıların doğrudan geri bildirim almak için kanal sağlamak*. Emin olmak için güvenlik denetimleri kodlama yanı sıra kullanıcılar şaşırtıcı veya rahatsız edici olabilecek rapor içeriği için geri bildirim için bir mekanizma sağlar, doğru içeriği doğru hedef kitlelere bakın. İçeriğinizi, kullanıcılar veya 3. taraflara özellikle gelen gözden geçirmek ve içerikleri doğrulamak için Microsoft Content Moderator veya ek araçları kullanmayı düşünün.
* *Sık çevrimdışı değerlendirme gerçekleştirmek*. Bu, eğilimleri izlemeniz ve verimliliğini bilinen emin olun yardımcı olur.
* *Bir işlem Algıla ve kötü amaçlı işleme üzerinde işlem yapma kurmak*. Makine öğrenimi ve yapay ZEKA sistemleri hedeflerine doğru sonucu kaydırılacak ortamlarında uzmanlardan olanağı faydalanması aktör vardır. Personalizer kullanımınız önemli seçenekleri etkilemek için bir konum varsa, algılama ve saldırıları, uygun durumlarda insan tarafından İnceleme de dahil olmak üzere bu sınıfların azaltmak için uygun bir yola sahip emin olun.

### <a name="security-and-privacy"></a>Güvenlik ve gizlilik
*Yapay ZEKA sistemleri güvenli ve gizliliğine saygı*. Personalizer kullanırken:

* *Kullanıcılar Önden toplanan veriler hakkında ve nasıl kullanıldığını bildiren ve kendi onayı önceden almak*, yerel ve sektörel düzenlemelere izleyerek.
* *Gizliliğinizi korumaya kullanıcı denetimleri sağlar.* Kişisel bilgi depolayan uygulamalar için bir kolayca Bul butonuna gibi işlevler için sağlama göz önünde bulundurun: 
   * `Show me all you know about me`    
   * `Forget my last interaction` 
   * `Delete all you know about me`

Bazı durumlarda, bu yasal olarak gerekli olabilir. Silinen verileri izlemeleri içermeyen için düzenli aralıklarla modelleri yeniden eğitme de ödün göz önünde bulundurun.

### <a name="inclusiveness"></a>İnclusiveness
*Geniş bir insan gereksinimlerini ve deneyimler adres*.
* *Erişilebilirlik özellikli arabirimler için kişiselleştirilmiş deneyimler sağlar.* Engelli kişiler için özellikle yararlı iyi kişiselleştirme - çaba, taşıma ve etkileşimleri can tekrarından miktarını azaltmak için uygulanan geldiği verimliliği.
* *Bağlam için uygulama davranışını ayarlama*. Personalizer hedefleri, örneğin, bir sohbet Robotu arasında ayırt etmek için kullanabileceğiniz doğru yorumu bağlamsal olabilir ve bir boyutu değil uygun tüm gibi. 


## <a name="proactive-readiness-for-increased-data-protection-and-governance"></a>Artan veri korumayı ve idare proaktif hazırlığı

Yasal bağlamları belirli değişiklikleri tahmin etmek zordur, ancak genel olarak, kişisel verilerin kullanımı saygılı sağlama ve saydamlık ve algoritmik karar için ilgili seçeneğe sağlayarak en düşük yasal çerçeve ötesine geçip yararınıza olacaktır.


* Burada yeni kısıtlamalar kişilerden gelen toplanan veriler üzerinde olabilir ve gerekir bir durum planlama göz önünde bulundurun nasıl kararları vermek için kullanılan gösterilecek.
* Burada kullanıcıları marginalized savunmasız ortalamaya, alt, ekonomik güvenlik açığı kullanıcılar veya kullanıcı aksi içerebilir ek hazırlık algoritmik işleme etkilemek için maruz göz önünde bulundurun.
* Yaygın memnuniyetsizliğine göz önünde bulundurun nasıl İzleyici hedefleme ve İzleyici etkileyen veri koleksiyonuyla programlar ve algoritmaları oynayıp ve kendini kanıtlamış stratejik hatalarını önlemek nasıl.


## <a name="proactive-assessments-during-your-project-lifecycle"></a>Projenizin yaşam döngünüzü sırasında proaktif değerlendirmeleri

Takım üyeleri, kullanıcılar ve sorumlu kullanımı ile ilgili ve bunların çözümü önceliklendirir ve retaliation engelleyen bir işlemi oluşturma rapor endişeleriniz için işletme sahipleri için yöntemleri oluşturmayı düşünün.

Herhangi bir teknolojinin kullanımının yan etkileri hakkında düşünmek herhangi bir kişi tarafından Perspektif ve yaşam deneyimlerini sınırlıdır. Takımlar, kullanıcılar veya danışmanlık panoları daha farklı sesler taşıyarak düşünceleri son derece kullanılabilir aralığını genişletin; Örneğin, olası ve bunları konuşmak için önerilir. Eğitim ve malzemelerin daha da bu etki alanında takım bilgisini genişletin ve karmaşık ve önemli konulara tartışmak için özelliği eklemek için göz önünde bulundurun.

Kullanıcıyla ilişkili görevleri deneyimi, güvenlik veya gibi devops uygulama yaşam döngüsü gibi diğer crosscutting görevleri sorumlu kullanımıyla ilgili görevleri değerlendirmesini göz önünde bulundurun. Bu görevleri ve bunların gereksinimleriyle akla olamaz. Sorumlu kullanımı ele alınan ve uygulama yaşam döngüsü boyunca doğrulandı.
 
## <a name="questions-and-feedback"></a>Sorular ve geri bildirim

Microsoft araçları ve aşağıdaki sorumluluklara göre hareket yardımcı olacak belgeler çaba sürekli olarak koyuyor. Ekibimiz, davet [Microsoft'a geri bildirimde](mailto:cogsvcs-RL-feedback@microsoft.com?subject%3DPersonalizer%20Responsible%20Use%20Feedback&body%3D%5BPlease%20share%20any%20question%2C%20idea%20or%20concern%5D) ek araçlar, ürün özellikleri düşünüyorsanız ve belgeler Personalizer kullanmak için bu yönergeleri uygulamanıza yardımcı.

## <a name="recommended-reading"></a>Önerilen okumalar

* Microsoft'un altı ilkelerine yapay ZEKA Ocak 2018'den kitap yayımlanan sorumlu geliştirmek için bkz. [gelecek dönemlere](https://news.microsoft.com/futurecomputed/)
* [Gelecek Kime ait? ](https://www.goodreads.com/book/show/15802693-who-owns-the-future) Jaron Lanier tarafından.
* [Matematik yok etme Silah](https://www.goodreads.com/book/show/28186015-weapons-of-math-destruction) tarafından - Cathy O'Neil
* [Etik kurallara ve veri bilimi](https://www.oreilly.com/library/view/ethics-and-data/9781492043898/) DJ Patil, Hilary Mason Mike Loukides tarafından.
* [ACM kodunu Ethics](https://www.acm.org/code-of-ethics)
* [Genetik bilgi Nondiscrimination Act - GINA](https://en.wikipedia.org/wiki/Genetic_Information_Nondiscrimination_Act)
* [Sorumlu algoritmaları için FATML ilkeleri](http://www.fatml.org/resources/principles-for-accountable-algorithms)


## <a name="next-steps"></a>Sonraki adımlar

[Özellikler: eylem ve bağlam](concepts-features.md).
