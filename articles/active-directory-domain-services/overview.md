---
title: Azure Active Directory etki alanı Hizmetleri'ne Genel Bakış | Microsoft Docs
description: Azure Active Directory etki alanı Hizmetleri'ne Genel Bakış
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 0d47178f-773e-45f9-9ff4-9e8cffa4ffa2
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: e29936915f0cd0b7e7ae7adfdbdb90d31195cd34
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67472741"
---
# <a name="azure-active-directory-ad-domain-services"></a>Azure Active Directory (AD) etki alanı Hizmetleri
## <a name="overview"></a>Genel Bakış
Azure altyapı hizmetlerinin, geniş kapsamlı bilgi işlem çözümlerini Çevik bir şekilde dağıtmanızı sağlar. Azure sanal makineler ile neredeyse anında dağıtım yapabilir ve yalnızca dakika başına ödeme yaparsınız. Windows, Linux, SQL Server, Oracle, IBM, SAP ve BizTalk için destek kullanarak dağıtabileceğiniz her türlü iş yükü, neredeyse tüm işletim sistemlerinde herhangi bir dilde. Bu avantajlar azure'a giderlerinizi üzerinde kaydetmek için şirket içi dağıtılan eski uygulamaları geçirmek etkinleştirin.

Bu uygulamaların kimlik ihtiyaçlarını temel bir yönü, şirket içi uygulamalarını azure'a geçirme işleyen. Dizin kullanan uygulamalarınızı okuma veya yazma erişimi Kurumsal dizin üzerinde LDAP kullanır veya Windows tümleşik son kullanıcıların kimliğini doğrulamak için kimlik doğrulaması (Kerberos veya NTLM kimlik doğrulaması) kullanır. Güvenli bir Grup İlkesi kullanarak yönetilebilir şekilde Windows Server üzerinde çalışan satır iş kolu (LOB) uygulamaları, genellikle etki alanına katılmış makinelerde dağıtılır. Kurumsal kimlik altyapısını Bu bağımlılıklar için 'lift-and-shift ile taşıma' şirket içi uygulamaları buluta çözülmesi gerekir.

Yöneticiler, genellikle uygulamalarını Azure'da dağıtılan kimlik ihtiyaçlarını karşılamak için aşağıdaki çözümlerden birini açın:

* Azure altyapı hizmetleri ve kurumsal dizin şirket içinde çalışan iş yükleri arasında siteden siteye VPN bağlantısı dağıtın.
* Kurumsal AD etki alanına/ormana altyapısı, kopya etki alanı denetleyicilerini Azure sanal makineleri kullanarak ayarlayarak genişletin.
* Tek başına bir etki alanına etki alanı denetleyicilerini Azure sanal makineleri olarak dağıtılan kullanarak azure'da dağıtın.

Bu yaklaşım, yüksek maliyet ve yönetim yükünü düşebilir. Yöneticiler, etki alanı denetleyicileri Azure'da sanal makineleri kullanarak dağıtmak için gereklidir. Ayrıca, yönetme, güvenli, düzeltme eki uygulama, izleme, yedekleme ve sorun giderme bu sanal makineler gerekir. VPN bağlantıları şirket içi dizine güvenme geçici ağ hataları veya kesintiler savunmasız azure'da dağıtılmış iş yüklerini neden olur. Bu ağ kesintileri, daha düşük bir çalışma süresi ve bu uygulamalar için düşük güvenilirlik, sırayla sonuçlanır.

Azure AD Domain Services daha kolay bir alternatif sağlamak için tasarladığımız.

### <a name="watch-an-introductory-video"></a>Tanıtım videosunu izleyin

