---
title: Azure'da mikro hizmetler giriş | Microsoft Docs
description: Bir genel bakış neden mikro hizmetler yaklaşımı ile bulut uygulamaları geliştirmek için modern uygulama geliştirmenin önemli olduğunu ve nasıl Azure Service Fabric, bunu yapmanın bir platform sağlar.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: fae2be85-0ab4-4cd3-9d1f-e0d95fe1959b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/02/2017
ms.author: msfussell
ms.openlocfilehash: 04342be06430747ef64cb69c27ee93e6896775e5
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39070895"
---
# <a name="why-a-microservices-approach-to-building-applications"></a>Neden mikro hizmetler yaklaşımı uygulamaları oluşturmak için?
Yazılım geliştiricileri yoktur nasıl bir uygulama bileşeni parçalara hesaba katacak şekilde hakkında düşünüyoruz içinde yeni bir şey yok. Nesneye dayalı, yazılım soyutlamaları ve temsilinde merkezi örnektir. Günümüzde, bu factorization sınıfları ve arabirimleri arasında paylaşılan kitaplıklar ve teknoloji katmanları biçiminde eğilimindedir. Genellikle, katmanlı bir yaklaşım, bir arka uç depolama, orta katman iş mantığı ve bir ön uç kullanıcı arabirimi (UI) ile alınır. Hangi *sahip* biz geliştiriciler, oluşturmakta olduğunuz son birkaç yılda değiştirilmiş olan dağıtılmış bulut için uygulamalar ve iş temelli.

Değişen iş gereksinimleri şunlardır:

* Yerleşik olarak bulunur ve müşterilerin yeni coğrafi bölgede (örneğin) erişmek için uygun ölçekte çalışır bir hizmettir.
* Özellikler ve yetenekler, Müşteri taleplerine Çevik bir şekilde yanıt verebilmesi için daha hızlı teslim.
* Maliyetleri azaltmak için geliştirilmiş kaynak kullanımı'nı tıklatın.

Bu iş ihtiyaçları etkileyen *nasıl* uygulamaları ekliyoruz.

Azure'da mikro hizmetler yaklaşımı hakkında daha fazla bilgi için okuma [mikro hizmetler: Bulut tarafından desteklenen bir uygulama Devrimi](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="monolithic-vs-microservice-design-approach"></a>Tek parçalı mikro hizmet yaklaşımı karşılaştırması
Tüm uygulamalar zamanla gelişmesinin. Kişiler için yararlı olan başarılı uygulamalar geliştirin. Başarısız uygulamalar değil evrim geçiren ve sonunda kullanım dışı bırakılmıştır. Soru olur: ne kadar gereksinimleriniz hakkında bugün biliyor musunuz ve ne, gelecekte sunulacak? Örneğin, bir departman için bir raporlama uygulaması oluşturuyorsanız varsayalım. Uygulama, şirketiniz kapsamında kalır ve raporları kısa süreli olduğundan emin olursunuz. Söyleyin tercih ettiğiniz yaklaşımı, farklı olduğundan, bir hizmet oluşturma için on milyonlarca müşteriye video içeriği sunar. 

Bazen, kavram kanıtı olarak bir kullanıma alma bildiğiniz uygulama daha sonra tasarlanması ancak sürükleyici etken olabilir. Hiçbir zaman kullanılan bir şey aşırı mühendislik içinde çok az noktası yok. Bu normal mühendislik dengedir. Şirketler için bulut yapı hakkında konuşurken, diğer taraftan, büyüme ve kullanım beklenir. Büyüme ve ölçek öngörülemeyen sorunudur. Hızlı bir şekilde gelecekteki başarılı bir şekilde ilgilenmenin bir yolu üzerinde olduğunu da bilerek prototip kullanabilmek istiyorsunuz. Yalın başlangıç yaklaşım budur: oluşturun, ölçün, öğrenin ve yineleme.

