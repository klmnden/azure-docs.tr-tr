---
title: Azure DNS hakkında SSS | Microsoft Docs
description: Azure DNS hakkında sık sorulan sorular
services: dns
documentationcenter: na
author: vhorne
manager: jeconnoc
editor: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/06/2017
ms.author: victorh
ms.openlocfilehash: 747b2e2499a9bafcf7a7b03bc2ce144828c55c75
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39172509"
---
# <a name="azure-dns-faq"></a>Azure DNS hakkında SSS

## <a name="about-azure-dns"></a>Azure DNS hakkında

### <a name="what-is-azure-dns"></a>Azure DNS nedir?

DNS veya etki alanı adı sistemi çevirmek için sorumludur (veya çözümleme) IP adresini bir Web sitesi veya hizmet adı. Azure DNS bir barındırma DNS etki alanları için Microsoft Azure altyapısı kullanılarak ad çözümlemesi olanağı sağlayan bir hizmettir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz.

Azure DNS'de DNS etki alanlarını Azure'nın genel DNS ad sunucularını ağda barındırılır. Bu, böylece her DNS sorgusu en yakın kullanılabilir DNS sunucusu tarafından yanıtlanır Anycast ağ kullanır. Azure DNS, hem hızlı performans ve etki alanınız için yüksek kullanılabilirlik sağlar.

Azure DNS hizmeti, Azure Resource Manager üzerindeki temel alır. Bu nedenle, Resource Manager rol tabanlı erişim denetimi, denetim günlüklerini ve kaynak kilitleme gibi özelliklerinden yararlanır. Etki alanları ve kayıtları, Azure portalı, Azure PowerShell cmdlet'leri ve platformlar arası Azure CLI yönetilebilir. Otomatik DNS Yönetimi gerektiren uygulamalar hizmeti SDK'ları ve REST API ile tümleştirilebilir.

### <a name="how-much-does-azure-dns-cost"></a>Azure DNS nin ücreti ne kadardır?

Azure DNS faturalandırma modeli, Azure DNS ve DNS sorgularının aldıkları sayısı barındırılan DNS bölgelerinin sayısı temel alır. Kullanıma göre indirim sağlanır.

