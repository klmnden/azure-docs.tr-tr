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
ms.date: 04/25/2019
ms.author: atsenthi
ms.openlocfilehash: feb82d2abb756d636aeb77042cc817b7b05f6b0c
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233671"
---
# <a name="why-a-microservices-approach-to-building-applications"></a>Neden mikro hizmetler yaklaşımı uygulamaları oluşturmak için?

Yazılım geliştiriciler olarak, bir uygulama bileşeni parçalara hesaba katacak şekilde mi düşünüyorsunuz yeni bir şey değildir. Genellikle, katmanlı bir yaklaşım, bir arka uç depolama, orta katman iş mantığı ve bir ön uç kullanıcı arabirimi (UI) ile alınır. Hangi *sahip* biz geliştiriciler olarak, bulut için dağıtılmış uygulamalar oluşturuyorsanız olan son birkaç yılda değiştirildi.

Değişen iş gereksinimleri şunlardır:

* Oluşturulur ve yeni coğrafi bölgelerdeki müşterilere ölçekte çalıştırılan bir hizmeti.
* Özellikler ve yetenekler, Müşteri taleplerine Çevik bir şekilde yanıt verebilmesi için daha hızlı teslim.
* Maliyetleri azaltmak için geliştirilmiş kaynak kullanımı'nı tıklatın.

Bu iş ihtiyaçları etkileyen *nasıl* uygulamaları ekliyoruz.

Azure'da mikro hizmetler yaklaşımı hakkında daha fazla bilgi için okuma [mikro hizmetler: Bulut tarafından desteklenen bir uygulama Devrimi](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="monolithic-vs-microservice-design-approach"></a>Tek parçalı mikro hizmet yaklaşımı karşılaştırması

Uygulamalar zamanla gelişmesinin. Kişiler için yararlı olan başarılı uygulamalar geliştirin. Başarısız uygulamalar değil evrim geçiren ve sonunda kullanım dışı bırakılmıştır. Soru da şudur: Ne kadar gereksinimleriniz hakkında bugün tanıdığınız ve ne, gelecekte olacak? Örneğin, bir departman için bir raporlama uygulaması oluşturuyorsanız varsayalım. Uygulamayı yalnızca şirketiniz kapsamında geçerlidir ve raporları kısa süreli olduğundan emin olursunuz. Söyleyin tercih ettiğiniz yaklaşımı, farklı olduğundan, bir hizmet oluşturma için on milyonlarca müşteriye video içeriği sunar.

Bazı durumlarda, kavram kanıtı olarak bir kullanıma alma uygulama daha sonra tasarlanması bilerek sürüş, faktördür. Hiçbir zaman kullanılan bir şey aşırı mühendislik içinde çok az noktası yok. Şirketler için bulut yapı hakkında konuşurken, diğer taraftan, büyüme ve kullanım beklenir. Büyüme ve ölçek öngörülemeyen sorunudur. Hızlı bir şekilde gelecekteki işleyebilen bir yolda olduğunu da bilerek prototip kullanabilmek istiyorsunuz. Yalın başlangıç yaklaşım budur: oluşturun, ölçün, öğrenin ve yineleme.

İstemci-sunucu dönemi sırasında size her katmanında belirli teknolojileri kullanarak katmanlı uygulamalar oluşturmaya odaklanın tended. Terim *tek parça* uygulama için bu yaklaşımları ortaya çıktı. Katmanlar arasında olacak şekilde arabirimleri tended ve daha sıkı şekilde bağlı bir tasarım, her bir katmanında bileşenleri arasında kullanıldı. Geliştiriciler tasarlanmış ve kitaplıklara derlenmiş ve birkaç yürütülebilir dosyaları ve dll birbirine üretilen sınıfları.

Bu tür bir tek yapılı tasarım yaklaşımın avantajları vardır. Genellikle tasarlamak daha basit olduğundan ve bu çağrıları genellikle işlemler arası iletişim (IPC) üzerinde olduğundan bileşenleri arasında daha hızlı çağrıları vardır. Ayrıca, herkesin daha fazla kişi-kaynak etkili olma eğilimindedir tek bir ürün sınar. Dezavantajı, katmanlı katmanlar arasında sıkı bir bağ vardır ve tek tek bileşenler ölçeklendirilemez ' dir. Düzeltmeleri veya yükseltmeler yapmanız gereken, diğerleri kendi test bitmesini beklemek zorunda. Çevik olun daha zordur.

