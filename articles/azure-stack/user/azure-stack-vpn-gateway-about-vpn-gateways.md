---
title: Azure Stack için VPN gateway hakkında | Microsoft Docs
description: Azure Stack ile kullandığınız VPN ağ geçitlerini yapılandırmak ve bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 0e30522f-20d6-4da7-87d3-28ca3567a890
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/15/2019
ms.author: sethm
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: 7bf7034d30a8aac187fb2eeae6569f2f495e4439
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56327264"
---
# <a name="about-vpn-gateway-for-azure-stack"></a>Azure Stack için VPN gateway hakkında

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure sanal ağınız ile şirket içi siteniz arasında ağ trafiği göndermek için önce sanal ağınız için sanal ağ geçidi oluşturmanız gerekir.

VPN ağ geçidi bir ortak bağlantı üzerinde şifrelenmiş trafik gönderen sanal ağ geçidi türüdür. Azure'da güvenli bir şekilde Azure Stack'te bir sanal ağ ve sanal ağ arasında trafik göndermek için VPN ağ geçidi'ni kullanabilirsiniz. Ayrıca, trafiği bir sanal ağ ve VPN cihazına bağlı başka bir ağ arasında güvenli bir şekilde gönderebilirsiniz.

Bir sanal ağ geçidi oluştururken, oluşturmak istediğiniz ağ geçidi türünü belirtirsiniz. Azure Stack, bir sanal ağ geçidi türünü destekler: **Vpn** türü.

Bir sanal ağın her ağ geçidi türünden yalnızca bir sanal ağ geçidi olabilir. Seçtiğiniz ayarlara bağlı olarak, tek bir VPN ağ geçidi ile birden fazla bağlantı oluşturabilirsiniz. Çok siteli bağlantı yapılandırması buna bir örnektir.

Oluşturma ve Azure Stack için VPN ağ geçitleri'ı yapılandırmadan önce gözden [Azure Stack ağ iletişimi için Değerlendirmeler](azure-stack-network-differences.md) yapılandırmaları Azure Stack için Azure'dan farkı öğrenin.

>[!NOTE]
>Azure'da ağ geçidine bağlı olan tüm bağlantılar üzerinden bant genişliği performansını seçtiğiniz SKU VPN ağ geçidi ayrılmalıdır. Azure Stack'te ancak VPN ağ geçidi SKU'su bant genişliği değeri gateway'e bağlı her bir bağlantı kaynak uygulanır.
>
> Örneğin:
> * Azure'da temel VPN ağ geçidi SKU'sunu toplam üretilen iş yaklaşık 100 MB/sn barındırabilir. Bu VPN ağ geçidi iki bağlantı oluşturun ve bir bağlantı 50 MB/sn bant genişliği kullanıyorsa, ardından 50 MB/sn bir bağlantı için kullanılabilen olur.
> * Azure stack'teki *her* temel VPN ağ geçidi SKU'sunu bağlantı 100 MB/sn aktarım hızının ayrılan.

## <a name="configuring-a-vpn-gateway"></a>VPN gateway yapılandırma

Bir VPN ağ geçidi bağlantısı belirli ayarlarla yapılandırılmış birden çok kaynak kullanır. Bu kaynakların birçoğu ayrı ayrı yapılandırılabilir, ancak bazı durumlarda, belirli bir sırayla yapılandırılması gerekir.

### <a name="settings"></a>Ayarlar

Her kaynak için seçtiğiniz ayarlar başarılı bir bağlantı oluşturmak için önemlidir.

Tek tek kaynakları ve VPN gateway ayarları hakkında daha fazla bilgi için bkz. [hakkında VPN gateway ayarları Azure Stack için](azure-stack-vpn-gateway-settings.md). Bu makalede anlamanıza yardımcı olur:

* Ağ geçidi türleri, VPN türleri ve bağlantı türleri.
* Ağ geçidi alt ağları, yerel ağ geçitleri ve kullanmayı isteyebileceğiniz diğer çeşitli ayarlar.

### <a name="deployment-tools"></a>Dağıtım araçları

Oluşturun ve Azure portalı gibi bir yapılandırma aracını kullanarak kaynakları yapılandırın. Daha sonra ek kaynaklar yapılandırmak ya da uygun olduğunda var olan kaynakları değiştirmek için PowerShell gibi başka bir araca geçiş. Şu anda Azure portalında her kaynağı ve kaynak ayarını yapılandıramazsınız. Her bağlantı topolojisine ilişkin makalelerdeki yönergelerde, belirli bir aracının ne zaman gerekli olduğu belirtilmektedir.

## <a name="connection-topology-diagrams"></a>Bağlantı topolojisi diyagramları

VPN gateway bağlantıları için kullanılabilecek farklı yapılandırmalar vardır. Gereksinimlerinize en uygun yapılandırmayı en iyi belirleyin. Aşağıdaki bölümlerde, aşağıdaki VPN ağ geçidi bağlantıları hakkında bilgi ve topoloji diyagramlarını görüntüleyebilirsiniz:

* Kullanılabilir dağıtım modeli
* Kullanılabilir yapılandırma araçları
* Varsa sizi doğrudan bir makaleye yönlendiren bağlantılar

