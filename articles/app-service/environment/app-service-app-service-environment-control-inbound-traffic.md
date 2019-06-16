---
title: App Service ortamı - Azure için gelen trafiği denetleme
description: Bir App Service ortamına gelen trafiği denetlemek için ağ güvenlik kurallarını nasıl yapılandıracağınızı öğrenin.
services: app-service
documentationcenter: ''
author: ccompy
manager: erikre
editor: ''
ms.assetid: 4cc82439-8791-48a4-9485-de6d8e1d1a08
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: 84575dcb67845a074ce19cf9d819e1dda3f90e20
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62130798"
---
# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>Bir App Service ortamına gelen trafiği denetleme
## <a name="overview"></a>Genel Bakış
App Service ortamı oluşturulabilir **ya da** bir Azure Resource Manager sanal ağı **veya** Klasik dağıtım modeli [sanal ağ] [ virtualnetwork].  App Service ortamı oluşturulduğunda, yeni sanal ağ ve yeni alt ağ tanımlanabilir.  Alternatif olarak, bir App Service ortamı, önceden mevcut olan sanal ağ ve önceden var olan bir alt ağ oluşturulabilir.  Haziran 2016'da yapılan bir değişiklikle Ase'ler genel adres aralıkları ya da RFC1918 adres alanları (yani özel adresler) kullanan sanal ağlara dağıtılabilir.  Bir App Service ortamı oluşturma hakkında daha fazla ayrıntı için bkz: [bir App Service ortamı oluşturma][HowToCreateAnAppServiceEnvironment].

Bir alt ağ Yukarı Akış cihazları ve Hizmetleri arkasındaki gelen trafik HTTP ve HTTPS trafiğini yalnızca gelen belirli kabul edilir şekilde kilitlemek için kullanılan bir ağ sınırı sağlar çünkü bir App Service ortamı her zaman bir alt ağ içinde yukarı akış oluşturulmalıdır IP adresleri.

Gelen ve giden ağ trafiği bir alt ağ kullanarak kontrol edilir bir [ağ güvenlik grubu][NetworkSecurityGroups]. Gelen trafiği denetleme, App Service ortamını içeren alt ağın bir ağ güvenlik grubunda ağ güvenlik kuralları oluşturma ve sonra ağ güvenlik atayarak Grup gerektirir.

Bir ağ güvenlik grubu bir alt ağa atandığında, App Service Ortamı'ndaki uygulamaların gelen trafiğe izin / izin ver tabanlı engellenir ve Reddet kurallarının ağ güvenlik grubunda tanımlı.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="inbound-network-ports-used-in-an-app-service-environment"></a>Gelen ağ bir App Service Ortamı'nda kullanılan bağlantı noktaları
Kilitlemeden önce gelen ağ trafiği bir ağ güvenlik grubu ile bir App Service ortamı tarafından kullanılan gerekli ve isteğe bağlı ağ bağlantı noktaları kümesini bilmek önemlidir.  Bazı bağlantı noktaları için trafiği kapalı yanlışlıkla kapatma bir App Service Ortamı'nda işlevsellik kaybına neden olabilir.

Bir App Service ortamı tarafından kullanılan bağlantı noktalarının listesi aşağıdadır. Tüm bağlantı noktaları **TCP**açıkça aksi belirtilmediği sürece:

