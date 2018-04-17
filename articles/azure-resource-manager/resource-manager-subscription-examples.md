---
title: Senaryolar ve örnekler için abonelik idare | Microsoft Docs
description: Yaygın senaryolar için Azure aboneliği idare uygulamak örnekler sağlar.
services: azure-resource-manager
documentationcenter: na
author: rdendtler
manager: timlt
editor: tysonn
ms.assetid: e8fbeeb8-d7a1-48af-804f-6fe1a6024bcb
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: rodend;karlku;tomfitz
ms.openlocfilehash: 6bd4e9f6bbc5bba73b2c169b7f3c5931f30029e6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Azure enterprise iskele uygulamanın örnekleri
Bu konu kuruluş için önerilerin nasıl uygulayabilirsiniz örnekler sağlayan bir [Azure enterprise iskele](resource-manager-subscription-governance.md). Contoso adlı kurgusal bir şirket yaygın senaryolar için en iyi yöntemleri göstermek için kullanır.

## <a name="background"></a>Arka plan
Contoso, paketlenmiş bir modele "Hizmet olarak yazılım" modelden her şeyi müşteriler için tedarik zinciri çözümleri sağlayan dünya çapında şirket içi dağıtılan ' dir.  Bunlar, Hindistan, ABD ve Kanada önemli geliştirme merkezlerine dünya çapında yazılım geliştirin.

Şirket ISV kısmı önemli bir işletmede ürünleri Yönet birkaç bağımsız iş birimleri ayrılmıştır. Her iş birimi kendi geliştiriciler, ürün yöneticileri ve mimarları sahiptir.

Madde Kurumsal teknoloji Hizmetleri (işaretleri) departmanı merkezi BT yeteneği sağlar ve Departmanlar uygulamalarını ana bilgisayar birkaç veri merkezleri yönetir. Veri merkezleri yönetme, birlikte madde işaretleri kuruluş sağlar ve merkezi işbirliği (örneğin, e-posta ve Web siteleri) ve ağ/telefon hizmetleri yönetir. Bunlar ayrıca müşterilerle iş yükleri işletimsel personel sahip olmayan küçük iş birimleri için yönetin.

Bu konuda aşağıdaki kişiler kullanılır:

* Dave madde işaretleri Azure yöneticisidir.
* Alice'i Contoso geliştirme Director tedarik zinciri departmandaki ' dir.

Bir iş kolu satır uygulama ve müşteri yönelik uygulama oluşturmak contoso gerekir. Azure üzerinde uygulamaları çalıştırmak karar vermiştir. Dave okur [Düzenleyici abonelik idare](resource-manager-subscription-governance.md) konusuna ve artık önerileri uygulamak hazırdır.

## <a name="scenario-1-line-of-business-application"></a>Senaryo 1: İş kolu satır uygulama
Contoso kaynak kodu yönetimi sistemi dünya genelindeki geliştiriciler tarafından kullanılacak (BitBucket) oluşturuyor.  Uygulama barındırma için hizmet (Iaas) olarak altyapısı kullanır ve web sunucuları ve bir veritabanı sunucusu oluşur. Geliştiriciler kendi geliştirme ortamlarında sunuculara erişmek, ancak azure'da sunucularına erişimi gerekmez. Contoso madde işaretleri uygulama sahibi ve ekibi uygulamasını yönetmek izin istiyor. Uygulama yalnızca Contoso şirket ağında sırasında kullanılabilir. Dave abonelik bu uygulama için ayarlamanız gerekir. Abonelik de gelecekte diğer Geliştirici ile ilgili yazılım barındırır.  

### <a name="naming-standards--resource-groups"></a>& Kaynak grupları adlandırma standartları
Dave tüm iş birimleri arasında ortak olan geliştirici araçları desteklemek için bir abonelik oluşturur. Abonelik ve kaynak grupları (uygulama ve ağları) için anlamlı adlar oluşturmak gerekir. Şu abonelik ve kaynak grupları oluşturur:

| Öğe | Ad | Açıklama |
| --- | --- | --- |
| Abonelik |Contoso madde işaretleri DeveloperTools üretim |Ortak geliştirici araçları destekler |
| Kaynak Grubu |bitbucket üretim rg |Uygulama web sunucusu ve veritabanı sunucusu içerir |
| Kaynak Grubu |corenetworks üretim rg |Sanal ağlar ve siteden siteye ağ geçidi bağlantısı içerir |

### <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi
Bu abonelik oluşturduktan sonra Dave uygun ekipleri ve uygulama sahipleri kaynaklarını erişebildiğinden emin olmak istiyor. Dave her takım farklı gereksinimlere sahip olduğunu algılar. Kendisine Contoso'nun şirket içi Active Directory (AD) Azure Active Directory ile eşitlenmiş grupları kullanır ve sağ takımlar için erişim düzeyi sağlar.

