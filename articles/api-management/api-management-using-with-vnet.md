---
title: "Sanal ağlar ile Azure API Management kullanma"
description: "Azure API Management ve erişim web Hizmetleri aracılığıyla sanal ağa bağlantısı kurma öğrenin."
services: api-management
documentationcenter: 
author: antonba
manager: erikre
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: apimpm
ms.openlocfilehash: 81634b366f5b66444d1e5474b4ab517208b50375
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2018
---
# <a name="how-to-use-azure-api-management-with-virtual-networks"></a>Sanal ağlar ile Azure API Management kullanma
Azure sanal ağlar (Vnet'ler) herhangi birini Azure kaynaklarınızı erişimi denetlemek Internet olmayan routeable ağ yerleştirin olanak sağlar. Bu ağlar sonra çeşitli VPN teknolojileri kullanarak, şirket içi ağlara bağlanabilir. Buradaki bilgiler ile başlangıç Azure sanal ağlar hakkında daha fazla bilgi edinmek için: [Azure Virtual Network'e genel bakış](../virtual-network/virtual-networks-overview.md).

Arka uç hizmetlerini ağda erişebilmesi için azure API Management (VNET) sanal ağ içinde dağıtılabilir. Internet'ten ya da yalnızca sanal ağ içinde erişilebilir olmasını Geliştirici Portalı ve API ağ geçidi, yapılandırılabilir.

> [!NOTE]
> Azure API Management hem Klasik hem de Azure Kaynak Yöneticisi sanal ağlar destekler.
>

## <a name="prerequisites"></a>Önkoşullar

Bu makalede açıklanan adımları gerçekleştirmek için şunlara sahip olmalısınız:

+ Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ APIM örneği. Daha fazla bilgi için bkz: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ VNET bağlantısı Premium ve geliştirici katmanda yalnızca kullanılabilir durumda. Geçiş bir bu katmanı'ndaki yönergeleri izleyerek [yükseltin ve ölçeklendirme](upgrade-and-scale.md#upgrade-and-scale) konu.

## <a name="enable-vpn"></a>Etkinleştir VNET bağlantısı

### <a name="enable-vnet-connectivity-using-the-azure-portal"></a>Azure portalını kullanarak VNET bağlantısını etkinleştir

