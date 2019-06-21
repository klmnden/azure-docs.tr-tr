---
title: Azure'da mikro hizmetler giriş | Microsoft Docs
description: Bir genel bakış neden mikro hizmetler yaklaşımı ile bulut uygulamaları geliştirmek için modern uygulama geliştirmenin önemli olduğunu ve nasıl Azure Service Fabric, bunu yapmanın bir platform sağlar.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: fae2be85-0ab4-4cd3-9d1f-e0d95fe1959b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/18/2019
ms.author: atsenthi
ms.openlocfilehash: 5bcb52165c7cae18b807eff03c80b51eae8e2717
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204803"
---
# <a name="why-use-a-microservices-approach-to-building-applications"></a>Neden mikro hizmetler yaklaşımı uygulamaları oluşturmak için kullanılır?

Yazılım geliştiricileri için bir uygulama bileşeni parçalara hesaba katacak şekilde yeni bir şey değildir. Genellikle, katmanlı bir yaklaşım, bir arka uç depolama, orta katman iş mantığı ve bir ön uç kullanıcı arabirimi (UI) kullanılır. Hangi *sahip* geliştiricilerin bulut için dağıtılmış uygulamalar oluşturuyorsanız olan son birkaç yılda değiştirildi.

Bazı değişen iş gereksinimleri şunlardır:

* Yerleşik ve yeni coğrafi bölgelerdeki müşterilere ölçekte çalıştırılan bir hizmeti.
* Özellikler ve yetenekler, Müşteri taleplerine Çevik bir şekilde yanıt vermek için daha hızlı teslim.
* Maliyetleri azaltmak için geliştirilmiş kaynak kullanımı'nı tıklatın.

Bu iş ihtiyaçları etkileyen *nasıl* uygulamaları ekliyoruz.