Aşağıdaki roller için abonelik Dave atar:

| Rol | Atanan: | Açıklama |
| --- | --- | --- |
| [Sahibi](../role-based-access-control/built-in-roles.md#owner) |Contoso'nun kimliği yönetilen AD |Bu kimliği ile sadece anında (JIT) erişim Contoso'nun kimlik yönetimi aracı üzerinden denetlenir ve abonelik sahibi erişim tam olarak denetlenir sağlar |
| [Güvenlik Yöneticisi](../role-based-access-control/built-in-roles.md#security-manager) |Güvenliği ve risk yönetimi bölümü |Bu rolü kullanıcılarının Azure Güvenlik Merkezi ve kaynakların durumunu bakın sağlar |
| [Ağ Katılımcısı](../role-based-access-control/built-in-roles.md#network-contributor) |Ağ ekibi |Bu rolü Contoso'nun ağ takım siteden siteye VPN ve sanal ağlar yönetmenizi sağlar |
| *Özel rol* |Uygulama sahibi |Dave kaynakların kaynak grubunda değiştirme olanağı veren bir rolü oluşturur. Daha fazla bilgi için bkz: [Azure rbac'de özel roller](../role-based-access-control/custom-roles.md) |

### <a name="policies"></a>İlkeler
Dave abonelik kaynaklarını yönetmek için aşağıdaki gereksinimlere sahiptir:

* Geliştirme araçları dünya genelindeki geliştiriciler desteklediğinden, kendisine herhangi bir bölgede kaynakları oluşturma kullanıcıların engellemek istememektedir. Ancak, kendisinin kaynakları oluşturulduğu bilmesi gerekir.
* Müşterinizle maliyetleri ile ilgili. Bu nedenle, uygulama sahipleri gereksiz yere pahalı sanal makinelerin oluşturulmasını önlemek istediği.  
* Geliştiriciler birçok iş birimleri bu uygulamaya hizmet olduğundan iş birimi ve uygulama sahip her bir kaynağın etiketlemek istediği. Bu etiketler kullanarak madde işaretleri uygun takımlar faturalandıracaktır.

Aşağıdaki oluşturur [Azure ilkeleri](../azure-policy/azure-policy-introduction.md):

| Alan | Etki | Açıklama |
| --- | --- | --- |
| location |Denetim |Denetim tüm bölgelerdeki kaynakları oluşturma |
| type |reddet |G-serisi sanal makinelerin oluşturulmasını Reddet |
| etiketler |reddet |Uygulama sahibi etiketi gerektirir |
| etiketler |reddet |Maliyet merkezi etiketi gerektirir |
| etiketler |ekleme |Etiket adı append **departmanı** ve etiket değeri **madde işaretleri** tüm kaynaklar için |

### <a name="resource-tags"></a>Kaynak etiketleri
Dave BitBucket uygulaması için maliyet merkezini tanımlamak için fatura belirli bilgileri olması gerektiğini bilir. Ayrıca, Dave madde işaretleri sahip tüm kaynakları bilmek ister.

Müşterinizle aşağıdaki ekler [etiketleri](resource-group-using-tags.md) kaynak grupları ve kaynaklara.

| Etiket adı | Etiket değeri |
| --- | --- |
| ApplicationOwner |Bu uygulama yöneten kişinin adı |
| Maliyet merkezi |Azure tüketimi için ödeme grubunun maliyet merkezi |
| Departmanı |**Madde işaretleri** (abonelikle ilişkili iş birimi) |

### <a name="core-network"></a>Çekirdek Ağ
Contoso madde işaretleri bilgi güvenliği ve risk yönetimi ekibi uygulamayı Azure'a taşımak için önerilen planı Dave'nın inceler. Uygulamanın Internet'e gösterilmeyen olmak isterler.  Dave Azure'a gelecekte taşınır Geliştirici uygulamaları da sahiptir. Bu uygulamalar genel arabirimler gerektirir.  Bu gereksinimleri karşılamak için kendisine iç ve dış sanal ağlar ve erişimi kısıtlamak için bir ağ güvenlik grubu sağlar.

Aşağıdaki kaynaklar oluşturur:

| Kaynak türü | Ad | Açıklama |
| --- | --- | --- |
| Sanal Ağ |vnet iç |ExpressRoute aracılığıyla Contoso şirket ağına ve BitBucket uygulama ile kullanılır.  Bir alt ağ (`bitbucket`) belirli bir IP adresi alanı uygulamayla sağlar |
| Sanal Ağ |Dış sanal ağ |Genel kullanıma yönelik uç noktalar gerektiren gelecekteki uygulamalar için kullanılabilir |
| Ağ Güvenliği Grubu |bitbucket nsg |Bu iş yükü, saldırı yüzeyini nerede uygulama yaşıyor yalnızca bağlantı noktası 443 üzerinden alt ağ için bağlantılara izin vererek indirilir sağlar (`bitbucket`) |

### <a name="resource-locks"></a>Kaynak kilitleri
Contoso şirket ağı iç sanal ağ bağlantısını herhangi bir wayward komut dosyası veya yanlışlıkla silinmesini korunmalıdır Dave tanır.

Aşağıdaki oluşturur [kaynak kilidi](resource-group-lock-resources.md):

| Kilit türü | Kaynak | Açıklama |
| --- | --- | --- |
| **CanNotDelete** |vnet iç |Kullanıcıların sanal ağ veya alt ağlar silmesini engeller, ancak yeni alt ağlar eklenmesi engellemez |

### <a name="azure-automation"></a>Azure Otomasyonu
Dave bu uygulama için otomatik hale getirmek için hiçbir şey vardır. Kendisine bir Azure Otomasyonu hesabı oluşturuldu ancak kendisine başlangıçta kullanmaz.

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi
Contoso BT Hizmet Yönetimi, hızlı bir şekilde tanımlamak ve tehditleri işlemek gerekiyor. Ayrıca hangi sorunları bulunabilecek anlamak isterler.  

Bu gereksinimleri karşılamak üzere Dave etkinleştirir [Azure Güvenlik Merkezi](../security-center/security-center-intro.md)ve güvenlik yöneticisi rolü erişim sağlar.

## <a name="scenario-2-customer-facing-app"></a>Senaryo 2: Müşteri dönük uygulama
Tedarik zinciri departmandaki iş liderlik bağlılık kart kullanarak Contoso'nun müşterilerle katılım artırmak için çeşitli fırsatlar belirledi. Alice'in ekibi, bu uygulama oluşturmanız gerekir ve Azure iş gereksinimini karşılamak üzere yeteneklerini artırır karar verir. Geliştirme ve bu uygulamayı işletim iki aboneliklerini yapılandırmak için madde işaretleri gelen Dave Alice çalışır.

### <a name="azure-subscriptions"></a>Azure abonelikleri
Dave Azure Enterprise Portal'de oturum açması ve tedarik zinciri departmanı zaten görür.  Ancak, bu proje için Azure tedarik zinciri grupta ilk geliştirme projesi olduğu gibi Dave Alice'in geliştirme ekibi yeni bir hesaba gereksinimini tanır.  Kendisi ekibi "R & D" hesabı oluşturur ve Alice için erişim atar. Alice Azure portalı üzerinden günlüğe kaydeder ve iki abonelikleri oluşturur: biri geliştirme sunucuları ve bir üretim sunucuları tutmak için tutmak için.  Aynen aşağıdaki abonelikler oluştururken, daha önce oluşturulmuş adlandırma standartları aşağıdaki gibidir:

| Abonelik kullan | Ad |
| --- | --- |
| Geliştirme |Contoso SupplyChain ResearchDevelopment LoyaltyCard geliştirme |
| Üretim |Contoso SupplyChain işlemleri LoyaltyCard üretim |

### <a name="policies"></a>İlkeler
Dave Alice uygulama görüşmek ve bu uygulamaya yalnızca Kuzey Amerika bölgede müşteriler hizmet tanımlayın.  Alice ve ekibi uygulaması oluşturmak için Azure uygulama hizmeti ortamı ve Azure SQL kullanmayı planlayın. Geliştirme sırasında sanal makineleri oluşturmanız gerekebilir.  Alice, kendi geliştiriciler keşfedin ve madde işaretleri içinde çekme olmadan sorunları incelemek için gereksinim duydukları kaynaklara sahip olduğundan emin olun ister.

İçin **geliştirme abonelik**, aşağıdaki ilkesi oluşturun:

| Alan | Etki | Açıklama |
| --- | --- | --- |
| location |Denetim |Denetim tüm bölgelerdeki kaynakları oluşturma |

Bir kullanıcı geliştirme oluşturabilirsiniz sku türünü sınırlamaz ve bunların etiketlerini herhangi bir kaynak grupları veya kaynaklar için gerektirmez.

İçin **üretim abonelik**, bunlar aşağıdaki ilkeleri oluşturur:

| Alan | Etki | Açıklama |
| --- | --- | --- |
| location |reddet |ABD veri merkezleri dışında herhangi bir kaynağa oluşturulmasını Reddet |
| etiketler |reddet |Uygulama sahibi etiketi gerektirir |
| etiketler |reddet |Departman etiketi gerektirir |
| etiketler |ekleme |Üretim ortamında gösterir her bir kaynak grubundaki etiket ekleme |

Bir kullanıcı üretimde oluşturabilirsiniz sku türünü sınırlamaz.

### <a name="resource-tags"></a>Kaynak etiketleri
Faturalama ve sahipliği için doğru iş gruplarını tanımlamak için özel bilgiler olması gerektiğini Dave bilir. Hüseyin, kaynak grupları ve kaynaklar için kaynak etiketleri tanımlar.

| Etiket adı | Etiket değeri |
| --- | --- |
| ApplicationOwner |Bu uygulama yöneten kişinin adı |
| Bölüm |Azure tüketimi için ödeme grubunun maliyet merkezi |
| EnvironmentType |**Üretim** (abonelik içerir olsa bile **üretim** adlarında bu etiketi de dahil olmak üzere kolay bir şekilde tanımlanması kaynakları portal ya da fatura bakıldığında sağlar) |

### <a name="core-networks"></a>Çekirdek Ağ
Contoso madde işaretleri bilgi güvenliği ve risk yönetimi ekibi uygulamayı Azure'a taşımak için önerilen planı Dave'nın inceler. Bunlar bağlılık kart uygulamasını düzgün bir şekilde yalıtılmış ve bir çevre ağında korumalı olmasını sağlamak istiyorsunuz.  Bu gereksinimi karşılamak için Dave ve Alice bir dış sanal ağ ve Bağlılık kartı uygulama Contoso şirket ağından ayırmak için bir ağ güvenlik grubu oluşturun.  

İçin **geliştirme abonelik**, bunlar oluşturur:

| Kaynak türü | Ad | Açıklama |
| --- | --- | --- |
| Sanal Ağ |vnet iç |Contoso Bağlılık kartı geliştirme ortamı sunar ve ExpressRoute aracılığıyla Contoso şirket ağına bağlı |

İçin **üretim abonelik**, bunlar oluşturur:

| Kaynak türü | Ad | Açıklama |
| --- | --- | --- |
| Sanal Ağ |Dış sanal ağ |Bağlılık kartı uygulamayı barındıran ve doğrudan Contoso'nun ExpressRoute bağlı değil. Kod kaynak kodu sistemlerine doğrudan PaaS hizmetlere gönderilir |
| Ağ Güvenliği Grubu |loyaltycard nsg |Bu iş yükü, saldırı yüzeyini bağlı iletişimi yalnızca TCP 443 numaralı vererek indirilir sağlar.  Contoso ayrıca ek koruma için bir Web uygulaması Güvenlik Duvarı'nı kullanarak araştırmaktadır. |

### <a name="resource-locks"></a>Kaynak kilitleri
Dave Alice confer ve bazı yalıtılarak bir kodun sırasında yanlışlıkla silinmesini önlemek için ortamı anahtar kaynakların kaynak kilitleri eklemeye karar.

Bunlar aşağıdaki kilit oluşturun:

| Kilit türü | Kaynak | Açıklama |
| --- | --- | --- |
| **CanNotDelete** |Dış sanal ağ |Kişiler sanal ağ veya alt ağlar silmesini önlemek için. Kilit yeni alt ağlar eklenmesi engellemez |

### <a name="azure-automation"></a>Azure Otomasyonu
Alice ve geliştirme ekibi bu uygulama için ortamınızı yönetmek için kapsamlı runbook'lar sahiptir. Runbook'ları uygulama ve diğer DevOps görevler için düğümleri eklendiği/silindiği izin verir.

Bu runbook'ları kullanmak için bunlar etkinleştirmeniz [Otomasyon](../automation/automation-intro.md).

### <a name="azure-security-center"></a>Azure Güvenlik Merkezi
Contoso BT Hizmet Yönetimi, hızlı bir şekilde tanımlamak ve tehditleri işlemek gerekiyor. Ayrıca hangi sorunları bulunabilecek anlamak isterler.  

Bu gereksinimleri karşılamak üzere Azure Güvenlik Merkezi Dave sağlar. Kendisine Azure Güvenlik Merkezi kaynakları izleme ve DevOps ve güvenlik ekipleri erişim sağlayan sağlar.

## <a name="next-steps"></a>Sonraki adımlar
* Resource Manager şablonları oluşturma hakkında bilgi edinmek için [en iyi uygulamalar Azure Resource Manager şablonları oluşturmak için](resource-manager-template-best-practices.md).
