---
title: Küresel ölçekli Azure Cosmos DB ile birden çok yöneticili | Microsoft Docs
description: ''
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: rimman
ms.openlocfilehash: cc66b2f506d81a7ba10b26c3b24287472e890682
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34724916"
---
# <a name="multi-master-at-global-scale-with-azure-cosmos-db"></a>Küresel ölçekli çok şablonu Azure Cosmos DB ile 
 
Genel olarak geliştirmeye verilerin tutarlı görünümlerini dünya çapında koruma zor bir sorundur ancak yerel gecikme ile yanıt uygulamaları dağıtılmış. Veri erişim gecikmesi geliştirmek, yüksek kullanılabilirlik elde etmek, garantili olağanüstü durum kurtarma sağlamak ve kendi iş gereksinimlerini karşılamak için ihtiyaç duydukları çünkü müşterilerin Genel dağıtılmış veritabanları, kullanın. Birden çok ana Azure Cosmos veritabanı yüksek düzeyde kullanılabilirlik (%99.999), veri ve ölçeklenebilirlik yerleşik kapsamlı ve esnek çakışma çözümleme desteğiyle yazmak için tek basamaklı milisaniyelik gecikme süresi sağlar. Bu özellikler, genel olarak dağıtılmış uygulamaların geliştirilmesini önemli ölçüde basitleştirir. Genel olarak dağıtılmış uygulamalar için birden çok yöneticili desteği önemlidir. 

![Birden çok yöneticili mimarisi](./media/multi-region-writers/multi-master-architecture.png)

Azure Cosmos DB birden çok yöneticili desteğiyle kapsayıcılarında (örneğin, koleksiyonlar, grafikler, tablolar) herhangi bir yere dünyada Dağıtılmış veri yazma işlemleri gerçekleştirebilir. Veritabanı hesabınızla ilişkili herhangi bir bölgeyi verileri güncelleştirebilirsiniz. Bu veri güncelleştirmeleri zaman uyumsuz olarak yayabilir. Hızlı erişim ve verilerinize yazma gecikmesi sağlamanın yanı sıra, birden çok ana ayrıca pratik bir çözüm yük devretme ve Yük Dengeleme sorunları sağlar. Özet olarak, Azure Cosmos DB ile yazma gecikme aldığınız < 99 yerindeki world, %99.999 yazma ve okuma kullanılabilirlik world ve her ikisi de ölçeklendirmenizi herhangi bir yere, 10 ms yazma ve dünyanın her yerden verimlilik okuyun.   

> [!IMPORTANT]
> Birden çok yöneticili desteğidir Önizleme sürümü kullanmak için özel Önizleme'de, [kaydolun](#sign-up-for-multi-master-support) şimdi.

## <a name="sign-up-for-multi-master-support"></a>Birden çok yöneticili desteği için kaydolun

Bir Azure aboneliğiniz zaten varsa, Azure portalında birden çok yöneticili Önizleme programına katılma kaydolabilir. Azure'da yeniyseniz, kaydolun bir [ücretsiz deneme sürümü](https://azure.microsoft.com/free) 12 ay ücretsiz Azure Cosmos DB erişimin nereden. Birden çok yöneticili Önizleme programı'na erişim istemek için aşağıdaki adımları tamamlayın.

