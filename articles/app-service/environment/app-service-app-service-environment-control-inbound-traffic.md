---
title: "Bir uygulama hizmeti ortamına gelen trafiğinin denetleme"
description: "Bir uygulama hizmeti ortamına gelen trafiği denetlemek için ağ güvenlik kuralları yapılandırma hakkında bilgi edinin."
services: app-service
documentationcenter: 
author: ccompy
manager: erikre
editor: 
ms.assetid: 4cc82439-8791-48a4-9485-de6d8e1d1a08
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: stefsch
ms.openlocfilehash: ed72bf3202d6cb2d2161bc0df693d3e6a1fc58ef
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>Bir uygulama hizmeti ortamına gelen trafiğinin denetleme
## <a name="overview"></a>Genel Bakış
Bir uygulama hizmeti ortamı oluşturulabilir **ya da** bir Azure Resource Manager sanal ağ **veya** Klasik dağıtım modeli [sanal ağ] [ virtualnetwork].  Bir uygulama hizmeti ortamı oluşturulduğunda, yeni sanal ağ ve yeni alt tanımlanabilir.  Alternatif olarak, bir uygulama hizmeti ortamı önceden var olan sanal ağ ve önceden var olan bir alt ağ içinde oluşturulabilir.  Haziran 2016'da yapılan bir değişiklikle ASEs ortak adres aralıkları veya RFC1918 adres alanları (özel adresleri) kullanan sanal ağları içinde dağıtılabilir.  Bir uygulama hizmeti ortamı oluşturma hakkında daha fazla bilgi için bkz [bir uygulama hizmeti ortamı oluşturmak için nasıl][HowToCreateAnAppServiceEnvironment].

Bir alt ağ Yukarı Akış aygıtlarını ve hizmetlerini arkasında gelen trafik HTTP ve HTTPS trafiği yalnızca gelen belirli kabul edilen şekilde kilitlemek için kullanılan bir ağ sınırında sağladığından bir uygulama hizmeti ortamı her zaman bir alt ağ içinde yukarı akış oluşturulmalıdır IP adresleri.

Bir alt ağdaki gelen ve giden ağ trafiğini kullanılarak denetlenir bir [ağ güvenlik grubu][NetworkSecurityGroups]. Gelen trafik denetleme uygulama hizmeti ortamı içeren alt ağ güvenlik kuralları bir ağ güvenlik grubu oluşturma ve ağ güvenliği atama Grup gerektirir.

Bir ağ güvenlik grubu bir alt ağa atandıktan sonra uygulama hizmeti ortamı uygulamalarında gelen trafiğe izin / izin ver dayalı engellenir ve reddetme kurallarının ağ güvenlik grubu listesinde tanımlı.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="inbound-network-ports-used-in-an-app-service-environment"></a>Gelen ağ bir uygulama hizmeti ortamı'nda kullanılan bağlantı noktaları
Bir ağ güvenlik grubu ile gelen ağ trafiğini kilitleme önce bir uygulama hizmeti ortamı tarafından kullanılan gerekli ve isteğe bağlı ağ bağlantı noktası kümesi bilmeniz önemlidir.  Bazı bağlantı noktaları için trafiği kapalı yanlışlıkla kapatma bir uygulama hizmeti ortamı işlev kaybı neden olabilir.

Bir uygulama hizmeti ortamı tarafından kullanılan bağlantı noktalarının bir listesi verilmiştir. Tüm bağlantı noktaları **TCP**aksi açıkça belirtilmediği sürece:

