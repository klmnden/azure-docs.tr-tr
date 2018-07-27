---
title: Azure Cosmos DB ile küresel ölçekte çok yöneticili | Microsoft Docs
description: ''
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: rimman
ms.openlocfilehash: 4911a302bf9055948827a72f2e631663b8be741e
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39266051"
---
# <a name="multi-master-at-global-scale-with-azure-cosmos-db"></a>Azure Cosmos DB ile küresel ölçekte çok yöneticili 
 
Dağıtılmış tutarlı verilerin görünümlerini dünya çapında Bakımı zor bir soruna durumdayken yerel gecikme süreleri ile yanıt veren uygulamalar genel olarak geliştirme. Müşteriler veri erişimi gecikmesini geliştirmek, yüksek kullanılabilirlik elde etmek, garantili olağanüstü durum kurtarma sağlamak ve kendi iş gereksinimlerini karşılamak için ihtiyaç duydukları Global olarak dağıtılmış veritabanları kullanın. Azure Cosmos DB'de çok yöneticili yüksek düzeyde kullanılabilirlik (sıra % 99,999), veri ve yerleşik kapsamlı ve esnek bir çakışma çözümü desteği ölçeklenebilirliğiyle yazmak için Tek haneli milisaniyelik gecikme sağlar. Bu özellikler, Global olarak dağıtılmış uygulamaların geliştirilmesini büyük ölçüde basitleştirir. Global olarak dağıtılmış uygulamalar için çok yöneticili destek çok önemlidir. 

![Çok yöneticili mimarisi](./media/multi-region-writers/multi-master-architecture.png)

Azure Cosmos DB çok yöneticili destekle, kapsayıcılar (örneğin, koleksiyonlar, grafikler, tabloları) dünyanın herhangi bir yere Dağıtılmış veri çubuğunda yazma işlemleri gerçekleştirebilirsiniz. Veritabanı hesabınızla ilişkili olan herhangi bir bölgedeki veri güncelleştirebilirsiniz. Bu veri güncelleştirmeleri zaman uyumsuz olarak yayabilir. Hızlı Erişim'i ve verilerinize yazma gecikme süresi yanı sıra, çok yöneticili ayrıca pratik bir çözüm için yük devretme ve Yük Dengeleme konuları sağlar. Özet olarak, Azure Cosmos DB ile yazma gecikme süresi elde < dünya, % 99,999 yazma ve okuma kullanılabilirliği yerinde dünya ve her ikisi de ölçeklendirme olanağı yerinde 99. yüzdebirlik dilimde 10 MS'nin yazma ve dünyanın dört bir yanındaki herhangi bir işleme okuyun.   