1. İçinde [Azure portal](https://portal.azure.com), tıklatın **kaynak oluşturma** > **veritabanları** > **Azure Cosmos DB**.  

2. Yeni hesap sayfası Azure Cosmos DB hesabınız için bir ad, API, abonelik, kaynak grubunu ve konumu seçin.  

3. Ardından **önizlemeye hemen kaydolun** çoklu Mater Önizleme alanı altında.  

   ![Birden çok yöneticili Önizleme için kaydolma](./media/multi-region-writers/sign-up-for-multi-master-preview.png)

4. İçinde **önizlemeye hemen kaydolun** bölmesinde tıklatın **Tamam**. İsteği gönderdikten sonra durum değişikliklerini **onay bekleyen** hesap oluşturma dikey.  

İsteği gönderdikten sonra isteğiniz onaylandıktan bir e-posta bildirimi alırsınız. İsteği nedeniyle yüksek hacimli bir hafta içinde bildirim almanız gerekir. İsteği tamamlamak için bir destek bileti oluşturun gerekmez. Sıraya alınmış olan istekleri incelenecektir.

## <a name="a-simple-multi-master-example--content-publishing"></a>Basit bir birden çok yöneticili örnek – içerik yayımlama  

Azure Cosmos DB ile birden çok yöneticili destek kullanmayı açıklar gerçek dünya senaryoları bakalım. Azure Cosmos DB'de yerleşik bir içerik yayımlama platform göz önünde bulundurun. Bu platform için harika kullanıcı deneyimi Yayımcılar ve tüketicileri için uyması gereken bazı gereksinimler şunlardır. 

* Yazarlar ve abonelerin tüm dünyadaki yayılır.  

* Yazarlar kendi yerel (en yakın) bölgesine (yazma) makaleleri yayımlamanız gerekir.  

* Yazarları okuyucular/dünya çapında dağıtılan aboneleri kendi makalelerin vardır.  

* Yeni makaleler yayımlandığında aboneleri bir bildirim almanız gerekir.  

* Aboneler kendi yerel bölgesinden makaleleri okuyabilir olması gerekir. Bunlar ayrıca bu makaleler incelemeler eklemeniz mümkün olması gerekir.  

* Herkes makaleleri yazarı dahil olmak üzere tüm değerlendirmeleri makaleler için yerel bir bölgesinden bağlı mümkün görünümü olmalıdır.  

Kullanıcıları ve yayımcılarına makaleler, milyarlarca ile milyonlarca varsayılarak yakında biz yere göre erişim güvence altına almak birlikte ölçeği sorunları üstesinden gelir gerekir. Bu tür bir kullanım örneği ideal bir aday Azure Cosmos DB çok için yöneticisidir. 

## <a name="benefits-of-having-multi-master-support"></a>Birden çok yöneticili destek sahip avantajları 

Birden çok yöneticili Destek genel dağıtılmış uygulamalar için gereklidir. Birden çok ana, oluşan [birden çok ana bölge](distribute-data-globally.md) bir yazma yerde modelde (etkin-etkin desen) eşit katılmak ve veri herhangi bir zamanda ihtiyaç duyacağınız kullanılabilir olduğundan emin olmak için kullanılır. Tek bir bölge için yapılan güncelleştirmeler (hangi sırayla ana bölgelerde kendi) diğer tüm bölgelere zaman uyumsuz olarak yayılır. Birden çok ana yapılandırma olarak ana bölgelerde otomatik olarak çalışan azure Cosmos DB bölgeler çalışma tüm çoğaltmaları veri yakınsamasını ve sağlamak için [genel tutarlılık ve veri bütünlüğü](consistency-levels.md). Aşağıdaki resimde bir tek yöneticili ve mult-master için okuma/yazma çoğaltma gösterir.

![Tek Yönetici ve birden çok yöneticili](./media/multi-region-writers/single-vs-multi-master.png)

Uygulama çok yöneticisinde kendi yük geliştiricilere ekler. Birden çok ana, kendi uygulamak için deneyin büyük ölçekli müşteriler yapılandırma ve dünya çapında birden çok yöneticili yapılandırmasını sınama saat yüzlerce harcayabilir ve çoğu, tek izlemek ve birden çok asıl korumak için iş mühendisleri ayrılmış bir dizi vardır Çoğaltma. Oluşturma ve kendi birden çok yöneticili Kurulum yönetme uygulamada innovating çıktığınızda kaynakları zaman alır ve çok daha yüksek maliyetlerini sonuçlanır. Azure Cosmos DB birden çok yöneticili desteği "out-of--box" sağlar ve bu yükünü geliştiricilerden kaldırır.  

Özet olarak, birden çok ana aşağıdaki avantajları sağlar:

* **Daha iyi olağanüstü durum kurtarma, kullanılabilirliği ve yük devretme yazma**-birden çok ana, büyük ölçüde kritik bir veritabanının yüksek kullanılabilirlik korumak için kullanılabilir. Birincil bölge kesinti veya bölgesel bir olağanüstü durum nedeniyle kullanılamaz hale geldiğinde Örneğin, birden çok ana veritabanına veri bir bölgesinden bir yük devretme bölgeye çoğaltabilirsiniz. Bu tür bir yük devretme bölge uygulamayı desteklemek için bir tam olarak işlevsel ana bölge hizmet verecektir. Bölgeler kalan bir garantili yazma kullanılabilirlik > %99.999 çok yöneticileriyle coğrafi olarak farklı olabilir çünkü çok ana doğal afetler, güç kesintileri veya sabotaj veya her ikisini göre büyük "survivability" koruma sağlar. 

* **Son kullanıcılar için geliştirilmiş yazma gecikmesi** - yakınsa (sunan), verileri son kullanıcılar için deneyimi daha iyi olacaktır. Örneğin, Avrupa kullanıcılar varsa, ancak veritabanınızı ABD ve Avustralya'da, yaklaşık 140 ms eklenen gecikme olur ve ilgili bölgeler için 300 ms. Gecikmeler başlamak birçok popüler oyunlar, bankacılık gereksinimleri veya etkileşimli uygulamaların (web veya mobil) edilemez. Gecikme süresi içinde yüksek kaliteli deneyimi müşterinin algısına büyük bir bölümü oynar ve bazı belirgin ölçüde kullanıcılara davranışını etkilemeye kanıtlandı. Teknoloji geliştikçe ve özellikle AR, VR ve MR geliştirilirken ile daha derinlikli ve gerçek deneyimleri gerektirme geliştiriciler artık sıkı gecikme gereksinimlerine yazılım sistemleriyle üretmek gerekir. Bu nedenle, yerel olarak kullanılabilir uygulamaları ve verileri (içerik uygulamalar için) sahip daha önemlidir. Birden fazla ile Azure Cosmos veritabanı, performans olarak normal yerel okuma ve yazma gibi hızlı ve coğrafi dağıtım tarafından genel olarak geliştirilmiş yöneticisidir.  

* **Geliştirilmiş yazma ölçeklenebilirlik ve yazma üretimi** - birden çok ana daha yüksek verimlilik verir ve doğruluk ile birden fazla tutarlılık modeli sunarken fazla kullanımı güvence altına alır ve SLA ile yedeklenir. 

  ![Birden çok ana ile ölçeklendirme yazma üretimi](./media/multi-region-writers/scale-write-throughput.png)

* **Bağlantısı kesilen ortam (örneğin, sınır cihazları) için daha iyi destek** -birden çok ana bağlantısı kesilmiş bir ortamda en yakın bir bölge için bir uç cihazın tüm çoğaltılması ya da bir veri alt kümesini kullanıcılardan sağlar. Bu senaryo tipik bir satış ekibi otomasyon sistemleri, bir veri alt kümesini temsilcisinin ilgili depoladığı bir bireyin dizüstü (bağlantısı kesilmiş bir aygıt). Dünyanın her yerden ana bulut bölgelerde uzak uç aygıtlardan kopyalama hedefi olarak çalışabilir.  

* **Yük Dengeleme** -birden çok ana ile uygulama arasında yük kullanıcıları/iş yükleri yoğun olarak yüklenen bölgesinden burada yük dengeli bir şekilde dağıtılır bölgelere taşıyarak yeniden dengelenir. Kapasite, yeni bir bölge ekleyerek ve yeni bölge için bazı yazma geçiş yapma kolayca genişletilebilir yazma. 

* **Sağlanan kapasitesini daha iyi kullanımı** - çok Yöneticisi, yazma yoğunluklu ve karma iş yükleri için birden çok bölgeler arasında sağlanan kapasite karşılayabilir...  Sağlanacak daha az verimlilik gerektirir şekilde, okuma ve yazma işlemleri daha eşit şekilde, dağıtabilirsiniz bazı durumlarda ve daha fazla tasarruf müşteriler için maliyet müşteri adayları.  

* **Daha basit ve daha esnektir uygulama mimarileri** -birden çok ana yapılandırma taşıma uygulamaların veri esnekliği garanti.  Tüm karmaşıklık gizleme Azure Cosmos DB ile önemli ölçüde uygulama tasarımı ve mimarisini basitleştirebilirsiniz. 

* **Risk sorunsuz yük devretme sınaması** -yük devretme sınaması sahip olmaz herhangi bir düşüşü yazma üretimi. Birden çok ana ile diğer tüm bölgeler tam yöneticileri olduğundan yük devretme üzerinde yazma üretimi kadar etkili olmaz.  

* **Toplam Maliyet Ownership(TCO) ve DevOps alt** -toplantı ölçeklenebilirlik, performans, genel dağıtım kurtarma zamanı hedeflerine pahalı eklentileri veya bekleyen olağanüstü durum kadar olan bir yedekleme altyapısı bakım nedeniyle pahalı genellikle sağlar. Endüstri lideri SLA tarafından yedeklenen Azure Cosmos DB çok şablonu ile geliştiriciler artık oluşturma ve "arka uç mantığı Birleştirici" kendilerini koruma gerektirir ve, kritik iş yüklerini çalıştıran içiniz alın. 

## <a name="use-cases-where-multi-master-support-is-needed"></a>Kullanım örnekleri birden çok yöneticili destek burada gereklidir

Azure Cosmos DB'de çok ana için çok sayıda kullanım örnekleri vardır: 

* **IOT** -Azure Cosmos DB çok ana IOT veri işleme Basitleştirilmiş dağıtılmış uygulama için izin verir. İade çakışma serbest kullanan coğrafi olarak dağıtılan kenar dağıtımlar veri türleri genellikle birden çok konumlardan zaman serisi veri izlemeniz gerekiyorsa çoğaltılan. Her aygıt en yakın bölgelerinden birine bağlantılı ve bir aygıtı (örneğin, bir araba) izler ve dinamik olarak başka bir bölgeye yazmak için barındırılması.  

* **E-ticaret** -e-ticaret senaryolarda iyi kullanıcı deneyimini modemlerin yüksek kullanılabilirlik ve esnekliği hatası senaryoları için gerekiyor. Bir bölge başarısız olursa, kullanıcı oturumları, Alışveriş sepetleri, etkin listeleri sorunsuz bir şekilde durumu kaybı olmadan başka bir bölgeye göre çekilmesi gerekir istiyor. Bu arada, kullanıcı tarafından yapılan güncelleştirmeler uygun şekilde ele alınması gerekir (örneğin, ekler ve Alışveriş sepetinden kaldırır üzerinden aktarım gerekir). Birden çok ana ile Azure Cosmos DB bu senaryolara düzgün biçimde, tutarlı bir görünüm kullanıcının açısından koruyarak etkin bölgeler arasında sorunsuz bir geçiş ile işleyebilir. 

* **Sahtekarlık/Anomali algılama** -genellikle bir kullanıcı etkinliği veya hesap etkinliği izlemek uygulamalar genel olarak dağıtılır ve çeşitli olaylarını aynı anda izlemek gerekir. Oluşturma ve bir kullanıcı için puanlarını bakımı sırasında farklı coğrafi bölgeler Eylemler aynı anda risk ölçümleri satır içi tutmak için puanları güncelleştirmeniz gerekir. Azure Cosmos DB geliştiricilerin uygulama düzeyinde çakışma senaryolarını ele gerekmez güvence altına almak. 

* **İşbirliği** - mal satış veya tüketilmesi medyayı gibi makalelerin popülerliği derece dayalı uygulamalar için vs. Telif hakkı özellikle yapılması ücretli veya gerçek zamanı reklam kararları olması gerektiğinde, coğrafi bölgeler arasında popülerliği izleme karmaşık, alabilirsiniz. Derecelendirme, sıralama ve birçok bölgelere dünya çapında, Azure Cosmos DB ile gerçek zamanlı raporlama gecikmeleri üzerinde tehlikeye olmadan ve çok az çaba ile özellikleri sunmak geliştiricilerin sağlar. 

* **Ölçüm** - sayım ve kullanım düzenlemek (API çağrıları gibi işlemleri/saniye, dakika kullanılan) ile Azure Cosmos DB çok ana kullanarak Basitlik genel olarak uygulanabilir. Her iki doğruluğu sayısı ve gerçek zamanlı düzenleme, yerleşik bir çakışma çözümü sağlar. 

* **Kişiselleştirme** - bağlılık gibi eylemleri tetikleyen coğrafi olarak dağıtılmış sayaçları koruma ödül noktaları veya kişiselleştirilmiş kullanıcı oturumu uygulama görünümleri, yüksek kullanılabilirlik ve coğrafi olarak dağıtılan Basitleştirilmiş Azure Cosmos DB tarafından sağlanan sayım uygulamaları teslim yüksek performanslı kolaylık sağlar. 

## <a name="conflict-resolution-with-multi-master"></a>Birden çok ana ile çakışma çözümü 

Çok Yöneticisi ile genellikle aynı kaydı iki (veya daha fazla) çoğaltmalarını aynı anda iki veya daha fazla farklı bölgelerdeki farklı yazarlar tarafından güncelleştirilmemiş olabilir iştir. Eşzamanlı yazma aynı kaydı ve yerleşik bir çakışma çözümleme olmadan iki farklı sürümleri neden olabilir ve uygulama bu tutarsızlığı çözmek için çakışma çözümü gerçekleştirmeniz gerekir.  

**Örnek** -iki bölgelerde Kalıcılık mağazada alışveriş sepeti uygulaması ve bu uygulama dağıtıldığında, Azure Cosmos DB kullandığınızı varsayalım: Doğu ABD ve Batı ABD.  Yaklaşık aynı anda bir kullanıcı, San Francisco öğeyi kendi alışveriş sepetine (örneğin, bir kitap) Doğu ABD bir stok yönetim işlemi sırasında eklerse farklı bir alışveriş sepeti öğe (örneğin, yeni bir telefon) yanıt olarak bir s aynı o kullanıcı için geçersiz kılar Yayın tarihi Gecikmeli upplier bildirimi. T1 anında, iki bölgede alışveriş sepeti kayıtlarında farklıdır. Veritabanı, çoğaltma ve çakışma çözme mekanizması tutarsızlığı çözmek için kullanır ve alışveriş sepeti iki sürümü biri sonunda seçilir. En sık (örneğin, son yazma WINS) birden çok ana veritabanı tarafından uygulanan çakışma çözümleme buluşsal yöntemlerini kullanarak, kullanıcı veya uygulamanın hangi sürümünün seçili tahmin etmek için mümkün değildir. Her iki durumda da, veri kaybı olmamasına veya beklenmeyen davranışları ortaya çıkabilir. Yeni bir satın alma öğesi (diğer bir deyişle, kitap) kullanıcının seçimini Doğu bölgesi sürümü seçilirse, sonra kaybolur ve Batı bölgesi seçilirse, ardından daha önce seçilen (diğer bir deyişle, telefon) Sepeti hala öğedir. Her iki durumda da bilgiler kaybolur. Son olarak, alışveriş inceleniyor başka bir işlem arasında süreleri de belirleyici olmayan davranışını görmek için T1 ve T2 olduğuna Sepeti. Örneğin, Karşılama ambarı seçer ve maliyetleri sevkiyat Sepeti güncelleştiren bir arka plan işlemi Sepeti nihai içeriğini çakışan sonuçlar üretir. Sepeti yalnızca bir öğe, kitap yakında olsanız bile işlem Batı bölgesinde çalıştığından ve gerçekte alternatif 1 olur, iki öğe sevkiyat maliyetlerini işlem. 

Azure Cosmos DB veritabanı altyapısı içinde çakışan yazma işleme mantığı uygular. Azure Cosmos DB sunar **esnek ve kapsamlı çakışma çözümü desteği** çözümleme modeller, otomatik dahil olmak üzere birkaç çakışma teklifi tarafından (iade çakışma serbest çoğaltılan veri türleri), son yazma WINS (LWW) ve özel () Saklı yordam) otomatik çakışma çözümü için. Çakışması çözümleme modelleri doğruluk ve tutarlılık garantileri sağlar ve tutarlılık, kullanılabilirlik, performans, çoğaltma gecikmesine ve olayları coğrafi yük altında karmaşık birleşimlerini hakkında düşünmek için geliştiriciler iş yükünü kaldırın ve çapraz bölge yazma çakışıyor.  

  ![Mult-master çakışma çözümü](./media/multi-region-writers/multi-master-conflict-resolution-blade.png)

