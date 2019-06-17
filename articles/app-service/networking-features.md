---
title: Dağıtım özellikleri - Azure App Service ağ | Microsoft Docs
description: Çeşitli App Service ağ özelliklerini kullanma
author: ccompy
manager: stefsch
editor: ''
services: app-service\web
documentationcenter: ''
ms.assetid: 5c61eed1-1ad1-4191-9f71-906d610ee5b7
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/28/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 666430a11fb95871eb601b2a38eb7b97ad16119f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66498957"
---
# <a name="app-service-networking-features"></a>App Service ağ özellikleri

Azure App Service'te uygulamaları birden çok yolla dağıtılabilir. Varsayılan olarak, barındırılan uygulamaları doğrudan İnternet'ten erişilemediği olduğu ve yalnızca bir App Service, barındırılan internet uç noktalar ulaşın. Birçok müşteri uygulamaları, ancak gelen ve giden ağ trafiğinizi denetlemek gerekir. App Service bu ihtiyaçları karşılamak için çeşitli özellikler vardır. Hangi özelliği belirli bir sorunu çözmek için kullanılması gereken sınama bilmektir. Bu belge bazı örnek kullanım örneklerine dayalı hangi özelliğinin kullanılması gereken belirlemelerine yardımcı olmak için tasarlanmıştır.

Azure App Service için iki birincil dağıtım türü vardır. App Service planlarında ücretsiz, paylaşılan, temel, standart, Premium ve Premiumv2 sunan çok kiracılı genel hizmetinde fiyatlandırma SKU'ları. Ardından tek bir kiracı yalıtılmış SKU App Service planlarında doğrudan Azure sanal ağınız (VNet) barındıran uygulama hizmeti engelleniyorsa yoktur. Çok kiracılı bir hizmet ya da bir ASE varsa, kullanmak istediğiniz özellikleri üzerinde değişir. 

## <a name="multi-tenant-app-service-networking-features"></a>Çok kiracılı App Service ağ özellikleri 

Azure App Service, dağıtılmış bir sistemde. Gelen HTTP/HTTPS istekleri işleyen rolleri, ön uç adı verilir. Müşterinin iş yükü ana bilgisayar rolleri çalışanları çağrılır. Bir App Service dağıtımı yer alan rollerin tümünde bir çok kiracılı ağ yok. Aynı App Service ölçek birimi birçok farklı müşterinin olduğundan, App Service ağ doğrudan ağa bağlanamıyor. Ağları bağlama yerine, uygulama iletişimine farklı yönlerini işlemek için özellikleri ihtiyacımız var. UYGULAMANIZDAN çağrıları yapılırken sorunları çözmek için uygulamanızı isteklerini işleyen özellikleri kullanılamaz. Benzer şekilde, UYGULAMANIZA sorunları çözmek için uygulamanızı yapılan çağrılar için çözmekte özellikleri kullanılamaz.  

| Gelen özellikleri | Giden özellikleri |
|---------------------|-------------------|
| Adres atanmış uygulama | Karma Bağlantılar |
| Erişim kısıtlamaları | ağ geçidi gerekli VNet tümleştirmesi |
| Hizmet Uç Noktaları | VNet tümleştirmesi (Önizleme) |

Aksi belirtilmediği sürece, bu özelliklerin tümünü birlikte kullanılabilir. Çeşitli sorunları çözmek için özellikleri karıştırabilirsiniz.

## <a name="use-case-and-features"></a>Kullanım örneği ve özellikleri

Herhangi bir belirli kullanım örneği için sorunu çözmek için birkaç şekilde olabilir.  Kullanılacak doğru bazen yalnızca kullanım örneği kendisi dışında nedenlerden dolayı özelliğidir. Aşağıdaki gelen kullanım örneklerini uygulamanıza giden trafiği denetleyen etrafında sorunları çözmek için App Service ağ özelliklerini kullanmayı önerin. 
 
