---
title: Azure sanal ağı performans sorunlarını giderme | Microsoft Docs
description: Bu sayfa, Azure ağ bağlantı performansını test etme için standartlaştırılmış bir yöntem sağlar.
services: expressroute
documentationcenter: na
author: tracsman
manager: rossort
editor: ''
ms.assetid: ''
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/20/2017
ms.author: jonor
ms.openlocfilehash: 56f011632a2aa3ef0632efd5ace472c0fc79a329
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
ms.locfileid: "27319263"
---
# <a name="troubleshooting-network-performance"></a>Ağ performansı sorunlarını giderme
## <a name="overview"></a>Genel Bakış
Azure, şirket içi ağınızdan Azure'a bağlanmak için kararlı ve hızlı yollar sağlar. Siteden siteye VPN yöntemleri gibi ve ExpressRoute başarıyla büyük ve küçük müşteriler tarafından Azure'da işlerini çalıştırmak için kullanılır. Ancak performans Beklenti veya önceki deneyimi karşılamıyor ne olur? Bu belge, test yolu ve temel ortamınıza özgü standartlaştırmak yardımcı olabilir.

Bu belge nasıl kolayca ve sürekli olarak ağ gecikme süresi ve bant genişliği iki ana bilgisayarlar arasında test edebilirsiniz gösterir. Bu belge, Azure ağı bakın ve sorunu noktaları yalıtmak için Yardım yolları bazı öneriler de sağlar. PowerShell Betiği ve açıklanan araçları iki ağdaki ana bilgisayarlara (her iki ucundaki sınanan bağlantı) gerektirir. Diğer Windows ya da Linux olması, bir ana bilgisayar bir Windows Server veya Masaüstü olması gerekir. 

>[!NOTE]
>Sorun giderme yaklaşım, Araçlar ve kullanılan yöntemleri kişisel tercihlerdir. Bu belge, genellikle ele yaklaşım ve araçları açıklanmaktadır. Durum yaklaşımınızı büyük olasılıkla farklılık gösterir, sorunu çözmek için farklı yaklaşımlara ile yanlış bir şey yok. Ancak, yerleşik bir yaklaşım yoksa, bu belgede yöntemlerinizi, araçları ve ağ sorunlarını giderme için Tercihler oluşturmaya yolunda başlamanıza yardımcı.
>
>

## <a name="network-components"></a>Ağ bileşenleri
Sorun giderme içine sorunda önce şimdi bazı ortak hüküm ve bileşenleri tartışın. Bu tartışma Azure bağlantısı sağlayan uçtan uca zincirindeki her bir bileşen hakkında düşündüğünüzü sağlar.
[![1]][1]

Yüksek düzeyde, ı üç ana ağ yönlendirme etki alanları açıklar;