Azure mikro hizmetler yaklaşımı hakkında daha fazla bilgi için bkz: [mikro hizmetler: Bulut tarafından desteklenen bir uygulama Devrimi](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="monolithic-vs-microservices-design-approach"></a>Tek parçalı mikro hizmet yaklaşımı karşılaştırması

Uygulamalar zamanla gelişmesinin. Kişiler için yararlı olan başarılı uygulamalar geliştirin. Başarısız uygulamalar evrim Geçiren yok ve sonunda kullanım dışı bırakılmıştır. Sorunun şu şekildedir: ne kadar biliyor musunuz gereksinimleriniz hakkında Bugün ve ne olmaları gelecekte? Örneğin, şirketinizde bir bölüm için bir raporlama uygulaması oluşturuyorsunuz varsayalım. Uygulamayı yalnızca şirketiniz kapsamında uygular ve raporları uzun tutulmayacak emin emin olabilirsiniz. Söyleyin yaklaşımınızı, farklı olacaktır, bir hizmet oluşturma için on milyonlarca müşteriye video içeriği sunar.

Bazen, bir kavram kanıtı olarak bir kullanıma alma sürükleyici etken olabilir. Uygulama daha sonra yeniden tasarlanmış biliyor. Hiçbir zaman kullanılan bir şey aşırı mühendislik içinde çok az noktası yok. Şirketler bulut için oluşturun, diğer taraftan, büyüme ve kullanım beklenir. Büyüme ve ölçek tahmin edilemez. Prototip hızla gelecekteki işleyebileceği yola olmamıza da bilerek istiyoruz. Yalın başlangıç yaklaşım budur: oluşturun, ölçün, öğrenin ve yineleme.

İstemci/sunucu dönemi sırasında size her katmanında belirli teknolojileri kullanarak katmanlı uygulamalar oluşturmaya odaklanın tended. Terim *tek parça* uygulama ortaya Bu yaklaşımları açıklamak için. Katmanlar arasında olacak şekilde arabirimleri tended ve daha sıkı şekilde bağlı bir tasarım, her bir katmanında bileşenleri arasında kullanıldı. Geliştiriciler tasarlanmış ve factored kitaplıklara derlenmiş ve birkaç yürütülebilir dosyaları ve dll birbirine sınıfları.

Tek yapılı tasarım yaklaşımın avantajları vardır. Genellikle tek yapılı uygulamalar tasarlamak daha basit ve çünkü bu çağrılar genellikle işlemler arası iletişim (IPC) yapılan çağrılar bileşenleri arasında daha hızlıdır. Ayrıca, herkesin İnsan kaynakları daha verimli bir kullanım olma eğilimindedir tek bir ürün sınar. Dezavantajı, katmanlı katmanlar arasında sıkı bir bağ vardır ve tek tek bileşenler ölçeklendirilemez ' dir. Düzeltmeleri veya yükseltmeler yapmanız gerekiyorsa, diğerleri kendi test bitmesini beklemek zorunda. Daha Çevik olmasını olduğu.

Mikro hizmetler bu downsides adresi ve önceki iş gereksinimleriyle daha yakından Hizala. Ancak avantajları ve Borçlar hem de sahiptir. Mikro hizmetler avantajları, her biri genellikle ölçeği artırın veya azaltın, test etmek, dağıtmak ve bağımsız olarak yönetmek daha basit iş işlevselliğini Kapsüller ' dir. Mikro hizmetler yaklaşımı önemli yararlarından biri, takımlar tarafından iş senaryolarını daha fazla bilgi teknolojisi tabanlı ' dir. Küçük takımlar, müşteri senaryo temel alınarak bir mikro hizmet geliştirin ve kullanmak istediğiniz tüm teknolojileri kullanın.

Diğer bir deyişle, kuruluş, mikro hizmet uygulamalarını korumak için teknik standart hale getirmek gerekmez. Ayrı takımlar kendi Hizmetleri algılama için bunları team uzmanlık düzeyine göre yapar veya sorunu çözmek en uygun ne yapabilirsiniz. Uygulamada, bir dizi belirli bir NoSQL deposu gibi teknolojileri önerilen veya web uygulama çerçevesi, tercih edilir.

Mikro hizmetler dezavantajı, daha fazla ayrı varlıklar yönetmek ve daha karmaşık dağıtımlar ve sürüm oluşturma sahip olduğunu belirtir. Karşılık gelen ağ gecikmeleri gibi mikro hizmetler arasındaki ağ trafiğini artırır. Sık iletişim kuran, ayrıntılı hizmetler çok sayıda performans onarımı kabus neden olabilir. Bu bağımlılıkları görüntülemek için Araçlar olmasaydı sisteminin tamamını görmek zordur.

İletişim kurma belirterek ve yalnızca bir hizmetten gereken şeyleri dayanıklılık iş yerine katı standartlar olun mikro hizmetler yaklaşımı sözleşme. Güncelleştirme Hizmetleri birbirinden olduğundan, bu sözleşme Önden tasarımında tanımlamak önemlidir. Bir mikro hizmetler yaklaşımı tasarlama "ayrıntılı hizmet odaklı mimari (SOA)." için de başka bir açıklaması

***En basit haliyle, mikro hizmetler yaklaşımı her bağımsız değişiklikleri ve anlaşılan devlet standartlarının iletişim hizmetlerinin ayrılmış bir Federasyon hakkındadır.***

Daha fazla bulut uygulaması üretilen gibi kişilerin bu ayrıştırma bağımsız, senaryo odaklı hizmetleri genel uygulamanın daha uzun süreli bir yaklaşım olan keşfettik.

## <a name="comparison-between-application-development-approaches"></a>Uygulama Geliştirme yaklaşımları arasında karşılaştırma

![Service Fabric platform uygulaması geliştirme][Image1]

1) Tek parça bir uygulamayı etki alanına özel işlevler içerir ve bu normalde web ve business veri gibi işlevsel katmanlara ayrılır.

2) Birden çok sunucu/sanal makinelerde kopyalayarak tek parça bir uygulamayı ölçeklendirme/kapsayıcıları.

3) Bir mikro hizmet uygulama işlevselliğini farklı daha küçük hizmetler halinde ayırır.

4) Dağıtarak out mikro hizmetler yaklaşımı ölçekler her bağımsız olarak, bu hizmetlerin örneklerini sunucuları/sanal makinelerde oluşturma hizmet/kapsayıcıları.

İle bir mikro hizmetler tasarlama, yaklaşım tüm projeler için uygun değildir, ancak daha önce açıklanan iş hedefleri ile hizalamaya daha yakından. Kod tasarımından mikro hizmet daha sonra yeniden fırsatına sahip olacaksınız biliyorsanız tek parçalı bir yaklaşım ile başlayan mantıklı olabilir. Daha sık monolitik bir uygulamayla başlar ve daha ölçeklenebilir ve daha Çevik olması gereken işlevsel alanları ile başlangıç aşamasında, yavaş bölün.