Mikro hizmetler bu downsides adres ve önceki iş gereksinimleriyle daha yakından hizalama, ancak onların avantajları ve Borçlar hem de vardır. Mikro hizmetler avantajları, her biri genellikle ölçeği artırın veya azaltın, test etmek, dağıtmak ve bağımsız olarak yönetmek daha basit iş işlevselliğini Kapsüller ' dir. Bir mikro hizmet yaklaşımı önemli yararlarından biri, takımlar tarafından iş senaryolarını daha fazla bilgi teknolojisi tabanlı ' dir. Uygulama, daha küçük takımlar müşteri senaryo temel alınarak bir mikro hizmet geliştirin ve tercih ettikleri herhangi teknolojileri kullanın.

Diğer bir deyişle, kuruluş, mikro hizmet uygulamalarını korumak için teknik standart hale getirmek gerekmez. Ayrı takımlar kendi Hizmetleri algılama için bunları team uzmanlık düzeyine göre yapar veya sorunu çözmek en uygun ne yapabilirsiniz. Uygulamada, belirli bir NoSQL gibi önerilen teknoloji kümesi depolamak veya web uygulama çerçevesi, tercih edilir.

Mikro hizmetler dezavantajı ayrı varlıklar sayısının artması yönetmek ve daha karmaşık dağıtımlar ve sürüm oluşturma ile ilgili olarak gelir. Karşılık gelen ağ gecikmeleri yanı sıra mikro hizmetler arasındaki ağ trafiğini artırır. Sık iletişim kuran, ayrıntılı hizmetler çok sayıda performans onarımı kabus için bir tarif ' dir. Görünüm yardımcı olan araçlar bu bağımlılıklar "sisteminin tamamını görmek" zordur.

İletişim kurma kabul etmiş ve yalnızca bir hizmetten gereken şeyleri dayanıklı iş yerine katı standartlar olun mikro hizmet yaklaşımı sözleşme. Güncelleştirme Hizmetleri birbirinden çünkü tasarımında, bu sözleşme Önden tanımlamak önemlidir. Bir mikro hizmetler yaklaşımı tasarlama "ayrıntılı hizmet odaklı mimari (SOA)." için de başka bir açıklaması

***En basit haliyle, mikro hizmetler yaklaşımı hizmetlerinin her bağımsız değişiklikleri ve anlaşılan standartları ile iletişim için ayrılmış bir Federasyon hakkındadır.***

Daha fazla bulut uygulaması üretilen gibi genel uygulama bu ayrıştırma bağımsız, senaryo odaklı hizmetlerine uzun vadeli daha iyi bir yaklaşım olan kişiler keşfettik.

## <a name="comparison-between-application-development-approaches"></a>Uygulama Geliştirme yaklaşımları arasında karşılaştırma

![Service Fabric platform uygulaması geliştirme][Image1]

1) Monolitik bir uygulamayı etki alanına özel işlevler içerir ve bu normalde web, iş ve veriler gibi işlevsel katmanları bölünür.

2) Birden çok sunucu/sanal makinelerde kopyalayarak monolitik bir uygulamayı ölçeklendirme/kapsayıcıları.

3) Bir mikro hizmet uygulama işlevselliğini farklı daha küçük hizmetler halinde ayırır.

4) Dağıtarak out mikro hizmetler yaklaşımı ölçekler her bağımsız olarak, bu hizmetlerin örneklerini sunucuları/sanal makinelerde oluşturma hizmet/kapsayıcıları.

Bir mikro hizmet tasarlama yaklaşım tüm projeleri için her derde deva değildir, ancak daha önce açıklanan iş hedefleri ile hizalamaya daha yakından. Tek parçalı bir yaklaşım ile başlayan kodu tasarımından mikro hizmet daha sonra yeniden fırsatı olacağını bildiğiniz kabul edilebilir olabilir. Daha sık monolitik bir uygulamayla başlar ve daha ölçeklenebilir ve daha Çevik olması gereken işlevsel alanları ile başlangıç aşamasında, yavaş bölün.