> [!IMPORTANT]
> Çok yöneticili desteği önizleme sürümünü kullanmak için özel Önizleme aşamasında olan [kaydolun](#sign-up-for-multi-master-support) şimdi.

## <a name="sign-up-for-multi-master-support"></a>Çok yöneticili desteği için kaydolun

Azure aboneliğiniz zaten varsa, Azure portalında çok yöneticili Önizleme programına katılmak kaydolabilirsiniz. Azure'da yeniyseniz, Azure Cosmos DB'ye 12 aylık ücretsiz erişim elde edeceğiniz [ücretsiz denemeye](https://azure.microsoft.com/free) kaydolun. Çok yöneticili Önizleme programına erişim istemek için aşağıdaki adımları tamamlayın.

1. [Azure portalı](https://portal.azure.com)'nda, **Kaynak oluştur** > **Veritabanları** > **Azure Cosmos DB**'ye tıklayın.  

2. Yeni hesap sayfasında, Azure Cosmos DB hesabınız için bir ad belirtin, API, abonelik, kaynak grubunu ve konumu seçin.  

3. Ardından **bugün Önizleme için kaydolun** birden çok ana Önizleme alanı altında.  

   ![Çok yöneticili önizlemesi için kaydolun](./media/multi-region-writers/sign-up-for-multi-master-preview.png)

4. İçinde **bugün Önizleme için kaydolun** bölmesinde tıklayın **Tamam**. İsteği gönderdikten sonra durum değişikliklerini **onay bekleyen** hesap oluşturma dikey penceresinde.  

İsteği gönderdikten sonra isteğiniz onaylandı bir e-posta bildirimi alırsınız. İsteklerin yüksek sayıda olması nedeniyle bildirimin gelmesi bir hafta alabilir. İsteği tamamlamak için bir istek bileti oluşturmanız gerekmez. İstekler alındıkları sırada incelenecektir.

## <a name="a-simple-multi-master-example--content-publishing"></a>Basit bir çok yöneticili örnek – içerik yayımlama  

Çok yöneticili desteği ile Azure Cosmos DB kullanmayı açıklar gerçek bir senaryo bakalım. Azure Cosmos DB üzerinde oluşturulmuş bir içerik yayımlama platform göz önünde bulundurun. Bu platform, Yayımcılar ve tüketiciler için harika bir deneyim için karşılaması gereken bazı gereksinimler aşağıda verilmiştir. 

* Yazarlar hem de aboneler tüm dünyayı yayılır.  

* Yazarlar kendi yerel (yakın) bölgeye (yazma) makaleleri yayımlamanız gerekir.  

* Yazarlar okuyucular/dünya çapında dağıtılan aboneleri, makale sahip.  

* Aboneler, yeni bir makale yayımlandığında bir bildirim almanız gerekir.  

* Aboneleri yerel bölgelerinden makaleleri okuyabilirsiniz olması gerekir. Ayrıca bu makaleler için gözden geçirmeleri eklemek çözebilmeleri.  

* Herhangi bir makale yazarı dahil olmak üzere mümkün görünümü tüm incelemeleri makaleler için yerel bir bölgeden bağlı olmalıdır.  

Kullanıcıları ve yayımcılarına makaleler, milyarlarca ile milyonlarca varsayılarak yakında yerleşim yeri erişim güvence altına almak birlikte, Ölçek sorunları üstesinden gelir sunuyoruz. Böyle bir kullanım örneği bir mükemmel Azure Cosmos DB için çok yöneticili adaydır. 

## <a name="benefits-of-having-multi-master-support"></a>Çok yöneticili desteğine sahip avantajları 

Çok yöneticili desteği genel olarak dağıtılan uygulamalar için gereklidir. Çok yöneticili, oluşturulan [birden çok ana bölge](distribute-data-globally.md) bir herhangi bir yazma modeli (etkin-etkin deseni) eşit katılmak ve verilerin herhangi bir zamanda ihtiyacınız olduğunda kullanılabilir durumda olduğundan emin olmak için kullanılır. Tek bir bölge için yapılan güncelleştirmeler, zaman uyumsuz olarak (hangi sırayla kendi ana bölge) diğer tüm bölgelere yayılır. Tüm çoğaltmalar verilerinizin yakınsama ve emin olmak için çok yöneticili yapılandırma gibi ana bölgelerde otomatik olarak çalışan azure Cosmos DB bölgeler çalışma [genel tutarlılık ve veri bütünlüğü](consistency-levels.md). Aşağıdaki görüntüde, bir tek yöneticili ve çok yöneticili için okuma/yazma çoğaltma gösterilmektedir.

![Tek yöneticili ve çok yöneticili](./media/multi-region-writers/single-vs-multi-master.png)

Kendi uygulama çok yöneticili yük geliştiriciler ekler. Çok yöneticili, kendi uygulamak için çalışan büyük ölçekli müşteriler yapılandırma ve dünya çapında bir çok yöneticili yapılandırmayı sınama saat yüzlerce harcayabilir ve birçok mühendisleri izlemek ve çok yöneticili korumak için tek bir proje olan ayrılmış bir dizi sahip Çoğaltma. Oluşturma ve yönetme çok yöneticili Kurulum kendi zaman, uygulamada yenilik uzağa kaynakları alır ve çok daha yüksek maliyetleri sonuçlanır. Azure Cosmos DB çok yöneticili destek "out-of--box" sağlar ve geliştiriciler bu yükünü kaldırır.  

Özet olarak, birden çok ana aşağıdaki avantajları sağlar:

* **Daha iyi bir olağanüstü durum kurtarma, kullanılabilirlik ve yük devretme yazma**-çok yöneticili, büyük ölçüde iş açısından önemli bir veritabanı yüksek kullanılabilirliğini korumak için kullanılabilir. Birincil bölgenin kesinti veya bölgesel bir olağanüstü durum nedeniyle kullanılamaz hale geldiğinde Örneğin, birden çok ana veritabanına veri bir bölgeden yük devretme bölgesine çoğaltabilirsiniz. Böyle bir yük devretme bölge uygulamayı desteklemek için bir tam işlevsel ana bölge hizmet verecektir. Bölgeleri kalan % garantili yazma kullanılabilirlik > 99,999 coğrafi olarak farklı çok yöneticileriyle kullanabilir olduğundan çok yöneticili doğal felaketler, bölgesel elektrik kesintileriyle veya sabotaj, bakımından daha büyük "survivability" koruma sağlar. 

* **Son kullanıcılar için geliştirilmiş yazma gecikmesi** - yaklaştıkça (hizmet), verileri son kullanıcılar için deneyimi daha iyi olacaktır. Örneğin, Avrupa kullanıcılar varsa, ancak veritabanı ABD veya Avustralya, yaklaşık 140 ms gecikme süresi olur ve ilgili bölge için 300 ms. Gecikmeler başlamak birçok popüler oyun, bankacılık gereksinimleri veya etkileşimli uygulamalar (web veya mobil) edilemez. Gecikme süresi, yüksek kaliteli bir deneyim müşterinin algısı büyük bir bölümünü oynatır ve bazı belirgin ölçüde kullanıcıların davranışını etkilemek için kanıtladılar. Teknoloji geliştikçe ve özellikle AR, VR ve MR gelişinden ile daha kapsamlı ve gerçek deneyimleri gerektiren geliştiriciler artık yazılım sistemleri katı gecikme süresi gereksinimlerine sahip üretmeniz gerekir. Bu nedenle, yerel olarak kullanılabilir uygulamaları ve verileri (içerik uygulamaları için) sahip daha önemli değildir. Azure Cosmos DB'de çok yöneticili, olabildiğince hızlı normal yerel okur ve yazar ve coğrafi dağıtım tarafından genel olarak geliştirilmiş performans.  

* **Geliştirilmiş yazma ölçeklenebilirliği ve yazma üretimi** - çok yöneticili daha yüksek performans verir ve doğruluk ile birden çok tutarlılık modeli sunarak sırasında daha fazla kullanımı garanti eder ve SLA'lar ile desteklenen. 

  ![Çok yöneticili ile ölçekleme yazma üretimi](./media/multi-region-writers/scale-write-throughput.png)

* **Bağlantısı kesilmiş ortamlarda (örneğin, uç cihazlarına) daha iyi destek** -çok yöneticili kullanıcılardan edge cihazına bağlantısı kesilmiş bir ortam en yakın bir bölgede tüm çoğaltma veya verilerin bir alt kümesini sağlar. Bu senaryo, tipik bir satış Otomasyon sistemleri, bir kişinin dizüstü (bağlantısı kesilmiş bir cihaz) temsilcisinin için ilgili verilerin bir alt kümesini depoladığı zorlayın. Dünyanın herhangi bir yerde bulunan ana bölge bulutta, uzak uç cihazlarına kopyalama hedefi olarak çalışabilir.  

* **Yük Dengeleme** -çok yöneticili yük boyunca uygulama kullanıcıları/iş yükleri aşırı yüklü bir bölgeden bölgeye nereye yük eşit olarak dağıtılmış taşıyarak dengelenir. Kapasite, yeni bir bölgeye ekleyerek ve ardından yeni bölge için bazı yazma geçiş kolayca genişletilebilir yazın. 

* **Daha iyi sağlanan kapasitesine kullanımı** - çok yöneticili yazma yoğunluklu ve karma iş yükleri için birden çok bölgede sağlanan kapasiteyi doygunluğa ulaştırabilir...  Bazı durumlarda sağlanması için daha az bir işlem hacmi gerektiren bu nedenle, okuma ve yazma işlemleri daha eşit, dağıtabilirsiniz ve müşteriler için daha fazla maliyet tasarrufu için müşteri adaylarını.  

* **Daha basit ve daha dayanıklı uygulama mimarileri** -uygulamalar için çok yöneticili yapılandırma taşıma veri esnekliği garantisi.  Tüm karmaşıklığı gizleme Azure Cosmos DB ile uygulama tasarımı ve mimarisi önemli ölçüde basitleştirin. 

* **Riske girmeden yük devretme testi** -yük devretme testi sahip olmaz herhangi bir performans düşüşü yazma aktarım hızını. Çok yöneticili, diğer tüm bölgeler tam yöneticileri olduğundan yük devretme yazma aktarım hızını kadar etkisi olmaz.  

* **Daha düşük toplam maliyeti Ownership(TCO) ve DevOps** -toplantı ölçeklenebilirlik, performans, genel dağıtım, Kurtarma süresi hedeflerini pahalı eklentiler ya da bekleme durumunda olağanüstü durum kadar olan bir yedekleme altyapısı bakımı nedeniyle pahalı genellikle sağlar. Sektör lideri SLA'lar ile desteklenen Azure Cosmos DB çok ana şablon ile geliştiriciler artık, oluşturma ve kendilerini "arka uç mantığınızı Yapıştırıcı" koruma gerektiren ve görev açısından kritik iş yüklerini çalıştıran rahat alın. 

## <a name="use-cases-where-multi-master-support-is-needed"></a>Çok yöneticili destek gerekmesi halinde kullanım örnekleri

Azure Cosmos DB için çok yöneticili çeşitli kullanım örnekleri vardır: 

* **IOT** -Azure Cosmos DB çok yöneticili IOT veri işleme Basitleştirilmiş dağıtılmış uygulama için izin verir. İade çakışma ücretsiz kullanan coğrafi olarak dağıtılmış uç dağıtımlar çoğaltılan verileri türleri genellikle zaman serisi verilerini birden çok konumlardan izlemeniz gerekiyorsa. Her bir cihaza en yakın bölgelerden birine bağlantılı ve bir cihaz (örneğin, bir araba) izler ve başka bir bölgeye yazmak için dinamik olarak barındırılması.  

* **E-ticaret** -e-ticaret senaryolarda harika kullanıcı deneyimini işlemlerini yüksek kullanılabilirlik ve dayanıklılığı hata senaryoları için gerekiyor. Bir bölgede başarısız olursa, kullanıcı oturumları, Alışveriş sepetleri, etkin sorunsuz bir şekilde durumu kaybı olmadan başka bir bölgeye göre işlenmek üzere listesi gerekir istiyor. Bu arada, kullanıcı tarafından yapılan güncelleştirmelerin uygun şekilde işlenmesi gereken (örneğin, ekler ve alışveriş sepetine öğesinden kaldırır üzerinden aktarım gerekir). Çok yöneticili, Azure Cosmos DB bu senaryolara düzgün bir şekilde, kullanıcının açısından tutarlı bir görünüm sürdürürken etkin bölgeler arasında sorunsuz bir geçiş ile işleyebilir. 

* **Sahtekarlık/Anomali algılama** -genellikle kullanıcı etkinliğinde veya hesap etkinliği izleme uygulamaları Global olarak dağıtılmış ve çeşitli etkinliklerde aynı anda izlemek gerekir. Oluşturma ve bir kullanıcı puanları bakımı sırasında farklı coğrafi bölgeler Eylemler aynı anda risk ölçümleri satır içi tutmak puanları güncelleştirmeniz gerekir. Azure Cosmos DB, geliştiricilerin uygulama düzeyinde çakışma senaryoları ele gerekmez sağlanması. 

* **İşbirliği** - ürün satış ya da kullanılacak medya gibi makale özelliği sayesinde Popülerlik derece dayalı uygulamalar için vs. Özellikle ödemiştir ücretli veya gerçek zaman reklam kararlarına olması gereken durumlarda coğrafi bölgeler arasında popülerliği izleme karmaşık, alabilirsiniz. Sıralama, sıralama ve çok sayıda bölgede dünya çapındaki Azure Cosmos DB ile gerçek zamanlı raporlama özellikleri çok az çabayla ve düşük gecikme sürelerini ödün olmadan geliştiricilerin sağlar. 

* **Kullanım ölçümü** - sayım ve kullanım düzenlenmesi (API çağrıları gibi işlemler/saniye, dakika kullanılır) kullanarak Azure Cosmos DB çok yöneticili kolaylığıyla genel olarak uygulanabilir. Her iki doğruluğu sayıları ve gerçek zamanlı düzenleme, yerleşik bir çakışma çözümü sağlar. 

* **Kişiselleştirme** - ödülleri işaret bağlılık gibi eylemleri tetikleyen coğrafi olarak dağıtılmış sayaçları koruma veya görünümler, yüksek kullanılabilirlik ve coğrafi olarak dağıtılmış Basitleştirilmiş kişiselleştirilmiş kullanıcı oturumunu uygulama Azure Cosmos DB tarafından sağlanan sayım kolaylıkla teslim yüksek performanslı uygulamalar sağlar. 

## <a name="conflict-resolution-with-multi-master"></a>Çok yöneticili ile çakışma çözümü 

Çok yöneticili, genellikle iki (veya daha fazla) çoğaltmaları aynı kaydın aynı anda iki veya daha fazla farklı bölgelerdeki farklı yazarları tarafından güncelleştirilmiş zorluktur. Eşzamanlı yazma iki farklı sürümü de aynı kaydın ve yerleşik bir çakışma olmadan neden olabilir ve uygulamanın kendisi Bu tutarsızlığı çözmek için çakışma çözümü gerçekleştirmeniz gerekir.  

**Örnek** -Kalıcılık mağazada alışveriş sepeti uygulaması ve bu uygulamayı iki bölgede dağıtılmış olduğundan, Azure Cosmos DB kullandığınızı varsayalım: Doğu ABD ve Batı ABD.  Yaklaşık aynı zamanda, San francisco'daki bir kullanıcı bir öğe, alışveriş sepetine (örneğin, bir kitap) Doğu ABD bir envanter yönetimi işlemi sırasında eklerse, farklı bir alışveriş sepetine öğe (örneğin, yeni bir telefon) yanıt olarak bir s aynı söz konusu kullanıcı için geçersiz kılar Yayın tarihi Gecikmeli upplier bildirimi. T1 zamanında, iki bölgeleri alışveriş sepeti kayıtlara farklıdır. Veritabanı tutarsızlığı çözümleme için kullanacağı, çoğaltma ve çakışma çözme mekanizması ve sonunda bir alışveriş sepetini iki sürümünü seçilir. Çoğunlukla çok yöneticili veritabanları (örneğin, son yazma WINS) tarafından uygulanan Çakışma çözümlemesi buluşsal yöntemlerini kullanarak kullanıcı veya uygulamanın hangi sürümünün seçili tahmin etmek için mümkün değildir. Her iki durumda da, veri kaybı olmamasına veya beklenmeyen davranış oluşabilir. Yeni satın alma öğesinin (diğer bir deyişle, bir kitap) kullanıcının seçimini Doğu bölgesi sürümü seçerseniz, sonra kaybolur ve Batı bölgesinde seçili ise, daha önce seçilen öğenin (diğer bir deyişle, telefon) hala arabasında olduğu. Her iki durumda da, bilgi kaybolur. Son olarak, alışveriş inceleyerek başka bir işlem sepete saatleri arasında T1 ve T2 gittiğini kararlı olmayan davranışı bakın. Örneğin, Karşılama ambarı seçer ve maliyetleri sevkiyat sepet güncelleştiren bir arka plan işlemi sepet nihai içeriğiyle çakışan sonuçlar. Sepet tek bir öğe, kitap yakında olsanız bile işlem Batı bölgesinde çalışıyor ve gerçeklik alternatif 1 olur, iki öğe nakliye maliyetlerini işlem. 

Azure Cosmos DB veritabanı altyapısının içinde çakışan yazmaları işlemek için mantığı uygular. Azure Cosmos DB sunar **kapsamlı ve esnek çakışma çözümü desteği** çeşitli çakışma otomatik dahil Çözümleme yöntemleri sunarak (iade çakışma ücretsiz çoğaltılan veri türleri), son yazma WINS (LWW) ve özel () Saklı yordamı) otomatik Çakışma çözümlemesi için. Çakışma çözümleme modelleri, doğruluk ve tutarlılık garantileri sağlar ve tutarlılık, kullanılabilirlik, performans, çoğaltma gecikmesi ve karmaşık birleşimlerini coğrafi yük devretmeleri altında olaylar hakkında düşünmek zorunda geliştiricilerden iş yükünü kaldırın ve bölgeler arası yazma çakışıyor.  

  ![Mult-master çakışma çözümü](./media/multi-region-writers/multi-master-conflict-resolution-blade.png)

Azure Cosmos DB tarafından sunulan Çakışma çözümlemesi modelleri 3 tür olacaktır. Çakışma çözümleme modelleri semantiği aşağıdaki gibidir: 

**Otomatik** -varsayılan çakışma çözümü İlkesi budur. Bu ilke seçerek, sunucu tarafı çakışan güncelleştirmeleri otomatik olarak çözümlemek ve güçlü-son-tutarlılık garantileri sağlamak Azure Cosmos DB neden olur. Dahili olarak, Azure Cosmos DB veritabanı altyapısının içinde yararlanarak çakışma-ücretsiz-çoğaltılan-veri türleri (CRDTs) tarafından otomatik çakışma uygular.  

**Son yazma WINS (LWW)** - eşitlenen zaman damgası özelliği bu ilke çakışmaları temel üzerinde sistem tarafından tanımlanan çözmenize sağlayacaktır seçme veya bir özel özellik tanımlanmış çakışan kayıtları sürümünde. Sunucu tarafında çakışma olur ve en son zaman damgası sürümüyle birinci olarak seçilir.  

**Özel** -saklı yordam kaydederek bir uygulama tanımlı çakışma çözümleme mantıksal kaydedebilirsiniz. Saklı yordam bir veritabanı işlemi, sunucu tarafında himayesinde güncelleştirme çakışmaları algılanması üzerine çağrılır. Seçeneğini belirleyin, ancak bir saklı yordam kaydı başarısız (veya saklı yordam çalışma zamanında bir özel durum oluşturursa), tüm çakışmaları akış aracılığıyla çakışan sürümlerinin erişebilir ve bunları tek tek çözün.  

## <a name="next-steps"></a>Sonraki adımlar  

Bu makalede Azure Cosmos DB ile küresel olarak dağıtılan çok yöneticili kullanmayı modellerin. Ardından, aşağıdaki kaynaklara göz atın: 

* [Azure Cosmos DB genel dağıtımını nasıl desteklediği hakkında bilgi edinin](distribute-data-globally.md)  

* [Azure Cosmos DB'de otomatik yük devretme işlemleri hakkında bilgi edinin](regional-failover.md)  

* [Azure Cosmos DB ile genel tutarlılık hakkında bilgi edinin](consistency-levels.md)  

* Birden çok bölgeye ile geliştirme - Azure Cosmos DB kullanarak [SQL API](tutorial-global-distribution-sql-api.md), [MongoDB API'si](tutorial-global-distribution-mongodb.md), veya [tablo API'si](tutorial-global-distribution-table.md)  
