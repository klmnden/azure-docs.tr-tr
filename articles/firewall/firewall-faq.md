---
title: Azure güvenlik duvarı ile ilgili SSS
description: Azure Güvenlik Duvarı hakkında SSS
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 5/30/2019
ms.author: victorh
ms.openlocfilehash: 75b1131f2853cb444481b9c7a6c96e28f8537538
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66384668"
---
# <a name="azure-firewall-faq"></a>Azure güvenlik duvarı ile ilgili SSS

## <a name="what-is-azure-firewall"></a>Azure Güvenlik Duvarı nedir?

Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Bir durum bilgisi olan tam olarak güvenlik duvarı olarak-hizmet ile yerleşik yüksek kullanılabilirlik ve ölçeklenebilirlik sınırsız bulut var. Aboneliklerle sanal ağlarda uygulama ve ağ bağlantısı ilkelerini merkezi olarak oluşturabilir, zorlayabilir ve günlüğe alabilirsiniz.

## <a name="what-capabilities-are-supported-in-azure-firewall"></a>Azure Güvenlik Duvarı'nda hangi özellikler destekleniyor?

* Hizmet olarak durum bilgisi olan güvenlik duvarı
* Sınırsız bulut ölçeklenebilirliği içeren yerleşik yüksek kullanılabilirlik
* FQDN filtreleme
* FQDN etiketleri
* Ağ trafiği filtreleme kuralları
* Giden SNAT desteği
* Gelen DNAT desteği
* Merkezi olarak oluşturma, zorlama ve Azure abonelikleri ve sanal ağlar arasında bağlantı ilkeleri uygulama ve ağ oturum
* Günlüğe kaydetme ve analiz için tamamen tümleşik Azure İzleyici

## <a name="what-is-the-typical-deployment-model-for-azure-firewall"></a>Azure Güvenlik Duvarı için tipik bir dağıtım modeli nedir?

Azure güvenlik duvarı herhangi bir sanal ağda dağıtabilirsiniz, ancak müşteriler genellikle merkez sanal ağda dağıtmak ve diğer sanal ağlara ona hub-and-spoke modelini eş. Varsayılan yol, bu Merkezi güvenlik duvarı sanal ağa işaret edecek şekilde eşlenen sanal ağlardan sonra ayarlayabilirsiniz. Küresel VNet eşlemesi desteklenir ancak bu olası performans ve gecikme sorunlarını nedeniyle bölgeler arasında önerilmez. En iyi performans için her bölge bir güvenlik duvarı dağıtın.

Bu modelin avantajı, merkezi olarak farklı abonelikler arasında birden çok uç sanal ağları denetimde uygulamak olanağıdır. Aynı zamanda her bir sanal ağ içindeki bir güvenlik duvarı ayrı ayrı dağıtmanız gerekmez olarak da maliyet tasarrufu vardır. Maliyet tasarruflarından ile müşteri trafik düzenlerini esas alarak ilişkilendirme eşleme maliyetin ölçülen.

## <a name="how-can-i-install-the-azure-firewall"></a>Azure Güvenlik Duvarı'nı nasıl yükleyebilirim?

Azure Güvenlik Duvarı'nı, Azure portalı, PowerShell, REST API kullanarak veya şablonları kullanarak ayarlayabilirsiniz. Bkz: [Öğreticisi: Dağıtma ve Azure Azure portalını kullanarak güvenlik duvarı yapılandırma](tutorial-firewall-deploy-portal.md) adım adım yönergeler için.

## <a name="what-are-some-azure-firewall-concepts"></a>Bazı Azure güvenlik duvarı kavramları nelerdir?

Azure güvenlik duvarı kuralları ve kural koleksiyonlarınızı destekler. Bir kural koleksiyonu, aynı sıraya ve önceliği paylaşan bir kurallar kümesidir. Kural koleksiyonları, öncelik sırasına göre çalıştırılır. Ağ kural topluluklarıdır uygulama kural koleksiyonları daha yüksek bir öncelik ve tüm kuralları sonlandırılıyor.

