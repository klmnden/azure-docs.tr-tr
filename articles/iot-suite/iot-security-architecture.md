---
title: "IOT güvenlik mimarisi | Microsoft Docs"
description: "IOT güvenlik mimarisi yönergeleri ve ilgili önemli noktalar"
services: 
suite: iot-suite
documentationcenter: 
author: YuriDio
manager: timlt
editor: 
ms.assetid: 18ed3eb0-9406-44e1-8a3a-93dc6726c7ac
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: yurid
ms.openlocfilehash: 2482dade7d17d05b2fc90fbf22b0466227a5983b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="internet-of-things-security-architecture"></a>Nesnelerin interneti güvenlik mimarisi
Sistem tasarlanırken, bu sistemde olası tehditler anlamak ve sistem tasarlanmış ve tasarlanmış gibi uygun savunma buna göre eklemek önemlidir. Nasıl bir saldırganın bir sistemden olabilir emin uygun Azaltıcı Etkenler hale getirir anlama olduğundan başlangıçtan itibaren yerinde göz önünde bulundurularak ile başlangıç ürün tasarlamak özellikle önemlidir. 

## <a name="security-starts-with-a-threat-model"></a>Bir tehdit modeli ile güvenlik başlar
Microsoft tehdit modelleri ürünlerinden için uzun kullandı ve herkese açık işlem modelleme şirketin tehdit sunmuştur. Şirket deneyimi modelleme olduğunu gösteren tehditleri en nelerdir hemen anlamak ötesinde beklenmeyen avantajları ilgili. Örneğin, ayrıca bir açık tartışma için bir avenue başkalarıyla yeni fikirleri ve ürün yenilikleri açabilir ve geliştirme ekibi dışında oluşturur.

Tehdit modelleme amacı, nasıl bir saldırganın bir sistemden ve uygun Azaltıcı Etkenler karşılandığından emin olabilir anlamaktır. Tasarım ekibi Azaltıcı Etkenler sistem tasarlandığı gibi yerine bir sistem sonra dikkate alınması gereken tehdit modelleme zorlar dağıtılır. Aygıtları alanındaki çok sayıda güvenlik savunmaları retrofitting uyuşmazlığa, olgunun kritik düzeyde önemli olduğundan hataya ve müşteriler risk bırakır.

Birçok geliştirme ekiplerinin müşteriler yararlanan işlevsel sistemi gereksinimleri yakalama mükemmel iş yapın. Ancak, birisi sistemin kötüye belirgin olmayan yolları tanımlayan daha zordur. Tehdit modelleme geliştirme ekiplerinin bir saldırgan ne anlamanıza yardımcı olabilir ve neden. Tehdit modelleme yol boyunca bu etkisi güvenlik yapılan tasarım değişiklikleri yanı sıra güvenlik hakkında tartışma tasarım kararları sistemde oluşturur, yapılandırılmış bir işlemdir. Yalnızca bir belge bir tehdit modeli olmakla birlikte, bu belge bilgi, dersleri bekletme sürekliliği öğrenilen ve hızlı bir şekilde Yardım yeni yerleşik takım emin olmak için ideal bir yöntem de temsil eder. Son olarak, bir tehdit modelleme sonucunu müşterileriniz için sağlamak istediğiniz hangi güvenlik taahhütleri gibi güvenlik diğer yönlerini dikkate almanız gereken sağlamaktır. Tehdit modelleme ile birlikte bu taahhüt bildirin ve nesnelerin interneti (IOT) çözümünüzün sınama sürücü.

### <a name="when-to-threat-model"></a>Model tehdit zamanı
[Tehdit modelleme](http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx) tasarım aşaması dahil edilmiş, büyük değer sunar. Tasarlarken, tehditleri ortadan kaldırmak için değişiklik yapmak için en büyük esnekliği sahip. Tehditler tasarım gereği ortadan istenen sonuca olur. Azaltıcı Etkenler ekleme, bunları test etme ve geçerli kalır ve ayrıca, bu tür eleme her zaman mümkün değildir sağlayarak daha çok daha kolay olur. Bir ürünün daha da olgun olur ve sırayla sonuçta daha fazla iş ve önceden geliştirme modelleme tehdit daha çok daha zor dengelemeden gerektirir olarak tehditleri ortadan kaldırmak daha zor hale gelir.

### <a name="what-to-threat-model"></a>Tehdit modeli gerekenler
İş parçacığı modeli bir bütün olarak çözüm ve ayrıca aşağıdaki alanlarda odaklanın:

* Güvenlik ve gizlilik özellikleri
* Hataları ilgili güvenlik özellikleri
* Güven sınırının touch özellikleri 

### <a name="who-threat-models"></a>Kimin modelleri tehdit
Tehdit modelleme gibi başka bir işlemdir.  Tehdit modeli belgenin çözümün herhangi bir bileşeni gibi ele alın ve bunu doğrulamak için iyi bir fikirdir. Birçok geliştirme ekiplerinin müşteriler yararlanan işlevsel sistemi gereksinimleri yakalama mükemmel iş yapın. Ancak, birisi sistemin kötüye belirgin olmayan yolları tanımlayan daha zordur. Tehdit modelleme geliştirme ekiplerinin bir saldırgan ne anlamanıza yardımcı olabilir ve neden.

### <a name="how-to-threat-model"></a>Nasıl yapılır tehdit modeli
İşlem modelleme tehdit dört adımdan oluşur; adımlar şunlardır:

* Uygulama modeli
* Tehditler listeleme
* Tehditlerin azaltılmasına
* Azaltıcı doğrula

#### <a name="the-process-steps"></a>İşlem adımları
Üç kuralları bir tehdit modeli oluşturulurken göz önünde bulundurmanız altın:

1. Referans Mimarisi dışında bir diyagram oluşturun. 
2. Avantajlarına ilk başlatın. Genel bir bakış elde ve derin girmeden önce bir bütün olarak sistem anlayın.  Bu sağlamaya yardımcı olur. Bu, ayrıntılı-Dalış doğru yerde.
3. İşlem sürücü, sürücüsü işlem izin vermeyin. Modelleme aşamasında bir sorun bulmak ve onu keşfetmek istediğiniz, bunun için Git!  Bu adımları slavishly gerek düşündüğünüz yok.  

#### <a name="threats"></a>Tehditleri
Bir tehdit modeli dört temel öğeleri şunlardır:

* İşlemler (web Hizmetleri, Win32 hizmetleri * nix Daemon, vs. Bir işlem teknik ayrıntıya olduğunda bu alanlarda mümkün değil gibi bazı karmaşık varlıklar (örneğin alan ağ geçitleri ve algılayıcılar) soyutlanması unutmayın.
* Verileri (her yerden bir yapılandırma dosyası veya veritabanı gibi veriler depolanır) depolar
* Veri akışı (burada veri uygulamadaki diğer öğeler arasında taşır)
* Dış varlıklar (sistem ile etkileşimde bulunan herhangi bir şey ancak uygulama denetimi altında örnekler kullanıcıları eklemek ve uydu akışları)

Mimari diyagramı tüm öğeler çeşitli tehditlere maruz; yine de uygun istiyor musunuz? STRIDE anımsatıcı kullanacağız. Okuma [yeniden tehdit modelleme, STRIDE](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) STRIDE öğeler hakkında daha fazla bilgi edinmek için.

Uygulama diyagramı farklı öğeler belirli STRIDE tehditlere maruz şunlardır:

* STRIDE tabi işlemlerdir
* Veri akışları KOMUTU tabi olan
* Veri depoları günlük dosyalarını veri depolarına KOMUTU ve R, tabi bazen bağlanırlar.
* Dış varlığı SRD tabi şunlardır:

## <a name="security-in-iot"></a>IOT güvenlik
Bağlı özel amaçlı cihazlar çok sayıda olası etkileşim yüzey alanlarını ve her biri, bu cihazlar dijital erişimin güvenliğini sağlamak için bir çerçeve sağlamak üzere düşünülmelidir etkileşim düzenleri sahip. "Dijital erişim" terimi burada doğrudan aygıt etkileşiminin gerçekleştirilen herhangi bir işlem ayırmak için erişim güvenliği fiziksel erişim denetimi aracılığıyla burada sağlanan kullanılır. Örneğin, cihaz kilit bir odada içine kapısı koyma. Fiziksel erişim, yazılım ve donanım kullanarak kısıtlanamaz olsa da, sistem kesintiye gelen başında fiziksel erişimi önlemek için ölçüler alınabilir. 

Biz etkileşim desenleri keşfetmenizde biz "aygıt denetimi" ve "cihaz verileri" dikkat aynı düzeyiyle arar. "Aygıt denetimi" bir aygıta değiştirme veya davranışını durumuna veya kendi ortamı durumunu doğru etkileyen amacı ile herhangi bir şirket tarafından sağlanan herhangi bir bilgi olarak sınıflandırılabilir. "Cihaz verileri" durumuna ve kendi ortamı gözlemlenen durumu hakkında herhangi bir taraf için bir aygıt yayar herhangi bir bilgi olarak sınıflandırılabilir.

En iyi güvenlik en iyi duruma getirmek için tipik bir IOT mimarisinin alıştırma modelleme tehdit bir parçası olarak birkaç bileşen/bölgelere ayrılmasına önerilir. Bu bölgeler tam olarak bu bölümde açıklanan ve şunları içerir:

* Cihazı
* Alan ağ geçidi
* Bulut, ağ geçitleri ve
* Hizmetler.

Bölgeleri şekilde bir çözüm segmentlere ayırmak için geniş; her bölge genellikle kendi veri ve kimlik doğrulama ve yetkilendirme gereksinimlerine sahiptir. Bölgeleri de yalıtım zarar görmesine kullanılması ve düşük güven bölgeleri daha yüksek güven bölgelerinde etkisini kısıtlayın.

Her bölge bir güven, aşağıdaki çizimde kırmızı noktalı çizgi belirtildiği sınır ile ayrılır. Veri/bilgileri geçişin bir kaynak sunucudan diğerine temsil eder. Bu geçiş sırasında veri/bilgileri sahtekarlık, kurcalama, ret, bilgi açıklama, hizmet reddi ve ayrıcalık yükseltme (STRIDE) tabi olabilir.

![IOT güvenlik bölgeleri](media/iot-security-architecture/iot-security-architecture-fig1.png) 

Her bir sınır içinde tanımlanan bileşenleri de tam 360 etkinleştirme STRIDE için tabi çözüm görünümünü modelleme tehdit. Aşağıdaki bölümlerde ayrıntılı şekilde her bileşenleri ve belirli güvenlik sorunlarının yerine sokulmalıdır çözümler.

Aşağıdaki bölümlerde, genellikle bölgelerinde bulunan standart bileşenleri ele alınacaktır.

### <a name="the-device-zone"></a>Cihaz bölge
Aygıt aygıt hemen fiziksel boşluk fiziksel burada ortamıdır erişim ve/veya "yerel ağ" eşler arası sayısal erişim aygıta uygulanabilir. "Yerel ağ" ayrı ve gelen – yalıtılmış ancak büyük olasılıkla genel Internet'e – bağlantı ve aygıtların eşler arası iletişime izin verir herhangi mesafeli Kablosuz radyo teknolojisi içeren bir ağ olduğu varsayılır. Mevcut *değil* yerel bir ağ atmosferini oluşturma herhangi bir ağ sanallaştırma teknolojisini içerir ve ortak ağ alanı arasında iletişim için herhangi iki aygıtları gerektiren genel işleç ağları da içermez bir eşler arası iletişim ilişkisi girmek için yoktu.

### <a name="the-field-gateway-zone"></a>Alan ağ geçidi bölge
Alan ağ geçidi aygıtı/gereç ya da iletişim etkinleştiricisi olarak ve da potansiyel olarak, aygıt denetim sistemi ve aygıt veri işleme hub gibi davranan bazı genel amaçlı sunucu bilgisayar yazılımı olur. Alan ağ geçidi bölge, alan ağ geçidi kendisi ve ona bağlı olan tüm aygıtları içerir. Adından da anlaşılacağı gibi alan ağ geçitleri dış özel veri işleme tesis hareket, genellikle konumu bağlı olan, olası fiziksel yetkisiz erişim tabi olan ve işletimsel artıklık sınırlı. Tüm bir alan ağ geçidi genellikle bir şey olduğunu söylemek için bir touch ve onun işlevini nedir bilerek sabotage. 

Bir alan ağ geçidi erişimi yönetme etkin bir rol oluşturdu ve bilgi akışını uygulama olduğu anlamına gelir, varlık ve ağ bağlantınızı veya terminal oturumu ele yalnızca trafik yönlendiriciden farklıdır. Bunlar açık bir bağlantı veya oturum Terminal, ancak bunun yerine bir yol (veya blok) bağlantıları olmayan bu yana bir NAT cihazı veya güvenlik duvarı, buna karşılık, alan ağ geçidi olarak nitelemek değil ya da bunları yapılan oturumları. Alan ağ geçidi iki ayrı yüzey alanı yok. Bir bağlı aygıtları bakarken ve bölgenin iç temsil eder ve diğer tüm dış tarafların bakarken ve bölgenin ucunun.   

### <a name="the-cloud-gateway-zone"></a>Bulut ağ geçidi bölge
Bulut ağ geçidi genellikle bir bulut tabanlı denetim ve veri analizi sistem, bu sistemlere Federasyonu doğrultusunda ortak ağ alanı boyunca ilk ve son aygıtları veya alan ağ geçitleri birkaç farklı sitelerin uzaktan iletişimi sağlayan bir sistemdir. Bazı durumlarda, bir bulut ağ geçidi hemen erişim için özel amaçlı cihazlar Terminal tabletler veya telefonlar gibi gelen kolaylaştırabilir. Burada tartışılan bağlamında "bulut" ekli cihazlara veya alan ağ geçitleri aynı sitede bağlı olmayan bir özel veri işleme sistemine başvurmak için tasarlanmıştır. Ayrıca bir bulut bölgedeki işletimsel ölçüleri hedeflenen fiziksel erişimi engellemek ve mutlaka bir "Genel bulut" altyapısı sunulmaz.  

Bulut ağ geçidi, bir ağ sanallaştırma katmana bulut ağ geçidi ve tüm bağlı aygıtları veya alan ağ geçitleri üzerinden diğer ağ trafiğinden verenlerden içine potansiyel olarak eşlenebilir. Bulut ağ geçidinin kendisi ne aygıt denetim sistem ya da bir işleme veya depolama tesisi cihaz verileri için olan; Bu olanakları ile bulut Ağ Geçidi Arabirimi. Bulut ağ geçidi bölge bulut ağ geçidinin tüm alan ağ geçitleri ve doğrudan veya dolaylı olarak kendisine bağlı cihazların yanı sıra içerir. Kenarın bölgenin, tüm dış tarafların üzerinden iletişim kurduğu ayrı bir yüzey alanıdır.

### <a name="the-services-zone"></a>Hizmetleri bölge
"Hizmet" Bu bağlamda herhangi bir yazılım bileşeni veya cihazlara sahip bir alan veya Bulut ağ geçidi üzerinden veri toplama ve analiz için yanı sıra komut ve denetim için arabirim modülü olarak tanımlanmıştır.  Ortam araçları hizmetleridir. Bunlar kimliklerini ağ geçitleri ve diğer alt sistemleri doğrultusunda altında hareket, depolamak ve verileri çözümlemek, sınırlarına veri Öngörüler veya zamanlamaları göre cihazlara komutlarını verin ve bilgi kullanıma ve yetkili son kullanıcılara özellikleri denetlemek.

### <a name="information-devices-vs-special-purpose-devices"></a>Özel amaçlı cihazlar ve aygıtlarını bilgiler
Bilgisayarlar, telefonlar ve tabletler öncelikle etkileşimli bilgi aygıtlardır. Telefonlar ve tabletler açıkça pil ömrü en üst düzeye çıkarma geçici hale getirilmiştir. Bunlar tercihen kısmen hemen bir kişiyle kullanılırken ya da müzik çalma veya belirli bir konuma kendi sahibi yönlendirmede gibi hizmetleri sağlayan değil kapatın. Sistemleri açısından bakıldığında, bu bilgi teknolojisi cihazlar proxy'leri kişiler doğru olarak esas olarak çalışıyor. "Kişiler erişim düzenekleri eylemleri ve"kişiler algılayıcılar giriş toplama"öneren" oldukları. 

Basit sıcaklık algılayıcıları karmaşık Üreteç üretim satırlarına bileşenleri içerdikleri, binlerce ile özel amaçlı cihazlar farklıdır. Bu cihazlar çok daha amacı kapsamlı ve bazı kullanıcı arabirimi sağladıkları olsa bile, arabirim ile büyük ölçüde kapsamına veya Fiziksel dünyadaki varlıklar içine tümleştirilmiştir. Bunlar ölçmek ve ortam koşullar raporu, vanalar açın, servos denetlemek, alarmlar ses, ışık geçin ve birçok diğer görevleri yapmak. İş için bir bilgi cihaz çok genel, çok pahalı, çok büyük veya çok kırılır yardımcı olurlar. Somut amacı teknik tasarımlarını, üretim ve zamanlanmış ömrü işlemi için de kullanılabilir parasal bütçe olarak hemen belirler. Bu iki önemli faktör birleşimi kullanılabilir işletimsel enerji bütçe, fiziksel ayak izini ve böylece kullanılabilir depolama, hesaplama ve güvenlik özellikleri kısıtlar.  

Bir şey varsa "gelecek yanlış" otomatik olarak veya uzaktan denetlenebilir aygıtlarla, örneğin, fiziksel hataları veya Denetim mantığı willful yetkisiz yetkisiz erişim ve işleme hataları. Üretim çok yok edilmesi, binalar looted veya Yakılan ve kişiler yaralı ve hatta özel olabilir. Bu, doğal olarak, bir tam farklı birisi bir çalınan Kredi kartının sınırı maxing daha kesimin sınıftır. Taşıma şeyler yapın cihazlar için ve ayrıca sonunda taşımak şey neden komutlarda sonuçları algılayıcı verileri için güvenlik çubuğunu herhangi bir e-ticaret veya banka senaryosu daha yüksek olmalıdır. 

### <a name="device-control-and-device-data-interactions"></a>Aygıt denetimi ve aygıt veri etkileşimleri
Bağlı özel amaçlı cihazlar çok sayıda olası etkileşim yüzey alanlarını ve her biri, bu cihazlar dijital erişimin güvenliğini sağlamak için bir çerçeve sağlamak üzere düşünülmelidir etkileşim düzenleri sahip. "Dijital erişim" terimi burada doğrudan aygıt etkileşiminin gerçekleştirilen herhangi bir işlem ayırmak için erişim güvenliği fiziksel erişim denetimi aracılığıyla burada sağlanan kullanılır. Örneğin, cihaz kilit bir odada içine kapısı koyma. Fiziksel erişim, yazılım ve donanım kullanarak kısıtlanamaz olsa da, sistem kesintiye gelen başında fiziksel erişimi önlemek için ölçüler alınabilir. 

Biz etkileşim desenleri keşfetmenizde biz "aygıt denetimi" ve "cihaz verileri" ile aynı düzeyde tehdit modelleme çalışırken dikkat arar. "Aygıt denetimi" bir aygıta değiştirme veya davranışını durumuna veya kendi ortamı durumunu doğru etkileyen amacı ile herhangi bir şirket tarafından sağlanan herhangi bir bilgi olarak sınıflandırılabilir. "Cihaz verileri" durumuna ve kendi ortamı gözlemlenen durumu hakkında herhangi bir taraf için bir aygıt yayar herhangi bir bilgi olarak sınıflandırılabilir. 

## <a name="threat-modeling-the-azure-iot-reference-architecture"></a>Tehdit modelleme Azure IOT başvuru mimarisi
Microsoft Azure IOT modelleme tehdit yapmak için yukarıda özetlenen çerçevesi kullanır. Bölümde bu nedenle Azure IOT başvuru mimarisi somut örneği için IOT modelleme tehdit düşünün ve tanımlanan tehditlere göstermek için kullanırız. Örneğimizde dört ana odak alanı belirledik:

* Cihazlar ve veri kaynakları
* Veri taşıma
* Cihaz ve olay işleme ve
* Sunu

![Tehdit için Azure IOT modelleme](media/iot-security-architecture/iot-security-architecture-fig2.png) 

Aşağıdaki diyagramda Microsoft tehdit modelleme aracı tarafından kullanılan bir veri akış diyagramı modeli kullanılarak Microsoft'un IOT mimarisinin basitleştirilmiş bir görünümünü sağlar:

![Tehdit MS tehdit modelleme aracını kullanarak Azure IOT için modelleme](media/iot-security-architecture/iot-security-architecture-fig3.png)

Mimari cihaz ve ağ geçidi özellikleri ayıran dikkate almak önemlidir. Bu kullanıcının daha güvenli olan ağ geçidi aygıtlarını yararlanan olanak sağlar: - thermostat gibi-yerel bir cihaz üzerinde sağlayabilir büyük yükü işlemi genellikle gerektiren güvenli protokolleri kullanılarak bulut ağ geçidi ile iletişim kurabilen kendi. Azure Hizmetleri bölgesinde bulut ağ geçidi Azure IOT Hub hizmeti tarafından temsil edilen varsayalım.

### <a name="device-and-data-sourcesdata-transport"></a>Aygıt ve veri kaynakları/veri aktarımı
Bu bölümde, tehdit modelleme Mercek yukarıda özetlenen mimarisi inceler ve nasıl biz bazı devralınmış sorunları ele alır genel bir bakış sağlar. Biz bir tehdit modeli çekirdek öğelerde odaklanır:

* İşlemler (bizim denetim ve dış öğeler altında olanlar)
* (Veri akışları olarak da bilinir) iletişimi
* Depolama (veri depoları olarak da bilinir)

#### <a name="processes"></a>İşlemler
Veri/bilgileri bulunmaktadır farklı aşamaları boyunca bir dizi farklı tehditleri azaltmak her Azure IOT mimarisinde özetlenen kategorileri, biz deneyin: işlem, iletişim ve depolama. Aşağıda en yaygın olanları nasıl bu en iyi şekilde azaltılması gereken genel bir bakış tarafından izlenen "işlem" kategorisi için genel bir bakış sunuyoruz: 

**(S) yanıltma**: bir saldırganın bir aygıttan yazılım veya donanım düzeyinde ya da şifreleme anahtar malzemesi ayıklamak ve daha sonra anahtar malzemesi cihaz kimliği altında farklı bir fiziksel veya sanal cihaz ile sistemine erişim gelen alınmış. Uzaktan herhangi TV kapatabilirsiniz ve popüler prankster araçları olan denetimleri buna iyi bir çizimidir.

**Engelleme, hizmet (D)**: bir aygıt çalışmıyor veya radyo frekansları veya kesme kablolarını uğratarak iletişim kuramadığı oluşturulabilir. Örneğin, özellikle gizleyen güç veya ağ bağlantısı olan bir izleme kamera verileri hiç bildirmez.

**(T) oynama**: bir saldırgan kısmen veya tamamen cihazda çalışan yazılım olası anahtar malzemesi veya bulunduran şifreleme tesis anahtarı cihaz kimliğini yararlanmak değiştirilen yazılım izin vererek değiştirebilir malzemeleri yasadışı programın kullanılabilir. Örneğin, bir saldırganın müdahale ve iletişim yolunun aygıtta verilerden bastırmak ve çalınan anahtar malzemesi ile kimlik doğrulaması yanlış verilerle değiştirmek için ayıklanan anahtar malzemesi yararlanın.

**Bilgilerin açığa çıkmasına (ı)**: cihaz yönetilebilen yazılım çalışıyorsa yönetilebilen yazılımla olası yetkisiz taraflara veri sızıntısı. Örneğin, bir saldırganın kendisini cihaz denetleyicisi veya alan ağ geçidi veya bilgi siphon için bulut ağ geçidi arasındaki iletişim yolunun içine eklemesine ayıklanan anahtar malzemesi yararlanın.

**Yükseltme, ayrıcalık (E)**: başka bir şey yapmak için belirli bir işlevi gerçekleştiren bir aygıtı zorlanabilir. Örneğin, yarı yol açmak üzere programlanmış Vana tüm açmak için sağladı.

| **Bileşen** | **Tehdit** | **Azaltma** | **Riski** | **Uygulama** |
| --- | --- | --- | --- | --- |
| Cihaz |S |Cihaz kimlik doğrulaması ve kimlik cihaza atama |Aygıt veya aygıtın başka bir aygıt ile değiştirin. Biz doğru cihaza Konuşmayı nasıl biliyoruz? |Aktarım Katmanı Güvenliği (TLS) veya IPSec kullanarak cihaz kimlik doğrulaması. Altyapı önceden paylaşılan anahtar (PSK) kullanarak tam asimetrik şifreleme işleyemiyor bu cihazlarda desteklemelidir. Azure AD yararlanan [OAuth](http://www.rfc-editor.org/in-notes/internet-drafts/draft-ietf-ace-oauth-authz-01.txt) |
| TRID |Tamperproof mekanizmaları çok zor anahtarları ve diğer şifreleme malzeme aygıttan ayıklamak imkansız için kolaylaştırarak cihaza örneğin uygulayın. |Birisi (fiziksel girişim) cihaz oynama, riski oluşturur. Gerçekleştirildiğine nasıl emin olun, bu cihaz üzerinde oynama değil. |En etkili Azaltıcı içinden anahtarları okunamıyor, ancak yalnızca anahtar kullanan ancak hiç anahtar ifşa şifreleme işlemleri için kullanılabilir özel yongadaki devresi anahtarlarını depolamak izin veren bir güvenilir platform Modülü (TPM) bir özelliktir. Aygıt bellek şifreleme. Cihaz için anahtar yönetimi. Kod imzalama. | |
| E |Cihazın erişim denetimi sahip. Yetkilendirme düzeni. |Tek tek eylemlerin gerçekleştirilmesini cihaz komutları bir dış kaynaktan ya da güvenliği aşılmış algılayıcılar temel alınarak izin veriyorsa, Bu saldırının aksi işlemleri için erişilebilir izin verir. |Cihaz için Yetkilendirme düzeni sahip | |
| Alan ağ geçidi |S |Bulut ağ geçidi (bağlı olarak, sertifika PSK, bağlı olarak, talep..) için alan ağ geçidi kimlik doğrulaması |Ardından, birisi alan ağ geçidi taklit edebilir, kendisini herhangi bir aygıtı sunabilir. |TLS RSA/PSK, IPSec, [RFC 4279](https://tools.ietf.org/html/rfc4279). Aynı genel – aygıtların temel depolama ve kanıtlama sorunlarının en iyi durum kullanın TPM. Kablosuz algılayıcı ağları (WSN) desteklemek IPSec 6LowPAN uzantısı. |
| TRID |Alan ağ geçidi'ni (TPM?) oynama karşı koruma |Sahtekarlığı alan ağ geçidi Konuşmayı bulut ağ geçidi düşünmeye kandırarak saldırıları bilgilerin açığa çıkması ve verileri izinsiz neden olabilir |Bellek şifreleme, TPM bilgisayarın, kimlik doğrulaması. | |
| E |Alan ağ geçidi için erişim denetimi mekanizması | | | |

Bu kategorideki tehditleri bazı örnekleri şunlardır:

Kimlik sahtekarlığı: Bir saldırgan şifreleme anahtar malzemesi bir aygıtı yazılım veya donanım düzeyinde ve daha sonra anahtar malzemesi cihazın kimlik altında farklı bir fiziksel veya sanal cihaz sistemiyle runbook'undan alınan erişim Al.

**Hizmet reddi**: bir aygıt çalışmıyor veya radyo frekansları veya kesme kablolarını uğratarak iletişim kuramadığı oluşturulabilir. Örneğin, özellikle gizleyen güç veya ağ bağlantısı olan bir izleme kamera verileri hiç bildirmez.

**İzinsiz**: bir saldırgan kısmen veya tamamen cihazda çalışan yazılım olası anahtar malzemesi veya bulunduran şifreleme tesis anahtarı cihaz kimliğini yararlanmak değiştirilen yazılım izin vererek değiştirebilir malzemeleri yasadışı programın kullanılabilir.

**İzinsiz**: boş koridor spektrumun görünür resmini gösteren bir izleme kamera böyle bir koridor fotoğrafı amaçlayan. Duman veya yangın algılayıcı birisi altındaki bir açık tutarak raporlama. Her iki durumda da aygıt teknik olarak doğru sistem tam olarak güvenilir olabilir, ancak yönetilebilen bilgi rapor eder.

**İzinsiz**: bir saldırgan müdahale ve iletişim yolunun aygıtta verilerden bastırmak ve çalınan anahtar malzemesi ile kimlik doğrulaması yanlış verilerle değiştirmek için ayıklanan anahtar malzemesi yararlanan.

**İzinsiz**: bir saldırgan kısmen veya tamamen cihazda çalışan yazılım potansiyel olarak, cihaz kimliğini yararlanacağınızı değiştirilen yazılım izin vererek değiştirebilir anahtar malzeme veya bulunduran şifreleme özellikleri anahtar malzeme yasadışı programın kullanılabilir.

**Bilgilerin açığa çıkmasına**: cihaz yönetilebilen yazılım çalışıyorsa yönetilebilen yazılımla olası yetkisiz taraflara veri sızıntısı.

**Bilgilerin açığa çıkmasına**: bir saldırgan kendisini cihaz denetleyicisi veya alan ağ geçidi veya devre dışı bilgi siphon için bulut ağ geçidi arasındaki iletişim yolunun içine eklemesine ayıklanan anahtar malzemesi yararlanan.

**Hizmet reddi**: aygıt devre dışı ya da bir moduna iletişimi (olduğu birçok endüstriyel makinelerinizde kasıtlı) mümkün olduğu açık.

**İzinsiz**: cihaz kontrol sistemine (dışında bilinen ayarlama parametreleri) bilinmeyen bir durumda çalışır ve böylece yorumlanabilir veri sağlamak için yeniden yapılandırılması

**Ayrıcalık yükseltme**: başka bir şey yapmak için belirli bir işlevi gerçekleştiren bir aygıtı zorlanabilir. Örneğin, yarı yol açmak üzere programlanmış Vana tüm açmak için sağladı.

**Hizmet reddi**: cihaz iletişimi olduğu olası bir duruma açılabilir.

**İzinsiz**: cihaz kontrol sistemine (dışında bilinen ayarlama parametreleri) bilinmeyen bir durumda çalışır ve böylece yorumlanabilir veri sağlamak için yeniden yapılandırılması.

**Kimlik sahtekarlığı/kurcalama/ret**: güvenli değilse (olduğu nadiren tüketici uzaktan denetimleri durumuyla) bir saldırganın aygıtın durumunu anonim olarak değiştirebilirsiniz. Uzaktan herhangi TV kapatabilirsiniz ve popüler prankster araçları olan denetimleri buna iyi bir çizimidir.

#### <a name="communication"></a>İletişim
Cihazlar, aygıtları ve alan ağ geçitleri ve cihaz ve bulut ağ geçidi arasındaki iletişim yolunun geçici tehditleri. Aşağıdaki tablo bazı yönergeler aygıt/VPN açık yuva geçici sahiptir:

| **Bileşen** | **Tehdit** | **Azaltma** | **Riski** | **Uygulama** |
| --- | --- | --- | --- | --- |
| Cihaz IOT hub'ı |KOMUTU |(D) TLS (trafiğini şifrelemek için PSK/RSA) |Gizli dinleme veya cihaz ve ağ geçidi arasındaki iletişimi engelliyor |Güvenlik protokolü düzeyi. Özel protokollerle onları korumak nasıl bağlayacağınızı ihtiyacımız. Çoğu durumda, iletişimin aygıttan (aygıt bağlantı başlatır) IOT Hub'ına gerçekleşir. |
| Aygıtın aygıt |KOMUTU |(D) TLS (trafiğini şifrelemek için PSK/RSA). |Cihazlar arasında Aktarımdaki verileri okunuyor. Verilerinize müdahale. Yeni bağlantıları aygıtla aşırı yüklemesi |Güvenlik protokolü düzeyinde (MQTT/AMQP/HTTP/CoAP. Özel protokollerle onları korumak nasıl bağlayacağınızı ihtiyacımız. Azaltma DoS tehdit için bir bulut ya da alan ağ geçidi üzerinden cihazları eş ve bunları ağ doğrultusunda istemcileri olarak yalnızca act sahip olmaktır. Eşlemeyi ağ geçidiyle aracılı sonra eşler arasında doğrudan bağlantı ile sonuçlanabilir |
| Dış varlık cihaz |KOMUTU |Dış varlık cihaz için güçlü eşleştirme |Cihaz bağlantısı gizli dinleme. Aygıtla iletişimi engelliyor |Güvenli bir şekilde NFC/Bluetooth LE aygıta Dış varlık çifti. Aygıt (fiziksel) işletimsel panelinin denetleme |
| Alan ağ geçidi bulut ağ geçidi |KOMUTU |TLS (trafiğini şifrelemek için PSK/RSA). |Gizli dinleme veya cihaz ve ağ geçidi arasındaki iletişimi engelliyor |Güvenlik protokolü düzeyinde (MQTT/AMQP/HTTP/CoAP). Özel protokollerle onları korumak nasıl bağlayacağınızı ihtiyacımız. |
| Cihaz bulut ağ geçidi |KOMUTU |TLS (trafiğini şifrelemek için PSK/RSA). |Gizli dinleme veya cihaz ve ağ geçidi arasındaki iletişimi engelliyor |Güvenlik protokolü düzeyinde (MQTT/AMQP/HTTP/CoAP). Özel protokollerle onları korumak nasıl bağlayacağınızı ihtiyacımız. |

Bu kategorideki tehditleri bazı örnekleri şunlardır:

**Hizmet reddi**: kısıtlanmış aygıtlardır genellikle DoS tehlike altında bir saldırganın birçok bağlantıları paralel olarak açabilir ve değil bunları hizmet veya hizmet çünkü bunlar etkin olarak gelen bağlantıları veya bir ağ üzerindeki istenmeyen veri birimleri için dinlerken bunları çok yavaş veya aygıt ile istenmeyen trafiği yayılmamış olabilir. Her iki durumda da, cihaz etkili bir şekilde ağda kullanılamaz hale getirilebilir.

**Yanıltma, bilgi İfşası**: kısıtlanmış aygıtları ve özel amaçlı cihazlar genellikle parola veya PIN koruma gibi tüm için bir güvenlik tesis sahiptir veya bunlar tamamen bunlar erişimine için anlamı ağ güvenen kullanır aynı ağ ve o ağ üzerindeki bir aygıt olduğunda bilgiler genellikle yalnızca bir paylaşılan anahtar tarafından korunur. Aygıt ya da ağ paylaşılan gizliliği duyurulmuş olduğunda cihazı denetlemek veya aygıttan yayılan verileri gözlemlemek mümkündür, anlamına gelir.  

**Kimlik sahtekarlığı**: bir saldırgan kesebilen veya kısmen yayın geçersiz kılmak ve gönderenin (ADAM ortada) aldatma

**İzinsiz**: bir saldırgan müdahale veya kısmen yayın geçersiz kılabilir ya da yanlış bilgi gönder 

**Bilgi İfşası:** bir saldırganın bir yayın üzerinde misafiri ve yetkilendirme bilgileri elde **hizmet reddi:** bir saldırganın yayın sinyal jam ve bilgi dağıtım Reddet

#### <a name="storage"></a>Depolama
Her cihaz ve alan ağ geçidi (işletim sistemi (OS) görüntüsü depolama, veri queuing geçici) depolama çeşit vardır.

| **Bileşen** | **Tehdit** | **Azaltma** | **Riski** | **Uygulama** |
| --- | --- | --- | --- | --- |
| Cihaz depolama |TRID |Depolama şifrelemesi, günlükleri imzalama |Depolama (PII veri) verileri telemetri verilerinize müdahale okuma. Değiştirilmesine sıraya veya komut denetim verileri önbelleğe alınmış. Yapılandırma veya bellenimi güncelleştirme paketleriyle oynama önbelleğe alınmış veya yerel olarak kuyruğa sırada güvenliğinin bozulması riskini işletim sistemi ve/veya sistem bileşenleri için yol açabilir |Şifreleme, ileti kimlik doğrulama kodu (MAC) veya dijital imza. Burada kaynak erişimi aracılığıyla olası, güçlü erişim denetim listeleri (ACL'ler) veya izinleri denetler. |
| Cihaz işletim sistemi görüntüsü |TRID | |İşletim sistemiyle oynama / işletim sistemi bileşenleri değiştirme |Salt okunur işletim sistemi bölümü, imzalı işletim sistemi görüntüsü, şifreleme |
| (Verileri queuing) ağ geçidi depolama alanı |TRID |Depolama şifrelemesi, günlükleri imzalama |Depolama (PII veri) telemetri verilerinize müdahale verileri okuma, sıraya değiştirilmesine veya komut denetim verileri önbelleğe alınmış. (Aygıtlar veya alan ağ geçidi için hedefleyen) yapılandırması veya bellenimi güncelleştirme paketleriyle oynama önbelleğe alınmış veya yerel olarak kuyruğa sırada güvenliğinin bozulması riskini işletim sistemi ve/veya sistem bileşenleri için yol açabilir |BitLocker'ı |
| Alan ağ geçidi işletim sistemi görüntüsü |TRID | |İşletim sistemiyle oynama / işletim sistemi bileşenleri değiştirme |Salt okunur işletim sistemi bölümü, imzalı işletim sistemi görüntüsü, şifreleme |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Aygıt ve olay işleme/bulut ağ geçidi bölge
Bulut ağ geçidi genellikle bir bulut tabanlı denetim ve veri analizi sistem, bu sistemlere Federasyonu doğrultusunda ortak ağ alanı boyunca ilk ve son aygıtları veya alan ağ geçitleri birkaç farklı sitelerin uzaktan iletişimi sağlayan sistemidir. Bazı durumlarda, bir bulut ağ geçidi hemen erişim için özel amaçlı cihazlar Terminal tabletler veya telefonlar gibi gelen kolaylaştırabilir. Ele alınan bağlamda burada "bulut" bağlı olmayan aynı siteye bağlı aygıtları veya alan ağ geçitleri olarak ve burada hedeflenen fiziksel erişimi engelle işletimsel ölçüler bir özel veri işleme sistemine başvurmak için tasarlanmıştır ancak mutlaka çok değil bir " Genel bulut"altyapısı.  Bulut ağ geçidi, bir ağ sanallaştırma katmana bulut ağ geçidi ve tüm bağlı aygıtları veya alan ağ geçitleri üzerinden diğer ağ trafiğinden verenlerden içine potansiyel olarak eşlenebilir. Bulut ağ geçidinin kendisi ne aygıt denetim sistem ya da bir işleme veya depolama tesisi cihaz verileri için olan; Bu olanakları ile bulut Ağ Geçidi Arabirimi. Bulut ağ geçidi bölge bulut ağ geçidinin tüm alan ağ geçitleri ve doğrudan veya dolaylı olarak kendisine bağlı cihazların yanı sıra içerir.

Bulut ağ geçidi genellikle özel yerleşik bir hizmet olarak çalışması için alan ağ geçidi ve aygıtları bağlamak gösterilen uç noktaları ile yazılım parçasıdır. Bu nedenle ile göz önünde bulundurularak tasarlanmalıdır. Lütfen izleyin [SDL](http://www.microsoft.com/sdl) tasarlama ve bu hizmet oluşturmak için işlem. 

#### <a name="services-zone"></a>Hizmetleri bölge
Bir denetim sistemi (veya denetleyicisi) bir cihaz veya alan ağ geçidi veya bir veya birden çok aygıt denetlemek amacıyla ve/veya toplamanıza ve/veya depolamak ve/veya sunum, cihaz verileri çözümlemek için bulut ağ geçidi arabirimleri bir yazılım çözümü olan veya izleyen denetim amaçlar. Denetim sistemleri kişilerle etkileşim hemen kolaylaştırabilir bu tartışma kapsamında yalnızca varlıklardır. Özel durum Ara fiziksel denetim yüzeyleri cihazlarda, cihazı kapatmak veya diğer özelliklerini değiştirmek bir kişi izin veren bir anahtar gibi ve hangi dijital olarak erişilebilen işlevsel bir eşdeğeri yoktur için kullanılır. 

Ara fiziksel denetim yüzeyleri bilgilerdir burada mantığını yöneten, herhangi bir biçimini kısıtlar fiziksel denetim yüzeyini işlevi sağlayacak şekilde eşdeğer bir işlevi uzaktan başlatılabilir veya hükümsüz kılınan – bu tür uzaktan giriş giriş çakışıyor olabilir intermediated denetim yüzeyleri cihaz paralel olarak eklenmiş diğer uzaktan denetim sistemi olarak aynı temel işlevsellikten yararlanan bir yerel kontrol sistemine kavramsal olarak eklenir. Olabilir, okuma bulut üst tehditleri [bulut güvenlik Alliance (CSA)](https://cloudsecurityalliance.org/research/top-threats/) sayfası.

## <a name="additional-resources"></a>Ek kaynaklar
Ek bilgi için aşağıdaki makalelere bakın:

* [SDL tehdit modelleme aracı](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
* [Microsoft Azure IOT başvuru mimarisi](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)

## <a name="see-also"></a>Ayrıca bkz.
IOT çözümünüzün güvenliğini sağlama hakkında daha fazla bilgi için bkz: [, IOT dağıtımınızın güvenliğini][lnk-security-deployment].

Önceden yapılandırılmış IoT Suite çözümlerinin diğer özelliklerinden bazılarını da keşfedebilirsiniz:

* [Önceden yapılandırılmış Tahmine dayalı bakım çözümüne genel bakış][lnk-predictive-overview]
* [IoT Paketi için sık sorulan sorular][lnk-faq]

IOT hub'ı güvenlik konusunda okuyabilirsiniz [IOT Hub'ına erişim denetim] [ lnk-devguide-security] IOT Hub Geliştirici Kılavuzu'nda.

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md

[lnk-security-deployment]: iot-suite-security-deployment.md
[lnk-devguide-security]: ../iot-hub/iot-hub-devguide-security.md