---
title: Azure üzerinde mikro giriş | Microsoft Docs
description: Bir genel bakış neden mikro yaklaşım ile bulut uygulamaları derleme modern uygulama geliştirme için önemlidir ve nasıl Azure Service Fabric Bunu başarmak için bir platform sağlar.
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
ms.openlocfilehash: 16757af0bab7cfd43488118f62300fb167c193a3
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="why-a-microservices-approach-to-building-applications"></a>Neden bir mikro yaklaşımını uygulamaları oluşturmak için?
Yazılım geliştiricileri yoktur nasıl bir uygulama bileşeni parçalara Finansman hakkında düşünüyoruz içinde yeni bir şey. Nesne yönü, yazılım soyutlamalar ve temsilinde merkezi örnektir. Günümüzde, bu factorization sınıflar ve arabirimler paylaşılan kitaplıklar ve teknoloji katmanlar arasında şeklinde eğilimindedir. Genellikle, katmanlı bir yaklaşım bir arka uç depolama, Orta katmanda iş mantığı ve bir ön uç kullanıcı arabirimi (UI) ile alınır. Ne *sahip* son birkaç yıl içinde değiştirilen biz geliştiriciler oluşturmakta olduğunuz olduğu dağıtılmış bulut için olan uygulamaları ve iş tarafından yönlendirilen.

Değişen iş gereksinimleri şunlardır:

* Yerleşik olarak bulunur ve ölçekli müşterilerin yeni coğrafi bölgelerde (örneğin) ulaşmaya çalışır bir hizmet.
* Daha hızlı teslim özellikleri ve yetenekleri müşteri taleplerini Çevik bir şekilde yanıt verebilir.
* Maliyetleri azaltmak için geliştirilmiş kaynak kullanımı.

Bu iş ihtiyaçları etkileyen *nasıl* biz uygulamaları oluşturun.

Mikro Azure'a yaklaşımı hakkında daha fazla bilgi için okuma [mikro: Bulut tarafından desteklenen bir uygulama revolution](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="monolithic-vs-microservice-design-approach"></a>Tek yapılı mikro hizmet tasarım yaklaşımı karşılaştırması
Zaman içinde tüm uygulamaları geliştirin. Kişiler için yararlı olan başarılı uygulamaları geliştirin. Başarısız uygulamalar değil gelişmesi ve sonunda kullanım dışı bırakılmıştır. Soru olur: ne kadar gereksinimleriniz hakkında bugün biliyor musunuz ve ne bunlar gelecekte olacak? Örneğin, bir bölüm için bir raporlama uygulaması oluşturduğunuzu varsayalım. Uygulama, şirketiniz kapsamı içinde kalmasını ve raporların kısa süreli olduğundan emin misiniz? Tercih ettiğiniz yaklaşımın söyleyin, farklıdır, video içerik milyonlarca müşteriler için sunan bir hizmet oluşturma. 

Uygulama daha sonra tasarlanması bildiğiniz karşın bazı durumlarda, kapının çıkışı bir şey kavram kanıtı olarak alma yönlendirmeli, faktördür. Hiçbir zaman kullanılan bir şey aşırı mühendislik içinde çok az noktası yoktur. Bu, olağan mühendislik dengelemeyi olur. Şirketler, bulut için yapı hakkında konuşurken, diğer yandan büyüme ve kullanım beklenir. Büyüme ve ölçek öngörülemeyen bir sorundur. Hızlı bir şekilde gelecekteki başarı ile mücadele etmek için bir yol üzerindeki duyuyoruz ayrıca bilerek için prototip kullanabilmek ister misiniz? Bu yalın başlangıç yaklaşımdır: oluşturma, ölçmek, öğrenin ve yineleme.

İstemci-sunucu dönemi sırasında size her katmanında belirli teknolojileri kullanarak katmanlı uygulamalar oluşturmaya odaklanmanıza tended. Terim *tek yapılı* uygulama için bu yaklaşım ortaya çıktı. Katmanlar arasında olacak şekilde arabirimler tended ve daha sıkı şekilde bağlı tasarım her katman içinde bileşenler arasındaki kullanıldı. Geliştiriciler tasarlanmış ve kitaplıklara derlenmiş ve birkaç yürütülebilir dosyaları ve DLL'ler birbirine sınıfları oluşturmak. 