Mikro hizmetler yaklaşımı kullandığınızda, birçok küçük hizmetten uygulamanızı oluşturun. Bu hizmetler, makine kümesi dağıtılan kapsayıcılarında çalıştırın. Küçük takımlar senaryoyu odaklanan bir hizmeti geliştirme ve test bağımsız olarak sürümü, dağıtma ve her hizmet ölçeklendirme tüm uygulama geliştirebilirsiniz.

## <a name="what-is-a-microservice"></a>Bir mikro hizmet nedir?

Mikro hizmetler farklı tanımları vardır. Ancak mikro hizmetlerin bu özelliklerin en yaygın olarak kabul edilir:

* Bir müşteri veya iş senaryosu kapsüller. Çözme?
* Küçük bir mühendislik ekibi tarafından geliştirilmiştir.
* Herhangi bir çerçeveyi kullanarak tüm programlama dilinde yazılır.
* Kodu oluşur ve isteğe bağlı olarak durumu, ikisi için de birbirinden bağımsız sürümlere, dağıtılan ve ölçeklendirilebilir.
* İle diğer mikro hizmetler, iyi tanımlanmış arabirimlere ve protokolleri üzerinden etkileşim kurun.
* (URL), konumu çözümlemek için kullanılan benzersiz adlara sahip.
* Tutarlı ve kullanılabilir oluşturucunun hata olması durumunda kalır.

Toplamak için:

***Mikro hizmet uygulamaları, iyi tanımlanmış arabirimlere sahip standart protokoller üzerinden birbiriyle iletişim kuran küçük, birbirinden bağımsız sürümlere ve ölçeklenebilir müşteri odaklı hizmetler oluşur.***

### <a name="written-in-any-programming-language-using-any-framework"></a>Herhangi bir çerçeveyi kullanarak tüm programlama dilinde yazılmış

Geliştiriciler olarak, bir dil veya framework, bizim becerileri ve oluşturuyoruz hizmet gereksinimlerine bağlı olarak seçmek ücretsiz olmasını istiyoruz. Bazı hizmetler için performans avantajlarını değeri C++ yukarıda başka bir şey. Diğerleri, aldığınız yönetilen geliştirme kolaylığı için C# veya Java daha önemli olabilir. Bazı durumlarda, istemcilere hizmet açığa çıkarmak için belirli bir iş ortağı kitaplığı, veri depolama teknolojisinin ya da yöntem kullanmanız gerekebilir.

Bir teknoloji seçtikten sonra işletimsel düşünün veya yönetimi ve hizmet ölçeklendirme ömrü gerekir.

### <a name="allows-code-and-state-to-be-independently-versioned-deployed-and-scaled"></a>Kod ve birbirinden bağımsız sürümlere olması durumuna dağıtılan ve ölçeği sağlar.

Nasıl, mikro hizmetlerin, kod ve isteğe bağlı olarak durumu yazma ne olursa olsun, bağımsız olarak dağıtma, yükseltme ölçeklendirin ve. Bu sorun, kendi seçtiğiniz teknolojilerle gelir çünkü çözmek zordur. Ölçeklendirme, anlamak için nasıl bölüm (veya parça) hem kod hem de durumu zor olur. Kod ve durum bugün yaygındır, farklı teknolojiler kullandığınızda, mikro hizmet için dağıtım betikleri her ikisinin de ölçeğini genişletebilmesi gerekir. Bu ayrımı da çevikliği ve esnekliği, olduğundan tümünün aynı anda yükseltmek zorunda kalmadan mikro hizmetlerin bazılarını yükseltebilirsiniz.

Kısa bir süre için tek parça ve mikro hizmetler yaklaşım bizim karşılaştırması şimdi geri dönün. Bu diyagram durumu depolamak üzere yaklaşımları farklılıkları gösterir:

#### <a name="state-storage-for-the-two-approaches"></a>Durum depolama için iki yaklaşım

![Service Fabric platform durumu depolama][Image2]

***Tek parçalı bir yaklaşım, tek bir veritabanı ve belirli teknolojileri katmanlarını sol tarafta, sahiptir.***