* 454:  **Gerekli bağlantı noktası** yönetmeye ve korumaya SSL aracılığıyla App Service ortamları için Azure altyapısı tarafından kullanılır.  Bu bağlantı noktası için trafiği engellemediğinizden.  Bu bağlantı noktası, ASE'nin genel VIP için her zaman bağlı.
* 455:  **Gerekli bağlantı noktası** yönetmeye ve korumaya SSL aracılığıyla App Service ortamları için Azure altyapısı tarafından kullanılır.  Bu bağlantı noktası için trafiği engellemediğinizden.  Bu bağlantı noktası, ASE'nin genel VIP için her zaman bağlı.
* 80:  App Service planları bir App Service ortamında çalışan uygulamalar için gelen HTTP trafiği için varsayılan bağlantı noktası.  ILB özellikli bir ASE'de, ase'nin ILB adresini bu bağlantı noktasına bağlıdır.
* 443: App Service planları bir App Service ortamında çalışan uygulamalar için gelen SSL trafiği için varsayılan bağlantı noktası.  ILB özellikli bir ASE'de, ase'nin ILB adresini bu bağlantı noktasına bağlıdır.
* 21:  FTP denetim kanalı.  FTP değil kullanılıyorsa, bu bağlantı noktasını güvenli bir şekilde engellenebilir.  ILB özellikli bir ASE'de, bu bağlantı noktası ASE ILB adresini bağlanabilir.
* 990:  Denetim kanalı'FTPS için.  Bu bağlantı, güvenli bir şekilde FTPS değil kullanılıyorsa engellenebilir.  ILB özellikli bir ASE'de, bu bağlantı noktası ASE ILB adresini bağlanabilir.
* 10001-10020: Veri kanalı için FTP.  FTP değil kullanılıyorsa, denetim kanalı olduğu gibi bu bağlantı noktaları güvenli bir şekilde engellenebilir.  ILB özellikli bir ASE'de, ASE'NİN'ın ILB adresini bu bağlantı noktası bağlı olabilir.
* 4016: Visual Studio 2012 ile uzaktan hata ayıklama için kullanılır.  Özellik kullanılmıyor, bu bağlantı noktasını güvenli bir şekilde engellenebilir.  ILB özellikli bir ASE'de, ase'nin ILB adresini bu bağlantı noktasına bağlıdır.
* 4018: Visual Studio 2013 ile uzaktan hata ayıklama için kullanılır.  Özellik kullanılmıyor, bu bağlantı noktasını güvenli bir şekilde engellenebilir.  ILB özellikli bir ASE'de, ase'nin ILB adresini bu bağlantı noktasına bağlıdır.
* 4020: Visual Studio 2015 ile uzaktan hata ayıklama için kullanılır.  Özellik kullanılmıyor, bu bağlantı noktasını güvenli bir şekilde engellenebilir.  ILB özellikli bir ASE'de, ase'nin ILB adresini bu bağlantı noktasına bağlıdır.

## <a name="outbound-connectivity-and-dns-requirements"></a>Giden bağlantı ve DNS gereksinimleri
Bir App Service düzgün çalışması ortamı için çeşitli uç noktalarına giden erişim de gerektirir. Tam bir ASE tarafından kullanılan dış uç noktaları "Ağ bağlantısı gerekli" bölümünde listesidir [ExpressRoute için ağ yapılandırması](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) makalesi.

App Service ortamları sanal ağ için yapılandırılmış geçerli bir DNS altyapısına gerektirir.  App Service ortamı oluşturulduktan sonra herhangi bir nedenle DNS yapılandırmasını değiştirilirse, geliştiricilerin yeni DNS yapılandırmasını seçmek için bir App Service ortamı zorlayabilirsiniz.  App Service ortamı yönetim dikey pencerede üst kısmındaki "Yeniden Başlat" simgesini kullanarak sıralı bir ortamı yeniden harekete bulunan [Azure portalında] [ NewPortal] yeni DNS seçmek için ortamı neden olur yapılandırma.

Özel DNS sunucuları vnet üzerinde App Service ortamı oluşturmadan önce önceden kurulum olması önerilir.  App Service ortamı oluşturma sırasında bir sanal ağın DNS yapılandırması değiştiyse, App Service ortamı oluşturma işlemi başarısız olmasıyla sonuçlanır.  Benzer bir damarlı içinde özel bir DNS sunucusu bir VPN ağ geçidi diğer ucundaki var ve DNS sunucusu erişilemez veya kullanılamaz, App Service ortamı oluşturma işlemi de başarısız olur.

