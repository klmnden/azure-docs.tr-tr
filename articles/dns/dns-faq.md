---
title: Azure DNS SSS | Microsoft Docs
description: Azure DNS hakkında sık sorulan sorular
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/06/2017
ms.author: kumud
ms.openlocfilehash: e0eb39ced1d88d2e0b6128493304f112f9c685fa
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="azure-dns-faq"></a>Azure DNS hakkında SSS

## <a name="about-azure-dns"></a>Azure DNS hakkında

### <a name="what-is-azure-dns"></a>Azure DNS nedir?

Etki alanı adı sistemi ya da DNS, çevirmek için sorumludur (veya çözme) bir Web sitesi ya da hizmet adı IP adresine. Azure DNS barındırma için bir DNS etki alanı, Microsoft Azure altyapısı kullanılarak ad çözümlemesi sağlamanın hizmetidir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz.

Azure DNS'de DNS etki alanı, Azure'nın genel DNS ad sunucuları ağda barındırılır. Bu, böylece her DNS sorgusu en yakın kullanılabilir DNS sunucusu tarafından yanıtlanan her noktaya yayın ağ kullanır. Azure DNS, etki alanınız için yüksek kullanılabilirlik ve hızlı performans sağlar.

Azure DNS hizmeti, Azure Resource Manager temel alır. Bu nedenle, rol tabanlı erişim denetimi, denetim günlüklerini ve kaynak kilitleme gibi Resource Manager özelliklerinden yararlanır. Etki alanları ve kayıtları Azure portalı, Azure PowerShell cmdlet'leri ve platformlar arası Azure CLI yönetilebilir. Otomatik DNS Yönetimi gerektiren uygulamalar, REST API ve SDK aracılığıyla hizmeti ile tümleştirebilirsiniz.

### <a name="how-much-does-azure-dns-cost"></a>Nasıl Azure DNS maliyeti nedir?

Azure DNS fatura modelini Azure DNS ve DNS sorgularının aldıkları sayısı barındırılan DNS bölgeleri sayısını temel alır. İndirim kullanımına dayalı sağlanır.