| Gelen kullanım örnekleri | Özellik |
|---------------------|-------------------|
| IP tabanlı SSL uygulamanız için gereken destek | Adres atanmış uygulama |
| Paylaşılan not uygulamanız için adanmış gelen adresi | Adres atanmış uygulama |
| İyi tanımlanmış adresleri kümesinden uygulamanıza erişimi kısıtlama | Erişim kısıtlamaları |
| Uygulamamı Ağımda özel IP'ler üzerinde kullanıma sunma | ILB ASE </br> Hizmet uç noktası ile uygulama ağ geçidi |
| Bir vnet'teki kaynakların uygulamama erişimi kısıtlama | Hizmet Uç Noktaları </br> ILB ASE |
| Uygulamamı bir özel IP Ağımda üzerinde kullanıma sunma | ILB ASE </br> Hizmet uç noktaları ile bir Application Gateway için özel IP gelen |
| Uygulamamı bir WAF ile koruma | Uygulama ağ geçidi + ILB ASE </br> Hizmet uç noktası ile uygulama ağ geçidi </br> Azure ön kapı erişimi kısıtlama |
| Farklı bölgelerdeki uygulamalarım trafiğinde Yük Dengeleme | Azure ön kapı erişimi kısıtlama | 
| Aynı bölgede trafik yükünü dengeleme | Hizmet uç noktası ile uygulama ağ geçidi | 

Aşağıdaki giden kullanım örnekleri, uygulamanız için giden erişim gereksinimlerini karşılamayı amaçlayan App Service ağ özelliklerini kullanmayı önerir. 

| Giden kullanım örnekleri | Özellik |
|---------------------|-------------------|
| Aynı bölgede bir Azure sanal ağdaki kaynaklara erişmek | Sanal Ağ Tümleştirmesi </br> ASE |
| Farklı bir bölgede bir Azure sanal ağdaki kaynaklara erişmek | ağ geçidi gerekli VNet tümleştirmesi </br> ASE ve VNet eşlemesi |
| Hizmet uç noktaları ile güvenli kaynaklara erişim | Sanal Ağ Tümleştirmesi </br> ASE |
| Azure'a bağlı değilse bir özel ağ kaynakları | Karma Bağlantılar |
| ExpressRoute bağlantı hatları kaynaklara erişim | VNet tümleştirmesi (RFC 1918 adreslere şimdilik sınırlıdır) </br> ASE | 


### <a name="default-networking-behavior"></a>Varsayılan ağ davranışı

Birçok müşteri, Azure App Service ölçek birimleri her dağıtımda destekler. Ücretsiz ve paylaşılan SKU planları, çok kiracılı çalışanlarında müşteri iş yüklerinin barındırın. Temel ve yalnızca bir App Service planı (ASP) adanmış planları konak müşteri iş yükleri üzerinde. Bir standart App Service planı tablonuz varsa, tüm uygulamaları bu planı aynı çalışan üzerinde çalışır. Çalışan ölçeklendirirseniz, ardından tüm ASP uygulamalarında, ASP her örneği için yeni bir çalışan üzerinde çoğaltılır. Diğer planlar için kullanılan çalışanlar için Premiumv2 kullanılan çalışanlar farklıdır. Her App Service dağıtımı için tüm gelen trafiği bu App Service dağıtımı uygulamalarında kullanılan bir IP adresine sahiptir. Vardır ancak herhangi bir yere 4'ten 11 adreslere giden çağrıları yapmak için kullanılır. Bu adresler bu App Service dağıtımı tüm uygulamalar tarafından paylaşılır. Giden adresleri farklı alt türlerine göre farklılık gösterir. Bu, ücretsiz, paylaşılan, temel, standart ve Premium ASP'ler tarafından kullanılan adreslerini Premiumv2 ASP'ler giden çağrılar için kullanılan adresleri farklı anlamına gelir. Uygulamanız için özellikler bakarsanız, uygulamanız tarafından kullanılan gelen ve giden adreslerini görebilirsiniz. Bir IP ACL ile bir bağımlılık kilitleme gerekiyorsa possibleOutboundAddresses kullanın. 