Bu tür bir tek yapılı bir tasarım yaklaşımı avantajları vardır. Tasarlamak genellikle basittir ve bu çağrıları genellikle işlemler arası iletişim (IPC) olduğundan bileşenleri arasında daha hızlı çağrıları vardır. Ayrıca, herkesin daha fazla kişi-kaynak etkili olma eğilimindedir tek bir ürün sınar. Dezavantajı, katmanlı katmanlar arasında sıkı bir ilişki yoktur ve bileşenleri tek tek ölçeği olamaz ' dir. Düzeltmeleri veya yükseltme gerçekleştirmek gerekiyorsa, başkaları için kendi test bitmesini beklemek zorunda. Çevik olması daha zordur.

Bu downsides mikro adres ve önceki iş gereksinimlerine sahip daha yakından Hizala, ancak ayrıca avantajları ve Borçları vardır. Mikro avantajları, her biri genellikle yukarı veya aşağı, test ölçeklendirme, dağıtmak ve bağımsız olarak yönetirsiniz daha basit iş işlevselliğini yalıtır ' dir. Mikro hizmet yaklaşımın önemli yararlarından biri, takımlar daha iş senaryolarını tarafından katmanlı yaklaşımın teşvik eden teknolojisiyle dayalı olup. Uygulamada, daha küçük ekipleri müşteri senaryoyu temel mikro hizmet geliştirin ve tercih ettikleri teknolojileri kullanır. 

Diğer bir deyişle, kuruluşun mikro hizmet uygulamalarını korumak için teknik standartlaştırmak gerekmez. Tek tek ekipler kendi Hizmetleri ne takım uzmanlık göre için mantıklı veya sorunu çözmek en uygun ne yapabilirsiniz. Uygulamada, belirli bir NoSQL gibi önerilen teknoloji depolayan veya web uygulama çerçevesi, tercih edilir.

Ayrı varlıklar sayısının artması yönetme ve daha karmaşık dağıtımlar ve sürüm oluşturma ile ilgili mikro dezavantajı gelir. Karşılık gelen ağ gecikmeleri yanı sıra mikro arasındaki ağ trafiğini artırır. Çok sayıda chatty, ayrıntılı Hizmetleri performans onarımı kabus için tarif ' dir. Görünüm yardımcı olan araçlar bu bağımlılıklar, "sisteminin tamamını görmek" zordur. 

Standartlar olun mikro hizmet yaklaşım iletişim kurma kabul ettiğinizi belirten ve yalnızca bir hizmetinden gereken şeyleri dayanıklı iş yerine katı sözleşmeleri. Hizmetleri diğer bağımsız olarak güncelleştirmek için bu sözleşmeleri Önden tasarımında tanımlamak önemlidir. Mikro yaklaşımı tasarlama "ayrıntılı hizmet odaklı mimari (SOA)." için de başka bir açıklaması

***En basit haliyle, mikro tasarım yaklaşımı hizmetlerinin her bağımsız değişiklikler ve anlaşılan standartları ile iletişim için ayrılmış bir Federasyon hakkındadır.***

Daha fazla bulut uygulamalarını üretilen gibi kişiler bu genel uygulama ayrıştırma bağımsız, senaryo odaklı hizmetlerine daha iyi uzun vadeli bir yaklaşım olduğunu bulur.

## <a name="comparison-between-application-development-approaches"></a>Uygulama Geliştirme yaklaşımları karşılaştırması
![Service Fabric platform uygulama geliştirme][Image1]

1) Tek yapılı bir uygulama etki alanına özgü işlevselliği içeriyor ve normal web, iş ve veriler gibi işlev Katmanlar bölünür.

2) Birden çok sunucu/sanal makinelerde kopyalanarak tek yapılı bir uygulama ölçeklendirme/kapsayıcıları.

3) Mikro hizmet uygulaması ayrı küçük Hizmetleri işlevsellik ayırır.

4) Dağıtarak out mikro yaklaşım ölçekler her bağımsız olarak, bu hizmetleri örneklerini sunucuları/sanal makinelerde oluşturma hizmet/kapsayıcıları.

