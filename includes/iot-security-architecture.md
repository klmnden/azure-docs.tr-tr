---
title: include dosyası
description: include dosyası
services: iot-fundamentals
author: robinsh
ms.service: iot-fundamentals
ms.topic: include
ms.date: 08/07/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: f3e05f213821b053f8cf6abbbc50a14e9ea62295
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67189029"
---
# <a name="internet-of-things-iot-security-architecture"></a>Nesnelerin interneti (IOT) güvenlik mimarisi

Sistem tasarlanırken bu sistemde olası tehditleri anlamak ve sistem tasarlanmış ve desteklemesi için uygun savunmaları buna göre eklemek önemlidir. Güvenlikten ödün anlama nasıl bir saldırganın bir sistemden olabilir emin uygun Azaltıcı olmasına yardımcı olur çünkü baştan yerinde başlangıç ürün tasarlamak önemlidir.

## <a name="security-starts-with-a-threat-model"></a>Güvenlik tehdit modeli ile başlar.

Microsoft tehdit modelleri için ürünlerinden uzun kullandı ve şirketin tehdit modelleme işlemi genel kullanıma sunmuştur. Şirket deneyimi modelleme olduğunu gösteren tehditleri en nelerdir hemen anlamak ötesinde beklenmeyen avantajları ile ilgili. Örneğin, ayrıca açık bir tartışma için duyurmanın bir yolu başkalarıyla yeni fikirler ve ürün geliştirmeleri açabilir geliştirme ekibi dışında oluşturur.

Tehdit modelleme amacı, bir saldırganın nasıl bir sistemden ve uygun bir risk azaltma işlemleri yerinde olduğundan emin olabilir öğrenmektir. Tehdit modelleme zorlar Tasarım ekibi azaltmaları sistem tasarlandığı gibi yerine bir sistem sonra dikkate alınması gereken dağıtılır. Alanındaki cihazlar'ın için güvenlik savunmaları retrofitting olanaksız olduğundan, bu çok önemli bir gerçeğidir hata yapmaya açık ve bırakır müşterilerine risk.

Birçok geliştirme ekipleri, müşterilerimiz sistem işlevsel gereksinimlerini yakalama mükemmel bir yarayacaktır. Ancak, belirgin olmayan yollar birisi sistemin kötüye tanımlayan daha zor olur. Tehdit modelleme, geliştirme takımları bir saldırgan ne anlamanıza yardımcı olabilir ve neden. Tehdit modelleme, süreç boyunca bu etkisi güvenlik yapılan tasarım değişiklikleri yanı sıra güvenlik hakkında bir tartışma tasarım kararları sistemde oluşturur. yapılandırılmış bir işlemdir. Bir tehdit modeli basitçe bir belge olsa da, bu belge de bilgi, bekletme derslerde sürekliliği hakkında bilgi edindiniz ve yeni Yardım yerleşik hızlı bir şekilde ekip emin olmak için ideal bir yöntem temsil eder. Son olarak, tehdit modelleme sonucu, güvenlik, müşterilerinize sağlamak istiyorsanız hangi güvenlik taahhütlerine gibi diğer yönlerini dikkate alınması gereken etkinleştirmektir. Bu taahhüt tehdit modelleme ile birlikte bildirin ve nesnelerin interneti (IOT) çözümünüzün test öncülük edin.

### <a name="when-to-do-threat-modeling"></a>Ne zaman iş parçacığı modelleme

