---
title: Azure DNS hakkında SSS
description: Azure DNS hakkında sık sorulan sorular
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 6/15/2019
ms.author: victorh
ms.openlocfilehash: bb5c4d508344f391d610aeaa7e0be54a93c997dc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080009"
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

## <a name="next-steps"></a>Sonraki adımlar

- [Azure DNS hakkında daha fazla bilgi](dns-overview.md).

- [Azure DNS özel etki alanları için kullanma hakkında daha fazla bilgi edinin](private-dns-overview.md).

- [DNS bölgeleri ve kayıtları hakkında daha fazla bilgi edinin](dns-zones-records.md).

- [Azure DNS ile çalışmaya başlama](dns-getstarted-portal.md).
