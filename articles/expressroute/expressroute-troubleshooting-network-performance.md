---
title: 'Sanal ağ performans sorunlarını giderme: Azure | Microsoft Docs'
description: Bu sayfa, Azure ağ bağlantısı performans testi için standartlaştırılmış bir yöntem sağlar.
services: expressroute
author: tracsman
ms.service: expressroute
ms.topic: article
ms.date: 12/20/2017
ms.author: jonor
ms.custom: seodec18
ms.openlocfilehash: 9ec310ffaa9d2bb297abde9341bf7b6c2dc763b4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60883342"
---
# <a name="troubleshooting-network-performance"></a>Ağ performansı sorunlarını giderme
## <a name="overview"></a>Genel Bakış
Azure, şirket içi ağınızdan Azure'a bağlanmak için kararlı ve hızlı yollar sunar. Yöntemleri ister siteden siteye VPN ve ExpressRoute başarıyla küçük ve büyük ölçekli müşteriler tarafından Azure'da yürütmek için kullanılır. Ancak, performans özel durumuyla veya önceki deneyime karşılamadığında ne olur? Bu belge, test etme biçimimizi ve temel ortamınıza standartlaştırmak yardımcı olabilir.

Bu belge, nasıl kolayca ve sürekli olarak ağ gecikme süresi ve bant genişliği iki konak arasında test edebilirsiniz gösterir. Bu belge, Azure ağı bakın ve sorunu noktaları yalıtmaya yardımcı yolları da bazı öneriler sağlar. Açıklanan araçları ve PowerShell Betiği (başındaki veya sonundaki bir bağlantıya edildiğinin) ağ üzerinde iki ana gerektirir. Bir konak, bir Windows Server ya da Masaüstü olmalıdır, diğer Windows veya Linux olabilir. 

>[!NOTE]
>Sorun giderme yaklaşımı, araçları ve kullanılan yöntemleri kişisel tercihlerdir. Bu belgede, genellikle ele yaklaşım ve araçlar açıklanmaktadır. Büyük olasılıkla yaklaşımınızı farklılık gösterir, sorunu çözmek için farklı yaklaşımlara ile yanlış bir şey yoktur. Ancak, bir yaklaşımdır yoksa, bu belge yolunda kendi yöntemlerini, araçları ve ağ sorunlarını gidermek için Tercihler oluşturmaya başlamanızı alabilirsiniz.
>
>

## <a name="network-components"></a>Ağ bileşenleri
Sorun giderme içine sorunda önce şimdi ortak bazı terimler ve bileşenleri tartışın. Bu tartışma azure'da bağlantıyı sağlayan uçtan uca zincirindeki her bileşenle ilgili düşündüğünüzü sağlar.
[![1]][1]

Yüksek düzeyde, ı üç ana ağ yönlendirme etki alanı açıklamak;

- Azure ağı (sağdaki mavi bulut)
- Internet veya WAN (yeşil bulutta Merkezi)
- Şirket ağına (soldaki açık bulut)