![Uygulama özellikleri](media/networking-features/app-properties.png)

App Service hizmetini yönetmek için kullanılan uç noktalarını birçok vardır.  Bu adresler ayrı bir belgede yayımlanır ve AppServiceManagement IP hizmet etiketi de olur. AppServiceManagement etiket yalnızca bir App Service ortamı (Bu tür trafiğe izin vermek için gerek duyduğunuz ASE ile) kullanılır. App Service içinde AppService IP hizmet etiketi gelen adresleri izlenir. App Service tarafından kullanılan çıkış adreslerini içeren IP hizmet etiketi yok yoktur. 

![App Service, gelen ve giden diyagramı](media/networking-features/default-behavior.png)

### <a name="app-assigned-address"></a>Adres atanmış uygulama 

Uygulama atanan adresi özellik bir offshoot IP tabanlı SSL yeteneğinin ve uygulamanızla SSL ayarlama ayarlayarak erişilir. Bu özellik, IP tabanlı SSL çağrıları için kullanılabilir, ancak uygulamanızı olan yalnızca bir adres sağlamak için de kullanılabilir. 

![Adres diyagramına atanmış uygulama](media/networking-features/app-assigned-address.png)

Adresi atanmış bir uygulama kullandığınızda, trafiğiniz App Service Ölçek birimine tüm gelen trafiği işleyen aynı ön uç rollerini yine de geçer. Ancak, uygulamanıza atanan adresi yalnızca, uygulamanız tarafından kullanılır. Bu özellik için kullanım örnekleri vardır:

* IP tabanlı SSL uygulamanız için gereken destek
* Başka bir şey ile paylaşılmayan uygulamanız için adanmış bir adresi ayarlayın

Uygulamanızdan öğreticisiyle bir adresi ayarlamak öğrenebilirsiniz [yapılandırma IP tabanlı SSL][appassignedaddress]. 

### <a name="access-restrictions"></a>Erişim kısıtlamaları 

Filtre erişim kısıtlamalarını yeteneği sağlar **gelen** istekleri tabanlı kaynak IP adresi. Filtreleme eylemi, uygulamalarınızı çalıştığı çalışan pay Yukarı Akış ön uç rollerini üzerinde gerçekleşir. Ön uç rollerini çalışanların Yukarı Akış olduğundan, erişim kısıtlamaları özelliği uygulamalarınız için ağ düzeyinde koruma olarak görülebilir. Özellik listesi oluşturmanıza olanak sağlayan izin verme ve reddetme adres bloklarını, öncelik sırasına göre değerlendirilir. Azure ağı içinde var olan ağ güvenlik grubu (NSG) özelliğine benzer.  Bir ASE veya çok kiracılı bir hizmet, bu özelliği kullanabilirsiniz. Bir ILB ASE ile kullanıldığında, özel adres bloklarından erişimi kısıtlayabilirsiniz.

![Erişim kısıtlamaları](media/networking-features/access-restrictions.png)

Erişim kısıtlamaları özelliği uygulamanıza erişmek için kullanılan IP adreslerini kısıtlamak istediğiniz senaryolarda yardımcı olur. Kullanımı arasında bu özellik için durum vardır:

* İyi tanımlanmış adresleri kümesinden uygulamanıza erişimi kısıtlama 
* Azure ön kapı gibi bir Yük Dengeleme hizmeti aracılığıyla gelen erişimi kısıtlayın. 147\.243.0.0/16 ve 2a01:111:2050 gelen trafiğe izin vermek için kuralları, gelen trafik için ön kapı Azure kilitleme istediyseniz, oluşturma:: / 44. 

![Erişim kısıtlamalarına sahip ön kapısı](media/networking-features/access-restrictions-afd.png)