* 454: **gerekli bağlantı noktası** yönetme ve SSL üzerinden uygulama hizmeti ortamları korumaya yönelik Azure altyapısı tarafından kullanılır.  Bu bağlantı noktası trafik engellemez.  Bu bağlantı noktası her zaman bir ana genel VIP bağlıdır.
* 455: **gerekli bağlantı noktası** yönetme ve SSL üzerinden uygulama hizmeti ortamları korumaya yönelik Azure altyapısı tarafından kullanılır.  Bu bağlantı noktası trafik engellemez.  Bu bağlantı noktası her zaman bir ana genel VIP bağlıdır.
* 80: varsayılan bağlantı noktası gelen HTTP trafiği App Service planları bir uygulama hizmeti ortamında çalışan uygulamalar için.  Bir ILB özellikli ana Bu bağlantı noktası ana ILB adresine bağlanır.
* 443: varsayılan bağlantı noktası gelen SSL trafiği App Service planları bir uygulama hizmeti ortamında çalışan uygulamalar için.  Bir ILB özellikli ana Bu bağlantı noktası ana ILB adresine bağlanır.
* 21: denetim kanalı FTP için.  FTP değil kullanılıyorsa, bu bağlantı noktası güvenli bir şekilde engellenebilir.  Bir ILB özellikli ana Bu bağlantı noktası için bir ana ILB adresine bağlanabilir.
* 990: denetim kanalı FTPS için.  FTPS değil kullanılıyorsa, bu bağlantı noktası güvenli bir şekilde engellenebilir.  Bir ILB özellikli ana Bu bağlantı noktası için bir ana ILB adresine bağlanabilir.
* 10001 10020: FTP için veri kanalı.  FTP değil kullanılıyorsa, denetim kanalı olduğu gibi bu bağlantı noktalarının güvenli bir şekilde engellenebilir.  Bir ILB özellikli ana Bu bağlantı noktası ana'nın ILB adresine bağlı olabilir.
* 4016: Visual Studio 2012 ile uzaktan hata ayıklama için kullanılır.  Özelliği olmayan kullanılıyorsa, bu bağlantı noktası güvenli bir şekilde engellenebilir.  Bir ILB özellikli ana Bu bağlantı noktası ana ILB adresine bağlanır.
* 4018: Visual Studio 2013 ile uzaktan hata ayıklama için kullanılır.  Özelliği olmayan kullanılıyorsa, bu bağlantı noktası güvenli bir şekilde engellenebilir.  Bir ILB özellikli ana Bu bağlantı noktası ana ILB adresine bağlanır.
* 4020: Visual Studio 2015 ile uzaktan hata ayıklama için kullanılır.  Özelliği olmayan kullanılıyorsa, bu bağlantı noktası güvenli bir şekilde engellenebilir.  Bir ILB özellikli ana Bu bağlantı noktası ana ILB adresine bağlanır.

## <a name="outbound-connectivity-and-dns-requirements"></a>Giden bağlantısı ve DNS gereksinimleri
Bir uygulama hizmeti düzgün şekilde çalışabilmesi ortamı için çeşitli uç noktalarına giden erişim de gerektirir. Tam bir ana tarafından kullanılan dış uç noktalar "Ağ bağlantısı gerekli" bölümünde listelenmiştir [ExpressRoute için ağ yapılandırması](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) makalesi.

Uygulama hizmeti ortamları sanal ağ için yapılandırılmış geçerli bir DNS altyapısı gerektirir.  Bir uygulama hizmeti ortamı oluşturulduktan sonra DNS yapılandırmasını herhangi bir nedenle değiştirilirse, geliştiricilerin yeni DNS yapılandırmasını almak için bir uygulama hizmeti ortamı zorlayabilirsiniz.  "Yeniden Başlat" simgesini kullanarak çalışırken bir ortamı yeniden başlatma tetikleme bulunan uygulama hizmeti ortamı yönetim dikey penceresinde üstündeki [Azure portal] [ NewPortal] yeni DNS araması seçmek ortam neden olur yapılandırma.

Ayrıca, tüm özel DNS sunucuları vnet üzerinde bir uygulama hizmeti ortamı oluşturmadan önce vaktinden Kurulum olması önerilir.  Bir uygulama hizmeti ortamı oluşturulurken bir sanal ağın DNS yapılandırması değiştiyse, uygulama hizmeti ortamı oluşturma işlemi başarısız olan neden olur.  Benzer bir damarlı içinde özel bir DNS sunucusu bir VPN ağ geçidi diğer ucundaki var ve DNS sunucusu erişilemez veya kullanılamaz, uygulama hizmeti ortamı oluşturma işlemi de başarısız olur.