Mikro hizmet yaklaşımı, birçok küçük hizmetler uygulamanızı oluşturma anlamına gelir. Bu hizmetler, makine kümesi dağıtılan kapsayıcılarında çalıştırın. Küçük takımlar senaryoyu odaklanan bir hizmeti geliştirme ve test bağımsız olarak sürümü, dağıtmanızı ve böylece uygulamanın tamamı geliştirebilirsiniz her hizmet ölçeklendirmenizi.

## <a name="what-is-a-microservice"></a>Bir mikro hizmet nedir?

Mikro hizmetler farklı tanımları vardır. Bununla birlikte, mikro hizmetler, aşağıdaki özelliklerin en yaygın olarak kabul edilir:

* Bir müşteri veya iş senaryosu kapsüller. Sorun çözme nedir?
* Küçük bir mühendislik ekibi tarafından geliştirilmiştir.
* Birinde yazılan programlama dilini ve herhangi bir çerçeveyi kullanın.
* Kodu oluşur ve durum (isteğe bağlı olarak), ikisi için de dağıtılır ve ölçeği birbirinden bağımsız sürümlere.
* İle diğer mikro hizmetler, iyi tanımlanmış arabirimlere ve protokolleri üzerinden etkileşim kurun.
* Kendi konumu çözümlemek için kullanılan benzersiz adlar (URL).
* Tutarlı ve kullanılabilir oluşturucunun hata olması durumunda kalır.

Özet:

***Mikro hizmet uygulamaları, iyi tanımlanmış arabirimlere sahip standart protokoller üzerinden birbiriyle iletişim kuran küçük, birbirinden bağımsız sürümlere ve ölçeklenebilir müşteri odaklı hizmetler oluşur.***

### <a name="written-in-any-programming-language-and-use-any-framework"></a>Birinde yazılan programlama dilini ve herhangi bir çerçeveyi kullanın

Geliştiriciler olarak, bir dil veya, bizim becerilerine veya hizmet gereksinimlerine bağlı olarak istiyoruz framework seçmek ücretsiz olmasını istiyoruz. Bazı hizmetler tüm başka C++ performans avantajlarını değeri. Diğer hizmetlerde, C# veya Java yönetilen geliştirme kolaylığı en önemli olabilir. Bazı durumlarda, belirli bir iş ortağı kitaplığı, veri depolama teknolojisinin veya istemcilere hizmet kullanıma sunma yolu kullanmanız gerekebilir.

Sonra teknoloji ve gelen işletimsel veya yaşam döngüsü yönetim ve hizmet ölçeklendirme seçtiniz.

### <a name="allows-code-and-state-to-be-independently-versioned-deployed-and-scaled"></a>Kod ve birbirinden bağımsız sürümlere olması durumuna dağıtılan ve ölçeği sağlar.

Nasıl, mikro hizmetler, kod ve isteğe bağlı olarak durumu yazmak seçtiğiniz ne olursa olsun, bağımsız olarak dağıtma, yükseltme ölçeklendirin ve. Bu, kendi seçtiğiniz teknolojilerle gelir, çünkü çözmek için daha zor sorunları biridir. Ölçeklendirme, anlamak için nasıl bölüm (veya parça) hem kod hem de durumu zor olur. Kod ve durum bugün yaygındır, ayrı teknolojiler kullandığınızda, mikro hizmet için dağıtım betikleri her ikisinin de ölçeğini genişletebilmesi gerekir. Mikro hizmetlerin bazılarını tümünün aynı anda yükseltmek zorunda kalmadan yükseltebilmek için aynı zamanda çevikliği ve esnekliği, budur.

Bir süre dönme tek parçalı mikro hizmet yaklaşımı yerine, aşağıdaki diyagramda için durumu depolamak üzere bir yaklaşım farklılıkları gösterir.

#### <a name="state-storage-between-application-styles"></a>Durum depolama arasındaki uygulama stilleri

![Service Fabric platform durumu depolama][Image2]

***Sol taraftaki tek parçalı bir yaklaşım, tek bir veritabanı ve belirli teknolojileri katmanlarını sahiptir.***

***Mikro hizmetler yaklaşımı sağdaki burada durumu genellikle mikro hizmet için kapsamlı ve çeşitli teknolojileri kullanılan birbirine mikro hizmetlerin bir grafik vardır.***