İstemci-sunucu dönemi sırasında size her katmanında belirli teknolojileri kullanarak katmanlı uygulamalar oluşturmaya odaklanın tended. Terim *tek parça* uygulama için bu yaklaşımları ortaya çıktı. Katmanlar arasında olacak şekilde arabirimleri tended ve daha sıkı şekilde bağlı bir tasarım, her bir katmanında bileşenleri arasında kullanıldı. Geliştiriciler tasarlanmış ve kitaplıklara derlenmiş ve birkaç yürütülebilir dosyaları ve dll birbirine üretilen sınıfları. 

Bu tür bir tek yapılı tasarım yaklaşımın avantajları vardır. Genellikle tasarlamak daha basit olduğundan ve bu çağrıları genellikle işlemler arası iletişim (IPC) olduğundan daha hızlı çağrılar bileşenleri arasındaki vardır. Ayrıca, herkesin daha fazla kişi-kaynak etkili olma eğilimindedir tek bir ürün sınar. Dezavantajı, katmanlı katmanlar arasında sıkı bir bağ vardır ve tek tek bileşenler ölçeklendirilemez ' dir. Düzeltmeleri veya yükseltmeler yapmanız gereken, diğerleri kendi test bitmesini beklemek zorunda. Çevik olun daha zordur.

Mikro hizmetler bu downsides adres ve önceki iş gereksinimleriyle daha yakından hizalama, ancak onların avantajları ve Borçlar hem de vardır. Mikro hizmetler avantajları, her biri genellikle ölçeği artırın veya azaltın, test, dağıtma ve bağımsız olarak yönetmek daha basit iş işlevselliğini Kapsüller ' dir. Bir mikro hizmet yaklaşımı önemli yararlarından biri, takımlar tarafından iş senaryolarını daha fazla katmanlı bir yaklaşımı teşvik eder teknolojisiyle temelli ' dir. Uygulama, daha küçük takımlar müşteri senaryo temel alınarak bir mikro hizmet geliştirin ve tercih ettikleri herhangi teknolojileri kullanın. 

Diğer bir deyişle, kuruluş, mikro hizmet uygulamalarını korumak için teknik standart hale getirmek gerekmez. Ayrı takımlar kendi Hizmetleri algılama için bunları team uzmanlık düzeyine göre yapar veya sorunu çözmek en uygun ne yapabilirsiniz. Uygulamada, belirli bir NoSQL gibi önerilen teknoloji kümesi depolamak veya web uygulama çerçevesi, tercih edilir.

Mikro hizmetler dezavantajı ayrı varlıklar sayısının artması yönetmek ve daha karmaşık dağıtımlar ve sürüm oluşturma ile ilgili olarak gelir. Karşılık gelen ağ gecikmeleri yanı sıra mikro hizmetler arasındaki ağ trafiğini artırır. Sık iletişim kuran, ayrıntılı hizmetler çok sayıda performans onarımı kabus için bir tarif ' dir. Görünüm yardımcı olan araçlar bu bağımlılıklar "sisteminin tamamını görmek" zordur. 

İletişim kurma kabul etmiş ve yalnızca bir hizmetten gereken şeyleri dayanıklı iş yerine katı standartlar olun mikro hizmet yaklaşımı sözleşme. Güncelleştirme Hizmetleri birbirinden çünkü tasarımında, bu sözleşme Önden tanımlamak önemlidir. Bir mikro hizmetler yaklaşımı tasarlama "ayrıntılı hizmet odaklı mimari (SOA)." için de başka bir açıklaması

***En basit haliyle, mikro hizmetler yaklaşımı hizmetlerinin her bağımsız değişiklikleri ve anlaşılan standartları ile iletişim için ayrılmış bir Federasyon hakkındadır.***

Daha fazla bulut uygulaması üretilen gibi kişilerin genel uygulamanın bu ayrıştırma bağımsız, senaryo odaklı hizmetler ile uzun süreli daha iyi bir yaklaşım olduğunu keşfedin.