- Azure ağı (sağdaki mavi bulut)
- Internet veya WAN (Merkezi'ndeki yeşil bulut)
- Şirket ağı (sol turuncu bulut)

Sağdan sola diyagramın baktığınızda, şimdi kısaca her bileşenin ele alınmıştır:
 - **Sanal makine** - sunucunun birden çok NIC, herhangi bir statik yollar, varsayılan yolların emin olabilir ve işletim sistemi ayarlarını gönderiyorsunuz ve onu düşündüğünüz yolu trafiğini alma. Ayrıca, her VM SKU bir bant genişliği sınırlaması vardır. Daha küçük bir VM SKU kullanıyorsanız, trafiğinizin NIC'ye kullanılabilir bant genişliği sınırlıdır Genellikle bir DS5v2 test etme (ve ardından paradan tasarruf testi ile bir kez yapılır Sil) kullanmam VM yeterli bant emin olmak için.
 - **NIC** -söz konusu NIC'ye atanan özel IP bildiğinizden emin olun.
 - **NIC NSG** - var olabilir NIC düzeyinde uygulanan belirli Nsg'ler olması, NSG kural kümesi geçirmeye çalıştığınız trafiği için uygun olduğundan emin olun. Örneğin, RDP 3389 numaralı bağlantı noktalarını 5201 iPerf, emin olun veya SSH için 22 test trafiğinin geçmesine izin vermek için açık.
 - **Sanal alt** -NIC, belirli bir alt ağa atanır, hangi tek ve kuralları bu alt ağ ile ilişkilendirilmiş bildiğiniz emin olun.
 - **Alt ağ NSG** - NIC, Nsg'ler alt yalnızca uygulanabilir gibi. NSG kural kümesi geçirmeye çalıştığınız trafiği için uygun olduğundan emin olun. (NSG ilk uygulandığı alt ağı, sonra NIC NSG NIC'ye gelen trafik için buna karşılık sanal makineden giden trafik için NIC NSG uygulanır ardından alt ağ NSG oyuna gelir).
 - **Alt ağ UDR** -kullanıcı tanımlı yollar, bir ara atlama (örneğin, bir Güvenlik Duvarı'nı veya yük dengeleyici) trafiği yönlendirebilir. UDR trafiğinizi yer ve bu nedenle nereye gidiyor bu sonraki atlama trafiğinizi için ne yapacağını durumunda olup olmadığını bilmek emin olun. (örneğin, bir güvenlik duvarı bazı trafiği geçiş ve aynı iki ana bilgisayarlar arasında diğer trafik reddetme).
 - **Ağ geçidi alt ağı / NSG / UDR** -yalnızca VM alt ağı gibi ağ geçidi alt ağı Nsg'ler ve Udr'ler olabilir. Bunlar vardır ve hangi bunlar trafiğinizi etkileri bildiğinizden emin olun.
 - **Sanal ağ geçidi (ExpressRoute)** -eşleme (ExpressRoute) veya VPN etkinleştirildikten sonra etkileyebilir birçok ayar yok nasıl veya yolları trafiği. Birden çok ExpressRoute bağlantı hatları veya VPN tünelleri aynı sanal ağ geçidine bağlı varsa, bu ayar etkiler bağlantı tercih bağlantı ağırlık ayarlarını bilmeniz gerekir ve trafiğinizi alan yolu etkiler.
 - **Filtre rota** (gösterilmez) - bir rota filtre yalnızca ExpressRoute üzerinde Microsoft Peering uygular, ancak, beklediğiniz yollar Microsoft Peering görmüyorsanız, denetlemek için önemlidir. 

Bu noktada, bağlantının WAN bölümü üzerinde demektir. Bu yönlendirme etki alanı, hizmet sağlayıcınızı, Kurumsal WAN'ınız veya Internet olabilir. Birçok atlama, teknolojileri ve şirketler bu bağlantıları ile ilgili sorun giderme biraz zor yapabilirsiniz. Genellikle, hem Azure hem de şirket ağlarına çıkışı kuralın ilk şirket ve atlama bu koleksiyona atlama önce çalışır.

Önceki diyagramda solundaki üzerinde şirket ağınıza ' dir. Şirketinizin boyutuna bağlı olarak, bu yönlendirme etki alanı birkaç ağ aygıtları ve WAN veya birden çok katman kampüs/kuruluş ağındaki aygıtların arasında olabilir.

Bu üç farklı üst düzey ağ ortamları karmaşıklığını verildiğinde, bu genellikle kenarlarında başlatmak için en iyi ve performans iyi olduğu ve burada düşürür göstermek deneyin. Bu yaklaşım, üç sorunu Yönlendirme etki alanını tanımlamak ve bu belirli ortamda sorun gidermeyi odaklanmanıza yardımcı olabilir.

## <a name="tools"></a>Araçlar
Birçok ağ sorununu çözümlenebilir ve ping ve izleme yolu gibi temel araçlar kullanarak yalıtılmış. Nadir olarak bir paket analiz Wireshark gibi ayrıntılı gitmeniz gerekir. Sorun giderme konusunda yardımcı olmak için Azure bağlantısını Araç Seti (AzureCT) Bu araçlardan bazıları kolay bir pakette koymak için geliştirilmiştir. Performans testi için ı iPerf ve PSPing kullanmak ister. iPerf yaygın olarak kullanılan bir araçtır ve çoğu işletim sistemleri üzerinde çalışır. iPerf temel performanslarını testler için uygundur ve kullanımı oldukça kolaydır. PSPing SysInternals tarafından geliştirilen bir ping aracıdır. PSPing tek bir kullanımı da kolay komutta ICMP ve TCP ping işlemlerini gerçekleştirmek için kolay bir yoludur. Bu araçların her ikisi de basit ve "sadece bir dizin dosyaları ana bilgisayardaki kopyalamayı tarafından yüklenen".

I tüm bu araçlar ve yöntemlerinin yükleyip kullanmak bir PowerShell modülüne (AzureCT) Sarmalanan.

### <a name="azurect---the-azure-connectivity-toolkit"></a>AzureCT - Azure bağlantısı Araç Seti
AzureCT PowerShell modülü iki bileşenden oluşur [kullanılabilirlik sınama] [ Availability Doc] ve [performans testi][Performance Doc]. Bu belgede yalnızca performans testi ile ilgili, bu nedenle bu PowerShell modülünü iki bağlantı performans komutlarda odaklanmanıza olanak tanır.

Bu araç seti performansını test etmek için kullanılacak üç temel adımdan oluşur. 1) PowerShell modülünü yükleyin, 2) destekleyen uygulamalar iPerf ve PSPing 3 yükleyin) performans testini çalıştırma.