Tek parçalı bir yaklaşım, uygulama genellikle tek bir veritabanı kullanır. Avantajı dağıtmayı kolaylaştıran tek konuma, olmasıdır. Her bileşenin durumunu depolamak için tek bir tabloyu olabilir. Kesinlikle bir sorundur durumu, ayrı takımlar gerekir. Kaçınılmaz olarak var olan bir müşteri tabloya yeni bir sütun ekleyin, tablolar arasında birleştirme yapmak ve depolama katmanında bağımlılıklar oluşturma temptations vardır. Bu gerçekleştikten sonra tek tek bileşenler ölçeklendirilemez.

Mikro hizmetler yaklaşımı, her hizmetin yönetir ve kendi durumunu depolar. Her hizmet, hem kod hem de durum birlikte servis taleplerini karşılamak için ölçekleme için sorumludur. Bir dezavantajı görünümleri ya da sorguları, uygulamanızın verilerini oluşturmak için ihtiyaç olduğunda, birden çok durum deposu arasında sorgu olmanızdır. Genellikle, bu mikro hizmetler koleksiyonu bir görünüm oluşturan ayrı bir mikro hizmet sağlayarak çözülür. Veriler üzerinde birden fazla hazırlıksız sorguları gerçekleştirmek gerekiyorsa, her bir mikro hizmet çevrimdışı analiz için veri ambarı hizmeti verilerini yazma düşünmelisiniz.

Mikro hizmetler tutulan ve bir mikro hizmet farklı sürümlerini yan yana çalışabilir mümkündür. Bir mikro hizmet daha yeni bir sürümünü yükseltme sırasında başarısız ve önceki bir sürüme geri alınması gerekir. Sürüm oluşturma, A/B-stili, burada hizmet farklı sürümlerini farklı kullanıcılar deneyimi test etmek için yararlıdır. Örneğin, belirli bir kümesi, daha fazla bilgi yaygın olarak sıralı önce yeni işlevleri sınamak için müşterilerin bir mikro hizmet yükseltmek için yaygındır.

### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>İle diğer mikro hizmetler iyi tanımlanmış arabirimlere ve protokolleri üzerinden etkileşim kurar.

Son 10 iletişim düzenleri açıklayan yıllar içinde hizmet odaklı mimari hakkında kapsamlı belgeleri yayımlandı. Genellikle, hizmet iletişimi REST yaklaşım ile HTTP ve TCP protokollerini ve XML veya JSON seri hale getirme biçimi kullanır. Bir arabirim açısından bakıldığında, bu web tasarım yaklaşımı benimsemenin hakkında olur. Ancak, hiçbir şey, ikili protokolleri veya kendi veri biçimleri kullanarak durdurur. Bu protokollerini ve biçimlerini açık yayımlanmaması mevcut değilse, mikro hizmetler kullanarak daha zor bir kez kişilerin için hazırlıklı olmalıdır.

### <a name="has-a-unique-name-url-used-to-resolve-its-location"></a>Konumu çözümlemek için kullanılan benzersiz bir ad (URL)

Kendi mikro hizmet, çalıştığı her yerde adreslenebilir olması gerekiyor. Makineler hakkında düşünüyor ve hangisinin belirli bir mikro hizmet çalışıyorsa, öğeleri hızlı bir şekilde hatalı gidin.

Geçerli konumuna bulunabilir olmasını sağlama DNS belirli bir makine için belirli bir URL'ye çözümler aynı şekilde, mikro hizmet benzersiz bir adı olmalıdır. Mikro hizmetler, çalıştırıldıkları altyapının bağımsız adreslenebilir adları olması gerekir. Bu, bir hizmet kayıt defteri olması gerekir çünkü hizmetinizin nasıl dağıtıldığını ve nasıl bulunduğunda, arasında etkileşim olduğunu belirtir. Bir makine başarısız olduğunda, kayıt defteri hizmeti hizmet nerede taşındı söylemeniz gerekir.

### <a name="remains-consistent-and-available-in-the-presence-of-failures"></a>Tutarlı ve kullanılabilir oluşturucunun hata olması durumunda kalır.