Azure sanal ağınız (VNet) içinde kaynaklardan yalnızca ulaşılabilir, uygulamanıza erişimi kilitleme istiyorsanız, ağınızda ne olursa olsun, kaynak üzerinde statik bir genel adresi gerekir. Kaynakları genel adresi yoksa, bunun yerine hizmet uç noktaları özelliğini kullanmanız gerekir. Bu özellik Eğitmeni ile etkinleştirmek öğrenin [erişim kısıtlamalarını yapılandırma][iprestrictions].

### <a name="service-endpoints"></a>Hizmet uç noktaları

Hizmet uç noktaları kilitlemenize olanak tanır **gelen** sağlayacak şekilde kaynak adresi seçtiğiniz alt kümesinden gelmelidir uygulamanıza erişim. Bu özellik, IP erişim kısıtlamaları ile birlikte çalışır. Hizmet uç noktaları, aynı kullanıcı deneyiminde IP erişim kısıtlamalarını olarak ayarlanır. Genel adresleri yanı sıra alt ağlar, sanal ağlarda içeren bir izin verme/reddetme listesi erişim kuralları oluşturabilirsiniz. Bu özellik, şöyle senaryoları destekler:

![Hizmet uç noktaları](media/networking-features/service-endpoints.png)

* Uygulamanıza gelen trafiği kilitlemek için uygulamanıza bir uygulama ağ geçidi ayarlama
* Uygulamanızı sanal ağınızdaki kaynaklara erişimi Testricting. Bu VM'ler, ase veya VNet tümleştirmesi kullanan bile diğer uygulamalar içerebilir 

![Hizmet uç noktaları ile uygulama ağ geçidi](media/networking-features/service-endpoints-appgw.png)

Öğreticide uygulamanızla birlikte hizmet uç noktalarını yapılandırma hakkında daha fazla bilgi [hizmet uç noktası erişim kısıtlamalarını yapılandırma][serviceendpoints]
 
### <a name="hybrid-connections"></a>Karma Bağlantılar

App Service karma bağlantılar verir hale getirmek uygulamalarınızı **giden** belirtilen TCP uç çağrıları. Şirket içinde bir sanal ağ içindeki uç nokta da olabilir veya Azure bağlantı noktası 443 üzerinden giden trafiğe izin veren herhangi bir yerde. Bu özellik, bir Windows Server 2012 veya daha yeni bir konak üzerinde karma Bağlantı Yöneticisi (HCM) adlı bir geçiş aracısı yüklenmesini gerektirir. HCM, 443 numaralı bağlantı noktasında Azure geçişi ulaşabildiğinden olması gerekiyor. HCM, App Service karma bağlantılar kullanıcı Arabiriminin Portalı'nda ' nden indirilebilir. 

![Karma bağlantılar, ağ akışı](media/networking-features/hybrid-connections.png)

App Service karma bağlantılar özelliği, Azure geçiş karma bağlantılar özelliği üzerinde oluşturulmuştur. App Service uygulamanızdan yapmayı giden çağrıları TCP konak ve bağlantı noktası yalnızca destekleyen özelliği özel biçimi kullanır. Bu konak ve bağlantı noktası HCM yüklendiği ana bilgisayar üzerinde çözümlenmesi yeterlidir. App Service'te uygulama ana bilgisayarı ve bağlantı noktası, karma bağlantıyı tanımlanan bir DNS araması yaptığında, trafiğin karma bağlantısı üzerinden ve karma Bağlantı Yöneticisi kullanıma gitmek için otomatik olarak yönlendirilir. Karma bağlantılar hakkında daha fazla bilgi için ilgili belgeleri okuyun [App Service karma bağlantılar][hybridconn]

Bu özellik için yaygın olarak kullanılır:

* Bir VPN veya ExpressRoute ile azure'a bağlı olmayan özel ağlar kaynakları
* Lift- and -shift şirket içi uygulamaları App Service için de destek veritabanlarını taşımak zorunda kalmadan desteği  
* Tek bir ana bilgisayar ve bağlantı noktası başına karma bağlantı, güvenli bir şekilde erişim sağlar. Çoğu ağ özellikleri ağ erişimi açın ve karma bağlantıları ile yalnızca size ulaşabildiğimizden bağlantı noktası ve tek bir konak gerekir.
* Diğer giden bağlantı yöntemler tarafından kapsanmayan senaryolarını kapsayan
* Burada uygulamaları şirket içi kaynaklara kolayca yararlanabilirsiniz App Service'te geliştirme gerçekleştirin 