Sağdan sola diyagramın bakarak, şimdi her bileşenin kısaca açıklanmaktadır:
 - **Sanal makine** - birden çok NIC herhangi bir statik yollar, varsayılan yollar emin olun, sunucuda yüklü olmayabilir ve işletim sistemi ayarlarını gönderiyorsunuz ve trafik, düşündüğünüz biçimini alan. Ayrıca, her sanal makine SKU'su bant genişliğini kısıtlama vardır. Daha küçük bir VM SKU kullanıyorsanız, trafik NIC'ye kullanılabilir bant genişliğini sınırlıdır Genellikle bir DS5v2 test (ve daha sonra paradan tasarruf etmek için test etme ile bir kez yapılır delete) için kullandığım VM konumunda yeterli bant genişliğini sağlamak için.
 - **NIC** -söz konusu NIC atanmış özel IP bildiğiniz emin olun.
 - **NIC NSG** - var olması NIC düzeyinde uygulanan belirli Nsg'ler, NSG kural kümesine geçirmeye çalıştığınız trafiği için uygun olduğundan emin olun. Örneğin, RDP için 3389 numaralı bağlantı noktalarını 5201 iPerf, emin olun veya SSH için 22 test trafiğinin geçmesine izin vermek için açık değil.
 - **Sanal ağ alt ağı** -NIC, belirli bir alt ağa atanır, hangi tek ve kuralları bu alt ağ ile ilişkili bildiğiniz emin olun.
 - **Alt ağ NSG'SİNDE** - Nsg, NIC'ye alt yalnızca uygulanabilir gibi. NSG kural kümesi, geçirmeye çalıştığınız trafiği için uygun olduğundan emin olun. (NSG ilk geçerli alt ağ ve NIC NSG'nin NIC'ye gelen trafik için buna karşılık VM'den giden trafik için NIC NSG uygular sonra alt ağın NSG dönüştürülerek).
 - **Alt ağ UDR** -kullanıcı tanımlı yollar (örneğin, bir güvenlik duvarı veya yük dengeleyici) bir ara atlama trafiği doğrudan. Bir UDR'de trafiğiniz için ve bu nedenle nereye, sonraki atlama trafiğiniz için ne yapacağını, olup olmadığını anlamak emin olun. (örneğin, bir güvenlik duvarı bazı trafik geçirebilir ve aynı iki konak arasında diğer trafiği reddetmeye).
 - **Ağ geçidi alt ağı / NSG / UDR** -yalnızca VM alt ağı gibi ağ geçidi alt ağı, Nsg ve Udr'ler olabilir. Bunlar vardır ve hangi bunlar trafiğiniz etkileri bildiğinizden emin olun.
 - **VNet ağ geçidi (ExpressRoute)** -eşleme (ExpressRoute) ya da VPN etkinleştirildikten sonra etkileyebilir birçok ayarı yok nasıl veya yolları trafiği. Birden çok ExpressRoute bağlantı hatları veya bağlı aynı VNet ağ geçidinin VPN tünelleri varsa, bu ayar etkiler bağlantı tercih bağlantı ağırlığı ayarları bilmeniz gerekir ve trafiğiniz alan yolu etkiler.
 - **Rota filtresi** (gösterilmez) - rota Filtresi Microsoft Peering ExpressRoute üzerinde geçerlidir, ancak, beklediğiniz yolları üzerinde Microsoft Peering görmediğiniz olmadığını denetlemek için önemlidir. 

Bu noktada, WAN bağlantısı bölümü temel demektir. Bu yönlendirme etki alanı, hizmet sağlayıcınıza, Kurumsal WAN'ınız veya Internet olabilir. Birçok atlama, teknolojileri ve şirketler bu bağlantıları ile ilgili sorun gidermek biraz zor hale getirir. Genellikle, hem Azure hem de şirket ağlarınızı kullanıma kuralın ilk şirketler ve atlama bu koleksiyona atlama önce çalışırsınız.

Yukarıdaki diyagramda, en solda, Kurumsal ağdır. Şirketinizin boyutuna bağlı olarak, bu yönlendirme etki alanı bazı ağ aygıtları ve WAN veya çok katmanlı bir kampüs/kurumsal ağdaki cihazları arasında olabilir.

Bu üç farklı üst düzey ağ ortamları karmaşıklığını göz önünde bulundurulduğunda, bu genellikle kenarlarını başlatmak için en iyi olduğunu ve performansı iyi olduğu ve burada düşürür göstermek deneyin. Bu yaklaşım, sorun, üç yönlendirme etki alanını tanımlamak ve ardından sorun gidermeyi, belirli bir ortamda odak yardımcı olabilir.

## <a name="tools"></a>Araçlar
Birçok ağ sorununu çözümlenebilir ve ping ve traceroute gibi temel araçları kullanarak yalıtılmış. Wireshark gibi bir paket analiz olarak derine gidin gerektiğini nadir olarak rastlanıyor. Azure Connectivity Toolkit (AzureCT) gidermeye yardımcı olması için bu araçlardan bazılarını bir kolayca paketinde koymak için geliştirilmiştir. Performans testi için iPerf ve PSPing kullanmak istiyorum. iPerf yaygın olarak kullanılan bir araçtır ve çoğu işletim sistemlerinde çalışır. iPerf temel performans testleri için geçerlidir ve kullanımı oldukça kolaydır. PSPing SysInternals tarafından geliştirilen bir ping aracıdır. PSPing kullanımı da kolaydır tek bir komutla ICMP ve TCP gerçekleştirmek için kolay bir yoludur. Bu araçlar her ikisi de basit olduğundan ve "yalnızca bir dizine dosyaları konaktaki artıştan tarafından yüklenen".

Ben tüm bu araçları ve yöntemleri yükleyin ve kullanabileceğiniz bir PowerShell modülünü (AzureCT) Sarmalanan.

### <a name="azurect---the-azure-connectivity-toolkit"></a>AzureCT - Azure bağlantısı Araç Seti
AzureCT PowerShell modülünün iki bileşenden oluşur [kullanılabilirlik testi] [ Availability Doc] ve [performans testi][Performance Doc]. Bu belgede yalnızca performans testi ile ilgili, bu nedenle bu PowerShell modülünü iki bağlantı performans komutlar odaklanmanıza olanak tanır.

Performans testi için bu araç seti kullanmak için üç temel adım vardır. (1) PowerShell modülünü yükleyin, 2) destekleyen uygulamalar iPerf ve PSPing 3 yükleyin) performans testini çalıştırın.