***Mikro hizmetler yaklaşımı, sağ tarafta, burada durumu genellikle mikro hizmet için kapsamlı ve çeşitli teknolojileri kullanılan birbirine mikro hizmetlerin bir grafik vardır.***

Tek parçalı bir yaklaşım, uygulama genellikle tek bir veritabanı kullanır. Bir veritabanı kullanmanın avantajı dağıtmayı kolaylaştıran tek bir konumda, olmasıdır. Her bileşenin durumunu depolamak için tek bir tabloyu olabilir. Kesinlikle bir sorundur durumu, ayrı takımlar gerekir. Kaçınılmaz olarak, var olan bir müşteri tabloya bir sütun ekleyin, tablolar arasında birleştirme yapmanıza ve depolama katmanında bağımlılıklar oluşturma isteği biri. Bu gerçekleştikten sonra tek tek bileşenler ölçeklendirilemez.

Mikro hizmetler yaklaşımı, her hizmetin yönetir ve kendi durumunu depolar. Her hizmet, hem kod hem de durum birlikte servis taleplerini karşılamak için ölçekleme için sorumludur. Bir dezavantajı, görünümleri ya da sorguları, uygulamanızın verilerini oluşturmak ihtiyacınız olduğunda, birden çok durum deposu arasında sorgu olmanızdır. Bu sorun genellikle bir görünüm üzerinde mikro hizmetler koleksiyonu oluşturan ayrı bir mikro hizmet tarafından çözüldü. Veriler üzerinde birden fazla hazırlıksız sorguları çalıştırmak ihtiyacınız varsa, her bir mikro hizmetin ait verileri bir veri ambarı hizmeti çevrimdışı analiz için yazma düşünmelisiniz.

Mikro hizmetler tutulur. Farklı sürümlerini yan yana çalıştırmak için bir mikro hizmet için mümkündür. Bir mikro hizmet daha yeni bir sürümü, yükseltme sırasında başarısız ve daha önceki bir sürüme geri alınması gerekir. Sürüm oluşturma için faydalı de / B testi nerede farklı bir kullanıcı deneyimi hizmetinin farklı sürümleri. Örneğin, belirli bir kümesi, daha fazla bilgi yaygın olarak sıralı önce yeni işlevleri sınamak için müşterilerin bir mikro hizmet yükseltmek için yaygındır.

### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>İle diğer mikro hizmetler iyi tanımlanmış arabirimlere ve protokolleri üzerinden etkileşim kurar.

Son 10 yıl içinde hizmet odaklı mimari desenleri iletişim açıklayan kapsamlı bilgiler yayımlandı. Genellikle, hizmet iletişimi REST yaklaşım ile HTTP ve TCP protokollerini ve XML veya JSON seri hale getirme biçimi kullanır. Bir arabirim açısından bakıldığında, bu web tasarım yaklaşımı hakkında olur. Ancak, hiçbir şey, ikili protokolleri veya kendi veri biçimleri kullanarak durdurmanız gerekir. Yalnızca kişilerin bu protokollerini ve biçimlerini açık yayımlanmaması kullanılabilir durumda değilse, mikro hizmetler kullanarak daha zor bir zaman sahip olacağını unutmayın.

### <a name="has-a-unique-name-url-used-to-resolve-its-location"></a>Konumu çözümlemek için kullanılan benzersiz bir ad (URL)

Kendi mikro hizmet, çalıştığı her yerde adreslenebilir olması gerekiyor. Makineler hakkında düşünüyorsanız ve hangisinin belirli bir mikro hizmet çalışıyorsa, öğeleri hızlı bir şekilde hatalı gidebilirsiniz.

Geçerli konumuna bulunabilir olmasını sağlama DNS belirli bir makine için belirli bir URL'ye çözümler aynı şekilde, mikro hizmet benzersiz bir adı olmalıdır. Mikro hizmetler, bunlar çalıştırdığınız bilgisayarda altyapısının bağımsız adreslenebilir adları olması gerekir. Bu, bir hizmet kayıt defteri olması gerekir çünkü hizmetinizin nasıl dağıtıldığını ve nasıl bulunduğunda, arasında etkileşim olduğunu belirtir. Bir makine başarısız olduğunda, hizmet nerede taşındı bildirmek kayıt defteri hizmeti gerekiyor.