Özelliği bir gelen güvenlik duvarı delik olmadan şirket kaynaklarına erişim sağladığından, geliştiriciler tarafından yaygın olarak kullanılır. Diğer giden App Service ağ çok Azure sanal ilgili ağ özellikleridir. Karma bağlantılar, giden bir sanal ağ üzerinde bir bağımlılık yok ve çok çeşitli ağ gereksinimleri için kullanılabilir. App Service karma bağlantılar özelliği değil dikkat edin veya üzerinde yaptığınız bilmeniz önemlidir. Bu veritabanı, bir web hizmeti veya rastgele bir TCP yuva bir ana bilgisayar üzerinden erişmek için kullanabileceğiniz söylemek olmasıdır. Özelliği, temelde TCP paketleri tüneller. 

Karma bağlantılar geliştirme için popüler olsa da, bu da çok sayıda üretim uygulamalarında kullanılır. Bir web hizmeti veya veritabanı erişmek için idealdir, ancak oluşturulan harika bir birçok bağlantılar içeren durum için uygun değil. 

### <a name="gateway-required-vnet-integration"></a>ağ geçidi gerekli VNet tümleştirmesi 

App Service VNet tümleştirme özelliğini olmak için uygulamanızı sağlayan ağ geçidi gerekli **giden** Azure sanal ağına istekleri. Özellik, uygulamanızı bir sanal ağ geçidine noktadan siteye VPN ile bir VNet üzerinde çalıştığı konak bağlanarak çalışır. Özelliği yapılandırırsanız, uygulamanızın her örneği için atanan noktadan siteye adreslerden birini alır. Bu özellik, Klasik veya Resource Manager sanal ağlarına herhangi bir bölgedeki kaynakları erişmenize olanak sağlar. 

![ağ geçidi gerekli VNet tümleştirmesi](media/networking-features/gw-vnet-integration.png)

Bu özellik, diğer vnet'lerdeki kaynaklar erişim sorununu çözer ve hatta diğer sanal ağ veya hatta şirket içi bir sanal ağ üzerinden bağlanmak için kullanılabilir. ExpressRoute ile bağlı çalışmaz Vnet'ler ancak siteden siteye VPN ile yoksa bağlı ağlar. ASE ağınızda zaten olduğundan, bir App Service ortamı (ASE içindeki), bir uygulamadan bu özelliği kullanmak normalde uygun değil. Bu özellik çözer kullanım örnekleri şunlardır:

* Azure sanal ağlarınıza özel IP'ler kaynaklara erişme 
* Kaynaklara erişme bir siteden siteye VPN ise şirket 
* Eşlenmiş Vnet'ler içindeki kaynaklara erişme 

Bu özellik etkinleştirildiğinde, uygulamanızı hedef VNet ile yapılandırılan DNS sunucusunu kullanır. Daha fazla bilgi edinebilirsiniz üzerinde belgelerinde bu özellik [App Service VNet tümleştirmesi][vnetintegrationp2s]. 

### <a name="vnet-integration"></a>Sanal Ağ Tümleştirmesi

Ağ geçidi, sanal ağ tümleştirmesi özellik çok kullanışlıdır ancak yine de kaynaklara erişmeyi ExpressRoute üzerinde çözmez gereklidir. ExpressRoute bağlantıları erişmek ihtiyaç duyan üzerinde hizmet uç noktası güvenli hale getirilen hizmetlere çağrı yapmak uygulamaları gerek yoktur. Her iki ek bu ihtiyaçları çözmek için başka bir VNet tümleştirmesi özelliği eklendi. Yeni VNet tümleştirme özelliğini aynı bölgede bir Resource Manager sanal ağ içindeki bir alt ağdaki arka uç uygulamanızın yerleştirmenize olanak sağlar. Bu özellik, bir sanal ağda zaten olan bir App Service ortamı kullanılamaz. Bu özellik şunları yapmanıza olanak sağlar:

* Aynı bölgedeki Resource Manager sanal ağlarına kaynaklara erişme
* Hizmet uç noktaları ile güvenli kaynaklara erişme 
* ExpressRoute veya VPN bağlantıları erişilebilen kaynaklara erişme

![Sanal Ağ Tümleştirmesi](media/networking-features/vnet-integration.png)

Bu özellik Önizleme aşamasındadır ve üretim iş yükleri için kullanılmamalıdır. Bu özellik hakkında daha fazla bilgi edinmek için belgeleri okuyun [App Service VNet tümleştirmesi][vnetintegration].

## <a name="app-service-environment"></a>App Service Ortamı 

App Service ortamı (ASE), Vnet'te çalışan Azure App Service'in tek kiracılı dağıtımıdır. ASE gibi kullanım örneklerini sağlar:

* Sanal ağınızdaki kaynaklara erişmek
* ExpressRoute üzerinden erişim kaynakları
* Özel bir adres kullanarak sanal ağınızdaki uygulamalarınızla kullanıma sunma 
* Hizmet uç noktaları arasında erişim kaynakları 

Bir ASE ile ASE ağınızda zaten olduğundan, sanal ağ tümleştirmesi veya hizmet uç noktaları gibi özellikleri kullanmak gerekmez. Hizmet uç noktaları üzerinden SQL veya depolama gibi kaynaklara erişmek istiyorsanız, ASE alt ağdaki hizmet uç noktalarını etkinleştirin. Sanal ağ içindeki kaynaklara erişmek istiyorsanız, gereken ek yapılandırma yoktur.  ExpressRoute kaynaklara erişmek istiyorsanız, VNet içinde zaten bulunan ve hiçbir şey ASE veya içindeki uygulamalar yapılandırmak gerekmez. 

Bir ILB ase'deki uygulamalar üzerinde özel bir IP adresi kullanıma sunulabilecek çünkü yalnızca internet'e istediğiniz ve rest güvenliğini uygulamalar kullanıma sunmak için WAF cihazları kolayca ekleyebilirsiniz. Kendisi çok katmanlı uygulamaları kolayca geliştirme için uygundur. 

Bir ASE'de olan henüz çok kiracılı hizmetinden mümkün olmayan bazı şeyler vardır. Bu gibi işlemler şunlardır:

* Özel bir IP adresi uygulamalarınızı kullanıma sunma
* Uygulamanızın bir parçası olmayan ağ denetimleri ile tüm giden trafiğin güvenliğini sağlama 
* Tek kiracılı bir hizmet olarak uygulamalarınızı barındırın 
* Çok kiracılı bir hizmet olası daha çok fazla örneğe ölçeği artırma 
* Uç noktaları yük özel CA istemci sertifikaları özel CA'yı uygulamalarınızla tarafından kullanılmak üzere güvenli 
* Tüm uygulama düzeyinde devre dışı bırakma olanağı olmadan sisteminde barındırılan uygulamalar, TLS 1.1 zorla 
* Tüm olan tüm müşterileri paylaşılmıyor, ase'deki uygulamalar için ayrılmış bir çıkış adresi sağlayın. 

