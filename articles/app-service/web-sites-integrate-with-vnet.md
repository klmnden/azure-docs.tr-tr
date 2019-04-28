---
title: Azure sanal ağ ile - uygulama tümleştirme Azure uygulama hizmeti
description: Azure uygulama Hizmeti'nde bir uygulamanın bir yeni veya var olan Azure sanal ağa bağlama işlemi gösterilmektedir
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 768179f8569eac14166bcbb0a888e1cdbe41d497
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62128424"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Uygulamanızı bir Azure sanal ağı ile tümleştirme
Bu belge, Azure App Service sanal ağ tümleştirme özelliğini açıklar ve uygulamalar ile ayarlama işlemi gösterilmektedir [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). [Azure sanal ağları] [ VNETOverview] (Vnet'ler) birçok Azure kaynaklarınızın bir internet olmayan routeable ağında yerleştirin izin verir. Bu ağlar VPN'si teknolojileri kullanarak, şirket içi ağlara bağlanabilirsiniz. 

Azure App Service iki biçimi vardır. 

1. Fiyatlandırma planı yalıtılmış dışında tam aralığını destekleyen çok kiracılı sistemleri
2. App Service ortamı (ağınızın içinde dağıtan ASE). 

Bu belge çok kiracılı App Service içinde kullanılmak üzere tasarlanmıştır VNet tümleştirme özelliğini geçer.  Uygulamanızı ise [App Service ortamı][ASEintro], bir sanal ağda zaten ve aynı sanal ağ kaynaklarına ulaşmak için VNet tümleştirmesi özelliğinin gerektirmez.

VNet tümleştirmesi, web uygulaması sanal ağınızdaki kaynaklara erişmenizi sağlar ancak özel web uygulamanıza sanal ağdan erişim yoktur. Özel site erişimi, uygulamanızı yalnızca özel ağdan gibi bir Azure sanal ağı içinde erişilebilir hale getirmek için ifade eder. Özel site erişimi yalnızca bir iç yük dengeleyici (ILB) ile yapılandırılmış bir ASE ile kullanılabilir. ILB ASE kullanma hakkında daha fazla ayrıntı için buraya makale ile başlayın: [Oluşturma ve ILB ASE kullanma][ILBASE]. 

VNet tümleştirmesi, genellikle bir veritabanlarına uygulamalardan erişime izin verin ve web Hizmetleri, sanal ağda çalışan için kullanılır. VNet Tümleştirmesi ile VM ancak can uygulamaları özel dışı Internet yönlendirilebilir adreslerini kullanmanız için genel bir uç nokta kullanıma sunmak gerekmez. 

VNet tümleştirmesi özelliği:

* Standart, Premium veya PremiumV2 fiyatlandırma planı gerektirir 
* Klasik veya Resource Manager sanal ağı ile çalışır 
* TCP ve UDP destekler
* Web, mobil, API uygulamalarıyla çalışır ve işlev uygulamaları
* aynı anda yalnızca 1 sanal ağa bağlanmak bir uygulama sağlar
* bir App Service planında tümleştirilmesini ile beş adede kadar sanal ağlar sağlar 
* bir App Service planı içinde birden fazla uygulama tarafından kullanılmak üzere aynı sanal ağda izin verir
* Noktadan siteye VPN ile yapılandırılmış bir sanal ağ geçidi gerektirir
* ağ geçidi üzerinde nedeniyle SLA'sı % 99,9 SLA destekler

VNet tümleştirmesi dahil olmak üzere desteklemiyor bazı şeyler vardır:

* bir sürücü bağlama
* AD tümleştirmesi 
* NetBios
* Özel site erişimi
* ExpressRoute kaynaklara erişme 
* Hizmet uç noktaları arasında kaynaklara erişme 

### <a name="getting-started"></a>Başlarken
Aşağıda, web uygulamanızı bir sanal ağa bağlamadan önce göz önünde bulundurmanız gereken bazı hususlar verilmiştir:

* Hedef sanal ağ ile rota tabanlı ağ geçidi uygulamasına bağlı önce etkin noktadan siteye VPN olması gerekir. 
* Sanal ağ, App Service Plan(ASP) ile aynı abonelikte olması gerekir.
* Bir sanal ağ ile tümleştirilen uygulamalar, o VNet için belirtilen DNS kullanın.

## <a name="enabling-vnet-integration"></a>VNet tümleştirmesi etkinleştiriliyor

Yeni bir sürümü önizlemede olan VNet tümleştirme özelliği yoktur. Noktadan siteye VPN'yi bağımlı değildir ve kaynaklara erişmeyi ExpressRoute veya hizmet uç noktaları arasında da destekler. Yeni önizleme özelliği hakkında daha fazla bilgi edinmek için bu belgenin sonuna gidin. 

### <a name="set-up-a-gateway-in-your-vnet"></a>Sanal bir ağ geçidi ayarlama ###

Noktadan siteye adresleriyle yapılandırılmış bir ağ geçidi zaten varsa, sanal ağ Tümleştirmesi ile uygulamanızı yapılandırmaya atlayabilirsiniz.  
Bir ağ geçidi oluşturmak için:

1. [Bir ağ geçidi alt ağı oluşturmanız] [ creategatewaysubnet] sanal.  

1. [VPN ağ geçidi oluşturma][creategateway]. Bir rota tabanlı VPN türü seçin.

1. [Noktası site adresleri olarak ayarlanmış][setp2saddresses]. Ağ geçidi temel SKU'da değilse, IKEv2 noktadan siteye yapılandırmasında devre ve SSTP seçilmelidir. Adres alanı aşağıdaki adres bloklarını biri olmalıdır:

* 10.0.0.0/8 - Bu bir IP adres aralığından 10.0.0.0 10.255.255.255 için anlamına gelir
* Bir IP adres aralığından 172.16.0.0 172.31.255.255 için yani 172.16.0.0/12- 
* Bir IP adres aralığından 192.168.0.0 192.168.255.255 için yani 192.168.0.0/16-

Yalnızca varsa ağ geçidi oluşturma App Service ile VNet tümleştirmesi kullanın, ardından bir sertifikayı karşıya yüklemek gerekmez. Ağ geçidi oluşturma, 30 dakika sürebilir. Ağ geçidi sağlanana kadar uygulamanız, sanal ağ ile tümleştirmek mümkün olmayacaktır. 

### <a name="configure-vnet-integration-with-your-app"></a>VNet Tümleştirmesi ile uygulamanızı yapılandırın ###

VNet tümleştirmesi uygulamanızı etkinleştirmek için: 

1. Azure portalında uygulamanıza gidin ve uygulama ayarları'nı açın ve ağ seçin > VNet tümleştirmesi. ASP standart SKU için ölçeği veya VNet tümleştirmesi yapılandırabilmeniz için önce daha iyi. 
 ![VNet tümleştirmesi kullanıcı Arabirimi][1]

1. Seçin **VNet ekleme**. 
 ![VNet tümleştirmesi ekleme][2]

1. Sanal ağınıza seçin. 
  ![Sanal ağınıza seçin][8]
  
Uygulamanızın son Bu adımdan sonra yeniden başlatılır.  

## <a name="how-the-system-works"></a>Sistemin nasıl çalışır?
Sanal ağ tümleştirme özelliği, noktadan siteye VPN teknolojileri oluşturulmuştur. Azure App Service'te uygulamaları doğrudan bir sanal ağ içindeki bir uygulama sağlama önleyen bir çok kiracılı sisteminde barındırılır. Noktadan siteye teknoloji yalnızca uygulamayı barındıran sanal makine ağ erişimi sınırlar. Yalnızca trafik İnternet'e VNet tümleştirmesi veya karma bağlantılar üzerinden göndermek için kısıtlı uygulamalardır. 

![VNet tümleştirmesi nasıl çalışır?][3]

## <a name="managing-the-vnet-integrations"></a>VNet tümleştirmeler yönetme
Bağlanmak ve bir sanal ağa bağlantısını kesmek için bir uygulama düzeyinde yeteneğidir. Birden fazla uygulama arasında VNet tümleştirmesi etkileyen işlemleri App Service planı düzeyindedir. Uygulamadan UID, sanal ağınızda ayrıntılarını alabilirsiniz. Aynı bilgileri ASP düzeyinde da gösterilir. 

 ![Sanal ağ ayrıntıları][4]

Ağ özelliği durumu sayfasından uygulamanızı sanal ağınıza bağlı olup olmadığını görebilirsiniz. Sanal ağ geçidiniz için herhangi bir nedenle çalışmıyorsa, durumunuzu değil bağlı olarak gösterilir. 

Artık uygulama düzeyi VNet tümleştirmesi UI ASP'den alma hakkında ayrıntılı bilgi ile aynıdır, kullanılabilir olan bilgiler. Bu öğeler şunlardır:

* VNet adı - kullanıcı Arabirimi sanal ağa bağlantılar
* Konumu - konum, sanal ağınızın yansıtır. Başka bir konumda bir sanal ağ ile tümleştirme, uygulamanız için gecikme sorunlarına neden olabilir. 
* Sertifika durumu - sertifikalarınızı App Service planınızı ağınız arasında eşit olup olmadığını gösterir.
* Ağ geçidi durumu - ağ geçitlerinizi herhangi bir nedenle kapalı olmalıdır sonra uygulamanızı sanal ağ içindeki kaynaklara erişemez. 
* VNet adres alanının - sanal ağınıza ait IP adresi alanını gösterir. 
* Noktadan siteye adres alanı - sanal ağınıza ait site IP adresi alanı noktasına gösterir. Sanal ağınıza çağırıyor yaparken, uygulama Kimden adresinizi Bu adreslerden birini olacaktır. 
* Siteden siteye adres alanı - şirket içi kaynaklarınıza veya başka bir sanal ağ için sanal ağınıza bağlanmak için siteden siteye VPN'ler kullanabilirsiniz. Bu VPN bağlantısı ile tanımlanan IP aralıklarını burada gösterilir.
* DNS sunucuları - DNS sunucuları, VNet ile yapılandırılan gösterir.
* -Vnet'e yönlendirilmiş IP adresleri, Vnet'te sürücü trafiği için kullanılan adres bloklarını yönlendirilen gösterir 

VNet tümleştirmenizi uygulama görünümünü gerçekleştirebileceğiniz tek işlem uygulamanız şu anda bağlı olduğu sanal ağ bağlantısını kesmeyi sağlamaktır. Uygulamanızı bir sanal ağdan bağlantıyı kesmek için seçin **Bağlantıyı Kes**. Bir sanal ağdan ayırdığınızda, uygulamanızı yeniden başlatılır. Bağlantıyı kesme, sanal ağınıza değişikliğe neden olmaz. Sanal ağ ve ağ geçitleri de dahil olmak üzere yapılandırmasını değişmeden kalır. Ardından sanal ağınıza silmek istiyorsanız, önce ağ geçitleri de dahil olmak üzere içindeki kaynakları silmek gerekir. 

ASP VNet tümleştirmesi UI ulaşmak için ASP UI'ı açın ve seçin **ağ**.  VNet tümleştirmesi altında seçin **yapılandırmak için buraya tıklayın** ağ özelliği durumu kullanıcı arabirimini açın.

![ASP VNet tümleştirmesi bilgileri][5]

ASP VNet tümleştirmesi UI tüm ASP uygulamaları tarafından kullanılan sanal ağlar gösterilir. En fazla 5 sanal ağlar, App Service planınızda herhangi bir sayıda uygulama bağlı olabilir. Her uygulamanın tek tümleştirmesi yapılandırılmış olabilir. Her VNet üzerinde ayrıntıları görmek için ilgilendiğiniz sanal ağa tıklayın. Burada gerçekleştirebileceğiniz iki eylemler vardır.

* **Eşitleme ağ**. Eşitleme ağ işlemi sertifikaları ve ağ bilgilerini eşitlenmiş olduğundan emin olur. Eklediğinizde veya değiştirdiğinizde, sanal ağınızın DNS, gerçekleştirmeniz gereken bir **eşitleme ağ** işlemi. Bu işlem, bu sanal ağ kullanarak herhangi bir uygulamayı yeniden başlatır.
* **Yollar** yollar ekleme sürücü giden trafik, sanal ağ içinde.

**Yönlendirme** ağınızda tanımlanmış rotalar uygulamanızı ağınızdan içine trafiği yönlendirmek için kullanılır. Vnet'te ek giden trafiği göndermek gerekiyorsa, bu adres blokları ekleyebilirsiniz.   

**Sertifikaları** etkin olduğunda VNet tümleştirmesi, gerekli bir bağlantının güvenliğini sağlamak için sertifika alışverişi yoktur. Sertifikaları yanı sıra DNS yapılandırmasını, yollar ve ağ açıklayan diğer benzer bir şey var.
Sertifikaları veya ağ bilgileri değiştirilirse, "Ağı Eşitle"'ye tıklamanız. "Ağı Eşitle"'ye tıkladığınızda, kısa bir kesinti bağlantısı uygulamanızı sanal ağınıza arasında neden. Uygulamanızı yeniden değildir, ancak bağlantı kaybı düzgün sitenize çalışmamasına neden olabilir. 

## <a name="accessing-on-premises-resources"></a>Erişim şirket içi kaynaklara
Uygulamalar, siteden siteye bağlantı ile sanal ağlar'ı tümleştirerek, şirket içi kaynaklara erişebilir. Şirket kaynaklarına erişmek için noktadan siteye adres bloklarını ile şirket içi VPN ağ geçidi yollarınızı güncelleştirmeniz gerekiyor. Siteden siteye VPN ilk ayarlandığında yapılandırmak için kullanılan komut rotalar düzgün şekilde ayarlamanız gerekir. Siteden siteye VPN oluşturduktan sonra noktadan siteye adres eklerseniz, yolların el ile güncelleştirmeniz gerekir. Bunu nasıl yapacağınız hakkında ayrıntılı bilgi, ağ geçidi değişir ve burada açıklanmamaktadır. Ayrıca, siteden siteye VPN bağlantısı ile yapılandırılmış BGP sahip olamaz.

> [!NOTE]
> VNet tümleştirme özelliği, bir uygulama bir ExpressRoute ağ geçidi olan bir VNet ile tümleştirilemeyeceğini. ExpressRoute ağ geçidi olarak yapılandırılmış olsa bile [bir arada bulunma modu] [ VPNERCoex] VNet tümleştirmesi çalışmaz. Bir ExpressRoute bağlantısı üzerinden kaynaklara erişmeye ihtiyacınız sonra kullanabileceğiniz bir [App Service ortamı][ASE], ağınızda çalışır. 
> 
> 

## <a name="peering"></a>Eşleme
VNet tümleştirmesi, bağlandığınız sanal ağa eşlenmiş Vnet'ler içindeki kaynaklara erişmek için kullanabilirsiniz. Uygulamanızın üzerinde çalışmak için eşlemeyi yapılandırmak için:

1. Uygulamanızı VNet üzerinde eşleme bağlantıları bağlandığı ekleyin. Eşleme bağlantısını eklerken etkinleştirme **sanal ağ erişimine izin ver** ve **iletilen trafiğe izin ver** ve **ağ geçidi aktarımına izin ver**.
1. Bağlandığınız sanal ağa eşlenen bir VNet üzerinde bir eşleme bağlantısı ekleyin. Hedef VNet eşleme bağlantısı eklerken, etkinleştirme **sanal ağ erişimine izin ver** ve **iletilen trafiğe izin ver** ve **uzak ağ geçitlerini izin**.
1. App Service planı'na gidin > Ağ > sanal ağ tümleştirmesi UI Portalı'nda.  Uygulamanıza bağlanan sanal ağ seçin. Yönlendirme bölümü altında uygulamanızın bağlı olduğu sanal ağ eşlendikten vnet'in adres aralığını ekleyin.  


## <a name="pricing-details"></a>Fiyatlandırma ayrıntıları
VNet tümleştirmesi özelliğinin kullanımına üç ilgili ücretlerine vardır:

* ASP fiyatlandırma katmanı gereksinimleri
* Veri aktarım maliyetleri
* VPN ağ geçidi maliyet.

Uygulamalarınızı bir standart, Premium veya PremiumV2 App Service planı içinde olması gerekir. Burada bu maliyetlerinden daha fazla bilgi görebilirsiniz: [App Service fiyatlandırması][ASPricing]. 

VNet aynı veri merkezinde olsa bile, veri çıkışı için bir ücret yoktur. Bu ücretler açıklanan [veri aktarma fiyatlandırma ayrıntıları][DataPricing]. 

VNet ağ geçidine noktadan siteye VPN için gerekli olan bir maliyeti yoktur. Ayrıntıları bulunan [VPN Gateway fiyatlandırması] [ VNETPricing] sayfası.

## <a name="troubleshooting"></a>Sorun giderme
Özellik ayarlamak kolay olsa da, bu deneyiminiz sorun ücretsiz olacaktır anlamına gelmez. Karşılaşmamanız gerekir, istenen uç noktası var. erişme uygulama konsolundan bağlantıyı test etmek için kullanabileceğiniz bazı yardımcı programlar sorunlardır. Kullanabileceğiniz iki konsol vardır. Kudu konsolunu biridir ve diğer Azure portalında konsoldur. Git araçları için Kudu Konsolu uygulamanızdan ulaşmak için Kudu ->. Bu [siteadı] için giderek ile aynı olur. scm.azurewebsites.net. Açıldıktan sonra hata ayıklama Konsolu sekmesine gidin. Git araçları için Azure portal barındırılan konsola ardından uygulamanızdan almak için konsolu ->. 

#### <a name="tools"></a>Araçlar
Araçlar **ping**, **nslookup** ve **tracert** güvenlik kısıtlamaları nedeniyle konsolu üzerinden çalışmaz. Void eklenen iki ayrı araçları doldurmak için. DNS işlevselliğini test etmek için nameresolver.exe adlı bir araç ekledik. Söz dizimi aşağıdaki gibidir:

    nameresolver.exe hostname [optional: DNS Server]

Kullanabileceğiniz **nameresolver** uygulamanızın bağımlı ana bilgisayar adları kontrol etmek için. Herhangi bir şey yanlış DNS ile yapılandırılmış olması ya da belki de, DNS sunucusuna erişiminiz yoksa, bu şekilde, test edebilirsiniz.

Sonraki aracı, bir konak ve bağlantı noktası birleşimi TCP bağlantısını test etmek sağlar. Bu aracı adlı **tcpping** ve söz dizimi şu şekildedir:

    tcpping.exe hostname [optional: port]

**Tcpping** yardımcı programı bildirir belirli ana bilgisayarı ve bağlantı noktası ulaşması durumunda. Yalnızca başarı gösterebilirsiniz varsa: konak ve bağlantı noktası birleşimini dinleyen bir uygulamanın yoktur ve belirtilen konak ve bağlantı noktası uygulamanızdan ağ erişimi yoktur.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>Kaynakları barındıran sanal ağdan sanal ağa erişim hata ayıklama
Uygulamanız belirli ana bilgisayarı ve bağlantı noktası ulaşmasını önleyen etmenizi vardır. Çoğu zaman bu üç şeylerden biri değil:

* **Bir güvenlik duvarı içinde yoludur.** Biçiminde bir güvenlik duvarı varsa, TCP zaman aşımı ulaşırsınız. TCP zaman aşımı 21 bu durumda saniyedir. Kullanım **tcpping** bağlantısını test etmek için aracı. TCP zaman aşımı, güvenlik duvarları ötesinde pek çok nedeni olabilir ancak kavramla başlayacağız. 
* **DNS erişilebilir durumda değil.** DNS zaman aşımı, DNS sunucusu başına üç saniyedir. İki DNS sunucusu varsa, zaman aşımı 6 saniyedir. DNS çalışıp çalışmadığını görmek için nameresolver kullanın. Ağınızın yapılandırılmış DNS kullanmaz olarak nslookup kullanamazsınız unutmayın.
* **Noktadan siteye adres aralığı geçersiz.** Noktadan siteye IP aralığı RFC 1918 özel IP aralığı olması gerekir (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255). Aralığın dışında IP'ler kullanıyorsa şeyler işe yaramaz. 

Bu öğeleri sorununuzu alamıyorsanız, gibi basit şeyler için ilk bakış: 

* Ağ geçidini oluşturan portalda olacak şekilde gösteriyor mu?
* Sertifikalar eşit olacak şekilde gösteriyor?
* Etkilenen ASP'ler içinde "eşitleme ağ" yapmadan herkes ağ yapılandırması değişti? 

Ağ geçidiniz kapalı ise, getirmeniz yedekleyin. Sertifikalarınızı eşitleme dışı ise, VNet tümleştirmesi ASP görünümüne gidin ve "Ağı Eşitle" isabet. ASP ile eşitlenmiş durumda, sanal ağ yapılandırması için yapılan bir değişikliği yapılmış şüpheleniyorsanız, "Ağı Eşitle" a basın.  "Ağı Eşitle" işlemi, bu sanal ağ ile tümleşik ASP herhangi bir uygulama yeniden başlatılır. 

Tüm bu durumda, iyi biraz daha ayrıntılı incelemek gerekir:

* Aynı sanal ağ kaynaklarına ulaşmak için VNet Tümleştirmesi'ni kullanarak diğer tüm uygulamalar var mı? 
* Uygulama konsoluna gidin yapabilir ve ağınızdaki diğer bir kaynaklarına ulaşmak için tcpping kullanabilirsiniz? 

Yukarıdaki doğruysa, ardından, sanal ağ tümleştirmesi uygundur ve başka bir yerde bir sorundur. Burada bir konak: bağlantı noktası neden ulaşılamıyor görmek için basit bir yolu olmadığından daha zor olmasını alır budur. Nedenlerden bazıları şunlardır:

* bir güvenlik duvarı erişim noktanız site IP aralığı uygulama bağlantı noktasına engelleyen konağınızdaki sahip. Çapraz alt ağlar genellikle ortak erişim gerektirir.
* Hedef ana bilgisayarınızda çalışmıyor
* Uygulamanızı kapalı
* yanlış IP veya ana bilgisayar adı olan
* Uygulamanızın beklediğiniz değerinden farklı bir bağlantı noktasında dinliyor. Uç nokta ana bilgisayarda "netstat - aon" kullanarak, işlem kimliği dinleme bağlantı noktası ile eşleşebilir. 
* Bunlar erişim bağlantı noktası ve uygulama ana bilgisayarı, noktadan siteye IP aralığına engelleyecek şekilde yapılandırılmış, ağ güvenlik grupları

Noktadan siteye IP aralığındaki tüm aralığından erişim izin vermeniz gerekir böylece uygulamanızın kullanacağı hangi IP bilmiyorum unutmayın. 

Ek hata ayıklama adımları içerir:

* Sanal ağınızdaki bir sanal makineye bağlanın ve, kaynak konak: bağlantı noktası buradan ulaşmaya çalışır. TCP erişimi test etmek için PowerShell komutunu kullanın. **test-netconnection**. Söz dizimi aşağıdaki gibidir:

      test-netconnection hostname [optional: -Port]

* VM ve test erişim uygulama söz konusu konakta ve bağlantı noktası uygulamanızdan konsolundan getirecek

#### <a name="on-premises-resources"></a>Şirket içi kaynaklara ####

Uygulamanızı, şirket içi kaynak ulaşamazsa, ağınızdan ulaşabileceği kaynak denetleyin. Kullanım **test-netconnection** TCP erişimi denetlemek için PowerShell komutu. Ardından, şirket içi kaynak Makinenizin ulaşamazsa, siteden siteye VPN bağlantınızın çalıştığından emin olun. Ardından çalışıyorsa, durumu ve şirket içi ağ geçidi yapılandırması yanı sıra daha önce Not aynı şey olup olmadığını denetleyin. 

Sanal ağınıza barındırıyorsa, VM, şirket içi sisteminizi ulaşabilir ancak uygulamanızı olamaz ve ardından nedeni aşağıdakilerden biri olabilir:

* yollarınızı noktanızın site IP aralıkları, şirket içi ağ geçidi için yapılandırılmadı
* Ağ güvenlik gruplarınızda noktadan siteye IP aralığınızı erişimi engelliyor
* Şirket içi güvenlik duvarlarınızdan noktadan siteye IP aralığınızı gelen trafiği engelliyor


## <a name="powershell-automation"></a>PowerShell Otomasyonu

App Service, bir Azure PowerShell kullanarak sanal ağ ile tümleştirebilirsiniz. Çalıştırılmaya hazır bir betik için bkz: [Azure uygulama Hizmeti'nde bir uygulamanın bir Azure sanal ağına bağlama](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3).

## <a name="hybrid-connections-and-app-service-environments"></a>Karma bağlantılar ve App Service ortamları
Barındırılan bir sanal ağ kaynaklarına erişimi etkinleştirme üç özellik vardır. Bunlar:

* Sanal Ağ Tümleştirmesi
* Karma Bağlantılar
* App Service ortamları

Karma bağlantılar, karma bağlantı Manager(HCM) ağınızdaki adlı bir geçiş aracısı yüklemek gerektirir. HCM, Azure ve uygulamanızı bağlanabilmesi gerekir. Karma bağlantılar, bir gelen internet erişilebilen bir uç nokta gerektirmez, uzak ağ için olduğu gibi bir VPN bağlantısı için gerekli. HCM, yalnızca Windows üzerinde çalışır ve yüksek kullanılabilirlik sağlamak için çalışan en fazla beş örnek olabilir. Karma bağlantılar yalnızca destekler TCP olsa ve her HC uç noktası için bir özel konak: bağlantı noktası bileşimi ile eşleştirmek. 

App Service ortamı Özelliği Azure App Service tek Kiracı örneğini ağınızda çalıştırmanıza olanak sağlar. Ardından uygulamalarınızı App Service Ortamı'nda, uygulamalarınızı herhangi bir ekstra adım olmadan sanal ağınızdaki kaynaklara erişebilir. App Service ortamı ile uygulamalarınızı daha güçlü çalışanları çalıştırın ve en fazla 100 ASP örneklerinin ölçeklendirme yapabilirsiniz. App Service ortamları tüm ExpressRoute ve hizmet uç noktaları da dahil olmak üzere ağ özellikleri ile çalışır.  

Olsa bazı büyük/küçük harf çakışma kullanır, bu özelliklerin hiçbiri diğerlerinden değiştirebilirsiniz. Bilerek hangi özelliği kullanacağınıza gereksinimlerinize bağlıdır. Örneğin:

* Bir geliştirici olan ve azure'da iş istasyonunda masanızın altında bir veritabanına erişebilen bir site çalıştırmak istediğiniz, ardından kullanılacak kolay karma bağlantılar şeydir. 
* Çok sayıda yerleştirmek ister büyük bir kuruluş ise web özellikleri genel Bulut ve kendi ağınızda yönetmek ve ardından App Service ortamı ile gitmek istiyorsanız. 
* Sanal ağınızdaki kaynaklara erişmek için gereken birden fazla uygulama varsa, daha sonra VNet tümleştirmesi Git yoludur. 

Olduğunda, sanal VNet Tümleştirmesi'ı kullanarak şirket içi ağınıza zaten bağlı veya bir App Service ortamı kullanmak için kolay bir yol şirket içi kaynaklara. Ardından sanal ağınızı şirket içi ağınıza bağlı değilse, bunu bir siteden siteye VPN HCM yükleme ile karşılaştırıldığında, sanal ağ ile ayarlamak için çok daha fazla getirdiği yüktür. 

İşlevsel farklılıklar dışında burada da fiyatlandırma farklarını göz. Bir Premium App Service ortamı özelliği olan hizmet teklifi ancak en iyi ağ diğer harika özelliklerin yanı sıra yapılandırma olanakları sunar. VNet tümleştirmesi standart veya Premium ASP'ler ile kullanılabilir ve güvenli bir şekilde ağınızdan çok kiracılı App Service kaynakları kullanan için idealdir. Karma bağlantılar şu anda bağlı olarak gereksinim duyduğunuz fiyatlandırması, ücretsiz olarak başlayın ve ardından aşamalı olarak daha pahalı düzeylerine olan hesap temel BizTalk. Ancak birçok ağlarda çalışmaya söz konusu olduğunda, 100'den de ayrı ağlar kaynaklara erişmek etkinleştirebilirsiniz karma bağlantılar gibi diğer özellik yoktur. 

## <a name="new-vnet-integration"></a>Yeni Sanal Ağ Tümleştirmesi ##

VNet tümleştirmesi yeteneğinin noktadan siteye VPN teknolojisine bağlı olmayan yeni bir sürümü var. Önceden var olan özellik, yeni bir önizleme özelliği, ExpressRoute ve hizmet uç noktaları ile çalışır. 

Yeni özelliği yalnızca yeni Azure App Service ölçek birimleri kullanılabilir. PremiumV2 için ölçeklendirilebilir, daha yeni bir App Service ölçek birimi demektir. Portalda VNet tümleştirmesi kullanıcı arabirimini uygulamanızı yeni VNet tümleştirme özelliğini kullanıp kullanamayacağını bildirir. 

Yeni sürümü Önizleme aşamasındadır ve aşağıdaki özelliklere sahiptir.

* Ağ geçidi yok yeni VNet tümleştirme özelliğini kullanmak için gereklidir
* ExpressRoute ile tümleştirme ötesinde herhangi bir ek yapılandırma, sanal ağa bağlı olmayan ExpressRoute bağlantılar üzerinden kaynaklara erişebilir.
* Uygulama ve sanal ağ aynı bölgede olması gerekir
* Yeni özellik, Resource Manager sanal ağınızdaki kullanılmayan bir alt ağ gerektiriyor.
* App Service planınızın standart, Premium veya PremiumV2 planına olmalıdır
* Önizleme aşamasında olduğu sürece üretim iş yükleri üzerinde yeni özelliği desteklenmiyor
* Uygulamanız Premium v2'ye yükseltme işleyebilen bir Azure App Service dağıtımı olması gerekir.
* Yeni VNet tümleştirme özelliği, bir App Service ortamında uygulamaları için çalışmaz.
* Tümleşik bir uygulama ile bir Vnet'i silemiyor.  
* Rota tabloları ve genel eşleme henüz yeni VNet Tümleştirmesi ile kullanılamaz.  
* Bir adres, her bir App Service planı örneği için kullanılır. Alt ağ boyutu atamasından sonra değiştirilemez olduğundan, birden fazla maksimum ölçek boyutunuzu ele bir alt ağ kullanın. 32 adreslerine sahip bir/27, 20 örnek için ölçeklendirilebilir bir App Service planı kapsayan önerilen boyutu aynıdır.
* Yeni VNet tümleştirme özelliğini kullanarak kaynakları güvenli hizmet uç kullanabilir. Bunu yapmak için sanal ağ tümleştirmesi için kullanılan alt ağ üzerindeki hizmet uç noktalarını etkinleştirin.

Yeni özelliği kullanmak için:

1. Portal ağ Arabirimine gidin. Ardından uygulamanızın yeni özelliğini kullanabilmek için yeni önizleme özelliğini kullanmak için bir özellik görürsünüz.  

   ![Yeni sanal ağ tümleştirmesi Önizleme seçin][6]

1. Seçin **VNet Ekle (Önizleme)**.  

1. Resource Manager ile tümleştirin ve daha sonra yeni bir alt ağ oluşturmak veya önceden var olan boş bir alt ağ seçin için istediğiniz sanal ağı seçin. Tümleştirme tamamlanması bir dakikadan kısa sürer. Tümleştirme sırasında uygulamanızı yeniden başlatılır.  Tümleştirme tamamlandığında, sanal ağ ile tümleştirilir ve özellik Önizleme aşamasındadır bildiren bir başlık ayrıntıları görürsünüz.

   ![VNet ve alt ağ seçin][7]

Uygulamanızı sanal ağınıza yapılandırılmış DNS sunucusu kullanacak şekilde etkinleştirmek için uygulamanız nerede WEBSITE_DNS_SERVER adıdır ve değer sunucusunun IP adresi için bir uygulama ayarı oluşturun.  İkincil DNS sunucusu varsa, ardından adı WEBSITE_DNS_ALT_SERVER olduğu başka bir uygulama ayarı oluşturun ve değeri sunucunun IP adresidir. 

Uygulamanızı sanal ağdan bağlantıyı kesmek için seçin **Bağlantıyı Kes**. Bu, web uygulamanızı yeniden başlatır. 

Yeni VNet tümleştirme özelliği, hizmet uç noktaları kullanmanıza olanak sağlar.  Hizmet uç noktaları ile uygulamanızı kullanmak için seçili bir sanal ağa bağlanmak ve ardından tümleştirmesi için kullanılan alt ağdaki hizmet uç noktaları yapılandırmak için yeni VNet Tümleştirmesi'ni kullanın. 

<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-app.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-addvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-details.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-aspdetails.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-newvnet.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-newvnetdetails.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-selectvnet.png

<!--Links-->
[VNETOverview]: https://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: https://portal.azure.com/
[ASPricing]: https://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: https://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[ASEintro]: environment/intro.md
[ILBASE]: environment/create-ilb-ase.md
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md
[ASE]: environment/intro.md
[creategatewaysubnet]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#gatewaysubnet
[creategateway]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#creategw
[setp2saddresses]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#addresspool