Daha fazla bilgi için [Azure DNS fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-the-sla-for-azure-dns"></a>Azure DNS için hani SLA sunulur?

Azure, geçerli DNS isteklerinin yanıt en az bir Azure DNS ad sunucusundan en az % 99,99 oranında zaman alacağını garanti eder.

Daha fazla bilgi için [Azure DNS SLA sayfamızda](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>'DNS bölgesi' nedir? DNS etki alanıyla aynı şey midir? 

Bir etki alanına benzersiz bir ad etki alanı adı sistemi, örneğin "contoso.com" dir.


DNS bölgesi belirli bir etki alanıyla ilgili DNS kayıtlarını barındırmak için kullanılır. Örneğin, 'contoso.com' etki alanı 'mail.contoso.com' (posta sunucusu için) ve 'www.contoso.com' (bir web sitesi için) gibi birden fazla DNS kaydını içerebilir. Bu kayıtları 'contoso.com' DNS bölgesindeki barındırılması.

Bir etki alanı adı *yalnızca bir ad*bilgileriyse bir DNS bölgesi bir etki alanı adı için DNS kayıtlarını içeren bir veri kaynağı. Azure DNS, bir DNS bölgesi barındırmanızı ve Azure'de bir etki alanı için DNS kayıtlarını yönetmenizi sağlar. Ayrıca, Internet'ten DNS sorgularını yanıtlamak için DNS ad sunucularını sağlar.

### <a name="do-i-need-to-purchase-a-dns-domain-name-to-use-azure-dns"></a>Azure DNS’yi kullanmak için DNS etki alanı adı satın almam gerekiyor mu? 

Olmayabilir.

Azure DNS’de bir DNS bölgesi barındırmak için etki alanı satın almanız gerekmez. Bir etki alanı adına sahip olmadan, istediğiniz zaman DNS bölgesi oluşturabilirsiniz. Bu bölge için DNS sorgularını, bunlar bölgesi için atanmış Azure DNS ad sunucularına yönlendirilmesini, yalnızca çözülecektir.

DNS bölgenizi Genel DNS hiyerarşisine bağlamak isterseniz etki alanı adını satın almanız – bağlama yerden DNS sorgularını sağlayan DNS bölgenizi bulmasına ve DNS kayıtlarınızı yanıtlanması dünya.

## <a name="azure-dns-features"></a>Azure DNS özellikleri

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Azure DNS, DNS tabanlı trafik yönlendirme veya uç nokta yük devretme destekliyor mu?

DNS tabanlı trafik yönlendirme ve uç nokta yük devretme, Azure Traffic Manager tarafından sağlanır. Azure Traffic Manager, Azure DNS ile birlikte kullanılan ayrı bir Azure hizmetidir. Daha fazla bilgi için [Traffic Manager'a genel bakış](../traffic-manager/traffic-manager-overview.md).

Azure DNS, yalnızca belirli bir DNS kaydı için her bir DNS sorgusu her zaman aynı DNS yanıtı alır burada 'static' DNS etki alanı, barındırma destekler.

### <a name="does-azure-dns-support-domain-name-registration"></a>Azure DNS, etki alanı adı kayıt destekliyor mu?

Hayır. Azure DNS, etki alanı adları satın alımını şu anda desteklemiyor. Etki alanları satın almak istiyorsanız, bir üçüncü taraf etki alanı adı kayıt şirketi kullanmanız gerekir. Kayıt şirketi, genelde küçük bir yıllık ücret uygular. Etki alanlarını Azure DNS'de DNS kayıtlarını yönetimi için ardından barındırılabilir. Bkz: [bir etki alanını Azure DNS'ye devretme](dns-domain-delegation.md) Ayrıntılar için.

Etki alanı satın alma, Azure biriktirme listesinde izlenmekte olan bir özelliktir. Geri bildirim sitesine kullanabileceğiniz [bu özellik için destek kaydetme](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-dnssec"></a>Azure DNS, DNSSEC destekliyor mu?

Hayır. Azure DNS, DNSSEC şu anda desteklemiyor.

DNSSEC, Azure DNS biriktirme listesi üzerinde izlenmekte olan bir özelliktir. Geri bildirim sitesine kullanabileceğiniz [bu özellik için destek kaydetme](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Azure DNS bölge aktarımı (AXFR/IXFR) destekliyor mu?

Hayır. Azure DNS bölge aktarımlarını şu anda desteklemiyor. DNS bölgeleri olabilir [Azure CLI kullanarak Azure DNS alınan](dns-import-export.md). DNS kayıtlarını ardından yönetilebilir aracılığıyla [Azure DNS Yönetim Portalı](dns-operations-recordsets-portal.md), bizim [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [PowerShell cmdlet'leri](dns-operations-recordsets.md), veya [CLI aracı](dns-operations-recordsets-cli.md).

Bölge aktarımı, Azure DNS biriktirme listesi üzerinde izlenmekte olan bir özelliktir. Geri bildirim sitesine kullanabileceğiniz [bu özellik için destek kaydetme](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>Azure DNS, URL yeniden yönlendirmeleri destekliyor mu?

Hayır. URL yeniden yönlendirme hizmetleri gerçekten bir DNS hizmeti olmayan - HTTP yerine düzeyinde şifreleme, DNS düzeyinde çalışır. Bir URL paket bazı DNS sağlayıcıları, hizmet tekliflerinin genel bir parçası olarak yönlendirin. Bu Azure DNS tarafından şu anda desteklenmiyor.

Yeniden yönlendirme URL'si özelliği, Azure DNS biriktirme listesinde izlenir. Geri bildirim sitesine kullanabileceğiniz [bu özellik için destek kaydetme](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

### <a name="does-azure-dns-support-extended-ascii-encoding-8-bit-set-for-txt-recordset-"></a>Azure DNS TXT kayıt kümesi için (8-bit) kümesi kodlama genişletilmiş ASCII destekliyor mu?

Evet. Azure REST API, SDK'ları, PowerShell ve CLI (2017-10-01 ' daha eski sürümleri veya genişletilmiş ASCII kümesi desteklemeyen SDK 2.1) en son sürümünü kullanıyorsanız, azure DNS TXT kayıt kümeleri için genişletilmiş ASCII kodlama kümesi destekler. Örneğin, kullanıcı bir dize değeri olarak bir TXT kaydı için sağlıyorsa, genişletilmiş ASCII karakter \128 sahip (örn: "abcd\128efgh"), Azure DNS, iç gösteriminde (128'dir) bu karakteri bayt değerini kullanır. DNS çözümlemesi zamanında yanıt olarak de bu bayt değeri döndürülür. Ayrıca çözüm endişe kadar "abc" ve "\097\098\099" birbirinin yerine olduğunu unutmayın. 

Sizinle [RFC 1035](https://www.ietf.org/rfc/rfc1035.txt) bölge dosyası ana biçim kaçış kuralları TXT kayıtları. Örneğin, ' \' gerçekten çıkışları RFC başına her şey şimdi. "A\B" TXT kaydı değer olarak belirtmeniz durumunda temsil edilir ve yalnızca "AB" çözün. Gerçekten TXT kaydını çözünürlükte "A\B" olmasını istiyorsanız, kaçış gerekir "\" yeniden, yani belirtin "A\\B". 

Bu destek şu anda Azure portalından oluşturulan TXT kayıtları için kullanılabilir olmadığını unutmayın. 

## <a name="using-azure-dns"></a>Azure DNS kullanma

### <a name="can-i-co-host-a-domain-using-azure-dns-and-another-dns-provider"></a>Alabilirim ortak sunuculuk Azure DNS ve başka bir DNS Sağlayıcısı'nı kullanarak bir etki alanı?

Evet. Azure DNS, diğer DNS hizmetleriyle ortak bir barındırma etki alanlarını destekler.

Bunu yapmak için etki alanına (hangi denetimin hangi sağlayıcıları Al DNS etki alanı için sorgular) ait NS kayıtlarını değiştirmeniz gerekir hem sağlayıcı adı sunuculara yönlendirin. Bu NS kayıtlarını üç yerde değiştirebilirsiniz: Azure DNS, diğer sağlayıcı ve üst bölge (genellikle etki alanı adı kayıt şirketi aracılığıyla yapılandırılır). DNS temsilcisi hakkında daha fazla bilgi için bkz. [DNS etki alanı temsilcisi](dns-domain-delegation.md).

Ayrıca, etki alanı için DNS kayıtlarını hem DNS sağlayıcıları arasında eşitlenmiş olduğundan emin olmak gerekir. Azure DNS, DNS bölge aktarımlarını şu anda desteklemiyor. DNS kayıtları kullanarak eşitlenmesi gereken [Azure DNS Yönetim Portalı](dns-operations-recordsets-portal.md), [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [PowerShell cmdlet'leri](dns-operations-recordsets.md), veya [CLI aracı](dns-operations-recordsets-cli.md).

### <a name="do-i-have-to-delegate-my-domain-to-all-four-azure-dns-name-servers"></a>Etki alanım tüm dört Azure DNS ad sunucularına temsilci gerekiyor mu?

Evet. Azure DNS, hata Yalıtımı ve artan esneklik için her DNS bölgesinin dört ad sunucusunun atar. Azure DNS SLA hakkı kazanmak için tüm dört ad sunucusunun etki alanını devretme gerekir.

### <a name="what-are-the-usage-limits-for-azure-dns"></a>Azure DNS için kullanım sınırları nelerdir?

Azure DNS kullanırken aşağıdaki varsayılan sınırlar geçerlidir:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>Bir Azure DNS bölgesi kaynak grupları arasında veya abonelikler arasında taşıyabilir miyim?

Evet. DNS bölgeleri, kaynak grupları arasında veya abonelikler arasında taşınabilir.

Hiçbir etkisi yoktur DNS sorgularını DNS bölgesi taşırken. Bölgeye atanan ad sunucularını aynı kalır ve DNS sorguları boyunca normal şekilde işlenir.

Daha fazla bilgi ve DNS bölgelerini taşımak yönergeler için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](../azure-resource-manager/resource-group-move-resources.md).

### <a name="how-long-does-it-take-for-dns-changes-to-take-effect"></a>Ne kadar süreyle DNS değişikliklerin etkili olması için sürer?

Genellikle yeni DNS bölgeleri ve DNS kayıtlarını Azure DNS ad sunucularında hızlı bir şekilde birkaç saniye içinde yansıtılır.

Var olan DNS kayıtlarında yapılan değişiklikler, biraz daha uzun sürebilir, ancak Azure DNS ad sunucularında 60 saniye içinde hala yansıtılması. Bu durumda, (DNS istemcilerini ve DNS özyinelemeli Çözümleyicileri) dışında Azure DNS'yi DNS önbelleğini de bir DNS değişikliğin etkili olması için geçen süreyi etkiler. Bu önbellek süresi, her bir kayıt kümesi, yaşam süresi (TTL) özelliği kullanılarak denetlenebilir.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>My DNS bölgelerini yanlışlıkla silinmesine karşı nasıl koruyabilirim?

Azure DNS, Azure Resource Manager kullanılarak yönetilir ve erişim denetimi avantajlarından, Azure Resource Manager özellikleri sağlar. Rol tabanlı erişim denetimi kaydı kümeleri ve hangi kullanıcıların okuma veya DNS bölgeleri yazma erişimi denetlemek için kullanılabilir. Kaynak kilitleri kaydı kümeleri ve yanlışlıkla değişiklik veya DNS bölgeleri silinmesini önlemek için uygulanabilir.

Daha fazla bilgi için [koruma DNS bölgelerini ve kayıtlarını](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>SPF kayıtlarının Azure DNS'de nasıl ayarlayabilirim?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="do-azure-dns-nameservers-resolve-over-ipv6"></a>IPv6 üzerinden Azure DNS ad çözümleme? 

Evet. Azure DNS ad ikili yığın (IPv4 ve IPv6 adreslerine sahip). Atanan DNS bölgenizi Azure DNS'ye atamak için IPv6 adresini bulmak için nslookup gibi bir araç kullanabilirsiniz (örneğin, `nslookup -q=aaaa <Azure DNS Nameserver>`).

### <a name="how-do-i-set-up-an-international-domain-name-idn-in-azure-dns"></a>Bir uluslararası etki alanı adı (IDN) oluşturan Azure DNS'de nasıl ayarlayabilirim?

Uluslararası etki alanı adlarını (IDN'ler) iş her DNS adını kullanarak kodlama '[punycode](https://en.wikipedia.org/wiki/Punycode)'. DNS sorguları bu punycode kodlu adları kullanılarak yapılır.

Kayıt kümesi adı için zayıf kod veya bölge adı dönüştürerek, Azure DNS'de uluslararası etki alanı adlarını (IDN'ler) yapılandırabilirsiniz. Azure DNS, zayıf kod içine/dışına yerleşik dönüştürme şu anda desteklemiyor.

## <a name="private-dns"></a>Özel DNS

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

### <a name="does-azure-dns-support-private-domains"></a>Azure DNS, 'private' etki alanı destekliyor mu?
'Private' etki alanı için destek, özel bölgeleri özelliğini kullanarak uygulanır.  Bu özellik şu anda genel önizlemede kullanılabilir.  Özel bölgeleriyle aynı araçları kullanarak internet'e yönelik Azure DNS bölgelerini yönetilen, ancak yalnızca, belirtilen sanal ağ dns'sinden.  Bkz: [genel bakış](private-dns-overview.md) Ayrıntılar için.

Şu anda Azure portalında özel bölgeleri desteklenmez. 

Azure'da diğer iç DNS seçenekleri hakkında daha fazla bilgi için bkz: [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="what-is-the-difference-between-registration-virtual-network-and-resolution-virtual-network-in-the-context-of-private-zones"></a>Özel bölgeler bağlamında kayıt sanal ağı ve çözümleme sanal ağı arasındaki fark nedir? 
DNS özel bölgesi iki yolla - kayıt sanal ağı veya çözümleme sanal ağı olarak sanal ağlara bağlayabilirsiniz. Her iki durumda da, sanal ağdaki sanal makinelerin özel bölgedeki kayıtları karşı başarılı bir şekilde çözmek mümkün olacaktır. Bir sanal ağın kayıt sanal ağı belirtirseniz, ancak Azure otomatik olarak (dinamik kayıt) DNS kayıtları bölge sanal ağdaki sanal makineler için içine kaydeder. Ayrıca, bir sanal makine, sanal ağ silindiğinde bir kayıt Azure da otomatik olarak ilgili DNS kaydı bağlı özel bölgeden kaldırır. 

### <a name="will-azure-dns-private-zones-work-across-azure-regions"></a>Azure bölgeleri arasında Azure DNS özel bölgeleri çalışacak mı?
Evet. Özel bölgeler için DNS çözümleme sanal ağları tüm özel bölge için çözümleme sanal ağları olarak belirtilir sürece olmasa bile açıkça sanal ağları eşleme Azure bölgelerindeki sanal ağları arasında desteklenir. Müşteriler, bir bölgeden diğerine Akış TCP/HTTP trafiği için eşlenmiş sanal ağlar gerekebilir. 

### <a name="is-connectivity-to-the-internet-from-virtual-networks-required-for-private-zones"></a>Bağlantı, özel bölgeler için gereken sanal ağlardan Internet'e mi?
Hayır. Özel bölgeler ve sanal makineleri veya diğer kaynaklar içinde ve arasında sanal ağlar için etki alanlarını yönet müşterilerinizin sanal ağlar ile birlikte çalışır. İnternet bağlantısı olmayan ad çözümlemesi için gereklidir. 

### <a name="can-the-same-private-zone-be-used-for-multiple-virtual-networks-for-resolution"></a>Aynı özel bölge çözümlemesi için birden fazla sanal ağ için kullanılabilir mi? 
Evet. Müşteriler, 10 adede kadar çözümleme sanal ağları tek bir özel bölge ile ilişkilendirebilirsiniz.

### <a name="can-a-virtual-network-that-belongs-to-a-different-subscription-be-added-as-a-resolution-virtual-network-to-a-private-zone"></a>Farklı bir aboneliğe ait bir sanal ağ, bir özel bölge için bir çözümleme sanal ağı eklenebilir? 
Yazma işlemi izni hem sanal ağlar, hem de özel DNS bölgesi kullanıcının sahip olduğu sürece Evet. Birden çok RBAC rolleri için yazma izni ayrılabileceği unutmayın. Örneğin, Klasik ağ Katılımcısı RBAC rolü sanal ağlar için yazma izinlerine sahiptir. RBAC rolleri hakkında daha fazla bilgi için bkz. [rol tabanlı erişim denetimi](../role-based-access-control/overview.md)

### <a name="will-the-automatically-registered-virtual-machine-dns-records-in-a-private-zone-be-automatically-deleted-when-the-virtual-machines-are-deleted-by-the-customer"></a>Özel bir bölge içinde otomatik olarak kayıtlı sanal makinenin DNS kayıtlarını otomatik olarak sanal makineler müşteri tarafından silindiğinde silinir?
Evet. Bir sanal makine bir kayıt sanal ağı içinde silerseniz, bu kayıt sanal ağı olması nedeniyle bölge içinde kayıtlı DNS kayıtlarını otomatik olarak sileriz. 

### <a name="can-an-automatically-registered-virtual-machine-record-in-a-private-zone-from-a-registration-virtual-network-be-deleted-manually"></a>(Sanal ağdan bir kayıt) özel bir bölgedeki bir sanal makine otomatik olarak kayıtlı kaydı el ile silinebilir? 
Hayır. Şu anda kayıt sanal ağdan bir özel bölge içinde otomatik olarak kayıtlı sanal makinenin DNS kayıtlarını görünür ve müşteriler tarafından düzenlenebilir değildir. Bununla birlikte, değiştirin (üzerine yaz) gibi otomatik olarak kayıtlı DNS kayıtları ile el ile oluşturulan bir DNS bölgesinde kaydedin. Aşağıdaki soru ve bu adresleri yanıt bakın.

### <a name="what-happens-when-we-attempt-to-manually-create-a-new-dns-record-into-a-private-zone-that-has-the-same-hostname-as-an-automatically-registered-existing-virtual-machine-in-a-registration-virtual-network"></a>El ile kayıt sanal ağı içinde (otomatik olarak kaydedilen) var olan bir sanal makine olarak aynı ana bilgisayar adı olan bir özel bölge içine yeni bir DNS kaydı oluşturma denediğimiz ne olur? 
El ile kayıt sanal ağda aynı ana bilgisayar adı olarak (otomatik olarak kayıtlı) bir sanal makine mevcut olan bir özel bölge içine yeni bir DNS kaydı oluşturma girişimi, otomatik olarak kayıtlı üzerine yeni DNS kaydı izin verir sanal makine kaydı. Ayrıca, daha sonra bu DNS kaydını bölgeden el ile oluşturulan silmeye çalışırsanız, silme başarılı olur ve otomatik kayıt yeniden olacağını (DNS kaydını otomatik olarak bölgede yeniden oluşturulur) sanal makineye kadar uzun hala var olduğundan ve özel IP sahip bağlı. 

### <a name="what-happens-when-we-unlink-a-registration-virtual-network-from-a-private-zone--would-the-automatically-registered-virtual-machine-records-from-the-virtual-network-be-removed-from-the-zone-as-well"></a>Özel bir bölgeye kayıt sanal ağdan biz bağlantısını ne olur? Sanal ağdan sanal makine otomatik olarak kayıtlı kayıtları bölgeden kaldırılması?
Evet. Kayıt sanal ağı (ilişkili kayıt sanal ağı kaldırmak için DNS bölgesini güncelleştirme) özel bir bölgeden bağlantısını, Azure sanal makine otomatik olarak kaydedilen kayıtları bölgesinden kaldırın. 

### <a name="what-happens-when-we-delete-a-registration-or-resolution-virtual-network-that-is-linked-to-a-private-zone--do-we-have-to-manually-update-the-private-zone-to-un-link-the-virtual-network-as-a-registration-or-resolution--virtual-network-from-the-zone"></a>Biz, özel bir bölgeyle bağlantılı kayıt (veya çözüm) sanal ağ sildiğinizde ne olur? El ile Çalıştır-bağlantı için bir kayıt (veya çözüm) sanal ağda sanal ağ özel bölge bölgeden güncelleştirmek zorunda mıyım?
Evet. Kayıt (veya çözümleme gibi) bir sanal ağ sildiğinizde özel bağlantısını olmadan bölgelendirmeniz, Azure, silme işleminin başarılı olması için olanak tanır, ancak sanal ağ, özel bölgeden varsa otomatik olarak bağlantısız değil. El ile özel bölge sanal ağdan bağlantısını kaldırmanız gerekir. Bu nedenle, silmeden önce özel bölgenizi, sanal ağdan ilk bağlantısını için önerilir.

### <a name="would-dns-resolution-using-the-default-fqdn-internalcloudappnet-still-work-even-when-a-private-zone-for-example-contosolocal-is-linked-to-a-virtual-network"></a>Varsayılan kullanarak DNS çözümlemesi FQDN (internal.cloudapp.net) çalışmaya devam özel bölge gerekir (örneğin: contoso.local) bir sanal ağa bağlı? 
Evet. Özel bölgeleri özelliğini kullanarak Azure tarafından sağlanan internal.cloudapp.net bölge için varsayılan DNS çözümleri yerini almaz ve bir ek özellik veya geliştirme olarak sunulur. Her iki durumda için (Azure tarafından sağlanan internal.cloudapp.net veya kendi özel bölge bağlı olup olmadığını) karşı çözümlemek istediğiniz bölgenin FQDN kullanmanız önerilir. 

### <a name="would-the-dns-suffix-on-virtual-machines-within-a-linked-virtual-network-be-changed-to-that-of-the-private-zone"></a>Bağlı sanal ağ içindeki sanal makinelerde DNS soneki, özel bölge değişecek mi? 
Hayır. Şu anda sanal makinelere bağlı sanal ağınızdaki DNS soneki, varsayılan Azure tarafından sağlanan sonek olarak kalır ("*. internal.cloudapp.net"). Ancak el ile bu DNS soneki üzerindeki sanal makinelerinize, özel bölge değiştirebilirsiniz. 

### <a name="are-there-any-limitations-for-private-zones-during-this-preview"></a>Bu önizleme boyunca özel bölgeler için sınırlamalar vardır?
Evet. Genel Önizleme sırasında aşağıdaki sınırlamalar mevcuttur:
* Özel bölge başına 1 kayıt sanal ağları
* Özel bölge başına en fazla 10 çözümleme sanal ağları
* Belirli bir sanal ağın yalnızca bir özel bölge için bir kayıt sanal ağı bağlanabilir
* Belirli bir sanal ağ en fazla 10 özel bölgeler için bir çözümleme sanal ağı bağlanabilir.
* Kayıt sanal ağı belirtilmediği takdirde, özel bölge için kaydedilen Vm'lerden söz konusu sanal ağ için DNS kayıtlarını görüntülenebilir veya Powershell/CLI/API'dan alınabilir olmaz, ancak VM kayıtları gerçekten kaydedilir ve çözümler başarıyla
* Ters DNS kayıt sanal ağ özel IP alanı için yalnızca çalışır
* Ters DNS özel bölgesi içinde kayıtlı değil özel IP için (örneğin: özel bir bölgeye çözümleme sanal ağı olarak bağlı bir sanal ağdaki bir sanal makine için özel IP) "internal.cloudapp.net" DNS sonek olarak, ancak döndürür Bu sonekin çözümlenebilir olmayacaktır.   
* Sanal ağ boş olması gerekir (yani hiçbir sanal makine bağlı bir NIC ile) ilk defa oluşturulurken (yani ilk kez) bir özel bölge kayıt veya çözümleme sanal ağı bağlama. Ancak, sanal ağ boş ardından olabilir bir kayıt veya çözümleme sanal ağ özel diğer bölgelere gelecekteki bağlama. 
* Şu anda koşullu iletme, örneğin Azure ve şirket içi ağlar arasındaki çözümleme etkinleştirmek için desteklenmiyor. Müşteriler, bu senaryo başka mekanizmalar aracılığıyla nasıl hayata geçirebilirsiniz ile ilgili belgeler için lütfen bkz [VM'ler ve rol örnekleri için ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)

### <a name="are-there-any-quotas-or-limits-on-zones-or-records-for-private-zones"></a>Tüm kota veya limitlerinizi bölgeleri veya özel bölgeler için kayıtları var mı?
Abonelik başına izin verilen bölge sayısı veya özel bölgeleri için bölge başına kayıt kümelerinin sayısı ayrı sınırı yoktur. Genel ve özel bölgeleri belgelendiği gibi genel DNS sınırları doğru sayısı [burada](../azure-subscription-service-limits.md#dns-limits)

### <a name="is-there-portal-support-for-private-zones"></a>Özel bölgeler için Portal destek var mı?
Önceden Portal olmayan mekanizmaları (API/PowerShell/CLI/SDK'lar) oluşturulan bir özel bölgeleri Azure portalında görünmez, ancak müşterilerin yeni özel bölgeler oluşturmak veya sanal ağları ilişkilerini yönetmek mümkün olur. Ayrıca, kayıt sanal ağları ilişkili sanal ağlar için otomatik olarak kayıtlı VM kayıt Portalı'ndan görünür olmaz. 

## <a name="next-steps"></a>Sonraki adımlar

[Azure DNS hakkında daha fazla bilgi edinin](dns-overview.md)
<br>
[Azure DNS özel etki alanları için kullanma hakkında daha fazla bilgi edinin](private-dns-overview.md)
<br>
[DNS bölgeleri ve kayıtları hakkında daha fazla bilgi edinin](dns-zones-records.md)
<br>
[Azure DNS ile çalışmaya başlama](dns-getstarted-portal.md)