1. PowerShell modülü yükleniyor

    ```powershell
    (new-object Net.WebClient).DownloadString("https://aka.ms/AzureCT") | Invoke-Expression
    
    ```

    Bu komut, PowerShell modülü indirilir ve yerel olarak yüklenir.

2. Destekleyen uygulamaları yükleme
    ```powershell
    Install-LinkPerformance
    ```
    Bu AzureCT komut yeni bir dizinde "C:\ACTTools" iPerf ve PSPing yükler, ayrıca ICMP izin vermek ve 5201 (iPerf) trafiği bağlantı noktası için Windows Güvenlik Duvarı bağlantı noktalarını açar.

3. Performans testi

    İlk olarak, uzak ana bilgisayarda yüklemeniz ve gerekir iPerf sunucu modunda çalıştırın. Ayrıca uzak ana bilgisayar ya da 3389 (Windows için RDP) dinliyor veya 22 (Linux için SSH) ve üzerinde trafiğe izin 5201 iPerf için bağlantı noktası emin olun. Uzak ana bilgisayarda windows ise, AzureCT yükleyin ve iPerf ve iPerf başarıyla sunucu modunda başlatmak için gereken güvenlik duvarı kuralları ayarlamak için yükleme LinkPerformance komutunu çalıştırın. 
    
    Uzak makineye hazır olduktan sonra yerel makinede PowerShell'i açın ve test başlatın:
    ```powershell
    Get-LinkPerformance -RemoteHost 10.0.0.1 -TestSeconds 10
    ```

    Bu komut, bir dizi eşzamanlı yük ve bant genişliği kapasitesi tahmin etmeye yardımcı olması için gecikme testleri ve ağ bağlantınızı gecikme çalıştırır.

4. Testleri çıkışını gözden geçirin

    PowerShell çıktı biçimi benzer şekilde görünür:

    [![4]][4]

    Tüm iPerf ve PSPing testleri ayrıntılı sonuçlarını "C:\ACTTools." AzureCT araçları dizininde bireysel metin dosyaları olan

## <a name="troubleshooting"></a>Sorun giderme
Performans testi değil vermiş, beklenen sonuçları nedenini anlamak, aşamalı bir adım adım işlemi olmalıdır. Bileşenlerin sayısını yolunda verilen, sistematik bir yaklaşım genellikle daha geçici atlama ve potansiyel olarak gereksiz yere birden çok kez aynı test bulunurken çözümleme daha hızlı bir yolunu sağlar.