[Tehdit modelleme](https://www.microsoft.com/en-us/sdl/adopt/threatmodeling.aspx) tasarım aşaması bünyesine aldığınızda en büyük değer sunar. Tasarlarken, tehditleri ortadan kaldırmak için değişiklik yapmak için en fazla esnekliğine sahipsiniz. Tasarım gereği tehditleri ortadan istenen sonuç elde edilir. Risk azaltma işlemleri ekleme, bunları test etmeye ve geçerli kalırlar ve ayrıca, bu tür eleme her zaman mümkün değildir daha çok daha kolay olur. Bir ürün daha olgun haline gelir ve daha fazla iş ve geliştirme erkenden modelleme tehdit daha çok daha zor ödünler sırayla sonuçta gerektirir tehditleri ortadan kaldırmak daha zor hale gelir.

### <a name="what-to-consider-for-threat-modeling"></a>Tehdit modelleme için göz önüne alınması gerekenler

Çözüm tam ve ayrıca aşağıdaki alanlara odaklanacak olarak gözden geçirmeniz:

* Güvenlik ve gizlilik özellikleri
* Güvenlik ilgili olan hataları özellikleri
* Güven sınırının touch özellikleri

### <a name="who-performs-threat-modeling"></a>Kimin tehdit modelleme gerçekleştirir

Tehdit modelleme gibi başka bir işlemdir. Tehdit modeli belgenin herhangi bir çözüm bileşeninin gibi ele almanız ve bunu doğrulamak için iyi bir fikirdir. Birçok geliştirme ekipleri, müşterilerimiz sistem işlevsel gereksinimlerini yakalama mükemmel bir yarayacaktır. Ancak, belirgin olmayan yollar birisi sistemin kötüye tanımlayan daha zor olur. Tehdit modelleme, geliştirme takımları bir saldırgan ne anlamanıza yardımcı olabilir ve neden.

### <a name="how-to-perform-threat-modeling"></a>Tehdit modelleme gerçekleştirme

Tehdit modelleme işlemi dört adımdan oluşur; adımlar şunlardır:

* Uygulama modeli
* Tehditleri listeleme
* Tehditleri azaltın
* Risk azaltma işlemleri doğrula

#### <a name="the-process-steps"></a>İşlem adımları

Üç kuralları bir tehdit modeli oluşturulurken göz önünde bulundurmanız karşısında:

1. Başvuru mimarisi dışında bir diyagramı oluşturun.

2. Avantajlarına ilk başlatın. Genel bir bakış edinin ve ayrıntılı girmeden önce bir bütün olarak sistemin anlayın. Bu yaklaşım sağlamaya yardımcı olur. Bu, yakından Bakış bölümünde doğru yerlerde.

3. İşlem sürücü, sürücüsü işlem izin vermeyin. Bir sorun model oluşturma aşamasında bulun ve araştırabilirsiniz istiyorsanız için gidin! Bu adımları slavishly gerektiğini gerektiği hissine kapılmayın.

#### <a name="threats"></a>Tehditler

Bir tehdit modeli dört temel öğeleri şunlardır:

* Web Hizmetleri, Win32 hizmetleri gibi işlemler ve * nix Daemon'ları. Bir işlem teknik ayrıntıya gitme, bu alanlarda mümkün olmadığından bazı karmaşık varlıkları (örneğin, alan ağ geçitleri ve sensörlerden) soyutlanır.

* Verileri (herhangi bir yapılandırma dosyası veya veritabanı gibi veriler depolanır) depolar.

* Veri akışı (veri uygulamadaki diğer öğeler arasındaki geçtiği)

* Dış varlıklar (sistemiyle etkileşimli herhangi bir şey ancak uygulama denetimi altında örnekler kullanıcıları eklemek ve uydu akışları)

Mimari diyagramdaki tüm öğeleri, çeşitli tehditleri tabi olan; Bu makalede STRIDE anımsatıcı. Okuma [yeniden tehdit modelleme, STRIDE](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) STRIDE öğeler hakkında daha fazla bilgi edinmek için.

Uygulama diyagramı listesinin farklı öğelerini tabi belirli STRIDE tehditleri şunlardır:

* STRIDE tabi işlemlerdir
* Bir veri akışı TID tabi olan
* Veri depolarını günlük dosyalarını olduğunda veri depoları TID ve R, bazen uygulanır.
* Dış varlıklar SRD tabi olan

## <a name="security-in-iot"></a>IOT güvenliği

Çok sayıda olası etkileşim yüzey alanlarını ve etkileşim desenleri, her biri, bu cihazlara dijital erişim güvenliğini sağlamak için bir çerçeve sunar alınmalıdır bağlı özel amaçlı cihazlar vardır. "Dijital access" terimini buraya doğrudan cihaz etkileşimini gerçekleştirilen tüm işlemler ayırmak için erişim güvenliği fiziksel erişim denetimi sağlanan nerede kullanılır. Örneğin, cihaz kilitli bir oda içine kapıyı yerleştiriliyor. Yazılım ve donanım kullanarak fiziksel erişim reddedildi olamaz, ancak fiziksel erişim gelen lider sistem kesintiye önlemek için ölçüler alınabilir.

Etkileşim desenleri keşfetmenizde "cihaz denetimi" ve "cihaz verileri" ile aynı düzeyde dikkat bakın. "Cihaz control" bir cihaz için değiştirme veya durumuna veya ortamın durumunu doğru davranışını etkileyen amacı ile herhangi bir şirket tarafından sağlanan herhangi bir bilgi olarak sınıflandırılabilir. "Cihaz verileri" durumuna ve ortam gözlemlenen durumu hakkında herhangi bir taraf için bir cihaz yayan herhangi bir bilgi olarak sınıflandırılabilir.

En iyi güvenlik uygulamaları en iyi duruma getirmek için tipik bir IOT mimarisi alıştırma modelleme tehdit bir parçası olarak birkaç bileşeni/alanına ayrılır önerilir. Bu bölgeler, tam olarak bu bölümde açıklanan ve şunları içerir:

* Cihaz,
* Alan ağ geçidi,
* Ağ geçidi, Bulut ve
* Hizmetler.

Bölgeler, geniş kapsamlı bir çözüm yolu segmentlere ayırmak için; her bölge, genellikle kendi veri ve kimlik doğrulaması ve yetkilendirme gereksinimlerine sahiptir. Bölgeler, ayrıca yalıtım zarar kullanılması ve düşük güven bölgeleri daha yüksek güven bölgelerinde etkisini sınırlamak.

Her bölge bir güven, aşağıdaki diyagramda kırmızı noktalı çizgi olarak belirtilen sınırı ile ayrılır. Bu geçiş veri/bilgilerinin bir kaynaktan diğerine temsil eder. Bu geçiş sırasında veri/bilgilerini sahtekarlık, kurcalama, Red, bilgilerin açıklanması, hizmet reddi ve ayrıcalık yükseltme (STRIDE) tabi olabilir.

![IOT güvenlik bölgeleri](media/iot-security-architecture/iot-security-architecture-fig1.png) 

Her bir sınır içinde gösterilen bileşenleri de tam 360 etkinleştirme ADIMI için tabi tehdit modelleme çözüm görünümü. Aşağıdaki bölümlerde ayrıntılı olarak incelemeniz her bileşenleri ve belirli güvenlik konuları yere yerleştirmeniz gereken çözümleri.

Aşağıdaki bölümlerde, genellikle bu bölgelerde bulunan standart bileşenler açıklanmaktadır.

### <a name="the-device-zone"></a>Cihaz bölge

Cihaz hemen gruplar fiziksel alan cihaz geçici fiziksel burada ortamıdır erişim ve/veya "yerel ağ" eşler arası sayısal erişim cihaza uygulanabilir. "Yerel ağ" ayrı ve yalıtılmış – ancak potansiyel olarak genel Internet'e – bağlantı ve cihazların eşler arası iletişime izin verir herhangi mesafeli Kablosuz radyo teknolojisi içeren bir ağ varsayılır. Mevcut *değil* atmosferini yerel bir ağ oluşturma herhangi bir ağ sanallaştırma teknolojisini içerir ve, ortak ağ alanı arasında iletişim kurmak için herhangi iki cihazların genel işleç ağlar da içermez eşler arası iletişim ilişkisi girmek için bırakıldı.

### <a name="the-field-gateway-zone"></a>Alan ağ geçidi bölgesi

Alan ağ geçidi, bir cihaz/gereç veya iletişim etkinleştiricisi olarak ve da potansiyel olarak, bir cihaz denetim sistemi ve veri işleme hub'ı cihaz gibi davranan bazı genel amaçlı sunucu bilgisayar yazılımları ' dir. Alan ağ geçidi bölgesi alan ağ geçidi kendisi ve bağlı tüm cihazları içerir. Adından da anlaşılacağı gibi alan ağ geçitleri dışında özel veri işleme özellikleri hareket, genellikle konum bağlı olan, olası fiziksel yetkisiz giriş tabi olan ve işletimsel yedeklilik sınırlıdır. Tüm bir alan ağ geçidi genellikle bir şey olduğunu söylemek bir dokunma ve onun işlevini nedir bilerek sabotage.

Bir alan ağ geçidi, erişim yönetimi etkin bir rol oluşturdu ve bilgi akışını, bir uygulama olduğu anlamına varlık ve ağ bağlantınızı veya terminal oturumu ele boyutundaydı trafiği yönlendiriciden farklıdır. Olmayan açık bir bağlantı veya oturumu terminaller, ancak bunun yerine bir yol (veya blok) bağlantıları olduğu bir NAT cihazı veya güvenlik duvarı, buna karşılık, alan ağ geçitleri uygun veya oturumları aracılığıyla yapılır. Alan ağ geçidi, iki farklı yüzey alanlara sahiptir. Biri olarak bağlanmış cihazları yüzleri ve bölgenin iç temsil eder ve diğer tüm dış tarafların yüzleri ve bölgenin Edge.

### <a name="the-cloud-gateway-zone"></a>Bulut ağ geçidi bölgesi

Bulut ağ geçidi arasında ortak ağ alanı, genellikle bir bulut tabanlı denetim ve veri analizi sistem, bu tür sistemleri Federasyonu doğrultusunda gelen ve cihazlar veya alan ağ geçitleri birkaç farklı sitelerde uzaktan iletişimi sağlayan bir sistemdir. Bazı durumlarda, bir bulut ağ geçidi hemen erişim için özel amaçlı cihazlar Tablet veya telefon gibi terminaller gelen kolaylaştırabilir. Burada tartışılan bağlamında, ekli cihazları veya alan ağ geçitleri aynı sitede bağlı olmayan bir özel veri işleme sistemine başvurmak için "bulut" yöneliktir. Ayrıca bir bulut bölgesi işletimsel ölçüler hedeflenen fiziksel erişimi engellemek ve mutlaka bir "Genel bulut" altyapısı için açık değildir.  

Bir bulut ağ geçidi, bulut ağ geçidi ve tüm bağlı cihazları veya alan ağ geçitleri üzerinden diğer ağ trafiğinden sorunlardan uzak tutmak için bir ağ sanallaştırma katmana içine potansiyel olarak eşlenebilir. Bulut ağ geçidinin kendisi bir cihaz denetim sistemi veya bir işleme veya depolama tesisi cihaz verileri için değildir; Bu bulut ağ geçidi ile arabirim. Bulut ağ geçidi bölgesi bulut ağ geçidinin kendisi tüm alan ağ geçitleri ve doğrudan veya dolaylı olarak kendisine bağlı cihazlarla birlikte içerir. Edge bölgenin, tüm dış tarafların üzerinden iletişim kurduğu ayrı bir yüzey alanıdır.

### <a name="the-services-zone"></a>Hizmetleri bölgesi

Bir "hizmet" herhangi bir yazılım bileşeni veya cihazı olan bir alan veya Bulut ağ geçidi üzerinden veri toplama ve çözümleme için yanı sıra komut ve denetim için arabirim modülü olarak bu bağlam için tanımlanır. Ortam araçları hizmetleridir. Bunlar ağ geçitleri ve diğer alt sistemlerin kimliklerini altında işlem, depolama ve verileri analiz etmek, otonom olarak veri öngörüleri veya zamanlama tabanlı cihazlar için komutları yürütün ve bilgilerin açığa çıkmasına neden ve yetkili son kullanıcılara özellikleri denetlemek.

### <a name="information-devices-versus-special-purpose-devices"></a>Bilgi-cihazlarıyla, özel amaçlı cihazlar

Bilgisayarlar, telefonlar ve tabletler öncelikle etkileşimli bilgi cihazlardır. Pil ömrü en üst düzeye etrafında telefonlar ve tabletler açıkça getirilir. Bunlar tercihen kısmen hemen bir kişi ile etkileşim kurulurken ya da müziği veya belirli bir konuma bunların sahibi kılavuzluk gibi hizmetler bulunmaması kapatın. Sistemleri açısından bakıldığında, bu bilgi teknolojisi cihazlar kişilere yönelik proxy olarak esas olarak davranan. "Eylemler ve"girişinin toplanması kişiler sensörlerden"önerme kişiler çalıştırıcılar" değildirler.

Basit sıcaklık algılayıcıları karmaşık Fabrika üretim hatlarının bileşenlerin bunların içinde binlerce için özel amaçlı cihazlar farklıdır. Bu cihazları daha amacı kapsamlı ve bazı kullanıcı arabirimi sağladıkları bile ile arabirim için büyük ölçüde kapsamlı veya Fiziksel dünyada varlıklar tümleştirilmesini. Bunlar ölçün ve ortam koşullar bildirmek Vana etkinleştirmek, servos denetlemek, alarmlar ses, ışıkları geçiş ve birçok diğer görevleri gerçekleştirmek. Çalışmak için bir bilgi cihaz çok genel, çok pahalı, çok büyük veya çok kırılır yardımcı olurlar. Somut amacı, teknik tasarım, üretim ve zamanlanmış ömrü işlemi için de kullanılabilir parasal bütçe olarak hemen belirler. Bu iki önemli faktör birleşimi kullanılabilir işletimsel enerji bütçe, fiziksel Ayak izi ve böylece kullanılabilir depolama alanı, işlem ve güvenlik özellikleri kısıtlar.

Bir şey varsa "gelecek yanlış" otomatik olarak veya uzaktan denetlenebilir cihazlarla, örneğin, fiziksel hataları veya Denetim mantığı willful yetkisiz giriş ve düzenleme için hataları. Üretim çok yok, binalar looted veya yazılmış ve kişi ve hatta yaralı zar olabilir. Bu kesimin birisi bir çalınan Kredi kartının sınırı maxing daha tamamen farklı bir sınıftır. Güvenlik çubuğunu taşımak şeyler yapmak cihazları için ve sonunda taşımak şeyler neden komutlarında sonuçları algılayıcı verileri için herhangi bir e-ticaret veya banka senaryosu daha yüksek olmalıdır.

### <a name="device-control-and-device-data-interactions"></a>Cihaz ve cihaz verilerini etkileşimleri

Çok sayıda olası etkileşim yüzey alanlarını ve etkileşim desenleri, her biri, bu cihazlara dijital erişim güvenliğini sağlamak için bir çerçeve sunar alınmalıdır bağlı özel amaçlı cihazlar vardır. "Dijital access" terimini buraya doğrudan cihaz etkileşimini gerçekleştirilen tüm işlemler ayırmak için erişim güvenliği fiziksel erişim denetimi sağlanan nerede kullanılır. Örneğin, cihaz kilitli bir oda içine kapıyı yerleştiriliyor. Yazılım ve donanım kullanarak fiziksel erişim reddedildi olamaz, ancak fiziksel erişim gelen lider sistem kesintiye önlemek için ölçüler alınabilir.

Etkileşim desenleri keşfetmenizde "cihaz denetimi" ve "cihaz verileri" ile aynı düzeyde dikkatini tehdit modelleme sırasında bakın. "Cihaz control" bir cihaz için değiştirme veya durumuna veya ortamın durumunu doğru davranışını etkileyen amacı ile herhangi bir şirket tarafından sağlanan herhangi bir bilgi olarak sınıflandırılabilir. "Cihaz verileri" durumuna ve ortam gözlemlenen durumu hakkında herhangi bir taraf için bir cihaz yayan herhangi bir bilgi olarak sınıflandırılabilir.

## <a name="performing-threat-modeling-for-the-azure-iot-reference-architecture"></a>Azure IOT başvuru Mimarisi Modelleme tehdit gerçekleştirme

Microsoft tehdit modelleme için Azure IOT yapmak için daha önce açıklanan framework kullanır. Aşağıdaki bölümde, IOT için modelleme tehdit hakkında düşünmek ve tanımlanan tehditlere göstermek için Azure IOT başvuru mimarisi somut örneği kullanır. Bu örnekte, dört ana alan odak tanımlar:

* Cihazlar ve veri kaynakları
* Veri taşıma
* Cihaz ve olay işleme ve
* Sunum

![Azure IOT modelleme tehdit](media/iot-security-architecture/iot-security-architecture-fig2.png)

Aşağıdaki diyagramda Microsoft tehdit modelleme aracı tarafından kullanılan bir veri akış diyagramı modeli kullanarak Microsoft'un IOT mimarisinin basitleştirilmiş bir görünümünü sağlar:

![Tehdit modelleme MS tehdit modelleme aracı kullanarak Azure IOT için](media/iot-security-architecture/iot-security-architecture-fig3.png)

Mimari cihaz ve ağ geçidi özellikleri ayıran dikkat edin önemlidir. Bu yaklaşım daha güvenli ağ geçidi cihazları yararlanarak sağlar: Bunlar genellikle - bir thermostat gibi-yerel bir cihaz olabilir, daha büyük işlem ek yükü gerektiren güvenli protokolleri kullanılarak bulut ağ geçidi ile iletişim kurabilen kendi kendine sağlar. Bulut ağ geçidi Azure IOT Hub hizmeti tarafından temsil edilen Azure Hizmetleri bölgesinde varsayılır.

### <a name="device-and-data-sourcesdata-transport"></a>Cihaz ve veri kaynakları/veri taşıma

Bu bölümde, tehdit modelleme odağı yukarıda özetlenen mimarisi inceler ve devralınan endişelere yer bırakmadan bazılarını ele almak nasıl bir genel bakış sağlar. Bu örnek, bir tehdit modeli çekirdek öğeleri üzerinde odaklanır:

* İşlemler (hem de dış öğeler ve denetim altında)
* (Bir veri akışı olarak da bilinir) iletişimi
* (Veri depoları olarak da bilinir) depolama

#### <a name="processes"></a>İşlemler

Her Azure IOT mimaride özetlenen kategorileri, bu örnek veri/bilgi bulunmaktadır farklı aşamaları boyunca birkaç farklı tehditleri azaltmak çalışır: işlem, iletişimi ve depolama. En yaygın olanlarından bu tehditleri en iyi şekilde nasıl giderilebilir genel bir bakış tarafından izlenen "işlem" kategorisi için genel bir bakış aşağıdadır:

**(S) yanıltma**: Bir saldırganın ya da yazılım veya donanım düzeyinde ve sonradan gelen anahtar malzemesi cihazın kimliği altında farklı bir fiziksel veya sanal cihaz sistemiyle gerçekleştirilen erişim bir cihaz şifreleme anahtar malzemesi Al. Uzaktan, herhangi bir TV etkinleştirebilir ve popüler prankster araçları olan denetimler buna iyi bir örnektir.

**(D) hizmet reddi**: Radyo frekansı gönderilerek veya kabloları kesilerek cihazların çalışması veya iletişim kurması engellenebilir. Örneğin elektrik veya ağ bağlantısı kesilen bir güvenlik kamerası veri iletemez.

**(T) oynama**: Saldırganlar cihaz üzerinde çalışan yazılımı kısmen veya tamamen değiştirebilir ve değiştirilen yazılım sayesinde cihazın özgün kimliğinden faydalanarak anahtarlardan veya önemli öğelerin yer aldığı şifreleme bilgilerinden faydalanabilir. Örneğin, bir saldırganın kesecek ve iletişim yolunun cihazda verileri gösterme ve çalınan anahtar malzemesi ile kimlik doğrulaması false veri yerine ayıklanan anahtar malzemesi yararlanarak.

**Bilgi İfşası (ı)** : Cihaz yönetilebilen yazılım çalışıyorsa, yönetilebilen yazılımla yetkisiz taraflara veri sızıntı. Örneğin, bir saldırganın, kendisi cihaz denetleyicisi veya alan ağ geçidi veya bilgileri siphon için bulut ağ geçidi arasındaki iletişim yolunun uygulamasına eklemesine ayıklanan anahtar malzemesi yararlanarak.

**(E) ayrıcalık yükseltme**: Belirli bir işlev için tasarlanmış olan bir cihaz başka bir göreve zorlanabilir. Örneğin, yarı yol açmak için programlanmış bir Vana tamamen açmak için sağladı.

| **Bileşen** | **Tehdit** | **Risk azaltma** | **Risk** | **Uygulama** |
| --- | --- | --- | --- | --- |
| Cihaz |S |Cihaza kimlik atama ve cihaz kimlik doğrulaması |Aygıt veya aygıtın başka bir cihazı ile değiştiriliyor. Nasıl doğru cihaza varsayılır biliyor musunuz? |Aktarım Katmanı Güvenliği (TLS) veya IPSec kullanarak cihaz kimlik doğrulaması. Altyapı önceden paylaşılan anahtar (PSK) kullanarak tam asimetrik şifreleme işleyemiyor bu cihazlarda desteklemelidir. Azure AD yararlanın [OAuth](https://www.rfc-editor.org/pdfrfc/rfc6755.txt.pdf) |
|| TRID |Cihaz tamperproof mekanizmalarına, örneğin, sabit için anahtarları ve diğer şifreleme malzemelerini CİHAZDAN ayıklamak mümkün kolaylaştırarak uygulayın. |Birisi (fiziksel girişim) cihaz oynama, riski oluşturur. Nasıl emin, bu cihaz misiniz oynanmış değil. |En etkili azaltma içinden anahtarları okunamaz, ancak yalnızca bu anahtarı kullanırsınız, ancak hiçbir zaman anahtarının ifşa şifreleme işlemleri için kullanılan özel yonga üzerinde devresi anahtarları depolama sağlayan güvenilir platform Modülü (TPM) özelliktir. Cihaz şifreleme bellek. Anahtar Yönetimi için cihaz. Kod imzalama. |
|| E |Cihazın erişim denetimi sahip. Yetkilendirme düzeni. |Cihazı için bireysel işlemlere gerçekleştirilecek bir dış kaynağa ya da güvenliği aşılmış sensörlerden komutları temel alınarak izin veriyorsa, aksi takdirde işlemleri gerçekleştirmek saldırı erişilebilir sağlar. |Cihaz için Yetkilendirme düzeni sahip |
| Alan ağ geçidi |S |Bulut ağ geçidi için bir alan ağ geçidi (PSK, sertifika tabanlı veya talep tabanlı gibi.) kimlik doğrulaması |Ardından, birisi alan ağ geçidi davranabilir, kendisini herhangi bir CİHAZDAN sunabilir. |RSA/PSK TLS, IPSec, [RFC 4279](https://tools.ietf.org/html/rfc4279). Aynı genel – aygıtların temel depolama ve kanıtlama sorunlarının en iyi durum kullanın TPM. Kablosuz algılayıcı ağları (WSN) desteklemek IPSec 6LowPAN uzantısı. |
|| TRID |Alan ağ geçidi (TPM?) izinsiz kullanıma karşı koruyun |Alan ağ geçidi için Bahsediyor bulut ağ geçidi düşünmeye kandırmaya saldırıları yanıltma bilgi ifşasına ve verilerin izinsiz kullanımına neden olabilir |Bellek şifreleme, TPM's, kimlik doğrulaması. |
|| E |Alan ağ geçidi için erişim denetimi mekanizması | | |

Tehditleri bu kategorideki bazı örnekleri aşağıda verilmiştir:

**Kimlik sahtekarlığı**: Bir saldırganın ya da yazılım veya donanım düzeyinde ve sonradan gelen anahtar malzemesi cihazın kimliği altında farklı bir fiziksel veya sanal cihaz sistemiyle gerçekleştirilen erişim bir cihaz şifreleme anahtar malzemesi Al.

**Hizmet reddi**: Radyo frekansı gönderilerek veya kabloları kesilerek cihazların çalışması veya iletişim kurması engellenebilir. Örneğin elektrik veya ağ bağlantısı kesilen bir güvenlik kamerası veri iletemez.

**İzinsiz**: Saldırganlar cihaz üzerinde çalışan yazılımı kısmen veya tamamen değiştirebilir ve değiştirilen yazılım sayesinde cihazın özgün kimliğinden faydalanarak anahtarlardan veya önemli öğelerin yer aldığı şifreleme bilgilerinden faydalanabilir.

**İzinsiz**: Boş bir koridor spektrumun görünen resmi gösteren bir gözetim kamera bu tür bir koridor hello'nun amaçlayan. Duman veya yangın algılayıcı birisi altındaki bir açık tutarak raporlama. Her iki durumda da, cihaz teknik Sistem tamamen güvenilir olabilir, ancak yönetilebilen bilgileri raporlar.

**İzinsiz**: Bir saldırganın kesecek ve iletişim yolunun cihazda verileri gösterme ve çalınan anahtar malzemesi ile kimlik doğrulaması false veri yerine ayıklanan anahtar malzemesi yararlanarak.

**İzinsiz**: Bir saldırganın kısmen veya tamamen cihaz üzerinde çalışan yazılımı potansiyel olarak değiştirilen yazılım anahtar malzemesi veya anahtar malzeme bulunduran şifreleme özellikleri kullanılabilir değilse, cihaz kimliğini kullanmasına izin vererek değiştirebilir gerçekleşiyorsa program.

**Bilgilerin açığa çıkması**: Cihaz yönetilebilen yazılım çalışıyorsa, yönetilebilen yazılımla yetkisiz taraflara veri sızıntı.

**Bilgilerin açığa çıkması**: Bir saldırganın, kendisi cihaz denetleyicisi veya alan ağ geçidi veya bilgileri siphon için bulut ağ geçidi arasındaki iletişim yolunun uygulamasına eklemesine ayıklanan anahtar malzemesi yararlanarak.

**Hizmet reddi**: Cihaz devre dışı bırakılmış veya iletişim (birçok endüstriyel makinelere kasıtlı olmayan) mümkün olduğu bir moduna açık.

**İzinsiz**: Denetim sistemine (dışında bilinen ayar parametreleri) bilinmeyen bir durumda çalışır ve bu nedenle anlaşılabilir veri sağlamak için cihazı yapılandırılması

**Ayrıcalık yükseltme**: Belirli bir işlev için tasarlanmış olan bir cihaz başka bir göreve zorlanabilir. Örneğin, yarı yol açmak için programlanmış bir Vana tamamen açmak için sağladı.

**Hizmet reddi**: Cihaz iletişimi mümkün olduğu bir duruma kapatılabilir.

**İzinsiz**: Cihaz, denetim sistemine (dışında bilinen ayar parametreleri) bilinmeyen bir durumda çalışır ve bu nedenle anlaşılabilir veri sağlamak için yapılandırılabilen.

**Kimlik sahtekarlığı/değişiklik/Red**: Güvenli olmayan (olduğu nadiren durum tüketici uzaktan kumandalar), bir saldırganın bir cihazın durumunu anonim olarak işleyebilirsiniz. Uzaktan, herhangi bir TV etkinleştirebilir ve popüler prankster araçları olan denetimler buna iyi bir örnektir.

#### <a name="communication"></a>İletişim

Cihazlar, cihazlar ve alan ağ geçitleri ve cihaz ve bulut ağ geçidi arasındaki iletişim yolunun etrafında tehditleri. Aşağıdaki tabloda bazı yönergeler açık yuva cihaz/VPN etrafında sahiptir:

| **Bileşen** | **Tehdit** | **Risk azaltma** | **Risk** | **Uygulama** |
| --- | --- | --- | --- | --- |
| Cihaz IOT hub'ı |TID |(D) TLS trafiğini şifrelemek için (PSK/RSA) |Gizlice veya cihaz ve ağ geçidi arasındaki iletişimi engelliyor |Protokol düzeyinde güvenlik. Özel protokoller ile korumak nasıl gerekir. Çoğu durumda, iletişimin (cihaz ve bağlantıyı başlatan) IOT Hub'ına CİHAZDAN yer alır. |
| Cihaz için cihaz |TID |(D) TLS trafiğini şifrelemek için (PSK/RSA). |Cihazlar arasında Taşınmakta olan veriler okunuyor. Verilerinize müdahale. Cihazı yeni bağlantıları ile aşırı yükleme |(MQTT/AMQP/HTTP/CoAP protokol düzeyinde güvenlik. Özel protokoller ile korumak nasıl gerekir. DoS tehdit azaltma, Bulut veya alan ağ geçidi aracılığıyla cihazları eş ve bunları yalnızca istemciler ağ doğru olarak kalanları oluşturmaktır. Ağ Geçidi tarafından aracılı sonra eşler arasında doğrudan bağlantı eşlemesi sonuçlanabilir |
| Dış varlık cihaz |TID |Dış varlık cihaza tanımlayıcı eşleştirme |Cihaz bağlantısı gizlice. Cihazla iletişimi engelliyor |Dış varlık ' % s'cihazına NFC/Bluetooth LE güvenli bir şekilde eşleştirilmiş. Cihaz (fiziksel) işletimsel panelini denetleme |
| Alan ağ geçidi bulut ağ geçidi |TID |TLS trafiğini şifrelemek için (PSK/RSA). |Gizlice veya cihaz ve ağ geçidi arasındaki iletişimi engelliyor |Güvenlik protokol düzeyinde (MQTT/AMQP/HTTP/CoAP). Özel protokoller ile korumak nasıl gerekir. |
| Cihaz bulut ağ geçidi |TID |TLS trafiğini şifrelemek için (PSK/RSA). |Gizlice veya cihaz ve ağ geçidi arasındaki iletişimi engelliyor |Güvenlik protokol düzeyinde (MQTT/AMQP/HTTP/CoAP). Özel protokoller ile korumak nasıl gerekir. |

Tehditleri bu kategorideki bazı örnekleri aşağıda verilmiştir:

**Hizmet reddi**: Bunlar etkin olarak gelen bağlantılara veya bir ağda istenmeyen veri birimi için bir saldırgan paralel olarak birçok Bağlantıları'nı açın ve değil bunları hizmet veya yavaş hizmet veya cihaz olabilir çünkü dinlerken kısıtlanmış cihazlar genellikle DoS tehlike altında olan istekte bulunulmamış trafik ile yayılan. Her iki durumda da, cihazın etkin ağ üzerinde çalışamaz işlenebilir.

**Yanıltma, bilgi İfşası**: Kısıtlanmış cihazları ve özel amaçlı cihazlar genellikle parola veya PIN koruma veya tamamen güvenen bir cihaz aynı ağ ve o ağ üzerinde olduğunda bunlar bilgilere erişim anlamı ağ üzerinde kullanan gibi tüm için bir güvenlik olanakları sahiptir genellikle yalnızca bir paylaşılan anahtar korunuyor. Cihaz veya ağ paylaşılan gizli diziyi bildirilen, bu cihazı denetlemek veya CİHAZDAN yayılan veriler gözlemek mümkün olduğunu anlamına gelir.  

**Kimlik sahtekarlığı**: bir saldırgan kesebilen veya kısmen yayın geçersiz kılmak ve gönderene (ortadaki adam) sızmasını

**İzinsiz**: bir saldırgan kesebilen veya kısmen yayın geçersiz kılmak ve yanlış bilgi gönder 

**Bilgi İfşası:** bir saldırganın yayınlar misafiri ve yetkilendirme olmadan daha fazla bilgi edinmek **hizmet reddi:** bir saldırganın yayın sinyal jam ve bilgi dağıtım Reddet

#### <a name="storage"></a>Depolama

Her cihaz ve alan ağ geçidi (işletim sistemi (OS) görüntüsü depolama, veri queuing geçici) depolama çeşit vardır.

| **Bileşen** | **Tehdit** | **Risk azaltma** | **Risk** | **Uygulama** |
| --- | --- | --- | --- | --- |
| Cihaz depolaması |TRID |Depolama şifrelemesi, günlükleri imzalama |(PII verileri), depolama alanından telemetri verilerinize müdahale veri okunuyor. Değiştirilmesine kuyruğa alınan veya komutu denetim verilerinin önbelleğe alınır. Yapılandırma veya üretici yazılımı güncelleştirme paketleri ile kurcalama önbelleğe ya da yerel olarak kuyruğa işletim sistemi ve/veya sistem bileşenleri güvenliğinin Bozulması neden olabilir |Şifreleme, ileti kimlik doğrulama kodu (MAC) veya dijital imza. Burada kaynak erişimi aracılığıyla olası, güçlü erişim denetim listeleri (ACL) veya izinleri denetleyin. |
| Cihaz işletim sistemi görüntüsü |TRID | |İşletim sistemi ile kurcalama / işletim sistemi bileşenleri değiştirme |İşletim sistemi bölümü, salt okunur imzalı işletim sistemi görüntüsü, şifreleme |
| Alan ağ geçidi depolama (veri fazlasını sıraya almaya) |TRID |Depolama şifrelemesi, günlükleri imzalama |(PII verileri), depolama, telemetri verilerinize müdahale verileri okuma, kuyruğa değiştirilmesine veya komutu denetim verilerinin önbelleğe alınmış. (Cihazları veya alan ağ geçidi için hedefleyen) yapılandırma veya üretici yazılımı güncelleştirme paketleri ile kurcalama önbelleğe ya da yerel olarak kuyruğa işletim sistemi ve/veya sistem bileşenleri güvenliğinin Bozulması neden olabilir |BitLocker |
| Alan ağ geçidi işletim sistemi görüntüsü |TRID | |İşletim sistemi ile kurcalama / işletim sistemi bileşenleri değiştirme |İşletim sistemi bölümü, salt okunur imzalı işletim sistemi görüntüsü, şifreleme |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Cihaz ve olay işleme/bulut ağ geçidi bölgesi

Bir bulut ağ geçidi arasında ortak ağ alanı, genellikle bir bulut tabanlı denetim ve veri analizi sistem, bu tür sistemleri Federasyonu doğrultusunda gelen ve cihazlar veya alan ağ geçitleri birkaç farklı sitelerde uzaktan iletişimi sağlayan sistemidir. Bazı durumlarda, bir bulut ağ geçidi hemen erişim için özel amaçlı cihazlar Tablet veya telefon gibi terminaller gelen kolaylaştırabilir. Ele alınan bağlamında, burada "bulut" değil bağlanan bağlı cihazları veya alan ağ geçitleri aynı sitede ve burada hedeflenen fiziksel erişimi engelle işletimsel ölçüler bir özel veri işleme sistemine başvurmak için tasarlanmıştır ancak mutlaka için değil bir " "Genel bulut altyapısı. Bir bulut ağ geçidi, bulut ağ geçidi ve tüm bağlı cihazları veya alan ağ geçitleri üzerinden diğer ağ trafiğinden sorunlardan uzak tutmak için bir ağ sanallaştırma katmana içine potansiyel olarak eşlenebilir. Bulut ağ geçidinin kendisi bir cihaz denetim sistemi veya bir işleme veya depolama tesisi cihaz verileri için değildir; Bu bulut ağ geçidi ile arabirim. Bulut ağ geçidi bölgesi bulut ağ geçidinin kendisi tüm alan ağ geçitleri ve doğrudan veya dolaylı olarak kendisine bağlı cihazlarla birlikte içerir.

Bulut ağ geçidi genellikle özel oluşturulmuş alan ağ geçidi ve cihazların bağlandığı ortaya çıkarılan uç noktalar ile bir hizmet olarak çalışan bir yazılım parçasıdır. Bu nedenle ile göz önünde bulundurularak tasarlanmalıdır. İzleyin [SDL](https://www.microsoft.com/sdl) tasarlama ve bu hizmeti oluşturmak için işlem.

#### <a name="services-zone"></a>Hizmetleri bölgesi

Bir denetim sistemidir (veya denetleyicisi) bir cihaz veya alan ağ geçidi ya da bir veya birden çok cihaz denetlemek amacıyla ve/veya toplamanıza ve/veya depolamak ve/veya cihaz verilerini sunumu, analiz etmek için bulut Ağ Geçidi Arabirimi bir yazılım çözümü olan veya sonraki denetim amaçlar. Denetim sistemleri, kullanıcılarla etkileşim hemen kolaylaştırabilir bu tartışma kapsamında yalnızca varlıklardır. Cihazlarda, cihazı kapatıp veya diğer özelliklerini değiştirmek bir kişi izin veren bir anahtar gibi ve var olan dijital olarak erişilebilen işlevsel eşdeğeri Ara fiziksel denetimi yüzeyleri durumlardır.

Ara fiziksel denetimi yüzeyleri aittir mantığını yöneten Fiziksel denetimi yüzeyinde işlevi eşdeğer bir işlev Uzaktan yeniden başlatılabilir ya da uzak giriş giriş çakışıyor önlenmiş – olacak şekilde kısıtlar burada gibi intermediated Denetim yüzeyleri cihaz paralel olarak eklenmesi herhangi bir uzaktan denetim sistemi olarak aynı temel işlevsellikten yararlanan bir yerel denetimi sistemine kavramsal olarak eklenir. Bulut bilgi işlem olabilir, okuma için en çok karşılaşılan tehditler [bulut güvenliği İttifakı (CSA)](https://cloudsecurityalliance.org/articles/csa-releases-top-threats-to-cloud-computing-deep-dive/) sayfası.

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [SDL tehdit modelleme aracı](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
* [Microsoft Azure IOT başvuru mimarisi](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)