1. PowerShell modülünü yükleme

    ```powershell
    (new-object Net.WebClient).DownloadString("https://aka.ms/AzureCT") | Invoke-Expression
    
    ```

    Bu komut, PowerShell modülünü yükler ve yerel olarak yükler.

2. Destekleyici uygulamalar yükleme
    ```powershell
    Install-LinkPerformance
    ```
    Bu AzureCT komut yeni bir dizinde "C:\ACTTools" iPerf ve PSPing yükler, ayrıca ICMP'ye izin ve 5201 (iPerf) trafiği bağlantı noktası için Windows Güvenlik Duvarı bağlantı noktalarını açar.

3. Performans testini çalıştırma

    İlk olarak, uzak ana bilgisayarda yüklemeli ve iPerf sunucu modunda çalıştırın. Ayrıca uzak konak ya da 3389 (RDP için Windows) dinliyor veya 22 (Linux için SSH) ve trafiğe izin vererek 5201 iPerf için bağlantı noktası emin olun. Windows uzak ana bilgisayar ise AzureCT yükleyin ve iPerf ve iPerf başarıyla sunucu modunda başlatmak için gereken güvenlik duvarı kurallarını ayarlamak için yükleme LinkPerformance komutunu çalıştırın. 
    
    Uzak makinenin hazır olduktan sonra yerel makinede PowerShell'i açın ve Testi Başlat:
    ```powershell
    Get-LinkPerformance -RemoteHost 10.0.0.1 -TestSeconds 10
    ```

    Bu komut bir dizi eşzamanlı yük ve bant genişliği kapasitesi tahmin etmenize yardımcı olmaya testleri gecikme süresi ve gecikme süresi, ağ bağlantısının çalıştırır.

4. Test çıkışını gözden geçirin

    PowerShell çıktı biçimini benzer şekilde görünür:

    [![4]][4]

    Bireysel metin dosyalarında "C:\ACTTools." AzureCT araçları dizininde bulunan tüm iPerf ve PSPing testler ayrıntılı sonuçlarını olan