>[!NOTE]
>Burada bir performans sorunu, bir bağlantı sorunu senaryodur. Adımları trafiği hiç geçirilirse değildi farklı olacaktır.
>
>

İlk olarak, varsayımları sınama. Beklenti makul mi? Örneğin, 1 GB/sn expressroute bağlantı hattı ve 100 ms gecikme varsa tam 1 GB/sn TCP performans özellikleri yüksek gecikme bağlantıları üzerinden verilen trafik beklediğiniz aşırı olur. Bkz: [başvuran bölüm](#references) üzerinde daha fazla performans varsayımlar için.

Ardından, Yönlendirme etki alanları arasında kenarları başlayarak önerilir ve tek bir ana Yönlendirme etki alanı için sorunu ayırt etmek deneyin; Şirket ağına, WAN veya Azure ağı. Özellikle sorun gerçekten bir alanda değişiklik olanağına sahip olduğunda, "siyah kutusuna" yol nedenlerle siyah kutuyu blaming sırasında yapmak kolaydır kişiler genellikle, bu önemli ölçüde çözümleme gecikmeye neden olabilir. Kapalı, servis sağlayıcınıza veya ISS teslim etmeden önce durum tespitlerini yaptığınızdan emin olun.

Sorun içeriyor ana Yönlendirme etki alanı tanımladıktan sonra alanın bir diyagram söz konusu oluşturmanız gerekir. Beyaz Tahta, Not Defteri veya Visio diyagram olarak ya da sağlar somut "var olma Savaşının harita" daha fazla ayırma sistemli bir yaklaşım sorun izin vermek için. Test noktaları planlama ve harita alanları veya test ilerledikçe daha derin dıg temizleyin gibi güncelleştirin.

Bir diyagram sahip olduğunuza göre ağ parçalara bölmek ve sorun Dar Aşağı başlatın. Burada çalışır ve burada değil öğrenin. Aşağıya doğru sorunlu bileşenini ayırmak için test noktalarınızı taşıma tutun.

Ayrıca, diğer OSI modeli katmanları bakmayı unutmayın. Ağ ve Katman 1-3 (fiziksel, veri ve ağ katmanları) odaklanmak kolaydır ancak sorunları da yukarı katman 7 uygulama katmanında olabilir. Açık bir unutmayın ve varsayımlar doğrulayın.

## <a name="advanced-expressroute-troubleshooting"></a>ExpressRoute Gelişmiş sorun giderme adımları
Bulut kenarına gerçekte olduğu emin değilseniz Azure bileşenleri yalıtma zor olabilir. ExpressRoute kullanıldığında, kenar Microsoft Enterprise Edge (MSEE) adlı bir ağ bileşenidir. **ExpressRoute kullanırken**, MSEE ilk Microsoft'un ağ ve Microsoft network bırakarak son atlama iletişim noktasıdır. Sanal ağ geçidiniz ile ExpressRoute devresi arasında bir bağlantı nesnesi oluşturduğunuzda, aslında MSEE için bir bağlantı değişiklik yapıyorsunuz. Azure'da sorundur kanıtlamak veya aşağı WAN veya şirket ağı içinde daha fazla (hangi yöne bağlı olarak, oluşturacağız) ilk veya son atlama ya da Azure ağ sorunları yalıtmak için çok önemli olduğu gibi MSEE tanıma. 

[![2]][2]

>[!NOTE]
> MSEE Azure bulutunda olmadığına dikkat edin. ExpressRoute azure'da aslında Microsoft ağının gerçekte altındadır. Bir MSEE için ExpressRoute ile bağlandıktan sonra Microsoft'un ağa bağlı değilseniz, buradan sonra Office 365 (ile Microsoft Peering) veya Azure (ile özel ve/veya Microsoft Peering) gibi bulut hizmetlerinden herhangi biri gidebilirsiniz.
>
>

İki sanal ağlar (Vnet'ler A ve B diyagramdaki) bağlıysanız **aynı** expressroute bağlantı hattı, bir dizi yalıtmak Azure (veya Azure'da değil kanıtlamak için) test gerçekleştirebilir
 
### <a name="test-plan"></a>Test planı
1. Get-LinkPerformance test VM1 ve VM2 arasında çalıştırın. Sorun veya yerel ise, bu test için değerli bilgiler sağlar. Bu test kabul edilebilir gecikme süresi ve bant genişliği sonuçları oluşturursa, yerel VNet ağ iyi işaretleyebilirsiniz.
2. Yerel VNet varsayılarak VM1 ve VM3 arasında Get-LinkPerformance testi iyi trafiğidir. Bu test MSEE aşağı kaydırın ve Azure uygulamasına geri Microsoft network üzerinden bağlantı uygular. Bu test kabul edilebilir gecikme süresi ve bant genişliği sonuçları oluşturursa, Azure ağını iyi işaretleyebilirsiniz.
3. Azure çizgili, şirket ağınıza testleri benzer bir dizi gerçekleştirebilirsiniz. Bu da iyi test, WAN bağlantısı tanılamak için hizmet sağlayıcınıza veya ISS çalışma zamanı geldi. Örnek: Bu test masanızın ve bir veri merkezi sunucusu arasında veya iki şube ofisiniz çalıştırın. Ne test ettiğiniz bağlı olarak, uç noktalarını bulma (sunucular, bilgisayarlar, vb.) yol alıştırma.

>[!IMPORTANT]
> Her test için test çalışmasını günün saatini işaretleyin ve sonuçları (t OneNote veya Excel gibi) bir ortak konuma kaydetmek, kritik öneme sahiptir. Test çalışmaları arasında sonuç verileri karşılaştırmak ve veri "delik" sahip değilse her test çalışması aynı çıktı olması gerekir. Birden çok sınama arasında tutarlılık AzureCT sorun giderme için kullandığım birincil nedenidir. Sihirli çalıştırabilirim, ancak bunun yerine tam yükleme senaryolarda değil *Sihirli* alıyorum gerçeğidir bir *tutarlı test ve veri çıkışı* her test. Zaman kaydetme ve tutarlı verileri tek her zaman sahip daha sonra sorun aralıklı olduğunu fark ederseniz özellikle yardımcı olur. İle veri toplama Önden dikkatli ve saat (ı sabit böylece birçok yıl önce öğrenilen) aynı senaryoları retesting, kaçının.
>
>

## <a name="the-problem-is-isolated-now-what"></a>Sorun yalıtılır şimdi ne?
Daha fazla sorunu düzeltmek için sorun giderme konusunda daha ayrıntılı ya da daha fazla nereye olamaz noktası ulaşmak ancak genellikle daha kolay olan ayırabilirsiniz. Örnek: Avrupa aracılığıyla atlama alma servis sağlayıcınız üzerinden bağlantıya bakın, ancak tüm Asya'da, beklenen yoludur. Bu noktaya yardımını ulaşmak zamandır. Kimin istenir sorunu yalıtılmış Yönlendirme etki alanı bağımlı veya belirli bir bileşen aşağıya doğru daraltmak tamamlayabilirseniz daha iyi değil.

Kurumsal ağ sorunları için iç BT departmanına ya da hizmet sağlayıcısı (olabilen donanım üreticisi), ağ destekleyen aygıt yapılandırması veya donanım onarımı yardımcı olmak mümkün olabilir.

WAN için hizmet sağlayıcısına veya ISS ile test sonuçlarınızı paylaşımı, bunları kullanmaya getirmek ve bazı test ettikten aynı taban kapsayan kaçının yardımcı olabilir. Bunlar, sonuçları doğrulamak istiyorsanız ancak, bilgisayar yok. Sorun giderme diğer kişilerin bildirilen sonuçlarına dayalı "Güven ancak doğrulayın" iyi slogan durumdur.

Yapabilir, olabildiğince çok ayrıntı sorunu yalıtmak sonra Azure ile gözden geçirmek için zamanı [Azure ağ belgeleri] [ Network Docs] ve ardından IF lazım [Destekbilet][Ticket Link].

## <a name="references"></a>Başvurular
### <a name="latencybandwidth-expectations"></a>Gecikme/bant genişliği beklentileri
>[!TIP]
> Test ettiğiniz uç noktaları arasındaki coğrafi gecikme (mil veya kilometre) göre şu ana kadar en büyük gecikme bileşenidir. Söz konusu ekipman gecikme (fiziksel ve sanal bileşenleri, atlama, vb. sayısı) olsa Coğrafya genel gecikme süresi en büyük bileşeninin WAN bağlantılarıyla ilgilenirken olduğunu kanıtlamış. Uzaklık değil doğrusal çalıştırmak fiber uzaklığını veya yol haritası uzaklığı olduğunu dikkate almak önemlidir. Bu uzaklık herhangi doğruluk ile almak son derece zordur. Sonuç olarak, t genellikle bir şehir uzaklığı hesaplayıcı Internet'te kullanın ve bu yöntem grossly yanlış ölçü ancak genel bir Beklenti ayarlamak için yeterli olduğunu biliyor.
>
>

Bir ExpressRoute Kurulum Seattle, Washington ABD'deki açıyor. Aşağıdaki tabloda gecikme süresini gösterir ve bant genişliği ı çeşitli Azure konumlara sınama gördünüz. Testin her bitiş arasındaki coğrafi uzaklığı tahmin.

Kurulum test edin:
 - Windows Server 2016 10 ile çalışan bir fiziksel sunucuda Gbps NIC, bir expressroute bağlantı hattı bağlı.
 - Etkin özel eşleme ile belirtilen konumda bir 10 GB/sn Premium expressroute bağlantı hattı.
 - Belirtilen bölgede bir UltraPerformance ağ geçidi ile bir Azure VNet.
 - Windows Server 2016 VNet üzerinde çalışan bir DS5v2 VM. VM birleştirilmiş, varsayılandan farklı Azure resmi (en iyi duruma getirme veya özelleştirme) yüklü AzureCT ile yerleşik etki alanı dışı.
 - Tüm test AzureCT Get-LinkPerformance komutu ile 5 dakikalık yük testi her altı test çalışmaları için kullanıyordu. Örneğin:

    ```powershell
    Get-LinkPerformance -RemoteHost 10.0.0.1 -TestSeconds 300
    ```
 - Her sınama için veri akışı (listelenen Azure bölgesinde iPerf sunucusu) Azure VM kadar şirket içi fiziksel sunucu (Seattle'ndaki iPerf istemcisi) akışını yük vardı.
 - "Gecikme" sütun veri Hayır yük testi (bir TCP gecikme test çalıştıran iPerf olmadan) değil.
 - "En fazla bant genişliği" sütun veri 1 Mb pencere boyutu olan 16 TCP akışı yük testi arasındadır.

[![3]][3]

### <a name="latencybandwidth-results"></a>Gecikme/bant genişliği sonuçları
>[!IMPORTANT]
> Bu yalnızca genel başvuru numaralarıdır. Gecikme pek çok etken etkiler ve zaman içinde bu değer genellikle tutarlı olmasına karşın, koşullar Azure veya hizmet sağlayıcılarının ağ içinde herhangi bir zamanda farklı yollar üzerinden trafik gönderebilir, böylece gecikme süresi ve bant genişliği etkilenebilir. Genellikle, bu değişikliklerin etkisini önemli farklılıklar neden yoktur.
>
>

| | | | | | |
|-|-|-|-|-|-|
|ExpressRoute<br/>Konum|Azure<br/>Bölge|Tahmini<br/>Uzaklık (km)|Gecikme süresi|1 oturumu<br/>Bant Genişliği|Maksimum<br/>Bant Genişliği|
| Seattle | Batı ABD 2        |    191 km |   5 ms | 262.0 Mbit/sn |  3.74 Gbits/sn | 21
| Seattle | Batı ABD          |  1,094 km |  18 ms |  82.3 Mbit/sn |  3.70 Gbits/sn | 20
| Seattle | Orta ABD       |  2,357 km |  40 ms |  38.8 Mbit/sn |  2.55 Gbits/sn | 17
| Seattle | Orta Güney ABD |  2,877 km |  51 ms |  30.6 Mbit/sn |  2.49 Gbits/sn | 19
| Seattle | Orta Kuzey ABD |  2,792 km |  55 ms |  27,7 Mbit/sn |  2.19 Gbits/sn | 18
| Seattle | Doğu ABD 2        |  3,769 km |  73 ms |  21.3 Mbit/sn |  1.79 Gbits/sn | 16
| Seattle | Doğu ABD          |  3,699 km |  74 ms |  21.1 Mbit/sn |  1.78 Gbits/sn | 15
| Seattle | Japonya Doğu       |  7,705 km | 106 ms |  14,6 Mbit/sn |  1.22 Gbits/sn | 28
| Seattle | Birleşik Krallık Güney         |  7,708 km | 146 ms |  10.6 Mbit/sn |   896 Mbit/sn | 24
| Seattle | Batı Avrupa      |  7,834 km | 153 ms |  10.2 Mbit/sn |   761 Mbit/sn | 23
| Seattle | Avustralya Doğu   | 12,484 km | 165 ms |   9,4 Mbit/sn |   794 Mbit/sn | 26
| Seattle | Güneydoğu Asya   | 12,989 km | 170 ms |   9.2 Mbit/sn |   756 Mbit/sn | 25
| Seattle | Brezilya Güney *   | 10,930 km | 189 ms |   8.2 Mbit/sn |   699 Mbit/sn | 22
| Seattle | Güney Hindistan      | 12,918 km | 202 ms |   7.7 Mbit/sn |   634 Mbit/sn | 27

\*Gecikme Brezilya için iyi olduğu doğrusal uzaklığı uzaklığı çalıştırmak fiber önemli ölçüde farklı bir örnektir. Gecikme 160 ms Komşuları'nda olacaktır, ancak gerçekte 189 ms olduğu beklemeniz. Bu fark my Beklenti karşı bir yerde bir ağ sorunu olduğunu gösteriyor olabilir, ancak büyük olasılıkla fiber çalıştığını ve Brezilya için bir çizgide geçmez fazladan 1.000 km veya seyahat için Brezilya İstanbul almak için bunu sahiptir.

## <a name="next-steps"></a>Sonraki adımlar
1. Azure bağlantı Araç Seti adresindeki github'dan indirin [http://aka.ms/AzCT][ACT]
2. Yönergeleri izleyin [performans testi bağlantı][Performance Doc]

<!--Image References-->
[1]: ./media/expressroute-troubleshooting-network-performance/network-components.png "azure ağ bileşenleri"
[2]: ./media/expressroute-troubleshooting-network-performance/expressroute-troubleshooting.png "ExpressRoute sorun giderme"
[3]: ./media/expressroute-troubleshooting-network-performance/test-diagram.png "performans sınama ortamı"
[4]: ./media/expressroute-troubleshooting-network-performance/powershell-output.png "PowerShell çıkışı"

<!--Link References-->
[Performance Doc]: https://github.com/Azure/NetworkMonitoring/blob/master/AzureCT/PerformanceTesting.md
[Availability Doc]: https://github.com/Azure/NetworkMonitoring/blob/master/AzureCT/AvailabilityTesting.md
[Network Docs]: https://docs.microsoft.com/azure/index#pivot=services&panel=network
[Ticket Link]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview
[ACT]: http://aka.ms/AzCT