### <a name="remains-consistent-and-available-in-the-presence-of-failures"></a>Tutarlı ve kullanılabilir oluşturucunun hata olması durumunda kalır.

Beklenmeyen hatalar uğraşmanızı uygulamalarınızdaki sorunları çözmeye özellikle dağıtılmış bir sistemde biridir. Biz geliştiriciler yazdığınız kod çoğunu özel durumları işlemek için olan. Sınama sırasında biz de en çok zaman üzerinde özel durum işleme ayırın. İşlem hataları işlemek için kod yazmaktan daha karmaşıktır. Mikro hizmet üzerinde çalıştığı makinenin başarısız olduğunda ne olur? Sabit bir sorun kendi kendine hatasını algılamak gerekir. Ancak, mikro hizmet yeniden başlatmanız gerekir.

Kullanılabilirlik için bir mikro hizmet hatalarına dayanıklı ve başka bir makinede yeniden başlatmanız mümkün olması gerekir. Bu esneklik gereksinimlerine ek olarak veri kaybı olmaması ve veri tutarlılığını korumanız gerekiyor.

Dayanıklılık, uygulama yükseltme sırasında hatalar meydana geldiğinde elde etmek zordur. Dağıtım sistemiyle çalışan mikro hizmet kurtarmak zorunda değildir. Bu en yeni sürüme ilerlemek tutarlı bir duruma korumak için bir önceki sürüme geri devam edebilir veya olup olmadığını belirlemek gerekir. Yetecek sayıda makine ileri taşıma tutmak kullanılabilir olup olmadığı ve mikro hizmet önceki sürümlerini kurtarmak nasıl gibi birkaç soruyu göz önünde bulundurmanız gerekir. Bu kararlar için sistem durumu bilgileri yaymak üzere mikro hizmet gerekir.

### <a name="reports-health-and-diagnostics"></a>Raporlar durumu ve tanılama

Açıktır, görünebilir ve genellikle kaçan, ancak bir mikro hizmet durumu ve tanılama raporu gerekiyor. Aksi takdirde, sistem durumu işlemleri açısından biraz öngörü gerekir. Tanılama Olayları bağımsız bir hizmetler kümesi arasında ilişkilendirme ve olay siparişin anlamlı makine saat farklarından ilgilenme zorludur. Anlaşılan protokolleri ve veri biçimleri üzerinde bir mikro hizmet ile etkileşimde bulunan aynı şekilde, sistem durumu ve sonuçta sorgulama ve görüntüleme için bir olay deposunda ulaşır tanılama olayları günlüğe kaydetmek nasıl standartlaştırmanız gerekir. Bir mikro hizmetler yaklaşımı ile farklı ekipler tek günlük biçimi kabul etmeniz gerekir. Burada bir bütün olarak uygulamadan tanılama olaylarını görüntüleme için tutarlı bir yaklaşım olması gerekir.

Sistem durumu, Tanılama'ya farklıdır. Geçerli durumuna uygun eylemleri raporlama mikro hizmet hakkında durumudur. İyi bir örnek, kullanılabilirliği sürdürmek için yükseltme ve dağıtım mekanizması ile çalışmaktadır. Bir hizmeti bir işlem kilitlenmesi nedeniyle şu anda sağlam veya makineyi yeniden başlatma da, hizmet işletimsel olabilir. İhtiyacınız olan son şey, bir yükseltmeye başlatmadan tarafından durum daha da kötüsü olmasını sağlamaktır. İlk olarak araştırmanız için en iyi yaklaşımdır veya kurtarmak mikro hizmet için zaman tanıyın. Bir mikro hizmet durumu olayları, bilgiye dayalı kararlar ve kendi kendini onaran hizmetleri oluşturma aslında yardımcı yardımcı olur.