Kural koleksiyonları üç tür vardır:

* *Uygulama kuralları*: Bir alt ağdan erişilebilen, tam etki alanı adlarını (FQDN) yapılandırın.
* *Ağ kuralları*: Kaynak adresleri, protokolleri, hedef bağlantı noktaları ve adresleri içeren kurallarını yapılandırın.
* *NAT kuralları*: Dnat'ı kurallarını, gelen bağlantılara izin verecek şekilde yapılandırın.

## <a name="does-azure-firewall-support-inbound-traffic-filtering"></a>Azure güvenlik duvarı, gelen trafiği filtreleme destekliyor mu?

Azure güvenlik duvarı, gelen ve giden filtrelemeyi destekler. HTTP/S dışındaki protokoller için gelen korumadır. Örneğin RDP, SSH ve FTP protokolleri.

## <a name="which-logging-and-analytics-services-are-supported-by-the-azure-firewall"></a>Günlüğe kaydetme ve Analiz Hizmetleri Azure güvenlik duvarı tarafından destekleniyor mu?

Azure güvenlik duvarı, görüntüleme ve güvenlik duvarı günlükleri analiz etmek için Azure İzleyici ile tümleşiktir. Günlükleri Log Analytics, Azure depolama veya Event Hubs gönderilebilir. Bunlar, Log analytics'te veya Excel ve Power BI gibi farklı araçları tarafından çözümlenebilir. Daha fazla bilgi için [Öğreticisi: Azure güvenlik duvarı günlükleri izleme](tutorial-diagnostics.md).

## <a name="how-does-azure-firewall-work-differently-from-existing-services-such-as-nvas-in-the-marketplace"></a>Azure güvenlik duvarı farklı Market'te nva'ları gibi var olan hizmetlerden nasıl sağlanır?

Azure güvenlik duvarı, belirli müşteri senaryoları ele bir temel güvenlik duvarı hizmetidir. Üçüncü taraf nva'ları ve Azure Güvenlik Duvarı bir karışımını gerekir beklenmektedir. Birlikte daha iyi çalışan temel bir önceliktir.

## <a name="what-is-the-difference-between-application-gateway-waf-and-azure-firewall"></a>Azure güvenlik duvarı ile Application Gateway WAF arasındaki fark nedir?

Web uygulaması Güvenlik Duvarı (WAF), web uygulamalarınızda açıklardan yararlanmaya ve güvenlik açıkları gelen merkezi koruma sağlayan bir Application Gateway özelliğidir. Azure güvenlik duvarı gelen koruma için HTTP/S olmayan protokolleri (örneğin, RDP, SSH, FTP), tüm bağlantı noktaları ve protokoller için giden ağ düzeyinde koruma ve uygulama düzeyinde koruma için giden HTTP/s sunar.

## <a name="what-is-the-difference-between-network-security-groups-nsgs-and-azure-firewall"></a>Ağ güvenlik grupları (Nsg'ler) ile Azure güvenlik duvarı arasındaki fark nedir?

Azure Güvenlik Duvarı hizmeti, ağ güvenlik grubu işlevselliğini tamamlar. Birlikte daha iyi "derinlemesine savunma" ağ güvenliği sağlar. Dağıtılmış ağ katmanı trafik filtreleme trafiğini her bir Abonelikteki sanal ağ içindeki kaynaklarla sınırlama için ağ güvenlik grupları belirtin. Bir tam durum bilgisi olan, merkezi bir ağ güvenlik duvarı-farklı abonelikler ve sanal ağları arasında ağ ve uygulama düzeyinde koruma sağlayan hizmet olarak, Azure güvenlik duvarı olup.

## <a name="are-network-security-groups-nsgs-supported-on-the-azure-firewall-subnet"></a>Ağ güvenlik grupları (Nsg'ler), Azure güvenlik duvarı alt ağda destekleniyor mu?