Mikro hizmet ile tasarlama yaklaşım panacea tüm projeleri için değil, ancak daha önce açıklanan iş hedeflerini ile daha yakından hizalayın. Tek yapılı bir yaklaşım ile başlayarak, kodu daha sonra mikro tasarım rework fırsatına sahip biliyorsanız, kabul edilebilir olabilir. Daha yaygın olarak, tek yapılı bir uygulama ile başlar ve daha ölçeklenebilir veya Çevik olmasına gerek işlevsel ile başlangıç aşamasında, yavaş bölün.

Özetlemek için mikro hizmet uygulamanız çok sayıda küçük hizmetleri oluşturmak için bir yaklaşımdır. Makine bir kümede dağıtılan kapsayıcılarında Hizmetleri çalıştırın. Küçük ekipleri senaryoyu odaklanan bir hizmeti geliştirmek ve test bağımsız olarak sürüm, dağıtmak ve her hizmetin tüm uygulama gelişmesi şekilde ölçeklendirir.

## <a name="what-is-a-microservice"></a>Bir mikro hizmet nedir?
Mikro farklı tanımlarını vardır. Internet arama yaparsanız, kendi görüşlerini ve tanımları sağlayan çok sayıda kullanışlı kaynaklar bulabilirsiniz. Ancak, mikro aşağıdaki özelliklerinin çoğu yaygın olarak üzerinde anlaşmaya varılan:

* Bir müşteriyi veya iş senaryosu kapsüller. Sorun çözme nedir?
* Küçük mühendislik ekibi tarafından geliştirilmiştir.
* Birinde yazılmış programlama dili ve tüm framework kullanın.
* (İsteğe bağlı) durum her ikisi de dağıtılır ve ölçeklendirilmiş bağımsız olarak sürümlü ve kodu oluşur.
* Diğer mikro ile iyi tanımlanmış arabirimleri ve protokolleri etkileşimde bulunur.
* Konumu çözümlemek için kullanılan benzersiz adlar (URL).
* Tutarlı ve hataları varlığında kullanılabilir kalır.

Bu özelliklere içine özetlemek:

***Mikro hizmet uygulamaları iyi tanımlanmış arabirimlerle standart protokoller üzerinden birbirleri ile iletişim kuran küçük, bağımsız olarak sürümlü ve ölçeklenebilir müşteri odaklı hizmetler oluşur.***

Biz ilk iki nokta önceki bölümde ele ve şimdi biz üzerinde genişletin ve diğerleri açıklamak.

### <a name="written-in-any-programming-language-and-use-any-framework"></a>Birinde yazılmış programlama dili ve tüm framework kullanın
Geliştiriciler, size bir dil veya, bizim becerileri veya hizmet gereksinimlerine bağlı olarak istiyoruz framework seçmek boş olmalıdır. Bazı hizmetler, sonraki tüm başka C++ performans yararlarını değeri. Diğer hizmetler, C# veya Java yönetilen geliştirme kolaylığı en önemli olabilir. Bazı durumlarda, belirli iş ortağı kitaplık, veri depolama teknolojisi veya istemcilere hizmet gösterme anlamına gelir kullanmanız gerekebilir.

Sonra teknoloji ve işletimsel veya yaşam döngüsü yönetimi için gelen ve hizmet ölçeklendirme seçtiniz.

### <a name="allows-code-and-state-to-be-independently-versioned-deployed-and-scaled"></a>Kod ve bağımsız olarak sürümlü olması durumuna dağıtılır ve ölçeklendirilmiş sağlar
Ancak, mikro, kod ve isteğe bağlı olarak durumu yazmak seçtiğiniz bağımsız olarak dağıtma, yükseltme ölçekleme ve. İçin tercih ettiğiniz teknolojilerin gelen gerçekte çözmek için daha zor sorunlardan biri olmasıdır. Ölçeklendirme, anlamak için nasıl bölüm (veya parça) hem kod hem de durum zorlu olur. Kodu ve durum bugün yaygın bir durumdur, ayrı teknolojiler kullandığınızda, mikro hizmet için dağıtım betikleri her ikisinin de ölçeklendirme başa çıkacak şekilde gerekir. Bunların tümü aynı anda yükseltme yapmak zorunda kalmadan bazı mikro yükseltebilmek için aynı zamanda çevikliği ve esnekliği, hakkında budur.

Mikro hizmet yaklaşım karşı tek yapılı bir süre dönme, aşağıdaki diyagramda durumu depolamak üzere yaklaşım farklılıkları gösterir.