Beklenmeyen hatalar uğraşmanızı uygulamalarınızdaki sorunları çözmeye özellikle dağıtılmış bir sistemde biridir. Biz geliştiriciler yazdığınız kod çoğunu özel durumları işleme ve ayrıca test en çok zaman nerede harcandığını budur. Sorun, hataları işlemek için kod yazmaktan daha karmaşıktır. Mikro hizmet çalıştırıldığı makine başarısız olduğunda ne olur? Yalnızca (sabit sorun kendi kendine) hatasını algılamak gerekir, ancak Ayrıca, mikro hizmet yeniden başlatmanız gerekir.

Kullanılabilirliği nedeniyle, bir mikro hizmet hatalarına dayanıklı ve başka bir makinede yeniden başlatmanız mümkün olması gerekir. Dayanıklı olan çalışan kodun yanı sıra veri kaybını olmamalıdır ve veri tutarlılığını korumanız gerekiyor.

Dayanıklılık, uygulama yükseltme sırasında hatalar meydana geldiğinde elde etmek zordur. Dağıtım sistemiyle çalışan mikro hizmet kurtarmak zorunda değildir. En yeni sürüme ilerlemek veya bunun yerine bir önceki sürüme tutarlı bir duruma sürdürmek için geri sürdürebilirsiniz olmadığını karar vermeniz gerekir. Yetecek sayıda makine ileri taşıma tutmak kullanılabilir olup olmadığı ve mikro hizmet önceki sürümlerini kurtarmak nasıl gibi soruları göz önünde bulundurulması gerekir. Bu, bu kararları için sistem durumu bilgileri yaymak üzere mikro hizmet gerektirir.

### <a name="reports-health-and-diagnostics"></a>Raporlar durumu ve tanılama

Belirgin, görünebilir ve genellikle kaçan, ancak bir mikro hizmet durumu ve tanılama bildirilmesi gerekir. Aksi takdirde, sistem durumu işlemleri açısından çok az bir anlayış yoktur. Tanılama Olayları bağımsız bir hizmetler kümesi arasında ilişkilendirme ve olay siparişin anlamlı makine saat farklarından ilgilenme zorludur. Anlaşılan protokolleri ve veri biçimleri üzerinde bir mikro hizmet ile etkileşimde bulunan aynı şekilde, sistem durumu ve sonuçta sorgulama ve görüntüleme için bir olay deposunda ulaşır tanılama olayları günlüğe kaydetmek nasıl standartlaştırmanız gerekir. Bir mikro hizmetler yaklaşımı sahip tuş, farklı ekipler, tek bir günlük biçimi kabul etmiş olursunuz. Burada bir bütün olarak uygulamadan tanılama olaylarını görüntüleme için tutarlı bir yaklaşım olması gerekir.

Sistem durumu, Tanılama'ya farklıdır. Geçerli durumuna uygun eylemleri raporlama mikro hizmet hakkında durumudur. İyi bir örnek, kullanılabilirliği sürdürmek için yükseltme ve dağıtım mekanizması ile çalışmaktadır. Hizmet şu anda bir işlem kilitlenmesi nedeniyle sağlıksız veya makineyi yeniden başlatma, ancak hizmet işletimsel olabilir. İhtiyacınız olan son bir şey bu yükseltme gerçekleştirerek yarışacağından olmasını sağlamaktır. İlk araştırma yapmak için en iyi yaklaşımdır veya kurtarmak mikro hizmet için zaman tanıyın. Bir mikro hizmet durumu olayları, bilgiye dayalı kararlar ve kendi kendini onaran hizmetleri oluşturma aslında yardımcı yardımcı olur.