## <a name="troubleshooting"></a>Sorun giderme
Performans testi değil vererek, beklenen sonuçları nedenini anlamak, aşamalı bir işlemi adım adım olmalıdır. Bileşen sayısı için belirtilen yol içinde sistematik bir yaklaşım genellikle etrafında atlama ve gereksiz yere potansiyel olarak birden çok kez aynı testi yapmadan çözümleme daha hızlı bir yolunu sağlar.

>[!NOTE]
>Burada bir performans sorunu, bir bağlantı sorunu senaryodur. Adımları, trafiği tüm geçirilirse değildi farklı olacaktır.
>
>

İlk olarak, bir varsayım meydan okuyun. Makul beklenir? Örneğin, bir 1 Gbps ExpressRoute bağlantı hattı ve 100 ms gecikme süresine sahip tam olarak 1 GB/sn TCP performans özellikleri yüksek gecikme bağlantılarda verilen trafik beklediğiniz mantıksız olabilir. Bkz: [başvurduğu bölüm](#references) üzerinde daha fazla performans varsayımlar için.

Ardından, Yönlendirme etki alanları arasında kenarlardaki başlamanızı öneririz ve tek bir ana Yönlendirme etki alanı için sorunu yalıtana deneyin; Şirket ağına, WAN veya Azure ağı. Özellikle sorun aslında bir alanda değişiklik olanağına sahip olduğunda, siyah kutu blaming çalışırken "siyah kutu" yolunda sorumluyu yapmak kolaydır insanlar genellikle, önemli ölçüde çözümleme gecikmesine neden. Kapatmak için service provider veya ISS'nize teslim etmeden önce aksaklıkla yaptığınızdan emin olun.

Sorun var gibi görünüyor ana Yönlendirme etki alanı belirledikten sonra söz konusu alanı diyagramı oluşturmalısınız. Bir pano, Not Defteri veya Visio bir diyagram üzerinde sağlayan bir somut "Savaşı Haritası" daha fazla ayırma sistemli bir yaklaşım sorun izin vermek için. Test noktaları planlama ve alanları veya test ilerledikçe daha derine inin Temizle eşlemeyi güncelleştirin.

Bir diyagram olduğuna göre ağ parçalara bölmek ve sorunu daraltmak başlatın. Burada değil ve nerede çalıştığını öğrenin. Aşağı soruna neden olan bileşen yalıtmak için testi noktalarınızı taşıma tutun.

Ayrıca, diğer katmanına OSI modeli unutmayın. Ağ ve Katman 1-3 (ağ fiziksel ve veri katmanları) odaklanan kolaydır ancak sorunları da yukarı 7. katmanda uygulama katmanında olabilir. Açık bir göz önünde bulundurun ve varsayımlar doğrulayın.

## <a name="advanced-expressroute-troubleshooting"></a>ExpressRoute Gelişmiş sorun giderme
Edge bulutun aslında olduğu emin değilseniz Azure bileşenlerini ayırma zor olabilir. ExpressRoute kullanıldığında, uç cihazlarında Microsoft Enterprise Edge (MSEE) adlı bir ağ bileşenidir. **Expressroute'u kullanırken**, MSEE ilk Microsoft'un ağ ve son atlama Microsoft network bırakarak iletişim noktasıdır. Sanal ağ geçidiniz ile ExpressRoute bağlantı hattı arasında bir bağlantı nesnesi oluşturduğunuzda, aslında MSEE için bir bağlantı yapılıyor. Azure'da sorun olduğunu kanıtlamak veya WAN veya kurumsal ağ içinde aşağı yönde daha ilk veya son atlama (hangi yöne bağlı olarak, yedekleyeceksiniz) ya da Azure ağ sorunları yalıtmak için önemli olduğundan MSEE tanıma. 

[![2]][2]

>[!NOTE]
> MSEE Azure bulutunda olmadığına dikkat edin. ExpressRoute, Microsoft ağının gerçekten azure'da aslında altındadır. Bir MSEE için ExpressRoute ile bağlandıktan sonra Microsoft'un ağa bağlı olduğunuzdan, buradan sonra (Microsoft Peering ile) Office 365 veya Azure (ile özel ve/veya Microsoft Peering) gibi bulut hizmetlerinden herhangi birine gidebilirsiniz.
>
>

İki sanal ağ (Vnet A ve B diyagramdaki) bağlıysa **aynı** ExpressRoute bağlantı hattı, bir dizi azure'da sorunu yalıtana (veya Azure'da değil kanıtlamak için) test gerçekleştirebilirsiniz
 
