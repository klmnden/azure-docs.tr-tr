---
title: "Bağlantı ve ağ iletişimi sorunları için Microsoft Azure bulut Hizmetleri ile ilgili SSS | Microsoft Docs"
description: "Bu makalede bağlantısı ve Microsoft Azure bulut Hizmetleri için ağ hakkında sık sorulan sorular listelenmektedir."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2017
ms.author: genli
ms.openlocfilehash: e89549f51abb896c1ddf48a46de78fb5e4988f22
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Azure bulut Hizmetleri için bağlantı ve ağ sorunları: sık sorulan sorular (SSS)

Bu makale için bağlantı ve ağ sorunları hakkında sık sorulan sorular içerir [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Boyutu bilgi için bkz: [bulut Hizmetleri VM boyutu sayfa](cloud-services-sizes-specs.md).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Bir çoklu VIP bulut hizmetindeki bir IP rezerve edemezsiniz.
İlk olarak, IP için ayrılacak deneyin sanal makine örneğini açık olduğundan emin olun. İkinci olarak, hazırlama ve üretim dağıtımları için ayrılmış IP kullandığınızdan emin olun. *Sağlamadığı* dağıtım yükseltme sırasında ayarlarını değiştirin.

## <a name="how-do-i-use-remote-desktop-when-i-have-an-nsg"></a>Bir NSG'yi varsa, Uzak Masaüstü nasıl kullanabilirim?
Noktalarında trafiğine izin veren NSG kuralları eklemek **3389** ve **20000**. Uzak Masaüstü bağlantı noktasını kullanır **3389**. Bulut hizmeti örnekleri yükü dengelenmiş, olduğundan, doğrudan bağlanmak için hangi örneğinin denetleyemezsiniz. *RemoteForwarder* ve *RemoteAccess* aracıları Uzak Masaüstü Protokolü (RDP) trafiği yönetmek ve istemcisinin bir RDP tanımlama bilgisi göndermek ve bağlanmak için tek bir örnek belirtin. *RemoteForwarder* ve *RemoteAccess* aracıları gerektiren bağlantı noktası **20000** açılması için hangi engellenmesi bir NSG varsa.

## <a name="can-i-ping-a-cloud-service"></a>Bir bulut hizmeti ping işlemi yapabilirsiniz?

Kullanarak "ping" normal olmayan Hayır'ı / ICMP protokolü. ICMP Protokolü Azure yük dengeleyici izin verilmez.

Bağlantıyı sınamak için bir bağlantı noktası ping yapmanızı öneririz. ICMP ping.exe kullanıyor, ancak belirli bir TCP bağlantı noktası bağlantısını test etme için PSPing, Nmap ve telnet, gibi diğer araçları kullanabilirsiniz.

Daha fazla bilgi için bkz: [bağlantı noktasına ping ICMP yerine Azure VM bağlantıyı sınamak için kullanın](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/).

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-might-indicate-a-malicious-attack-to-the-cloud-service"></a>Ne ı önlemek alma binlerce isabet kötü niyetli bir saldırının bulut hizmetine gösterebilir bilinmeyen IP adreslerinden?
Azure platform hizmetlerini dağıtılmış hizmet engelleme (DDoS) saldırılarına karşı korumak için çok katmanlı ağ güvenliği uygular. Azure DDoS savunma sistemi sızma testi aracılığıyla sürekli iyileştirilir Azure'un sürekli izleme işleminin bir parçasıdır. Bu DDoS savunma sistemi yalnızca saldırıları dışarıdan ancak diğer Azure kiracılar dayanacak şekilde tasarlanmıştır. Daha fazla bilgi için bkz: [Azure ağ güvenliği](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf).

Bazı belirli IP adreslerini seçerek engellemek için bir başlangıç görevi de oluşturabilirsiniz. Daha fazla bilgi için bkz: [belirli bir IP adres bloğu](cloud-services-startup-tasks-common.md#block-a-specific-ip-address).

## <a name="when-i-try-to-rdp-to-my-cloud-service-instance-i-get-the-message-the-user-account-has-expired"></a>My bulut hizmet örneği için RDP çalıştığınızda, "kullanıcı hesabının süresi doldu." iletisi görüntüleniyor
RDP ayarlarında sona erme tarihini atlama zaman "Bu kullanıcı hesabının süresi doldu" hata iletisini alabilirsiniz. Aşağıdaki adımları izleyerek, sona erme tarihini portalından değiştirebilirsiniz:

1. Oturum [Azure portal](https://portal.azure.com), bulut hizmetine gidin ve seçin **Uzak Masaüstü** sekmesi.

2. Seçin **üretim** veya **hazırlama** dağıtım yuvası.

3. Değişiklik **süresi** tarih ve yapılandırmayı kaydedin.

Şimdi makinenize için RDP görüntüleyebiliyor olmalısınız.

## <a name="why-is-azure-load-balancer-not-balancing-traffic-equally"></a>Neden Azure yük dengeleyici eşit Dengeleme trafiğini olduğu değil mi?
Bir iç yük dengeleyicisi nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure yük dengeleyici yeni dağıtım modu](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/).

Kullanılan dağıtım 5-tanımlama grubu (kaynak IP, kaynak bağlantı noktası, hedef IP, hedef bağlantı noktası ve protokol türü) algoritmasıdır trafiği kullanılabilir sunuculara eşlemek için karma. Bu, yalnızca bir aktarım oturumu içinde sürekliliği sağlar. Aynı TCP veya UDP oturumda paketleri aynı veri merkezinde IP'yi (DIP) örneğini yük dengeli uç nokta arkasında yönlendirilir. İstemci kapatır ve bağlantı açana veya aynı kaynak IP yeni bir oturum başlatıldığında, kaynak bağlantı noktası değiştirir ve farklı bir DIP bitiş noktasına gitmek trafiği neden olur.

## <a name="how-can-i-redirect-incoming-traffic-to-the-default-url-of-my-cloud-service-to-a-custom-url"></a>Nasıl bulut Hizmetimi özel bir URL için varsayılan URL'sini miyim gelen trafiği yönlendirebilir? 

IIS URL yeniden yazma modülü bulut hizmeti için varsayılan URL gelen trafiği yönlendirmek için kullanılabilir (örneğin, \*. cloudapp.net) bazı özel ad/URL. URL yeniden yazma modülü web rollerinde varsayılan olarak etkindir ve kurallarını uygulamanın web.config dosyasında yapılandırılmış olduğundan her zaman yeniden başlatmalar/reimages bağımsız olarak VM üzerinde kullanılabilir. Daha fazla bilgi için bkz.

- [URL yeniden yazma modülü için yeniden yazma kuralları oluşturma](https://docs.microsoft.com/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module)
- [Varsayılan bağlantıyı Kaldır](https://stackoverflow.com/questions/32286487/azure-website-how-to-remove-default-link?answertab=votes#tab-top)

## <a name="how-can-i-blockdisable-incoming-traffic-to-the-default-url-of-my-cloud-service"></a>Nasıl ı blok/bulut Hizmetim varsayılan URL'sini gelen trafiği devre dışı bırakabilir? 

URL/bulut hizmeti adını varsayılan olarak gelen trafiği engel olabilir (örneğin, \*. cloudapp.net). Özel bir DNS adı (örneğin, www.MyCloudService.com) için ana bilgisayar üstbilgisi site bağlaması yapılandırmada bulut hizmet tanımı (*.csdef) dosyasında belirtildiği şekilde ayarlayın: 
 

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
 
Bu ana bilgisayar üstbilgisi bağlama csdef dosyası aracılığıyla zorlandığından, yalnızca özel ad yoluyla "www.MyCloudService.com." erişilebilir hizmetidir Gelen tüm istekleri "*. cloudapp.net" etki alanı her zaman başarısız. Hizmete özel bir SLB araştırma veya bir iç yük dengeleyicisi kullanıyorsanız, varsayılan engelleme yoklama davranışla URL/hizmetin adını etkileyebilir. 

## <a name="how-can-i-make-sure-the-public-facing-ip-address-of-a-cloud-service-never-changes"></a>Bir bulut hizmeti genel kullanıma yönelik IP adresini hiçbir zaman değişiklikleri nasıl emin olabileceğiniz?

Geleneksel olarak Güvenilenler listesine birkaç belirli istemciler tarafından olabilir şekilde genel kullanıma yönelik IP adresi (VIP olarak da bilinir) bulut hizmeti hiçbir zaman değiştiğini emin olmak için kendisiyle ilişkili bir ayrılmış IP olması önerilir. Aksi takdirde, dağıtım silerseniz, Azure tarafından sağlanan sanal IP aboneliğinizden serbest. Başarılı VIP takas işlemi için üretim ve hazırlama yuvası için tek tek ayrılmış IP'ler gerekir. Bunları değiştirme işlemi başarısız olur. Bir IP adresi ayırın ve bulut hizmetiniz ile ilişkilendirmek için aşağıdaki makalelere bakın:
 
- [Var olan bir bulut hizmetini, IP adresi ayırın](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
- [Hizmet yapılandırma dosyası kullanarak bir bulut hizmeti için ayrılmış bir IP ilişkilendirin](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file) 

Rollerinizi için birden çok örneği varsa, RIP bulut hizmetiniz ile ilişkilendirme herhangi kesinti süresine neden döndürmemelidir. Alternatif olarak, beyaz liste IP aralığı Azure veri merkezinizin olabilir. Tüm Azure IP aralıklarını konumunda bulabilirsiniz [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=41653). 

Bu dosya, Azure veri merkezlerinde kullanılan (işlem, SQL ve depolama aralıkları dahil) IP adres aralıklarını içerir. Güncelleştirilen bir dosya şu anda dağıtılmış aralıklarını ve IP aralıklarını yaklaşan değişiklikleri yansıtan haftalık nakledilir. Dosyasında yeni aralıkları, en az bir hafta boyunca veri merkezlerinde kullanılmaz. Her hafta yeni .xml dosyasını indirin ve doğru şekilde Azure'da çalışan hizmetleri tanımlamak için sitenizde gerekli değişiklikleri yapın. Azure ExpressRoute kullanıcıların bu dosyayı Azure alanı BGP reklamı her ayın ilk haftası içinde güncelleştirmek için kullanılan fark edebilirsiniz. 

## <a name="how-can-i-use-azure-resource-manager-virtual-networks-with-cloud-services"></a>Azure Resource Manager sanal ağlar ile birlikte bulut hizmetlerini nasıl kullanabilirim? 

Bulut Hizmetleri, Azure Resource Manager sanal ağlarda yerleştirilemez. Klasik dağıtım sanal ağları ve Kaynak Yöneticisi Sanal ağları eşleme aracılığıyla bağlanabilir. Daha fazla bilgi için bkz: [sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md).