## <a name="microservices-design-guidance-on-azure"></a>Azure'da mikro hizmet Tasarım Kılavuzu
Azure Mimari Merkezi için tasarım yönergeleri ziyaret [Azure'da mikro hizmetler oluşturma](https://docs.microsoft.com/azure/architecture/microservices/)

## <a name="service-fabric-as-a-microservices-platform"></a>Service Fabric, mikro hizmet platformu olarak

Microsoft Hizmetleri sunmak için genellikle tek parçalı stili, paketlenmiş ürün teslim geçti, azure Service Fabric belirlendi. Service Fabric oluşturma ve Azure SQL veritabanı ve Azure Cosmos DB gibi büyük hizmetleri çalıştırma deneyimini şeklinde. Bunu daha da fazla Hizmetleri benimsenen gibi platform zaman içinde gelişerek. Service Fabric, yalnızca Azure aynı zamanda tek başına Windows Server dağıtımlarındaki çalıştırması gerekiyordu.

***Takımlar bir mikro hizmet yaklaşımı kullanarak iş sorunlarını çözebilir amacı, Service Fabric oluşturma ve bir hizmet çalıştırma zor sorunları çözmek ve altyapı kaynaklarını verimli bir şekilde kullanmayı, böylelikle.***

Service Fabric, mikro hizmetler yaklaşımı sağlayarak kullanan uygulamalar oluşturmanıza yardımcı olur:

* Dağıtmak için sistem hizmetleri sağlayan bir platform yükseltme, algılamanıza ve başarısız hizmetlerini yeniden başlatın, hizmetlerini keşfedin, iletileri yönlendirmek, durumunu yönetme ve durumunu izleyin.
* Kapsayıcılar veya işlemler ya da çalışan uygulamaları dağıtma yeteneği. Service Fabric kapsayıcı ve işlem Düzenleyici ' dir.
* Mikro hizmet uygulamaları oluşturmanıza yardımcı olmak için üretken programlama API'leri: [ASP.NET Core, Reliable Actors ve güvenilir hizmetler](service-fabric-choose-framework.md). Örneğin, sistem durumu ve tanılama bilgileri elde edebilirsiniz veya yerleşik yüksek kullanılabilirlik yararlanabilir.

***Service Fabric hizmetinizin nasıl oluşturulacağına belirsiz olduğundan ve tüm teknolojileri kullanabilirsiniz. Bununla birlikte, mikro hizmetler oluşturmayı kolay hale getiren yerleşik programlama API'leri sağlar.***

### <a name="migrating-existing-applications-to-service-fabric"></a>Var olan Service Fabric uygulamalarını geçirme

Service Fabric, ardından yeni mikro hizmetler ile modernleştirdiğini mevcut kodu yeniden olanak sağlar. Uygulama modernizasyonu için beş aşaması vardır ve başlatabilir ve aşamalar birini durdurun. Bunlar:

1) Geleneksel monolitik bir uygulamayla başlatın.  
2) Kaldırma ve kaydırma - Service fabric'te mevcut kodu barındırmak için kapsayıcılar veya konuk yürütülebilir dosyaları'nı kullanın.  
3) Modernizasyonu - mevcut kapsayıcılı koduyla birlikte eklenen yeni mikro hizmetler.  
4) Yenilik yapma - tek parçalı tamamen ihtiyaca göre mikro hizmetler halinde bölün.  
5) Mikro hizmetlere - sıfırdan yeni uygulamalar oluşturmak veya var olan tek yapılı uygulamaları dönüşümü dönüştürülür.

![Mikro hizmetlere geçiş][Image3]

Erişebildiğiniz yeniden vurgulamak önemlidir **başlatma ve durdurma Bu aşamalardan hiçbirini**. Sonraki aşamasına ilerlemek için etmemiz değil. Artık örneklere bu aşamaların her biri için göz atalım.

**Lift- and -Shift**  
Şirket çok sayıda kaldırarak ve var olan tek yapılı uygulamaları kapsayıcılara iki nedenden dolayı kaydırma:

* Maliyet azaltma nedeniyle birleştirme ve temizleme çalıştıran donanım ya da mevcut uygulamaların en yüksek yoğunluklu.
* Geliştirme ve operasyon arasında tutarlı dağıtım sözleşme.

Maliyet indirimleri anlaşılır ve Microsoft bünyesinde, mevcut uygulamaları çok sayıda yalnızca milyonlarca dolar tasarruf için kapsayıcıya alınmış. Tutarlı dağıtım, değerlendirilecek daha zor, ancak eşit oranda önemli değildir. Bu geliştiricilerin hala işlemleri yalnızca bu uygulamaları dağıtmak ve yönetmek için tek bir yolu kabul eder ancak onlara uygun teknolojiyi seçin ücretsiz olabileceği anlamına gelir. Bu, birçok farklı teknoloji karmaşıklığı ile uğraşmak zorunda ya da yalnızca belirli olanları seçmeyi geliştiricilerin zorlama işlemlerini azaltır. Temelde her uygulama kendi başına dağıtım görüntüleri olarak kapsayıcılı hale.

