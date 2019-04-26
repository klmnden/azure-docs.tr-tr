---
title: Sanal ağlar ile Azure API Management'ı kullanma
description: Azure API yönetimi ve erişim web Hizmetleri aracılığıyla bir sanal ağa bir bağlantı ayarlamayı öğrenin.
services: api-management
documentationcenter: ''
author: vlvinogr
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2019
ms.author: apimpm
ms.openlocfilehash: 532c1051522410c496fb3809c06c7e3a74340adb
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60006055"
---
# <a name="how-to-use-azure-api-management-with-virtual-networks"></a>Sanal ağlar ile Azure API Management'ı kullanma
Azure sanal ağları (Vnet) herhangi birini kullanarak Azure kaynaklarınızı erişimini denetleyen bir ağdaki internet olmayan routeable yerleştirmenize olanak sağlar. Bu ağlar ardından teknolojiler VPN kullanarak şirket içi ağa bağlanabilir. Buradaki bilgileri ile Azure sanal ağları başlangıç hakkında daha fazla bilgi için: [Azure sanal ağına genel bakış](../virtual-network/virtual-networks-overview.md).

Azure API Management, ağından arka uç hizmetlerine erişebilmesi için bu sanal ağ (VNET) içinde dağıtılabilir. Internet'ten veya yalnızca sanal ağ içinde erişilebilir olacak şekilde Geliştirici portal ve API ağ geçidi, yapılandırılabilir.