## <a name="comparison-between-application-development-approaches"></a>Uygulama Geliştirme yaklaşımları arasında karşılaştırma
![Service Fabric platform uygulaması geliştirme][Image1]

1) Monolitik bir uygulamayı etki alanına özel işlevler içerir ve bu normalde web, iş ve veriler gibi işlevsel katmanları bölünür.

2) Birden çok sunucu/sanal makinelerde kopyalayarak monolitik bir uygulamayı ölçeklendirme/kapsayıcıları.

3) Bir mikro hizmet uygulama işlevselliğini farklı daha küçük hizmetler halinde ayırır.

4) Dağıtarak out mikro hizmetler yaklaşımı ölçekler her bağımsız olarak, bu hizmetlerin örneklerini sunucuları/sanal makinelerde oluşturma hizmet/kapsayıcıları.

Bir mikro hizmet tasarlama yaklaşım tüm projeleri için her derde deva değildir, ancak daha önce açıklanan iş hedefleri ile hizalamaya daha yakından. Tek parçalı bir yaklaşım ile başlayan kodu tasarımından mikro hizmet daha sonra yeniden fırsatı olacağını bildiğiniz kabul edilebilir olabilir. Daha sık monolitik bir uygulamayla başlar ve daha ölçeklenebilir ve daha Çevik olması gereken işlevsel alanları ile başlangıç aşamasında, yavaş bölün.

Özetlemek gerekirse, mikro hizmet yaklaşımı birçok küçük hizmetler uygulamanızı oluşturma sağlamaktır. Makine bir kümede dağıtılan kapsayıcılardaki Hizmetleri çalıştırın. Küçük takımlar senaryoyu odaklanan bir hizmeti geliştirme ve test bağımsız olarak sürümü, dağıtmanızı ve böylece uygulamanın tamamı geliştirebilirsiniz her hizmet ölçeklendirmenizi.

## <a name="what-is-a-microservice"></a>Bir mikro hizmet nedir?
Mikro hizmetler farklı tanımları vardır. Internet arama yaparsanız, kendi bakış açılarını ve tanımları sağlayan birçok faydalı kaynaklar bulacaksınız. Bununla birlikte, mikro hizmetler, aşağıdaki özelliklerin en yaygın olarak anlaşmaya varılmış:

* Bir müşteri veya iş senaryosu kapsüller. Sorun çözme nedir?
* Küçük bir mühendislik ekibi tarafından geliştirilmiştir.
* Birinde yazılan programlama dilini ve herhangi bir çerçeveyi kullanın.
* Kodu oluşur ve durum (isteğe bağlı olarak), ikisi için de dağıtılır ve ölçeği birbirinden bağımsız sürümlere.
* İle diğer mikro hizmetler, iyi tanımlanmış arabirimlere ve protokolleri üzerinden etkileşim kurun.
* Kendi konumu çözümlemek için kullanılan benzersiz adlar (URL).
* Tutarlı ve kullanılabilir oluşturucunun hata olması durumunda kalır.

Bu özellikleri uygulamasına özetleyebilir:

***Mikro hizmet uygulamaları, iyi tanımlanmış arabirimlere sahip standart protokoller üzerinden birbiriyle iletişim kuran küçük, birbirinden bağımsız sürümlere ve ölçeklenebilir müşteri odaklı hizmetler oluşur.***

Önceki bölümdeki ilk iki noktaları ele aldığımız ve şimdi biz üzerinde genişletin ve diğerleri açıklamak.

### <a name="written-in-any-programming-language-and-use-any-framework"></a>Birinde yazılan programlama dilini ve herhangi bir çerçeveyi kullanın
Geliştiriciler olarak, biz bir dil veya, bizim becerilerine veya hizmet gereksinimlerine bağlı olarak istiyoruz framework seçmek boş olmalıdır. Bazı hizmetler tüm başka C++ performans avantajlarını değeri. Diğer hizmetlerde, C# veya Java yönetilen geliştirme kolaylığı en önemli olabilir. Bazı durumlarda, belirli bir iş ortağı kitaplığı, veri depolama teknolojisinin veya istemcilere hizmet kullanıma sunma yolu kullanmanız gerekebilir.