#### <a name="state-storage-between-application-styles"></a>Uygulama stilleri arasında durum depolama
![Service Fabric platform durumu depolama][Image2]

***Sol taraftaki tek yapılı bir yaklaşım tek bir veritabanı ve belirli teknolojileri katmanlarını vardır.***

***Sağdaki mikro yaklaşımın burada durumu genellikle mikro hizmet için kapsamlı ve kullanılan çeşitli teknolojiler birbirine mikro grafiğini vardır.***

Tek yapılı bir yaklaşım, uygulama genellikle tek bir veritabanı kullanır. Avantajı dağıtmayı kolaylaştırır tek bir konum olmasıdır. Her bileşenin durumunu depolamak için tek bir tabloyu olabilir. Takımlar zor olduğu durum kesinlikle ayırmanız gerekir. Kaçınılmaz olarak var olan bir müşteri tabloya yeni bir sütun ekleyin, tablolar arasında birleştirme yapın ve depolama katmanında bağımlılıkları oluşturmak için temptations vardır. Bu gerçekleştikten sonra tek bileşenlerin ölçeklendirme olamaz. 

Mikro yaklaşımda her hizmet yöneten ve kendi durumu depolar. Her hizmet, hem kod hem de birlikte hizmet isteklerini karşılamak için durum ölçekleme için sorumludur. Bir dezavantajı, görünümler veya uygulamanızın veri sorguları oluşturmak için bir gereksinimi olduğunda ayrık durumu mağazada sorgu gerektiğini ' dir. Genellikle, bu görünüm mikro koleksiyonunda derlemeler ayrı bir mikro hizmet sağlayarak çözülür. Verilerin birden çok hazırlıksız sorguları gerçekleştirmek gerekiyorsa, her mikro veri ambarı hizmeti çevrimdışı analiz için verileri yazma düşünmelisiniz.

Sürüm oluşturma bir mikro hizmet dağıtılan sürümüne özeldir bu nedenle bu birden çok farklı sürümlerini dağıtma ve yan yana çalıştırma. Sürüm oluşturma bir mikro hizmet, yeni bir sürümünü yükseltme sırasında başarısız olduğu ve bir önceki sürüme geri almak gereken senaryoları yer almaktadır. Sürüm oluşturma için diğer senaryo nerede hizmet farklı sürümlerini farklı kullanıcılar deneyimi A/B-style sınama gerçekleştiriyor. Örneğin, müşteriler, daha fazla bilgi yaygın olarak çalışırken önce yeni işlevselliğini sınamak için belirli bir dizi için mikro hizmet yükseltmek için yaygındır. Mikro yaşam döngüsü yönetimi sonra bunu şimdi bize bunların arasındaki iletişimi için getirir.

### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>İyi tanımlanmış arabirimleri ve protokolleri diğer mikro ile etkileşim
Son 10 yıl içinde yayınlanan hizmet odaklı mimari hakkında kapsamlı belgeleri iletişim düzenleri açıkladığı için bu konuda burada az kontrol edilmesi gerekiyor. Genellikle, hizmet iletişimi bir REST yaklaşım HTTP ve TCP protokollerini ve XML veya JSON ile seri hale getirme biçimi kullanır. Arabirim açısından bakıldığında, bu web tasarım yaklaşımı benimsemenin hakkında olur. Ancak hiçbir şey, ikili protokolleri veya kendi veri biçimleri kullanarak durdurur. Bu protokoller ve biçimleri açık yayımlanmaması kullanılabilir değilse, mikro kullanarak daha zor bir kez kişilerin için hazır olun.

### <a name="has-a-unique-name-url-used-to-resolve-its-location"></a>Konumu çözümlemek için kullanılan benzersiz bir ad (URL)
Nasıl biz mikro hizmet yaklaşım gibi web olduğunu söyleyen tutmak hatırlıyor? Web gibi mikro hizmet çalıştığı her yerde adreslenebilir olması gerekir. Makineler hakkında düşünüyor ve hangisinin belirli mikro hizmet çalıştırıyorsa şeyler hızla hatalı gidin. 