## <a name="guidance-for-designing-microservices-on-azure"></a>Azure'da mikro hizmetler tasarlama için yönergeler 
Yönergeler için Azure Mimari Merkezi ziyaret [tasarlama ve Azure'da mikro hizmetler oluşturma](https://docs.microsoft.com/azure/architecture/microservices/).

## <a name="service-fabric-as-a-microservices-platform"></a>Service Fabric, mikro hizmet platformu olarak

Microsoft Hizmetleri sunmak için genellikle tek parçalı, paketlenmiş ürün teslim geçti, azure Service Fabric belirlendi. Service Fabric oluşturma ve Azure SQL veritabanı ve Azure Cosmos DB gibi büyük hizmetleri çalıştırma deneyimini şeklinde. Diğer hizmetler bu benimsenen gibi platform zaman içinde gelişerek. Service Fabric, yalnızca Azure aynı zamanda tek başına Windows Server dağıtımlarındaki çalıştırması gerekiyordu.

***Takımlar, mikro hizmetler yaklaşımı kullanarak iş sorunlarını çözebilir amacı, Service Fabric oluşturma ve bir hizmet çalıştırma zor sorunları çözmek ve altyapı kaynaklarını verimli bir şekilde kullanmak için olduğundan.***

Service Fabric, mikro hizmetler yaklaşımı sağlayarak kullanan uygulamalar oluşturmanıza yardımcı olur:

* Dağıtmak için sistem hizmetleri sağlayan bir platform yükseltme, algılamanıza ve başarısız hizmetlerini yeniden başlatın, hizmetlerini keşfedin, iletileri yönlendirmek, durumunu yönetme ve durumunu izleyin.
* Kapsayıcılar veya işlemler ya da çalışan uygulamaları dağıtma yeteneği. Service Fabric kapsayıcı ve işlem Düzenleyici ' dir.
* Mikro hizmet uygulamaları oluşturmanıza yardımcı olmak için üretken programlama API'leri: [ASP.NET Core, Reliable Actors ve güvenilir hizmetler](service-fabric-choose-framework.md). Örneğin, sistem durumu ve tanılama bilgileri elde edebilirsiniz veya yerleşik yüksek kullanılabilirlik yararlanabilir.

***Service Fabric hizmetinizin nasıl oluşturacağınız hakkında belirsiz olduğundan ve tüm teknolojileri kullanabilirsiniz. Ancak mikro hizmetler oluşturmayı kolay hale getiren yerleşik programlama API'leri sağlar.***

### <a name="migrating-existing-applications-to-service-fabric"></a>Var olan Service Fabric uygulamalarını geçirme

Service Fabric, mevcut kodu yeniden kullanmak ve yeni mikro hizmetler ile modernleştirin olanak tanır. Uygulama modernizasyonu için beş aşaması vardır ve herhangi bir aşamasında başlatıp durdurabilirsiniz. Aşama vardır:

1) Geleneksel monolitik bir uygulamayla başlatın.  
2) Geçirin. Service fabric'te mevcut kodu barındırmak için kapsayıcılar veya konuk yürütülebilir kullanın.  
3) Modernize. Mevcut kapsayıcılı kod yanı sıra yeni mikro hizmetler ekleyin.  
4) Yenilik yapın. Tek parça bir uygulamayı, ihtiyaca göre mikro hizmetler halinde bölün.  
5) Mikro hizmetler uygulamalara dönüştürün. Var olan tek yapılı uygulamaları dönüştürmek veya sıfırdan yeni uygulamalar oluşturun.

![Mikro hizmetlere geçiş][Image3]

Unutmayın, yapabilecekleriniz *başlatma ve durdurma Bu aşamalardan hiçbirini*. Sahip olmadığınız için sonraki adıma devam eden. 

Örneklere bu aşamaların her biri için göz atalım.

**Geçirme**  
İki nedenden dolayı mevcut tek parça uygulamalara kapsayıcılar birçok şirket geçişi:

* Maliyet azaltma, birleştirme ve var olan donanımı kaldırılmasını nedeniyle ya da daha yüksek yoğunlukta çalışan uygulamaların nedeniyle.
* Geliştirme ve operasyon arasında tutarlı dağıtım sözleşme.

Maliyet indirimleri basittir. Microsoft'ta birçok mevcut uygulamaları, milyonlarca dolar tasarruf içinde için önde gelen kapsayıcıya alınmış. Tutarlı dağıtım, değerlendirilecek daha zor ama eşit oranda önemli değildir. Geliştiriciler, bunları uygun teknolojileri seçebilirsiniz, ancak operations uygulamaları dağıtma ve yönetme için yalnızca tek bir yöntem kabul anlamına gelir. Bu, farklı teknoloji, geliştiricilerin yalnızca belirli olanları seçmeyi zorlamadan destekleme karmaşıklığı ile uğraşmak zorunda işlemlerini azaltır. Esas olarak, her uygulama kendi başına dağıtım görüntüleri olarak kapsayıcılı hale.