### <a name="test-plan"></a>Test planı
1. Get-LinkPerformance test VM2 VM1 arasında çalıştırın. Sorunu yerel değilse veya bu test için öngörü sağlar. Bu test, kabul edilebilir gecikme süresi ve bant genişliği sonuçları oluşturursa, yerel VNet ağ iyi işaretleyebilirsiniz.
2. Yerel Vnet'e varsayarak trafiği VM1 ve VM3 arasında Get-LinkPerformance test çalıştırması, uygundur. Bu test bağlantı MSEE aşağı ve azure'a geri Microsoft ağı üzerinden uygular. Bu test, kabul edilebilir gecikme süresi ve bant genişliği sonuçları oluşturursa, Azure ağı iyi işaretleyebilirsiniz.
3. Azure çizgili, testleri benzer bir dizi Kurumsal ağınızda gerçekleştirebilirsiniz. Ayrıca iyi testlerin, WAN bağlantınızı tanılamak için için service provider veya ISS'nize çalışma zamanı olur. Örnek: Bu test masanızın ve bir veri merkezi sunucusu arasında veya iki şube ofisiniz arasında çalıştırın. Hangi test ettiğiniz bağlı olarak, uç noktaları bulun (sunucular, bilgisayarlar, vb.) yol alıştırma.

>[!IMPORTANT]
> Her test için test çalıştırması günün saatini işaretlemek ve sonuçları (ben OneNote veya Excel gibi), ortak bir konumda olduğunu önemlidir. Her test çalışması, test çalıştırmaları arasında sonuç verileri karşılaştırmak ve verileri "açıklarına" yok aynı çıktı olması gerekir. Birden çok test arasında tutarlılığı sorun giderme için AzureCT kullanmam birincil nedenidir. Magic çalıştırabilir, ancak bunun yerine tam yük senaryolarda değil *Sihirli* alabilirim yükselten bir *tutarlı test ve veri çıkışı* her test. Zaman kaydetme ve tutarlı veri sahip tek her zaman daha sonra sorun ara sıra olduğunu fark ederseniz özellikle yardımcı olur. İle veri toplama Önden dikkatli ve (sabit böylece birçok yıl önce öğrendim) aynı senaryoları çözülüp saatler önlenir.
>
>

## <a name="the-problem-is-isolated-now-what"></a>Sorun yalıtılır şimdi ne?
Daha fazla, sorunu düzeltmek için sorun giderme konusunda daha ayrıntılı veya başka nereye olamaz noktası ulaşana ancak genellikle daha kolay olan ayırabilirsiniz. Örnek: hizmet sağlayıcınız aracılığıyla Avrupa atlama alma arasında bağlantıya bakın, ancak tüm Asya'da, beklenen yoludur. Yardım için ulaşın, bu noktasıdır. Kimin, sorun bağımlı soruna yalıtılmış bir yönlendirme etki alanı veya belirli bir bileşeni aşağı daraltmak tamamlayabilirseniz daha iyi olur.

Kurumsal ağ sorunları için iç BT departmanı veya hizmet sağlayıcısı ağınıza (aynı donanım üreticisi olabilir) destekleyen cihaz yapılandırması veya donanım Onarım yardımcı olabilir.

WAN için test sonuçlarınız, hizmet sağlayıcısı veya ISS paylaşma, başlatılan alın ve bazı test ettikten aynı yerden kapsayan önlemek yardımcı olabilir. Bunlar, sonuçları doğrulamak istiyorsanız ancak bilgisayar yok. "Güven ancak doğrulama" sorun giderme başkalarının bildirilen sonuçlarına göre iyi bir slogan olur.