Azure Cosmos DB tarafından sunulan çakışma çözüm modellerinin 3 türüne sahip olacaktır. Çakışması çözümleme modelleri semantiği aşağıdaki gibidir: 

**Otomatik** -varsayılan çakışma çözüm İlkesi budur. Bu ilke belirlenmesi, sunucu tarafında çakışan güncelleştirmeleri otomatik olarak Çözümle ve güçlü nihai-tutarlılığı garanti sağlamak Azure Cosmos DB neden olur. Dahili olarak, Azure Cosmos DB yararlanmayı çakışma-serbest-çoğaltılan-veri türlerinde (CRDTs) veritabanı altyapısı tarafından otomatik çakışma çözümü uygular.  

**Son yazma WINS (LWW)** - eşitlenen zaman damgası özelliği bu ilke çakışmaları dayalı üzerinde ya da sistem tarafından tanımlanan çözmenize izni verdiği seçme veya bir özel özellik çakışan kayıtları sürümünde tanımlı. Çakışma çözümü sunucu tarafında gerçekleşir ve en son zaman damgası sürümüyle kazanan seçilir.  

**Özel** -bir saklı yordam kaydederek bir uygulama tanımlı çakışma çözümleme mantığı kaydedebilirsiniz. Veritabanı işlemi, sunucu tarafında himayesinde güncelleştirme çakışması algılandığında saklı yordam çağrılan. Seçeneğini belirleyin, ancak bir saklı yordam kaydı başarısız olursa (veya saklı yordam çalışma zamanında bir özel durum oluşturursa), tüm çakışmaları akış aracılığıyla çakışan sürümlerinin erişebilir ve bunları ayrı ayrı çözümleyebilir.  

## <a name="next-steps"></a>Sonraki adımlar  

Bu makalede Azure Cosmos DB ile genel dağıtılmış birden çok ana kullanmayı learnt. Ardından, aşağıdaki kaynaklara göz atın: 

* [Azure Cosmos DB genel dağıtım nasıl desteklediği hakkında bilgi edinin](distribute-data-globally.md)  

* [Otomatik Yük devretme işlemlerini Azure Cosmos veritabanı hakkında bilgi edinin](regional-failover.md)  

* [Azure Cosmos DB ile genel tutarlılık hakkında bilgi edinin](consistency-levels.md)  

* Birden çok bölgesiyle geliştirmek Azure Cosmos DB - kullanarak [SQL API](tutorial-global-distribution-sql-api.md), [MongoDB API](tutorial-global-distribution-mongodb.md), veya [tablo API](tutorial-global-distribution-table.md)  