## <a name="creating-a-network-security-group"></a>Bir ağ güvenlik grubu oluşturma
Aşağıdaki iş nasıl ağ güvenlik grupları hakkında tam bilgi için bkz [bilgi][NetworkSecurityGroups].  Aşağıdaki örnek dokunur Azure Hizmet Yönetimi yapılandırma ve bir uygulama hizmeti ortamı içeren bir alt ağ için ağ güvenlik grubu uygulama odağı olan ağ güvenlik gruplarını vurgular.

**Not:** ağ güvenlik grupları, grafik kullanılarak yapılandırılabilir [Azure Portal](https://portal.azure.com) veya Azure PowerShell aracılığıyla.

Ağ güvenlik grupları, önce bir aboneliği ile ilişkili bir tek başına varlık olarak oluşturulur. Ağ güvenlik grupları bir Azure bölgesinde oluşturulduğundan, ağ güvenlik grubu uygulama hizmeti ortamı ile aynı bölgede oluşturulduğundan emin olun.

Aşağıdaki ağ güvenlik grubu oluşturmayı gösterir:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Bir ağ güvenlik grubu oluşturulduktan sonra bir veya daha fazla ağ güvenlik kuralları için eklenir.  Kuralları zamanla değişebileceği için kural önceliklerini zamanla ek kurallar eklemek kolaylaştırmak için kullanılan numaralandırma düzeni çıkışı boşluk tavsiye edilir.

Aşağıdaki örnek, yönetme ve uygulama hizmeti ortamını sürdürmek için Azure altyapısı tarafından gerekli yönetim bağlantı noktalarını erişim açıkça veren bir kural gösterir.  Tüm yönetim trafiğini SSL üzerinden akar ve bağlantı noktaları açılmış olsa da, bunlar Azure Yönetim altyapısı dışında herhangi bir varlık tarafından erişilemez; bu nedenle istemci sertifikaları tarafından sağlanıyorsa unutmayın.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP


80 ve 443 "Bir uygulama hizmeti ortamı Yukarı Akış aygıtları veya hizmetleri arkasındaki gizlemek için" bağlantı noktasına erişime kilitleme, Yukarı Akış IP adresini bilmeniz gerekir.  Bir web uygulaması Güvenlik Duvarı (WAF) kullanıyorsanız, örneğin, WAF kendi IP adresi (veya adresleri) ne zaman kullanan sahip bir aşağı akış uygulama hizmeti ortamı proxy trafiği.  Bu IP adresi kullanmanız gerekecektir *SourceAddressPrefix* bir ağ güvenlik kuralı parametresi.

Aşağıdaki örnekte, belirli bir Yukarı Akış IP adresinden gelen gelen trafiğe açıkça izin verilir.  Adres *1.2.3.4* yer tutucu olarak bir Yukarı Akış WAF IP adresi için kullanılır.  Yukarı Akış aygıt veya hizmet tarafından kullanılan adres değerini değiştirin.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

FTP desteği isterseniz, aşağıdaki kurallar Kanal bağlantı noktaları FTP denetim bağlantı noktası ve veri erişim vermek için bir şablon olarak kullanılabilir.  FTP durum bilgisi olan kuralıdır olduğundan, geleneksel HTTP/HTTPS güvenlik duvarı veya proxy üzerinden bir cihazın FTP trafiği yönlendirmek mümkün olmayabilir.  Bu durumda, ayarlamanız gerekir *SourceAddressPrefix* için farklı bir değer - örneğin IP adresi aralığı üzerinde hangi FTP istemcileri çalıştığını geliştirici veya dağıtım makinelerin. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Not:** veri kanalı bağlantı noktası aralığı için Önizleme dönemi boyunca değişebilir.)