DNS için belirli bir makine belirli bir URL çözümleyen aynı şekilde, mikro hizmet geçerli konumuna bulunabilirlik böylece benzersiz bir ad olmalıdır. Mikro bunlar çalıştıran altyapısından bağımsız duruma adreslenebilir adları olması gerekir. Bu hizmet kayıt defteri olması gerekir çünkü hizmetinizin nasıl dağıtıldığını ve nasıl bulunduğundan, arasındaki etkileşim olduğunu gösterir. Bir makine başarısız olduğunda, eşit, kayıt defteri hizmeti, hizmet artık çalıştığı bildirmeniz gerekir. 

Bu bize sonraki konusuna getirir: esnekliği ve tutarlılık.

### <a name="remains-consistent-and-available-in-the-presence-of-failures"></a>Tutarlı ve hataları varlığında kullanılabilir kalır
Beklenmeyen hatalar postalarla özellikle dağıtılmış bir sistemde çözmenin en zor sorunlar biridir. Biz geliştiricileri de yazma kodu çoğunu özel durumları işleme ve bu da testinde en uzun süre burada harcanmaktadır. Sorun daha hataları işlemek için kod yazma daha karmaşıktır. Mikro hizmet çalıştığı makine başarısız olduğunda ne olur? Yalnızca bu mikro hizmet hatası (sabit kendi başına bir sorun) algılayan gerekir, ancak ayrıca şey, mikro hizmet yeniden başlatmanız gerekir. 

Bir mikro hizmet hatalarına dayanıklı olmasını ve genellikle başka bir makinede kullanılabilirliği nedeniyle yeniden başlatmanız gerekir. Bu ayrıca mikro hizmet adına mikro hizmet bu durumdan kurtarabilir ve mikro hizmet başarılı bir şekilde yeniden verip veremediğini kaydedilmiş duruma gelir. Diğer bir deyişle, vardır (işlem başlatır) işlem esnekliği ve bunun yanı sıra esnekliği durumu ya da veri (veri kaybı ve verileri tutarlı kalabilmesi) olması gerekir.

Dayanıklılık sorunları zaman uygulama yükseltme sırasında hatalar meydana gibi diğer senaryolar sırasında araya geldiğinde. Dağıtım sistemiyle çalışma mikro hizmet kurtarmak zorunda değildir. Ayrıca bu sürüme ilerlemek yerine tutarlı bir duruma korumak için bir önceki sürüme geri kullanmaya devam edebilir veya olup olmadığını karar vermeniz gerekir. Yeterli makineler ileri taşıma tutmak kullanılabilir olup olmadığı ve mikro hizmet önceki sürümlerini kurtarmak nasıl gibi soruları ele alınması gerekir. Bu kararlar yapabilmek için sistem durumu bilgisi yayması için mikro hizmet gerektirir.

### <a name="reports-health-and-diagnostics"></a>Raporları durum ve tanılama
Açıktır, görünebilir ve genellikle atlamış, ancak bir mikro hizmet, durum ve tanılama raporu gerekir. Aksi takdirde işlemleri açısından biraz Insight yoktur. Tanılama Olayları bağımsız Hizmetleri kümesi arasında ilişkilendirme ve olayı sırası anlamlı makine saat eğriltir ilgilenme zordur. Anlaşılan protokolleri ve veri biçimleri üzerinden mikro hizmet ile etkileşime aynı şekilde, sistem durumu ve sorgulama ve görüntülemek için bir olay deposunda sonuçta şunun tanılama olayları günlüğe kaydetme hakkında standartlaştırma gereksinimi görünür. İsteğe bağlı olarak bir mikro yaklaşım anahtarı olan farklı ekipler'in tek günlük biçimi kabul etmiş olursunuz. Var. bir bütün olarak uygulama tanılama olayları görüntüleme için tutarlı bir yaklaşım olması gerekir.

Sistem durumu tanılama farklıdır. Sistem durumu geçerli durumuna uygun eylemleri raporlama mikro hizmet hakkında ' dir. İyi bir örnek kullanılabilirliğini sağlamak için yükseltme ve dağıtım mekanizmaları ile çalışmaktadır. Bir hizmet işlemi kilitlenme nedeniyle şu anda sağlam veya yeniden başlatma makine rağmen hizmet işletimsel olabilir. Gereksinim duyduğunuz son şey bu yükseltme gerçekleştirerek kötü olmasını sağlamaktır. Önce bir araştırma yapmak için en iyi yaklaşımdır veya kurtarmak mikro hizmet için zaman tanıyın. Bir mikro hizmet sistem durumu olayları bilinçli kararlar almanıza ve etkili, kendini onarma hizmetleri oluşturmanıza yardımcı yardımcı olur.

