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
ms.date: 06/14/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: b269c75be7fec55fb77afecc6d04b86266c74a6f
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147297"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Uygulamanızı bir Azure sanal ağı ile tümleştirme
Bu belgede Azure App Service sanal ağ tümleştirme özelliği ve uygulamalar ile ayarlama açıklanır [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714). [Azure sanal ağları] [ VNETOverview] (Vnet'ler) birçok Azure kaynaklarınızın bir internet olmayan routeable ağında yerleştirin izin verir.  

Azure App Service iki biçimi vardır. 

1. Fiyatlandırma planı yalıtılmış dışında tam aralığını destekleyen çok kiracılı sistemleri
2. App Service ortamı (ağınızın içinde dağıtır ve yalıtılmış fiyatlandırma planı uygulamaları destekler ASE),

Bu belge için çok kiracılı App Service kullanımda olan iki sanal ağ tümleştirme özellikleriyle gider. Uygulamanızı ise [App Service ortamı][ASEintro], bir sanal ağda zaten ve aynı sanal ağ kaynaklarına ulaşmak için VNet tümleştirmesi özelliğinin gerektirmez. Tüm App Service ağ özellikleri hakkında daha fazla bilgi için okuma [App Service ağ özellikleri](networking-features.md)

VNet tümleştirme özelliğini iki biçimi vardır

1. Bir sürümü, aynı bölgedeki sanal ağlar ile tümleştirme sağlar. Bu özelliğin biçimi, aynı bölgedeki bir sanal ağ içindeki alt ağ gerektirir. Bu özellik hala Önizleme aşamasındadır ancak bazı uyarılar aşağıda belirtildiği ile Windows app üretim iş yükleri için desteklenir.
2. Başka bir sürüm, Klasik sanal ağları veya diğer bölgelerdeki sanal ağlar ile tümleştirme sağlar. Sanal ağınızı bir sanal ağ geçidi dağıtımına özellik bu sürümü gerektirir. Bu noktadan siteye VPN temel özelliğidir.

Uygulama, aynı anda yalnızca sanal ağ tümleştirme özelliği, bir form kullanabilirsiniz. Sorunun daha sonra hangi özelliğini kullanmanız gerekir ' dir. Birçok şey için kullanabilirsiniz. Ancak NET erişilebilirliktir şunlardır:

| Sorun  | Çözüm | 
|----------|----------|
| Aynı bölgede bir RFC 1918 adresi (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) erişmek istediğiniz | Bölgesel sanal ağ tümleştirmesi |
| Klasik sanal ağ veya başka bir bölgedeki bir sanal ağa erişmek istediğiniz | ağ geçidi gerekli VNet tümleştirmesi |
| ExpressRoute RFC 1918 uç noktalarına erişmek istediğiniz | Bölgesel sanal ağ tümleştirmesi |
| Hizmet uç noktaları arasında kaynaklarına ulaşmak istiyorsanız | Bölgesel sanal ağ tümleştirmesi |

Her iki özelliği, ExpressRoute RFC 1918 adreslerine ulaşmak etkinleştirilir. Bunu yapmak için şu an için bir ASE kullanmanız gerekir.

Bölgesel sanal ağ Tümleştirmesi'ni kullanarak şirket içi ağınıza bağlantı kurulamadı veya hizmet uç noktalarını yapılandırma. Ağ yapılandırmasını ayrı olmasıdır. Bölgesel sanal ağ tümleştirmesi yalnızca bu bağlantı türleri arasında arama yapmak uygulamanızı sağlar.

Kullanılan sürümünden bağımsız olarak sanal ağ tümleştirmesi, web uygulaması sanal ağınızdaki kaynaklara erişmenizi sağlar ancak gelen özel web uygulamanıza sanal ağdan erişim yoktur. Özel site erişimi, uygulamanızı yalnızca özel ağdan gibi bir Azure sanal ağı içinde erişilebilir hale getirmek için ifade eder. Sanal ağ tümleştirmesi yalnızca, sanal ağ içinde uygulamanızdan giden çağrıları yapmak için ' dir. 

VNet tümleştirmesi özelliği:

* Standart, Premium veya PremiumV2 fiyatlandırma planı gerektirir 
* TCP ve UDP destekler
* App Service uygulamaları ve işlev uygulamaları ile çalışır

VNet tümleştirmesi dahil olmak üzere desteklemiyor bazı şeyler vardır:

* bir sürücü bağlama
* AD tümleştirmesi 
* NetBios

## <a name="regional-vnet-integration"></a>Bölgesel sanal ağ tümleştirmesi 

VNet tümleştirmesi, uygulamanız ile aynı bölgedeki sanal ağlar ile kullanıldığında temsilci olarak atanan bir alt ağ ile en az 32 adres kullanımını gerektirir. Alt ağ başka bir şey için kullanılamaz. Uygulamanızdan giden çağrıların temsil edilen alt adreslerinden yapılır. VNet tümleştirmesi bu sürümünü kullandığınızda, sanal ağınızdaki adreslerinden çağrı yapılır. Adresleri ağınızda kullanarak uygulamanıza sağlar:

* Hizmet uç noktası güvenli hale getirilen hizmetlere çağrı yapın
* ExpressRoute bağlantısı üzerinden erişim kaynakları
* Bağlandığınız sanal ağ kaynakları
* ExpressRoute bağlantıları dahil olmak üzere eşlenmiş bağlantılar üzerinden kaynaklara erişmek

Bu özellik Önizleme aşamasındadır ancak aşağıdaki sınırlamalara sahip Windows uygulama üretim iş yükleri için desteklenir:

* RFC 1918 aralık içinde adresleri yalnızca ulaşabilirsiniz. 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 adres bloklarını adresleri olanlardır.
* Genel eşleme bağlantıları arasında kaynaklarına erişemiyor
* yollar, sanal ağ içinde uygulamanızdan gelen trafiği ayarlanamaz
* Bu özellik yalnızca PremiumV2 App Service planları destekleyen yeni App Service ölçek birimleri kullanılabilir.
* Bu özellik, bir App Service Ortamı'nda, yalıtılmış planı uygulamalar tarafından kullanılamaz
* Bu özellik, kullanılmayan bir alt ağ ile Resource Manager sanal ağınızdaki en az 32 adres gerektirir.
* Uygulama ve sanal ağ aynı bölgede olması gerekir
* Bir adres, her bir App Service planı örneği için kullanılır. Alt ağ boyutu atamasından sonra değiştirilemez olduğundan, birden fazla maksimum ölçek boyutunuzu ele bir alt ağ kullanın. 32 adreslerine sahip bir/27, 20 örnek için ölçeklendirilebilir bir App Service planı kapsayan önerilen boyutu aynıdır.
* Tümleşik bir uygulama ile bir Vnet'i silemiyor. Tümleştirme önce kaldırmanız gerekir 
* App Service planı başına yalnızca bir bölgesel sanal ağ tümleştirmesi olabilir. Aynı sanal ağda birden fazla uygulama aynı App Service planında kullanabilirsiniz. 

Linux için de önizleme özelliğidir. Aynı bölgede bir Resource Manager sanal ağı ile VNet tümleştirme özelliğini kullanmak için:

1. Portal ağ Arabirimine gidin. Ardından uygulamanızın yeni özelliğini kullanabilmek için sanal ağa ekleme (Önizleme) bir seçenek görürsünüz.  

   ![VNet tümleştirmesi seçin][6]

1. Seçin **VNet Ekle (Önizleme)** .  

1. Resource Manager ile tümleştirin ve daha sonra yeni bir alt ağ oluşturmak veya önceden var olan boş bir alt ağ seçin için istediğiniz sanal ağı seçin. Tümleştirme tamamlanması bir dakikadan kısa sürer. Tümleştirme sırasında uygulamanızı yeniden başlatılır.  Tümleştirme tamamlandığında, sanal ağ ile tümleştirilir ve özellik Önizleme aşamasındadır bildiren bir başlık ayrıntıları görürsünüz.

   ![VNet ve alt ağ seçin][7]

Uygulamanız, sanal ağ ile tümleştirildiğinde, Vnet'inizi yapılandırılmış DNS sunucusunu kullanır. 

Uygulamanızı sanal ağdan bağlantıyı kesmek için seçin **Bağlantıyı Kes**. Bu, web uygulamanızı yeniden başlatır. 

Yeni VNet tümleştirme özelliği, hizmet uç noktaları kullanmanıza olanak sağlar.  Hizmet uç noktaları ile uygulamanızı kullanmak için seçili bir sanal ağa bağlanmak ve ardından tümleştirmesi için kullanılan alt ağdaki hizmet uç noktaları yapılandırmak için yeni VNet Tümleştirmesi'ni kullanın. 

#### <a name="web-app-for-containers"></a>Kapsayıcılar için Web App

App Service, Linux üzerinde yerleşik görüntü ile kullanırsanız, bölgesel sanal ağ tümleştirme özelliği ek değişikliğe gerek kalmadan çalışır. Kapsayıcılar için Web uygulaması kullanıyorsa, docker görüntünüzü VNet tümleştirmesi kullanmak için değiştirmeniz gerekir. Docker görüntünüzü bağlantı noktası ortam değişkeni olarak bir sabit kodlanmış bağlantı noktası numarası kullanmak yerine ana web sunucusunun dinleme bağlantı noktasını kullanın. Bağlantı noktası ortam değişkeni, kapsayıcı başlatma zaman App Service platformu tarafından otomatik olarak ayarlanır.

### <a name="how-vnet-integration-works"></a>VNet tümleştirmesi nasıl çalışır?

App Service uygulamaları, çalışan rolü barındırılır. Barındırma planları temel ve daha yüksek fiyatlandırma planları ayrılmış olduğu aynı çalışanları üzerinde çalışan diğer müşterilerin iş yükleri. VNet tümleştirmesi, temsilci alt adreslerine sahip sanal arabirimi bağlayarak çalışır. Çünkü gelen adresidir: ağınızdaki, yalnızca sanal ağınızdaki bir VM ile aynı şekilde bu bilgilerin çoğunu veya ağınız aracılığıyla erişebilir. Ağ uygulama, sanal ağınıza ve diğer bir deyişle neden bazı ağ özellikleri henüz bu özelliği kullanırken kullanılamayan bir VM çalıştıran daha farklıdır.

![Sanal Ağ Tümleştirmesi](media/web-sites-integrate-with-vnet/vnet-integration.png)

VNet tümleştirmesi etkin olduğunda, uygulamanızın normal olarak aynı Kanallar üzerinden İnternet'e giden çağrıları hale getirir. Uygulama özellikleri Portalı'nda listelenen giden adresleri, yine de uygulamanız tarafından kullanılan adresleridir. Değişiklikleri uygulamanız için hizmet uç noktasına çağrı güvenli hale getirilen hizmetlere veya RFC 1918 adresleri olan, Vnet'te gider. 

Özelliği, yalnızca çalışan her bir sanal arabirim destekler.  Çalışan her bir sanal arabirim App Service planı başına bir bölgesel sanal ağ tümleştirmesi anlamına gelir. Tüm uygulamaları aynı App Service planında aynı VNet tümleştirmesi kullanabilirsiniz, ancak ek bir sanal ağa bağlanmak için bir uygulama gerekiyorsa, başka bir App Service planı oluşturmanız gerekir. Kullanılan sanal arabirim müşteriler doğrudan erişimi olan bir kaynak değil.

Bu teknolojinin nasıl çalıştığı yapısı nedeniyle, VNet Tümleştirmesi ile kullanılan trafiği Ağ İzleyicisi veya NSG akış günlüklerini görünmez.  

## <a name="gateway-required-vnet-integration"></a>ağ geçidi gerekli VNet tümleştirmesi 

Ağ geçidi, sanal ağ tümleştirme özelliği gerekli:

* herhangi bir bölgedeki sanal ağları Resource Manager veya Klasik sanal ağları olmaları bağlanmak için kullanılan
* aynı anda yalnızca 1 sanal ağa bağlanmak bir uygulama sağlar
* bir App Service planında tümleştirilmesini ile beş adede kadar sanal ağlar sağlar 
* App Service planı birden fazla uygulama tarafından bir App Service planı tarafından kullanılan toplam sayı etkilemeden kullanılacak aynı sanal ağda izin verir.  6 varsa aynı sanal ağda aynı App Service planında kullanarak uygulamaları, sayar 1 VNet kullanılıyor. 
* Noktadan siteye VPN ile yapılandırılmış bir sanal ağ geçidi gerektirir
* Linux uygulamaları ile kullanmak için desteklenmiyor.
* Ağ geçidi üzerinde nedeniyle SLA'sı % 99,9 SLA destekler

Bu özelliği desteklemez:

* ExpressRoute üzerinden kaynaklara erişme 
* Hizmet Uç Noktaları üzerinden kaynaklara erişme 

### <a name="getting-started"></a>Başlarken

Aşağıda, web uygulamanızı bir sanal ağa bağlamadan önce göz önünde bulundurmanız gereken bazı hususlar verilmiştir:

* Hedef sanal ağ ile rota tabanlı ağ geçidi uygulamasına bağlı önce etkin noktadan siteye VPN olması gerekir. 
* Sanal ağ, App Service Plan(ASP) ile aynı abonelikte olması gerekir.
* Bir sanal ağ ile tümleştirilen uygulamalar, o VNet için belirtilen DNS kullanın.

### <a name="set-up-a-gateway-in-your-vnet"></a>Sanal bir ağ geçidi ayarlama ###

Noktadan siteye adresleriyle yapılandırılmış bir ağ geçidi zaten varsa, sanal ağ Tümleştirmesi ile uygulamanızı yapılandırmaya atlayabilirsiniz.  
Bir ağ geçidi oluşturmak için:

1. [Bir ağ geçidi alt ağı oluşturmanız] [ creategatewaysubnet] sanal.  

1. [VPN ağ geçidi oluşturma][creategateway]. Bir rota tabanlı VPN türü seçin.

1. [Noktası site adresleri olarak ayarlanmış][setp2saddresses]. Ağ geçidi temel SKU'da değilse, IKEv2 noktadan siteye yapılandırmasında devre ve SSTP seçilmelidir. Adres alanı olmalıdır RFC 1918 adres bloklarını, 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16

Yalnızca varsa ağ geçidi oluşturma App Service ile VNet tümleştirmesi kullanın, ardından bir sertifikayı karşıya yüklemek gerekmez. Ağ geçidi oluşturma, 30 dakika sürebilir. Ağ geçidi sağlanana kadar uygulamanız, sanal ağ ile tümleştirmek mümkün olmayacaktır. 

### <a name="configure-vnet-integration-with-your-app"></a>VNet Tümleştirmesi ile uygulamanızı yapılandırın 

VNet tümleştirmesi uygulamanızı etkinleştirmek için: 

1. Azure portalında uygulamanıza gidin ve uygulama ayarları'nı açın ve ağ seçin > VNet tümleştirmesi. ASP standart SKU'da veya her iki VNet tümleştirme özelliğini kullanmak için daha iyi gerekir. 
 ![VNet tümleştirmesi kullanıcı Arabirimi][1]

1. Seçin **VNet ekleme**. 
 ![VNet tümleştirmesi ekleme][2]

1. Sanal ağınıza seçin. 
  ![Sanal ağınıza seçin][8]
  
Uygulamanızın son Bu adımdan sonra yeniden başlatılır.  

### <a name="how-the-gateway-required-vnet-integration-feature-works"></a>Ağ geçidi VNet tümleştirme özelliğini nasıl gerekli çalışır

VNet tümleştirme özelliğini noktadan siteye VPN teknolojisi üzerine kurulmuştur ağ geçidi gereklidir. Noktadan siteye teknoloji yalnızca uygulamayı barındıran sanal makine ağ erişimi sınırlar. Yalnızca trafik İnternet'e VNet tümleştirmesi veya karma bağlantılar üzerinden göndermek için kısıtlı uygulamalardır. 

![VNet tümleştirmesi nasıl çalışır?][3]

## <a name="managing-vnet-integration"></a>VNet tümleştirmesi yönetme
Bağlanmak ve bir sanal ağa bağlantısını kesmek için bir uygulama düzeyinde yeteneğidir. Birden fazla uygulama arasında VNet tümleştirmesi etkileyen işlemleri App Service planı düzeyindedir. Uygulamasından > Ağ > sanal ağ tümleştirmesi portal, sanal ağınızda ayrıntılarını alabilirsiniz. ASP ASP düzeyinde benzer bilgileri görebilirsiniz > Ağ > sanal ağ tümleştirmesi portal bu App Service planında hangi uygulamaların belirli bir tümleştirme kullanıyorsanız dahil olmak üzere.

 ![Sanal ağ ayrıntıları][4]

Kullanımına sahip bilgileri, sanal ağ tümleştirmesi kullanıcı arabiriminde olan uygulama ve ASP portalları arasında aynı.

* VNet adı - kullanıcı Arabirimi sanal ağa bağlantılar
* Konumu - konum, sanal ağınızın yansıtır. Başka bir konumda bir sanal ağ ile tümleştirme, uygulamanız için gecikme sorunlarına neden olabilir. 
* Sertifika durumu - sertifikalarınızı App Service planınızı ağınız arasında eşit olup olmadığını gösterir.
* Ağ geçidi durumu -, kullanıyor ağ geçidi gerekli VNet tümleştirmesi, ağ geçidi durumunu görebilirsiniz.
* VNet adres alanının - sanal ağınıza ait IP adresi alanını gösterir. 
* Noktadan siteye adres alanı - sanal ağınıza ait site IP adresi alanı noktasına gösterir. Ağ geçidi gerekli özelliğini kullanırken Vnet'inizi çağırıyor yaparken, uygulama Kimden adresinizi Bu adreslerden birini olacaktır. 
* Siteden siteye adres alanı - şirket içi kaynaklarınıza veya başka bir sanal ağ için sanal ağınıza bağlanmak için siteden siteye VPN'ler kullanabilirsiniz. Bu VPN bağlantısı ile tanımlanan IP aralıklarını burada gösterilir.
* DNS sunucuları - DNS sunucuları, VNet ile yapılandırılan gösterir.
* -Vnet'e yönlendirilmiş IP adresleri, Vnet'te sürücü trafiği için kullanılan adres bloklarını yönlendirilen gösterir 

VNet tümleştirmenizi uygulama görünümünü gerçekleştirebileceğiniz tek işlem uygulamanız şu anda bağlı olduğu sanal ağ bağlantısını kesmeyi sağlamaktır. Uygulamanızı bir sanal ağdan bağlantıyı kesmek için seçin **Bağlantıyı Kes**. Bir sanal ağdan ayırdığınızda, uygulamanızı yeniden başlatılır. Bağlantıyı kesme, sanal ağınıza değişikliğe neden olmaz. Alt ağ veya ağ geçidi kaldırılmaz. Ardından, sanal ağı silmek istiyorsanız, ilk uygulamanızı sanal ağdan bağlantısını kesmek ve ağ geçitleri gibi kaynakları da silmeniz gerekir. 

ASP VNet tümleştirmesi UI ulaşmak için ASP UI'ı açın ve seçin **ağ**.  VNet tümleştirmesi altında seçin **yapılandırmak için buraya tıklayın** ağ özelliği durumu kullanıcı arabirimini açın.

![ASP VNet tümleştirmesi bilgileri][5]

ASP VNet tümleştirmesi UI tüm ASP uygulamaları tarafından kullanılan sanal ağlar gösterilir. Her VNet üzerinde ayrıntıları görmek için ilgilendiğiniz sanal ağa tıklayın. Burada gerçekleştirebileceğiniz iki eylemler vardır.

* **Eşitleme ağ**. Eşitleme ağ işlemi yalnızca ağ geçidi bağlı VNet tümleştirmesi özelliğidir. Eşitleme ağ işlemi gerçekleştirilirken sertifikaları ve ağ bilgilerini eşitlenmiş olmasını sağlar. Eklediğinizde veya değiştirdiğinizde, sanal ağınızın DNS, gerçekleştirmeniz gereken bir **eşitleme ağ** işlemi. Bu işlem, bu sanal ağ kullanarak herhangi bir uygulamayı yeniden başlatır.
* **Yollar** yollar ekleme sürücü giden trafik, sanal ağ içinde.

**Yönlendirme** ağınızda tanımlanmış rotalar uygulamanızı ağınızdan içine trafiği yönlendirmek için kullanılır. Vnet'te ek giden trafiği göndermek gerekiyorsa, bu adres blokları ekleyebilirsiniz. Bu özellik yalnızca çalışır ağ geçidi ile VNet tümleştirmesi gereklidir.

**Sertifikaları** VNet tümleştirmesi etkin ağ geçidi gerekli olduğunda, gerekli bir bağlantının güvenliğini sağlamak için sertifika alışverişi yoktur. Sertifikaları yanı sıra DNS yapılandırmasını, yollar ve ağ açıklayan diğer benzer bir şey var.
Sertifikaları veya ağ bilgileri değiştirilirse, "Ağı Eşitle"'ye tıklamanız. "Ağı Eşitle"'ye tıkladığınızda, kısa bir kesinti bağlantısı uygulamanızı sanal ağınıza arasında neden. Uygulamanızı yeniden değildir, ancak bağlantı kaybı düzgün sitenize çalışmamasına neden olabilir. 

## <a name="accessing-on-premises-resources"></a>Erişim şirket içi kaynaklara
Uygulamalar, siteden siteye bağlantı ile sanal ağlar'ı tümleştirerek, şirket içi kaynaklara erişebilir. Kullanıyorsanız Ağ Geçidi gerekli VNet tümleştirmesi, noktadan siteye adres blokları, şirket içi VPN ağ geçidi yollarınızı güncelleştirmeniz gerekir. Siteden siteye VPN ilk ayarlandığında yapılandırmak için kullanılan komut rotalar düzgün şekilde ayarlamanız gerekir. Siteden siteye VPN oluşturduktan sonra noktadan siteye adres eklerseniz, yolların el ile güncelleştirmeniz gerekir. Bunu nasıl yapacağınız hakkında ayrıntılı bilgi, ağ geçidi değişir ve burada açıklanmamaktadır. Siteden siteye VPN bağlantısı ile yapılandırılmış BGP sahip olamaz.

Ağınız üzerinden ve şirket içi ulaşmak bölgesel sanal ağ tümleştirme özelliği için gereken ek yapılandırma yoktur. Şirket içi ağınıza bağlanmak yeterlidir ExpressRoute veya siteden siteye VPN kullanarak. 

> [!NOTE]
> Ağ geçidi, sanal ağ tümleştirme özelliği, bir ExpressRoute ağ geçidi olan bir VNet ile bir uygulama tümleştirilemeyeceğini gereklidir. ExpressRoute ağ geçidi olarak yapılandırılmış olsa bile [bir arada bulunma modu] [ VPNERCoex] VNet tümleştirmesi çalışmaz. Bir ExpressRoute bağlantısı üzerinden kaynaklara erişmeye ihtiyacınız varsa bölgesel sanal ağ tümleştirme özelliği kullanabilirsiniz veya bir [App Service ortamı][ASE], ağınızda çalışır. 
> 
> 

## <a name="peering"></a>Eşleme
Eşleme bölgesel sanal ağ Tümleştirmesi ile kullanıyorsanız, herhangi bir ek yapılandırma yapmanız gerekmez. 

Kullanıyorsanız Ağ Geçidi eşlemesi ile sanal ağ tümleştirmesi gerekli, bazı ek öğeler yapılandırmanız gerekir. Uygulamanızın üzerinde çalışmak için eşlemeyi yapılandırmak için:

1. Uygulamanızı VNet üzerinde eşleme bağlantıları bağlandığı ekleyin. Eşleme bağlantısını eklerken etkinleştirme **sanal ağ erişimine izin ver** ve **iletilen trafiğe izin ver** ve **ağ geçidi aktarımına izin ver**.
1. Bağlandığınız sanal ağa eşlenen bir VNet üzerinde bir eşleme bağlantısı ekleyin. Hedef VNet eşleme bağlantısı eklerken, etkinleştirme **sanal ağ erişimine izin ver** ve **iletilen trafiğe izin ver** ve **uzak ağ geçitlerini izin**.
1. App Service planı'na gidin > Ağ > sanal ağ tümleştirmesi UI Portalı'nda.  Uygulamanıza bağlanan sanal ağ seçin. Yönlendirme bölümü altında uygulamanızın bağlı olduğu sanal ağ eşlendikten vnet'in adres aralığını ekleyin.  


## <a name="pricing-details"></a>Fiyatlandırma ayrıntıları
Fiyatlandırma katmanı ücretleri ASP ötesinde kullanım için ek ücret ödemeden bölgesel sanal ağ tümleştirme özelliği vardır.

Ağ geçidi gerekli VNet tümleştirme özelliğini kullanımını üç ilgili ücretler vardır:

* ASP fiyatlandırma katmanı ücretleri - uygulamalarınızı bir standart, Premium veya PremiumV2 App Service planı içinde olması gerekir. Burada bu maliyetlerinden daha fazla bilgi görebilirsiniz: [App Service fiyatlandırması][ASPricing]. 
* VNet aynı veri merkezinde olsa bile veri çıkışı için ücret olan veri aktarım maliyetleri - vardır. Bu ücretler açıklanan [veri aktarma fiyatlandırma ayrıntıları][DataPricing]. 
* VPN ağ geçidi maliyetleri - Burada, VNet ağ geçidine noktadan siteye VPN için gerekli olan bir maliyeti olur. Ayrıntıları bulunan [VPN Gateway fiyatlandırması] [ VNETPricing] sayfası.


## <a name="troubleshooting"></a>Sorun giderme
Özellik ayarlamak kolay olsa da, bu deneyiminiz sorun ücretsiz olacaktır anlamına gelmez. Karşılaşmamanız gerekir, istenen uç noktası var. erişme uygulama konsolundan bağlantıyı test etmek için kullanabileceğiniz bazı yardımcı programlar sorunlardır. Kullanabileceğiniz iki konsol vardır. Kudu konsolunu biridir ve diğer Azure portalında konsoldur. Git araçları için Kudu Konsolu uygulamanızdan ulaşmak için Kudu ->. Bu [siteadı] için giderek ile aynı olur. scm.azurewebsites.net. Açıldıktan sonra hata ayıklama Konsolu sekmesine gidin. Git araçları için Azure portal barındırılan konsola ardından uygulamanızdan almak için konsolu ->. 

#### <a name="tools"></a>Araçlar
Araçlar **ping**, **nslookup** ve **tracert** güvenlik kısıtlamaları nedeniyle konsolu üzerinden çalışmaz. Void eklenen iki ayrı araçları doldurmak için. DNS işlevselliğini test etmek için nameresolver.exe adlı bir araç ekledik. Sözdizimi şöyledir:

    nameresolver.exe hostname [optional: DNS Server]

Kullanabileceğiniz **nameresolver** uygulamanızın bağımlı ana bilgisayar adları kontrol etmek için. Herhangi bir şey yanlış DNS ile yapılandırılmış olması ya da belki de, DNS sunucusuna erişiminiz yoksa, bu şekilde, test edebilirsiniz. Uygulamanızı ortam değişkenleri WEBSITE_DNS_SERVER ve WEBSITE_DNS_ALT_SERVER bakarak konsolda kullanacağı DNS sunucusu görebilirsiniz.

Sonraki aracı, bir konak ve bağlantı noktası birleşimi TCP bağlantısını test etmek sağlar. Bu aracı adlı **tcpping** ve söz dizimi şu şekildedir:

    tcpping.exe hostname [optional: port]

**Tcpping** yardımcı programı bildirir belirli ana bilgisayarı ve bağlantı noktası ulaşması durumunda. Yalnızca başarı gösterebilirsiniz varsa: konak ve bağlantı noktası birleşimini dinleyen bir uygulamanın yoktur ve belirtilen konak ve bağlantı noktası uygulamanızdan ağ erişimi yoktur.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>Kaynakları barındıran sanal ağdan sanal ağa erişim hata ayıklama
Uygulamanız belirli ana bilgisayarı ve bağlantı noktası ulaşmasını önleyen etmenizi vardır. Çoğu zaman bu üç şeylerden biri değil:

* **Bir güvenlik duvarı içinde yoludur.** Biçiminde bir güvenlik duvarı varsa, TCP zaman aşımı ulaşırsınız. TCP zaman aşımı 21 bu durumda saniyedir. Kullanım **tcpping** bağlantısını test etmek için aracı. TCP zaman aşımı, güvenlik duvarları ötesinde pek çok nedeni olabilir ancak kavramla başlayacağız. 
* **DNS erişilebilir durumda değil.** DNS zaman aşımı, DNS sunucusu başına üç saniyedir. İki DNS sunucusu varsa, zaman aşımı 6 saniyedir. DNS çalışıp çalışmadığını görmek için nameresolver kullanın. Ağınızın yapılandırılmış DNS kullanmaz olarak nslookup kullanamazsınız unutmayın. Erişilemez bir güvenlik duvarı olabilir veya NSG erişimi engelleme DNS veya kullanılamıyor olabilir.

Bu öğeleri sorunlarınızı alamıyorsanız, gibi şeyler için ilk bakış: 

**Bölgesel sanal ağ tümleştirmesi**
* Hedef bir RFC 1918 adresi mi
* Tümleştirme alt ağdan bir NSG engelleme çıkış yok
* ExpressRoute veya VPN giderek, şirket içi ağ geçidi, Azure yedeklemeye trafiği yönlendirmek için yapılandırılmış mı? VNet ancak şirket içinde değil, uç noktaları erişebiliyorsa, bu kontrol etmek faydalı olabilir.

**ağ geçidi gerekli VNet tümleştirmesi**
* RFC 1918 aralıklardaki noktadan siteye adres aralığı (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255)?
* Ağ geçidini oluşturan portalda olacak şekilde gösteriyor mu? Ağ geçidiniz kapalı ise, getirmeniz yedekleyin.
* Sertifikalar eşit olacak şekilde gösterme veya ağ yapılandırmasının değiştiğini şüphelendiğiniz?  Sertifikalarınızı eşitlenmiş halde değil ya da, ASP ile eşitlenmiş durumda, sanal ağ yapılandırması için yapılan bir değişikliği yapılmış şüpheleniyorsanız, "Ağı Eşitle" a basın.
* ExpressRoute veya VPN giderek, şirket içi ağ geçidi, Azure yedeklemeye trafiği yönlendirmek için yapılandırılmış mı? VNet ancak şirket içinde değil, uç noktaları erişebiliyorsa, bu kontrol etmek faydalı olabilir.

Orada ne belirli ana bilgisayar: bağlantı noktası bileşimi erişimi engelliyor göremediğiniz için ağ sorunlarını hata ayıklama kolay değildir. Nedenlerden bazıları şunlardır:

* bir güvenlik duvarı erişim noktanız site IP aralığı uygulama bağlantı noktasına engelleyen konağınızdaki sahip. Çapraz alt ağlar genellikle ortak erişim gerektirir.
* Hedef ana bilgisayarınızda çalışmıyor
* Uygulamanızı kapalı
* yanlış IP veya ana bilgisayar adı olan
* Uygulamanızın beklediğiniz değerinden farklı bir bağlantı noktasında dinliyor. Uç nokta ana bilgisayarda "netstat - aon" kullanarak, işlem kimliği dinleme bağlantı noktası ile eşleşebilir. 
* Bunlar erişim bağlantı noktası ve uygulama ana bilgisayarı, noktadan siteye IP aralığına engelleyecek şekilde yapılandırılmış, ağ güvenlik grupları

Uygulamanızı gerçekten kullandığınız hangi adresi tanımadığınız unutmayın. Tüm adres aralığından erişime gerek Tümleştirme alt ağı veya noktadan siteye adres aralığında, herhangi bir adres olabilir. 

Ek hata ayıklama adımları içerir:

* Sanal ağınızdaki bir sanal makineye bağlanın ve, kaynak konak: bağlantı noktası buradan ulaşmaya çalışır. TCP erişimi test etmek için PowerShell komutunu kullanın. **test-netconnection**. Sözdizimi şöyledir:

      test-netconnection hostname [optional: -Port]

* Bu konak için bir uygulama VM ve test erişim getirecek ve bağlantı noktası uygulama kullanarak konsolundan **tcpping**

#### <a name="on-premises-resources"></a>Şirket içi kaynaklara ####

Uygulamanızı, şirket içi kaynak ulaşamazsa, ağınızdan ulaşabileceği kaynak denetleyin. Kullanım **test-netconnection** TCP erişimi denetlemek için PowerShell komutu. Sanal makinenizin, şirket içi kaynağa ulaşamazsa, VPN veya ExpressRoute bağlantınızın düzgün şekilde yapılandırılmamış olabilir.

Sanal ağınıza barındırıyorsa, VM, şirket içi sisteminizi ulaşabilir ancak uygulamanızı olamaz ve ardından nedeni aşağıdakilerden biri olabilir:

* yollarınızı alt ağınız ile yapılandırılmamış veya site adresi aralığı, şirket içi ağ geçidi üzerine gelin
* Ağ güvenlik gruplarınızda noktadan siteye IP aralığınızı erişimi engelliyor
* Şirket içi güvenlik duvarlarınızdan noktadan siteye IP aralığınızı gelen trafiği engelliyor
* Bölgesel sanal ağ Tümleştirmesi özelliğini kullanarak bir RFC 1918 adresi erişmeye çalıştığınız


## <a name="powershell-automation"></a>PowerShell Otomasyonu

App Service, bir Azure PowerShell kullanarak sanal ağ ile tümleştirebilirsiniz. Çalıştırılmaya hazır bir betik için bkz: [Azure uygulama Hizmeti'nde bir uygulamanın bir Azure sanal ağına bağlama](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3).


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