Mümkün olduğunca çok ayrıntı sorunu izole sonra Azure ile gözden geçirmek için zamanı [Azure ağ belgeleri] [ Network Docs] ve sonra hala gerekirse [bir destek bileti oluşturarak][Ticket Link].

## <a name="references"></a>Başvurular
### <a name="latencybandwidth-expectations"></a>Gecikme süresi/bant genişliği beklentileri
>[!TIP]
> Test ettiğiniz uç noktaları arasındaki coğrafi gecikme (mil veya kilometre) şu ana kadar en büyük gecikme bileşenidir. İlgili donanım gecikme süresi (fiziksel ve sanal bileşenleri, atlama vb. sayısı) olsa Coğrafya toplam gecikme süresi en büyük bileşeninin WAN bağlantılarıyla ilgilenirken olmasını kanıtlanmıştır. Uzaklık yok doğrusal çalıştırma fiber uzaklığı veya yol haritası uzaklık olduğuna dikkat edin önemlidir. Bu uzaklığı ile bir doğruluk almak son derece zordur. Sonuç olarak, miyim genel olarak internet'te bir şehir uzaklık hesaplayıcısını kullanın ve bu yöntem grossly yanlış bir ölçüdür ama genel beklentisi ayarlamak için yeterlidir.
>
>

Seattle, Washington ABD'deki bir ExpressRoute Kurulum aldım. Aşağıdaki tablo, gecikme süresini gösterir ve bant genişliği için çeşitli Azure konumları sınama olan gördüm. Her bir testin sonu arasındaki coğrafi uzaklık tahmin edilen.