## <a name="creating-a-network-security-group"></a>Bir ağ güvenlik grubu oluşturma
Nasıl iş ağ güvenlik grupları hakkında tam Ayrıntılar için aşağıdaki bkz [bilgi][NetworkSecurityGroups].  Aşağıdaki örnekte dokunduğu üzerindeki Azure Service Management'ta yapılandırma ve App Service ortamı içeren bir alt ağa bir ağ güvenlik grubu uygulayarak bir odağa sahip ağ güvenlik gruplarının vurgular.

**Not:** Ağ güvenlik grupları, grafik kullanarak yapılandırılabilir [Azure portalı](https://portal.azure.com) veya Azure PowerShell aracılığıyla.

Ağ güvenlik grupları, öncelikle bir abonelikle ilişkili bir tek başına varlık olarak oluşturulur. Ağ güvenlik grupları, bir Azure bölgesinde oluşturulduğundan, ağ güvenlik grubu App Service ortamı ile aynı bölgede oluşturduğunuzdan emin olun.

Aşağıdaki ağ güvenlik grubu oluşturmayı gösterir:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Bir ağ güvenlik grubu oluşturulduktan sonra bir veya daha fazla ağ güvenlik kuralları kendisine eklenir.  Kural kümesinin, zaman içinde değişebilir olduğundan, kural için öncelikleri zaman içinde ek kuralları eklemek kolay hale getirmek için kullanılan numaralandırma düzeni çıkış alanı için önerilir.

Aşağıdaki örnek, açıkça yönetmek ve bir App Service ortamını sürdürmek için Azure altyapısı tarafından gerekli yönetim bağlantı noktalarını erişim veren bir kural gösterir.  Tüm yönetim trafiğini SSL üzerinden akar ve bağlantı noktaları açılmış olsa da, bunlar Azure Yönetim Altyapısı dışındaki herhangi bir varlık tarafından erişilemez hale gelir için istemci sertifikaları tarafından korunmaktadır unutmayın.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP


80 numaralı ve "Yukarı Akış cihazları veya hizmetleri arkasındaki bir App Service ortamı gizlemek için" 443 bağlantı noktasına erişime kilitlemek, Yukarı Akış IP adresini bilmeniz gerekecektir.  Bir web uygulaması Güvenlik Duvarı (WAF) kullanıyorsanız, örneğin, WAF kendi IP adresini (veya adreslerini) ne zaman kullanan olur proxy trafiği bir aşağı akış App Service ortamı.  Bu IP adresini kullanmanız gerekecektir *SourceAddressPrefix* bir ağ güvenlik kuralı parametresi.

Aşağıdaki örnekte, belirli bir Yukarı Akış IP adresinden gelen gelen trafiğe açıkça izin verilir.  Adresi *1.2.3.4* bir Yukarı Akış WAF IP adresi için bir yer tutucu olarak kullanılır.  Değer, Yukarı Akış cihaz ya da hizmet tarafından kullanılan adresiyle eşleşecek şekilde değiştirin.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

FTP desteği isterseniz, aşağıdaki kurallar Kanal bağlantı noktası FTP denetim bağlantı noktası ve veri erişim vermek için bir şablon olarak kullanılabilir.  FTP durum bilgisi olan bir protokolü olduğundan, geleneksel bir HTTP/HTTPS güvenlik duvarı veya proxy aygıt FTP trafiği yönlendirmek mümkün olmayabilir.  Bu durumda ayarlama gerekecektir *SourceAddressPrefix* farklı bir değer - örneğin IP adres aralığı geliştirici veya dağıtım makine üzerinde hangi FTP istemcileri çalışıyor. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Not:** veri kanalı bağlantı noktası aralığı, Önizleme dönemi boyunca değişebilir.)

