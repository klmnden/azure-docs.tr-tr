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
ms.openlocfilehash: 7b435b6904b05228a63e3ed3a9fed78747b843c9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Azure bulut Hizmetleri için bağlantı ve ağ sorunları: sık sorulan sorular (SSS)

Bu makale için bağlantı ve ağ sorunları hakkında sık sorulan sorular içerir [Microsoft Azure bulut Hizmetleri](https://azure.microsoft.com/services/cloud-services). Ayrıca başvurabilirsiniz [bulut Hizmetleri VM boyutu sayfa](cloud-services-sizes-specs.md) boyutu bilgi.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Bir çoklu VIP bulut hizmetindeki bir IP rezerve edilemez
İlk olarak, IP için ayrılacak çalıştığınız sanal makine örneğini açık olduğundan emin olun. İkinci olarak, hazırlama ve üretim dağıtımları için ayrılmış IP kullandığınızdan emin olun. **Sağlamadığı** dağıtım yükseltme sırasında ayarlarını değiştirin.

## <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a>Nasıl yedeklerim Uzak Masaüstü bir NSG olduğunda?
Noktalarında trafiğine izin veren NSG kuralları eklemek **3389** ve **20000**.  Uzak Masaüstü bağlantı noktasını kullanır **3389**.  Bulut hizmeti örnekleri yükü dengelenmiş, olduğundan, doğrudan bağlanmak için hangi örneğinin denetleyemezsiniz.  *RemoteForwarder* ve *RemoteAccess* aracıları RDP trafiği yönetmek ve istemcisinin bir RDP tanımlama bilgisi göndermek ve bağlanmak için tek bir örnek belirtin.  *RemoteForwarder* ve *RemoteAccess* aracıları gerektiren bu bağlantı noktasını **20000** açılıp, hangi engellenmiş olabilir bir NSG varsa.

## <a name="can-i-ping-a-cloud-service"></a>Bir bulut hizmeti ping işlemi yapabilirsiniz?

Kullanarak "ping" normal olmayan Hayır'ı / ICMP protokolü. ICMP Protokolü Azure yük dengeleyici izin verilmez.

Bağlantıyı sınamak için bir bağlantı noktası ping yapmanızı öneririz. ICMP ping.exe kullanıyor, ancak PSPing, Nmap ve telnet gibi diğer araçlar, belirli bir TCP bağlantı noktası bağlantısını test etme olanak tanır.

Daha fazla bilgi için bkz: [bağlantı noktasına ping ICMP yerine Azure VM bağlantıyı sınamak için kullanın](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/).

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-indicate-some-sort-of-malicious-attack-to-the-cloud-service"></a>Ne ı önlemek alma binlerce isabet bulut hizmetine kötü amaçlı saldırı çeşit belirtmek bilinmeyen IP adreslerinden?
Azure platform hizmetlerini dağıtılmış hizmet engelleme (DDoS) saldırılarına karşı korumak için çok katmanlı ağ güvenliği uygular. Azure DDoS savunma sistemi sürekli sızma testi aracılığıyla geliştirilmiş Azure'un sürekli izleme işleminin bir parçasıdır. Bu DDoS savunma sistemi yalnızca saldırıları dışarıdan ancak diğer Azure kiracılar dayanacak şekilde tasarlanmıştır. Daha fazla ayrıntı için [Microsoft Azure ağ güvenliği](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf).

Bazı belirli IP adreslerini seçerek engellemek için bir başlangıç görevi de oluşturabilirsiniz. Daha fazla bilgi için bkz: [belirli bir IP adres bloğu](cloud-services-startup-tasks-common.md#block-a-specific-ip-address).

## <a name="when-i-try-to-rdp-to-my-cloud-service-instance-i-get-the-message-the-user-account-has-expired"></a>My bulut hizmet örneği için RDP çalıştığınızda, ileti almak için "kullanıcı hesabının süresi doldu."
RDP ayarlarında sona erme tarihini atlama, "Bu kullanıcı hesabının süresi doldu" hata iletisini alabilirsiniz. Aşağıdaki adımları izleyerek, sona erme tarihini portalından değiştirebilirsiniz:
1. Azure Yönetim Konsolu (https://manage.windowsazure.com) oturum açma, bulut hizmetine gidin ve seçin **yapılandırma** sekmesi.
2. Seçin **uzak**.
3. "Expires On" tarih değiştirin ve ardından yapılandırmayı kaydedin.

Şimdi makinenize için RDP görüntüleyebiliyor olmalısınız.

## <a name="why-is-loadbalancer-not-balancing-traffic-equally"></a>Neden LoadBalancer eşit Dengeleme trafiğini olduğu değil mi?
Nasıl iç yük dengeleyici çalıştığı hakkında daha fazla bilgi için bkz: [Azure yük dengeleyici yeni dağıtım modu](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/).

Kullanılan dağıtım 5-tanımlama grubu algoritmasıdır (kaynak IP, kaynak bağlantı noktası, hedef IP, hedef bağlantı noktası, protokol türü) trafiği kullanılabilir sunuculara eşlemek için karma. Bu, yalnızca bir aktarım oturumu içinde sürekliliği sağlar. Yük dengeli uç nokta arkasındaki aynı veri merkezinde IP'yi (DIP) örneğini paketleri aynı TCP veya UDP oturumda yönlendirilir. İstemci kapatır ve bağlantıyı yeniden açar veya aynı kaynak IP yeni bir oturum başlatıldığında, kaynak bağlantı noktası değiştirir ve farklı bir DIP bitiş noktasına gitmek trafiği neden olur.

## <a name="how-can-i-redirect-the-incoming-traffic-to-my-default-url-of-cloud-service-to-a-custom-url"></a>Nasıl my varsayılan özel bir URL için bulut hizmeti URL'si miyim gelen trafiği yönlendirebilir? 

URL yeniden yazma modülü, IIS varsayılan URL bulut hizmeti için gelen trafiği yönlendirmek için kullanılabilir (örneğin, \*. cloudapp.net) için bazı özel DNS adı/URL. URL yeniden yazma modülü Web rollerinde etkin varsayılan olarak seçilir ve kurallarını uygulamanın web.config dosyasında yapılandırılmış olduğundan, bu her zaman yeniden başlatmalar/reimages yedeklemiş VM üzerinde kullanılabilir olacaktır. Daha fazla bilgi için bkz.

- [Oluşturma yeniden yazma kuralları için URL yeniden yazma Modülü](https://docs.microsoft.com/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module)
- [Varsayılan bağlantı kaldırma](https://stackoverflow.com/questions/32286487/azure-website-how-to-remove-default-link?answertab=votes#tab-top)

## <a name="how-can-i-blockdisable-the-incoming-traffic-to-the-default-url-of-my-cloud-service"></a>Nasıl ı blok/bulut Hizmetim varsayılan URL'sini gelen trafiği devre dışı bırakabilir? 

URL/bulut hizmeti adını varsayılan olarak gelen trafiği engel olabilir (örneğin, \*. cloudapp.net) bir özel DNS adına (örneğin, www. ana bilgisayar üstbilgisi ayarlayarak MyCloudService.com) site bağlama yapılandırması aşağıda gösterildiği gibi bulut hizmeti tanım dosyasındaki (*.csdef) altında: 
 

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
 
Bu ana bilgisayar üstbilgisi bağlama csdef dosyası aracılığıyla zorlanır gibi tüm gelen istekleri için ise hizmet yalnızca özel ad 'www.MyCloudService.com' erişilebilir olan ' *. cloudapp.net' etki alanı her zaman başarısız olurdu. Ancak özel bir SLB araştırma ya da bir iç Yük Dengeleyici Hizmeti kullanıyorsanız, kabul, varsayılan URL/hizmetin adını engelleme ile yoklama davranışı etkileyebilir. 

## <a name="how-to-make-sure-the-public-facing-ip-address-of-a-cloud-service-aka-vip-never-changes-so-that-it-could-be-customarily-whitelisted-by-few-specific-clients"></a>Geleneksel olarak Güvenilenler listesine birkaç belirli istemciler tarafından olabilir şekilde genel kullanıma yönelik IP adresi, bir bulut Hizmeti'nin (diğer adıyla, VIP) hiçbir zaman değiştiğini emin olmak nasıl?

Uygulamaları güvenilir listeye almayı için bulut hizmetlerinizi IP adresini, ayrılmış bir aksi dağıtım silerseniz sanal Azure tarafından sağlanan IP aboneliğinizden bırakılmasına ilişkili IP olması önerilir. Başarılı VIP takas işlemi için tek tek ayrılmış IP'ler üretim ve hazırlama yuvaları hangi takas işlemi başarısız olur olmadığında gerekir unutmayın. Bir IP adresi ayırın ve bulut Hizmetleri ile ilişkilendirmek için bu makaleler izleyin:  
 
- [Var olan bir bulut hizmetini, IP adresi ayırın](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
- [Hizmet yapılandırma dosyası kullanarak bir bulut hizmeti için ayrılmış bir IP ilişkilendirin](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file) 

Birden fazla örnek rolleriniz için sahip olduğu sürece, RIP bulut hizmetiniz ile ilişkilendirme herhangi kesinti süresine neden döndürmemelidir. Alternatif olarak, beyaz liste IP aralığı Azure veri merkezinizin olabilir. Tüm Azure IP aralıklarını bulabilirsiniz [burada](https://www.microsoft.com/en-us/download/details.aspx?id=41653). 

Bu dosya, Microsoft Azure Veri Merkezlerinde kullanılan IP adresi aralıklarını (İşlem, SQL ve Depolama aralıkları dahil olmak üzere) içerir. O anda dağıtılmış aralıkları ve IP adreslerinde gelecekte yapılacak değişiklikleri yansıtan güncelleştirilmiş bir dosya haftalık olarak yayınlanır. Dosyada görünen yeni aralıklar en az bir hafta boyunca veri merkezlerinde kullanılmaz. Lütfen her hafta yeni xml dosyasını indirin ve Azure’da çalışan hizmetleri doğru şekilde tanımlamak üzere sitenizde gerekli değişiklikleri yapın. Express Route kullanıcıları bu dosyanın, her ayın ilk haftasında Azure alanındaki BGP tanıtımını güncelleştirmek için kullanıldığını fark edebilir. 

## <a name="how-can-i-use-azure-resource-manager-vnets-with-cloud-services"></a>Bulut Hizmetleri ile Azure Resource Manager sanal ağlara nasıl kullanabilirim? 

Bulut Hizmetleri bir Azure Kaynak Yöneticisi'ni çift yerleştirilemez, ancak bir Azure Kaynak Yöneticisi sanal ağlar ve klasik Vnet eşleme aracılığıyla bağlanabilir. Daha fazla bilgi için bkz: [sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md).