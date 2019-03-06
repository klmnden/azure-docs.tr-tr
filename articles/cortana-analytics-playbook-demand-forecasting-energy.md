---
title: Talep tahmini enerji için Cortana Intelligence çözüm şablonu kitabı
description: İsteğe bağlı bir enerji yardımcı şirket için tahmini yardımcı olan bir Microsoft Cortana Intelligence çözüm şablonuyla.
services: machine-learning
author: ilanr9
manager: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/24/2016
ms.author: garye
ms.openlocfilehash: bb5ef610e55495c372a47ff78e3252c9d8ec7055
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57435925"
---
# <a name="cortana-intelligence-solution-template-playbook-for-demand-forecasting-of-energy"></a>Talep tahmini enerji için Cortana Intelligence çözüm şablonu kitabı
## <a name="executive-summary"></a>Yönetici Özeti
Son birkaç yılda, nesnelerin interneti (IOT), alternatif enerji kaynakları ve büyük veri yardımcı programı ve enerji etki alanında geniş fırsatları oluşturmak için birleştirilmiş. Aynı zamanda, yardımcı program ve tüm enerji sektörü tüketim enerji kullanımını denetlemek için daha iyi yollarını zorlu tüketicileriyle düzleştirme gördünüz. Bu nedenle, akıllı şebeke şirketler ve yardımcı programı yenilik yapın ve kendilerini yenilemek için harika ihtiyacı olan. Ayrıca, birçok güç ve yardımcı kılavuzlar, güncel ve korumak ve yönetmek çok yüksek maliyetli hale gelmektedir. Geçen yıl içinde takım enerji etki alanı içinde yaşadığımız sayıda üzerinde çalışmaktadır. Çoğu durumda, ISV (bağımsız yazılım satıcıları) ve yardımcı programlar için gelecekteki enerji talebini tahmin içine mı arıyorsunuz bu bildirimi sırasında karşılaştık. Bu tahminler, şimdiki ve gelecekteki iş önemli bir rol oynar ve çeşitli kullanım örnekleri için temel haline geldi. Bunlar kısa ve uzun süreli power yük tahmin, ticaret, Yük Dengeleme, kılavuz iyileştirme vb. içerir. Büyük veri ve Gelişmiş analiz (AA) yöntemleri Machine Learning (ML) gibi anahtar etkinleştiricilerden doğru ve güvenilir tahminler üretmek için olan.

Playbook'u, iş ve başarılı bir geliştirme için gereken analitik yönergeleri araya ve çözüm dağıtımı enerji talebini tahmin. Bu önerilen yönergeler, yardımcı programlar, veri uzmanları ve tam olarak çalışır hale getirilen, bulut tabanlı, Talep tahmini çözümleri oluşturma, veri mühendisleri yardımcı olabilir. Yalnızca kendi büyük veri ve Gelişmiş analiz yolculuğunun başlangıç şirketler için böyle bir çözümü, uzun vadeli bir akıllı şebeke stratejisi, ilk çekirdek temsil edebilir.

> [!TIP]
> Bu şablon bir mimari genel bakış sağlayan bir diyagram indirmek için bkz [Cortana Intelligence çözüm şablonu mimarisi için Talep tahmini enerji](cortana-analytics-architecture-demand-forecasting-energy.md).
>
>

## <a name="overview"></a>Genel Bakış
Bu belge, iş, verileri ve Cortana Intelligence'ı kullanma ve de belirli Azure Machine Learning (AML) uygulama ve dağıtım tahmini enerji çözümleri için teknik yönlerini kapsar. Belge üç ana bölümden oluşur:

1. Kurumsal yaklaşım
2. Veri anlama
3. Teknik uygulama

**İş anlama** bölümü gerektiren anlamak ve bir yatırım karar vermeden önce göz önünde bulundurun iş en boy özetler. Bu, eldeki Tahmine dayalı analiz ve makine öğrenimi gerçekten etkili ve geçerli olduğundan emin olmak için iş sorununu nitelemek nasıl açıklar. Daha fazla belge, makine öğrenimi ve enerji tahmini sorunları ele almak için nasıl kullanıldığı hakkındaki temel bilgileri açıklar. Bunu, önkoşulları ve kullanım nitelik ölçütlerini açıklar. Bazı örnek kullanım örneklerinize ve İş Gerekçesi senaryoları da sağlanır.

Ana içerik için herhangi bir makine öğrenimi çözümünüzü verilerdir. **Veri anlama** bölümünü bu belgenin bazı önemli yönlerini kapsar. Bunu, enerji tahmini, veri kalitesi gereksinimleri ve genellikle hangi veri kaynaklarını mevcut için gereken veri türünü açıklar. Ayrıca ham verileri gerçekte modelleme bölümü sürücü veri özellikleri hazırlamak için nasıl kullanıldığını açıklar.

Üçüncü kısmı olan belgenin kapsar **teknik uygulama** bir çözüm yönü. Özellik Mühendisliği ve modelleme data science Process'i özünde ve bazı ayrıntılı olarak bu nedenle açıklanır. Bu Tahmine dayalı analiz çözümlerini bulut dağıtımı için önemli bir araç olan web hizmetleri kavramı ele alınmaktadır. Biz de tipik bir uçtan uca çalışır hale getirilen çözüm mimarisini özetler.

Ayrıca, belgeyi başka bir etki alanı ve teknoloji anlamak için kullanabileceğiniz bir başvuru malzemesi içerir.