Visual Studio ile uzaktan hata ayıklama kullanılıyorsa, aşağıdaki kurallar erişim göstermektedir.  Her sürüm uzaktan hata ayıklama için farklı bir bağlantı noktasını kullandığından Visual Studio'nun desteklenen her sürümü için ayrı bir kuralı yok.  FTP erişimi gibi uzak hata ayıklama trafiğine düzgün şekilde geleneksel WAF veya proxy cihaz akışı.  *SourceAddressPrefix* bunun yerine Visual Studio çalıştıran Geliştirici makinelerine IP adres aralığı için ayarlanabilir.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-to-a-subnet"></a>Bir alt ağa bir ağ güvenlik grubu atama
Tüm dış trafiği erişimi engelleyen bir varsayılan güvenlik kuralından bir ağ güvenlik grubu vardır.  Yukarıda açıklanan ağ güvenlik kuralları ve varsayılan güvenlik kuralından gelen trafiği engelleyen birleştirme gelen sonuç ile ilişkili kaynak adres aralıklarından yalnızca bu trafiği, bir *izin* eylem gönderebilmesi için olacaktır trafiği bir App Service ortamında çalışan uygulamalar için.

Bir ağ güvenlik grubu güvenlik kuralları ile doldurulduktan sonra App Service ortamını içeren alt ağına atanması gerekir.  Atama komutu hem App Service ortamı bulunduğu sanal ağ adını hem de App Service ortamı oluşturulduğu alt ağın adı başvuruyor.  

Aşağıdaki örnek, bir alt ağ ve sanal ağ için atanmış bir ağ güvenlik grubu gösterir:

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Ağ güvenlik grubu ataması başarılı olduktan sonra (atama uzun süre çalışan işlemler olduğundan ve tamamlanması birkaç dakika sürebilir), trafikle eşleşen yalnızca gelen *izin* kuralları başarıyla App Service'te uygulamaları ulaşın Ortam.

Bütünlük açısından, aşağıdaki örnekte kaldırın ve bu nedenle alt ağdan ağ güvenlik grubu ayrılmış ilişkilendirmek nasıl gösterir:

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Açık IP SSL ile ilgili özel konular
Bir uygulama bir açık IP SSL adresi ile yapılandırılmışsa (geçerli *yalnızca* genel VIP sahip Ase'ler için), App Service ortamı varsayılan IP adresini kullanmak yerine, hem HTTP hem de HTTPS alt ağa akışlar üzerinde trafiği bir farklı bağlantı noktaları 80 ve 443 dışındaki bağlantı noktaları kümesi.

Tek tek çiftinin her IP SSL adresi tarafından kullanılan bağlantı noktaları, App Service ortamının ayrıntılarını UX dikey penceresinden portal kullanıcı arabiriminde bulunabilir.  "IP adresleri" seçin "tüm ayarlar" '-->.  "IP adresleri" dikey penceresinde bir tablosu açıkça yapılandırılmış tüm IP SSL adresleri her IP SSL adresi ile ilişkili HTTP ve HTTPS trafiği yönlendirmek için kullanılan özel bağlantı noktası çifti yanı sıra, App Service ortamı için gösterir.  Bir ağ güvenlik grubu kurallarını yapılandırırken DestinationPortRange parametreler için kullanılması gereken Bu bağlantı noktası çiftidir.

Bir ASE üzerinde uygulama IP SSL kullanacak şekilde yapılandırıldığında, dış müşteriler görmezsiniz ve özel bağlantı noktası çifti eşleme hakkında endişelenmeniz gerekmez.  Uygulamalara yönelik trafiği için yapılandırılan IP SSL adresi normalde akar.  Özel bağlantı noktası çifti çeviri otomatik olarak dahili olarak sırasında trafiği Yönlendirme ' son oluşturan bir ASE'nin bulunduğu alt ağa gerçekleşir. 

## <a name="getting-started"></a>Başlarken
App Service ortamları ile çalışmaya başlamak için bkz: [App Service Ortamı'na giriş][IntroToAppServiceEnvironment]

Uygulamaları güvenli bir App Service ortamından arka uç kaynağa bağlanma ile ilgili ayrıntılar için bkz: [bir App Service ortamından arka uç kaynaklarına güvenli bir şekilde bağlanma][SecurelyConnecttoBackend]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md
[SecurelyConnecttoBackend]:  app-service-app-service-environment-securely-connecting-to-backend-resources.md
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->