1. APIM örneğinizi gidin [Azure portal](https://portal.azure.com/).
2. Seçin **sanal ağ**.
3. Bir sanal ağ içinde dağıtılacak API Management örneği yapılandırın.

    ![Sanal ağ menü API Yönetimi][api-management-using-vnet-menu]
4. İstenen erişim türünü seçin:
    
    * **Dış**: API Yönetimi ağ geçidi ve Geliştirici Portalı aracılığıyla ortak Internet üzerinden bir dış yük dengeleyici erişilebilir. Ağ geçidi sanal ağ içindeki kaynaklara erişebilir.
    
    ![Ortak eşleme][api-management-vnet-public]
    
    * **İç**: API Yönetimi ağ geçidi ve Geliştirici Portalı aracılığıyla bir iç yük dengeleyici sanal ağda yalnızca üzerinden erişilebilir. Ağ geçidi sanal ağ içindeki kaynaklara erişebilir.
    
    ![Özel eşleme][api-management-vnet-private]`

    API Management hizmetiniz burada sağlanan tüm bölgelerin bir listesi şimdi görürsünüz. VNET ve her bölge için alt ağ seçin. Liste, Klasik ve Resource Manager yapılandırmakta olduğunuz bölge kurulumunda olan Azure aboneliklerinize bulunan sanal ağlar ile doldurulur.
    
    > [!NOTE]
    > **Hizmet uç noktası** Yukarıdaki diyagramda Ağ Geçidi/Proxy, yayımcı portalında, Geliştirici Portalı, GIT ve doğrudan yönetim uç noktası içerir.
    > **Yönetim uç noktası** Yukarıdaki diyagramda olan Azure portalı ve Powershell aracılığıyla yapılandırmasını yönetmek için service üzerinde barındırılan uç noktası.
    > Ayrıca, aşağıdakilere dikkat edin, diyagram, çeşitli uç için API Management hizmeti IP adreslerini gösterir olsa bile, **yalnızca** üzerinde yapılandırılmış kendi ana bilgisayar adları yanıt verir.
    
    > [!IMPORTANT]
    > Azure API Management örneği için Resource Manager VNET'i dağıtırken hizmet Azure API Management örnekleri dışında başka kaynaklar içeren özel bir alt ağda olmalıdır. Azure API Management örneği bir Resource Manager VNET'i alt ağa dağıtmak için bir girişimde diğer kaynakları içeren, dağıtım başarısız olur.
    >

    ![VPN seçin][api-management-setup-vpn-select]

5. Tıklatın **kaydetmek** ekranın üstünde.

> [!NOTE]
> API Management örneğinin VIP adresi VNET etkin veya devre dışı her zaman değiştirir.  
> API Management gelen taşındığında VIP adresi da değiştirir **dış** için **dahili** veya tersi
>

> [!IMPORTANT]
> API Management bir sanal ağdan kaldırın veya içinde dağıtılan bir değişiklik, daha önce kullanılan VNET 4 saat için kilitli kalabilir. Bu süre zarfında VNET silin veya yeni bir kaynak dağıtma mümkün olmaz.

## <a name="enable-vnet-powershell"></a>PowerShell cmdlet'lerini kullanarak etkinleştir VNET bağlantısı
PowerShell cmdlet'lerini kullanarak VNET bağlantısı da etkinleştirebilirsiniz

* **Bir sanal ağ içinde bir API Management hizmeti oluşturma**: cmdlet'ini kullanın [yeni AzureRmApiManagement](/powershell/module/azurerm.apimanagement/new-azurermapimanagement) VNET içinde bir Azure API Management hizmet oluşturmak için.

* **VNET içinde varolan bir API Management hizmetini dağıtma**: cmdlet'ini kullanın [güncelleştirme AzureRmApiManagementDeployment](/powershell/module/azurerm.apimanagement/update-azurermapimanagementdeployment) var olan bir Azure API Management hizmetini sanal ağ içinde taşımak için.

## <a name="connect-vnet"></a>Bir sanal ağ içinde barındırılan bir web hizmetine bağlanma
API Management hizmetiniz için VNET bağlandıktan sonra içindeki arka uç hizmetlerine erişme ortak Hizmetleri erişimden çok farklı değildir. Yalnızca yerel IP adresi veya ana bilgisayar adı web hizmetinizin (bir DNS sunucusu VNET için yapılandırılmışsa) yazın **Web hizmeti URL'si** alan yeni bir API oluştururken veya var olan bir düzenleme.

![VPN bağlantısını API ekleme][api-management-setup-vpn-add-api]

## <a name="network-configuration-issues"></a>Ortak ağ yapılandırma sorunları
Sanal ağınıza API Management hizmeti dağıtırken oluşabilecek yaygın yetersizliğini sorunların bir listesi verilmiştir.

* **Özel DNS Sunucusu Kurulumu**: API Management hizmeti üzerinde çeşitli Azure hizmetlerine bağlıdır. API Management, özel bir DNS sunucusu ile bir VNET içinde barındırıldığında Azure hizmetlerin ana bilgisayar adları çözümlemek gerekir. Lütfen izleyin [bu](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) özel DNS kurulumu hakkında yönergeler. Aşağıdaki bağlantı noktaları tablo ve diğer ağ gereksinimleri başvuru için bkz.

> [!IMPORTANT]
> VNET için bir özel DNS sunucularını kullanıyorsanız, ayarladığınız, önerilen **önce** içine bir API Management hizmeti dağıtma. Aksi takdirde, DNS sunucuları (s) çalıştırarak her seferinde değiştirirseniz API Management hizmeti güncelleştirmeniz gerekir [ağ yapılandırma işlemi Uygula](https://docs.microsoft.com/rest/api/apimanagement/ApiManagementService/ApplyNetworkConfigurationUpdates)

* **API yönetimi için gereken bağlantı noktaları**: gelen ve giden trafik API Management dağıtıldığı alt içine kullanılarak denetlenebilir [ağ güvenlik grubu][Network Security Group]. Bu bağlantı noktalarının hiçbirinde yoksa, API Management düzgün çalışmayabilir ve erişilemez duruma gelebilir. Bir veya daha fazla engellenen Bu bağlantı noktalarına sahip başka bir ortak yetersizliğini API Management bir VNET ile birlikte kullanırken bir sorundur.

API Management hizmet örneği sanal ağ içinde barındırıldığında, aşağıdaki tabloda bağlantı noktaları kullanılır.

| Kaynak / hedef bağlantı noktaları | Yön | Aktarım Protokolü | Kaynak / hedef | Amaç (*) | Sanal ağ türü |
| --- | --- | --- | --- | --- | --- |
| * / 80, 443 |Gelen |TCP |INTERNET / VIRTUAL_NETWORK|API Management istemci iletişimi|Dış |
| * / 3443 |Gelen |TCP |INTERNET / VIRTUAL_NETWORK|Azure portalı ve Powershell yönetim uç noktası |İç |
| * / 80, 443 |Giden |TCP |VIRTUAL_NETWORK / INTERNET|Azure Storage, Azure Service Bus ve Azure Active Directory bağımlılığını (uygunsa).|Dış & iç | 
| * / 1433 |Giden |TCP |VIRTUAL_NETWORK / INTERNET|**Azure SQL Uç noktalara erişimi** |Dış & iç |
| * / 5671, 5672 |Giden |TCP |VIRTUAL_NETWORK / INTERNET|Olay hub'ı İlkesi ve İzleme Aracısı günlüğü bağımlılığı |Dış & iç |
| * / 445 |Giden |TCP |VIRTUAL_NETWORK / INTERNET|Azure dosya paylaşımı için GIT bağımlılığı |Dış & iç |
| * / 25028 |Giden |TCP |VIRTUAL_NETWORK / INTERNET|E-postaları göndermek için SMTP geçişi Bağlan |Dış & iç |
| * / 6381 - 6383 |Gelen ve giden |TCP |VIRTUAL_NETWORK / VIRTUAL_NETWORK|Erişim Redis önbelleği örnekleri Roleınstances arasında |Dış & iç |
| * / * | Gelen |TCP |AZURE_LOAD_BALANCER / VIRTUAL_NETWORK| Azure altyapı yük dengeleyici |Dış & iç |

>[!IMPORTANT]
> * Hangi bağlantı noktalarını *amacı* olan **kalın** API Management hizmeti başarıyla dağıtılması gereklidir. Diğer bağlantı noktalarının engellenmesi ancak düşüşü kullanın ve çalışan hizmetin izleme yeteneği neden olur.

* **SSL işlevselliği**: SSL sertifika zinciri oluşturma ve doğrulama API Management etkinleştirmek için hizmet ocsp.msocsp.com, mscrl.microsoft.com ve crl.microsoft.com giden ağ bağlantısı gerekir. API Management karşıya yüklediğiniz herhangi bir sertifikayı tam zincir CA kök içeriyorsa, bu bağımlılık gerekli değil.

* **DNS erişim**: DNS sunucuları ile iletişim için bağlantı noktası 53 giden erişim gereklidir. Özel bir DNS sunucusu bir VPN ağ geçidi diğer ucundaki varsa, DNS Sunucusu API Management barındıran alt ağdan erişilebilir olması gerekir.

* **Ölçümleri ve sistem durumu izleme**: altında şu etki alanlarına çözmek Azure Monitoring uç noktalarına giden ağ bağlantısı: global.metrics.nsatc.net, shoebox2.metrics.nsatc.net, prod3.metrics.nsatc.net.

* **Hızlı rota Kurulum**: bir ortak müşteri bunun yerine şirket içi giden Internet akışına zorlar kendi varsayılan yol (0.0.0.0/0) tanımlamak için bir yapılandırmadır. Bu trafik akışı neredeyse şaşmaz biçimde ya da engellenen şirket içi giden trafik olduğundan veya tanınmayan bir artık çeşitli Azure uç noktaları ile çalışma adresleri kümesini NAT ister Azure API Management ile bağlantısını keser. Çözümü bir (veya daha fazla) kullanıcı tanımlı yollar tanımlamaktır ([Udr'ler][UDRs]) Azure API Management içeren alt ağ üzerinde. Bir UDR yerine varsayılan yol uyulacaktır alt özel yollar tanımlar.
  Mümkünse, aşağıdaki yapılandırmayı kullanmak için önerilir:
 * ExpressRoute yapılandırma 0.0.0.0/0 tanıtır ve varsayılan olarak tüm giden trafiği şirket içi zorlamalı tünel oluşturur.
 * Azure API Management içeren alt ağa uygulanan UDR Internet sonraki atlama türü olan 0.0.0.0/0 tanımlar.
 Bu adımları etkilerini, alt düzey UDR tünel, zorunlu ExpressRoute böylece Azure API Yönetimi'nden giden Internet erişimi sağlama öncelikli olacağını ' dir.

**Ağ sanal Gereçleri aracılığıyla yönlendirme**: kullanan varsayılan yol (0.0.0.0/0) ile UDR API Management alt ağdan hedefleyen Internet trafiği yönlendirmek için Azure'da çalışan bir ağ vitrual Gereci aracılığıyla yapılandırmaları tam engeller API Management ve gerekli hizmetler arasındaki iletişim. Bu yapılandırma desteklenmiyor. 

>[!WARNING]  
>Azure API Management ile ExpressRoute yapılandırmaları desteklenmez, **yanlış arası-özel eşleme yoluna ortak eşleme yolu yolları tanıtma**. Yapılandırılmış, ortak eşleme sahip ExpressRoute yapılandırmaları alacağı Yol tanıtımlarını Microsoft'tan çok sayıda Microsoft Azure IP adres aralıkları için. Bu adres aralıklarını yanlış cross-özel eşleme yoluna üzerinde tanıtılan, sonuç tüm giden ağ paketlerinin Azure API Management örneğinin alt ağdan hatalı zorla bir müşterinin şirket içi ağ tünelli ise Altyapı. Bu ağ akışı Azure API Management keser. Bu sorun için çözüm ortak eşleme yolu arası reklam yolları özel eşleme yoluna önlemektir.


## <a name="troubleshooting"></a>Sorunlarını giderme
* **İlk kurulum**: API Management hizmeti ilk dağıtımı bir alt ağ ile başarılı olduğunda, ilk aynı alt ağ bir sanal makineyi dağıtmak için önerilir. Sanal makinede sonraki Uzak Masaüstü ve bir azure aboneliğinizde her bir kaynağın bağlantısı olduğunu doğrulayın 
    * Azure depolama blobu
    * Azure SQL Database

 > [!IMPORTANT]
 > Bağlantı doğrulandıktan sonra alt ağ API Management dağıtmadan önce alt ağda dağıtılan tüm kaynakları kaldırdığınızdan emin olun.

* **Artımlı güncelleştirmeler**: ağınıza değişiklik yaparken başvurmak [NetworkStatus API](https://docs.microsoft.com/rest/api/apimanagement/networkstatus), API Management hizmeti herhangi birine bağlı olduğu kritik kaynaklara erişim kaybetti değil olduğunu doğrulayın. Bağlantı durumunun her 15 dakikada güncelleştirilmesi gerekir.

* **Kaynak Gezinti Bağlantıları**: Resource Manager stili sanal alt dağıtırken, API Management alt kaynak Gezinti bağlantısı oluşturarak ayırır. Alt ağ zaten farklı bir sağlayıcı kaynağı içeriyorsa, dağıtım olacak **başarısız**. Benzer şekilde, bir API Management hizmeti farklı bir alt ağa taşıyın veya silin, biz bu kaynak Gezinti bağlantıyı kaldırır. 

## <a name="routing"></a> Yönlendirme
+ Bir yük dengeli ortak IP adresi (VIP), tüm hizmet uç noktalarına erişim sağlamak için ayrılacak.
+ Bir alt ağ IP aralığı (DIP) bir IP adresinden vnet içindeki kaynaklara erişmek için kullanılan ve bir genel IP adresi (VIP) sanal ağ dışında kaynaklara erişmek için kullanılır.
+ Yük dengeli genel IP adresi, Azure portalında genel bakış/Essentials dikey bulunabilir.

## <a name="limitations"></a>Sınırlamaları
* API Management örnekleri içeren bir alt ağ başka bir Azure kaynak türleri içeremez.
* Alt ağ ve API Management hizmeti aynı abonelikte olması gerekir.
* API Management örnekleri içeren bir alt ağ, abonelikler arasında taşınamaz.
* İç sanal ağ modunda yapılandırılmış bölgeli API Management dağıtımları için yönlendirme oldukları gibi birden çok bölgeler arasında dengelemesini yönetmekten sorumlu kullanıcılardır.


## <a name="related-content"></a>İlgili içerik
* [Bir sanal ağ Vpn ağ geçidi kullanarak arka ucuna bağlama](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti)
* [Farklı dağıtım modelinden bir sanal ağa bağlanma](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
* [Azure API Management'te izleme API denetleyici kullanma çağırır](api-management-howto-api-inspector.md)

[api-management-using-vnet-menu]: ./media/api-management-using-with-vnet/api-management-menu-vnet.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-type.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-using-with-vnet/api-management-using-vnet-add-api.png
[api-management-vnet-private]: ./media/api-management-using-with-vnet/api-management-vnet-private.png
[api-management-vnet-public]: ./media/api-management-using-with-vnet/api-management-vnet-public.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[UDRs]: ../virtual-network/virtual-networks-udr-overview.md
[Network Security Group]: ../virtual-network/virtual-networks-nsg.md
