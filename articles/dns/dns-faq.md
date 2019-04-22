---
title: Azure DNS hakkında SSS
description: Azure DNS hakkında sık sorulan sorular
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 3/21/2019
ms.author: victorh
ms.openlocfilehash: 4f0800dfd264059e1dc8aac32a54f216f777647f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58905724"
---
# <a name="azure-dns-faq"></a>Azure DNS hakkında SSS

## <a name="about-azure-dns"></a>Azure DNS hakkında

### <a name="what-is-azure-dns"></a>Azure DNS nedir?

Etki alanı adı sistemi (DNS) çevirir ve çözümler, IP adresini bir Web sitesi veya hizmet adı. Azure DNS, DNS etki alanları için bir barındırma hizmetidir. Bu, Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlar. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz.

Azure DNS'de DNS etki alanlarını DNS ad sunucuları Azure genel ağda barındırılır. Bu sistem, böylece her DNS sorgusu en yakın kullanılabilir DNS sunucusu tarafından yanıtlanır Anycast ağ kullanır. Azure DNS, hızlı performans ve etki alanınız için yüksek kullanılabilirlik sağlar.

Azure DNS, Azure Resource Manager üzerindeki temel alır. Azure DNS, Resource Manager rol tabanlı erişim denetimi, denetim günlüklerini ve kaynak kilitleme gibi özelliklerinden yararlanır. Etki alanları ve Azure portalı, Azure PowerShell cmdlet'leri ve platformlar arası Azure CLI aracılığıyla kayıtlarını yönetebilirsiniz. Otomatik DNS Yönetimi gerektiren uygulamalar hizmeti SDK'ları ve REST API ile tümleştirilebilir.

### <a name="how-much-does-azure-dns-cost"></a>Azure DNS nin ücreti ne kadardır?

Azure DNS faturalandırma modeli, Azure DNS'de barındırılan DNS bölgelerinin sayısı temel alır. Ayrıca DNS sorguları aldıkları sayısına temel alır. Kullanıma göre indirim sağlanır.