Visual Studio ile uzaktan hata ayıklama kullanılırsa, aşağıdaki kurallar erişim vermek nasıl ekleyebileceğiniz gösterilmektedir.  Her bir sürümü uzaktan hata ayıklama için farklı bir bağlantı noktasını kullandığından Visual Studio'nun desteklenen her sürümü için ayrı bir kural yok.  FTP erişimli olarak, uzaktan hata ayıklama trafiği geleneksel WAF ya da proxy aygıt akışı düzgün değil.  *SourceAddressPrefix* bunun yerine Visual Studio çalıştıran Geliştirici makinelerinde IP adres aralığını ayarlayın.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-to-a-subnet"></a>Bir alt ağa bir ağ güvenlik grubu atanması
Bir ağ güvenlik grubu, tüm dış trafiğe erişimini engellediği bir varsayılan güvenlik kuralı vardır.  Kaynak adres aralığı ile ilişkili yalnızca o trafiğinden yukarıda açıklanan ağ güvenlik kuralları ve gelen trafiği engelleyen varsayılan güvenlik kuralını birleştirme gelen sonucu olan bir *izin* eylem gönderebilmesi için olacaktır Uygulama hizmeti ortamı'nda çalışan uygulamalara trafiği.

Bir ağ güvenlik grubu güvenlik kuralları ile doldurulduktan sonra uygulama hizmeti ortamı içeren alt ağına atanmış olması gerekiyor.  Atama komutu hem uygulama hizmeti ortamı bulunduğu sanal ağın adını, hem de uygulama hizmeti ortamı oluşturulduğu alt ağ adı başvurur.  

Aşağıdaki örnek, bir alt ağ ve sanal ağ için atanmış bir ağ güvenlik grubu gösterir:

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Ağ güvenlik grubu ataması başarılı olduktan sonra (ataması uzun süre çalışan işlemleri olduğu ve tamamlanması birkaç dakika sürebilir), yalnızca trafik eşleşen gelen *izin* kuralları başarıyla uygulama hizmetinde uygulamalar ulaşmak Ortam.

Eksiksiz olması için aşağıdaki örnek kaldırın ve bu nedenle ağ güvenlik grubu alt ağdan dis-ilişkilendirmek gösterilmektedir:

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Açık IP SSL için özel hususlar
Bir uygulama olan açık bir IP SSL adresi yapılandırılmışsa (uygulanabilir *yalnızca* genel VIP'ye sahip ASEs için), uygulama hizmeti ortamı varsayılan IP adresini kullanmak yerine, HTTP ve HTTPS alt akar üzerinden trafik bir 80 ve 443 numaralı bağlantı noktaları dışındaki bağlantı noktaları farklı kümesi.

Uygulama hizmeti ortamı'nın ayrıntıları UX dikey penceresinden portal kullanıcı arabiriminde her IP SSL adresi tarafından kullanılan bağlantı noktalarını tek tek çiftinin bulunabilir.  Select "tüm ayarları", "IP adreslerini"-->.  "IP adreslerini" dikey her IP SSL adresi ile ilişkili HTTP ve HTTPS trafiği yönlendirmek için kullanılan özel bağlantı noktası çifti birlikte uygulama hizmeti ortamı için açıkça yapılandırılmış tüm IP SSL adresleri tablosu gösterir.  DestinationPortRange parametrelerini kuralları içinde bir ağ güvenlik grubu yapılandırırken kullanılmalıdır bu bağlantı noktası çiftidir.

Bir uygulama bir ana üzerinde IP SSL kullanmak üzere yapılandırıldığında, dış müşterileri görmezsiniz ve özel bağlantı noktası çifti eşleme hakkında endişelenmeniz gerekmez.  Uygulamalar için trafik normal olarak yapılandırılmış IP SSL adres akar.  Çeviri özel bağlantı noktası çifti için otomatik olarak dahili olarak Yönlendirme trafiğini son leg sırasında ana içeren alt ağ gerçekleşir. 

## <a name="getting-started"></a>Başlarken
Uygulama hizmeti ortamları ile çalışmaya başlamak için bkz: [uygulama hizmeti ortamı giriş][IntroToAppServiceEnvironment]

Uygulamaları güvenli bir uygulama hizmeti ortamındaki arka uç kaynağa bağlamadan geçici daha fazla bilgi için bkz [bir uygulama hizmeti ortamı'ndan arka uç kaynaklarına güvenli bir şekilde bağlanma][SecurelyConnecttoBackend]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md
[SecurelyConnecttoBackend]:  app-service-app-service-environment-securely-connecting-to-backend-resources.md
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->

