---
title: Azure yığınının VPN gateway hakkında | Microsoft Docs
description: Hakkında bilgi edinin ve Azure yığın ile kullandığınız VPN ağ geçitleri yapılandırın.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 0e30522f-20d6-4da7-87d3-28ca3567a890
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/01/2017
ms.author: brenduns
ms.openlocfilehash: 10b2bf863540330a57b5aecac438f2b9e4bc8a74
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="about-vpn-gateway-for-azure-stack"></a>Azure yığınının VPN gateway hakkında
*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*


Azure sanal ağınıza ve şirket içi siteniz arasında ağ trafiğini göndermeden önce sanal ağınız için bir sanal ağ geçidi oluşturmanız gerekir.

VPN ağ geçidi bir ortak bağlantı üzerinde şifrelenmiş trafik gönderen sanal ağ geçidi türüdür. VPN ağ geçitleri, Azure'da Azure yığınında sanal ağ ve sanal ağ arasında güvenli bir şekilde trafiği göndermek için kullanabilirsiniz. Bir sanal ağ ve bir VPN cihazına bağlı olan başka bir ağ arasında güvenli bir şekilde trafiği de gönderebilirsiniz.

Bir sanal ağ geçidi oluştururken, oluşturmak istediğiniz ağ geçidi türünü belirtirsiniz. Azure yığını bir sanal ağ geçidi türünü destekler: 'Vpn' tür.

Bir sanal ağın her ağ geçidi türünden yalnızca bir sanal ağ geçidi olabilir. Seçtiğiniz ayarlara bağlı olarak, tek bir VPN ağ geçidi ile birden fazla bağlantı oluşturabilirsiniz. Çok siteli bağlantı yapılandırması örneğidir.

> [!NOTE]
> Azure'da, VPN ağ geçidi SKU'su seçtiğiniz için bant genişliği verimliliği ona bağlı tüm bağlantıları üzerinden ayrılmalıdır.  Azure yığınında VPN ağ geçidi SKU'su bant genişliği değeri kendisine bağlı her bağlantı kaynağı uygulanır.     

> Örneğin, içinde Azure temel VPN ağ geçidi SKU'su yaklaşık 100 Mbps toplam verimlilik uyum sağlayabilir.  Bu VPN ağ geçidi iki bağlantıları oluşturma ve bir bağlantı 50 MB/sn bant genişliği kullanıyorsa, ardından 50 MB/sn diğer bağlantısı için kullanılabilir olur.   

> Azure yığınında *her* temel VPN ağ geçidi SKU'su bağlantı 100 MB / sn'ye ayrılan.

## <a name="configuring-a-vpn-gateway"></a>VPN Gateway yapılandırma
VPN ağ geçidi bağlantısı belirli ayarlarla yapılandırılmış birden fazla kaynağı kullanır. Kaynakların birçoğu ayrı ayrı yapılandırılabilir, ancak bazı durumlarda belirli bir sırayla yapılandırılması gerekir.

### <a name="settings"></a>Ayarlar
Her kaynak için seçtiğiniz ayarlar başarılı bir bağlantı oluşturmak için önemlidir. Tek tek kaynakları ve VPN ağ geçidi için ayarları hakkında daha fazla bilgi için bkz: [hakkında VPN ağ geçidi ayarlarını Azure yığınının](azure-stack-vpn-gateway-settings.md). Ağ geçidi türleri, VPN türleri, bağlantı türleri, ağ geçidi alt ağları, yerel ağ geçitleri ve düşünmek isteyebilirsiniz diğer çeşitli kaynak ayarlarını anlamanıza yardımcı olacak bilgiler bulabilirsiniz.

### <a name="deployment-tools"></a>Dağıtım araçları
Oluşturun ve Azure portalı gibi bir yapılandırma aracını kullanarak kaynaklarını yapılandırın. Daha sonra ek kaynaklar yapılandırmak veya var olan kaynakların uygulanabilir olduğunda değiştirmek için PowerShell gibi başka bir araç geçmeniz. Şu anda Azure portalında her kaynağı ve kaynak ayarını yapılandıramazsınız. Her bağlantı topolojisine ilişkin makalelerdeki yönergelerde, belirli bir aracının ne zaman gerekli olduğu belirtilmektedir.

## <a name="connection-topology-diagrams"></a>Bağlantı topoloji diyagramları
VPN ağ geçidi bağlantıları için kullanılabilecek farklı yapılandırmalar vardır. Gereksinimlerinize en iyi yapılandırma belirler. Aşağıdaki bölümlerde, aşağıdaki VPN ağ geçidi bağlantıları hakkında bilgi ve topoloji diyagramlarını görüntüleyebilirsiniz: Aşağıdaki bölümlerde şu listeleri içeren tablolar bulunur:

