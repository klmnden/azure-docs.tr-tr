---
title: Bağlantı ve ağ sorunları için Microsoft Azure bulut Hizmetleri ile ilgili SSS | Microsoft Docs
description: Bu makalede, bağlantı ve Microsoft Azure bulut Hizmetleri için ağ hakkında sık sorulan sorular listelenmektedir.
services: cloud-services
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2018
ms.author: genli
ms.openlocfilehash: 2a46879a6882e6d45e4a7ccce59e4a02feea9005
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61432968"
---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Bağlantı ve sorunları, Azure bulut Hizmetleri için ağ: Sık sorulan sorular (SSS)

Bu makale için bağlantı ve ağ sorunları hakkında sık sorulan sorular içerir [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Boyut için bilgi [Cloud Services sanal makine boyutu sayfa](cloud-services-sizes-specs.md).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Ben bir çoklu VIP bulut hizmetinde bir IP ayıramazsınız.
İlk olarak, için IP'si deneyin. sanal makine örneği açık olduğundan emin olun. İkinci olarak, hem hazırlık hem de üretim dağıtımları için ayrılmış IP'ler kullandığınızdan emin olun. *Sağlamadığı* dağıtım yükseltme sırasında ayarlarını değiştirin.

## <a name="how-do-i-use-remote-desktop-when-i-have-an-nsg"></a>Bir NSG olduğunda Uzak Masaüstü nasıl kullanabilirim?
Bağlantı noktası üzerinde trafiğe izin veren NSG kuralları eklemek **3389** ve **20000**. Uzak Masaüstü bağlantı noktasını kullanır **3389**. Bulut hizmeti örneklerinin, yük dengeli olduğundan doğrudan bağlanmak için hangi örneğinin denetleyemezsiniz. *RemoteForwarder* ve *RemoteAccess* aracılar Uzak Masaüstü Protokolü (RDP) trafiği yönetmek ve bir RDP tanımlama bilgisi göndermek ve bağlanmak için tek bir örneğini belirtmek istemcinin sağlamak. *RemoteForwarder* ve *RemoteAccess* aracıların bağlantı noktası gerektirir **20000** açık olması için hangi engelleniyor olabilir bir NSG varsa.

## <a name="can-i-ping-a-cloud-service"></a>Bir bulut hizmeti ping komutu gönderebilir?

Hayır, normal "ping" kullanarak değil / ICMP protokolü. ICMP protokolü üzerinden Azure yük dengeleyicisine izin verilmez.

Bağlantıyı test etmek için bir bağlantı noktasına ping yapmanız önerilir. Ping.exe ICMP kullanırken, belirli bir TCP bağlantı noktasını bağlantısını test etmek için telnet, PSPing ve nmap komutunu çalıştırdığınız gibi diğer araçları kullanabilirsiniz.

Daha fazla bilgi için bkz: [bağlantı noktasına ping ICMP yerine Azure VM bağlantıyı sınamak için kullanın](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/).

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-might-indicate-a-malicious-attack-to-the-cloud-service"></a>Nasıl alma engelleyebilirim binlerce isabet bir kötü amaçlı saldırı bulut hizmetine işaret eden bilinmeyen IP adreslerinden mi?
Azure platformu hizmetlerinin dağıtılmış hizmet engelleme (DDoS) saldırılarına karşı korumak için çok katmanlı bir ağ güvenlik uygular. Azure DDoS koruma işleminin bir parçası olan sızma testi aracılığıyla sürekli olarak geliştirilir Azure'nın sürekli izleme sistemidir. Bu DDoS koruma sistemi yalnızca saldırıları dışarıdan aynı zamanda Azure diğer kiracılardan dayanacak şekilde tasarlanmıştır. Daha fazla bilgi için [Azure ağ güvenliği](https://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf).

Belirli bazı IP adreslerini seçerek engellemek için bir başlangıç görevi de oluşturabilirsiniz. Daha fazla bilgi için [belirli bir IP adres bloğu](cloud-services-startup-tasks-common.md#block-a-specific-ip-address).

## <a name="when-i-try-to-rdp-to-my-cloud-service-instance-i-get-the-message-the-user-account-has-expired"></a>RDP için bulut hizmeti Örneğim denediğinizde "kullanıcı hesabının süresi doldu." iletisi görüntüleniyor
RDP ayarlarınızda yapılandırılmış sona erme tarihi atlama, "Bu kullanıcı hesabının süresi doldu" hata iletisi alabilirsiniz. Aşağıdaki adımları izleyerek, portaldan sona erme tarihini değiştirebilirsiniz:

1. Oturum [Azure portalında](https://portal.azure.com)bulut hizmetinize gidin ve seçin **Uzak Masaüstü** sekmesi.

2. Seçin **üretim** veya **hazırlama** dağıtım yuvası.

3. Değişiklik **sonu** tarih ve yapılandırmayı kaydedin.

Sizin makinenize yönelik RDP çağırabilmesi gerekir.

## <a name="why-is-azure-load-balancer-not-balancing-traffic-equally"></a>Neden Azure Load Balancer trafiği dengelemiyor?
İç yük dengeleyici nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure Load Balancer yeni dağıtım modunu](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/).

Kullanılan dağıtım algoritmasını 5-tanımlama grubu (kaynak IP, kaynak bağlantı noktası, hedef IP, hedef bağlantı noktası ve protokol türü) olan kullanılabilir olan sunucular için trafiği eşlemek için karma. Bu, yalnızca bir aktarım oturumu içinde süreklilik sağlar. Paketler aynı TCP veya UDP oturumunda yük dengeli uç nokta arkasındaki aynı veri merkezi IP (DIP) örneğine yönlendirilir. İstemci kapatır ve bağlantıyı yeniden açana veya yeni bir oturum başlatır aynı kaynak IP, kaynak bağlantı noktası değiştirir ve farklı bir DIP uç noktasına gidin, trafik neden olur.

## <a name="how-can-i-redirect-incoming-traffic-to-the-default-url-of-my-cloud-service-to-a-custom-url"></a>Nasıl bir özel URL'ye bulut hizmetimde varsayılan URL'sine miyim gelen trafiği yönlendirebilirsiniz?

IIS URL yeniden yazma modülü, bulut hizmeti için varsayılan URL için gelen trafiği yönlendirmek için kullanılabilir (örneğin, \*. cloudapp.net) bazı özel adı/URL. URL yeniden yazma modülü web rollerinde, varsayılan olarak etkindir ve uygulamanın web.config dosyasında yapılandırılmış, bir kural olduğundan, her zaman sanal makine yeniden başlatmaları/görüntüsünü yeniden oluşturur bağımsız olarak kullanılabilir. Daha fazla bilgi için bkz:

- [URL yeniden yazma modülü için yeniden yazma kuralları oluşturma](https://docs.microsoft.com/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module)
- [Varsayılan bağlantısını Kaldır](https://stackoverflow.com/questions/32286487/azure-website-how-to-remove-default-link?answertab=votes#tab-top)

## <a name="how-can-i-blockdisable-incoming-traffic-to-the-default-url-of-my-cloud-service"></a>Nasıl miyim blok/bulut hizmetimde varsayılan URL'sine gelen trafiği devre dışı bırakabilir?

Varsayılan URL/bulut hizmetinizin adını gelen trafiği engelleyebilir (örneğin, \*. cloudapp.net). Özel bir DNS adı (örneğin, www.MyCloudService.com) ana bilgisayar üst bilgisinde site bağlama Yapılandırması altında bulut hizmet tanımı (*.csdef) dosyasında belirtildiği gibi ayarlayın:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicesDemo" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
    <WebRole name="MyWebRole" vmsize="Small">
        <Sites>
            <Site name="Web">
            <Bindings>
                <Binding name="Endpoint1" endpointName="Endpoint1" hostHeader="www.MyCloudService.com" />
            </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
</ServiceDefinition>
```

Bu ana bilgisayar üst bilgisi bağlama csdef dosyasıyla zorunlu kılındığından yalnızca özel adı "www.MyCloudService.com." hizmet erişilebilir Gelen tüm istekleri "*. cloudapp.net" etki alanı her zaman başarısız. Özel bir SLB araştırma ya da iç load balancer hizmeti kullanıyorsanız, varsayılan engelleme URL/hizmet adı algılama davranışı etkileyebilir.

## <a name="how-can-i-make-sure-the-public-facing-ip-address-of-a-cloud-service-never-changes"></a>Hiçbir zaman bir bulut hizmetinin genel kullanıma yönelik IP adresi değişiklikleri nasıl emin olabilirim?

Beyaz listeye birkaç belirli istemciler tarafından özel olarak olabilir (VIP olarak da bilinir), bulut hizmeti genel kullanıma yönelik IP adresini hiçbir zaman değiştirir, böylece emin olmak için kendisiyle ilişkili bir ayrılmış IP'ye sahip olmanızı öneririz. Aksi takdirde, Azure tarafından sağlanan sanal IP dağıtımı silerseniz aboneliğinizden serbest bırakıldı. Başarılı VIP takas işlemi için ayrı ayrı ayrılmış IP'ler için üretim ve hazırlama yuvası gerekir. Bunlar olmadan değiştirme işlemi başarısız olur. Bir IP adresini ayırabilir ve bulut hizmetiniz ile ilişkilendirmek için şu makalelere bakın:

- [Mevcut bir bulut hizmetinin IP adresini ayırma](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
- [Hizmet yapılandırma dosyasını kullanarak bir bulut hizmeti için ayrılmış bir IP'yi ilişkilendirme](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file)

Rollerinizi için birden fazla örnek varsa, RIP, bulut hizmetiyle ilişkilendirme herhangi kapalı kalma durumlarına neden olmaması gerekir. Alternatif olarak, beyaz liste, Azure veri merkezi IP aralığını kullanabilirsiniz. Tüm Azure IP aralıklarına bulabilirsiniz [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=41653).

Bu dosya, Azure veri merkezlerinde kullanılan (işlem, SQL ve depolama aralıkları dahil) IP adresi aralıklarını içerir. Güncelleştirilen bir dosya şu anda dağıtılmış aralıkları ve IP adreslerinde gelecekte yapılacak değişiklikleri yansıtan haftalık gönderilir. Dosyada görünen yeni aralıklar en az bir hafta boyunca veri merkezlerinde kullanılmaz. Her hafta yeni bir .xml dosyasını indirin ve Azure'da çalışan hizmetleri doğru şekilde tanımlamak üzere sitenizde gerekli değişiklikleri gerçekleştirin. Bu dosya her ayın ilk haftasında Azure alanındaki BGP tanıtımını güncelleştirmek için kullanılan Azure ExpressRoute kullanıcıları fark edebilirsiniz.

## <a name="how-can-i-use-azure-resource-manager-virtual-networks-with-cloud-services"></a>Azure Resource Manager sanal ağları ile bulut hizmetleri nasıl kullanabilirim?

Bulut Hizmetleri, Azure Resource Manager sanal ağları yerleştirilemez. Resource Manager sanal ağları ve klasik dağıtım sanal ağ eşlemesi üzerinden bağlanabilir. Daha fazla bilgi için [sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md).


## <a name="how-can-i-get-the-list-of-public-ips-used-by-my-cloud-services"></a>My bulut Hizmetleri tarafından kullanılan genel IP'lerin nasıl alabilirim?

PS komut dosyasını Genel IP'lerin Cloud Services için aboneliğiniz kapsamındaki almak için kullanabilirsiniz

```powershell
$services = Get-AzureService  | Group-Object -Property ServiceName

foreach ($service in $services)
{
    "Cloud Service '$($service.Name)'"

    $deployment = Get-AzureDeployment -ServiceName $service.Name
    "VIP - " +  $deployment.VirtualIPs[0].Address
    "================================="
}
```
