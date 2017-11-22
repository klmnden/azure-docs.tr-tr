---
title: Azure DNS SSS | Microsoft Docs
description: "Azure DNS hakkında sık sorulan sorular"
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/06/2017
ms.author: kumud
ms.openlocfilehash: a8cff450730d05970398989ceb3e8585aefd6771
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2017
---
# <a name="azure-dns-faq"></a>Azure DNS hakkında SSS

## <a name="about-azure-dns"></a>Azure DNS hakkında

### <a name="what-is-azure-dns"></a>Azure DNS nedir?

Etki alanı adı sistemi ya da DNS, çevirmek için sorumludur (veya çözme) bir Web sitesi ya da hizmet adı IP adresine. Azure DNS barındırma için bir DNS etki alanı, Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlamanın hizmetidir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz.

Azure DNS'de DNS etki alanı, Azure'nın genel DNS ad sunucuları ağda barındırılır. Böylece her DNS sorgusu en yakın kullanılabilir DNS sunucusu tarafından yanıtlanan her noktaya yayın ağ kullanın. Bu, etki alanınız için yüksek kullanılabilirlik ve hızlı performans sağlar.

Azure DNS hizmeti, Azure Resource Manager temel alır. Bu nedenle, rol tabanlı erişim denetimi, denetim günlüklerini ve kaynak kilitleme gibi Resource Manager özelliklerinden yararlanır. Etki alanları ve kayıtları Azure portalı, Azure PowerShell cmdlet'leri ve platformlar arası Azure CLI yönetilebilir. Otomatik DNS Yönetimi gerektiren uygulamalar, REST API ve SDK aracılığıyla hizmeti ile tümleştirebilirsiniz.

### <a name="how-much-does-azure-dns-cost"></a>Nasıl Azure DNS maliyeti nedir?

Azure DNS fatura modelini Azure DNS ve DNS sorgularının aldıkları sayısı barındırılan DNS bölgeleri sayısını temel alır. İndirim kullanımına dayalı sağlanır.