Test Kurulumu:
 - 10 ile Windows Server 2016 çalıştıran bir fiziksel sunucu GB/sn NIC, bir ExpressRoute bağlantı hattına bağlı.
 - 10 GB/sn Premium ExpressRoute bağlantı hattı özel eşdüzey hizmet sağlama etkinleştirilmiş ile belirtilen konumda.
 - Belirtilen bölgede bir UltraPerformance ağ geçidi ile Azure sanal ağı.
 - VNet üzerinde Windows Server 2016 çalıştıran bir DS5v2 VM. VM katılmış varsayılandan (iyileştirme veya özelleştirme), Azure görüntü yüklü AzureCT ile oluşturulan etki alanı dışı.
 - Tüm test AzureCT Get-LinkPerformance komut 5 dakikalık yük testi ile her altı test çalıştırması için kullanıyordu. Örneğin:

    ```powershell
    Get-LinkPerformance -RemoteHost 10.0.0.1 -TestSeconds 300
    ```
 - Her test için veri akışı (iPerf sunucu listelenen bir Azure bölgesinde) Azure VM kadar şirket içi fiziksel sunucu (Seattle'ndaki iPerf istemcisi) akışını yük vardı.
 - "Gecikme süresi" sütun verileri Hayır yük testi (TCP gecikme olmadan bir test çalıştırma iPerf) arasındadır.
 - "En fazla bant genişliği" sütun verileri 1 MB'lık pencere boyutu olan 16 TCP akışı yük testi arasındadır.

[![3]][3]

### <a name="latencybandwidth-results"></a>Gecikme süresi/bant genişliği sonuçları
>[!IMPORTANT]
> Bu yalnızca genel başvuru sayılardır. Gecikme süresi faktörlerden etkiler ve zaman içinde bu değerleri genellikle tutarlı olmasına rağmen Azure veya hizmet sağlayıcılarının ağ koşulları, herhangi bir zamanda farklı yollar aracılığıyla trafiği gönderebilirsiniz, böylece gecikme süresi ve bant genişliği etkilenebilir. Genellikle, bu değişikliklerin etkisini önemli farklılıklar neden yoktur.
>
>

| | | | | | |
|-|-|-|-|-|-|
|ExpressRoute<br/>Location|Azure<br/>Bölge|Tahmini<br/>Uzaklık (km)|Gecikme süresi|1 oturumu<br/>Bant genişliği|Maksimum<br/>Bant genişliği|
| Seattle | Batı ABD 2        |    191 km |   5 ms | 262.0 Mbits/sn |  3.74 Gbits/sn |
| Seattle | Batı ABD          |  1,094 km |  18 ms |  82.3 Mbits/sn |  3.70 Gbits/sn |
| Seattle | Orta ABD       |  2,357 km |  40 ms |  38.8 Mbits/sn |  2.55 Gbits/sn |
| Seattle | Orta Güney ABD |  2,877 km |  51 ms |  30.6 Mbits/sn |  2.49 Gbits/sn |
| Seattle | Orta Kuzey ABD |  2,792 km |  55 ms |  27,7 Mbits/sn |  2.19 Gbits/sn |
| Seattle | Doğu ABD 2        |  3,769 km |  73 ms |  21.3 Mbits/sn |  1.79 Gbits/sn |
| Seattle | Doğu ABD          |  3,699 km |  74 ms |  21.1 Mbits/sn |  1.78 Gbits/sn |
| Seattle | Japonya Doğu       |  7,705 km | 106 ms |  14,6 Mbits/sn |  1.22 Gbits/sn |
| Seattle | Birleşik Krallık Güney         |  7,708 km | 146 ms |  10.6 Mbits/sn |   896 Mbits/sn |
| Seattle | Batı Avrupa      |  7,834 km | 153 ms |  10.2 Mbits/sn |   761 Mbits/sn |
| Seattle | Avustralya Doğu   | 12,484 km | 165 ms |   9.4 sürümünden Mbits/sn |   794 Mbits/sn |
| Seattle | Güneydoğu Asya   | 12,989 km | 170 ms |   9.2 Mbits/sn |   756 Mbits/sn |
| Seattle | Brezilya Güney *   | 10,930 km | 189 ms |   8.2 Mbits/sn |   699 Mbits/sn |
| Seattle | Güney Hindistan      | 12,918 km | 202 ms |   7.7 Mbits/sn |   634 Mbits/sn |

\* Brezilya gecikme süresini burada doğrusal uzaklık uzaklık çalıştırma fiber önemli ölçüde farklı iyi bir örnektir. Gecikme süresini 160 MS Komşuları'olacaktır, ancak gerçekte 189 ms olduğunu beklediğiniz gibi. Bu fark karşı Bilgisayarım beklentisi yere bir ağ sorunu olduğunu gösteriyor olabilir, ancak büyük olasılıkla otomatik olarak fiber çalıştığını ve Brezilya için düz bir çizgi geçmez ek bir 1.000 km veya seyahat için Brezilya Seattle'dan almak için bunu.

## <a name="next-steps"></a>Sonraki adımlar
1. Azure bağlantısı Araç Seti adresindeki github'dan indirin [https://aka.ms/AzCT][ACT]
2. Yönergelerini izleyin [performans testi bağlantı][Performance Doc]

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-network-performance/network-components.png "azure ağ bileşenleri"
[2]: ./media/expressroute-troubleshooting-network-performance/expressroute-troubleshooting.png "ExpressRoute sorun giderme"
[3]: ./media/expressroute-troubleshooting-network-performance/test-diagram.png "Performans Test Ortamı"
[4]: ./media/expressroute-troubleshooting-network-performance/powershell-output.png "PowerShell çıkışı"

<!--Link References-->
[Performance Doc]: https://github.com/Azure/NetworkMonitoring/blob/master/AzureCT/PerformanceTesting.md
[Availability Doc]: https://github.com/Azure/NetworkMonitoring/blob/master/AzureCT/AvailabilityTesting.md
[Network Docs]: https://docs.microsoft.com/azure/index
[Ticket Link]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview
[ACT]: https://aka.ms/AzCT