Biz bu belgede daha ayrıntılı veri bilimi işlemi kapsayacak şekilde düşünmüyorsanız dikkat edin önemlidir, matematiksel ve teknik yönden. Bu ayrıntıları bulunabilir [Azure Machine Learning hizmeti belgeleri](https://azure.microsoft.com/services/machine-learning/) ve [blogları](https://blogs.microsoft.com/blog/tag/azure-machine-learning/).

### <a name="target-audience"></a>Hedef kitle
Bu belgenin hedef kitlesi, işletme ve teknik bilgi elde etmek için istediğiniz personeli olduğundan ve çözümleri ve özellikle enerji tahmini etki alanında bunlar nasıl kullanıldığını anlama Machine Learning tabanlı.

Veri bilimcileri, ayrıca bir enerji çözümü tahmini dağıtımı sürücüleri yönelik yüksek düzeyli işlem daha iyi bir anlayış kazanmak için bu belgeyi okuma yararlı olabilir. Bu bağlamda, de iyi bir taban çizgisi oluşturmak için kullanılabilir ve daha fazla bilgi için başlangıç noktası ayrıntılı ve malzeme Gelişmiş.

### <a name="industry-trends"></a>Sektör eğilimleri
Son birkaç yılda büyük fırsatlar yardımcı programı ve enerji alanı oluşturmak için IOT, alternatif enerji kaynakları ve büyük veri birleştirilmiş. Aynı zamanda, yardımcı program ve tüm enerji kesimler tüketim enerji kullanımını denetlemek için daha iyi yollarını zorlu tüketicileriyle düzleştirme gördünüz.

Pek çok yardımcı ve akıllı enerji şirketlerinin öncü [akıllı şebeke](https://en.wikipedia.org/wiki/Smart_grid) birkaç kullanım dağıtarak kılavuz tarafından oluşturulan verilerin yaptığınız çalışmaları kullanın. Birçok kullanım örnekleri, elektrik devralınmış özelliklere çalışmalarınızı: Bu olamaz toplanan veya kenara envanter olarak depolanır. Bu nedenle, hangi üretilen kullanılması gerekir. Daha verimli olmasını istediğiniz yardımcı programlar gereksinim güç tüketimini yalnızca tahmin etmek, daha yüksek bir beceri verir çünkü **Bakiye arz ve talep**, böylece enerji atık önleme **greenhouse gaz azaltın Arabellek**ve maliyetini denetleyebilirsiniz.

Maliyetleri konuşurken fiyatı bir diğer önemli unsuru yoktur. Yardımcı programlar arasında Power ticari yeni yetenekleri için harika bir gereksinim getirildi **gelecek talebi ve elektrik gelecekteki fiyat tahmini**. Bu şirketler üretim hacimleri belirlemek yardımcı olabilir.

'Akıllı' sözcük kullanıyoruz, aslında öğrenin ve ardından tahminlerde bir kılavuz için diyoruz. Tüketim alışkanlıklarıyla tahmin yanı **için otomatik olarak ayarlayın ve geçici bir aşırı yük durumlarından öngörüyor**. Uzaktan tüketim (Bu akıllı ölçüm cihazlarından yardımıyla) düzenleyerek yerelleştirilmiş aşırı yük durumlarından işlenebilir. **İlk tahmin etme ve ardından çalışan**, kılavuz kendisini daha akıllı zamanla kolaylaştırır.

Biz, geleceği tahmin kapsayan kullanım örneklerinin belirli ailesinde odaklanmak bu belgenin geri kalanında kısa vadede ve uzun vadeli enerji talebi. Biz bu alanlarda birkaç ay boyunca çalışmaktadır ve bazı bilgi ve sektör sınıf sonuçlar olanak beceri elde etmiştir. Diğer kullanım örnekleri de belgede yakın gelecekte ele alınmaktadır.

## <a name="business-understanding"></a>İşin Gereksinimlerini Anlama
### <a name="business-goals"></a>İş hedefleri
**Enerji tanıtım** hedeftir tipik Tahmine dayalı analiz ve makine öğrenimi çok kısa bir zaman dilimi içinde dağıtılabilir çözüm göstermek için. Özellikle, iş değeri kullanılabilir hızla gerçekleşen ve üzerinde kullanılabilir böylece enerji talebi tahmin çözümleri etkinleştirme kazanmasının geçerli olur. Playbook'u bilgileri aşağıdaki hedefleri yerine getirmeye müşteri yardımcı olabilir:

* Kısa süre değerinin machine learning tabanlı çözümüdür
* Bir pilot genişletme olanağı kullanım örneği, diğer kullanım örnekleri veya kendi iş ihtiyaca göre daha geniş bir kapsama
* Hızlı bir şekilde Cortana Intelligence Suite ürün bilgisi edinin

Bu hedefleri göz önünde bulundurarak, iş ve bu hedefleri elde etmeye yardımcı olacak teknik bilgi sunan bu playbook amaçlar.

### <a name="power-load-and-demand-forecasting"></a>Güç yük ve Talep tahmini
Enerji sektörü bağlamında hangi isteğe bağlı olarak tahmin kritik iş sorunlarını çözmenize yardımcı olabilecek birçok yolu olabilir. Aslında, Talep tahmini foundation sektörde çok çekirdekli kullanım örnekleri için kabul edilebilir. Genel olarak, şu iki tür enerji talep tahminleri düşünün: kısa vadeli ve uzun vadeli. Her biri farklı bir amaca hizmet ve farklı bir yaklaşım kullanın. İkisi arasındaki temel fark, zaman aralığı için biz tahmin geleceğe anlamına gelir tahmin Ufuk ' dir.

#### <a name="short-term-load-forecasting"></a>Kısa süreli yük tahmin
Enerji talebi bağlamında, kısa süreli yük tahmin (STLF) kılavuz (veya bir bütün olarak kılavuz) çeşitli bölümleri yakın gelecekte tahmini toplam yükü olarak tanımlanır. Bu bağlamda, kısa vadeli süresini 1 saatin 24 saatlik aralıkta olarak tanımlanır. Bazı durumlarda, 48 saat kapsamını da mümkündür. Bu nedenle, STLF kılavuz bir işletimsel kullanım örneği, yaygın olarak görülür. Kullanım örnekleri temelli STLF bazı örnekleri aşağıda verilmiştir:

* Arz ve talep Dengeleme
* Güç alım-satım desteği
* Pazar yapma (ayarı power fiyatı)
* Kılavuz işletimsel en iyi duruma getirme
* [İsteğe yanıt](https://en.wikipedia.org/wiki/Demand_response)
* Talep tahmini en üst seviyeye
* İsteğe bağlı tarafı Yönetimi
* Yük Dengeleme ve önleme aşırı yükleme
* Uzun süreli yük tahmin
* Hata ve anomali algılama
* Yoğun curtailment/Dengeleme

STLF modeli çoğunlukla yakın geçmişi tabanlı (son gün veya hafta) tüketim verilerini ve kullanım tahmini önemli bir tahmin unsuru olarak sıcaklık. Önümüzdeki bir saati tahmin doğru sıcaklık almak ve ayarlamak için 24 saat daha az bir sınama şimdi gün gelmektedir. Bu modeller dönemsel desenleri ya da uzun süreli tüketim eğilimleri daha az duyarlı olan.

SLTF çözümleri (hizmet istek) tahmin çağrısı yüksek hacimli üreteceği Ayrıca bunlar saatlik bazda ve bazı durumlarda bile daha yüksek sıklık ile çağrılmakta bu yana. Ayrıca, burada her ayrı alt istasyon veya transformer tek başına model olarak temsil edilir ve bu nedenle tahmin istek hacmi daha da fazla implantation görmek için oldukça sık rastlanıyor.

#### <a name="long-term-load-forecasting"></a>Uzun süreli yük tahmin
' % S'hedefi, uzun süreli yük tahmin (LTLF) ile birden çok aya (ve bazı durumlarda bir yıl sayısı için) 1 hafta arasında bir süresini güç talebini tahmin olmaktır. Gelecekte bu aralığı çoğunlukla planlama için geçerlidir ve yatırım kullanım örnekleri.

Uzun vadeli senaryoları için birden çok yıl (en az 3 yıl) aralığını kapsayan yüksek kaliteli veri olması önemlidir. Bu modeller genellikle mevsimsellik desenleri geçmiş verilerin ayıklanacağı ve gibi dış predicators kullanan hava durumu ve iklim desenleri olarak.

Tahmin gelecekte uzun olduğundan, daha az doğru tahmin olabilir açıklamak önemlidir. Bu nedenle, bazı güvenle aralıkları birlikte bunların planlama işlemine olası değişim etkimesi insanlar izin gerçek tahmin oluşturmak önemlidir.

Tüketim senaryo LTLF için çoğunlukla planlama olduğundan, biz çok daha düşük tahmin birimleri (karşılaştırıldığında STLF) bekleyebilirsiniz. Biz genellikle bu tahminler, Excel veya Power BI gibi bir görselleştirme araçları içine katıştırılmış görür ve kullanıcı tarafından el ile çağrılan.

### <a name="short-term-vs-long-term-prediction"></a>Kısa vadeli vs. Uzun vadeli tahmin
Aşağıdaki tabloda, en önemli öznitelikleri açısından STLF ve LTLF karşılaştırılır:

| Öznitelik | Kısa süreli yük tahmin | Yükü tahmin'uzun süreli |
| --- | --- | --- |
| Ufuk tahmin |1 saat 48 saattir |1 ile 6 ay veya daha fazla bilgi için |
| Verilerin ayrıntı düzeyi |Saatlik |Saatlik veya günlük |
| Tipik kullanım durumları |<ul><li>İsteğe bağlı/tedarik dengeleme</li><li>Tahmini saatlik seçin</li><li>İsteğe yanıt</li></ul> |<ul><li>Uzun süreli planlama</li><li>Planlama Kılavuzu varlıklar</li><li>Kaynak planlama</li></ul> |
| Tipik adaylarının |<ul><li>Gün veya hafta</li><li>günün saati</li><li>Saatlik sıcaklık</li></ul> |<ul><li>Yılın ayı</li><li>Ayın günü</li><li>Uzun vadeli sıcaklık ve iklim</li></ul> |
| Geçmiş veri aralığı |İki ila üç yılda değerinde veri |5-10 yıl değerinde veri |
| Tipik doğruluğu |MAPE * % 5 veya daha düşük |MAPE * % 25 veya daha düşük |
| Tahmin sıklığı |Her saat veya 24 saatte oluşturulan |Aylık, üç aylık veya yıllık sonra üretilen |

\*[MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) – ortalama yüzde hata anlama

Bu tablodan görüldüğü gibi kısa ve uzun vadede bunlar farklı iş ihtiyaçlarını temsil eder ve farklı dağıtım ve tüketim düzenlerine sahip gibi senaryoları tahmin ayırt etmek çok önemlidir.

### <a name="example-use-case-1-esmart-systems--overload-optimization"></a>Örnek Kullanım örneği 1: eSmart sistemleri – aşırı yükleme en iyi duruma getirme
Önemli bir rol, bir [akıllı şebeke](https://en.wikipedia.org/wiki/Smart_grid) dinamik ve sürekli olarak iyileştirin ve değişen tüketim düzenlerine için ayarlayın. Kısa vadeli değişikliklerden çoğunlukla sıcaklık dalgalanmalarına göre neden olduğu güç tüketimini etkilenebilir (*örn*, daha fazla güç hava durumu veya ısıtma kullanılır). Aynı zamanda, güç tüketiminde de uzun vadeli eğilimleri etkiler. Bunlar, mevsimsellik etkileri, Ulusal tatilleri, uzun vadeli tüketim büyüme ve tüketici dizini, Petrol fiyat ve gayrisafi yurt içi HASILA gibi daha ekonomik etkenler olabilir.

Bu kullanım örneğindeki [eSmart](https://www.esmartsystems.com/) herhangi belirli alt istasyon üzerinde aşırı yükleme durumuna kılavuzunun eğilimini tahmin sağlayan bulut tabanlı bir çözümü dağıtmak için istiyordu. Özellikle, bir Acil eylem önlemenize veya bu sorunu çözmek için yapılması için sonraki bir saat içinde tekrar olasılığı şalt tanımlamak eSmart istiyordu.

Bir doğru ve hızlı tahmin gerçekleştirme üç Tahmine dayalı modelleri uygulama gerektirir:

* Uzun vadeli modeli, her alt istasyon üzerinde güç tüketimi sırasında sonraki birkaç hafta veya ayda tahmin sağlar.
* Bir sonraki saatte belirli bir alt istasyon üzerinde aşırı yükleme durum öngörü sağlayan kısa süreli modeli
* Birden fazla senaryoyu gelecekteki sıcaklığını tahmin sağlayan sıcaklık modeli

Uzun vadeli modelinin amacı, eğilimini (Güç iletim kapasitelerini) sonraki hafta veya ayda sırasında aşırı yükleme tarafından şalt rank. Bu, kısa vadeli tahmin için giriş olarak hizmet verecek şalt kısa bir listesi oluşturulmasına izin verir. Uzun vadeli modeli için önemli bir tahmin unsuru sıcaklık olduğundan, sürekli olarak çok senaryo sıcaklık tahminlerini oluşturmak ve bunları uzun vadeli modeli giriş olarak akış gerek yoktur. Kısa vadeli tahmin sonra hangi alt istasyon bir sonraki saat içinde tekrar olasıdır tahmin etmek için çağrılır.

Kısa vadede ve uzun modelleri, tek tek her alt istasyon dağıtılır. Bu nedenle, bu modeller pratik yürütülmesini kapsamlı düzenleme gerektirir. Kısa vadede daha yüksek tahmin doğruluğunu sağlamak için daha ayrıntılı bir model günün her saati için ayrılmıştır. Bu modeller saatte çalıştırılır ve yanıt ve gerekirse önleyici eylemleri gerçekleştirmek için yeterli zamana izin vermek için birkaç dakika içinde yürütülmesi biter. Dönemsel yeniden eğitme en son verileri kullanarak bu modellerin koleksiyonu güncel tutulur.

Bu kullanım örneği hakkında daha fazla bilgi bulunabilir [burada](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18945).

#### <a name="use-case-qualification-criteria--prerequisites"></a>Büyük/küçük harf nitelik ölçütleri – önkoşulları kullanın
Cortana Intelligence ana gücünü dağıtmanızı ve ölçeklendirmenizi makine öğrenimi odaklı çözümleri güçlü içinde olmasıdır. Bu eşzamanlı olarak yürütülen Öngörüler, binlerce desteği için tasarlandı. Değişen bir tüketim deseni karşılamak üzere otomatik olarak ölçeklendirebilirsiniz. Bir çözümün odağı, bu nedenle, doğruluk ve bilgi işlem performansı üzerinde gereklidir. Örneğin, bir yardımcı şirket doğru enerji talebini tahmin sonraki bir saat ve günün her saati için üretimiyle ilgilenmektedir. Öte yandan, isteğe bağlı olarak olması neden tahmin soruyu yanıtlayarak daha az ilgilenen duyuyoruz (modeli, ilgileniriz).

Bu nedenle, tüm kullanım örnekleri ve iş sorunlarını etkili bir şekilde makine öğrenimini kullanarak çözülebilir bilmeniz önemlidir.

Cortana Intelligence ve machine learning, aşağıdaki ölçütler sağlandığında belirli iş sorununu çözme yüksek oranda etkili olabilir:

* Elle iş sorunudur **Tahmine dayalı** yapısı. Tahmine dayalı bir kullanım örneği örnek sonraki bir saat boyunca belirli bir alt istasyon power yükünü tahmin etmek istediğiniz yardımcı programı bir şirkettir. Öte yandan, çözümleme ve sürücüleri, geçmiş talep sıralaması olacaktır **açıklayıcı** yapısı ve bu nedenle daha az uygulanabilir.
* Açık olduğundan **eylemin yolunu** tahmin kullanılabilir olduğunda gerçekleştirilecek. Örneğin, bir sonraki saatte bir alt istasyon üzerinde aşırı tahmin etme, o alt istasyon ile ilişkili yük azaltma ve bu nedenle büyük olasılıkla aşırı önleme önleyici bir eylem tetikleyebilirsiniz.
* Kullanım örneğini temsil eder bir **tipik sorun türünü** şekilde Çözüldü zaman biçimini pave diğer benzer çözme için kullanım örnekleri.
* Müşteri ayarlayabilirsiniz **nicel ve nitel hedeflerini** başarılı çözümün uygulamasını göstermek için. Örneğin, enerji talebi tahmin için iyi bir nicel hedefi gerekli doğruluk eşiği olacaktır (*örn*, %5 hata varan izin verilir) veya ne zaman alt istasyon tahmin aşırı ardından duyarlık (gerçek pozitif sonuç oranı) ve çağırma belirli bir eşiğin üstünde (doğru pozitif sonuçlar kapsamını) olmalıdır. Bu hedefleri müşterinin iş hedeflerden türetilmesi.
* Açık olduğundan **tümleştirme senaryosu** şirketin iş akışı ile. Örneğin, alt istasyon yük tahmin aşırı önleme etkinlikleri izin vermek için kılavuz denetim Merkezi'nde oturum tümleştirilebilir.
* Hazır kullanmak için müşterinin **yeterli kalite verilerle** kullanım örneğini desteklemek için (sonraki bölümde, daha fazla bilgi bkz **veri kalitesi**, bu playbook'ın).
* Müşteri benimsediğini bulut odaklı veri mimarisi veya **bulut tabanlı makine öğrenme**Azure ML ve diğer Cortana Intelligence Suite bileşenleri dahil olmak üzere.
* Müşteri kurma konusunda istekli mi **bir uçtan uca veri akışı** bu tesislerde veri teslimini düzenli olarak, buluta ve iradeye sahip **faaliyete** çözüm.
* Müşteri için hazırdır **kaynak ayırması** kim etkin olarak bağlı sırasında ilk pilot uygulaması böylece bilgi ve çözüm sahipliğini başarıyla tamamlandıktan sonra müşteriye aktarılabilir.
* Müşteri kaynak olmalıdır bir **nitelikli veri professional**, tercihen bir veri Bilimcisi.

Nitelik yukarıdaki ölçütlere göre bir kullanım örneği, büyük ölçüde kullanım başarı oranları artırmak ve uygulama gelecekte kullanım için iyi bir beachhead oluşturun.

### <a name="cloud-based-solutions"></a>Bulut tabanlı çözümler
Cortana Intelligence Suite, Azure üzerinde bulutta yer alan tümleşik bir ortamdır. Gelişmiş analiz çözümünün bir bulut ortamında dağıtım, işletmelerin ve aynı zamanda önemli avantajları hala kullanım BT çözümleri şirket, şirketler için büyük bir değişiklik gelebilir tutar. Enerji sektörü bağlamında buluta işlemlerinin aşamalı geçiş Temizle eğilimi vardır. Bu eğilim, de, yukarıda açıklandığı gibi akıllı şebeke geliştirilmesini birlikte gider **sektör eğilimlerini**. Playbook'u, bulut tabanlı bir çözüm enerji etki alanındaki odaklanan gibi avantajları ve bulut tabanlı bir çözüm dağıtma ile diğer değerlendirmeler açıklamak önemlidir.

Belki de en büyük avantajı, bulut tabanlı bir çözüm maliyetidir. Bir çözümü olarak kullanır buluta dağıtılan bileşenleri, ön maliyet veya ilişkili COGS (maliyet ürünlerin satılan) bileşen maliyetleri yoktur. Bu, donanım, yazılım ve BT bakım yatırım yapmaya gerek yoktur ve bu nedenle iş riski önemli bir azalma var. anlamına gelir.

Başka bir önemli avantajı, bulut tabanlı çözümler, Kullandıkça Öde maliyet yapısıdır. Bilgi işlem ve depolama için bulut tabanlı sunuculara dağıtılabilir ve gerektiğinde tam olarak Ölçeklendirildi. Bu, bulut tabanlı bir çözüm maliyet verimliliği avantajlarından temsil eder.

Son olarak, tüm bu bulut tabanlı teklif parçası olarak BT Bakım veya gelecekteki altyapı geliştirme yatırım için gerek yoktur. Bu ölçüde, yol haritası gelişen tutar ve Cortana Intelligence Suite en iyi sınıf hizmetleri içerir. Yeni özellikler, bileşenler ve Özellikler sürekli olarak sunulan ve evrim Geçiren.

Buluta, geçiş yeni başlayan bir şirket için yüksek oranda bulut geçişi yol haritası uygulayarak aşamalı bir yaklaşım olması için öneriyoruz. Yardımcı programları ve enerji etki alanındaki şirketler için Tahmine dayalı analiz çözümlerini bulut uygulamalar için mükemmel bir fırsat bu playbook'ları tartışılan kullanım örneklerini temsil inanıyoruz.

#### <a name="business-case-justification-considerations"></a>Servis talebi İş Gerekçesi konuları
Çoğu durumda, müşteri, bir bulut tabanlı bir çözüm ve Machine Learning önemli bileşenleri olan belirli bir kullanım örneği için bir iş gerekçesi yaparken ilginizi çekebilir. Bulut tabanlı bir çözüm söz konusu olduğunda bir şirket içi çözüm aksine Minimum ön ödeme maliyeti bileşendir ve maliyet öğelerin çoğu gerçek kullanım ile ilişkilendirilir. Cortana Intelligence Suite çözümü tahmini bir enerji dağıtımına söz konusu olduğunda, birden çok hizmet, tek bir ortak maliyet yapısı ile tümleştirilebilir. Örneğin, veritabanları (*örn*, SQL Azure) ham verilerini depolamak için kullanılan ve Azure ML gerçek tahminleri için ardından tahmin hizmetlerini barındırmak için kullanılır. Bu örnekte, maliyet yapısında, depolama ve işlem bileşenleri dahil olabilir.

Öte yandan, bir bir enerji talebini tahmin etme (kısa veya uzun vadeli) çalışan iş değerini bir iyi anlamış olmanız gerekir. Aslında, tahmin her işlemin iş değerin anlaşılabilmesi önemlidir. Örneğin, doğru bir şekilde sonraki 24 saat boyunca yükünü tahmin overproduction engelleyebilir veya aşırı yükleme kılavuzundaki engellemeye yardımcı olabilir ve bu günlük olarak finansal tasarrufu açısından amaçlarıyla.

İsteğe bağlı finansal avantajı hesaplamak için temel bir formül çözüm olacaktır tahmin: ![İsteğe bağlı finansal avantajı hesaplamak için temel formül çözümü tahmini](media/cortana-analytics-playbook-demand-forecasting-energy/financial-benefit-formula.png)

Cortana Intelligence Suite, Kullandıkça Öde fiyatlandırma modeli sağlar olduğundan, bu formülü bir sabit maliyete bileşenine yansıtılmasını için gerek yoktur. Bu formül günlük, aylık veya yıllık olarak hesaplanabilir.

Geçerli Cortana Intelligence Suite ve Azure ML fiyatlandırma planları bulunabilir [burada](https://azure.microsoft.com/pricing/details/machine-learning/).

### <a name="solution-development-process"></a>Çözüm geliştirme işlemi
Geliştirme döngüsü bir enerji talebini tahmin çözümü genellikle her biri vermiyoruz 4 aşamaya içerir, bulut tabanlı teknolojiler ve Cortana Intelligence Suite Hizmetleri kullanın.

Bu, aşağıdaki diyagramda gösterilmiştir:

![Akıllı şebeke döngüsü](media/cortana-analytics-playbook-demand-forecasting-energy/smart-grid-cycle.png)

Aşağıdaki paragrafta bu 4 adım işlemi açıklanmaktadır:

1. **Veri toplama** – daha gelişmiş veri dayalı bir analiz çözümü kullanır (bkz **veri anlama**). Özellikle, Tahmine dayalı analiz ve tahmin için söz konusu olduğunda, biz üzerinde sürekli olarak dinamik akış veri kullanır. Enerji Talep tahmini, söz konusu olduğunda bu verileri doğrudan akıllı ölçüm cihazlarından kaynaklanan veya zaten bir şirket içi veritabanı üzerinde bir araya getirilebilir. Ayrıca diğer dış hava durumu ve sıcaklık gibi veri kaynaklarının bağımlı olduğumuz. Devam eden bu veri akışını düzenlenmiş, zamanlanmış depolanan ve gerekir. [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) (ADF), bu görevi yerine ana bizim workhorse olduğu.
2. **Modelleme** – doğru ve güvenilir enerji tahminleri, bir gerekir (eğitme) geliştirme ve bakımını yaptığı geçmiş verilerini kullanın ve verileri anlamlı ve Tahmine dayalı modelleri ayıklar harika bir model. Machine Learning (ML) alanında düzenli olarak geliştirilen daha gelişmiş algoritmalar ile hızla büyüyen. Azure ML Studio, bir tam iş akışı içinde en gelişmiş ML algoritmaları kullanmasına yardımcı olacak harika bir deneyim sağlar. Bu iş akışı, sezgisel bir akış diyagramda gösterilmiştir ve veri hazırlama, özellik ayıklama, modelleme ve model değerlendirme içerir. Kullanıcı, bu ortamda bulunan çeşitli modelleri yüzlerce çekebilirsiniz. Bu aşama sonunda bir veri Bilimcisi tamamen değerlendirilir ve dağıtım için hazır olan bir çalışma modeli gerekir.

   Aşağıdaki diyagram tipik bir iş akışı bir örnektir:

   ![İş akışı modelleme](media/cortana-analytics-playbook-demand-forecasting-energy/modeling-workflow.png)
3. **Dağıtım** – çalışma modelini yandan, sonraki adıma dağıtımıdır. Burada model çeşitli tüketim istemcilerinden gelen Internet üzerinden aynı anda çağrılabilen bir RESTful API sunan bir web hizmeti dönüştürülür. Azure ML, doğrudan bir düğmeye tek bir tıklamayla Azure ML Studio üzerinden model dağıtma basit bir yol sağlar. Tüm dağıtım işlemi Bileşenler'olmuyor. Bu çözüm, gerekli tüketim karşılamak üzere otomatik olarak ölçeklendirebilirsiniz.
4. **Tüketim** – bu aşamasında, biz aslında yapmak tahminler üretmek için tahmin modelini kullanın. Bir kullanıcı uygulamadan tüketim temelli (*örn*, Pano) veya doğrudan gibi bir işletim sisteminden talep/tedarik sistem veya bir kılavuz iyileştirmesi çözümü. Birden çok kullanım durumu, tek bir modelden odaklı.

## <a name="data-understanding"></a>Veri anlama
İş konuları kapsayan sonra (bkz **iş anlama**) tahmin çözümü, bir enerji talebi artık veri bölümünü tartışmak hazırız. Herhangi bir Tahmine dayalı analiz çözümü güvenilir veri kullanır. Enerji Talep tahmini için çeşitli düzeylerde verileriyle geçmiş tüketim bağımlı olduğumuz. Bu geçmiş verileri ham madde kullanılır. Bu, veri uzmanı gerekli tahminleri sonunda oluşturacak bir modele koyabilirsiniz (Özellikler da bilinir) adaylarının tanımlayacak bir çözümlenmesiyle tutulacaktır.

Bu bölümün kalanında, biz çeşitli adımları ve veri ve için kullanılabilir biçime getirilmesi anlama konuları anlatmaktadır.

### <a name="the-model-development-cycle"></a>Model geliştirme döngüsü
İyi tahmini modeller bazı dikkatli hazırlıkları gerektirir ve planlama üretir. Birden çok adım modelleme işlemine bölmek ve aynı anda bir adımında odaklanarak tüm işleminin sonucunu önemli ölçüde geliştirebilirsiniz.

Aşağıdaki diyagram, modelleme işleminin içine birden çok adımı nasıl bölünebilir gösterir:

![Model geliştirme döngüsü](media/cortana-analytics-playbook-demand-forecasting-energy/model-development-cycle.png)

Döngü altı adımlardan oluşur görülebileceği gibi:

* Sorun, oluşumunu
* Veri alımı ve veri keşfi
* Veri hazırlama ve özellik Mühendisliği
* Modelleme
* Modeli değerlendirme
* Geliştirme

Bu bölümün geri kalanında biz, her adımda göz önünde bulundurmanız gerekenler ve tek tek adımları anlatmaktadır.

### <a name="problem-formulation"></a>Sorun, oluşumunu
Biz sorun formülasyonu aşağıdakilerden herhangi bir Tahmine dayalı analiz çözümü uygulamadan önce yapması gereken en önemli adım olarak düşünebilirsiniz. Burada biz problemini dönüştürme ve belirli öğeleri kullanarak veri ve modelleme teknikleri çözülebileceğini parçalara ayırın. Sorular yanıtlanacak istiyoruz bir dizi sorunu formüle etmek için iyi bir uygulamadır. Enerji talebi tahmin kapsamı içinde geçerli olabilecek bazı olası sorular şunlardır:

* Sonraki saat veya gün içinde tek bir alt istasyon beklenen yükü nedir?
* Günün hangi saatinde my kılavuz en üst düzey talepleri yaşar?
* Nasıl büyük olasılıkla beklenen en yüksek yükü sürdürebilmek için benim kılavuz var mı?
* Ne kadar güce power istasyon her gün saat boyunca oluşturmalı mı?

Bu soruları formulating doğru veri alma ve tam olarak eldeki problemini ile hizalanan bir çözüm uygulama olanak sağlıyor. Ayrıca, biz model performansını değerlendirme olanak bazı ana ölçümleri daha sonra ayarlayabilirsiniz. Örneğin, tahmin ne kadar doğru olmalıdır ve iş için kabul edilebilir olmaya hata aralığı nedir?

### <a name="data-sources"></a>Veri Kaynakları
Modern bir akıllı şebeke çeşitli bölümleri ve kılavuz bileşenlerinin verileri toplar. Bu veriler, çeşitli yönlerini işlemi ve güç kılavuz kullanımını temsil eder. Enerji talebi hakkında tahmin kapsamında, biz gerçek tüketim yansıtan veri kaynaklarında tartışma sınırlıyoruz. Akıllı ölçüm cihazlarından enerji tüketimi önemli bir kaynağı var. Dünya çapındaki yardımcı programlar, hızlı bir şekilde kendi Tüketiciler için akıllı ölçüm cihazlarından dağıtıyorsanız. Akıllı ölçüm cihazlarından gerçek güç tüketimi kaydetmek ve bu verileri geri yardımcı şirket için sürekli geçiş. Veriler toplanır ve 5 dakikada 1 saate kadar geri sabit bir aralık, gönderilir. Daha gelişmiş akıllı ölçüm cihazlarından uzaktan denetim ve içinde bir evin gerçek kullanım dengelemek için de programlanabilir. Akıllı ölçüm verileri görece güvenilir ve zaman damgası içerir. Bu, Talep tahmini için önemli bir tarifi kolaylaştırır. Ölçer veri toplanabilir (yukarı toplanan) grid topolojisi içindeki çeşitli düzeylerde: transformer, alt istasyon, bölge, *vb*. Ardından, biz bunun için bir tahmin modelli oluşturmak için gerekli bir toplama düzeyinde seçebilirsiniz. Örneğin, yardımcı şirket her birinin kendi kılavuz şalt gelecekteki yükü tahmin istiyorsanız, ardından tüm ölçümleri veriler için tek tek her alt istasyon toplanır ve için tahmin modeli giriş olarak kullanılır. Bir iç veri kaynağı olarak için akıllı ölçüm cihazlarından diyoruz.

Bir güvenilir enerji talebi tahmin, ayrıca diğer dış veri kaynaklarına bağlıdır. Güç tüketimini etkileyen tek önemli faktör, hava durumu ve daha kesin sıcaklık ' dir. Geçmiş verileri dış sıcaklığı ve güç tüketimini arasında güçlü bir bağıntısı gösterir. Sık erişimli Yaz gün boyunca, tüketicilerin olun, hava bağıntıyı ve sistemleri ısıtma üzerinde Kış power sırasında kullanın. Güvenilir bir kaynak geçmiş Sıcaklıkların kılavuz konumda bu nedenle anahtardır. Ayrıca, biz de sıcaklık, doğru bir tahmin üzerinde bir güç tüketimini tahmin unsuru kullanır.

Diğer dış veri kaynakları, enerji talebi tahmin modellerini oluşturmaya da yardımcı olabilir. Bu uzun vadeli iklim değişiklikler, ekonomik dizinler içerebilir (*örn*, gayrisafi yurt içi HASILA) ve diğerleri. Bu belgede size bu diğer veri kaynakları dahil edilmez.

### <a name="data-structure"></a>Veri yapısı
Gerekli veri kaynaklarına belirledikten sonra toplanmış olan ham verileri doğru veri özelliklerini içerdiğinden emin olmak istiyoruz. Güvenilir bir talep tahmin modeli oluşturmak için biz toplanan veriler, gelecek talebi tahmin yardımcı olabilecek veri öğeleri içermesini sağlamak amacıyla gerekir. Ham veri (şema) veri yapısı ile ilgili bazı temel gereksinimler aşağıda verilmiştir.

Ham verileri satırlar ve sütunlar oluşur. Her ölçüm verileri tek bir satır temsil edilir. Her veri satırının birden çok sütun (Özellikler veya alanlar da bilinir) içerir.

1. **Zaman damgası** – ne zaman ölçüm kaydedilen zaman gerçek zaman damgası alanı temsil eder. Genel tarih/saat biçimlerinden biri ile uyumlu olmalıdır. Tarih ve saat bölümleri eklenmelidir. Çoğu durumda, ikinci taneciklik düzeyini kaydedilecek kez gerek yoktur. Hangi veri kaydedilen saat dilimi belirtmek önemlidir.
2. **Ölçüm kimliği** -meter veya ölçüm cihaz Bu alan tanımlar. Bu Kategorik bir değişkendir ve bir rakam ve karakter birleşimi olabilir.
3. **Tüketim değer** – belirli bir tarih/saat gerçek tüketime budur. Tüketim (kilowatt-hour) kWh cinsinden ölçülen veya diğer tercih edilen birimler. Ölçü birimi verilerdeki tüm ölçümler genelinde tutarlı kalması gereken dikkat edin önemlidir. Bazı durumlarda, tüketim 3 power aşamaları sağlanabilir. Bu durumda biz bağımsız tüketim aşamaları toplamak gerekir.
4. **Sıcaklık** – genellikle bağımsız bir kaynaktan toplanan sıcaklık. Ancak, tüketim veri türüyle uyumlu olmalıdır. Bu, gerçek kullanım verileri ile eşitlenmesine olanak tanıyan yukarıda açıklandığı gibi bir zaman damgasını içermelidir. Sıcaklık değeri Santigrat derece ya da Fahrenhayt belirtilebilir, ancak tüm ölçümler tutarlı kalır.
5. **Konum –** konum alanı genellikle burada sıcaklık verilerini toplanmış yer ile ilişkilidir. Bir posta kodu sayı olarak veya enlem/boylam (lat/uzun) biçiminde gösterilebilir.

Aşağıdaki tablolarda, iyi bir tüketim ve sıcaklık veri biçimi örneklerini gösterir:

| **Tarih** | **saat** | **Ölçüm kimliği** | **1. Aşama** | **2. Aşama** | **3. Aşama** |
| --- | --- | --- | --- | --- | --- |
| 7/1/2015 |10:00:00 |ABC1234 |7.0 |2.1 |5.3 |
| 7/1/2015 |10:00:01 |ABC1234 |7.1 |2.2 |4.3 |
| 7/1/2015 |10:00:02 |ABC1234 |6.0 |2.1 |4.0 |

| **Tarih** | **saat** | **Konum** | **Sıcaklık** |
| --- | --- | --- | --- |
| 7/1/2015 |10:00:00 |11242 |24.4 |
| 7/1/2015 |10:00:01 |11242 |24.4 |
| 7/1/2015 |10:00:02 |11242 |24.5 |

Yukarıda görüldüğü gibi bu örnek 3 güç aşaması olan ilişkili tüketim için 3 farklı değerler içerir. Ayrıca, tarih ve saat alanları ayrılmış olduğunu, ancak bunlar de tek bir sütunda birleştirilebilir unutmayın. Bu örnekte konum sütunu 5 basamaklı posta kodu biçimi ve sıcaklığı Santigrat derece biçimde temsil edilir.

### <a name="data-format"></a>Veri biçimi
Cortana Intelligence Suite, CSV, TSV, JSON, en yaygın veri biçimlerini destekleyebilir *vb*. Veri biçimi için tüm yaşam döngüsünü projenin tutarlı kalmasını önemlidir.

### <a name="data-ingestion"></a>Veri Alımı
Enerji talebi tahmin sürekli ve sık bir şekilde tahmin olduğundan, biz ham veriler sağlam ve güvenilir veri alma işlemi yoluyla geçiyor emin olmanız gerekir. Alma işlemi, ham verileri gereken zaman tahmin işlemi için kullanılabilir olacağını garanti gerekir. Bu, veri alımı sıklığı tahmin sıklığından büyük olması gerektiğini anlamına gelir.

Örneğin: 8: 00'da her gün yeni tahmin tahmin çözümü bizim isteğe bağlı oluşturursanız sonra tüm son 24 saat toplanan verileri tam olarak alınan o noktadan kasa ve hatta veri son bir saat içermek zorundadır emin olmak ihtiyacımız var.

Bunu gerçekleştirmek için Cortana Intelligence Suite güvenilir veri alma işlemini desteklemek için çeşitli yollar sunar. Bu daha ayrıntılı olarak açıklanmıştır **dağıtım** bu belgenin bölüm.

### <a name="data-quality"></a>Veri Kalitesi
Güvenilir ve doğru Talep tahmini gerçekleştirmek için gerekli olan ham veri kaynağı, bazı temel veri kalite ölçütlerini karşılamalıdır. Bazı olası veri kalitesi sorunu dengelemek için Gelişmiş istatistiksel yöntemler kullanılabilir olsa da, yine de biz bazı temel veri kalitesi eşiği yeni veri alma, geçmeden emin olmak ihtiyacımız var. Ham veri kalitesi ilgili bazı önemli noktalar şunlardır:

* **Eksik değer** – belirli bir ölçü değil toplandığında, bu duruma başvurur. Temel burada eksik değer oranı belirli bir süre için % 10'den büyük olmamalıdır, gereksinimidir. Durumda, tek bir değer eksik önceden tanımlı bir değer kullanarak belirtilmesi (örneğin: '9999') ve 'geçerli bir ölçü olabilecek 0'.
* **Ölçüm doğruluğu** – tüketim veya sıcaklık gerçek değerini doğru bir şekilde kaydedilmelidir. Yanlış ölçümleri yanlış tahminleri üretecektir. Genellikle, ölçüm hata %1 true değerine göre daha düşük olmalıdır.
* **Ölçü zaman** – gerekli verilerin gerçek zaman damgası, toplanan 10 saniyeden göre doğru zamanı gerçek ölçü tarafından sapma değil.
* **Eşitleme** – birden çok veri kaynağına kullanıldığında (*örn*, tüketim ve sıcaklık) size hiçbir zaman eşitleme olduğundan emin olmalısınız aralarında sorunları. Bu, herhangi iki bağımsız veri kaynaklarından toplanan zaman damgası arasındaki zaman farkı 10 saniyeden fazla aşmamalıdır anlamına gelir.
* **Gecikme süresi** - de, yukarıda açıklandığı gibi **veri alımı**, bir güvenilir veri akışı ve alım işleme bağımlı duyuyoruz. Denetimi biz size veri gecikme süresinin denetim emin olmalısınız. Bu, gerçek ölçüm alındığı saat ve, Cortana Intelligence Suite depolama alanına yüklenmiş ve kullanıma hazır saat arasındaki zaman farkı olarak belirtilir. Kısa süreli yük için toplam gecikme süresini tahmin 30 dakikadan fazla olmamalıdır. Uzun süreli yük için toplam gecikme süresini tahmin 1 günden daha büyük olmamalıdır.

### <a name="data-preparation-and-feature-engineering"></a>Veri hazırlama ve özellik Mühendisliği
Ham verileri alınan sonra (bkz **veri alımı**) ve güvenli bir şekilde kaldırıldı depolanır; bunu işlenmek üzere hazırdır. Veri Hazırlık aşaması temelde ham verileri alma ve dönüştürme (, yeniden şekillendirmeyi dönüştürme) içine modelleme aşaması için bir form. Ham veri sütunu olduğundan, gerçek ölçülen değeri, standartlaştırılmış değerleri, gibi daha karmaşık işlemler kullanarak gibi basit işlemler içerebilir [zaman İzolasyonu](https://en.wikipedia.org/wiki/Lag_operator)ve diğerleri. Yeni oluşturulan veri sütunlarını veri özellikleri adlandırılır ve bunlar oluşturma işlemi özellik Mühendisliği olarak adlandırılır. Bu işlem sonunda biz ham verilerden türetilmiş ve model için kullanılacak yeni bir veri kümesi gerekir. Ayrıca, veri Hazırlık aşaması ilgileniriz eksik değerleri gerekir (bkz **veri kalitesi**) ve bunları dengelemek. Bazı durumlarda, biz de tüm değerler aynı ölçek temsil edilebilmesi için verilerin'leri normalleştirmek gerekir.

Bu bölümde, biz enerjinin dahil edilen genel verileri özelliklerinden bazıları listesinde tahmin modellerini isteğe bağlı.

**Özellik temelli süresi:** Bu özellikler, tarih/zaman damgası verilerden türetilir. Bunlar ayıklanır ve gibi kategorik özellikler dönüştürülür:

* Saat gün saat değerleri 0 ile 23 arasında alan gün budur
* Gün haftanın – Bu haftanın gününü temsil eder ve 1 arasında değerleri alır (Pazar) ile 7 (Cumartesi)
* Gün, ayın – Bu gerçek tarihini temsil eder ve değerleri 1 ile 31 alabilir
* Ay yıl – bu ayı temsil eder ve 1 arasında değerleri alır (Ocak) ile 12 (aralık)
* Hafta sonu – Bu haftanın günleri veya 1 hafta için 0 değerini alan bir ikili değer özelliğidir
* Tatil - bu değeri 0 veya 1 normal bir gün için bir tatil için alan bir ikili değer özelliğidir
* Fourier – Fourier koşulları itibaren zaman damgası türetilir ve ' % s'mevsimsellik (döngüler) yakalamak için kullanılan ağırlıkları verileri terimlerdir. Biz birden çok sezonlar verilerimizi olabileceği birden çok Fourier koşulları ihtiyacımız. Örneğin, isteğe bağlı değerler yıllık, haftalık ve günlük sezonlar/3 Fourier koşullarını sonuçlanacak döngüleri olabilir.

**Bağımsız ölçüm özellikleri:** Bağımsız özellikler adaylarının modelimizi de olarak kullanılacak istiyoruz tüm veri öğeleri içerir. Burada, tahmin etmek için ihtiyacımız bağımlı özellik tutarız.

* Lag özellik – bunlar zaman kaydırılacağı uzaklık değerleri gerçek isteğe bağlı. Örneğin, lag 1 özellikleri, isteğe bağlı değerin göreli geçerli zaman damgasını (saatlik veri varsayılarak) önceki saat içinde tutar. Benzer şekilde, biz 2 gecikme eklemek için 3, öteleme *vb*. Kullanılan lag özellikleri gerçek birleşimi, modelleme aşaması sırasında model sonuçlarını değerlendirmesi tarafından belirlenir.
* Uzun süreli popüler: Bu özellik isteğe bağlı yılları arasında doğrusal artışı temsil eder.

**Bağımlı özellik:** Bağımlı modelimizi tahmin etmek istiyoruz. veri sütunu özelliğidir. İle [denetimli makine öğrenimi](https://en.wikipedia.org/wiki/Supervised_learning), önce (Bu aynı zamanda etiket olarak adlandırılır), bağımlı özellikleri kullanarak modeli eğitmek için oluşturmamız gerekir. Bu bağımlı özellikle ilişkilendirilen veri desenlerinde öğrenmek model sağlar. Tahmini enerji talebini genellikle gerçek talep tahmin etmek istiyoruz ve bu nedenle biz bağımlı bir özellik olarak kullanılır.

**Eksik değerleri işleme:** Veri hazırlama aşamasında, biz eksik değerleri işlemek için en iyi stratejisini belirlemek gerekir. Bu çeşitli istatistik kullanarak çoğunlukla yapılır [veri imputation yöntemleri](https://en.wikipedia.org/wiki/Imputation_\(statistics\)). Enerji talebi tahmin söz konusu olduğunda, biz genellikle eksik değerleri önceki kullanılabilir veri noktalarından hareketli ortalama kullanarak impute.

**Veri normalleştirme:** Veri normalleştirme benzer bir ölçek Talep tahmini gibi tüm sayısal verileri getirmek için kullanılan dönüştürme başka bir türüdür. Bu, genellikle duyarlık ve doğruluğu artırmak yardımcı olur. Genellikle veri aralığına göre gerçek değeri bölerek bunu.
Bu özgün değer -1 ile 1 arasında genellikle daha küçük bir aralıkta birleştirir, ölçeği.

## <a name="modeling"></a>Modelleme
Veri dönüştürme bir modeline gerçekleştiği modelleme aşamasıdır. Bu işlemin var. çekirdek geçmiş verileri (eğitim) tarayın, desenleri ayıklayın ve model derleme algoritmalar Gelişmiş. Bu model, daha sonra modeli oluşturmak için kullanılmamış olan yeni veriler üzerinde tahmin etmek için kullanılabilir.

Bir çalışma güvenilir modeli sahibiz sonra sonra gerekli özellikleri (X) içerecek şekilde yapılandırılmış yeni verileri puanlamak için kullanabiliriz. Puanlama işlemi, kalıcı modelinin (eğitim aşaması nesneden) kullanın ve Ŷ tarafından belirtilen hedef değişkeni tahmin hale getirir.

### <a name="demand-forecasting-modeling-techniques"></a>Talep tahmini teknikleri modelleme
Talep tahmini vermiyoruz söz konusu olduğunda, zamana göre sıralı geçmiş verilerin kullanın. Zaman boyutu olarak içeren verileri genel olarak diyoruz [zaman serisi](https://en.wikipedia.org/wiki/Time_series). Hedef içinde zaman serisi modelleme bulmaktır zaman ilgili eğilim, mevsimsellik, otomatik-bağıntı (bağıntı zaman içinde) ve bunları da bir model oluşturmak.

Son yıllarda, zaman serisi tahmin uyum sağlamak için ve tahmin doğruluğunu artırmak için gelişmiş algoritmalar geliştirilmiştir. Kısaca birkaç tanesi burada ele alır.

> [!NOTE]
> Bu bölümde, makine öğrenimi ve genel bakış tahmin ancak Talep tahmini için yaygın olarak kullanılır teknikler modelleme çok kısa bir ankete olarak kullanılmak üzere tasarlanmamıştır. Daha fazla bilgi ve zaman serisi tahmini hakkında eğitim malzemesi için çevrimiçi kitap öneririz [tahmin: ilkeleri ve uygulama](https://www.otexts.org/).
>
>

#### <a name="ma-moving-average"></a>**MA (hareketli ortalama)**
Hareketli ortalama zaman serisi tahmini için kullanılan ilk analitik tekniklerden birini ve yine de bir en sık kullanılan teknikleri bugünden itibaren şeklindedir. Ayrıca tahmin tekniklerinin daha gelişmiş için bir temel niteliğindedir. Hareketli Ortalama biz burada K hareketli ortalama sırasını gösterir K en son noktaları ortalaması alınarak sonraki veri noktası tahmin.

Hareketli Ortalama teknik tahmin düzgünleştirme etkisi vardır ve bu nedenle de büyük geçiciliğine veri yönetemeyen.

#### <a name="ets-exponential-smoothing"></a>**ETS (Üstel düzeltme)**
Üstel düzgünleştirme (ETS), son veri noktalarını ağırlıklı ortalamasını sonraki veri noktası tahmin etmek için kullanabileceğiniz çeşitli yöntemler ailesidir. Daha yüksek ağırlıkları için daha yeni değerler atayın ve giderek eski ölçülen değerleri için bu ağırlığı olur. Bazı verileri gibi dönemsel yöntemi Holt-Winters mevsimsellik işlenmesi dahil, birkaç farklı yöntemle bu aile vardır.

Bu yöntemlerden bazıları aynı zamanda verilerin mevsimsellik faktörü.

#### <a name="arima-auto-regression-integrated-moving-average"></a>**ARIMA (otomatik gerileme bütünleşik hareketli ortalama)**
Otomatik gerileme tümleşik hareketli ortalama (ARIMA) için zaman serisi tahmin yaygın olarak kullanılan başka bir yöntem ailesidir. Hareketli Ortalama otomatik gerileme yöntemleri bulundurmanızı birleştirir. Otomatik gerileme yöntemleri sonraki tarih noktası hesaplamak için önceki zaman seri değerlerini yararlanarak regresyon modellerini kullanın. ARIMA yöntemleri veri noktaları arasındaki farkı hesaplamak ve özgün ölçülen değeri yerine bu kullanarak içeren fark kayıt yöntemleri için de geçerlidir. Son olarak, ARIMA ayrıca yukarıda açıklanan hareketli ortalama teknikleri kullanır. Tüm bu yöntemleri çeşitli şekillerde ne ARIMA yöntemleri ailesi oluşturur birleşimidir.

ETS hem de ARIMA yaygın bugün enerji Talep tahmini ve diğer birçok tahmin sorunları için kullanılır. Çoğu durumda bunlar birleştirilir birlikte çok doğru sonuçlar sağlamak için.

**Genel çoklu regresyon** regresyon modelleri, makine öğrenimi ve istatistikleri etki alanı içinde en önemli modelleme yaklaşımı olabilir. Zaman serisi bağlamında regresyon gelecekteki değerleri tahmin etmek için kullanırız (*örn*, talep). Regresyon, adaylarının doğrusal birleşimi alın ve bu adaylarının ağırlıkları (katsayıları da bilinir) bir eğitim işlemi sırasında öğrenin. Bizim tahmin edilen bir değer tahmin bir regresyon çizgi oluşturmak için kullanılan hedeftir. Hedef değişkeni sayısal ve bu nedenle de zaman serisi tahmin uygun olduğunda regresyon yöntemleri uygundur. Regresyon yöntemlerden çok basit bir regresyon modelleri gibi çeşitli yoktur [doğrusal regresyon](https://en.wikipedia.org/wiki/Linear_regression) ve olanları karar ağaçları'gibi daha gelişmiş [rastgele ormanları](https://en.wikipedia.org/wiki/Random_forest), [Neural Ağları](https://en.wikipedia.org/wiki/Artificial_neural_network)ve Artırılmış karar ağaçları.

Regresyon problemi tahmini enerji talebini oluşturmak çok fazla gerçek zaman serisi verilerini ve sıcaklık gibi dış faktörler, birleştirilebilen veri özelliklerimizi seçme esnekliği sağlıyor. Seçilen özellikler hakkında daha fazla bilgi, özellik Mühendisliği içinde açıklanmıştır (bkz **veri hazırlama ve özellik Mühendisliği**) playbook'u bölümü.

Deneyimlerimizden uygulama ve enerji talep tahminleri pilot dağıtımı, Azure ML üzerinde mevcut olan gelişmiş regresyon modelleri, en iyi sonuçlar eğilimindedir ve vermiyoruz bulduk bunları kullanın.

## <a name="model-evaluation"></a>Modeli değerlendirme
Modeli değerlendirme sahip kritik bir rol içinde **Model geliştirme döngüsü**. Bu adımda modeli ve gerçek verilerle performansını doğrulama içine bakın. Modelleme adımı sırasında kullanılabilir verilerin bir kısmını modeli eğitmek için kullanırız. Değerlendirme aşaması sırasında biz kalan modeli test etmek üzere verileri alın. Pratikte, biz yeniden oluşturulamaz ve eğitim veri kümesi ile aynı özellikleri içeren model yeni verileri besleme anlamına gelir. Ancak, doğrulama işlemi sırasında model hedefi değişkeni tahmin etmek yerine kullanılabilir hedef değişkeni sağlamak için kullanırız. Genellikle bu işlemi olarak Puanlama modeli diyoruz. Biz sonra doğru hedef değerleri kullanın ve bunları tahmin edilen oluşturduklarıyla karşılaştırın. Buradaki amaç, tahminler ve gerçek değer arasındaki farkı anlamı tahmin hata en aza indirmek ve ölçmek sağlamaktır. Model ince ayar yapma ve hata gerçekten azalan dağıtılamayacağını doğrulamak isteriz olduğundan hata ölçüm niceleme anahtardır. Model ince ayar yapılabilir learning işlemini denetleyen model parametreleri değiştirerek veya ekleyerek veya kaldırarak veri özelliklerini (olarak adlandırılan [parametreleri gözden geçirme](https://channel9.msdn.com/Blogs/Azure/Data-Science-Series-Building-an-Optimal-Model-With-Parameter-Sweep)). Pratikte, biz modelleme, özellik Mühendisliği arasında yineleme yapmak ve hata için gerekli düzeyi azaltabilmenizi duyuyoruz kadar değerlendirme aşamaları birden çok kez model gerekebileceğini anlamına gelir.

Tahmin hatası asla sıfır olarak olacağını Vurgu önemlidir hiçbir zaman her sonuç mükemmel bir şekilde tahmin edebilen bir model yok. Ancak, belirli bir boyuta, iş için kabul edilebilir bir hata yoktur. Doğrulama işlemi sırasında model tahmin hata iletilerimizi düzeyinde olduğundan emin olmak istiyoruz veya iş tolerans düzeyi daha iyi. Bu nedenle döngüsü sırasında başındaki fazla hata düzeyini ayarlamak önemli olan **sorun, oluşumunu** aşaması.

### <a name="typical-evaluation-techniques"></a>Tipik bir değerlendirme tekniklerini
Hangi tahmine hata ölçülen amaçlarıyla ve çeşitli yolları vardır. Bu bölümde biz değerlendirme tekniklerini zaman serisi ve enerji talebini tahmin özel ilgili tartışma odaklanır.

#### <a name="mapehttpsenwikipediaorgwikimeanabsolutepercentageerror"></a>[**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
MAPE ortalama mutlak yüzdesi hata anlamına gelir. İle MAPE biz arasındaki farkı bilgisayar her nokta ve o noktadan gerçek değerini tahmini. Biz ardından nokta başına hata oranı gerçek değeri göreli fark hesaplayarak ölçme. Son adımda size bu değerleri ortalama. MAPE için kullanılan matematik formülünü aşağıda verilmiştir:

![MAPE formül](media/cortana-analytics-playbook-demand-forecasting-energy/mape-formula.png)
*nerede A<sub>t</sub> gerçek değeri, F<sub>t</sub> tahmin edilen değer ve n tahmin gelecekte.*

## <a name="deployment"></a>Dağıtım
Biz modelleme aşamayı nailed ve model performansını doğrulanmış sonra dağıtım aşaması gitmek hazırız. Bu bağlamda dağıtım modeli gerçek Öngörüler üzerinde büyük ölçekte çalıştırarak kullanma müşteriye etkinleştirme anlamına gelir. Ana Amacımız yalnızca verilerden öngörü edinme aksine Öngörüler sürekli olarak çağrılacak olduğundan dağıtım kavramı Azure ML anahtardır. Dağıtım aşaması, büyük ölçekli olarak kullanılacak bir model nerede etkinleştiririz parçasıdır.

Enerji talebi tahmin bağlamında, bir yandan devamlı Periyodik tahminlerini yeni bir veri modeli için kullanılabilir olduğunu ve tahmini veri kaybı istemciye geri gönderilir sağlarken çağırmaktır.

### <a name="web-services-deployment"></a>Web Hizmetleri dağıtımı
Ana dağıtılabilir yapı taşı Azure ML web hizmetidir. Bu, bulutta Tahmine dayalı bir model kullanımını etkinleştirmek için en etkili yoludur. Web hizmetini model kapsüller ve ile geldik bir [RESTful](https://www.restapitutorial.com/) API (uygulama programlama arabirimi). API, aşağıdaki diyagramda gösterildiği gibi istemci kodunun bir parçası olarak kullanılabilir.

![Hizmet dağıtımı ve tüketimi](media/cortana-analytics-playbook-demand-forecasting-energy/web-service-deployment-and-consumption.png)

Görülebileceği gibi web hizmeti, Cortana Intelligence Suite bulutta dağıtılan ve, kullanıma sunulan REST API uç noktası çağrılabilir. Çeşitli etki alanlarında istemciler farklı türde hizmet aracılığıyla Web API'si aynı anda çalıştırabilirsiniz. Web hizmeti, ayrıca eş zamanlı çağrılar binlerce desteklemek üzere ölçeklendirilebilir.

### <a name="a-typical-solution-architecture"></a>Tipik bir çözüm mimarisi
Bir enerji talebini tahmin çözümü dağıtırken tahmin web hizmeti geçer ve tüm veri akışını kolaylaştıran bir uçtan uca çözüm dağıtmada ilgilenen duyuyoruz. Yeni bir tahmin ediyoruz çağırma zaman modeli güncel verileri özelliklerle beslenir emin olmak biz gerekir. Bu yeni toplanan ham verileri sürekli alınan, işlenen ve hangi model üzerinde geliştirildiği ayarlanmış gerekli özellik olarak dönüştürülür anlamına gelir. Aynı zamanda, tahmin edilen verilerin kullanan istemcileri ve uç kullanılabilir olmasını istiyoruz. Bir örnek veri akışı döngüsü (veya veri işlem hattı), aşağıdaki diyagramda gösterilmiştir:

![Uçtan uca veri akışı enerji talebini tahmin](media/cortana-analytics-playbook-demand-forecasting-energy/energy-demand-forecase-end-data-flow.png)

Enerji talebi tahmin döngüsünün bir parçası gerçekleşmesi adımlar şunlardır:

1. Dağıtılan veri ölçümlerine milyonlarca sürekli güç tüketimi verilerinin gerçek zamanlı olarak oluşturduğunuzdan.
2. Bu veri toplanan ve bulut deposuna yüklenir (*örn*, Azure Blob).
3. İşlenmeden önce ham veriler için bir alt istasyon veya bölgesel düzeyde işletme tarafından tanımlandığı şekilde toplanır.
4. Özellik işleme (bkz **veri hazırlama ve işleme özelliği**) sonra gerçekleşir ve üretir için gerekli olan veri modeli eğitimi veya Puanlama – özellik kümesi veriler bir veritabanında depolanır (*örn*, SQL Azure).
5. Yeniden eğitim hizmet çağrılır Puanlama web hizmeti tarafından kullanılabilir, böylece yeniden – tahmin modeli eğitmek için model güncelleştirilmiş sürümünün kalıcıdır.
6. Puanlama web hizmeti gerekli tahmin sıklığı en uygun bir zamanlamaya göre çağrılır.
7. Tahmin edilen veriler son tüketim istemci tarafından erişilebilen bir veritabanında depolanır.
8. Tüketim istemci tahminleri alır, ızgaranın içine uygular ve uygun şekilde gerekli kullanım örneğini kullanır.

Tüm bu döngü tam olarak otomatik değildir ve bir zamanlamaya göre çalışan unutulmaması önemlidir. Bu veri döngüsünün tamamını düzenleme gibi araçları kullanarak yapılabilir [Azure Data Factory](https://azure.microsoft.com/services/data-factory/).

### <a name="end-to-end-deployment-architecture"></a>Uçtan uca dağıtım mimarisi
Pratikte bir enerji talebi tahmin çözümü Cortana Intelligence'ı dağıtmak için gerekli bileşenleri kurulan ve doğru yapılandırıldığından emin olmak gerekiyor.

Aşağıdaki diyagram, uygular ve yukarıda açıklanan veri akışı döngüsü düzenler tipik bir Cortana Intelligence'nın temel mimarisini gösterir:

![Uçtan uca dağıtım mimarisi](media/cortana-analytics-playbook-demand-forecasting-energy/architecture.png)

Her bileşen ve mimarinin tamamını hakkında daha fazla bilgi için lütfen enerji çözümü şablona bakın.