Birçok kuruluşun burada durabilir. Zaten sahip oldukları kapsayıcılarının avantajları ve Service Fabric, dağıtım, yükseltmeler, sürüm oluşturma, geri alma işlemleri, sistem durumu izleme vb. tam yönetim deneyimi sağlar.

**Modernizasyonu**  
Varolan yanı sıra yeni hizmetler eklenen kod kapsayıcıya alınmış. Yeni kod yazmak için kullanacaksanız, mikro hizmetler yolunu küçük adımlar atın karar en iyisidir. Bu, yeni REST API uç noktası veya yeni bir iş mantığı ekleme. Bu şekilde, yeni mikro hizmetler oluşturma ve uygulama geliştirme ve bunları dağıtma yolculukta başlatın.

**Yenilik yapın**  
Mikro hizmetler yaklaşımı, değişen iş gereksinimlerini karşılar. Bu aşamada, tek parçalı uygulama hizmetleri veya yenilik bölme Başlat gerekip gerekmediğini kararıdır. Bir iş akışı sırası olarak kullanılan bir veritabanı işleme tıkanıklık olduğunda bir Klasik burada örnektir. İş akışı sayısı arttıkça istekleri gibi iş için ölçek dağıtılması gerekir. Belirli olan İnceleyenleri olmaması ölçeklendirmeyle veya, daha sık güncelleştirilmesi gerekiyor uygulamanın bu mikro hizmete bölmek ve yenilik yapın.

**Mikro hizmetler halinde dönüştürdü**  
Bu, uygulamanızın tam olarak oluştuğundan, (veya bölünmüş içine) olduğu, mikro hizmetler. Buraya ulaşmak için mikro hizmetler yolculuğu yapıldı. Buradan başlayın, ancak bir mikro hizmetler Bunu yapmak için yardımcı olması için ciddi bir yatırım platformudur.

### <a name="are-microservices-right-for-my-application"></a>Mikro hizmetler sağ Uygulamam için misiniz?

Olabilir. Biz deneyimli iş nedenleriyle bulut oluşturmak giderek daha fazla Microsoft teams'de başlangıcından gibi çoğu bir mikro hizmet benzeri yaklaşımı avantajları gerçekleşen oluştu. Örneğin, Bing, yıl boyunca mikro hizmetler kullanarak geliştirmektedir. Diğer ekipler, mikro hizmetler yaklaşımı yeni. Takımlar, orada zor sorunları gücü kendi çekirdek alanlarının dışında çözmek için bulundu. Service Fabric hizmetleri oluşturmak için tercih ettiğiniz bir teknoloji olarak oldukça yaygınlaştı kazanılan nedeni budur.

Service Fabric amacı, çok yüksek maliyetli yönelik çalışmalarımızı gitmek zorunda değil bir mikro hizmet uygulamaları oluşturmak, karmaşıklık azaltmaktır. Küçükten başlayabilir, gerektiğinde ölçeklendirin, hizmetleri kullanımdan, yenilerini ekleyin ve evrim Geçiren ile müşteri kullanım. Mikro hizmetler Çoğu geliştirici için daha erişilebilir hale getirmek için henüz çözülmesi gereken birçok diğer sorunları olduğunu biliyoruz. Kapsayıcılar ve aktör programlama modeli bu yöndeki küçük adımlara ilişkin örnekler ve bunu kolaylaştırmak için daha fazla yenilik çıkacaktır emin duyuyoruz.


## <a name="next-steps"></a>Sonraki adımlar

* [Mikro hizmetler: Bulut tarafından desteklenen bir uygulama Devrimi](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/)
* [Azure Mimari Merkezi: Azure'da mikro hizmetler oluşturma](https://docs.microsoft.com/azure/architecture/microservices/)
* [Azure Service Fabric uygulama ve küme için en iyi yöntemler](service-fabric-best-practices-overview.md)
* [Service Fabric terminolojiye genel bakış](service-fabric-technical-overview.md)

[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
[Image3]: media/service-fabric-overview-microservices/microservices-migration.png