Daha fazla bilgi için bkz: [Azure fiyatlandırma sayfası DNS](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-the-sla-for-azure-dns"></a>Azure DNS için hani SLA sunulur?

Geçerli DNS isteklerinin en az %99,99’unun, en az bir Azure DNS ad sunucusundan yanıt alacağını garanti ediyoruz.

Daha fazla bilgi için bkz: [Azure DNS SLA sayfa](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>'DNS bölgesini' nedir? DNS etki alanıyla aynı şey midir? 

Bir etki alanında benzersiz bir ad etki alanı adı sistemi, örneğin "contoso.com" dir.

DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Örneğin, "contoso.com" etki alanı 'mail.contoso.com"(bir posta sunucusu) ve 'www.contoso.com' (bir web sitesi) gibi birçok DNS kayıtlarını içerebilir. Bunlar 'contoso.com' DNS bölgesindeki barındırılması.

Bir etki alanı adı *yalnızca bir adı*, buna karşın bir DNS bölgesi bir etki alanı adı için DNS kayıtlarını içeren bir veri kaynağı. Azure DNS, bir DNS bölgesi barındırmanızı ve Azure'de bir etki alanı için DNS kayıtlarını yönetmenizi sağlar. Ayrıca, Internet'ten DNS sorgularını yanıtlamak için DNS ad sunucuları sağlar.

### <a name="do-i-need-to-purchase-a-dns-domain-name-to-use-azure-dns"></a>Azure DNS’yi kullanmak için DNS etki alanı adı satın almam gerekiyor mu? 

Olmayabilir.

Azure DNS’de bir DNS bölgesi barındırmak için etki alanı satın almanız gerekmez. Bir etki alanı adına sahip olmadan, istediğiniz zaman DNS bölgesi oluşturabilirsiniz. Bu bölge için DNS sorgularını bölgeye atanan Azure DNS ad sunucularını yönlendirilir, yalnızca çözer.

DNS bölgenizi Genel DNS hiyerarşiye bağlamak istiyorsanız etki alanı adı satın almanız – bu DNS sorgularını yerden sağlayan DNS bölgenizi bulmak ve DNS kayıtlarınızı yanıtlanması dünyada.

## <a name="azure-dns-features"></a>Azure DNS özellikleri

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Azure DNS, DNS tabanlı trafik yönlendirme veya uç nokta yük devretme destekliyor mu?

DNS tabanlı trafik yönlendirme ve uç nokta yük devretme Azure trafik Yöneticisi tarafından sağlanır. Azure DNS ile birlikte kullanılan ayrı bir Azure hizmet budur. Daha fazla bilgi için bkz: [trafik Yöneticisi'ne genel bakış](../traffic-manager/traffic-manager-overview.md).

Azure DNS, yalnızca her bir DNS sorgusu belirli bir DNS kaydı için her zaman aynı DNS yanıtını aldıktan burada 'static' DNS etki alanı, barındırma destekler.

### <a name="does-azure-dns-support-domain-name-registration"></a>Azure DNS etki alanı adı kayıt destekliyor mu?

Hayır. Azure DNS, etki alanı adlarını satın alma şu anda desteklemiyor. Etki alanı satın almak istiyorsanız, bir üçüncü taraf etki alanı adı kayıt kullanmanız gerekir. Kayıt, genellikle küçük bir yıllık ücret ister. Etki alanları için DNS kayıtlarını Yönetimi sonra Azure DNS'de barındırılabilir. Bkz: [bir etki alanını Azure DNS'ye devretme](dns-domain-delegation.md) Ayrıntılar için.

Bu bizim biriktirme listesi üzerinde izlemekte olduğunuz bir özelliktir. Geri bildirim sitemizi kullanabilirsiniz [bu özellik için destek kaydetmek](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-private-domains"></a>Azure DNS 'özel' etki alanları destekliyor mu?
'Özel' etki alanı için destek, özel DNS bölgeleri kullanılarak uygulanır.  Bu özellik şu anda önizleme olarak kullanılabilir.  Özel DNS bölgeleri aynı araçlarını internet'e yönelik Azure DNS bölgeleri kullanılarak yönetilir ancak bunlar yalnızca, belirtilen sanal ağ dns'sinden olur.  Bkz: [genel bakış](private-dns-overview.md) Ayrıntılar için.

Azure'da diğer iç DNS seçenekleri hakkında daha fazla bilgi için bkz: [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="does-azure-dns-support-dnssec"></a>Azure DNS DNSSEC destekliyor mu?

Hayır. Azure DNS DNSSEC şu anda desteklemiyor.

Bu bizim biriktirme listesi üzerinde izlemekte olduğunuz bir özelliktir. Geri bildirim sitemizi kullanabilirsiniz [bu özellik için destek kaydetmek](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Azure DNS bölge aktarımları (AXFR/IXFR) destekliyor mu?

Hayır. Azure DNS bölge aktarımlarının şu anda desteklemiyor. DNS bölgelerini olabilir [Azure Azure CLI kullanarak DNS içeri](dns-import-export.md). DNS kayıtlarını sonra yönetilebilir aracılığıyla [Azure DNS Yönetim Portalı](dns-operations-recordsets-portal.md), bizim [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [PowerShell cmdlet'leri](dns-operations-recordsets.md), veya [CLI aracı](dns-operations-recordsets-cli.md).

Bu bizim biriktirme listesi üzerinde izlemekte olduğunuz bir özelliktir. Geri bildirim sitemizi kullanabilirsiniz [bu özellik için destek kaydetmek](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>Azure DNS URL yeniden yönlendirmeleri destekliyor mu?

Hayır. URL yeniden yönlendirme hizmetleri gerçekte bir DNS hizmeti olmayan - HTTP, yerine düzeyinde DNS düzeyi çalışırlar. Bir URL gruplanacağını bazı DNS sağlayıcıları hizmet kendi genel teklifinin bir parçası olarak yeniden yönlendir. Bu Azure DNS tarafından şu anda desteklenmiyor.

Bu özellik bizim biriktirme listesi üzerinde izlenir. Geri bildirim sitemizi kullanabilirsiniz [bu özellik için destek kaydetmek](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

## <a name="using-azure-dns"></a>Azure DNS kullanma

### <a name="can-i-co-host-a-domain-using-azure-dns-and-another-dns-provider"></a>Yapabilirim barındırmayı Azure DNS ve başka bir DNS sağlayıcısı kullanarak bir etki alanı?

Evet. Azure DNS diğer DNS Hizmetleri ile birlikte barındırma etki alanlarını destekler.

Bunu yapmak için (hangi sağlayıcıları alma DNS denetleyen sorgular etki alanı için) etki alanı için NS kayıtlarını değiştirmeniz gerekir her iki sağlayıcı ad sunucularını gösterecek şekilde. Bu NS kayıtlarını 3 yerlerde değiştirilmesi gereken: Azure DNS, diğer sağlayıcı ve üst bölge (genellikle etki alanı adı kayıt yapılandırılmış). DNS temsilcisi hakkında daha fazla bilgi için bkz: [DNS etki alanı temsilcisi](dns-domain-delegation.md).

Ayrıca, etki alanı için DNS kayıtlarını hem DNS sağlayıcılar arasında eşit olduğundan emin olmak gerekir. Azure DNS, DNS bölge aktarımları şu anda desteklemiyor. DNS kayıtlarını kullanarak eşitleme yapmanız [Azure DNS Yönetim Portalı](dns-operations-recordsets-portal.md), bizim [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [PowerShell cmdlet'leri](dns-operations-recordsets.md), veya [CLI aracı](dns-operations-recordsets-cli.md).

### <a name="do-i-have-to-delegate-my-domain-to-all-4-azure-dns-name-servers"></a>Etki alanım tüm 4 Azure DNS ad sunucularına temsilci var mı?

Evet. Azure DNS hata Yalıtımı ve artan esneklik için her DNS bölgesinin 4 ad sunucuları atar. Azure DNS SLA nitelemek için tüm 4 ad sunucuları etki alanınıza temsilci gerekir.

### <a name="what-are-the-usage-limits-for-azure-dns"></a>Azure DNS için kullanım sınırları nelerdir?

Azure DNS kullanarak aşağıdaki varsayılan sınırları uygulayın:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>Bir Azure DNS bölgesi kaynak grupları arasında veya abonelikler arasında taşıyabilir miyim?

Evet. DNS bölgelerini kaynak grupları arasında veya abonelikler arasında taşınabilir.

Üzerinde etkisi yoktur DNS sorgularını bir DNS bölgesi taşırken. Bölgeye atanan ad sunucularını aynı kalır ve DNS sorgularını boyunca normal olarak işlenir.

Daha fazla bilgi ve DNS bölgeleri taşımak yönergeler için bkz: [bir yeni kaynak grubu veya abonelik kaynaklarını taşıma](../azure-resource-manager/resource-group-move-resources.md).

### <a name="how-long-does-it-take-for-dns-changes-to-take-effect"></a>Ne kadar süreyle DNS değişikliklerin etkili olması için sürer?

Genellikle yeni DNS bölgeleri ve DNS kayıtlarını Azure DNS ad sunucuları üzerinde hızlı bir şekilde, birkaç saniye içinde yansıtılır.

Var olan DNS kayıtlarının değişiklikler biraz uzun sürebilir, ancak Azure DNS ad sunucuları üzerinde 60 saniye içinde hala yansıtılması. Bu durumda, DNS Azure DNS dışında (DNS istemcileri ve DNS özyinelemeli çözümleyiciler) önbelleğe alma de bir DNS değişikliğin etkili olması için geçen süre etkileyebilir. Bu önbellek süresi, her kayıt kümesinin yaşam süresi (TTL) özelliği kullanılarak denetlenebilir.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>My DNS bölgeleri yanlışlıkla silinmeye karşı nasıl koruyabilirim?

Azure DNS, Azure Resource Manager kullanılarak yönetilir ve avantajları erişim denetimi özellikleri, Azure Resource Manager sağlar. Rol tabanlı erişim denetimi, hangi kullanıcıların okuma veya DNS bölgeleri yazma erişimi denetlemek ve kayıt kümeleri için kullanılabilir. Kaynak kilitleri yanlışlıkla değiştirilmesini veya DNS bölgeleri silinmesini önlemek ve kayıt kümeleri için uygulanabilir.

Daha fazla bilgi için bkz: [koruma DNS bölgeleri ve kayıtları](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>Azure DNS'de SPF kayıtlarının nasıl ayarlayabilirim?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="do-azure-dns-nameservers-resolve-over-ipv6-"></a>Azure DNS Nameservers IPv6 üzerinden çözümleme? 

Evet. Azure DNS Nameservers ikili yığını (hem IPv4 hem de IPv6 adreslerini vardır). DNS bölgenize atanan Azure DNS nameservers IPv6 adresini bulmak için nslookup gibi bir araç kullanabilirsiniz (örneğin, `nslookup -q=aaaa <Azure DNS Nameserver>`).

### <a name="how-do-i-set-up-an-international-domain-name-idn-in-azure-dns"></a>Bir uluslararası etki alanı adı (IDN) ayarlama Azure DNS'ye nasıl ayarlarım?

Uluslararası etki alanı adlarını (IDN'ler) iş her DNS adını kullanarak kodlayarak '[punycode](https://en.wikipedia.org/wiki/Punycode)'. DNS sorguları bu punycode kodlu adları kullanılarak yapılır.

Uluslararası etki alanı adlarını (IDN'ler) Azure DNS'de bölge adı dönüştürerek yapılandırabileceğiniz veya kodlar, zayıf koda kayıt kümesinin adı. Azure DNS denetleyicisinden punycode yerleşik dönüştürme şu anda desteklemiyor.

## <a name="next-steps"></a>Sonraki adımlar

[Azure DNS hakkında daha fazla bilgi edinin](dns-overview.md)
<br>
[Özel etki alanları için Azure DNS kullanma hakkında daha fazla bilgi edinin](private-dns-overview.md)
<br>
[DNS bölgeleri ve kayıtlar hakkında daha fazla bilgi edinin](dns-zones-records.md)
<br>
[Azure DNS ile çalışmaya başlama](dns-getstarted-portal.md)