- Kullanılabilir dağıtım modeli
- Kullanılabilir yapılandırma araçları
- Varsa sizi doğrudan bir makaleye yönlendiren bağlantılar

Aşağıdaki bölümlerde açıklamaları ve diyagramları gereksinimlerinize uygun bir bağlantı topolojisini seçmenize yardımcı olabilir. Diyagramlarda temel topolojilerin başlıca olanları gösterilmektedir ancak diyagramları bir kılavuz olarak kullanıp daha karmaşık yapılandırmalar da oluşturabilirsiniz.

## <a name="site-to-site-and-multi-site-ipsecike-vpn-tunnel"></a>Siteden Siteye ve Çok Siteli (IPsec/IKE VPN tüneli)
### <a name="site-to-site"></a>Siteden Siteye
Siteden Siteye (S2S) VPN ağ geçidi bağlantısı, IPSec/IKE (IKEv1 veya IKEv2) VPN tüneli üzerinden kurulan bir bağlantıdır. Bu bağlantı türü için, şirket içinde ortak IP adresi atanmış olan ve NAT'nin arkasında bulunmayan bir VPN cihazı gerekir. S2S bağlantıları, şirket içi ve dışı yapılandırmalar ile birlikte karma yapılandırmalar için kullanılabilir.    
![Siteden siteye VPN bağlantısı yapılandırma örneği](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="multi-site"></a>Çok Siteli
Bu türden bir bağlantı, Siteden Siteye bağlantının bir çeşididir. Sanal ağ geçidinizden genellikle birden fazla şirket içi siteye bağlanan birden fazla VPN bağlantısı oluşturursunuz. Birden fazla bağlantıyla çalışırken Yol Tabanlı VPN türü (klasik sanal ağlar ile çalışırken “dinamik ağ geçidi” adıyla kullanılır) kullanmanız gerekir. Her sanal ağın yalnızca bir VPN ağ geçidi olabileceğinden, ağ geçidi boyunca tüm bağlantılar mevcut bant genişliğini paylaşır. Bu tür genellikle "çok siteli" bağlantı olarak adlandırılır.   
![Azure VPN Gateway Çok Siteli bağlantı örneği](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-multisite-connection-diagram.png)



## <a name="gateway-skus"></a>Ağ geçidi SKU'ları
Bir sanal ağ geçidi için Azure yığınına oluşturduğunuzda, kullanmak istediğiniz SKU ağ geçidi belirtin. Aşağıdaki VPN ağ geçidi SKU'ları desteklenir:
- Temel
- Standart
- HighPerformance

Daha yüksek bir ağ geçidi SKU'su, Basic üzerinden standart ya da HighPerformance gibi standart veya Basic seçtiğinizde, ağ geçidine daha fazla CPU ve ağ bant genişliği ayrılır. Sonuç olarak, ağ geçidi sanal ağ için daha yüksek ağ verimliliği destekleyebilir.

Azure yığın UltraPerformance ağ geçidi özel olarak hızlı rota ile kullanılan SKU desteklemiyor.

SKU seçtiğinizde aşağıdakileri göz önünde bulundurun:
- Azure yığın ilke tabanlı ağ geçitleri desteklemez.
- Sınır Ağ Geçidi Protokolü (BGP) temel SKU üzerinde desteklenmiyor.
- ExpressRoute VPN ağ geçidi bir arada yapılandırmaları Azure yığınında desteklenmiyor
- Etkin-etkin S2S VPN Gateway bağlantıları, yalnızca HighPerformance değerine sahip SKU'larda yapılandırılabilir.

## <a name="estimated-aggregate-throughput-by-sku"></a>SKU'ya göre tahmini toplam verimlilik
Aşağıdaki tabloda ağ geçidi türleri ve ağ geçidi SKU’suna göre tahmini toplam verimlilik gösterilmiştir.

|   | VPN Gateway işlemesi *(1)* | VPN Gateway maks. IPSec tünelleri *(2)* |
|-------|-------|-------|
|**Temel SKU** ***(3)***    | 100 Mbps  | 10    |
|**Standart SKU**       | 100 Mbps  | 10    |
|**Yüksek performanslı SKU** | 200 Mbps    | 5 |
***(1)***  VPN verimlilik değil şirket içi bağlantılar için garantili işleme Internet üzerinden. Mümkün olan en yüksek verimlilik ölçümüdür.  
***(2)***  Max tüneller sayıdır tüm abonelikler için Azure yığın dağıtımına başına toplam.  
***(3)***  BGP temel SKU için desteklenmiyor.  

## <a name="next-steps"></a>Sonraki adımlar
Hakkında bilgi edinin [VPN ağ geçitleri için ayarları](azure-stack-vpn-gateway-settings.md) Azure yığınının.