Azure güvenlik duvarı, NIC düzeyinde Nsg'ler (görüntülenemez) ile platform koruması da dahil olmak üzere birden çok koruma katmanları ile yönetilen bir hizmettir.  Alt ağ düzeyindeki Nsg'leri Azure güvenlik duvarı alt ağda gerekmez ve hizmet kesintisine neden olmadan emin olmak için devre dışı bırakılır.


## <a name="how-do-i-set-up-azure-firewall-with-my-service-endpoints"></a>My hizmet uç noktaları ile Azure Güvenlik Duvarı'nı nasıl ayarlayabilirim?

PaaS hizmetlerine güvenli erişim için hizmet uç noktaları önerilir. Azure güvenlik duvarı alt ağdaki hizmet uç noktalarının etkinleştirilmesi ve bağlı uç sanal ağlarında devre dışı bırakmak isteyebilirsiniz. Bu şekilde özellikleri--Hizmeti uç nokta güvenliği ve tüm trafik için Merkezi günlük yararlanır.

## <a name="what-is-the-pricing-for-azure-firewall"></a>Azure Güvenlik Duvarı için fiyatlandırma nedir?

Bkz: [Azure güvenlik duvarı fiyatlandırma](https://azure.microsoft.com/pricing/details/azure-firewall/).

## <a name="how-can-i-stop-and-start-azure-firewall"></a>Nasıl durdurun ve Azure güvenlik duvarını başlatma?

Azure PowerShell kullanabilirsiniz *serbest* ve *tahsis* yöntemleri.

Örneğin:

```azurepowershell
# Stop an existing firewall

$azfw = Get-AzFirewall -Name "FW Name" -ResourceGroupName "RG Name"
$azfw.Deallocate()
Set-AzFirewall -AzureFirewall $azfw
```

```azurepowershell
# Start a firewall

$azfw = Get-AzFirewall -Name "FW Name" -ResourceGroupName "RG Name"
$vnet = Get-AzVirtualNetwork -ResourceGroupName "RG Name" -Name "VNet Name"
$publicip = Get-AzPublicIpAddress -Name "Public IP Name" -ResourceGroupName " RG Name"
$azfw.Allocate($vnet,$publicip)
Set-AzFirewall -AzureFirewall $azfw
```

> [!NOTE]
> Bir güvenlik duvarı ve genel IP özgün kaynak grubu ve abonelik tahsis gerekir.

## <a name="what-are-the-known-service-limits"></a>Bilinen hizmet sınırları nelerdir?

Azure Güvenlik Duvarı hizmeti limitleri için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md#azure-firewall-limits).

## <a name="can-azure-firewall-in-a-hub-virtual-network-forward-and-filter-network-traffic-between-two-spoke-virtual-networks"></a>Azure Güvenlik Duvarı bir hub sanal ağında İleri olabilir ve iki ağlı sanal ağlar arasındaki ağ trafiğini filtreleme?

Evet, hub sanal ağında iki uç sanal ağ arasında yönlendirme ve filtreleme trafiği Azure Güvenlik Duvarı'nı kullanabilirsiniz. Her bağlı sanal ağlar, alt ağları düzgün çalışması için Azure Güvenlik Duvarı bu senaryo için bir varsayılan ağ geçidi olarak işaret eden UDR olması gerekir.

## <a name="can-azure-firewall-forward-and-filter-network-traffic-between-subnets-in-the-same-virtual-network-or-peered-virtual-networks"></a>Azure güvenlik duvarı İleri yapabilirsiniz ve aynı sanal ağdaki veya eşlenmiş sanal ağlardaki alt ağlar arasındaki ağ trafiğini filtreleme?

Evet. Ancak, aynı sanal ağda alt ağlar arasındaki trafiği yönlendirmek için Udr'leri yapılandırma, ek bir ilgi gerektirir. Hedef öneki UDR için yeterli olduğundan sanal ağ adres aralığı kullanırken, bu da Azure güvenlik duvarı örneği ile aynı alt ağda başka bir makineye bir makineden tüm trafiği yönlendirir. Bunu önlemek için bir sonraki atlama türü ile UDR alt ağ için bir rota içerecek. **VNET**. Bu yolları yönetmek sıkıcı ve hataya açık olabilir. İç ağ kesimleme için önerilen yöntem, Udr gerektirmeyen ağ güvenlik grupları kullanmaktır.

## <a name="is-forced-tunnelingchaining-to-a-network-virtual-appliance-supported"></a>Bir sanal ağ desteklenen Gerecine tünel oluşturma ve zincirleme zorlanır?

Varsayılan olarak zorlamalı tünel desteklenmez, ancak destek yardımıyla etkinleştirilebilir.

Azure güvenlik duvarı, doğrudan Internet bağlantısı olması gerekir. Bir varsayılan yolu BGP aracılığıyla şirket içi ağınıza, AzureFirewallSubnet öğrenir, bu ile 0.0.0.0/0 UDR ile geçersiz kılmanız gerekir **NextHopType** değer kümesini olarak **Internet** doğrudan korumak için Internet bağlantısı. Varsayılan olarak, bir şirket içi ağ için zorlamalı tünel, Azure Güvenlik Duvarı'nı desteklemez.

Bir şirket içi ağ için zorlamalı tünel yapılandırma gerektirir, ancak Microsoft bunu tek olay temelinde destekler. Biz durumunuzu gözden geçirmek, desteğe başvurun. Kabul ediyoruz aboneliğinizi ve gerekli güvenlik duvarı Internet bağlantısı korunduğundan emin olmak.

## <a name="are-there-any-firewall-resource-group-restrictions"></a>Kaynak grubu kısıtlaması herhangi bir güvenlik duvarı vardır?

Evet. Güvenlik Duvarı, alt ağ, sanal ağ ve genel IP adresini tüm aynı kaynak grubunda olmalıdır.

## <a name="when-configuring-dnat-for-inbound-network-traffic-do-i-also-need-to-configure-a-corresponding-network-rule-to-allow-that-traffic"></a>Gelen ağ trafiğini dnat'ı yapılandırırken, ayrıca bu trafiğe izin vermek için ilgili ağ kuralını yapılandırmak ihtiyacım var?

Hayır. NAT kuralları, örtük olarak çevrilmiş trafiğine izin verecek şekilde karşılık gelen bir ağ kuralı ekleyin. Bu davranışı, çevrilen trafikle eşleşen reddetme kuralları olan bir ağ kural koleksiyonunu açıkça ekleyerek geçersiz kılabilirsiniz. Azure Güvenlik Duvarı kural işleme mantığı hakkında daha fazla bilgi için bkz: [Azure Güvenlik Duvarı kural işleme mantığı](rule-processing.md).

## <a name="how-do-wildcards-work-in-an-application-rule-target-fqdn"></a>Joker karakterler bir uygulama kuralı hedef FQDN nasıl çalışır?

Yapılandırırsanız * **. contoso.com**, veren *anyvalue*. contoso.com, ancak contoso.com (etki alanı tepesinde). Etki alanı tepesinde izin vermek istiyorsanız, bunu açıkça FQDN hedef olarak yapılandırmanız gerekir.

## <a name="what-does-provisioning-state-failed-mean"></a>Ne yaptığını *sağlama durumu: Başarısız* anlama gelir?

Bir yapılandırma değişikliği olduğunda, Azure güvenlik duvarı, temel alınan tüm arka uç örnekleriyle güncelleştirmek çalışır. Nadiren de olsa, bu arka uç örnekleriyle birini daha yeni yapılandırmayla başarısız olabilir ve başarısız bir sağlama durumu ile güncelleştirme işlemi durdurur. Azure güvenlik duvarınızı hala çalışıyor, ancak uygulanan yapılandırma bazı örnekleri önceki yapılandırmayı güncelleştirilmiş kural kümesi sahip olduğu diğer sahip olduğu tutarsız bir durumda olabilir. Böyle bir durumda güncelleştirme yapılandırma işlemi başarılı olur ve güvenlik duvarınızı bulunduğu kadar bir kez daha deneyin bir *başarılı* sağlama durumu.