Sonra teknoloji ve gelen işletimsel veya yaşam döngüsü yönetim ve hizmet ölçeklendirme seçtiniz.

### <a name="allows-code-and-state-to-be-independently-versioned-deployed-and-scaled"></a>Kod ve birbirinden bağımsız sürümlere olması durumuna dağıtılan ve ölçeği sağlar.
Bununla birlikte, mikro hizmetler, kod ve isteğe bağlı olarak durumu yazmak seçtiğiniz bağımsız olarak dağıtma, yükseltme ölçeklendirin ve. Kendi seçtiğiniz teknolojilerle birlikte gelen gerçekten çözmek için daha zor sorunlarından biri, olmasıdır. Ölçeklendirme, anlamak için nasıl bölüm (veya parça) hem kod hem de durumu zor olur. Kod ve durum bugün yaygındır, ayrı teknolojiler kullandığınızda, mikro hizmet için dağıtım betikleri ikisini ölçeklendirme ile büyümesinin üstesinden gelmek gerekir. Mikro hizmetlerin bazılarını tümünün aynı anda yükseltmek zorunda kalmadan yükseltebilmek için aynı zamanda çevikliği ve esnekliği, budur.

Bir süre dönme tek parçalı mikro hizmet yaklaşımı yerine, aşağıdaki diyagramda için durumu depolamak üzere bir yaklaşım farklılıkları gösterir.

#### <a name="state-storage-between-application-styles"></a>Durum depolama arasındaki uygulama stilleri
![Service Fabric platform durumu depolama][Image2]

***Sol taraftaki tek parçalı bir yaklaşım, tek bir veritabanı ve belirli teknolojileri katmanlarını sahiptir.***

***Mikro hizmetler yaklaşımı sağdaki burada durumu genellikle mikro hizmet için kapsamlı ve çeşitli teknolojileri kullanılan birbirine mikro hizmetlerin bir grafik vardır.***

Tek parçalı bir yaklaşım, uygulama genellikle tek bir veritabanı kullanır. Avantajı dağıtmayı kolaylaştıran tek konuma, olmasıdır. Her bileşenin durumunu depolamak için tek bir tabloyu olabilir. Kesinlikle bir sorundur durumu, ayrı takımlar gerekir. Kaçınılmaz olarak var olan bir müşteri tabloya yeni bir sütun ekleyin, tablolar arasında birleştirme yapmak ve depolama katmanında bağımlılıklar oluşturma temptations vardır. Bu gerçekleştikten sonra tek tek bileşenler ölçeklendirilemez. 

Mikro hizmetler yaklaşımı, her hizmetin yönetir ve kendi durumunu depolar. Her hizmet, hem kod hem de durum birlikte servis taleplerini karşılamak için ölçekleme için sorumludur. Bir dezavantajı görünümleri ya da sorguları, uygulamanızın verilerini oluşturmak için ihtiyaç olduğunda, birbirinden farklı durum depoları arasında sorgu olmanızdır. Genellikle, bu mikro hizmetler koleksiyonu bir görünüm oluşturan ayrı bir mikro hizmet sağlayarak çözülür. Veriler üzerinde birden fazla hazırlıksız sorguları gerçekleştirmek gerekiyorsa, her bir mikro hizmet çevrimdışı analiz için veri ambarı hizmeti verilerini yazma düşünmelisiniz.

Sürüm oluşturma dağıtılmış bir mikro hizmet sürümüne özeldir bu nedenle, birden çok ve farklı sürümleri dağıtın ve yan yana çalıştırın. Sürüm oluşturma burada bir mikro hizmet daha yeni bir sürümünü yükseltme sırasında başarısız oluyor ve önceki bir sürümüne geri almak gereken senaryoları ele alır. Sürüm oluşturma için diğer senaryo nerede hizmet farklı sürümlerini farklı kullanıcılar deneyimi A/B-style testi gerçekleştiriyor. Örneğin, belirli bir kümesi, daha fazla bilgi yaygın olarak sıralı önce yeni işlevleri sınamak için müşterilerin bir mikro hizmet yükseltmek için yaygındır. Mikro hizmetler yaşam döngüsü yönetimi sonra bunu şimdi bunların arasındaki iletişimi için olduk.

### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>İle diğer mikro hizmetler iyi tanımlanmış arabirimlere ve protokolleri üzerinden etkileşim kurar.
Son 10 yılda yayımlanan ve hizmet odaklı mimari hakkında kapsamlı belgeleri açıkladığı desenleri için bu konuda burada küçük ilgilenilmesi gerekiyor. Genellikle, hizmet iletişimi REST yaklaşım ile HTTP ve TCP protokollerini ve XML veya JSON seri hale getirme biçimi kullanır. Bir arabirim açısından bakıldığında, bu web tasarım yaklaşımı benimsemenin hakkında olur. Ancak, hiçbir şey, ikili protokolleri veya kendi veri biçimleri kullanarak durdurur. Bu protokollerini ve biçimlerini açık yayımlanmaması mevcut değilse, mikro hizmetler kullanarak daha zor bir kez kişilerin için hazırlıklı olmalıdır.

### <a name="has-a-unique-name-url-used-to-resolve-its-location"></a>Konumu çözümlemek için kullanılan benzersiz bir ad (URL)
Nasıl biz mikro hizmet yaklaşımı gibi web olduğunu söyleyen tutmak hatırlıyor musunuz? Web gibi mikro hizmet çalıştığı her yerde adreslenebilir olması gerekir. Makineler hakkında düşünüyor ve hangisinin belirli bir mikro hizmet çalışıyorsa, öğeleri hızlı bir şekilde hatalı gidin. 

DNS için belirli bir makinenin belirli bir URL'ye çözümler aynı şekilde, mikro hizmet geçerli konumuna bulunabilir olmasını sağlama benzersiz bir ad olmalıdır. Mikro hizmetler, çalıştırıldıkları altyapının bağımsız hale adreslenebilir adları olması gerekir. Bu, bir hizmet kayıt defteri olması gerekir çünkü hizmetinizin nasıl dağıtıldığını ve nasıl bulunduğunda, arasında etkileşim olduğunu belirtir. Bir makine başarısız olduğunda, eşit olarak, kayıt defteri hizmeti hizmet artık çalıştığı bildirmeniz gerekir. 

Bu bir sonraki konuya geldik: esneklik ve tutarlılık.

### <a name="remains-consistent-and-available-in-the-presence-of-failures"></a>Tutarlı ve kullanılabilir oluşturucunun hata olması durumunda kalır.
Beklenmeyen hatalar uğraşmanızı uygulamalarınızdaki sorunları çözmeye özellikle dağıtılmış bir sistemde biridir. Biz geliştiriciler yazdığınız kod çoğunu özel durumları işleme ve ayrıca test en çok zaman nerede harcandığını budur. Sorun, hataları işlemek için kod yazmaktan daha karmaşıktır. Mikro hizmet çalıştırıldığı makine başarısız olduğunda ne olur? Yalnızca bu mikro hizmet hatası (sabit sorun kendi kendine) algılamak ihtiyacınız, ancak ayrıca bir şey, mikro hizmet yeniden başlatmanız gerekir. 

Bir mikro hizmet hatalara karşı dayanıklı ve kullanılabilirliği nedeniyle başka bir makinede yeniden başlatmanız gerekir. Bu aynı zamanda mikro hizmet adına mikro hizmet bu durumdan kurtarmak ve mikro hizmet başarıyla yeniden başlatmanız mümkün olup olmadığını kaydedilmiş duruma gelir. Diğer bir deyişle, vardır (işlem başlatır) işlem esnekliği ve bunun yanı sıra durumu ya da (veri kaybı olmadan veri kalır ve tutarlı) veri dayanıklılığı olması gerekir.