> [!NOTE]
> Azure API Management, hem Klasik hem de Azure Resource Manager sanal ağlarına destekler.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [premium-dev.md](../../includes/api-management-availability-premium-dev.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makalede açıklanan adımları gerçekleştirmek için aşağıdakiler gerekir:

+ Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ APIM örneği. Daha fazla bilgi için [Azure API Management örneği oluşturma](get-started-create-service-instance.md).

## <a name="enable-vpn"> </a>Sanal ağ bağlantısını etkinleştirme

### <a name="enable-vnet-connectivity-using-the-azure-portal"></a>Azure portalını kullanarak sanal AĞA bağlantı etkinleştir

1. APIM Örneğinize gidin [Azure portalında](https://portal.azure.com/).
2. Seçin **sanal ağ**.
3. API Management örneği, bir sanal ağ içinde dağıtılacak şekilde yapılandırın.

    ![API Management'ın sanal ağ menüsü][api-management-using-vnet-menu]
4. İstenen erişim türünü seçin:

   * **Dış**: API Management ağ geçidi ve Geliştirici Portalı bir dış yük dengeleyici aracılığıyla genel internet'ten erişilebilir. Ağ geçidi sanal ağ içindeki kaynaklara erişebilir.

     ![Ortak eşleme][api-management-vnet-public]

   * **İç**: API Management ağ geçidi ve Geliştirici Portalı aracılığıyla bir iç yük dengeleyici sanal ağda yalnızca erişilebilir. Ağ geçidi sanal ağ içindeki kaynaklara erişebilir.

     ![Özel eşleme][api-management-vnet-private]

     Şimdi burada API Yönetimi hizmetiniz sağlandıktan tüm bölgelerin bir listesini görürsünüz. Bir sanal ağ ve her bölge için alt ağ seçin. Hem Klasik hem de Resource Manager sanal ağları bulunan Kurulum yapılandırmakta olduğunuz bölgedeki Azure aboneliklerinizin listesi doldurulur.

     > [!NOTE]
     > **Hizmet uç noktası** Yukarıdaki diyagramda Ağ Geçidi/Proxy, Azure portalı, Geliştirici Portalı, GIT ve doğrudan yönetim uç noktası içerir.
     > **Yönetim uç noktası** Yukarıdaki diyagramda olup Azure portalı ve PowerShell'de aracılığıyla yapılandırmasını yönetmek için hizmet üzerinde barındırılan uç nokta.
     > Ayrıca unutmayın, çeşitli uç için API Yönetimi hizmetinin IP adresleri diyagramda gösterilmektedir olsa da, **yalnızca** yapılandırılmış kendi ana bilgisayar adları üzerinde yanıt verir.

     > [!IMPORTANT]
     > Azure API Management örneği için Resource Manager VNET'i dağıtırken, hizmetin Azure API Management örneği dışında başka kaynaklar içeren ayrılmış alt ağında olmalıdır. Bir Resource Manager VNET'i alt ağ için Azure API Management örneği dağıtmak için bir girişimde diğer kaynakları içeren, dağıtım başarısız olur.
     >

     ![VPN seçin][api-management-setup-vpn-select]

5. Tıklayın **Kaydet** üst gezinti çubuğunda.
6. Tıklayın **Uygula ağ yapılandırması** üst gezinti çubuğunda.

> [!NOTE]
> API Management örneğinin VIP adresi VNET etkin veya devre dışı her seferinde değişir.
> API Management gelen taşındığında VIP adresi da değişecektir **dış** için **iç** veya tersi
>

> [!IMPORTANT]
> Daha önce kullanılan sanal ağ içinde dağıtılan bir sanal ağdan API Yönetimi'ni kaldırmak veya değiştirirseniz iki saate kadar kilitli kalabilir. Bu süre zarfında VNET silin veya yeni bir kaynak dağıtmak mümkün olmayacaktır.

## <a name="enable-vnet-powershell"> </a>PowerShell cmdlet'lerini kullanarak VNET bağlantısını etkinleştirme
PowerShell cmdlet'lerini kullanarak VNET bağlantısı da etkinleştirebilirsiniz

* **Bir sanal ağ içindeki bir API Management hizmeti oluşturma**: Cmdlet'i kullanmak [yeni AzApiManagement](/powershell/module/az.apimanagement/new-azapimanagement) bir sanal ağ içindeki bir Azure API Management hizmeti oluşturmak için.

* **Bir sanal ağ içinde mevcut bir API Management hizmeti dağıtma**: Cmdlet'i kullanmak [güncelleştirme AzApiManagementRegion](/powershell/module/az.apimanagement/update-azapimanagementregion) mevcut bir Azure API Management hizmeti bir sanal ağ içinde taşımak için.

## <a name="connect-vnet"> </a>Bir sanal ağ içinde barındırılan bir web hizmetine bağlanma
API Yönetimi hizmetiniz sanal ağa bağlandıktan sonra içindeki arka uç hizmetlerine erişme kamu hizmetlerine erişme değerinden farklı değildir. Yerel IP adresi veya ana bilgisayar adı web hizmetinizin (bir DNS sunucusu VNET için yapılandırılmışsa) yazmanız yeterlidir **Web hizmeti URL'si** alan yeni bir API oluştururken veya mevcut bir düzenleme.

![VPN bağlantısını API ekleme][api-management-setup-vpn-add-api]

## <a name="network-configuration-issues"> </a>Ortak ağ yapılandırma sorunları
Bir sanal ağa API Management hizmet dağıtımı sırasında oluşabilecek yaygın hatalı yapılandırma sorunları listesi aşağıda verilmiştir.

* **Özel DNS Sunucusu Kurulumu**: API Management hizmeti, çeşitli Azure hizmetlerinde bağlıdır. API Management, özel bir DNS sunucusu ile bir sanal ağ içinde barındırıldığında, bu Azure hizmetlerini ana bilgisayar adlarını çözümlemek gerekir. Lütfen izleyin [bu](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) özel DNS kurulum yönergeleri. Aşağıdaki bağlantı noktaları tabloya ve diğer ağ gereksinimleri başvurusu bakın.

> [!IMPORTANT]
> Bir özel DNS sunucuları için sanal ağ kullanmayı planlıyorsanız, kurabilecek **önce** içine bir API Management hizmeti dağıtma. Aksi takdirde, DNS sunucuları çalıştırarak her değiştirdiğinizde API Management hizmeti güncelleştirmeye gerek duyduğunuz [ağ yapılandırma işlemi uygulama](https://docs.microsoft.com/rest/api/apimanagement/ApiManagementService/ApplyNetworkConfigurationUpdates)

* **API yönetimi için gereken bağlantı noktaları**: API Management dağıtıldığı alt ağa gelen ve giden trafiği kullanılarak denetlenebilir [ağ güvenlik grubu][Network Security Group]. Bu bağlantı noktalarından birini kullanılamıyorsa, API Management düzgün çalışmayabilir ve erişilemez duruma gelebilir. Bir veya daha fazla engellenen Bu bağlantı noktalarına sahip başka bir yaygın hatalı yapılandırma sorunu API yönetimi bir VNET ile birlikte kullanıldığında.

<a name="required-ports"> </a> API Management hizmet örneği, sanal ağ içinde barındırıldığında, aşağıdaki tabloda bağlantı noktaları kullanılır.

| Kaynak / hedef bağlantı noktaları | Direction          | Aktarım Protokolü |   [Hizmet etiketleri](../virtual-network/security-overview.md#service-tags) <br> Kaynak / hedef   | Amaç (*)                                                 | Sanal ağ türü |
|------------------------------|--------------------|--------------------|---------------------------------------|-------------------------------------------------------------|----------------------|
| * / 80, 443                  | Gelen            | TCP                | INTERNET / VIRTUAL_NETWORK            | İstemci iletişimi için API Yönetimi                      | Dış             |
| * / 3443                     | Gelen            | TCP                | ApiManagement / VIRTUAL_NETWORK       | Azure portalı ve Powershell yönetim uç noktası         | Dış ve iç  |
| * / 80, 443                  | Giden           | TCP                | Vırtual_network / depolama             | **Azure depolama üzerinde bağımlılık**                             | Dış ve iç  |
| * / 80, 443                  | Giden           | TCP                | Vırtual_network / AzureActiveDirectory | Azure Active Directory (uygunsa)                   | Dış ve iç  |
| * / 1433                     | Giden           | TCP                | VIRTUAL_NETWORK / SQL                 | **Azure SQL uç noktalarına erişimi**                           | Dış ve iç  |
| * / 5672                     | Giden           | TCP                | Vırtual_network / EventHub            | Olay hub'ı İlkesi ve İzleme Aracısı için günlük bağımlılığı | Dış ve iç  |
| * / 445                      | Giden           | TCP                | Vırtual_network / depolama             | Azure dosya paylaşımı için GIT bağımlılığı                      | Dış ve iç  |
| * / 1886                     | Giden           | TCP                | VIRTUAL_NETWORK / INTERNET            | Kaynak Durumu'nda sistem durumu yayımlamak gerekli          | Dış ve iç  |
| * / 443                     | Giden           | TCP                | Vırtual_network / AzureMonitor         | Yayımlama tanılama günlükleri ve ölçümler                        | Dış ve iç  |
| * / 25                       | Giden           | TCP                | VIRTUAL_NETWORK / INTERNET            | E-postalar göndermek için SMTP geçişi bağlanma                    | Dış ve iç  |
| * / 587                      | Giden           | TCP                | VIRTUAL_NETWORK / INTERNET            | E-postalar göndermek için SMTP geçişi bağlanma                    | Dış ve iç  |
| * / 25028                    | Giden           | TCP                | VIRTUAL_NETWORK / INTERNET            | E-postalar göndermek için SMTP geçişi bağlanma                    | Dış ve iç  |
| * / 6381 - 6383              | Gelen ve giden | TCP                | VIRTUAL_NETWORK / VIRTUAL_NETWORK     | Azure önbelleği için Redis örneği arasında Roleınstances erişim          | Dış ve iç  |
| * / *                        | Gelen            | TCP                | AZURE_LOAD_BALANCER / VIRTUAL_NETWORK | Azure altyapı yük Dengeleyicisini                          | Dış ve iç  |

>[!IMPORTANT]
> İçin hangi bağlantı noktalarını *amaçlı* olduğu **kalın** sorunsuz dağıtılması API Management hizmeti için gereklidir. Diğer bağlantı noktalarının engellenmesi ancak performans düşüşü kullanma ve çalışan hizmetin izleme olanağı neden olur.

+ **SSL işlevselliği**: SSL sertifikası zincir oluşturma ve doğrulama API Management'ı etkinleştirmek için hizmet ocsp.msocsp.com ve mscrl.microsoft.com crl.microsoft.com giden ağ bağlantısı gerekir. API Management karşıya yüklediğiniz herhangi bir sertifikayı tam CA kök zinciri içeriyorsa, bu bağımlılık gerekli değildir.

+ **DNS erişim**: Giden erişim bağlantı noktası 53, DNS sunucuları ile iletişim için gereklidir. Özel bir DNS sunucusu bir VPN ağ geçidi diğer ucundaki varsa, DNS Sunucusu API Management'ı barındıran alt ağdan erişilebilir olmalıdır.

+ **Ölçümler ve sistem durumu izleme**: Giden ağ bağlantısını aşağıdaki etki alanlarının altında çözmek Azure izleme uç:

    | Azure ortamı | Uç Noktalar                                                                                                                                                                                                                                                                                                                                                              |
    |-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Azure kamu      | <ul><li>prod.warmpath.msftcloudes.com</li><li>shoebox2.Metrics.nsatc.NET</li><li>prod3.Metrics.nsatc.NET</li><li>prod3 black.prod3.metrics.nsatc.net</li><li>prod3 red.prod3.metrics.nsatc.net</li><li>prod.warm.ingestion.msftcloudes.com</li><li>`azure region`. warm.ingestion.msftcloudes.com Burada `East US 2` eastus2.warm.ingestion.msftcloudes.com olduğu</li></ul> |
    | Azure Kamu  | <ul><li>fairfax.warmpath.usgovcloudapi.net</li><li>shoebox2.Metrics.nsatc.NET</li><li>prod3.Metrics.nsatc.NET</li></ul>                                                                                                                                                                                                                                                |
    | Azure Çin       | <ul><li>mooncake.warmpath.chinacloudapi.cn</li><li>shoebox2.Metrics.nsatc.NET</li><li>prod3.Metrics.nsatc.NET</li></ul>                                                                                                                                                                                                                                                |

+ **SMTP geçiş**: Giden ağ bağlantısını altında konak çözümler SMTP geçiş `smtpi-co1.msn.com`, `smtpi-ch1.msn.com`, `smtpi-db3.msn.com`, `smtpi-sin.msn.com` ve `ies.global.microsoft.com`

+ **Geliştirici Portalı CAPTCHA**: Giden ağ bağlantısını için konak altında gideren Geliştirici portal'ın CAPTCHA `client.hip.live.com`.

+ **Azure portalı tanılama**: Giden erişim için bir sanal ağ içinde API Management uzantısını kullanarak Azure Portal'dan tanılama günlüklerinin akışını etkinleştirmek için `dc.services.visualstudio.com` bağlantı noktası 443 gereklidir. Bu, uzantısı kullanırken karşılaştığınız sorunları gidermeye yardımcı olur.

+ **Zorlamalı tünel Express Route veya ağ sanal Gereci kullanarak şirket içi güvenlik duvarının trafiği**: Yaygın müşteri tüm trafiğin API Management'tan zorlar, kendi varsayılan yolun (0.0.0.0/0) tanımlamak için alt ağ bir ağ sanal gerecine veya bir şirket içi güvenlik duvarı üzerinden flow'a temsilci bir yapılandırmadır. Bu trafik akışını neredeyse şaşmaz biçimde ya da engellenen şirket içi giden trafiği olduğundan veya tanınmayan bir artık çeşitli Azure uç noktaları ile çalışma adresleri kümesi NAT istersiniz Azure API Management ile bağlantısını keser. Çözüm birkaç şey yapmanızı gerektirir:

  * API Management hizmeti dağıtıldığı alt ağdaki hizmet uç noktalarını etkinleştirin. [Hizmet uç noktaları] [ ServiceEndpoints] Azure Sql, Azure depolama, Azure Event Hubs'a ve Azure Service Bus için etkinleştirilmesi gerekir. API Management hizmeti trafiği için en iyi yönlendirmeyi sağlayan Microsoft Azure omurga ağı kullanmak bu hizmetlere yönelik temsilci alt veren doğrudan uç noktaları etkinleştiriliyor. Hizmet uç noktaları ile zorlamalı tünel API Management kullanıyorsanız, yukarıdaki Azure hizmetlerine trafiğine zorlamalı tünel uygulanmaz. Hizmet bağımlılık trafiğine zorlamalı diğer API Management tünelli ve kaybedilemez veya API Management hizmeti düzgün çalışmaz.
    
  * Tüm denetim düzlemi trafiği Internet'ten yönetim uç noktası, API Management hizmetiniz için API Management tarafından barındırılan IP'ler gelen belirli bir dizi aracılığıyla yönlendirilir. Yanıtları simetrik trafiğe zorlamalı olduğunda bu gelen kaynağına geri IP'ler eşleştirmez. Sınırlamanın üstesinden gelmek için aşağıdaki kullanıcı tanımlı yollar eklemeniz gerekir ([Udr'ler][UDRs]) trafiği azure'a geri bu konak yolları "Internet" hedefini ayarlayarak faaliyetidir için. Gelen IP'ler kümesini denetim düzlemi trafiği için aşağıdaki gibidir:
    
    > 13.84.189.17/32, 13.85.22.63/32, 23.96.224.175/32, 23.101.166.38/32, 52.162.110.80/32, 104.214.19.224/32, 13.64.39.16/32, 40.81.47.216/32, 51.145.179.78/32, 52.142.95.35/32, 40.90.185.46/32, 20.40.125.155/32

  * Zorlamalı tünel uygulanır diğer API Management Hizmet bağımlılıkları için ana bilgisayar adını çözümleyemiyor ve uç noktaya ulaşmak için bir yol olmalıdır. Bunlar
      - Ölçümler ve sistem durumu izleme
      - Azure portalı tanılama
      - SMTP geçiş
      - Geliştirici Portalı CAPTCHA

## <a name="troubleshooting"> </a>Sorun giderme
* **İlk kurulum**: API Management hizmeti bir alt ağa ilk dağıtımı başarısız olduğunda, aynı alt ağa bir sanal makine dağıtmanız önerilir. Sanal makineye sonraki Uzak Masaüstü ve azure aboneliğinizde her bir kaynaktan bir bağlantı olduğunu doğrulayın
    * Azure depolama blobu
    * Azure SQL Veritabanı
    * Azure depolama tablosu

  > [!IMPORTANT]
  > Bağlantı doğrulandıktan sonra API Management alt ağa dağıtmadan önce alt ağda dağıtılan tüm kaynakları kaldırmak emin olun.

* **Artımlı güncelleştirmeler**: Ağınızda değişiklikler yaparken, başvurmak [NetworkStatus API](https://docs.microsoft.com/rest/api/apimanagement/networkstatus), API Management hizmeti bağlıdır, kritik kaynaklara erişimi kesildi değil doğrulayın. Bağlantı durumu, her 15 dakikada güncelleştirilmelidir.

* **Kaynak Gezinti Bağlantıları**: API Management Resource Manager stili vnet alt ağa dağıtırken, kaynak Gezinti bağlantısı oluşturarak alt ayırır. Alt ağ zaten farklı bir sağlayıcı kaynaktan içeriyorsa, dağıtım olacak **başarısız**. Benzer şekilde, bir API Management hizmeti farklı bir alt ağa taşıyın veya silin, o kaynak Gezinti bağlantısı kaldıracağız.

## <a name="subnet-size"> </a> Alt ağ boyutu gereksinimi
Azure her alt ağ içinde bazı IP adreslerini ayırır ve bu adresi kullanılamaz. Alt ağların ilk ve son IP adresleri, Azure Hizmetleri için kullanılan üç daha fazla adres birlikte protokol uyumluluğu için ayrılmıştır. Daha fazla bilgi için [bu alt ağları içindeki IP adresleri kullanarak herhangi bir kısıtlama var mıdır?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Azure sanal ağ altyapısı tarafından kullanılan IP adresleri ek olarak, alt ağdaki her bir API Management örneği için geliştirici SKU Premium SKU'ların birim başına iki IP adresi veya bir IP adresi kullanır. Her örnek, dış yük dengeleyici için ek bir IP adresi ayırır. İç sanal ağ ile dağıtırken iç yük dengeleyici için ek bir IP adresi gerektirir.

API Management dağıtılabilir alt ağın en küçük boyutu yukarıdaki hesaplama verilen üç IP adresini sağlayan /29 olur.

## <a name="routing"> </a> Yönlendirme
+ Tüm hizmet uç noktalarına erişimi sağlamak için yük dengeli genel IP adresi (VIP) ayrılacaktır.
+ Bir alt ağ IP aralığı (DIP) ağdan bir IP adresi, sanal ağ içindeki kaynaklara erişmek için kullanılacak ve sanal ağ dışındaki kaynaklara erişmek için genel bir IP adresi (VIP) kullanılır.
+ Yük dengeli genel IP adresi, Azure portalında genel bakış/Essentials dikey penceresinde bulunabilir.

## <a name="limitations"> </a>Sınırlamaları
* API Management örneği içeren bir alt ağ başka bir Azure kaynak türlerini içeremez.
* Alt ağ ile API Management hizmeti aynı abonelikte olmalıdır.
* API Management örneği içeren bir alt ağ, abonelikler arasında taşınamaz.
* İç sanal ağ modunda yapılandırılmış çok bölgeli API Management dağıtımları için kullanıcıların yönlendirme oldukları gibi Yük Dengeleme, birden çok bölgede yönetmekten sorumludur.
* Genel olarak eşlenmiş vnet'teki başka bir bölgede bir kaynak bağlantısı iç modunda API Management hizmeti için platform sınırlaması nedeniyle çalışmaz. Daha fazla bilgi için [bir sanal ağ içindeki kaynaklarla Azure iç yük dengeleyici eşlenen sanal ağ ile iletişim kurmak olamaz](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints)


## <a name="related-content"> </a>İlgili içerik
* [Bir sanal ağ Vpn ağ geçidi kullanarak arka ucuna bağlama](../vpn-gateway/vpn-gateway-about-vpngateways.md#s2smulti)
* [Farklı dağıtım modellerindeki sanal ağa bağlanma](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
* [Azure API Management'ta API denetçisi izleme için kullanmayı çağırır](api-management-howto-api-inspector.md)
* [Sanal ağ hakkında SSS](../virtual-network/virtual-networks-faq.md)
* [Hizmet etiketleri](../virtual-network/security-overview.md#service-tags)

[api-management-using-vnet-menu]: ./media/api-management-using-with-vnet/api-management-menu-vnet.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-using-with-vnet/api-management-using-vnet-add-api.png
[api-management-vnet-private]: ./media/api-management-using-with-vnet/api-management-vnet-internal.png
[api-management-vnet-public]: ./media/api-management-using-with-vnet/api-management-vnet-external.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[UDRs]: ../virtual-network/virtual-networks-udr-overview.md
[Network Security Group]: ../virtual-network/security-overview.md
[ServiceEndpoints]: ../virtual-network/virtual-network-service-endpoints-overview.md
[ServiceTags]: ../virtual-network/security-overview.md#service-tags