>[!VIDEO https://www.youtube.com/embed/T1Nd9APNceQ]

## <a name="introducing-azure-ad-domain-services"></a>Azure AD etki alanı hizmetlerine giriş

Azure AD Domain Services yönetilen etki alanı Hizmetleri gibi etki alanına katılmış, Windows Server Active Directory ile tamamen uyumlu Grup İlkesi, LDAP, Kerberos/NTLM kimlik doğrulaması sağlar. Bu etki alanı Hizmetleri dağıtma, yönetme ve bulutta etki alanı denetleyicileri düzeltme eki gereksinimi olmadan kullanabilir. Azure AD etki alanı Hizmetleri, böylece kullanıcılar şirket kimlik bilgilerini kullanarak oturum açması için olası yapmadan mevcut Azure AD kiracınız ile tümleştirilir. Ayrıca, bu nedenle bir daha yumuşak 'lift-and-shift' Azure altyapı hizmetleri için şirket içi kaynakların sağlama kaynaklarına erişimi güvenli hale getirmek için var olan Grup ve kullanıcı hesaplarını kullanabilirsiniz.

Azure AD etki alanı hizmetleri işlevleri, Azure AD kiracınızda yalnızca Bulut veya şirket içi Active Directory ile eşitlenen bağımsız olarak sorunsuz çalışır.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>Yalnızca bulut kuruluşlar için Azure AD etki alanı Hizmetleri

Yalnızca bulutta herhangi bir şirket içi kimlik ayak (genellikle 'yönetilen kiracılar' denir) Azure AD kiracısına sahip değil. Diğer bir deyişle, kullanıcı hesapları, parolaları ve grup üyeliklerini diğer bir deyişle, oluşturulan ve Azure AD'de yönetilen bir bulut - tüm yerel. Bir süre için Contoso salt bulut olduğunu göz önünde bulundurun. Azure AD kiracısı. Aşağıdaki çizimde gösterildiği gibi Contoso'nun yönetici Azure altyapı hizmetlerinde bir sanal ağ yapılandırdı. Uygulamaları ve sunucu iş yükleri, Azure sanal makineler'de bu sanal ağa dağıtılır. Contoso bir yalnızca bulut Kiracı olduğundan, tüm kullanıcı kimlikleri, kimlik bilgilerini ve grup üyeliklerini oluşturulur ve Azure AD'de yönetilir.

![Azure AD etki alanı hizmetleri genel bakış](./media/active-directory-domain-services-overview/aadds-overview.png)

Contoso'nun BT yöneticisi, Azure AD kiracısı için Azure AD Domain Services etkinleştirebilir ve etki alanı hizmetlerini bu sanal ağda kullanılabilir hale getirmek seçin. Bundan sonra Azure AD etki alanı Hizmetleri, yönetilen bir etki alanı sağlar ve sanal ağda kullanılabilir hale getirir. Ayrıca bu yeni oluşturulan etki alanındaki tüm kullanıcı hesapları, grup üyeliklerini ve Contoso Azure AD kiracısında mevcut kullanıcı kimlik bilgileri kullanılabilir. Bu özellik kuruluştaki kullanıcıların Kurumsal kimlik bilgileriyle - Örneğin, etki alanına katılmış makinelerde Uzak Masaüstü aracılığıyla uzaktan bağlanırken oluşan kullanarak etki alanında oturum açmak sağlar. Yöneticiler, var olan grup üyelikleri kullanarak etki alanındaki kaynaklara erişim sağlayabilirsiniz. Sanal ağda sanal makinelerde dağıtılan uygulamalar, etki alanına katılım, LDAP okuma, LDAP bağlaması, NTLM ve Kerberos kimlik doğrulaması ve Grup İlkesi gibi özellikleri kullanabilirsiniz.

Yönetilen etki alanı, Azure AD Domain Services tarafından sağlanan bazı belirgin yönleri şunlardır:

* Contoso'nun BT yöneticisi, yönetme, düzeltme eki veya bu etki alanına veya bu yönetilen etki alanı için herhangi bir etki alanı denetleyicilerini izlemek gerekmez.
* Bu etki alanı için AD çoğaltmayı yönetmek için gerek yoktur. Bu yönetilen etki alanı içinde otomatik olarak kullanıcı hesapları ve grup üyeliklerini Contoso Azure AD Kiracı kimlik bilgileri kullanılabilir.
* Azure AD etki alanı Hizmetleri tarafından Contoso kişinin etki alanı yönetilmesinden BT yöneticisi bu etki alanında etki alanı yöneticisinin veya Kurumsal Yönetici ayrıcalıkları yok.

### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>Hibrit kuruluşlar için Azure AD etki alanı Hizmetleri
Karma bir BT altyapısı olan kuruluşlar, Bulut ve şirket kaynaklarının bir karışımını kullanır. Bu durumdaki kuruluşların, kendi Azure AD kiracısı için kimlik bilgileri, şirket içi dizinden eşitleyin. Hibrit kuruluşlar daha geçirmek için konum olarak kendi şirket içi uygulamaların buluta, özellikle dizin kullanan eski uygulamalarınızı, Azure AD Domain Services için yararlı olabilir.

Litware Corporation dağıtıldı [Azure AD Connect](../active-directory/hybrid/whatis-hybrid-identity.md), kendi Azure AD kiracısı için kendi şirket içi dizinden kimlik bilgileri eşitlenemedi. Eşitlenen kimlik bilgilerini, kullanıcı hesapları, kimlik doğrulaması (parola karması eşitleme) ve grup üyeliklerini, kimlik bilgisi karmalarını içerir.

> [!NOTE]
> **Hibrit kuruluşlar Azure AD Domain Services'ı kullanmak parola karması eşitleme zorunludur**. Bu gereksinim, NTLM veya Kerberos kimlik doğrulama yöntemleri aracılığıyla bu kullanıcıların kimliğini doğrulamak için Azure AD Domain Services tarafından sağlanan yönetilen etki alanındaki kullanıcıların kimlik bilgileri gerekli olmasıdır.
>
>

![Litware Corporation için Azure AD etki alanı Hizmetleri](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

Önceki çizim, karma bir BT altyapısının, Litware Corporation gibi bir kuruluşlarıyla Azure AD Domain Services nasıl kullanabileceğinizi gösterir. Lıtware 's uygulamalar ve etki alanı Hizmetleri gerektiren sunucu iş yükleri, Azure altyapı Hizmetleri'nde bir sanal ağda dağıtılır. Litware BT yöneticisi, Azure AD kiracısı için Azure AD Domain Services'ı etkinleştirmek ve yönetilen bir etki alanı bu sanal ağda kullanılabilir hale getirmek seçin. Karma bir BT altyapısının bir kuruluşla Lıtware olduğundan, kendi Azure AD kiracısına kullanıcı hesapları, grupları ve kimlik bilgilerini, şirket içi dizininizden eşitlenmiş. Bu özellik makinelere uzaktan bağlanma Uzak Masaüstü aracılığıyla etki alanına katıldığında, şirket kimlik bilgilerini - örneğin, kullanarak etki alanına oturum açmalarını sağlar. Yöneticiler, var olan grup üyelikleri kullanarak etki alanındaki kaynaklara erişim sağlayabilirsiniz. Sanal ağda sanal makinelerde dağıtılan uygulamalar, etki alanına katılım, LDAP okuma, LDAP bağlaması, NTLM ve Kerberos kimlik doğrulaması ve Grup İlkesi gibi özellikleri kullanabilirsiniz.

Yönetilen etki alanı, Azure AD Domain Services tarafından sağlanan bazı belirgin yönleri şunlardır:

* Yönetilen etki alanı, tek başına bir etki alanıdır. Litware'nın şirket içi etki alanı uzantısı değil.
* Litware, düzeltme eki, yönetme veya bu yönetilen etki alanı için etki alanı denetleyicilerini izlemek BT yöneticinize gerekmez.
* Bu etki alanı için AD çoğaltmayı yönetmek için gerek yoktur. Kullanıcı hesapları ve grup üyeliklerini Litware'nın şirket içi dizinden kimlik, Azure AD Connect yoluyla Azure ad'yle eşitlenir. Bu kullanıcı hesapları, Grup üyeliklerinin ve kimlik bilgilerini yönetilen etki alanında otomatik olarak kullanılabilir.
* Azure AD etki alanı Hizmetleri tarafından Litware kişinin etki alanı yönetilmesinden BT yöneticisi bu etki alanında etki alanı yöneticisinin veya Kurumsal Yönetici ayrıcalıkları yok.

## <a name="benefits"></a>Avantajlar
Azure AD Domain Services ile aşağıdaki avantajlardan yararlanabilirsiniz:

* **Basit** – Azure altyapı hizmetlerine birkaç basit tıklama ile dağıtılan sanal makinelerin kimlik ihtiyaçlarını karşılayabilecek. Dağıtma ve Azure ya da kurulum bağlantısı, şirket içi kimlik altyapınızı dön kimlik altyapısını yönetmek gerekmez.
* **Tümleşik** – Azure AD Domain Services, Azure AD kiracınız ile son derece tümleşiktir. Azure AD, hem modern uygulamalar hem de Geleneksel dizin kullanan uygulamalar için ihtiyaçlarını oluşturabilmesine olanak sağlar. bir tümleşik bulut tabanlı Kurumsal dizini olarak artık kullanabilirsiniz.
* **Uyumlu** – Azure AD Domain Services, Windows Server Active Directory kanıtlanmış kurumsal sınıf altyapısında oluşturulur. Bu nedenle, uygulamalarınızı Windows Server Active Directory özellikleri ile uyumluluk büyük ölçüde güvenebilirsiniz. Windows Server AD tüm özellikler şu anda Azure AD Etki Alanı Hizmetleri'nde kullanılabilir. Ancak, kullanılabilir özellikler, şirket içi altyapınızda kullandığınız karşılık gelen Windows Server AD özelliklerinin ile uyumludur. LDAP, Kerberos, NTLM, Grup İlkesi ve etki alanı katılma özellikleri test edilmiş ve çeşitli Windows Server sürümlere göre iyileştirilmektedir olgun bir teklif oluşturur.
* **Uygun maliyetli** – Azure AD Domain Services ile geleneksel dizin kullanan uygulamalarınızı desteklemek için kullanılan kimlik altyapısını yönetme ile ilgili olan altyapı ve yönetim yükünü önleyebilirsiniz. Bu uygulamaların Azure altyapı hizmetleri için taşıma ve işletimsel giderlerinizi hakkında daha fazla tasarruf yararlanın.


## <a name="next-steps"></a>Sonraki adımlar
### <a name="learn-more-about-azure-ad-domain-services"></a>Azure AD Domain Services hakkında daha fazla bilgi edinin
* [Özellikler](active-directory-ds-features.md)
* [Dağıtım senaryoları](scenarios.md)
* [Azure AD Domain Services, kullanım örneklerinize uygun öğrenin](comparison.md)
* [Azure AD Domain Services ile Azure AD dizininizi nasıl eşitleneceğini anlama](synchronization.md)

### <a name="get-started-with-azure-ad-domain-services"></a>Azure AD Etki Alanı Hizmetleri’ni kullanmaya başlama
* [Azure portalını kullanarak Azure AD Domain Services'ı etkinleştir](create-instance.md)