Birçok kuruluşun burada durabilir. Zaten sahip oldukları kapsayıcılarının avantajları ve Service Fabric, dağıtım, yükseltmeler, sürüm oluşturma, geri alma işlemleri ve sistem durumu izleme dahil olmak üzere tam yönetim deneyimi sağlar.

**Modernize**  
Modernizasyonu, mevcut kapsayıcı kodu yanı sıra yeni hizmetlerin ektir. Yeni kod yazmak için kullanacaksanız, mikro hizmetler yolunu küçük adımlar atın en iyisidir. Bu, yeni REST API uç noktası ya da yeni bir iş mantığı ekleme gelebilir. Bu şekilde, yeni mikro hizmetler oluşturma ve uygulama geliştirme ve bunları dağıtma işlemini başlatın.

**Yenilik yapın**  
Mikro hizmetler yaklaşımı, değişen iş gereksinimlerini karşılar. Bu aşamada, tek parçalı Hizmetleri uygulamasına bölme veya yenilik başlatmaya karar vermeniz gerekir. Bir iş akışı sırası olarak kullandığınız bir veritabanı işleme tıkanıklık olduğunda bir Klasik burada örnektir. İş akışı sayısı arttıkça istekleri gibi iş için ölçek dağıtılması gerekir. Uygulamanın olmaması ölçeklendirmeyle ya da daha sık güncelleştirilmesini, bir mikro hizmet kullanıma bölmek ve yenilik yapın, gereken belirli parçasının yararlanın.

**Mikro hizmetler uygulamalara dönüştürün**  
Bu aşamada, uygulamanızı tamamen oluştuğundan, (veya bölünmüş içine) mikro hizmetler. Bu noktaya ulaşması için mikro hizmetler yolculuğu yaptık. Buradan başlayın, ancak bir mikro hizmetler Bunu yapmak için platform yardımcı olması için ciddi bir yatırım gerektirir.

### <a name="are-microservices-right-for-my-application"></a>Mikro hizmetler sağ Uygulamam için misiniz?

Olabilir. Diğer takımları için iş nedenleri için bulut yapı başladığı gibi Microsoft olarak kaç tanesinin benzer mikro hizmet yaklaşımı avantajları gerçekleşmiş. Bing, örneğin, mikro hizmetler yıllardır kullanıyor. Diğer ekipler, mikro hizmetler yaklaşımı yeni. Takımlar, orada zor sorunları gücü kendi çekirdek alanlarının dışında çözmek için bulundu. Service Fabric hizmetleri oluşturmak için bir teknoloji olarak oldukça yaygınlaştı kazanılan nedeni budur.

Service Fabric amacı, çok pahalı yönelik çalışmalarımızı gitmek zorunda olmadığınız bir mikro hizmet uygulamaları oluşturmak, karmaşıklık azaltmaktır. Küçükten başlayabilir, gerektiğinde ölçeklendirin, hizmetleri kullanımdan, yenilerini ekleyin ve evrim Geçiren ile müşteri kullanım. Mikro hizmetler Çoğu geliştirici için daha erişilebilir hale getirmek için henüz çözülmesi gereken birçok diğer sorunları olduğunu biliyoruz. Kapsayıcılar ve aktör programlama modeli küçük adımlar bu yönde örnekleridir. Daha fazla yenilik, mikro hizmetler yaklaşımı kolaylaştırmak için çıkacaktır emin ediyoruz.


## <a name="next-steps"></a>Sonraki adımlar

* [Mikro hizmetler: Bulut tarafından desteklenen bir uygulama Devrimi](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/)
* [Azure Mimari Merkezi: Azure'da mikro hizmetler oluşturma](https://docs.microsoft.com/azure/architecture/microservices/)
* [Azure Service Fabric uygulama ve küme için en iyi yöntemler](service-fabric-best-practices-overview.md)
* [Service Fabric terminolojiye genel bakış](service-fabric-technical-overview.md)

[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
[Image3]: media/service-fabric-overview-microservices/microservices-migration.png