Dayanıklılık sorunları, ne zaman uygulama yükseltme sırasında hatalar meydana gibi diğer senaryolar sırasında compounded. Dağıtım sistemiyle çalışan mikro hizmet kurtarmak zorunda değildir. Ardından, en yeni sürüme ilerlemek bunun yerine tutarlı bir duruma korumak için önceki bir sürüme geri alma devam edebilir veya olup olmadığını karar vermeniz de gerekir. Yetecek sayıda makine ileri taşıma tutmak kullanılabilir olup olmadığı ve mikro hizmet önceki sürümlerini kurtarmak nasıl gibi soruları göz önünde bulundurulması gerekir. Bu sistem durumu bilgilerinin bu kararları yaymak üzere mikro hizmet gerektirir.

### <a name="reports-health-and-diagnostics"></a>Raporlar durumu ve tanılama
Belirgin, görünebilir ve genellikle kaçan, ancak bir mikro hizmet durumu ve tanılama bildirilmesi gerekir. Aksi takdirde, işlemleri açısından biraz Insight yoktur. Tanılama Olayları bağımsız bir hizmetler kümesi arasında ilişkilendirme ve olay siparişin anlamlı makine saat farklarından ilgilenme zordur. Anlaşılan protokolleri ve veri biçimleri üzerinde bir mikro hizmet ile etkileşimde bulunan aynı şekilde, sistem durumu ve sonuçta sorgulama ve görüntüleme için bir olay deposuna düştüğünden tanılama olayları günlüğe kaydetmek nasıl standartlaştırma gereksinimi görünür. İsteğe bağlı olarak mikro hizmetler yaklaşımı, anahtar olan farklı ekipler'in üzerinde bir tek günlük biçimini kabul etmiş olursunuz. Burada bir bütün olarak uygulamadan tanılama olaylarını görüntüleme için tutarlı bir yaklaşım olması gerekir.

Sistem durumu, Tanılama'ya farklıdır. Geçerli durumuna uygun eylemleri raporlama mikro hizmet hakkında durumudur. İyi bir örnek, kullanılabilirliği sürdürmek için yükseltme ve dağıtım mekanizması ile çalışmaktadır. Hizmet şu anda bir işlem kilitlenmesi nedeniyle sağlıksız veya makineyi yeniden başlatma, ancak hizmet işletimsel olabilir. İhtiyacınız olan son bir şey bu yükseltme gerçekleştirerek yarışacağından olmasını sağlamaktır. İlk araştırma yapmak için en iyi yaklaşımdır veya kurtarmak mikro hizmet için zaman tanıyın. Bir mikro hizmet durumu olayları, bilgiye dayalı kararlar ve kendi kendini onaran hizmetleri oluşturma aslında yardımcı yardımcı olur.

## <a name="service-fabric-as-a-microservices-platform"></a>Service Fabric, mikro hizmet platformu olarak
Gelen geçiş Microsoft tarafından Hizmetleri sunmak için stilinde genellikle tek parçalı, kutusu ürünleri teslim Azure Service Fabric belirlendi. Service Fabric oluşturma ve Azure SQL veritabanı ve Azure Cosmos DB gibi büyük hizmetleri çalıştırma deneyimini şeklinde. Bunu daha da fazla Hizmetleri benimsenen gibi platform zaman içinde gelişerek. Önemlisi, yalnızca Azure aynı zamanda tek başına Windows Server dağıtımlarındaki çalıştırmak Service Fabric vardı.

***Takımlar bir mikro hizmet yaklaşımı kullanarak iş sorunlarını çözebilir amacı, Service Fabric oluşturma ve bir hizmet çalıştırma zor sorunları çözmek ve altyapı kaynaklarını verimli bir şekilde kullanmayı, böylelikle.***

Service Fabric, mikro hizmetler yaklaşımı kullanan uygulamalar oluşturmanıza yardımcı olmak üzere üç geniş alanları sağlar:

* Dağıtmak için sistem hizmetleri sağlayan bir platform yükseltme, algılamanıza ve başarısız hizmetlerini yeniden başlatın, hizmetlerini keşfedin, iletileri yönlendirmek, durumunu yönetme ve durumunu izleyin. Bu sistem hizmetlerini etkin mikro Hizmetleri daha önce açıklanan özelliklerini etkinleştirir.
* Kapsayıcılar veya işlemler ya da çalışan uygulamaları dağıtma yeteneği. Service Fabric kapsayıcı ve işlem Düzenleyici ' dir.
* Mikro hizmet uygulamaları oluşturmanıza yardımcı olmak üzere, üretken programlama API'leri: [ASP.NET Core, Reliable Actors ve Reliable Services](service-fabric-choose-framework.md). Kendi mikro hizmet oluşturmak için herhangi bir kod seçebilirsiniz. Ancak bu API'leri işi daha basit hale ve bunlar daha ayrıntılı bir platform ile tümleştirin. Bu şekilde, örneğin, sistem durumu ve tanılama bilgileri elde edebilirsiniz veya yerleşik yüksek kullanılabilirlik yararlanabilir.

***Service Fabric hizmetinizin nasıl oluşturulacağına belirsiz olduğundan ve tüm teknolojileri kullanabilirsiniz. Bununla birlikte, mikro hizmetler oluşturmayı kolay hale getiren yerleşik programlama API'leri sağlar.***

### <a name="migrating-existing-applications-to-service-fabric"></a>Var olan Service Fabric uygulamalarını geçirme
Bir anahtar Service fabric'e sonra yeni mikro hizmetler ile modernleştirdiğini mevcut kodu yeniden kullanmak için bir yaklaşımdır. Uygulama modernizasyonu için beş aşaması vardır ve başlatabilir ve aşamalar birini durdurun. Bunlar:

1) Geleneksel monolitik bir uygulamayla başlatın.  
2) Kaldırma ve kaydırma - Service fabric'te mevcut kodu barındırmak için kapsayıcılar veya konuk yürütülebilir dosyaları'nı kullanın.  
3) Modernizasyonu - mevcut kapsayıcılı koduyla birlikte eklenen yeni mikro hizmetler.  
4) Yenilik yapma - tek parçalı tamamen ihtiyaca göre mikro hizmetler halinde bölün.  
5) Mikro hizmetlere - sıfırdan yeni uygulamalar oluşturmak veya var olan tek yapılı uygulamaları dönüşümü dönüştürülür.

![Mikro hizmetlere geçiş][Image3]

Erişebildiğiniz yeniden vurgulamak önemlidir **başlatma ve durdurma Bu aşamalardan hiçbirini**. Sonraki aşamasına ilerlemek için etmemiz değil. Artık örneklere bu aşamaların her biri için göz atalım.

**Lift- and -Shift** -şirketler çok sayıda kaldırarak ve var olan tek yapılı uygulamaları kapsayıcılara iki nedenden dolayı kaydırma:

- Maliyet azaltma nedeniyle birleştirme ve temizleme çalıştıran donanım ya da mevcut uygulamaların en yüksek yoğunluklu. 
- Geliştirme ve operasyon arasında tutarlı dağıtım sözleşme.

Maliyet indirimleri anlaşılır ve Microsoft bünyesinde, mevcut uygulamaları çok sayıda yalnızca milyonlarca dolar tasarruf için kapsayıcıya alınmış. Tutarlı dağıtım, değerlendirilecek daha zor, ancak eşit oranda önemli değildir. Bu geliştiricilerin hala işlemleri yalnızca bu uygulamaları dağıtmak ve yönetmek için tek bir yolu kabul eder ancak onlara uygun teknolojiyi seçin ücretsiz olabileceği anlamına gelir. Bu, birçok farklı teknoloji karmaşıklığı ile uğraşmak zorunda ya da yalnızca belirli olanları seçmeyi geliştiricilerin zorlama işlemlerini azaltır. Temelde her uygulama kendi başına dağıtım görüntüleri olarak kapsayıcılı hale.