![Bir vnet'teki ASE](media/networking-features/app-service-environment.png)

ASE yalıtılmış ve ayrılmış uygulama barındırma etrafında en iyi hikayeyi sağlar, ancak bazı yönetim zorluklar geliyor. İşletimsel bir ASE kullanmadan önce dikkat etmeniz gerekenler şunlardır:
 
 * ASE, Vnet'inizde çalışır ancak VNet dışında bağımlılıkları vardır. Bu bağımlılıkların izin verilmesi gerekir. Daha fazla bilgi [için App Service ortamı ağ konuları][networkinfo]
 * Bir ASE, çok kiracılı bir hizmet gibi hemen ölçeklenmez. Öngörülebiliyorsa ölçeklendirmek yerine ölçeklendirme gereksinimlerini öngörmek gerekir. 
 * Bir ASE daha yüksek bir peşin ücret ilişkili sahip. En iyi ASE'niz için bir ASE içinde çok sayıda iş yükü yerleştirme planlama yerine, küçük çalışmaları için kullanılan olması
 * Bir ase'deki uygulamalar ASE ve diğer bazı uygulamalar için erişim kısıtlayamazsınız.
 * ASE alt ağda olduğunu ve tüm gelen ve giden trafiği, ASE herhangi bir ağ kuralları uygulanır. Yalnızca bir uygulama için gelen trafik kuralları atamak istiyorsanız, erişim kısıtlamaları kullanın. 

## <a name="combining-features"></a>Birleştirme özellikleri 

Çok kiracılı bir hizmet için belirtilen özellikleri birlikte daha ayrıntılı kullanım durumları çözmek için kullanılabilir. İki daha yaygın kullanım örnekleri, burada açıklanan ancak bunlar yalnızca örnektir. Çeşitli özelliklerini neler anlayarak, neredeyse tüm sistem mimarisinin gereksinimlerinizi çözebilirsiniz.

### <a name="inject-app-into-a-vnet"></a>Uygulama bir Vnet'e ekleme

Genel sanal ağ içinde uygulamanızı nasıl isteğidir. Uygulamanızı bir Vnet'e koyarak, bir uygulama için gelen ve giden uç noktaları bir sanal ağ içinde olduğunu anlamına gelir. Bu sorunu çözmek için en iyi çözüm ASE sağlar ancak ile çok kiracılı bir hizmet özellikleri bir araya getirerek ihtiyacınız olan şey birçok alabilirsiniz. Örneğin, yalnızca intranet uygulamalarını özel adresleriyle gelen ve giden tarafından barındırabilirsiniz:

* Özel gelen ve giden adresi ile bir uygulama ağ geçidi oluşturma
* Hizmet uç noktaları ile uygulamanıza gelen trafiği güvenli hale getirme 
* Arka uç uygulamanızın ağınızda bu nedenle, yeni VNet tümleştirmesi kullanın 

Bu dağıtım stili değil İnternet'e giden trafik için adanmış bir adres size mi uygulamanızdan giden tüm trafiği kilitlemenize olanak tanır.  Bu dağıtım stili çok ne ile bir ASE yalnızca aksi elde edebileceğiniz, verirsiniz. 

### <a name="create-multi-tier-applications"></a>Çok katmanlı uygulamalar oluşturma

Çok katmanlı bir uygulama, burada API arka uç uygulamaları yalnızca ön uç katmanından erişilebilir bir uygulamadır. Çok katmanlı bir uygulama oluşturmak için şunları yapabilirsiniz:

* Bir vnet'teki bir alt ağ ile arka uca ön uç web uygulamanızın bağlamak için VNet Tümleştirmesi'ni kullanın
* Ön uç web uygulamanız tarafından kullanılan alt ağ yalnızca geldiği API uygulamanıza gelen trafiği güvenli hale getirmek için hizmet uç noktaları kullanma

![çok katmanlı uygulama](media/networking-features/multi-tier-app.png)

Aynı API uygulaması VNet tümleştirmesi diğer ön uç uygulama ve hizmet uç noktalarından alt ağlarını API uygulamasında kullanarak birden çok ön uç uygulama olabilir.  

<!--Links-->
[appassignedaddress]: https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl#bind-your-ssl-certificate
[iprestrictions]: https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions
[serviceendpoints]: https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions
[hybridconn]: https://docs.microsoft.com/azure/app-service/app-service-hybrid-connections
[vnetintegrationp2s]: https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet
[vnetintegration]: https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet
[networkinfo]: https://docs.microsoft.com/azure/app-service/environment/network-info
