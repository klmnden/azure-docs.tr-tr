---
title: "Azure - Cortana Intelligence çözüm şablonu ile Havacılık Tahmine dayalı bakım | Microsoft Docs"
description: "Microsoft Cortana Intelligence Tahmine dayalı bakım Havacılık, yardımcı programlar ve taşıma için bulunduğu bir çözüm şablonu."
services: cortana-analytics
documentationcenter: 
author: fboylu
manager: jhubbard
editor: cgronlun
ms.assetid: 2e8b66db-91eb-432b-b305-6abccca25620
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: fboylu
ms.openlocfilehash: aafa395f8c0593d9597f74cd5cd2a41f26897c6f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="cortana-intelligence-solution-template-playbook-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Cortana Intelligence çözüm şablonu Playbook Havacılık ve diğer işletmelerin Tahmine dayalı bakım
## <a name="executive-summary"></a>Özet
Tahmine dayalı bakım en talep edilen maliyet tasarrufları İnanılmaz derecede unarguable yararlar ile Tahmine dayalı analiz uygulamalarının biridir. Bu playbook önemli kullanım örneklerini indirimlere ile Tahmine dayalı bakım çözümler için bir başvuru sağlamaya amaçlar.
En yaygın iş senaryolarına Tahmine dayalı bakım, zorluklar, iş sorunlarını Tahmine dayalı modelleme teknikleri kullanarak bu tür veri çözümleri oluşturmak üzere bu iş sorunlarını çözmek için gerekli verileri bu tür çözümler için uygun bir anlayış okuyucuya vermek hazır olan ve örnek çözüm mimarileri ile en iyi uygulamalar.
Tahmine dayalı modelleri ayrıntılarını ayrıca açıklanır özellik mühendisliği gibi geliştirilen modeli geliştirme ve performans değerlendirme. Esas olarak, iş ve başarılı geliştirme ve Tahmine dayalı bakım Çözüm dağıtımı için gerekli analitik yönergeleri Bu playbook araya getirir. Bu yönergeler, uzun vadeli Tahmine dayalı bakım stratejisi başlangıç noktası olarak Cortana Intelligence Suite ve özellikle Azure Machine Learning kullanarak ilk çözümünü oluşturun İzleyici yardımcı olmak için hazırlanır. Cortana Intelligence Suite ve Azure Machine Learning ile ilgili belgelere bulunabilir [Cortana Analytics](http://www.microsoft.com/server-cloud/cortana-analytics-suite/overview.aspx) ve [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) sayfaları.

> [!TIP]
> Bu çözüm şablonu uygulamak için teknik kılavuzu için bkz: [teknik kılavuzu için Tahmine dayalı bakım Cortana Intelligence çözüm şablonuna](cortana-analytics-technical-guide-predictive-maintenance.md).
> Bu şablon mimari bir genel bakış sağlayan bir diyagram indirmek için bkz: [Cortana Intelligence çözüm şablonu Tahmine dayalı bakım mimarisini](cortana-analytics-architecture-predictive-maintenance.md).
> 
> 

## <a name="playbook-overview-and-target-audience"></a>Playbook genel bakış ve hedef kitle
Bu playbook teknik ve teknik olmayan İzleyici değişen arka planları ve Tahmine dayalı bakım alanı ilgi yararlanmak için düzenlenmiştir. Playbook iki farklı Tahmine dayalı bakım çözümü türleri ve bunların nasıl uygulanacağını ayrıntılarını üst düzey boyutlarının kapsar. Her ikisi de gereksinimini karşılamak için içerik dengeli yalnızca çözüm alanı ve uygulamaların yanı sıra bu çözümlerin bakarak ve bu nedenle teknik ayrıntılara ilgilendiğiniz olanlar türünü anlamak ilgilenen İzleyici.

Bu playbook içeriği çoğunluğu önceki veri bilimi bilgi veya uzmanlığı varsaymaz. Ancak, bazı bölümleri playbook biraz uygulama ayrıntılarını izleyin yapabilmek için veri bilimi kavramlarına aşina gerektirir. Giriş düzeyinde veri bilimi becerileri malzeme bu bölümler, tam olarak yararlanmasını gereklidir.

İş sorununu ayrıntılarla durumlarda Tahmine dayalı bakım nitelemek için çözüm, ortak bir koleksiyonunu kullanmak nasıl playbook kapsar Tahmine dayalı bakım uygulamalarına Giriş bir ilk yarısı, bunlar çevreleyen veri durumları ve bu Tahmine dayalı bakım çözümleri uygulamak iş avantajları kullanın. Bu bölüm Tahmine dayalı analiz etki alanındaki tüm teknik bilgi gerektirmez.

Playbook ikinci yarısında Tahmine dayalı modelleme teknikleri Tahmine dayalı bakım uygulamalar ve bu modeller playbook ilk yarısında özetlenen kullanım örnekleri örneklerinden aracılığıyla uygulama türlerini kapsar. Bu model verileri etiketleme ve mühendislik, özellik seçimi, eğitim ve sınama ve performansı en iyi yöntemler değerlendirme gibi verileri ön işleme adımları boyunca giderek gösterilmiştir. Bu bölüm, teknik İzleyici için uygundur.

## <a name="predictive-maintenance-in-iot"></a>IOT Tahmine dayalı bakım
Zamanlanmamış ekipman kapalı kalma süresi etkisi işletmeler için son derece zararlı olabilir. Alan donanım kullanımını ve performansını en üst düzeye çıkarmak için çalışan tutmak için çok önemlidir ve pahalı en aza indirerek, zamanlanmamış kapalı kalma süresi. Yalnızca hata oluştuğu için bekleniyor bugünün iş işlemleri Sahne uygun maliyetli değildir. Rekabet kalması için şirketler yaparak varlık performansını en üst düzeye çıkarmak için yeni yollar için Ara verilerin kullanımını toplanan çeşitli kanaldan. Gibi bilgileri analiz etmek için bir önemli gelecekteki sonuçları öngörmek için geçmiş modelleri kullanmanıza Tahmine dayalı analitik teknikleri kullanmaya yoludur. En popüler biri bu çözümler genellikle olabilen Tahmine dayalı bakım adlı olarak tanımlanan ancak böylece varlıkları önceden hataları belirlemek ve hataları oluşmadan önce harekete için izlenen bir varlığı hata olasılığını yakın gelecekte tahmin etmeye için sınırlı değildir. Bu çözümlerin hatasının en yüksek risk altındadır varlıklar belirlemek için hata desenlerini algılayabilir. Bu sorunların erken kimliği sınırlı bakım kaynakları daha düşük maliyetli bir şekilde dağıtmasını ve kalitesini geliştirmek ve tedarik zinciri süreçlerinin yardımcı olur.

Nesnelerin interneti (IOT) uygulamalar, bakım dünyasında veri koleksiyonu olarak artan dikkat kazanmasını ve teknolojileri işleme oluşturmak için yeterli olgunlaştığından Tahmine dayalı neden ile iletme, depolamak ve her türlü yığınlardaki veya gerçek zamanlı verileri analiz edin. Bu tür teknolojileri kolay geliştirme ve en büyük avantajı tartışmaya açık bir şekilde sağlayarak Tahmine dayalı bakım çözümleriyle Gelişmiş analiz çözümleri ile uçtan uca çözümler dağıtımını etkinleştirin.

İş sorunlarını beklenmeyen hataları ve sorunları karmaşık iş ortamlarında kök nedenini sınırlı bir anlayış nedeniyle işletimsel yüksek riskli Tahmine dayalı bakım etki alanı aralığında. Bu sorunların çoğunu altında aşağıdaki iş sorularını düşmesine kategorilere ayrılabilir:

* Bir donanım parçası yakın gelecekte başarısız olma olasılığını nedir?
* Kalan kullanım ömrü ekipmanın nedir?
* Hataları ve bu sorunları gidermek için hangi bakım eylemlerin gerçekleştirilmesi gereken nedenlerini nelerdir?

Bu soruları yanıtlamak için Tahmine dayalı bakım yararlanarak, işletme yapabilirsiniz:

* İşletimsel riskini azaltmak ve bunlar oluşmadan önce hataları belirleme varlıklar üzerinde geri dönüş oranını artırmak
* Gereksiz zamana dayalı bakım işlemlerini azaltmak ve bakım maliyetini denetleme
* Genel marka görüntü geliştirmek, hatalı tanıtım ve sonuçta elde edilen kayıp satış müşteri attrition ortadan kaldırmak.
* Stok düzeylerini sipariş noktayı tahmin ederek çalışmasını azaltarak stok maliyetleri
* Çeşitli bakım sorunlarına bağlı desenleri Bul

Tahmine dayalı bakım çözüm, gerçek zamanlı varlık koşul, varlıklar, ileriye dönük bakım etkinlikler için öneri ve yedek parçaların için tahmini sipariş tarihleri kalan kullanım ömrü tahmini izlemek için sistem durumu puanları gibi anahtar performans göstergelerinin işletmeler sağlayabilir.

## <a name="qualification-criteria-for-predictive-maintenance"></a>Tahmine dayalı bakım için nitelik ölçütleri
Tüm kullanım veya iş sorunlarını tarafından Tahmine dayalı bakım etkili bir şekilde çözülebilir vurgulamak önemlidir. Önemli sorun önceden ve en önemlisi, algılandığında hatalarını önlemek için eylem açık bir yolu var doğası gereği Tahmine dayalı olup niteliği ölçütünü dahil kullanım örneğini desteklemek için yeterli kalite verilerle kullanılabilir. Burada, biz başarılı Tahmine dayalı bakım çözümü oluşturmak için veri gereksinimleri odaklanılmaktadır.

Tahmine dayalı modeller oluştururken, geçmiş verileri, gizli desenleri tanıyabilir ve daha sonraki verilerdeki bu düzenleri tanımlamak modeli eğitmek için kullanırız. Bu modeller özelliklerine ve tahmin hedefi tarafından açıklanan örnekleriyle eğitilmiş olup. Eğitilen model yalnızca yeni örneklerin özelliklerinin bakarak hedef tahminlerde beklenir. Modeli özellik ve öngörü hedefi arasındaki ilişkiyi yakalama önemlidir. Model öğrenme etkili bir makine eğitmek için verilerin doğru tahminler yaptığınızdan beklenir tahmin hedefe ilgili anlamı tahmin hedef doğrultusunda Tahmine dayalı güç olan özellikler içeren eğitim verileri ihtiyacımız var.

Örneğin, hedef tren Tekerlek hatalarının tahmin etmek için ise, eğitim verileri tekerleği ilgili özelliklerin (Tekerlek mesafe, car yük, vb. sistem durumunu yansıtacak şekilde örneğin telemetri.) içermelidir. Hedef Eğitimi altyapısı hataları tahmin etmek için ise, ancak biz büyük olasılıkla başka bir altyapısı güvenlikle ilgili özellikler sahip eğitim veri kümesi gerekir. Tahmine dayalı modelleri oluşturmadan önce iş verilerini uygunluk için gereksinim anlamak ve ilgili veri alt kümesi analiz için seçmek için gerekli etki alanı bilgi sağlamak için uzman bekliyoruz.

Biz Tahmine dayalı bakım çözümü için uygun olması için bir iş sorununu uygun olduğunda aramak için üç temel veri kaynağı vardır:

1. Hata geçmişi: Genellikle, Tahmine dayalı bakım uygulamalarda hata çok nadir olaylardır. Ancak, hataları tahmin Tahmine dayalı modeller oluştururken, normal işlem düzeni ve bunun yanı sıra eğitim işlemiyle hatası düzeni öğrenmek algoritma gerekiyor. Bu nedenle, eğitim verileri bu iki farklı desenler bilgi almak için her iki kategorileri örneklerde yeterli sayıda içeren gereklidir. Bu nedenle, veri hatası olaylarının yeterli sayıda olmasını gerektirir. Hata olayları bakım kayıtlarında bulunabilir ve bölümleri değiştirme geçmiş veya eğitim verileri içinde anormallikleri de hataları olarak etki alanı uzmanlar tarafından tanımlandığı gibi kullanılabilir.
2. Bakım/tamir geçmişini: Temel bir Tahmine dayalı bakım çözümleri için veri kaynağını yerini bileşenler hakkında bilgi içeren varlık ayrıntılı bakım geçmişini, önleyici bakım gerçekleştirilen, vb. etkinleştirir. Bu olaylar düşüşü desenleri bu etkiler yakalamak son derece önemlidir ve bu bilgileri yokluğu yanıltıcı sonuçları neden olur.
3. Makine koşullar: başarısız önce kaç gün daha (saat, mil, işlemler, vb.) bir makine sürer tahmin etmek için bu işlemi sırasında zamanla makinenin sistem durumu düşürür varsayıyoruz. Bu nedenle, düşmesine yol açar, bu eskime düzeni ve tüm anormallikleri yakalama zaman değişen özellikleri içerecek şekilde veri bekliyoruz. IOT uygulamalarında iyi bir örnek farklı algılayıcılar telemetri verilerini temsil eder. Bir makine bir zaman çerçevesi içinde başarısız olacağını varsa tahmin etmek için ideal olarak verilerini düşürmesini eğilimi gerçek hata olayı önce bu zaman dilimi sırasında yakalama.

Ayrıca, işletim koşullarını öngörme hedef varlık için doğrudan ilgili veri gerektirir. Hedef karar hem de iş gereksinimlerinize ve veri kullanılabilirliğine bağlıdır. Tren tekerleği hata tahmini bir örnek olarak alma, biz "tekerleği bir sorununuz yayınlanıyorsa" veya "tüm tren bir sorununuz edecekse" tahmin. İkincisi tren başarısızlığını hedefler ancak ilk daha belirli bir bileşeni hedefler. İkinci bir model oluşturmak daha zor hale getirme ilk olandan çok daha fazla dağınık veri öğeleri gerektirir daha genel bir soru olabilir. Bilgi bileşen düzeyinde içermiyor olarak buna karşılık, yalnızca üst düzey tren koşul verileri bakarak tekerleği hataları tahmin çalışılırken uygun olmayabilir. Genel olarak, belirli hatası olaylarının daha genel olanları daha tahmin etmek daha duyarlı.

Genellikle hatası geçmiş verileri hakkında sorular bir ortak Soru "nasıl bir model ve kaç tane"yeterli"kabul edilen eğitmek için birçok hata olayları gereklidir? Bu soruya birçok Tahmine dayalı analiz senaryolarında olduğu gibi Temizle yanıt yok, genellikle kabul edilebilir nedir belirleyen veri kalitesi. Veri kümesi hatası tahmin için uygun olan özellikleri içermiyorsa, birçok hata olayları olsa bile sonra iyi bir model oluşturmak mümkün olmayabilir. Ancak, daha iyi modeli daha fazla hata olayları ve kaç tane hata örnekleri gereklidir, kabaca bir tahmin olduğunu çok bağlamını ve veri bağımlı ölçü udur. Bu sorun, burada size yeterli hataları olmaması ile ilgili sorun başa çıkacak şekilde yöntemleri önermek imbalanced veri kümeleri işlemek için bölümünde ele alınmıştır.

## <a name="sample-use-cases"></a>Örnek kullanım durumları
Bu bölümde, Tahmine dayalı bakım kullanım örneklerinden Havacılık, yardımcı programlar ve taşıma gibi birkaç sektörlerde koleksiyonu odaklanır. Her alt bölümde bu alanlarından toplanan kullanım örnekleri içine ayrıntılı açıklanmıştır ve iş sorununu ve Tahmine dayalı bakım çözümü avantajlarını çevreleyen veri bir iş sorununu ele almaktadır.

### <a name="aerospace"></a>Havacılık
#### <a name="use-case-1-flight-delay-and-cancellations"></a>Kullanım örneği 1: Uçuş gecikmesi ve iptalleri
##### <a name="business-problem-and-data-sources"></a>*İş sorun ve veri kaynakları*
Mekanik sorunları nedeniyle erteleniyor uçuşlar ilişkilendirilmiş önemli maliyetleri airlines yüz olduğu önemli iş sorunlardan biri. Mekanik hatası onarılamıyorsa uçuşlar bile iptal edilebilir. Bu, son derece gecikmeler zamanlama ve işlemleri, nedenleri hatalı saygınlığı ve diğer birçok sorunu birlikte müşteri memnuniyetsizliği sorunları oluştururken maliyetlidir. Airlines, özellikle, böylece uçuş gecikmeler veya iptalleri azaltabilir böyle mekanik hataları önceden tahmin ilgilendiğiniz. Bu durumlarda Tahmine dayalı bakım çözümü Gecikmeli veya iptal edilen, ilgili veri kaynaklarına bakım geçmişi ve uçuş rota bilgilerini gibi temel Uçağın olasılığını tahmin etmek için hedefidir. Bu kullanım durumu için iki önemli veri kaynakları uçuş Bacak ve sayfa günlükleri olur. Uçuş leg veri uçuş rota ayrıntıları tarih ve saat ayrılma ve varış, ayrılma ve varış havaalanları, vb. gibi ilgili veriler içerir. Sayfa günlük verileri bir dizi bakım personeli tarafından kaydedilen hata ve bakım kodları içerir.

##### <a name="business-value-of-the-predictive-model"></a>*Tahmine dayalı bir model iş değeri*
Kullanılabilir geçmiş verileri kullanarak Tahmine dayalı bir model gecikmesini ya da bir uçuş iptal sonraki 24 saat içinde sonuçlanır mekanik sorun türü tahmin etmek için çok sınıflandırma algoritması kullanılarak oluşturuldu. Bu tahmin yaparak, gerekli bakım Eylemler Uçağın hizmet ederken riskini azaltmak ve böylece olası gecikmeler veya iptalleri önlemek için alınabilir. Azure Machine Learning web hizmetini kullanarak, Tahmine dayalı modelleri sorunsuz bir şekilde ve kolayca airlines var olan işletim platformlarında tümleştirilebilir. 

#### <a name="use-case-2-aircraft-component-failure"></a>Kullanım örneği 2: Uçak bileşeni hatası
##### <a name="business-problem-and-data-sources"></a>*İş sorun ve veri kaynakları*
Uçak motorları ekipman çok hassas ve pahalı parçası ve altyapısı bölümü değişikliklerini uçak sektörde en sık kullanılan bakım görevlerini arasında. Bakım çözümleri airlines için bileşen stok kullanılabilirliği, teslim ve planlama dikkatli yönetim gerektirir. Bileşen güvenilirlik üzerinde Intelligence önemli ölçüde azalma yatırım maliyetlerinde müşteri adayları toplamak bölümlemeye. Algılayıcılar uçak sağlayan bilgi sayısından uçak koşula toplanan telemetri verileri bu kullanım örneği için ana veri kaynağıdır. Bakım kayıtları da zaman bileşen hatalar oluştu ve değişiklik yapıldı belirlemek için kullanıldı.

##### <a name="business-value-of-the-predictive-model"></a>Tahmine dayalı bir model iş değeri
Birden çok sınıf sınıflandırma modeli, bir sonraki ay içinde belirli bir bileşeni nedeniyle bir hata olasılığını tahmin oluşturuldu. Bu çözümlerin mimarisi kullanarak airlines bileşen onarım maliyetleri azaltmak, bileşen stok kullanılabilirliğini artırın, ilgili varlıklar stok düzeylerini küçültmek ve Bakım planlama geliştirmek.

### <a name="utilities"></a>Yardımcı programlar
#### <a name="use-case-1-atm-cash-dispense-failure"></a>Kullanım örneği 1: ATM nakit hatası etiket.
##### <a name="business-problem-and-data-sources"></a>*İş sorun ve veri kaynakları*
İşlerini birincil işletimsel risk varlıklarına beklenmeyen hatalarının olduğunu Yöneticiler varlık yoğun sektörlerde genellikle durum içinde. Örnek olarak, endüstri bankacılık içinde makineler ATM gibi başarısızlığını sıklıkla ortaya çok yaygın bir sorundur. Bu tür sorunları gibi makine işleçler için Tahmine dayalı bakım çözüm çok istenen olun. Bu kullanım örneğinde tahmin ATM nakit mevzuatı işlem nakit dağıtıcısının kağıt sıkıştı gibi bir hata veya bir bölümü hatası nedeniyle kesintiye uğrarsa olasılık hesaplamak için sorunudur. Ana veri bu durumda nakit notları dispensed sırada ölçümleri toplamak sensör okumaları ve aynı zamanda bakım kayıtları toplanan zamanla kaynaklarıdır. Tamamlanan her işlem başına sensör okumaları ve ayrıca sensör okumaları dispensed her Not başına algılayıcı verilerini dahil. Notlar, kalınlığı arasındaki boşlukları gibi algılayıcı sağlanan okumalar ölçümleri varış uzaklığı vb. unutmayın. Bakım veri, hata kodları ve onarım bilgileri dahil. Bunlar, hata durumlarını tanımlamak için kullanılmıştır.

##### <a name="business-value-of-the-predictive-model"></a>*Tahmine dayalı bir model iş değeri*
İki tahmine dayalı modelleri hataları nakit mevzuatı işlemler ve işlem sırasında dispensed notları tek tek başarısızlık tahmin etmek için oluşturulmuştur. İşlem hataları önceden tahmin etmek erişebildiklerinden, ATM proaktif olarak hataları oluşmasını önlemek için hizmet verebilir. Ayrıca, bir işlem büyük olasılıkla Not nedeniyle tamamlanmadan önce başarısız olması durumunda Not hatası tahmin ile hatası, etiket işlemi durdurur ve tamamlanmamış işlem müşteri yerine Bakım hizmeti bekleniyor büyük müşteri memnuniyetsizliği neden olabilir hata oluştuktan sonra gelmesi uyarmak en iyi yöntem olabilir.

#### <a name="use-case-2-wind-turbine-failures"></a>Kullanım örneği 2: Rüzgar Türbin hataları
##### <a name="business-problem-and-data-sources"></a>*İş sorun ve veri kaynakları*
Ortam tanıma olursa, Rüzgar turbines enerji nesil başlıca kaynaklarından biri duruma gelmiş ve bunlar genellikle milyonlarca dolar maliyeti. Rüzgar turbines anahtar bileşenleri bulunur Oluşturucu motor Türbin koşullar ve durumunu izlemek için yardımcı olacak birçok algılayıcılar ile biridir. Sensör okumaları başarısızlık Rüzgar Türbin bileşenleri için ortalama süresi gibi kritik anahtar performans göstergelerini (KPI'lar) tahmin etmek için Tahmine dayalı bir model oluşturmak için kullanılan değerli bilgiler içerir. Bu kullanım örneği için veriler üç farklı grubu konumda bulunan birden çok Rüzgar turbines gelir. Ölçümler yüzlerce algılayıcılar her Türbin gelen yakın 10 dakikada bir yıl için kaydedilmiş. Bu okumalar sıcaklık, oluşturucu hızı, Türbin güç ve oluşturucu sargı gibi ölçüleri içerir.

##### <a name="business-value-of-the-predictive-model"></a>*Tahmine dayalı bir model iş değeri*
Tahmine dayalı modelleri oluşturucuları ve sıcaklık algılayıcıları için kalan kullanım ömrü tahmin etmek için oluşturulmuştur. Hata olasılığını tahmin ederek çalışmasını bakım teknisyenleri büyük olasılıkla yakında zamana dayalı bakım regimes tamamlamak üzere başarısız olacak şüpheli turbines üzerinde odaklanabilirsiniz.
Ayrıca, Tahmine dayalı modelleri sorunları kök nedenini daha iyi anlamasına yardımcı olması için iş yardımcı olan bir hata olasılığını farklı faktörlerine katkısı düzeyine Insight getirin.

#### <a name="use-case-3-circuit-breaker-failures"></a>Kullanım örneği 3: devre kesici hataları
##### <a name="business-problem-and-data-sources"></a>*İş sorun ve veri kaynakları*
Oluşturma, dağıtım ve elektrik enerji satışının dahil elektrik ve gaz işlemler için ekleyebileceği enerji teslimini güvence altına almak için her zaman güç satırları işletimsel olduğundan emin olmak için bakım önemli miktarda gerektirir. Bu tür işlemler başarısız neredeyse her varlık güç sorunları ortaya bölgelerde parametreden gibi kritik öneme sahiptir. Bağlantı hattı ayırıcıları sorunları ve herhangi bir zarar güç satırlarına oluşmasını önlemek için kısa devreler durumunda elektrik geçerli kesme donanım parçası olarak bu tür işlemler için kritik öneme sahiptir. Verilen bakım günlükleri, komut geçmişi ve teknik belirtimler devre kesici hataları tahmin etmek için bu kullanım örneği için iş sorun değildir.

Bu durumda üç ana veri kaynağı düzeltme, önleyici ve sistematik eylemlerini içeren bakım günlüklerin, otomatik ve el ile komutları içeren işletimsel veri hattı ayırıcıları açma ve kapama eylemleri ve her devre kesici yıl gibi özellikleri hakkında teknik belirtim verileri için yaptığınız gibi konum, model vb. gönderin.

##### <a name="business-value-of-the-predictive-model"></a>*Tahmine dayalı bir model iş değeri*
Tahmine dayalı bakım çözüm onarım maliyetlerini azaltma ve bağlantı hattı ayırıcıları gibi donanım yaşam döngüsü artırmak yardımcı olur. Ayrıca bu modeller modelleri olan hizmet daha az kesintilerine neden beklenmeyen hataları azaltır uyarıları önceden sağladıklarından dolayı güç ağ kalitesini yardımcı olur.

#### <a name="use-case-4-elevator-door-failures"></a>Kullanım örneği 4: Fırsatınızdır kapı hataları
##### <a name="business-problem-and-data-sources"></a>*İş sorun ve veri kaynakları*
En büyük fırsatınızdır şirketlerin tipik olarak dünyanın çalıştıran asansörler milyonlarca vardır. Rekabet sağlamak için bunların ne müşterilerine en önemli olan güvenilirlik odaklanır. Nesnelerin interneti, olası üzerinde kendi asansörler buluta bağlayarak ve fırsatınızdır algılayıcılar ve sistemlerden, veri toplamayı çizim bunlar artıran çok operations henüz rakiplere için kullanılabilir olan bir şey olmayan Tahmine dayalı ve PreEmptive tarafından bakım sunarak değerli iş zekası verileri dönüştürmek mümkün. Bu durumda iş gereksinimini kapı hatalarının olası neden tahmin bir Bilgi Bankası Tahmine dayalı uygulaması sağlamaktır. Bu uygulama için gerekli verileri kullanım bilgilerini (örneğin sayısı kapı döngüleri, ortalama kapı Kapat zaman, vb.) ve hata geçmişi (yani geçmiş hata kaydeder ve bunların nedenleri) (örneğin tanımlayıcıları sözleşme bakım sıklığı, yapı türü, vb.) fırsatınızdır statik özellikleri olan üç bölümden oluşur.

Bir çok sınıflı Lojistik regresyon modeli tümleşik statik özellikler ve kullanım verileri özellikleri olarak ve geçmiş başarısızlığı kayıtları sınıfı etiket olarak nedenleri ile tahmin sorunu çözmek için Azure Machine Learning ile oluşturuldu. Bu Tahmine dayalı bir model alanı teknisyenleri çalışma verimliliğini artırmaya yardımcı olmak için kullanılan bir mobil cihazda bir uygulama tarafından kullanılır. Bir teknisyen bir fırsatınızdır onarmak site gittiğinde, klasöründe bu uygulama için önerilen nedenleri ve en iyi kurslar bakım eylemlerinin kadar hızlı fırsatınızdır kapıları düzeltmek başvurabilirsiniz.

### <a name="transportation-and-logistics"></a>Taşıma ve lojistik
#### <a name="use-case-1-brake-disc-failures"></a>Kullanım örneği 1: Fren disk hataları
##### <a name="business-problem-and-data-sources"></a>*İş sorun ve veri kaynakları*
Tipik bakım araçları için düzeltme ve önleyici bakım ilkelerdir. Düzeltme bakım ciddi bir sorundan dolayı beklenmeyen bir arıza sonucunda sürücü neden olabilir bir hata gerçekleştikten sonra araç onarılana ve zaman teknisyenin bir ziyarette küçülttüğü iyi bir şekilde anlamına gelir. Çoğu araçları Ayrıca belirli incelemeleri araba alt sistemleri gerçek koşulunu dikkate almaz bir zamanlamasında gerçekleştirilmesi bir önleyici bakım İlkesi tabidir. Bu yaklaşım hiçbiri tam olarak sorunların giderilmesinde başarılı. Belirli kullanım burada Fren disk hatası tahmin, geçmiş yönlendirmeli desenleri ve araba maruz başka koşullar izler bir araba lastiği sisteminde yüklü algılayıcılar aracılığıyla toplanan verileri temel alan bir durumdur. Bu durumda en önemli veri kaynağı örneği için yürüten uzaklıkları, hız, vb. düzenleri braking ivmelerini ölçmek algılayıcı verilerini ' dir. Bu bilgiler, bağlı diğer statik gibi bilgiler araba özellikleri iyi bir Tahmine dayalı bir model kullanılabilir predictors kümesi Yardım derleme. Başka bir önemli bilgi (araba işlendikçe yedek parça sipariş tarihleri ve sayıları dealerships tutmak için kullanılan) bölümü sipariş veritabanından olayla hatası verileri kümesidir.

##### <a name="business-value-of-the-predictive-model"></a>*Tahmine dayalı bir model iş değeri*
İş burada Tahmine dayalı bir yaklaşım önemli değeri. Tahmine dayalı bakım sistem Tahmine dayalı bir modeli temelinde dağıtıcı ziyaret zamanlayabilirsiniz. Model, car ve yönlendirmeli geçmişi geçerli durumunu temsil eden sensory bilgiye dayalı olabilir. Bu yaklaşım, sonraki düzenli bakım önce de oluşabilir beklenmeyen arıza riskini en aza indirebilirsiniz.
Ayrıca, gereksiz önleyici bakım miktarını da azaltabilirsiniz. Sürücü proaktif olarak bölümleri, bir değişiklik birkaç hafta içinde gerekli ve bu bilgilerle dağıtıcı tedarik bilgilendirilmek. Dağıtıcı sonra tek tek bakım paket sürücüsünü önceden hazırlayın.

#### <a name="use-case-2-subway-train-door-failures"></a>Kullanım örneği 2: Yeraltı treni tren kapı hataları
##### <a name="business-problem-and-data-sources"></a>*İş sorun ve veri kaynakları*
Tren araba kapı hatalarının gecikmeleri ve sorunları Yeraltı treni işlemlerini ana nedenlerinden biridir. Tren araba kapı hatası veya sonraki door hatası kasa gün sayısını tahmin mümkün gerekecekse tahmin etmeye son derece önemli öngörü olur. Tren kapı bakım iyileştirmek ve tren'ın aşağı süresini azaltmak olanağı sağlar.

#### <a name="data-sources"></a>Veri kaynakları
Bu kullanım örneğindeki verilerin üç kaynakları 

* **Olay verileri eğitmek**, geçmiş kayıtlarını tren olayların olduğu 
* **Bakım verileri** bakım türleri, iş emri türleri ve öncelik kodları gibi  
* **hataları kayıtlarını**.

##### <a name="business-value-of-the-predictive-model"></a>*Tahmine dayalı bir model iş değeri*
İki modeli ikili sınıflandırma ve gün regresyon kullanarak hatanın kasa kullanarak sonraki gün hatası olasılığını tahmin etmek için oluşturulmuştur. Önceki durumlarda benzer, modelleri hizmet kalitesini ve düzenli bakım regimes depolamanın tarafından müşteri memnuniyetini artırmak için büyük bir fırsat oluşturun.

## <a name="data-preparation"></a>Veri hazırlama
### <a name="data-sources"></a>Veri kaynakları
Tahmine dayalı bakım sorunları için ortak veri öğeleri şu şekilde özetlenebilir:

* Hata geçmiş: makine ya da makineyi içinde bileşen hatası geçmişi.
* Bakım geçmişi: Örneğin hata kodları, önceki bakım etkinlikleri veya bileşen değişikliklerini makinenin onarım geçmişi.
* Makine koşullar ve Kullanım: Makine Örneğin veri işletim koşulları toplanan algılayıcı.
* Makine özellikler: Örneğin altyapısı boyutu, bir makine özelliklerini marka ve model, konumu.
* İşleç özellikleri: işleci özelliklerini örneğin cinsiyet, deneyimi.

Bu olası ve genellikle özel hata kodları veya yedek parça sipariş tarihleri biçiminde bakım geçmişi gibi hata geçmişi bulunan durumda olur. Bu gibi durumlarda hataları bakım verilerden ayıklanabilir. Ayrıca, farklı iş etki alanı burada ayrıntısına listelenmeyen hatası desenleri etkileyen diğer veri kaynaklarının çeşitli olabilir. Bu, etki alanı uzmanlar Tahmine dayalı modeller oluştururken danışmanlık tanımlanmalıdır.

Bazı veri öğeleri yukarıda kullanım örneklerinden örnekler:

Hata geçmiş: gecikme tarihleri, uçak bileşen hatası tarihleri ve türleri, ATM nakit mevzuatı işlem hataları, tren/fırsatınızdır kapı hataları, Fren disk değiştirme sipariş tarihleri, Rüzgar Türbin hatası tarihleri ve devre kesici komutu hataları mücadele.

Bakım geçmişi: uçuş hata günlükleri, ATM işlem hata günlüklerini, eğitmek bakım türü, kısa bir açıklama vb. ve devre kesici bakım kayıtları dahil olmak üzere bakım kaydeder.

Makine koşullar ve Kullanım: uçuş yönlendirir ve uçak motorları, ATM işlemleri ait sensör okumaları toplanan algılayıcı verilerini kez, olaylar verilerini, Rüzgar turbines, asansörler ve bağlı araba ait sensör okumaları eğitmek.

Makine özellikler: Voltaj düzeyleri gibi devre kesici teknik özellikler, marka, model, altyapı gibi coğrafi konuma veya araba özellikleri boyut, lastiği türleri, üretim tesis vb.

Yukarıdaki veri kaynakları verildiğinde, biz Tahmine dayalı bakım etki alanında gözlemlemek iki ana veri zamana bağlı veri ve statik veri türleridir.
Hata geçmişi, makine koşullar, her veri parçası için koleksiyon saati gösteren zaman damgalarını ile gelen geçmişi, neredeyse her zaman kullanım geçmişini onarın. Bunlar genellikle makineler veya işlecin özellikleri teknik özellikleri açıklamak bu yana makine özellikler ve işleci özellikler genel statik. Zaman içinde değiştirmek bu özellikleri mümkündür ve bu durumda veri kaynakları damgalı sürede olunmalıdır.

### <a name="merging-data-sources"></a>Veri Kaynakları birleştirme
Herhangi bir türde özellik Mühendisliği veya etiketleme işlem almadan önce biz öncelikle verilerimizi özelliklerinden oluşturmak için gereken biçimde hazırlamanız gerekir. Nihai amacıyla öğrenme algoritmasının makinesine beslenecek özellikleri ve etiketleri ile her varlık için her zaman birimi için bir kayıt oluşturmaktır. Temiz son bu veri kümesi hazırlamak için önceden işlem bazı adımlar gerçekleştirilmelidir. Bir varlık için bir zaman birimi her kayıt ait olduğu zaman birimlerine veri toplama süresi bölmek ilk adımdır. Kolaylık sağlamak için zaman birimlerini açıklamaları geri kalanı için kullandığımız ancak veri toplama aynı zamanda diğer birimler gibi eylemler, bölünebilir.

Ölçü birimi kez saniye, dakika, saat, gün, ay, döngü, mil veya veri hazırlığı ve değişiklikleri verimliliğini bağlı olarak hareket zaman biriminden varlık diğer veya diğer etkenlere bağlı koşullar etki alanına özel gözlenen olabilir. Diğer bir deyişle, verileri olduğu gibi birçok veri toplama sıklığını durumlarda aynı herhangi bir birimi farkı diğer gösterilmeyebilir olmasını zaman birimi yok. Sıcaklık değerleri 10 saniyede toplanmakta, örneğin, bir zaman birimi tüm çözümleme için 10 saniye çekme örnekleri sayısı ek bilgileri sağlamadan Şişir. Daha iyi stratejisi, örnek olarak bir saat üzerinden ortalama kullanacak biçimde olacaktır.

Önceki bölümde açıklanan olası veri kaynakları için örnek bir genel veriyi şemaları şunlardır:

Bakım kayıtları: Bunlar gerçekleştirilen bakım işlemleri kaydeder. Ham bakım verileri, genellikle bir varlık kimliği ve zaman damgası o anda bakım etkinlikleri gerçekleştirilmiş hakkında bilgi ile birlikte gelir. Bu tür ham veriler durumunda bakım etkinlikleri kategorik sütunlara bir bakım eylemi türüne karşılık gelen her kategorisiyle çevrilmesi gerekir. Temel veri şeması bakım kayıtları için varlık kimliği, saati ve Bakım eylemi sütunları içerir.

Hata kaydeder: Bu hataları veya hata nedeni dediğimiz tahmin hedefine ait kayıtlardır. Bu, belirli hata kodları veya hatalar belirli iş koşul tarafından tanımlanan olayları olabilir. Bazı durumlarda, verileri bazıları ilgi hatalarına karşılık gelen birden çok hata kodları içerir. Tüm hataları tahmin hedefi olduğundan, diğer hatalar genellikle hatalarıyla ilişkilendirilebilir özellikleri oluşturmak için kullanılır. Neden olup olmadığını başarısızlığı kayıtları için temel veri şeması varlık kimliği, zaman ve hata veya başarısızlık nedeni sütunları içerir.

Makine koşul: Bu verilerin çalışma koşullarını ilgili verilerin izlenmesi tercihen gerçek zamanlı. Örneğin, kapı hataları için Kapak açma ve kapatma süreleri iyi kapıları geçerli durumuyla ilgili göstergesidir. Temel veri şeması makine koşulları için varlık kimliği, saati ve durum değeri sütunları içerir.

Makine and işleci veri: Makine and işleci veri birleştirilmiş hangi varlık varlık ve işleci özelliklerinin yanı sıra hangi işleci tarafından işletilen tanımlamak için bir şema içine. Örneğin, bir araba genellikle yaşı gibi deneyimi vb. yürüten özniteliklere sahip bir sürücü aittir. Bu veriler zamanla değişirse, bu veriler ayrıca bir saat sütunu içermelidir ve özellik oluşturma için veri değişen zaman olarak değerlendirilmelidir. Temel veri şeması makine koşulları için varlık kimliği, varlık özellikleri, operatör kodu ve işleci özellikler içerir.

Etiketleme ve özellik oluşturma önce son tablo makine koşulları hatası kayıtlarda varlık kimliği ve zaman alanları tabloyla birleştirme sol tarafından oluşturulabilir. Bu tabloda ardından varlık kimliği üzerinde bakım kayıtlarla katılabilir ve zaman alanları ve son olarak makine ve işleçle birlikte varlık kimliği temel özellikleri Makine normal işlemde olduğunda ilk sol birleştirme hatası sütunu için null değerler bırakır, bunlar normal işlem için bir gösterge değeri tarafından imputed. Bu hata sütun etiketleri için Tahmine dayalı bir model oluşturmak için kullanılır.

### <a name="feature-engineering"></a>Özellik mühendisliği
İlk modelleme özellik Mühendisliği adımdır. Özellik nesil kavramsal açıklamak ve zaman için en fazla toplanan geçmiş verileri kullanarak belirli bir zamanda bir makinenin sistem durumu koşulu soyut olur. Sonraki bölümde, Tahmine dayalı Bakım ve etiketleme için her tekniği nasıl yapıldığını için kullanılan teknikleri tür genel bir bakış sağlar. Kullanılması gereken tam teknik verileri ve iş soruna bağlıdır. Ancak, aşağıda açıklanan özelliği mühendislik yöntemleri taban çizgisi olarak özellikleri oluşturmak için kullanılır. Aşağıda, biz, zaman damgaları ve ayrıca statik veri kaynaklarından oluşturulan statik özellikler birlikte gelen veri kaynaklarından oluşturulması ve kullanım örnekleri örneklerinden sağlayan öteleme ele alınmaktadır.

#### <a name="lag-features"></a>Gecikme özellikleri
Tahmine dayalı Bakım'ın daha önce belirtildiği gibi geçmiş verileri genellikle her veri parçası için koleksiyon saati gösteren zaman damgalı gelir. Zaman damgalı verilerle birlikte gelen verileri özellikleri oluşturma, birçok yolu vardır. Bu bölümde, Tahmine dayalı bakım için kullanılan bu yöntemlerden bazıları aşağıdakiler ele. Ancak, biz bu yöntemleri sınırlı değildir. Özellik Mühendisliği Tahmine dayalı modelleme en yaratıcı alanlarında biri olarak kabul edilir beri özellikleri oluşturmak için birçok yolu olabilir. Burada, bazı genel teknikleri sunuyoruz.

##### <a name="rolling-aggregates"></a>*Toplamalar alınıyor*
Her bir varlık kayıt için çalışırken bir pencere için geçmiş toplamları hesaplamak için isteriz zaman birimlerinin sayısı olan "w" harfinin boyutunu seçin. Bu kayıt tarihinden önce W dönemlerini kullanarak toplama özellik çalışırken sonra işlem. Toplamalar çalışırken bazı örnek çalışırken sayıları, anlamına gelir, Standart sapmanın, Standart sapmanın dayalı aykırı değerlerini, CUSUM ölçer, pencere için minimum ve maksimum değerleri. Başka bir ilginç teknik eğilim değişiklikler, ani ve anomali algılama algoritmalarını kullanarak verileri anormallikleri algılama algoritmalarını kullanarak düzeyi değişikliklerini yakalamaktır.

Tanıtımı için bkz: Şekil burada biz zaman mavi çizgili her birim için bir varlık için kaydedilen algılayıcı değerlerini temsil eden ve çalışırken t kayıtları ortalama özellik hesaplama W = 3 işaretlemek 1<sub>1</sub> ve t<sub>2</sub> turuncu tarafından belirtilir ve gruplandırmaları sırasıyla yeşil.

![Şekil 1 '. Toplama özellikleri alınıyor](./media/cortana-analytics-playbook-predictive-maintenance/rolling-aggregate-features.png)

Şekil 1 '. Toplama özellikleri alınıyor

Örnek olarak, uçak bileşeni hatası için son üç günde ve son günü geçen hafta algılayıcı değerlerini çalışırken anlamına gelir, standart sapma ve toplam özellikleri oluşturmak için kullanılmıştır. Benzer şekilde, üç Standart sapmanın, üst ve alt CUMSUM özellikleri ötesinde aykırı değerlerini sayısı için ATM hataları, ham algılayıcı değerlerini ve çalışırken anlamına gelir, Orta, aralığı, Standart sapmanın kullanılmıştır.

Uçuş gecikme tahmin için geçen hafta kodlarından özellikleri oluşturmak için kullanılan hata sayar. Tren kapı hataları için olayları sayıları son gün önceki 2 hafta içinde olayların sayısı ve olayları önceki 15 gün sayısı varyansını öteleme özellikleri oluşturmak için kullanılmıştır. Aynı sayım bakım ilgili olayları için kullanıldı.

Ayrıca, bir W çekme tarafından çok büyük (örn. Yıl), tüm bakım kayıtları sayım gibi bir varlık tam geçmişini bakmak olası hataları vs. Kayıt süre kadar. Bu yöntem, son üç yıl devre kesici hataları sayım için kullanıldı. Ayrıca tren hataları için tüm bakım olayları kullanarak uzun süreli bakım efektleri yakalamak için bir özellik sayılan.

##### <a name="tumbling-aggregates"></a>*Dönen toplar*
Bir varlık etiketli her kayıt için kimliğinizi bir pencere boyutunu çekme "W -<sub>k</sub>" k Burada, sayı veya windows "W" boyutunun öteleme özellikleri oluşturmak için istiyoruz. "k", uzun vadeli düşüşü desenleri yakalamak için büyük bir sayı veya kısa vadeli efektleri yakalamak için küçük bir sayı olarak çekilebilir. Ardından k dönen windows W - kullanırız<sub>k</sub> , W -<sub>(k-1)</sub>,..., W -<sub>2</sub> , W -<sub>1</sub> (bkz. Şekil 2) kayıt tarihi ve saati önce süreyle toplama özellikleri oluşturmak için. Bunlar ayrıca windows kayıt düzeyinde yakalanmaz için bir zaman birimi Şekil 2'de geri ancak aynı şekilde, Şekil 1 where fikirdir t<sub>2</sub> çalışırken etkisi göstermek için de kullanılır.

![Şekil 2. Toplama özellikleri dönen](./media/cortana-analytics-playbook-predictive-maintenance/tumbling-aggregate-features.png)

Şekil 2. Toplama özellikleri dönen

Örnek olarak, Rüzgar turbines, H = 1 ve k = 3 ay öteleme özellikleri her üst ve alt aykırı değerlerini kullanarak son 3 ay için oluşturmak için kullanılmış.

#### <a name="static-features"></a>Statik özellikler
Üretim tarihi, model numarası, konum vb. gibi ekipmanının teknik özellikleri şunlardır. Öteleme özellikleri genellikle doğası gereği sayısal olsa da, statik özellikleri genellikle modellerindeki kategorik değişkenleri duruma gelir. Bir örnek, voltaj, geçerli gibi devre kesici özellikleri ve güç belirtimleri transformer türleri ile birlikte, güç kaynakları vb. kullanılmıştır. Fren disk hataları için karışımı oldukları veya çelik statik özelliklerden bazıları kullanılmış gibi wheels gibi lastiği türü.

Özellik oluşturma sırasında eksik değerleri ve normalleştirme işleme gibi bazı diğer önemli adımlar gerçekleştirilmelidir. Eksik değeri imputation ve ayrıca burada ele alınmamıştır veri normalleştirme çeşitli yöntemler vardır. Ancak, tahmini performans artışı mümkün olup olmadığını görmek için farklı yöntemler denemek faydalıdır.

Zaman birimi bir gün olduğunda önceki bölümde açıklanan adımları mühendislik özelliği sonra son özellik tablosu aşağıdaki örnek veri şeması benzemelidir:

| Varlık Kimliği | Zaman | Özellik sütunları | Etiket |
| --- | --- | --- | --- |
| 1 |Günde 1 | | |
| 1 |Gün 2 | | |
| ... |... | | |
| 2 |Günde 1 | | |
| 2 |Gün 2 | | |
| ... |... | | |

## <a name="modeling-techniques"></a>Modelleme teknikleri
Tahmine dayalı bakım genellikle birçok farklı açılarını Tahmine dayalı modelleme perspektif yaklaşıldığında iş sorularını kullanan çok zengin bir etki alanıdır. Sonraki bölümlerde, Tahmine dayalı bakım çözümleriyle yanıtlanması farklı iş sorularını model oluşturmak için kullanılan ana teknikleri sunuyoruz. Benzerlikler olsa da, her modeli kendi yolunu ayrıntılı olarak açıklanan oluşturma etiket vardır. Eşlik eden bir kaynak olarak Azure Machine Learning içinde sağlanan örnek denemeleri dahil Tahmine dayalı bakım şablonu başvurabilirsiniz. Bu şablon için çevrimiçi malzeme bağlantılar kaynaklar bölümünde verilmiştir. Bazı özellik mühendislik teknikleri yukarıda açıklanan ve Azure Machine Learning kullanarak uçak motoru hataları tahmin etmek için sonraki bölümlerde açıklanan modelleme teknikleri uygulanır nasıl görebilirsiniz.

### <a name="binary-classification-for-predictive-maintenance"></a>Tahmine dayalı bakım için ikili sınıflandırma
Tahmine dayalı bakım için ikili Sınıflandırma, gelecekteki bir süre içinde donanım başarısız olma olasılığını tahmin etmek için kullanılır. Süre tarafından belirlenir ve iş kuralları ve veri elinizdeki göre. Bazı ortak dönemleri hasar bileşenleri ya da bu süre içinde oluştuğu sorunu düzeltmek için bakım yordamları gerçekleştirmeye bakım kaynak dağıtmak için gereken süre büyük olasılıkla değiştirmek için yedek parçaların satın almak için gerekli minimum sağlama süresi ' dir. Bu gelecekteki yatay süresi "X" diyoruz.

İkili sınıflandırma kullanmak için iki tür pozitif ve negatif dediğimiz örnekler tanımlamak ihtiyacımız. Her örneğin bir zaman birimi kavramsal olarak açıklayan ve o zaman birimi özellik Mühendisliği geçmiş kullanarak ve daha önce açıklanan diğer veri kaynakları aracılığıyla kadar işletim koşullar özetleyen bir varlık için ait olduğu bir kayıttır. Tahmine dayalı bakım için ikili sınıflandırma bağlamında pozitif türde (etiketi 1) hataları gösterir ve negatif tür normal işlemleri gösterir (etiket = 0) etiketleri türü kategorik olduğu. Hedeftir her yeni örnek büyük olasılıkla başarısız ya da sonraki içinde normal olarak çalışmasına olarak tanımlayan bir model bulmak için zaman birimlerini X.

#### <a name="label-construction"></a>Etiket oluşturma
Soruyu yanıtlamak için Tahmine dayalı bir model oluşturmak için "sonraki varlık başarısız olma olasılığını nedir zaman birimleri X?", etiketleme alma X kayıtlar bir varlık ve bunları "için yaklaşık başarısız" olarak etiketleme arıza öncesinde tarafından yapılır (Etiket = 1) "normal" olarak diğer tüm kayıtları etiketleme sırasında (etiket = 0). Bu yöntemde, etiketleri (bkz: Şekil 3) kategorik değişkenlerdir.

![Şekil 3 '. İkili sınıflandırma etiketleme](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-binary-classification.png)

Şekil 3 '. İkili sınıflandırma etiketleme

Uçuş gecikmeleri ve iptalleri için X gecikmeler sonraki 24 saat içindeki tahmin etmek için bir gün çekilir. Hataları önce 24 saat içinde olan tüm uçuşları 1'ler etiketlenmiş. Hatalar için ATM nakit etiket, iki ikili sınıflandırma modelleri sonraki 10 dakika içinde bir işlem hatası olasılığını tahmin etmek ve dispensed sonraki 100 notları hatası olasılığını tahmin etmek için oluşturulmuş. Hatanın son 10 dakika içinde gerçekleşen tüm işlemleri, ilk model için 1 olarak etiketlenir. Ve bir hatanın son 100 notlarına dispensed tüm notları ikinci modeli için 1 olarak etiketlenmiş. Devre kesici hataları için bir sonraki komut olması durumda X seçilir sonraki devre kesici komutu başarısız olma olasılığını tahmin etmek için bir görevdir. Tren kapı hataları için hataları sonraki 7 gün içinde tahmin etmek için ikili sınıflandırma modeli oluşturuldu. Rüzgar Türbin hataları için X 3 ay seçildi.

Türbin Rüzgar ve durumlarda da aynı verileri kullanarak kullanım ömrü kalan tahmin etmek regresyon çözümlemesi için ancak, sonraki bölümde açıklanan farklı bir etiketleme stratejisi aynı kullanarak kullanılan kapı eğitmek.

### <a name="regression-for-predictive-maintenance"></a>Tahmine dayalı bakım regresyon
Tahmine dayalı bakım regresyon modellerinde süreyi sonraki hata gerçekleşmeden önce varlık çalışır durumda tanımlanan bir malın kalan kullanım ömrü (RUL) hesaplamak için kullanılır. İkili sınıflandırma aynı, her bir varlık için bir zaman birimi "Y" ait bir kayıt örnektir. Ancak, regresyon bağlamında, her yeni örnek kalan kullanım ömrü hatasından önce kalan süre olan sürekli bir sayı olarak hesaplar bir model bulmak için hedeftir. Bu süre içinde bazı birden çok y diyoruz. Her örneğin ayrıca sonraki hata vermeden önce bu örneğin kalan süreyi ölçerek hesaplanan bir kalan kullanım ömrü vardır.

#### <a name="label-construction"></a>Etiket oluşturma
Soru verilen "ekipmanın kalan kullanım ömrü nedir?
", regresyon modeli arıza öncesinde her kayıt alma ve sonraki hatasından önce kaç saat biriminin kalır hesaplayarak etiketleme tarafından oluşturulan için etiketler. Bu yöntemde, etiketleri sürekli (bkz: Şekil 4) değişkenlerdir.

![Şekil 4 '. Regresyon için etiketleme](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-regression.png)

Şekil 4 '. Regresyon için etiketleme

İkili Sınıflandırma, regresyon, farklı veri herhangi bir hata olmadan varlıklar etiketleme bir hata noktası bağlamında yapılır ve kendi hesaplama ne kadar varlık hatasından önce derdi'bitti bilmeden olası değil olarak modelleme için kullanılamaz. Bu sorunu en iyi hayatta analiz adında başka bir istatistik teknik tarafından ele.
Biz hayatta analiz bu playbook sık aralıklarla zaman değişen verilerle ilgili Tahmine dayalı bakım kullanım durumları için teknik uygulama işlemi sırasında oluşabilecek olası karışıklıklardan dolayı ele alınacaktır değil.

### <a name="multi-class-classification-for-predictive-maintenance"></a>Tahmine dayalı bakım için birden çok sınıf sınıflandırma
Tahmine dayalı bakım için birden çok sınıf sınıflandırma iki gelecekteki sonuçları öngörmek için kullanılabilir. Birinci birden fazla olası süreler hatasına her varlık için bir zaman aralığı vermek birine bir varlık atamaktır. İkincisi birden çok kök nedenlerden biri nedeniyle gelecekteki bir süre içinde hata olasılığını belirlemektir. Bu bilgiyle sorunları önceden işlemek için donatılmıştır bakım personeli sağlayan. Başka bir çok sınıfı modelleme teknikleri en olası kök nedenini belirlemeye odaklanan bir hata verilir. Bu bir hata çözmek için yapılması üst bakım işlemleri verilecek önerileri sağlar.
Sahip tarafından kök sıralı bir listesini neden olur ve ilişkili onarım Eylemler,  
teknisyenler arızalarının ardından ilk onarım eylemlerini alırken daha etkili olabilir.

#### <a name="label-construction"></a>Etiket oluşturma
Olan iki soruya verilen "bir varlığı zaman sonraki"aZ"birimlerinde başarısız olma olasılığını nedir"a"dönem sayısını burada" ve "sonraki varlık başarısız olma olasılığını nedir sorunu nedeniyle zaman birimleri X" P<sub>ı</sub>"" i"etiketleme, olası nedenlerini sayısı olduğu aşağıdaki şekilde bu teknikler için yapılır.

İlk soru için etiketleme bir varlık arıza öncesinde aZ kayıtları almak ve bunları etiketleme yapılır "normal" olarak diğer tüm kayıtları etiketleme sırasında etiketlerine süre (3Z, 2Z, Z) demet kullanarak (etiket = 0). Bu yöntemde, etiket kategorik (bkz: Şekil 5) değişkendir.

![Şekil 5. İçin hata zaman tahmini çok sınıflı sınıflandırma etiketleme](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-failure-time-prediction.png)

Şekil 5. İçin hata zaman tahmini çok sınıflı sınıflandırma etiketleme

İkinci soru için etiketleme alma X kayıtları bir varlık ve bunları olarak etiketleme arıza öncesinde gerçekleştirilir "P sorunu nedeniyle başarısız üzere<sub>ı</sub>" (etiket = P<sub>ı</sub>) diğer tüm kayıtları "normal" olarak etiketleme sırasında (etiket = 0). Bu yöntemde, etiketleri kategorik (bkz: Şekil 6) değişkenlerdir.

![Şekil 6. İçin kök nedeni tahmin çok sınıflı sınıflandırma etiketleme](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-root-cause-prediction.png)

Şekil 6. İçin kök nedeni tahmin çok sınıflı sınıflandırma etiketleme

Model hatası olasılığını her P nedeniyle atar<sub>ı</sub> hiçbir hata olasılığını yanı sıra. Bu olasılıklar gelecekte gerçekleşmesi en olası sorunları tahmin izin vermek için büyüklük sıralanabilir. Uçak bileşen hatası kullanım örneği çok sınıflı sınıflandırma sorunu yapılandırılmış. Bu, sonraki ay içinde gerçekleşen iki farklı baskısı vana bileşenleri çalıştırılamadığından olasılıklar tahminini sağlar.

Bakım işlemleri arızalarının ardından önermek için etiketleme çekilmesi için gelecekteki bir yatay gerektirmez. Model hatası gelecekte tahmin etmektir değil, ancak hata zaten gerçekleştirilmedi sonra en olası kök nedeni yalnızca tahmin etmeye nedeni budur. Burada belirtilen koşullar işletim üzerinde geçmiş veri hatanın nedenini tahmin etmek için hedef, üçüncü durumda içine fırsatınızdır kapı hataları ayrılır. Bu model, ardından bir hata gerçekleştikten sonra büyük olasılıkla kök neden tahmin etmek için kullanılır. Bu model önemli yararlarından biri, aksi takdirde yıllık deneyimi gerekir sorunları kolayca tanılayıp deneyimsiz teknisyenlerin yardımcı olur.

## <a name="training-validation-and-testing-methods-in-predictive-maintenance"></a>Eğitim, doğrulama ve Tahmine dayalı bakım test yöntemleri
Tahmine dayalı bakım, zaman damgalı verileri içeren diğer tüm çözüm alanı için benzer olarak görünmeyen gelecekteki verileri daha iyi zaman çeşitli yönlerini tipik eğitim ve rutin dikkate alması gerekiyor sınama genelleştirin.

### <a name="cross-validation"></a>Çapraz doğrulama
Çok sayıda makine öğrenimi algoritmalarını modeli performansını önemli ölçüde değiştirebilirsiniz hyperparameters sayısına bağlıdır. Bu hyperparameters en iyi değerlerini otomatik olarak modeli eğitimindeki hesaplanır değil veri Bilimcisi tarafından belirtilmesi gerekir, ancak. Hyperparameters iyi değerlerini bulma birkaç yolu vardır. En yaygın "k-Katlama çapraz hangi örnekler rastgele"k"Katlama böler doğrulama" olur. Her hyperparameters değer kümesi öğrenme algoritmasını çalışma k katıdır. Her yinelemesinde geçerli Katlama örneklerde doğrulama kümesi olarak kullanılan, örnekler geri kalanı, eğitim kümesi olarak kullanılır. Algoritma trenler eğitim örnekleri ve performans ölçümleri üzerinde doğrulama örnekler hesaplanır. Bu döngü her hyperparameter değer kümesi için sonunda k performans ölçümü değerlerinin ortalamasını işlem ve en iyi ortalama performans sahip hyperparameter değerleri seçin.

Tahmine dayalı bakım sorunları, daha önce belirtildiği gibi verileri çeşitli veri kaynaklarından gelen olayların zaman serisi olarak kaydedilir. Bu kayıtları bir kayıt veya bir örnek etiketleme zaman göre sıralanabilir. Hangi veri kümesi rasgele olarak eğitim ve doğrulama kümesi olarak bölme biz, bu nedenle, bazı eğitim örnekler zamanında doğrulama örnekler bazıları sonraki demektir. Bu model eğitilmiş önce gelen verileri temel alan hyperparameter değerlerin gelecekte performans tahmininde sonuçlanır. Bu tahminler özellikle zaman serisi sabit değildir ve zaman içinde davranışlarını değiştirme aşırı iyimser olabilir. Sonuç olarak, seçilen hyperparameter değerleri iyinin olabilir.

Hyperparameters iyi değerlerini bulma bir daha iyi şekilde tüm doğrulama örnekler tüm eğitim örnekleri zamanında sonraki örnekler eğitim ve bir zamana bağımlı şekilde ayarlanması doğrulama Bölünecek yoludur. Daha sonra her hyperparameters değer kümesi için Eğitim kümesi üzerinden algoritma eğitmek, aynı doğrulama küme üzerinde modelinin performansını ölçmek ve en iyi performansı Göster hyperparameter değerleri seçin. Zaman zaman serisi veri sabit değildir ve zaman içinde daha iyi gelecekteki "modelinin daha performans rastgele çapraz doğrulama tarafından seçilen değerlerle. müşteri adayına bölme tren/doğrulama tarafından seçilen hyperparameter değerleri dönüşmesi

Son model, eğitim/doğrulama Böl veya çapraz doğrulama kullanarak bulunan en iyi hyperparameter değerleri kullanarak tüm veriler üzerinde bir öğrenme algoritması eğitim tarafından oluşturulur.

### <a name="testing-for-model-performance"></a>Model performansını test etme
Bir model oluşturduktan sonra size gelecekteki performansını yeni verilerin tahmin etmeniz gerekir. En basit tahmin modeli performansını eğitim verileri içinde olabilir. Ancak performans tahmin etmek için kullanılan veri modeli uyarlanmış çünkü bu tahmin aşırı iyimser. Daha iyi tahmini performans ölçüm hyperparameter değerlerin doğrulama küme üzerinde hesaplanan veya bir ortalama performans ölçümü çapraz doğrulama hesaplanan olabilir. Ancak bu tahminler daha önce belirtildiği gibi aynı nedenlerle hala aşırı iyimser. Model performansını ölçmek için daha gerçekçi yaklaşımlar ihtiyacımız var.

Verileri eğitim, doğrulama ve test ayarlar rastgele bölünmüş bir yoludur. Eğitim ve doğrulama kümeleri hyperparameters değerini seçin ve onlarla modeli eğitmek için kullanılır. Model performansını test küme üzerinde ölçülür.

Tahmine dayalı bakım için uygun olan başka bir şekilde olduğunu örnekler eğitim, doğrulama ve sınama gruplarını bir zamana bağımlı şekilde bölmek için tüm test örnekleri tüm eğitim ve doğrulama örnekler zamanında sonraki gibi. Bölünmüş sonra model oluşturma ve performans ölçüm gerçekleştirilir aynı daha önce açıklandığı gibi.

Zaman serisi tahmin etmek kolay ve sabit olduğunda her iki yaklaşımın gelecekteki performansını benzer tahminler oluşturur. Ancak zaman serisi ileti örneği olmayan ve/veya tahmin etmek sabit olduğunda ikinci yaklaşımı daha gerçekçi gelecekte performans tahminleri ilk olandan oluşturur.

### <a name="time-dependent-split"></a>Zamana bağlı Böl
En iyi uygulama, bu bölümde biz yakın nasıl zamana bağımlı bölünmüş uygulanacağını göz atın. Biz zamana bağımlı iki yönlü bölme eğitim ve sınama gruplarını arasındaki ancak tam olarak açıklayan zamana bağımlı için eğitim ve doğrulama kümesi bölmek için aynı mantığı uygulanmalıdır.

Bir akış gibi çeşitli algılayıcılar ölçümler zaman damgalı olayların sahibiz varsayalım. Eğitim ve test örnekleri yanı sıra, bunların etiketlerini özelliklerini birden çok olay içeren üretildikten tanımlanır.
Örneğin, ikili sınıflandırma özelliği mühendislik ve modelleme teknikleri bölümlerinde açıklandığı gibi özellikleri son olaylarına temel alınarak oluşturulur ve etiketleri içindeki gelecekteki olayları "X" gelecekte zaman birimleri temel alınarak oluşturulur. Bu nedenle, bir örneğin etiketleme zaman çerçevesini daha sonra sonra özelliklerinin bir zaman çerçevesi gelir. Zamana bağlı bölme için hangi biz işaret eden en çok geçmiş verileri kullanarak bizi hyperparameters olan bir modeli eğitmek zaman içinde bir nokta seçin. Eğitim verileri içinde eğitim sonlandırma ötesinde gelecekteki etiketler sızıntısını önlemek için etiketi eğitim örnekleri X olması için en son zaman çerçevesini seçeneğini belirledik birimleri eğitim sonlandırma tarihinden önce. Şekil 7'de bir satır için etiketleri ve özellikleri yukarıda açıklanan yönteme göre hesaplanır son özellik veri kümesindeki her dolu daire temsil eder. Şekil, verilen, eğitim ve ayarlar zamana bağımlı bölünmüş X = 2 ve W = 3 için uygularken test gitmesi kayıtları gösterilmektedir:

![Şekil 7. Zamana bağlı için ikili sınıflandırma bölme](./media/cortana-analytics-playbook-predictive-maintenance/time-dependent-split-for-binary-classification.png)

Şekil 7. Zamana bağlı için ikili sınıflandırma bölme

Yeşil kareler için eğitim kullanılabilmesi için zaman birimlerini ait kayıtları temsil eder. Daha önce açıklandığı gibi her son özellik tablosu eğitim örnekte bakarak oluşturulan özellik oluşturma ve eğitim gün sonlandırma önce etiketleme 2 gelecekteki dönem için 3 nokta geçti. Biz örnekler kümesini biz eğitim sonlandırma ötesinde görünürlük olmayan varsayıyoruz eğitim sonlandırma 2 gelecekteki dönem, örneğin, herhangi bir parçası olduğu zaman eğitim kullanmayın. Bu sınırlama nedeniyle siyah örnekleri eğitim veri kümesinde kullanılmamalıdır son etiketli veri kümesini kayıtları temsil eder. Bu kayıtları eğitim sonlandırma ve bunların etiketleme önce olduğundan veri ya da test kullanılmayacak üretildikten kısmen bağlı eğitim ve etiket bilgi sızıntısını önlemek için test etme için etiketleme üretildikten tamamen ayrı isteriz gibi durum olmamalıdır eğitim zaman çerçevesi.

Bu teknik eğitim ve eğitim sonlandırma yakın olan örnekler test arasında özellik oluşturma için kullanılan veri örtüşme sağlar. Veri kullanılabilirliğine bağlı olarak daha ciddi bir ayırma herhangi birini W zaman eğitim sonlandırma içinde birimleridir sınama kümesi örneklerde kullanarak değildir gerçekleştirilebilir.

Bizim işten kalan kullanım ömrü etmede kullanılan regresyon modeli daha ciddi bir şekilde sızıntısını sorundan etkilenen ve rastgele bir bölme kullanan aşırı overfitting müşteri adayları bulduk. Benzer şekilde, varlıklar kesilmiş eğitim önce hatalarıyla ait kayıtları Eğitim kümesi ve hataları sonlandırma kümesi test etmek için kullanılması gereken sonra olan varlıklar için kullanılması gereken şekilde regresyon sorunlarını bölme olmalıdır.

Genel bir yöntem olarak başka bir önemli en iyi bölme verileri eğitim ve test etme için varlık Kimliğine göre bölme kullanabilir, böylece eğitim kullanılan varlıkları hiçbiri yeni bir varlık üzerinde tahminlerde için kullanıldığında, model gerçekçi sonuçları sağladığından emin olmak için test fikir olduğundan test etmek için kullanılan uygulamadır.

### <a name="handling-imbalanced-data"></a>İmbalanced verileri işleme
' Den bir sınıfın diğer, daha fazla örnek varsa sınıflandırma sorunlarını veri imbalanced olarak kabul edilir. İdeal olarak, her sınıfın yeterli temsilcileri farklı sınıflar arasında ayırt edebilmek için eğitim verilerini sağlamak isteriz. Bir sınıf verilerin % 10'dan az ise, biz verilerin imbalanced ve underrepresented dataset azınlık sınıfı diyoruz söyleyebilirsiniz. Büyük ölçüde, çoğu durumda biz yalnızca veri noktalarının %0,001 oluşturan tarafından başkalarına örneğin karşılaştırıldığında burada bir sınıf ciddi bir şekilde underrepresented imbalanced veri kümeleri bulun. Sınıf dengesizliği hataları genellikle nadir oluşum azınlık sınıfı örnekleri oluşturan ve varlıkları ömrü içinde nerede sahtekarlık algılama, ağ yetkisiz erişim ve Tahmine dayalı bakım gibi birden çok etki alanı içinde bir sorundur.

Genel hata oranı en aza hedefleyin gibi sınıfı dengesizliği durumunda çoğu standart learning algoritmaları performansını tehlikeye. Örneğin, % 99 negatif sınıfı örnekleri ve %1 pozitif sınıfı örnekleri ile bir veri kümesi için yalnızca negatif olarak tüm örneklerini etiketleme tarafından biz % 99 doğruluğu alabilirsiniz. Ancak, doğruluk ölçüm çok yüksek olmasına rağmen algoritma yararlı bir nedenle bunu tüm olumlu örnekler misclassifies. Sonuç olarak, hata oranı üzerinde genel doğruluğu gibi geleneksel değerlendirme ölçümleri olmayan durumunda imbalanced öğrenme yeterli. Duyarlık, geri çağırma, F1 puanlarını ve ayarlanmış ROC Eğriler değerlendirme ölçümleri bölümünde anlatılan imbalanced veri kümeleri halinde değerlendirmeleri için kullanılan maliyet gibi diğer ölçümleri.

Ancak, sınıf düzeltmek Yardım bazı yöntemler vardır dengesizliği sorun. İki önemli olanları teknikleri örnekleme ve hassas öğrenme maliyeti.

#### <a name="sampling-methods"></a>Örnekleme yöntemleri
İmbalanced öğrenme yöntemlerinde örnekleme kullanımını dataset değiştirilmesini dengeli bir veri kümesi sağlamak için bazı mekanizmaları tarafından oluşur. Çok sayıda farklı örnekleme teknikleri olsa da, çoğu düz iletme rastgele olanlardır oversampling ve altında örnekleme.

Yalnızca belirtilen, rastgele oversampling rasgele bir örnek azınlık sınıfından Bu örnekler çoğaltmak ve eğitim veri kümesine ekleme seçmektir. Bu toplam örneklerde azınlık sınıfı sayısını artırır ve sonunda farklı sınıfların örnekleri sayısı dengeleyin. Bir tehlike oversampling, belirli örnekleri birden çok örneğini sınıflandırıcı overfitting için önde gelen çok belirli hale gelmesine neden olabilir ' dir.
Bu yüksek eğitim doğruluk neden olur, ancak performans verileri test etme görünmeyen üzerinde çok düşük olabilir. Buna karşılık, rastgele örnekleme altında rasgele bir örnek çoğunluğu sınıfı ve bu örnekleri eğitim veri kümesinden kaldırma seçmektir. Ancak, çoğu sınıfından örnekler kaldırma çoğunluğu sınıfına ilgili önemli kavramları kurtulması sınıflandırıcı neden olabilir. Burada azınlık sınıfı fazla örneklenen ve çoğu sınıf altında aynı anda örneklenen karma örnekleme başka bir uygun bir yaklaşımdır. Varsa diğer birçok daha karmaşık örnekleme teknikler kullanılabilir ve etkili örnekleme yöntemleri sınıfı dengesizliği için sabit dikkat ve katkı pek çok kanaldan alma bir popüler araştırma alanı. Üzerinde en etkili olanları karar vermek için farklı teknikleri kullanımı genellikle araştırın ve denemek için veri Bilimcisi bırakılır ve veri özellikleri yüksek oranda bağımlı. Ayrıca, örnekleme yöntemleri yalnızca uygulandığından emin emin olmak için Eğitim kümesi ancak test ayarlamak önemlidir.

#### <a name="cost-sensitive-learning"></a>Hassas öğrenme maliyet
Tahmine dayalı bakım azınlık sınıfı oluşturan hataları normal örnekler'den daha fazla ilgi ve böylece odağı üzerinde hatalarda algoritması performansını genellikle odak noktasıdır. Bu genellikle eşit olmayan kaybı veya öğeleri; burada görüntülerle hatalı pozitif negatif olarak tahmin maliyet farklı sınıfların misclassifying asimetrik maliyetlerini denir birden fazla tersi. İstenen sınıflandırıcı çoğunluğu sınıfı doğruluğu üzerinde ciddi bir şekilde ödün vermeden azınlık sınıfı yüksek tahmin doğruluğunu vermek olmalıdır.

Bu elde birkaç yolu vardır. Eşit olmayan sorun kaybeder azınlık sınıfı misclassification için yüksek Maliyet atama ve genel maliyeti en aza indirmek çalışırken etkili bir şekilde ile ele alınabilir. Pozitif ve negatif örnekler maliyetini eğitim süre boyunca burada birleştirilebilir bu fikir kendiliğinden SVMs (Destek vektör makineler) gibi bazı machine learning algoritmaları kullanın. Benzer şekilde, artırma yöntemleri kullanılır ve genellikle ağaç algoritmalar gibi boosted imbalanced veri karar durumunda iyi bir performans gösterir.

## <a name="evaluation-metrics"></a>Değerlendirme ölçümleri
Daha önce belirtildiği gibi sınıfı dengesizliği algoritmaları çoğunluğu sınıfı örnekleri çoğu sınıf doğru etiketli toplam misclassification hata çok artırıldı gibi daha iyi azınlık sınıf örneklerinin gider sınıflandırmak eğilimindedir gibi düşük performans neden olur. Bu, düşük geri çağırma oranları neden olur ve yanlış uyarılar iş maliyetini çok yüksek olduğunda daha büyük bir sorun haline gelir. Doğruluk sınıflandırıcı 's performans tanımlamak için kullanılan en popüler ölçümüdür. Yukarıda açıklandığı şekilde doğruluğu etkisiz ancak ve veri dağıtımları için çok önemli olduğu gibi sınıflandırıcı 's işlevlerin gerçek performans yansıtmaz. Bunun yerine, diğer değerlendirme ölçümleri imbalanced öğrenme sorunları değerlendirmek için kullanılır. Bu durumda, kesinlik, geri çağırma ve F1 puanları Tahmine dayalı bakım modeli performansını değerlendirirken bakmak için başlangıç ölçümlerini olması gerekir. Tahmine dayalı bakım hataları için test kümesi içinde kaç doğru model tarafından tanımlanmış geri çağırma hızlarını gösterir. Yüksek geri çağırma hızları model doğru hatalarını yakalama başarılı olduğu anlamına gelir. Duyarlık ölçüm false nerede yüksek yanlış alarmlar düşük duyarlılık ücretlerin karşılık gelen uyarılar oranı ile ilgilidir. F1 puan hem duyarlık hem de geri çağırma oranları 1 olan ve en kötü 0 olan en iyi değerle göz önünde bulundurur.

Ayrıca, ikili Sınıflandırma, decile tabloları ve yükseltme grafikleri performansını değerlendirirken çok bilgilendirici içindir. Bunlar yalnızca pozitif sınıfında (hata) odaklanmanıza ve ne ROC (alıcı işletim karakteristiğini) eğri yalnızca sabit işletim bir noktada bakarak görülür değerinden daha karmaşık bir resim algoritması performans sağlar.
Decile tabloları, test örnekleri kendi tahmin edilen olasılıklar son etikette karar vermek için hatalar eşik önce modeli tarafından hesaplanan göre sıralama tarafından elde edilir. Sıralı örnekleri deciles (yani % 10 örnekleri ile büyük olasılık ve sonra % 20, % 30 vb.) sonra gruplandırılmıştır. Her decile true pozitif oranını ve rastgele taban (yani 0.1, 0.2..) arasındaki oran bilgi işlem tarafından bir algoritma performans her decile nasıl değiştiğini tahmin edebilirsiniz. Yükseltme grafikler decile true pozitif oranı tüm deciles için rastgele true pozitif oranı çiftleri karşı Çizdirmek tarafından decile değerleri çizmek için kullanılır. Genellikle, burada en büyük kazançlar gösteriliyor bu yana ilk deciles sonuçları odak ' dir. İlk deciles, Tahmine dayalı bakım için kullanıldığında temsili için "riskli" olarak da görülebilir.

## <a name="sample-solution-architecture"></a>Örnek çözüm mimarisi
Tahmine dayalı bakım çözümünü dağıtırken, sürekli bir veri alımı, veri depolama döngüsünü model eğitim, özellik oluşturma, tahmin ve bir uyarı mekanizması Pano izleme bir varlık gibi oluşturan birlikte sonuçlarını görselleştirme için sağlar bir uçtan uca çözüm ilginizi duyuyoruz. Kullanıcıya bir sürekli otomatik şekilde gelecekteki Öngörüler sağlayan veri ardışık istiyoruz. Bu tür bir IOT veri ardışık düzeni için bir örnek Tahmine dayalı bakım mimari Şekil 8'de gösterilmiştir. Mimarisinde, gerçek zamanlı telemetri akış verilerini depolayan bir Event Hub toplanır. Bu veriler, gerçek zamanlı işleme özelliğini oluşturma gibi veri akış analizi tarafından alınan. Özellikleri Tahmine dayalı bir model web hizmetini çağırmak için kullanılır ve sonuçları panosunda görüntülenir. Aynı anda alınan veri ayrıca geçmiş bir veritabanında depolanır ve gibi şirket içi veri modelleme için eğitim örnekleri oluşturmak için taban dış veri kaynakları ile birleştirilir.
Aynı veri ambarları, toplu işlem örnekleri Puanlama ve yeniden Panoda Tahmine dayalı raporlar sağlamak için kullanılabilecek sonuçlarının depolamak için kullanılabilir.

![Şekil 8'de. Tahmine dayalı bakım için örnek çözümü mimarisi](./media/cortana-analytics-playbook-predictive-maintenance/example-solution-architecture-for-predictive-maintenance.png)

Şekil 8'de. Tahmine dayalı bakım için örnek çözümü mimarisi

Daha fazla bilgi mimarisi için bileşenlerinden her biri hakkında bkz [Azure](https://azure.microsoft.com/) belgeleri.