Diyagramları ve açıklamaları aşağıdaki bölümlerde, gereksinimlerinize uygun bağlantı topolojisini seçmenize yardımcı olabilir. Başlıca temel topolojileri diyagramları gösterir, ancak diyagramları bir kılavuz olarak kullanıp daha karmaşık yapılandırmalar oluşturmak mümkündür.

## <a name="site-to-site-and-multi-site-ipsecike-vpn-tunnel"></a>Siteden siteye ve çok siteli (IPSec/IKE VPN tüneli)

### <a name="site-to-site"></a>Siteden siteye

A *siteden siteye* (S2S) VPN ağ geçidi bağlantısı, IPSec/IKE (Ikev2) VPN tüneli üzerinden kurulan bağlantı olduğu. Bu tür bir bağlantı, şirket içinde ve genel bir IP adresi atanmış bir VPN cihazı gerektirir. Bu cihazı bir NAT'nin arkasında olamaz S2S bağlantıları, şirket içi ve dışı yapılandırmalar ile birlikte karma yapılandırmalar için kullanılabilir.

![Siteden siteye VPN bağlantısı yapılandırma örneği](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="multi-site"></a>Çoklu site

A *çok siteli* siteden siteye bağlantı çeşitlemesi bağlantısıdır. Sanal ağ geçidinizden genellikle birden fazla şirket içi siteye bağlanan birden fazla VPN bağlantısı oluşturursunuz. Birden fazla bağlantıyla çalışırken (Klasik sanal ağlar ile çalışırken dinamik ağ geçidi bilinir) bir rota tabanlı VPN türü kullanmalısınız. Her sanal ağın yalnızca bir VPN ağ geçidi olabileceğinden, ağ geçidi boyunca tüm bağlantılar mevcut bant genişliğini paylaşır.

![Azure VPN Gateway Çok Siteli bağlantı örneği](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-multisite-connection-diagram.png)

## <a name="gateway-skus"></a>Ağ geçidi SKU'ları

Azure Stack için sanal ağ geçidi oluşturduğunuzda, kullanmak istediğiniz SKU ağ geçidi belirtin. Aşağıdaki VPN ağ geçidi SKU'ları desteklenir:

* Temel
* Standart
* Yüksek Performans

Standart veya temel daha yüksek bir ağ geçidi gibi temel, standart veya yüksek performans SKU'su seçtiğinizde ağ geçidine daha fazla CPU ve ağ bant genişliği ayrılır. Sonuç olarak, ağ geçidi, sanal ağı daha yüksek ağ verimliliğini destekleyebilir.

Azure Stack Ultra performans ağ geçidi yalnızca Express Route ile kullanılan SKU desteklemez.

SKU seçtiğinizde aşağıdakileri göz önünde bulundurun:

* Azure Stack, ilke tabanlı ağ geçitleri desteklemez.
* Sınır Ağ Geçidi Protokolü (BGP) temel SKU'da desteklenmiyor.
* Azure Stack'te ExpressRoute-VPN ağ geçidi arada var olabilen yapılandırmaları desteklenmez.
* Etkin-etkin S2S VPN gateway bağlantıları, yüksek performanslı SKU üzerinde yalnızca yapılandırılabilir.

## <a name="estimated-aggregate-throughput-by-sku"></a>SKU'ya göre tahmini toplam verimlilik

Aşağıdaki tabloda, ağ geçidi SKU'suna göre ağ geçidi türleri ve tahmini toplam verimlilik gösterilmektedir:

|   | VPN Gateway işlemesi *(1)* | VPN Gateway maks. IPSec tünelleri *(2)* |
|-------|-------|-------|
|**Temel SKU** ***(3)***    | 100 Mbps  | 20    |
|**Standart SKU**       | 100 Mbps  | 20    |
|**Yüksek performanslı SKU** | 200 Mbps    | 10    |

**Tablo Notlar:**

*Not (1)* -VPN aktarım hızını değil şirketler arası bağlantılar için garantili bir verimlilik Internet üzerinden. Mümkün olan en yüksek verimlilik ölçümüdür.  
*Not (2)* -en fazla tünel olan tüm abonelikler için Azure Stack dağıtım başına toplam.  
*Not (3)* -BGP yönlendirme temel SKU için desteklenmiyor.

>[!NOTE]
>Yalnızca bir siteden siteye VPN bağlantısı arasında iki Azure Stack dağıtımı oluşturulabilir. Yalnızca tek bir VPN bağlantısı aynı IP adresine izin veren bir platformda bir sınırlama nedeniyle budur. Azure Stack, Azure Stack sistemdeki tüm VPN ağ geçitleri için tek bir genel IP kullanan çok kiracılı ağ geçidi yararlanır çünkü iki Azure Stack sistemleri arasında yalnızca bir VPN bağlantısı olabilir. Bu sınırlama, birden fazla siteden siteye VPN bağlantısı tek bir IP adresi kullanan bir VPN gateway'e bağlanırken için de geçerlidir. Azure Stack birden fazla yerel ağ geçidi kaynağı, aynı IP adresini kullanarak oluşturulmasına izin vermiyor.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack için VPN gateway yapılandırma ayarları](azure-stack-vpn-gateway-settings.md)