Birçok kuruluşun burada durabilir. Zaten sahip oldukları kapsayıcılarının avantajları ve Service Fabric, dağıtım, yükseltmeler, sürüm oluşturma, geri alma işlemleri, sistem durumu izleme vb. tam yönetim deneyimi sağlar.

**Modernizasyonu** -mevcut kapsayıcılı kod yanı sıra yeni hizmetlerin ektir. Yeni kod yazmak için kullanacaksanız, mikro hizmetler yolunu küçük adımlar atın karar en iyisidir. Bu, yeni REST API uç noktası veya yeni bir iş mantığı ekleme. Bu şekilde, yeni mikro hizmetler oluşturma ve uygulama geliştirme ve bunları dağıtma yolculukta başlatın.

**Yenilik** -mikro hizmetler yaklaşımı yürüten bu özgün değişen iş ihtiyaçlarınıza bu makalenin başında Hatırla? Bu aşama kararıdır, bunlar geçerli Uygulamam'olmuyor olan ve bu durumda, tek parçalı uygulama hizmetlerine bölme veya yenilik başlatmak istiyorum. Bir işlem sorunu worflow sırası olarak kullanılan bir veritabanı hale geldiğinde buraya örneğidir. İş akışı sayısı arttıkça istekleri gibi iş için ölçek dağıtılması gerekir. Bu belirli parçası olmayan uygulama için ölçekleme ya da daha sık güncelleştirmeniz gerekiyor, bu mikro hizmete bölmek ve yenilik yapın. 

**Mikro hizmetler halinde dönüştürülmüş** -Bu, uygulamanızın tam olarak oluştuğundan, (veya ayrılmış olarak) olduğu, mikro hizmetler. Buraya ulaşmak için mikro hizmetler yolculuğu yapıldı. Buradan başlayın, ancak bir mikro hizmetler Bunu yapmak için yardımcı olması için ciddi bir yatırım platformudur. 

### <a name="are-microservices-right-for-my-application"></a>Mikro hizmetler sağ Uygulamam için misiniz?
Olabilir. Biz deneyimli iş nedenleriyle bulut oluşturmak giderek daha fazla Microsoft teams'de başlangıcından gibi çoğu bir mikro hizmet benzeri yaklaşımı avantajları gerçekleşen oluştu. Bing, mikro hizmetler arama yıl için örneğin, geliştirme. Diğer ekipler, mikro hizmetler yaklaşımı yeni. Takımlar, orada zor sorunları gücü kendi çekirdek alanlarının dışında çözmek için bulundu. Service Fabric hizmetleri oluşturmak için tercih ettiğiniz bir teknoloji olarak oldukça yaygınlaştı kazanılan nedeni budur.

Amacı, Service Fabric uygulamaları ile bir mikro hizmet yaklaşımı oluşturma karmaşıklıkları azaltmak için böylece olarak üzerinden geçmek zorunda değilsiniz çok pahalı yönelik çalışmalarımızı. Küçükten başlayabilir, gerektiğinde ölçeklendirin, hizmetleri kullanımdan yenilerini ekleyin ve evrim Geçiren ile müşteri kullanım yaklaşımdır. Mikro hizmetler Çoğu geliştirici için daha erişilebilir hale getirmek için henüz çözülmesi gereken birçok diğer sorunları olduğunu biliyoruz. Kapsayıcılar ve aktör programlama modeli bu yöndeki küçük adımlara ilişkin örnekler ve bunu kolaylaştırmak için daha fazla yenilik çıkacaktır emin duyuyoruz.
 
<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric terminolojiye genel bakış](service-fabric-technical-overview.md)
* [Mikro hizmetler: Bulut tarafından desteklenen bir uygulama Devrimi](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/)

[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
[Image3]: media/service-fabric-overview-microservices/microservices-migration.png