Daha fazla bilgi için bkz: [Azure fiyatlandırma sayfası DNS](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-the-sla-for-azure-dns"></a>Azure DNS için hani SLA sunulur?

Azure geçerli DNS isteklerine yanıt en az bir Azure DNS ad sunucusundan en az süre % 99,99 almalarını güvence altına alır.

Daha fazla bilgi için bkz: [Azure DNS SLA sayfa](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>'DNS bölgesini' nedir? DNS etki alanıyla aynı şey midir? 

Bir etki alanında benzersiz bir ad etki alanı adı sistemi, örneğin "contoso.com" dir.


DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Örneğin, "contoso.com" etki alanı 'mail.contoso.com"(bir posta sunucusu) ve 'www.contoso.com' (bir web sitesi) gibi birçok DNS kayıtlarını içerebilir. Bu kayıtları 'contoso.com' DNS bölgesindeki barındırılması.

Bir etki alanı adı *yalnızca bir adı*, buna karşın bir DNS bölgesi bir etki alanı adı için DNS kayıtlarını içeren bir veri kaynağı. Azure DNS, bir DNS bölgesi barındırmanızı ve Azure'de bir etki alanı için DNS kayıtlarını yönetmenizi sağlar. Ayrıca, Internet'ten DNS sorgularını yanıtlamak için DNS ad sunucuları sağlar.

### <a name="do-i-need-to-purchase-a-dns-domain-name-to-use-azure-dns"></a>Azure DNS’yi kullanmak için DNS etki alanı adı satın almam gerekiyor mu? 

Olmayabilir.

Azure DNS’de bir DNS bölgesi barındırmak için etki alanı satın almanız gerekmez. Bir etki alanı adına sahip olmadan, istediğiniz zaman DNS bölgesi oluşturabilirsiniz. Bu bölge için DNS sorgularını bölgeye atanan Azure DNS ad sunucularını yönlendirilir, yalnızca çözer.

DNS bölgenizi Genel DNS hiyerarşiye bağlamak istiyorsanız etki alanı adı satın almanız – Bu bağlama yerden DNS sorgularını sağlayan DNS bölgenizi bulmak ve DNS kayıtlarınızı yanıtlanması dünyada.

## <a name="azure-dns-features"></a>Azure DNS özellikleri

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Azure DNS, DNS tabanlı trafik yönlendirme veya uç nokta yük devretme destekliyor mu?

DNS tabanlı trafik yönlendirme ve uç nokta yük devretme Azure trafik Yöneticisi tarafından sağlanır. Azure trafik Yöneticisi Azure DNS ile birlikte kullanılan ayrı bir Azure hizmetidir. Daha fazla bilgi için bkz: [trafik Yöneticisi'ne genel bakış](../traffic-manager/traffic-manager-overview.md).

Azure DNS, yalnızca her bir DNS sorgusu belirli bir DNS kaydı için her zaman aynı DNS yanıtını aldıktan burada 'static' DNS etki alanı, barındırma destekler.

### <a name="does-azure-dns-support-domain-name-registration"></a>Azure DNS etki alanı adı kayıt destekliyor mu?

Hayır. Azure DNS, etki alanı adlarını satın alma şu anda desteklemiyor. Etki alanı satın almak istiyorsanız, bir üçüncü taraf etki alanı adı kayıt kullanmanız gerekir. Kayıt, genellikle küçük bir yıllık ücret ister. Etki alanları için DNS kayıtlarını Yönetimi sonra Azure DNS'de barındırılabilir. Bkz: [bir etki alanını Azure DNS'ye devretme](dns-domain-delegation.md) Ayrıntılar için.

Etki alanı satın alma Azure biriktirme listesinde izlenmekte olan bir özelliktir. Geri bildirim sitesi kullanabileceğiniz [bu özellik için destek kaydetmek](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-dnssec"></a>Azure DNS DNSSEC destekliyor mu?

Hayır. Azure DNS DNSSEC şu anda desteklemiyor.

DNSSEC Azure DNS biriktirme listesi üzerinde izlenmekte olan bir özelliktir. Geri bildirim sitesi kullanabileceğiniz [bu özellik için destek kaydetmek](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Azure DNS bölge aktarımları (AXFR/IXFR) destekliyor mu?

Hayır. Azure DNS bölge aktarımlarının şu anda desteklemiyor. DNS bölgelerini olabilir [Azure Azure CLI kullanarak DNS içeri](dns-import-export.md). DNS kayıtlarını sonra yönetilebilir aracılığıyla [Azure DNS Yönetim Portalı](dns-operations-recordsets-portal.md), bizim [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [PowerShell cmdlet'leri](dns-operations-recordsets.md), veya [CLI aracı](dns-operations-recordsets-cli.md).

Bölge aktarımı Azure DNS biriktirme listesi üzerinde izlenmekte olan bir özelliktir. Geri bildirim sitesi kullanabileceğiniz [bu özellik için destek kaydetmek](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>Azure DNS URL yeniden yönlendirmeleri destekliyor mu?

Hayır. URL yeniden yönlendirme hizmetleri gerçekte bir DNS hizmeti olmayan - HTTP, yerine düzeyinde DNS düzeyi çalışırlar. Bir URL gruplanacağını bazı DNS sağlayıcıları hizmet kendi genel teklifinin bir parçası olarak yeniden yönlendir. Bu Azure DNS tarafından şu anda desteklenmiyor.

URL yeniden yönlendirme özelliğini Azure DNS biriktirme listesi üzerinde izlenir. Geri bildirim sitesi kullanabileceğiniz [bu özellik için destek kaydetmek](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

### <a name="does-azure-dns-support-extended-ascii-encoding-8-bit-set-for-txt-recordset-"></a>Azure DNS TXT kayıt kümesi için (8-bit) kümesi kodlaması genişletilmiş ASCII destekliyor mu?

Evet. Azure REST API'leri, SDK'ları, PowerShell ve CLI (2017-10-01'den daha eski sürümleri veya genişletilmiş ASCII kümesi desteklemiyor SDK 2.1 yapın) en son sürümünü kullanıyorsanız, azure DNS TXT kayıt kümeleri için genişletilmiş ASCII kodlama kümesini destekler. Örneğin, kullanıcı bir dize değeri olarak bir TXT kaydı sağlarsa, genişletilmiş ASCII karakter \128 sahip (örn: "abcd\128efgh"), Azure DNS iç gösterimi (128'dir) bu karakteri bayt değeri kullanır. DNS çözümlemesi aynı anda yanıt olarak da bu bayt değeri döndürülür. Ayrıca çözümleme kaygı kadar "abc" ve "\097\098\099" birbirinin yerine olduğunu unutmayın. 

Biz izleyin [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt) bölge TXT kayıtları için dosya asıl biçimini kaçış kuralları. Örneğin, ' \' gerçekte çıkışları RFC başına her şeyi şimdi. TXT kaydı değeri olarak "A\B" belirtirseniz, temsil edilir ve yalnızca "AB" çözümleyin. Gerçekten TXT kaydı çözünürlükte "A\B" olmasını istiyorsanız, atlamanız gerekir "\" yeniden, yani olarak belirtin "A\\B". 

Bu destek şu anda Azure Portalı'ndan oluşturulan TXT kayıt için kullanılabilir olmadığını unutmayın. 

## <a name="using-azure-dns"></a>Azure DNS kullanma

### <a name="can-i-co-host-a-domain-using-azure-dns-and-another-dns-provider"></a>Yapabilirim barındırmayı Azure DNS ve başka bir DNS sağlayıcısı kullanarak bir etki alanı?

Evet. Azure DNS diğer DNS Hizmetleri ile birlikte barındırma etki alanlarını destekler.

Bunu yapmak için (hangi sağlayıcıları alma DNS denetleyen sorgular etki alanı için) etki alanı için NS kayıtlarını değiştirmeniz gerekir her iki sağlayıcı ad sunucularını gösterecek şekilde. Bu NS kayıtlarını üç yerde değiştirebilirsiniz: Azure DNS, diğer sağlayıcı ve üst bölge (genellikle etki alanı adı kayıt yapılandırılmış). DNS temsilcisi hakkında daha fazla bilgi için bkz: [DNS etki alanı temsilcisi](dns-domain-delegation.md).

Ayrıca, etki alanı için DNS kayıtlarını hem DNS sağlayıcılar arasında eşit olduğundan emin olmak gerekir. Azure DNS, DNS bölge aktarımları şu anda desteklemiyor. DNS kayıtlarını kullanarak eşitlenmesi gereken [Azure DNS Yönetim Portalı](dns-operations-recordsets-portal.md), [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [PowerShell cmdlet'leri](dns-operations-recordsets.md), veya [CLI aracı](dns-operations-recordsets-cli.md).

### <a name="do-i-have-to-delegate-my-domain-to-all-four-azure-dns-name-servers"></a>Etki alanım tüm dört Azure DNS ad sunucularına temsilci var mı?

Evet. Azure DNS hata Yalıtımı ve artan esneklik için her DNS bölgesinin dört ad sunucuları atar. Azure DNS SLA nitelemek için tüm dört ad sunucuları etki alanınıza temsilci gerekir.

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

### <a name="do-azure-dns-nameservers-resolve-over-ipv6"></a>Azure DNS Nameservers IPv6 üzerinden çözümleme? 

Evet. Azure DNS Nameservers ikili yığını (hem IPv4 hem de IPv6 adreslerini vardır). DNS bölgenize atanan Azure DNS nameservers IPv6 adresini bulmak için nslookup gibi bir araç kullanabilirsiniz (örneğin, `nslookup -q=aaaa <Azure DNS Nameserver>`).

### <a name="how-do-i-set-up-an-international-domain-name-idn-in-azure-dns"></a>Bir uluslararası etki alanı adı (IDN) ayarlama Azure DNS'ye nasıl ayarlarım?

Uluslararası etki alanı adlarını (IDN'ler) iş her DNS adını kullanarak kodlayarak '[punycode](https://en.wikipedia.org/wiki/Punycode)'. DNS sorguları bu punycode kodlu adları kullanılarak yapılır.

Uluslararası etki alanı adlarını (IDN'ler) Azure DNS'de bölge adı dönüştürerek yapılandırabileceğiniz veya kodlar, zayıf koda kayıt kümesinin adı. Azure DNS denetleyicisinden punycode yerleşik dönüştürme şu anda desteklemiyor.

## <a name="private-dns"></a>Özel DNS

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

### <a name="does-azure-dns-support-private-domains"></a>Azure DNS 'özel' etki alanları destekliyor mu?
'Özel' etki alanı için destek özel bölgeler özelliği kullanılarak gerçekleştirilir.  Bu özellik şu anda genel Önizleme'de kullanılabilir.  Yalnızca, belirtilen sanal ağ dns'sinden olan ancak özel bölgeler aynı araçlarını internet'e yönelik Azure DNS bölgeleri kullanılarak yönetilir.  Bkz: [genel bakış](private-dns-overview.md) Ayrıntılar için.

Şu anda Azure Portal'da özel bölgeleri desteklenmiyor. 

Azure'da diğer iç DNS seçenekleri hakkında daha fazla bilgi için bkz: [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="what-is-the-difference-between-registration-virtual-network-and-resolution-virtual-network-in-the-context-of-private-zones"></a>Özel bölgeler bağlamında kaydı sanal ağ ve çözümleme sanal ağ arasındaki fark nedir? 
Bir DNS özel bölgesi iki yolla - kayıt sanal ağ veya çözümleme sanal ağ olarak sanal ağlara bağlayabilirsiniz. Her iki durumda da, sanal ağdaki sanal makinelerden özel bölgedeki kayıtları karşı başarıyla çözümleyebilmesi olacaktır. Bir kayıt sanal ağ olarak bir sanal ağ belirtirseniz, ancak Azure otomatik olarak (dinamik kayıt) DNS kayıtları bölge sanal ağdaki sanal makineler için içine kaydeder. Ayrıca, bir sanal makine, sanal ağ silinir kayıt Azure da otomatik olarak ilgili DNS kaydı bağlantılı özel bölgesi'nden kaldırır. 

### <a name="will-azure-dns-private-zones-work-across-azure-regions"></a>Azure DNS özel bölgeler arasında Azure bölgeleri çalışacak mı?
Evet. Özel bölgeler, sanal ağlar tüm özel bölgenin çözümleme sanal ağlar olarak belirtilen sürece olmasa bile sanal ağlar, açıkça eşliği Azure bölgeler arasında sanal ağlar arasında DNS çözümlemesi için desteklenir. Müşteriler, sanal ağlar bir bölgesinden diğerine Akış TCP/HTTP trafiği için eşlenen gerekebilir. 

### <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>Bağlantı Internet'e özel bölgeler için gereken sanal ağlardan mi?
Hayır. Özel bölgeler sanal ağlar ile birlikte çalışır ve müşterilerin sanal makineleri veya diğer kaynakları içinde ve sanal ağlar arasında etki alanlarını yönetme. İnternet bağlantısı ad çözümlemesi için gereklidir. 

### <a name="can-the-same-private-zone-be-used-for-multiple-virtual-networks-for-resolution"></a>Aynı özel bölge çözümlemesi için birden çok sanal ağ için kullanılabilir mi? 
Evet. Müşteriler en fazla 10 çözümleme sanal ağlar tek bir özel bölge ile ilişkilendirebilirsiniz.

### <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-added-as-a-resolution-virtual-network-to-a-private-zone"></a>Farklı bir aboneliğe ait bir sanal ağ özel bölgesine bir çözümleme sanal ağ olarak eklenebilir mi? 
Evet, hem sanal ağlar, hem de özel DNS bölgesi izninin yazma işlemi kullanıcının sahip olduğu sürece. Yazma izni için birden fazla RBAC rolü ayrılabilir unutmayın. Örneğin, Klasik ağ Katılımcısı RBAC rolü sanal ağlar için yazma izinlerine sahiptir. RBAC rolleri hakkında daha fazla bilgi için bkz: [rol tabanlı erişim denetimi](../role-based-access-control/overview.md)

### <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-the-virtual-machines-are-deleted-by-the-customer"></a>Sanal makineler müşteri tarafından silindiğinde özel bir bölgedeki otomatik olarak kayıtlı sanal makine DNS kayıtları otomatik olarak silinir mi?
Evet. Bir sanal makine kaydı sanal ağ içinde silerseniz, biz bu kaydı sanal ağ olması nedeniyle bölge içine kayıtlı olan DNS kayıtları otomatik olarak silecektir. 

### <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-registration-virtual-network-be-deleted-manually"></a>Bir özel bölgeden (kayıt bir sanal ağ) otomatik olarak kayıtlı sanal makinenin kaydında el ile silinebilir mi? 
Hayır. Şu anda kayıt sanal ağdan özel bir bölgedeki otomatik olarak kaydedilmiş sanal makine DNS kayıtlarını görünür veya müşteriler tarafından düzenlenebilir değildir. Ancak, değiştirin (üzerine yaz) gibi otomatik olarak kayıtlı DNS kayıtlarını el ile oluşturulan bir DNS bölgesinde kaydedin. Aşağıdaki soru ve bu adresleri yanıt bakın.

### <a name="what-happens-when-we-attempt-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-registration-virtual-network"></a>Biz el ile olarak (otomatik olarak kayıtlı) olan bir sanal makineyi aynı ana bilgisayar kaydı sanal bir ağa sahip özel bir bölge içinde yeni bir DNS kaydı oluşturma girişimi ne olur? 
El ile varolan (otomatik olarak kayıtlı) bir sanal makine olarak aynı ana bilgisayar kaydı sanal bir ağa sahip özel bir bölge içinde yeni bir DNS kaydı oluşturmak denerseniz, otomatik olarak kayıtlı üzerine yazmak yeni DNS kaydı izin verir sanal makine kaydı. Ayrıca, daha sonra bu DNS kaydını bölgeden el ile oluşturulan silmeyi denerseniz, silme işlemi başarılı olur ve otomatik kayıt yeniden olacağını (DNS kaydını otomatik olarak bölgede yeniden oluşturulur) sanal makine kadar uzun hala var olduğundan ve özel IP sahip bağlı. 

### <a name="what-happens-when-we-unlink-a-registration-virtual-network-from-a-private-zone--would-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-as-well"></a>Size özel bölgesinden kaydı sanal ağ bağlantısını kaldırdığınızda ne olur? Sanal ağı otomatik olarak kayıtlı sanal makine kayıtlarından bölgeden kaldırılırlar?
Evet. Özel bir bölgeden kaydı sanal ağ (güncelleştirme ilişkili kaydı sanal ağı kaldırmak için DNS bölgesi) bağlantısını kaldırmak, Azure herhangi bir otomatik olarak kayıtlı sanal makine kayıt bölgesinden kaldırın. 

### <a name="what-happens-when-we-delete-a-registration-or-resolution-virtual-network-that-is-linked-to-a-private-zone--do-we-have-to-manually-update-the-private-zone-to-un-link-the-virtual-network-as-a-registration-or-resolution--virtual-network-from-the-zone"></a>Size özel bir bölgeye bağlı bir kayıt (ya da çözümleme) sanal ağ sildiğinizde ne olur? Özel bölge kaydını bağlantı için bir kayıt (ya da çözümleme) olarak sanal ağ sanal ağ bölgeden el ile güncelleştirmeniz var mı?
Evet. Kayıt (ya da çözümleme) bir sanal ağ sildiğinizde özel kaynağınızın olmadan önce bölge, Azure başarılı olması için silme işlemi sağlar, ancak sanal ağ özel bölgeden varsa otomatik olarak bağlantısız değil. El ile özel bölge sanal ağdan bağlantısını gerekir. Bu nedenle, sanal özel bölgenizi ağınızdan silmeden önce ilk bağlantısını tavsiye edilir.

### <a name="would-dns-resolution-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-contosolocal-is-linked-to-a-virtual-network"></a>Varsayılan kullanarak DNS çözümlemesi FQDN (internal.cloudapp.net) çalışmaya devam bile özel bölge gerekir (örneğin: contoso.local) bir sanal ağa bağlı? 
Evet. Özel bölgeler Özelliği Azure tarafından sağlanan internal.cloudapp.net bölge kullanarak varsayılan DNS çözümlemeler değiştirmez ve bir ek özellik veya geliştirme olarak sunulur. Her iki durumda için (Azure tarafından sağlanan internal.cloudapp.net veya kendi özel bölge bağlı olup olmadığını) karşı çözümlemek istediğiniz bölgeyi FQDN kullanmanız önerilir. 

### <a name="would-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>Bağlı bir sanal ağ içindeki sanal makinelerde DNS soneki, özel bölge değişen? 
Hayır. Şu anda bağlı sanal ağınızda sanal makinelerde DNS soneki varsayılan Azure tarafından sağlanan sonek olarak kalacak ("*. internal.cloudapp.net"). Ancak el ile bu DNS soneki, sanal makinelerde, özel bölge değiştirebilirsiniz. 

### <a name="are-there-any-limitations-for-private-zones-during-this-preview"></a>Bu önizleme sırasında özel bölgeler için herhangi bir sınırlama var mı?
Evet. Genel Önizleme sırasında aşağıdaki sınırlamalar bulunmaktadır:
* Özel bölge başına 1 kaydı sanal ağlar
* Özel bölge başına fazla 10 çözümleme sanal ağlar
* Belirli bir sanal ağdaki tek bir özel bölgesine bir kayıt sanal ağ olarak bağlanabilir
* Belirli bir sanal ağ en fazla 10 özel bölgeler için bir çözüm sanal ağ olarak bağlanabilir.
* Kayıt sanal ağ belirtilirse, bu sanal ağdan özel bölgesine kayıtlı olan VM'ler için DNS kayıtlarını görüntülenebilir veya Powershell/CLI/API'leri gelen alınabilir olmaz, ancak VM kayıtları gerçekten kaydedilir ve çözümler başarıyla
* Geriye doğru DNS kaydı sanal ağında özel IP alanı için yalnızca çalışır
* Özel bölgesinde kayıtlı olmayan bir özel IP için DNS geriye doğru (örneğin: bir sanal makine olarak özel bir bölge çözümleme sanal ağa bağlı bir sanal ağ için özel IP) "internal.cloudapp.net" ancak DNS son eki olarak döndürür Bu son eki çözümlenebilen olmaz.   
* Sanal ağ boş olması gerekir (yani hiçbir sanal makine bağlı bir NIC ile) ilk defa (yani ilk kez) özel bölgeye kayıt ya da çözümleme sanal ağı bağlama. Ancak, sanal ağ sonra boş olabilir kayıt ya da çözümleme sanal ağı olarak, diğer özel bölgeler için gelecekteki bağlama. 
* Şu anda koşullu iletme, örneğin Azure ve OnPrem ağları arasında çözümleme etkinleştirmek için desteklenmiyor. Müşteriler bu senaryo başka mekanizmalar aracılığıyla nasıl sağlarsınız hakkında daha fazla belgeleri için lütfen bkz [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)

### <a name="are-there-any-quotas-or-limits-on-zones-or-records-for-private-zones"></a>Herhangi bir kotaları veya sınırları bölgeleri veya özel bölgeler için kayıtları vardır?
Abonelik izin verilen bölge sayısı veya özel bölgeleri için bölge başına kayıt kümesi sayısı ayrı sınır yoktur. Ortak ve özel bölgeler belirtildiği gibi genel DNS sınırları doğru sayısı [burada](../azure-subscription-service-limits.md#dns-limits)

### <a name="is-there-portal-support-for-private-zones"></a>Özel bölgeler için Portal destek var mı?
Portal olmayan mekanizmaları (API/PowerShell/CLI/SDK) önceden oluşturulan özel bölgeler Azure Portalı'nda görünmez, ancak müşterilerin yeni özel bölgeler oluşturmak ve sanal ağlar ilişkilerini yönetmek mümkün olur. Ayrıca, kaydı sanal ağları olarak ilişkili sanal ağlar için otomatik olarak kayıtlı VM kayıt Portalı'ndan görünür olmaz. 

## <a name="next-steps"></a>Sonraki adımlar

[Azure DNS hakkında daha fazla bilgi edinin](dns-overview.md)
<br>
[Özel etki alanları için Azure DNS kullanma hakkında daha fazla bilgi edinin](private-dns-overview.md)
<br>
[DNS bölgeleri ve kayıtlar hakkında daha fazla bilgi edinin](dns-zones-records.md)
<br>
[Azure DNS ile çalışmaya başlama](dns-getstarted-portal.md)