## <a name="service-fabric-as-a-microservices-platform"></a>Service Fabric mikro platform olarak
Azure Service Fabric Microsoft tarafından bir geçiş gelen Hizmetleri teslim etmek için stilde genellikle tek yapılı, kutusunu ürünleri teslim ortaya çıktı. Yapı ve Azure SQL Database ve Azure Cosmos DB gibi büyük Hizmetleri işletim deneyimini Service Fabric şeklinde. Bu daha da fazla Hizmetleri benimsenen gibi platform zamanla gelişen. Önemlisi, Service Fabric yalnızca azure'da aynı zamanda tek başına Windows Server dağıtımlarında çalıştırmak gerekiyordu.

***Takımlar mikro yaklaşımı kullanarak iş sorunlarını çözebilir Service Fabric amacı oluşturma ve bir hizmeti çalıştıran sabit sorunlarını çözmek ve altyapı kaynaklarını verimli bir şekilde kullanmak için böylelikle.***

Service Fabric mikro yaklaşım kullanan uygulamalar oluşturmanıza yardımcı olmak için üç geniş alanları sağlar:

* Dağıtmak için sistem hizmetleri sağlayan bir platform yükseltme, algılamak, başarısız hizmetlerini yeniden başlatın, hizmetleri bulmak, iletileri yönlendirmek, durumunu yönetme ve durumunu izleyin. Bu sistem hizmetleri yürürlükte daha önce açıklanan mikro özelliklerinin çoğunu sağlar.
* Kapsayıcılardaki veya işlemler olarak ya da çalışan uygulamaları dağıtma yeteneği. Service Fabric bir kapsayıcı ve işlem orchestrator ' dir.
* Mikro uygulamalar oluşturmanıza yardımcı olmak için üretken programlama API'leri: [ASP.NET Core, Reliable Actors ve güvenilir hizmetler](service-fabric-choose-framework.md). Mikro hizmet oluşturmak için herhangi bir kod seçebilirsiniz. Ancak bu API'leri iş daha kolay hale getirin ve daha ayrıntılı bir düzeyde platform tümleştirileceğini. Bu şekilde, örneğin, durum ve tanılama bilgileri elde edebilirsiniz veya yerleşik yüksek kullanılabilirlik yararlanabilir.

***Service Fabric hizmetinizin nasıl oluşturulacağına bağımsızdır ve herhangi bir teknoloji kullanabilirsiniz. Ancak, daha kolay mikro oluşturmak için yerleşik programlama API'ler sağlar.***

### <a name="migrating-existing-applications-to-service-fabric"></a>Service Fabric mevcut uygulamalarını geçirme
Bir anahtar için Service Fabric sonra ile yeni mikro modernized varolan kodunu yeniden yaklaşımdır. Uygulama modernization beş aşama vardır ve başlatma ve herhangi bir aşamaları sırasında durdurma. Bunlar;

1) Geleneksel tek yapılı uygulamayı yap  
2) Kaldırın ve Shift - kapsayıcıları veya konuk yürütülebilir dosyalar Service Fabric var olan kodda barındırmak için kullanın.  
3) Modernization - yeni mikro yanı sıra var olan kapsayıcılı kodu eklendi.  
4) Yenilik - tamamen gereksinimleri temelinde mikro içine tek yapılı bölün.  
5) Mikro - tek yapılı uygulamaları mevcut veya yeni greenfield uygulamaları derleme dönüştürme dönüştürüldüğünde.

![Mikro geçiş][Image3]

Şunları yapabilirsiniz yeniden vurgulamak önemlidir **başlatmak ve durdurmak bu aşamalar hiçbirini**. Sonraki aşamaya ilerleme etmeye mecbur değil. Şimdi örnekler her Bu aşamalar için bakalım.

**Kaldırın ve Shift** -şirketler çok sayıda kaldırma ve var olan tek yapılı uygulamaları kapsayıcılarına iki nedenden dolayı; kaydırma

- Maliyet azaltma ya da birleştirme ve daha yüksek yoğunluk mevcut donanım ya da çalışan uygulamaları kaldırılmasını nedeniyle. 
- Geliştirme ve işlemler arasında tutarlı bir dağıtım sözleşme.