Daha fazla bilgi için [Azure DNS fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-the-sla-for-azure-dns"></a>Azure DNS için hani SLA sunulur?

Azure, geçerli DNS isteklerinin en az bir Azure DNS ad sunucusundan yanıt süresini %100 alma garanti eder.

Daha fazla bilgi için [Azure DNS SLA sayfamızda](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>DNS bölgesi nedir? DNS etki alanıyla aynı şey midir? 

Bir etki alanında benzersiz bir ad etki alanı adı sistemi ' dir. Örneğin: contoso.com.

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Örneğin, contoso.com etki alanı birden fazla DNS kaydını içerebilir. Bir posta sunucusu ve www mail.contoso.com kayıtları içerebilir\.bir Web sitesi contoso.com. Bu kayıtlar, contoso.com bölgesindeki DNS barındırılır.

Bir etki alanı adı *yalnızca bir ad*. Bir DNS bölgesi bir etki alanı adı için DNS kayıtlarını içeren bir veri kaynaktır. Azure DNS’yi kullanarak bir DNS bölgesi barındırabilir ve Azure'da bir etki alanının DNS kayıtlarını yönetebilirsiniz. Ayrıca, Internet'ten DNS sorgularını yanıtlamak için DNS ad sunucularını sağlar.

### <a name="do-i-need-to-buy-a-dns-domain-name-to-use-azure-dns"></a>Azure DNS kullanacak şekilde bir DNS etki alanı adı satın alma gerekiyor mu? 

Olmayabilir.

Azure DNS'de bir DNS bölgesi barındırmak için bir etki alanı satın almanız gerekmez. Bir etki alanı adına sahip olmadan, istediğiniz zaman DNS bölgesi oluşturabilirsiniz. Bunlar bölgesi için atanmış Azure DNS ad sunucularına yönlendirilirsiniz, bu bölge için DNS sorgularını yalnızca çözümleyin.

DNS bölgenizi Genel DNS hiyerarşisine bağlamak için etki alanı adını satın almanız gerekir. Ardından, DNS sorguları yerden dünyada DNS bölgesi ve DNS kayıtlarınızı ile yanıt bulun.

## <a name="azure-dns-features"></a>Azure DNS özellikleri

### <a name="are-there-any-restrictions-when-using-alias-records-for-a-domain-name-apex-with-traffic-manager"></a>Herhangi bir kısıtlama için bir etki alanı adı apex Traffic Manager ile diğer ad kayıtlarını kullanırken var mıdır?

Evet. Azure Traffic Manager ile statik genel IP adreslerini kullanmanız gerekir. Yapılandırma **dış uç noktası** statik bir IP adresi kullanarak hedef. 

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Azure DNS, DNS tabanlı trafik yönlendirme veya uç nokta yük devretme destekliyor mu?

DNS tabanlı trafik yönlendirme ve uç nokta yük devretme Traffic Manager tarafından sağlanır. Traffic Manager, Azure DNS ile kullanılabilmesi için ayrı bir Azure hizmetidir. Daha fazla bilgi için [Traffic Manager'a genel bakış](../traffic-manager/traffic-manager-overview.md).

Azure DNS, yalnızca belirli bir DNS kaydı için her bir DNS sorgusu her zaman aynı DNS yanıtı alır burada statik DNS etki alanı, barındırma destekler.

### <a name="does-azure-dns-support-domain-name-registration"></a>Azure DNS, etki alanı adı kayıt destekliyor mu?

Hayır. Azure DNS, etki alanı adları satın alma seçeneği şu anda desteklemiyor. Etki alanları satın almak için bir üçüncü taraf etki alanı adı kayıt şirketi kullanmanız gerekir. Kayıt şirketi, genelde küçük bir yıllık ücret uygular. Etki alanlarını Azure DNS'de DNS kayıtlarını yönetimi için ardından barındırılabilir. Daha fazla bilgi için bkz. [Bir etki alanını Azure DNS'ye devretme](dns-domain-delegation.md).

Etki alanı adları satın alma özelliği, Azure biriktirme listesinde izlenir. Kullanmak için geri bildirim sitesinde [bu özellik için destek kaydetme](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-dnssec"></a>Azure DNS, DNSSEC destekliyor mu?

Hayır. Azure DNS, etki alanı adı sistemi Güvenlik Uzantıları (DNSSEC) şu anda desteklemiyor.

DNSSEC özelliği, Azure DNS biriktirme listesinde izlenir. Kullanmak için geri bildirim sitesinde [bu özellik için destek kaydetme](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Azure DNS bölge aktarımı (AXFR/IXFR) destekliyor mu?

Hayır. Azure DNS bölge aktarımlarını şu anda desteklemiyor. DNS bölgeleri olabilir [Azure CLI'yi kullanarak Azure DNS alınan](dns-import-export.md). DNS kayıtları aracılığıyla yönetilir [Azure DNS Yönetim Portalı](dns-operations-recordsets-portal.md), [REST API](https://docs.microsoft.com/powershell/module/az.dns), [SDK](dns-sdk.md), [PowerShell cmdlet'leri](dns-operations-recordsets.md), veya [ CLI aracı](dns-operations-recordsets-cli.md).

Bölge aktarma özelliği, Azure DNS biriktirme listesinde izlenir. Kullanmak için geri bildirim sitesinde [bu özellik için destek kaydetme](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>Azure DNS, URL yeniden yönlendirmeleri destekliyor mu?

Hayır. URL yeniden yönlendirme hizmetleri, DNS hizmeti değildir. DNS düzeyi yerine HTTP düzeyinde çalışırlar. Bazı DNS sağlayıcıları genel tekliflerinin bir parçası olarak bir URL yeniden yönlendirme hizmeti paket. Bu hizmet, Azure DNS tarafından şu anda desteklenmemektedir.

URL yeniden yönlendirme özelliği, Azure DNS biriktirme listesinde izlenir. Kullanmak için geri bildirim sitesinde [bu özellik için destek kaydetme](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

### <a name="does-azure-dns-support-the-extended-ascii-encoding-8-bit-set-for-txt-record-sets"></a>Azure DNS TXT kayıt kümesi (8-bit) kümesi kodlama genişletilmiş ASCII destekliyor mu?

Evet. Azure DNS TXT kayıt kümeleri için ayarlanan kodlama genişletilmiş ASCII destekler. Ancak Azure REST API, SDK'ları, PowerShell ve CLI'yı en son sürümünü kullanmanız gerekir. 1 Ekim 2017'den daha eski sürümleri ya da SDK 2.1 genişletilmiş ASCII kümesi desteklemez. 

Örneğin, bir dize değeri olarak genişletilmiş ASCII karakter \128 içeren bir TXT kaydı için sağlayabilir. "Abcd\128efgh." örneğidir Azure DNS, iç gösteriminde 128'dir, bu karakterin bayt değeri kullanır. DNS çözümlemesi zamanında yanıt olarak bu bayt değeri döndürülür. Ayrıca çözüm endişe kadar "abc" ve "\097\098\099" birbirinin yerine olduğunu unutmayın. 

Sizinle [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt) bölge dosyası ana biçim kaçış kuralları TXT kayıtları. Örneğin, `\` gerçekten çıkışları RFC başına her şey şimdi. Belirtirseniz `A\B` TXT kaydı değer olarak, bu temsil olup olarak çözümlendi `AB`. TXT kaydı için gerçekten istiyorsanız `A\B` çözünürlükte, kaçış karakteriyle `\` yeniden. Örneğin, belirtin `A\\B`.

Bu destek, Azure portalından oluşturulan TXT kayıtları için şu anda kullanılamıyor.

## <a name="alias-records"></a>Diğer ad kayıtları

### <a name="what-are-some-scenarios-where-alias-records-are-useful"></a>Diğer ad kayıtlarını yararlı olduğu bazı senaryolar nelerdir?

Senaryoları kısmına bakın [Azure DNS diğer ad kayıtlarını genel bakış](dns-alias.md).

### <a name="what-record-types-are-supported-for-alias-record-sets"></a>Hangi kayıt türleri için diğer ad kaydı kümeleri desteklenir?

Diğer kayıt kümeleri, aşağıdaki kayıt türlerinin bir Azure DNS bölgesindeki desteklenir:
 
- A 
- AAAA
- CNAME 

### <a name="what-resources-are-supported-as-targets-for-alias-record-sets"></a>Hangi kaynakların diğer ad kaydı kümeleri için hedefler olarak destekleniyor mu?

- **Bir genel IP kaynağı için bir DNS A/AAAA kayıt kümesi gelin.** A/AAAA kayıt kümesi oluşturma ve bir genel IP kaynağına işaret edecek bir diğer ad kayıt kümesi kolaylaştırır.
- **Traffic Manager profili için bir DNS A/AAAA/CNAME kayıt kümesi gelin.** Bir DNS CNAME kayıt kümesi için bir Traffic Manager profilinin CNAME işaret edebilir. Contoso.trafficmanager.net buna bir örnektir. Şimdi de dış uç noktalardan gelen bir A veya AAAA kaydı DNS bölgenizi kümesi olan bir Traffic Manager profiline işaret edebilir.
- **Bir Azure Content Delivery Network (CDN) uç noktası**. Azure depolama ve Azure CDN kullanarak statik Web sitesi oluşturduğunuzda, bu yararlıdır.
- **Aynı bölgede başka bir DNS kayıt kümesi gelin.** Diğer ad kayıtlarını aynı türdeki diğer kayıt kümelerine başvurabilir. Örneğin, bir DNS CNAME kayıt kümesinin aynı türdeki başka bir CNAME kayıt kümesine diğer ad olmasını sağlayabilirsiniz. Bu düzenleme, diğer adlar olmasını bazı kayıt kümelerini ve bazı diğer olmayan istiyorsanız kullanışlıdır.

### <a name="can-i-create-and-update-alias-records-from-the-azure-portal"></a>Ben oluşturabilir ve diğer ad kayıtlarını Azure portalından güncelleştirme?

Evet. Oluşturun veya diğer ad kayıtlarını Azure REST API'ler, PowerShell, CLI ve SDK'ları ile birlikte Azure portalında yönetme.

### <a name="will-alias-records-help-to-make-sure-my-dns-record-set-is-deleted-when-the-underlying-public-ip-is-deleted"></a>Diğer ad kayıtlarını my DNS kayıt kümesi, temel alınan genel IP silindiğinde silinir emin olmak için yardımcı olur?

Evet. Bu özellik, diğer ad kayıtlarını temel özellikleriyle biridir. Uygulamanızın kullanıcıları için olası kesintileri önlemek yardımcı olur.

### <a name="will-alias-records-help-to-make-sure-my-dns-record-set-is-updated-to-the-correct-ip-address-when-the-underlying-public-ip-address-changes"></a>Diğer ad kayıtlarını temel genel IP adresini değiştiğinde my DNS kayıt kümesi için doğru IP adresi güncelleştirildiğinden emin olmak için Yardım?

Evet. Bu özellik, diğer ad kayıtlarını temel özellikleriyle biridir. Olası kesintileri veya uygulamanız için güvenlik risklerini önlemek yardımcı olur.

### <a name="are-there-any-restrictions-when-using-alias-record-sets-for-a-or-aaaa-records-to-point-to-traffic-manager"></a>Traffic Manager'a işaret edecek şekilde A veya AAAA kayıt için diğer ad kaydı kümeleri kullanırken herhangi bir kısıtlama var mıdır?

Evet. Bir Traffic Manager profiline bir A veya AAAA kaydı kümesinden Traffic Manager diğer ad olarak işaret edecek şekilde profilini yalnızca harici son noktaları kullanması gerekir. Dış uç noktaları Traffic Manager'da oluşturduğunuzda, uç noktaları gerçek IP adreslerini sağlar.

### <a name="is-there-an-additional-charge-to-use-alias-records"></a>Diğer ad kayıtlarını kullanmak için ek bir ücreti var mıdır?

Diğer ad, geçerli bir DNS kaydı kümesi üzerinde bir nitelik kayıtlardır. Hiçbir ek bir diğer ad kayıtlarını faturalandırması yoktur.

## <a name="use-azure-dns"></a>Azure DNS kullanma

### <a name="can-i-co-host-a-domain-by-using-azure-dns-and-another-dns-provider"></a>Kullanabilir miyim ortak sunuculuk bir etki alanını Azure DNS'ye ve başka bir DNS Sağlayıcısı'nı kullanarak?

Evet. Azure DNS, diğer DNS hizmetleriyle ortak bir barındırma etki alanlarını destekler.

Ortak barındırma yukarı ayarlamak için her iki sağlayıcıları için ad sunucularını işaret edecek şekilde etki alanına ait NS kayıtlarını değiştirin. Ad sunucusu (NS) etki alanı için DNS sorgularını hangi sağlayıcıları Al denetimi kaydeder. Azure DNS, diğer sağlayıcı ve üst bölgedeki NS kayıtlarının değiştirebilirsiniz. Üst bölge, genellikle etki alanı adı kayıt şirketi aracılığıyla yapılandırılır. DNS temsilcisi hakkında daha fazla bilgi için bkz. [DNS etki alanı temsilcisi](dns-domain-delegation.md).

Ayrıca, etki alanı için DNS kayıtlarını hem DNS sağlayıcıları arasında eşitlenmiş olduğundan emin olun. Azure DNS, DNS bölge aktarımlarını şu anda desteklemiyor. Kullanarak DNS kayıtlarını eşitlenmelidir [Azure DNS Yönetim Portalı](dns-operations-recordsets-portal.md), [REST API](https://docs.microsoft.com/powershell/module/az.dns), [SDK](dns-sdk.md), [PowerShell cmdlet'leri](dns-operations-recordsets.md), veya [CLI aracı](dns-operations-recordsets-cli.md).

### <a name="do-i-have-to-delegate-my-domain-to-all-four-azure-dns-name-servers"></a>Etki alanım tüm dört Azure DNS ad sunucularına temsilci gerekiyor mu?

Evet. Azure DNS, her DNS bölgesine dört ad sunucusunun atar. Bu düzenleme, hata Yalıtımı ve artan esneklik içindir. Azure DNS SLA hakkı kazanmak için tüm dört ad sunucusunun etki alanınızla temsilci.

### <a name="what-are-the-usage-limits-for-azure-dns"></a>Azure DNS için kullanım sınırları nelerdir?

Azure DNS kullandığınızda aşağıdaki varsayılan sınırlar geçerlidir.

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>Bir Azure DNS bölgesi kaynak grupları arasında veya abonelikler arasında taşıyabilir miyim?

Evet. DNS bölgeleri, kaynak grupları arasında veya abonelikler arasında taşınabilir.

Bir DNS bölgesi taşıdığınız zaman DNS sorgularının üzerinde hiçbir etkisi yoktur. Bölgeye atanan ad sunucularını aynı kalır. DNS sorguları boyunca normal şekilde işlenir.

Daha fazla bilgi ve DNS bölgelerini taşımak yönergeler için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/resource-group-move-resources.md).

### <a name="how-long-does-it-take-for-dns-changes-to-take-effect"></a>Ne kadar süreyle DNS değişikliklerin etkili olması için sürer?

Genellikle Azure DNS ad sunucularını yeni DNS bölgeleri ve DNS kayıtlarını hızlı bir şekilde görünür. Zamanlama birkaç saniyedir.

Var olan DNS kayıtlarına yapılan değişikliklerin biraz uzun sürebilir. Genellikle Azure DNS ad sunucularını 60 saniye içinde görünürler. DNS, DNS özyinelemeli DNS istemcileri ile Azure DNS dışında Çözümleyicileri de önbelleğe alma, zamanlama etkileyebilir. Bu önbellek süresi denetlemek için her bir kayıt kümesi yaşam süresi (TTL) özelliğini kullanın.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>My DNS bölgelerini yanlışlıkla silinmesine karşı nasıl koruyabilirim?

Azure DNS, Azure Resource Manager kullanılarak yönetilir. Azure DNS, Azure Resource Manager sağlayan erişim denetimi özelliklerden faydalanır. Rol tabanlı erişim denetimi, hangi kullanıcıların okuma veya yazma erişimi DNS bölgelerini ve kayıt kümelerini denetler. Kaynak kilitleri yanlışlıkla değişiklik veya DNS bölgeleri ve kayıt kümelerini silinmesini engeller.

Daha fazla bilgi için [korumak DNS bölgelerini ve kayıtlarını](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>SPF kayıtlarının Azure DNS'de nasıl ayarlayabilirim?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="do-azure-dns-name-servers-resolve-over-ipv6"></a>Azure DNS ad sunucularını IPv6 üzerinden çözmeniz? 

Evet. Azure DNS ad sunucularını çift yığın ' dir. Çift yığın sahip oldukları IPv4 ve IPv6 adresleri anlamına gelir. IPv6 adresi Azure DNS, DNS bölgenize atanan ad sunucularını bulmak için nslookup gibi bir araç kullanın. `nslookup -q=aaaa <Azure DNS Nameserver>` bunun bir örneğidir.

### <a name="how-do-i-set-up-an-idn-in-azure-dns"></a>Azure DNS'de bir IDN nasıl ayarlayabilirim?

Uluslararası etki alanı adlarını (IDN'ler) kullanarak her bir DNS adı kodlama [punycode](https://en.wikipedia.org/wiki/Punycode). DNS sorguları bu punycode kodlu adları kullanılarak yapılır.

Azure DNS'de IDN'ler yapılandırmak için kayıt kümesi adı ve bölge adını kodlar, zayıf koda dönüştürün. Azure DNS şu anda yerleşik dönüştürme için veya punycode desteklememektedir.

## <a name="private-dns"></a>Özel DNS

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

### <a name="does-azure-dns-support-private-domains"></a>Azure DNS özel etki alanları destekliyor mu?

Özel etki alanı için destek özel bölgeleri özelliğini kullanarak uygulanır. Bu özellik şu anda genel önizlemede kullanılabilir. Özel bölgeler, internet'e yönelik Azure DNS bölgelerini aynı araçları kullanarak yönetilir. Bunlar, belirtilen sanal ağ dns'sinden yalnızca. Daha fazla bilgi için [genel bakış](private-dns-overview.md).

Özel bölgeler Azure portalında şu anda desteklenmiyor.

Azure'da diğer iç DNS seçenekleri hakkında daha fazla bilgi için bkz: [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="whats-the-difference-between-registration-virtual-network-and-resolution-virtual-network-in-the-context-of-private-zones"></a>Özel bölgeler bağlamında kayıt sanal ağı ve çözümleme sanal ağı arasındaki fark nedir?

DNS özel bölgesi için kayıt sanal ağı veya çözümleme sanal ağı olarak sanal ağlara bağlayabilirsiniz. Her iki durumda da, sanal ağdaki sanal makinelerin özel bölgedeki kayıtları karşı başarılı bir şekilde çözün. Kayıt sanal ağı ile DNS kayıtlarını sanal ağdaki sanal makineler için bir bölge içinde otomatik olarak kaydedilir. Bir sanal makine, sanal ağ silinmiş bir kayıt bağlı özel bölge karşılık gelen DNS kaydını otomatik olarak kaldırılır. 

### <a name="will-azure-dns-private-zones-work-across-azure-regions"></a>Azure bölgeleri arasında Azure DNS özel bölgeleri çalışacak mı?

Evet. Özel bölgeler için DNS çözümlemesi Azure bölgelerindeki sanal ağları arasında desteklenir. Özel bölgeler bile açıkça sanal ağları eşleme olmadan çalışır. Tüm sanal ağları, çözümleme sanal ağları özel bölge olarak belirtilmelidir. Müşteriler, bir bölgeden diğerine Akış TCP/HTTP trafiği için eşlenmiş sanal ağlar gerekebilir.

### <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>Bağlantı, özel bölgeler için gereken sanal ağlardan Internet'e mi?

Hayır. Özel bölgeler, sanal ağlar ile birlikte çalışır. Müşteriler, bunları sanal makineleri veya diğer kaynaklar içinde hem de sanal ağlar arasında etki alanlarını yönetmek için kullanın. Internet bağlantısı ad çözümlemesi için gerekli değildir. 

### <a name="can-the-same-private-zone-be-used-for-several-virtual-networks-for-resolution"></a>Aynı özel bölge çözümlemesi için birkaç sanal ağlar için kullanılabilir mi?

Evet. Müşteriler, 10 adede kadar çözümleme sanal ağları tek bir özel bölge ile ilişkilendirebilirsiniz.

### <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-added-as-a-resolution-virtual-network-to-a-private-zone"></a>Farklı bir aboneliğe ait bir sanal ağ özel bir bölgeye bir çözümleme sanal ağı eklenebilir?

Evet. Sanal ağlar ve özel DNS bölgesi yazma işlemi izni olmalıdır. Birkaç RBAC rolleri için yazma izni verilebilir. Örneğin, Klasik ağ Katılımcısı RBAC rolü sanal ağlar için yazma izinlerine sahiptir. RBAC rolleri hakkında daha fazla bilgi için bkz. [rol tabanlı erişim denetimi](../role-based-access-control/overview.md).

### <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-the-virtual-machines-are-deleted-by-the-customer"></a>Özel bir bölge içinde otomatik olarak kayıtlı sanal makinenin DNS kayıtlarını otomatik olarak sanal makineler müşteri tarafından silindiğinde silinir?

Evet. Kayıt sanal ağ içindeki sanal makineyi silme bölgeye kaydedilmiş DNS kayıtlarını otomatik olarak silinir. 

### <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-registration-virtual-network-be-deleted-manually"></a>Kayıt sanal ağdan özel bir bölgedeki bir sanal makine otomatik olarak kayıtlı kaydı el ile silinebilir?

Hayır. Kayıt sanal ağdan bir özel bölge içinde otomatik olarak kayıtlı sanal makinenin DNS kayıtlarını görünür veya müşteriler tarafından düzenlenebilir değildir. Bölgesinde el ile oluşturulan DNS kaydını otomatik olarak kayıtlı DNS kayıtlarını üzerine yazabilirsiniz. Bu konuda aşağıdaki soru ve yanıt adresi.

### <a name="what-happens-when-we-try-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-registration-virtual-network"></a>El ile kayıt sanal ağda aynı ana bilgisayar adı olarak var olan bir sanal makine otomatik olarak kayıtlı olan bir özel bölge içine yeni bir DNS kaydı oluşturmak çalıştığınızda ne olur?

El ile kayıt sanal ağı içinde var olan ve otomatik olarak kayıtlı bir sanal makine olarak aynı ana bilgisayar adı olan bir özel bölge içine yeni bir DNS kaydı oluşturma deneyin. Bunu yaptığınızda yeni DNS kaydını otomatik olarak kayıtlı sanal makine kaydı üzerine yazar. El ile oluşturulan bu DNS kaydını yeniden bölgeden silmeye çalışırsanız, silme başarılı olur. Otomatik kayıt, sanal makine hala mevcut olduğundan ve özel bir IP, kendisine eklenmiş sürece yeniden gerçekleşir. DNS kaydını otomatik olarak bölgede yeniden oluşturulur.

### <a name="what-happens-when-we-unlink-a-registration-virtual-network-from-a-private-zone-will-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-too"></a>Özel bir bölgeye kayıt sanal ağdan biz bağlantısını ne olur? Sanal ağdan sanal makine otomatik olarak kayıtlı kayıtları bölgeden çok kaldırılacak?

Evet. Özel bir bölgeye kayıt sanal ağdan bağlantısını kaldırmak için ilişkili kayıt sanal ağı kaldırmak için DNS bölgesini güncelleştirme. Bu işlemde otomatik olarak kaydedilmiş bir sanal makine kayıtları bölgesinden kaldırıldı. 

### <a name="what-happens-when-we-delete-a-registration-or-resolution-virtual-network-thats-linked-to-a-private-zone-do-we-have-to-manually-update-the-private-zone-to-unlink-the-virtual-network-as-a-registration-or-resolution--virtual-network-from-the-zone"></a>Biz, özel bir bölgeyle bağlantılı kayıt veya çözümleme sanal ağ sildiğinizde ne olur? Sanal ağda bir kayıt veya çözümleme sanal ağını bölgesinden bağlantısını için özel bölge el ile güncelleştirmek zorunda mıyım?

Evet. Bir kayıt veya çözümleme sanal ağ özel bölgesinden ilk bağlantısını olmadan sildiğinizde, silme işlemi başarılı olur. Ancak sanal ağ özel bölgeden varsa otomatik olarak bağlantısız değildir. El ile özel bölge sanal ağdan bağlantısını kaldırmanız gerekir. Bu nedenle, silmeden önce sanal ağınızdan özel bölgenizi bağlantısını Kaldır.

### <a name="will-dns-resolution-by-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-privatecontosocom-is-linked-to-a-virtual-network"></a>Hatta özel bir bölgesi (örneğin, private.contoso.com) bir sanal ağa bağlandığında kullanarak FQDN (internal.cloudapp.net) varsayılan DNS çözümlemesi çalışmaya devam eder mi?

Evet. Özel bölgeler için varsayılan DNS çözümleri, Azure tarafından sağlanan internal.cloudapp.net bölge kullanarak yerini almaz. Bir ek özellik veya geliştirme olarak sunulur. Azure tarafından sağlanan internal.cloudapp.net veya kendi özel bölge kullanan, karşı çözümlemek istediğiniz bölgeyi FQDN'sini kullanın. 

### <a name="will-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>Bağlı sanal ağ içindeki sanal makinelerde DNS soneki, özel bölge değiştirilecek?

Hayır. Sanal makinelere bağlı sanal ağınızdaki DNS soneki, varsayılan Azure tarafından sağlanan sonek olarak kalır ("*. internal.cloudapp.net"). El ile bu DNS soneki üzerindeki sanal makinelerinize, özel bölge değiştirebilirsiniz. 

### <a name="are-there-any-limitations-for-private-zones-during-this-preview"></a>Bu önizleme boyunca özel bölgeler için sınırlamalar vardır?

Evet. Genel Önizleme sırasında aşağıdaki sınırlamalar bulunmaktadır.
* Bir kayıt sanal ağı özel bölge başına.
* En fazla 10 çözümleme sanal ağları özel bölge başına.
* Kayıt sanal ağı olarak yalnızca bir özel bölge için sanal ağ bağlantıları belirli bir.
* Sanal ağ bağlantıları için 10 adede kadar özel bölgelerini çözümleme sanal ağı verilen bir.
* Kayıt sanal ağı belirtilmediği takdirde, özel bölgeye kaydedilen Vm'lerden söz konusu sanal ağ için DNS kayıtlarını görüntülenemez veya PowerShell, CLI veya API'ler aracılığıyla alınan. VM kayıtları kaydedilir ve başarılı bir şekilde çözün.
* Kayıt sanal ağ özel IP alanı için yalnızca geriye doğru DNS çalışır.
* Ters DNS özel bölgesi içinde kayıtlı değil özel bir IP için DNS son eki "internal.cloudapp.net" döndürür. Bu sonekin çözümlenemiyor. Özel bir IP, özel bir bölgeye çözümleme sanal ağı olarak bağlı bir sanal ağdaki bir sanal makine için bir örnek verilmiştir.
* Bir sanal ağ, özel bir bölgeye kayıt veya çözümleme sanal ağı olarak ilk kez bağlantı boş olmalıdır. Sanal ağ sonra boş olabilir gelecekteki diğer özel bölgelerine kayıt veya çözümleme sanal ağı olarak bağlama.
* Koşullu iletme, örneğin, Azure ve şirket içi ağlar arasındaki çözümleme etkinleştirmek için desteklenmiyor. Müşteriler, bu senaryo başka mekanizmalar aracılığıyla nasıl hayata geçirebilirsiniz öğrenin. Bkz: [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)

### <a name="are-there-any-quotas-or-limits-on-zones-or-records-for-private-zones"></a>Herhangi bir kota veya bölgeler ve kayıtlar özel bölgeler için sınırlar var mıdır?

Özel bölgeler için abonelik başına izin verilen bölge sayısı sınırı yoktur. Özel bölgeleri için bölge başına kayıt kümelerinin sayısı sınırı yoktur. Genel ve özel bölgeleri Genel DNS sınırları doğru sayısı. Daha fazla bilgi için [Azure aboneliği ve hizmet sınırlamaları](../azure-subscription-service-limits.md#azure-dns-limits)

### <a name="is-there-portal-support-for-private-zones"></a>Özel bölgeler için portal destek var mı?

API, PowerShell, CLI ve SDK'lar önceden oluşturulmuş özel bölgeler Azure portalında görünür. Ancak müşterilerin yeni özel bölgeler oluşturmak veya sanal ağları ilişkilerini yönetme. Kayıt sanal ağları ilişkili sanal ağlar için otomatik olarak kayıtlı VM kayıt Portalı'ndan görünmez. 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure DNS hakkında daha fazla bilgi](dns-overview.md).

- [Azure DNS özel etki alanları için kullanma hakkında daha fazla bilgi edinin](private-dns-overview.md).

- [DNS bölgeleri ve kayıtları hakkında daha fazla bilgi edinin](dns-zones-records.md).

- [Azure DNS ile çalışmaya başlama](dns-getstarted-portal.md).