Maliyet azaltması anlaşılabilir ve Microsoft bünyesinde, mevcut uygulamaları çok sayıda milyonlarca dolar kaydetmeyi kapsayıcılı. Tutarlı bir dağıtım değerlendirmek daha zor ama aynı derecede önemli değildir. Geliştiriciler hala teknolojisi bu paketleri seçmek boş olabileceğini yazacaktır bunları işlemlerini yalnızca bu uygulamaları dağıtmak ve yönetmek için tek bir yolu kabul eder ancak. Birçok farklı teknolojiler karmaşıklığını uğraşmak zorunda ya da yalnızca belirli olanları seçmek için geliştiricilere zorlama işlemlerini azaltır. Temelde her uygulamanın kendi içinde bulunan dağıtım görüntülere kapsayıcılı.

Birçok kuruluş burada durdurun. Zaten sahip oldukları kapsayıcıları avantajları ve Service Fabric dağıtım, yükseltmeler, sürüm oluşturma, alma, sistem durumu izleme vb. tam yönetim deneyimi sağlar.

**Modernization** -varolan kapsayıcılı kodu yanı sıra yeni hizmetler ektir. Yeni kod yazma kullanacaksanız, küçük adımlar mikro yoldan karar en iyisidir. Bu, yeni REST API uç noktası veya yeni bir iş mantığı ekleme. Bu şekilde, yeni mikro hizmetler oluşturma ve uygulama geliştirme ve bunları dağıtma gezisine üzerinde başlatın.

**Yenilik** -mikro yaklaşım yürüten bu özgün değişen iş gereksinimlerini bu makalenin başlangıcında unutmayın? Bu aşamada karardır bunlar geçerli Uygulamam gerçekleştiği ve öyleyse monolith bölme veya innovating başlatmak ihtiyacım. Bir iş akışı sırası olarak kullanılmakta olduğundan bir veritabanı işleme tıkanıklık olduğunda burada örneğidir. İş akışı sayısı olarak iş artırma istekleri için ölçek dağıtılması gerekir. İçin değil uygulamanın belirli parçasının ölçeklendirme veya daha sık güncelleştirme olması gerekir, bu çıkışı mikro hizmet bölme ve yenilik. 

**Mikro dönüştürülmüş** -Bu, uygulamanızın tümüyle oluştuğundan, (veya içine ayrıştırılmış) olduğu mikro. Burada ulaşmak için mikro gezisine yaptınız. Burada başlatabilirsiniz, ancak bunu mikro yapmak için yardımcı olması için önemli bir yatırım platformudur. 

### <a name="are-microservices-right-for-my-application"></a>Mikro sağ Uygulamam için misiniz?
Olabilir. Biz ne yaşadığı ekiplerin Microsoft bulut iş nedenleri için yapı başlamıştır gibi bunların çoğu, mikro hizmet benzeri yaklaşımı avantajlarını gerçekleşen oluştu. Bing, örneğin, arama yıllık mikro geliştirme. Diğer ekipler için mikro yaklaşım yeni. Takımlar, var. sabit sorunları gücü dışında kendi çekirdek alanları çözmek için bulunamadı. Service Fabric hizmetleri oluşturmak için tercih ettiğiniz teknoloji olarak traction kazanılan nedeni budur.

Service Fabric amacı mikro hizmet yaklaşım uygulamalarla oluşturmanın karmaşıklıkları azaltmak için böylece olarak üzerinden geçmek zorunda değilsiniz birçok pahalı redesigns. Küçük başlayın, ölçeklendirme gerektiğinde, hizmetleri itiraz etme, yenilerini ekleyin ve gelişmesi müşteriyle kullanım yaklaşımdır. Mikro çoğu geliştiriciler için daha erişilebilir yapmak için henüz çözülmesi gereken birçok diğer sorunlar olduğunu biliyoruz. Kapsayıcılar ve aktör programlama modeli bu yönde küçük adımları örnektir ve size daha fazla yenilikleri kolaylaştırmak için ortaya çıkacaktır emin.
 
<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric terminolojisi genel bakış](service-fabric-technical-overview.md)
* [Mikro: Bulut tarafından desteklenen bir uygulama devrim](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/)

[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
[Image3]: media/service-fabric-overview-microservices/microservices-migration.png
