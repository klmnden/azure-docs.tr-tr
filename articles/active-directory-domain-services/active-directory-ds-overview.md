---
title: "Azure Active Directory etki alanı Hizmetleri'ne Genel Bakış | Microsoft Docs"
description: "Azure Active Directory etki alanı Hizmetleri'ne Genel Bakış"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mahesh-unnikrishnan
editor: curtand
ms.assetid: 0d47178f-773e-45f9-9ff4-9e8cffa4ffa2
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: maheshu
ms.openlocfilehash: be18ee0266a97057499baccc5bb39a35224336d7
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="azure-active-directory-ad-domain-services"></a>Azure Active Directory (AD) etki alanı Hizmetleri
## <a name="overview"></a>Genel Bakış
Azure altyapı hizmetleri çözümleri Çevik bir şekilde bilgi işlem çok çeşitli dağıtmanıza olanak sağlar. Azure Virtual Machines ile neredeyse anlık olarak dağıtabilir ve yalnızca dakikaya göre ödeme. Windows, Linux, SQL Server, Oracle, IBM, SAP ve BizTalk için destek kullanarak dağıtabileceğiniz herhangi bir iş yükü, neredeyse tüm işletim sisteminde herhangi bir dil. Avantajlar içi dağıtılan eski uygulamalar üzerinde işletim masrafları'nı kaydetmek için Azure geçirmek etkinleştirin.

Temel bir yönü azure'a geçirme şirket içi uygulamalar, bu uygulamaların kimlik gereksinimlerini işliyor. Dizin ile uyumlu uygulamalar okuma veya yazma erişimi şirket dizinine üzerinde LDAP kullanır veya Windows tümleşik son kullanıcıların kimliğini doğrulamak için kimlik doğrulaması (Kerberos veya NTLM kimlik doğrulaması) kullanır. Güvenli bir şekilde Grup İlkesi kullanılarak yönetilebilir dağıtılır, bu nedenle Windows Server'da çalışan satır iş kolu (LOB) uygulamaları genellikle etki alanına katılmış makinelerde. Buluta 'yükseltme ve shift' şirket içi uygulamalar için Kurumsal kimlik altyapısı bu bağımlılıkları çözülmesi gerekir.

Yöneticiler için Azure üzerinde dağıtılan uygulamalarını kimlik ihtiyaçlarını karşılamak için aşağıdaki çözümlerden birini genellikle açın:

* Azure altyapı hizmetleri ve kurumsal şirket içi dizin çalışan iş yükleri arasında bir siteden siteye VPN bağlantısı dağıtın.
* Azure sanal makineleri kullanarak kopya etki alanı denetleyicilerini ayarlayarak şirket AD etki alanında/ormanda altyapısını genişletir.
* Tek başına bir etki alanında etki alanı denetleyicilerini Azure sanal makineleri olarak dağıtılan kullanarak Azure dağıtın.

Bu yaklaşım, yüksek maliyet ve yönetim yükünü yaşar. Yöneticiler, Azure sanal makineleri kullanarak etki alanı denetleyicileri dağıtmak için gereklidir. Ayrıca, yönetme, güvenli, düzeltme eki, izlemek, yedekleme ve bu sanal makineleri sorun giderme gerekir. Şirket içi dizin için VPN bağlantılarında olmasının geçici ağ hataları veya kesintilerine karşı savunmasız azure'da dağıtılan iş yükleri neden olur. Bu ağ kesintileri alt açık kalma süresi ve bu uygulamalar için düşük güvenilirlik, sırayla sonuçlanır.

Biz Azure AD etki alanı Hizmetleri kolay alternatif sağlamak için tasarlanmıştır.

### <a name="watch-an-introductory-video"></a>Bir tanıtım videosunu izleyin
<iframe width="560" height="315" src="https://www.youtube.com/embed/T1Nd9APNceQ" frameborder="0" allowfullscreen></iframe>


## <a name="introducing-azure-ad-domain-services"></a>Azure AD etki alanı hizmetlerine giriş
Azure AD etki alanı Hizmetleri yönetilen etki alanı Hizmetleri gibi etki alanına katılma, Windows Server Active Directory ile tamamen uyumlu Grup İlkesi, LDAP, Kerberos/NTLM kimlik doğrulaması sağlar. Bu etki alanı Hizmetleri, dağıtmak, yönetmek ve etki alanı denetleyicileri bulutta düzeltme eki gerek kalmadan kullanabilir. Azure AD etki alanı Hizmetleri, böylece kullanıcılar şirket kimlik bilgilerini kullanarak oturum açması için olası yapma, mevcut Azure AD kiracısı ile tümleşir. Ayrıca, bu nedenle bir sorunsuz 'yükseltme-ve-shift' şirket içi kaynaklar Azure altyapı hizmetleri sağlama kaynaklarına erişim güvenliğini sağlamak için var olan grupları ve kullanıcı hesaplarını kullanabilirsiniz.

Azure AD kiracınıza yalnızca Bulut veya şirket içi Active Directory ile eşitlenmiş olduğuna bakılmaksızın Azure AD etki alanı hizmetleri işlevleri sorunsuz çalışır.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>Yalnızca bulut kuruluşlar için Azure AD etki alanı Hizmetleri
Yalnızca bulutta Azure AD kiracısı (genellikle 'yönetilen kiracılar' adlandırılır), tüm şirket içi kimlik ayak izini sahip değil. Diğer bir deyişle, kullanıcı hesaplarını, parolaları ve grup üyeliklerini tüm yerel buluta - diğer bir deyişle, oluşturulur ve Azure AD içinde yönetilir. Bir süre Contoso salt bulut olduğunu göz önünde bulundurun Azure AD kiracısı. Aşağıdaki çizimde gösterildiği gibi Contoso'nun yönetici Azure altyapı Hizmetleri'nde bir sanal ağ yapılandırdı. Uygulamaları ve sunucu iş yükleri, bu sanal ağ Azure sanal makinelerde dağıtılır. Contoso bir yalnızca bulut Kiracı olduğundan, tüm kullanıcı kimlikleri, kimlik bilgileri ve grup üyeliklerini oluşturulur ve Azure AD içinde yönetilir.

![Azure AD etki alanı hizmetleri genel bakış](./media/active-directory-domain-services-overview/aadds-overview.png)

Contoso'nun BT yöneticisi kendi Azure AD kiracısı Azure AD Etki Alanı Hizmetleri'ni etkinleştirmek ve etki alanı Hizmetleri bu sanal ağın kullanılabilmesi seçin. Bundan sonra Azure AD etki alanı Hizmetleri yönetilen etki alanı sağlar ve sanal ağda kullanılabilir hale getirir. Ayrıca tüm kullanıcı hesapları, grup üyelikleri ve kullanıcı kimlik bilgilerini Contoso Azure AD kiracısında kullanılabilir yeni oluşturulan bu etki alanında kullanılabilir. Bu özellik, kuruluşunuzdaki kullanıcılar Kurumsal kimlik - Örneğin, uzaktan etki alanına katılmış makinelere Uzak Masaüstü bağlanırken kullanarak etki alanında oturum açmak sağlar. Yöneticiler, var olan grup üyelikleri kullanarak etki alanındaki kaynaklara erişim sağlayabilir. Sanal makineler sanal ağda dağıtılmış uygulamalar etki alanına katılma, LDAP okuma, LDAP bağlama, NTLM ve Kerberos kimlik doğrulaması ve Grup İlkesi gibi özellikleri kullanabilir.

Azure AD etki alanı Hizmetleri tarafından sağlanan yönetilen etki alanı birkaç belirgin yönleri şunlardır:

* Contoso'nun BT yöneticisi, yönetme, düzeltme eki veya bu etki alanına veya bu yönetilen etki alanı için herhangi bir etki alanı denetleyicilerini izlemek gerekmez.
* Bu etki alanı için AD çoğaltmayı yönetmek için gerek yoktur. Kullanıcı hesapları, grup üyelikleri ve Contoso Azure AD Kiracı kimlik bilgilerini Bu yönetilen etki alanı içinde otomatik olarak kullanılabilir.
* Azure AD etki alanı Hizmetleri tarafından Contoso kişinin etki alanı yönetildiğinden BT yöneticisine bu etki alanında etki alanı yöneticisi veya kuruluş yöneticisi ayrıcalıkları yok.

### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>Karma kuruluşlar için Azure AD etki alanı Hizmetleri
Karma BT altyapısı olan kuruluşlar, Bulut ve şirket kaynaklarının karışımını tüketir. Bu tür kuruluşlar kendi Azure AD kiracısı kendi şirket içi dizin kimlik bilgileriyle eşitleyin. Karma kuruluşlar daha geçirmek için konum olarak kendi şirket içi uygulamaların buluta, özellikle eski dizin algılayan uygulamaları, Azure AD etki alanı Hizmetleri için yararlı olabilir.

Lıtware Corporation dağıtılan [Azure AD Connect](../active-directory/active-directory-aadconnect.md), kendi şirket içi dizin kimlik bilgilerinden kendi Azure AD kiracınıza eşitlemek için. Eşitlenen kimlik bilgileri, kullanıcı hesapları, kimlik doğrulaması (Parola Eşitleme) ve grup üyeliklerini kendi kimlik bilgisi karmalarını içerir.

> [!NOTE]
> **Azure AD Etki Alanı Hizmetleri'ni kullanmak üzere karma kuruluşlar için parola eşitleme zorunludur**. Bu gereksinim, NTLM veya Kerberos kimlik doğrulama yöntemleri aracılığıyla bu kullanıcıların kimliklerini doğrulamak için Azure AD etki alanı Hizmetleri tarafından sağlanan yönetilen etki alanındaki kullanıcıların kimlik bilgileri gerekli olmasıdır.
>
>

![Lıtware Corporation için Azure AD etki alanı Hizmetleri](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

Önceki çizim, karma Lıtware Corporation gibi BT altyapısı olan kuruluşlar Azure AD etki alanı hizmetleri nasıl kullanabileceğinizi gösterir. Lıtware 's uygulamaları ve etki alanı Hizmetleri gerektiren sunucu iş yükleri, Azure altyapı Hizmetleri'nde bir sanal ağ içinde dağıtılır. Lıtware 's BT yöneticisi kendi Azure AD kiracısı Azure AD Etki Alanı Hizmetleri'ni etkinleştirmek ve yönetilen bir etki alanına bu sanal ağın kullanılabilmesi seçin. Lıtware bir kuruluşla karma BT altyapısı olmadığından, kullanıcı hesaplarını, grupları ve kimlik bilgilerini Azure AD kiracısının kendi şirket içi dizininden eşitlenir. Bu özellik makinelere uzaktan bağlanma Uzak Masaüstü etki alanına katıldığında şirket kimlik bilgilerini - örneğin, kullanarak etki alanına oturum açmalarını sağlar. Yöneticiler, var olan grup üyelikleri kullanarak etki alanındaki kaynaklara erişim sağlayabilir. Sanal makineler sanal ağda dağıtılmış uygulamalar etki alanına katılma, LDAP okuma, LDAP bağlama, NTLM ve Kerberos kimlik doğrulaması ve Grup İlkesi gibi özellikleri kullanabilir.

Azure AD etki alanı Hizmetleri tarafından sağlanan yönetilen etki alanı birkaç belirgin yönleri şunlardır:

* Yönetilen etki alanı tek başına bir etki alanıdır. Uzantı Lıtware 's şirket içi etki alanının değil.
* Lıtware 's, düzeltme eki, yönetme veya yönetilen bu etki alanı için etki alanı denetleyicilerini izlemek BT yöneticinize gerekmez.
* Bu etki alanı için AD çoğaltmayı yönetmek için gerek yoktur. Kullanıcı hesapları, grup üyelikleri ve Lıtware 's şirket içi dizin kimlik bilgilerini Azure AD Connect aracılığıyla Azure ad ile eşitlenir. Bu kullanıcı hesapları, grup üyelikleri ve kimlik bilgilerini yönetilen etki alanı içinde otomatik olarak kullanılabilir.
* Etki alanı Azure AD etki alanı Hizmetleri tarafından Lıtware 's yönetildiğinden BT yöneticisine bu etki alanında etki alanı yöneticisi veya kuruluş yöneticisi ayrıcalıkları yok.

## <a name="benefits"></a>Avantajlar
Azure AD etki alanı Hizmetleri ile aşağıdaki yararları keyfini çıkarabilirsiniz:

* **Basit** – Azure altyapı hizmetleri birkaç basit tıklama ile dağıtılan sanal makinelerin kimlik gereksinimlerini karşılayabilen. Dağıtma ve kimlik altyapısı Azure veya Kurulum bağlantısı geri şirket içi kimlik altyapınızı yönetmek gerekmez.
* **Tümleşik** – Azure AD etki alanı Hizmetleri ile Azure AD kiracınıza son derece tümleşiktir. Azure AD, modern uygulamalar ve geleneksel directory kullanabilen uygulamaların gereksinimlerini caters bir tümleşik Kurumsal bulut tabanlı dizini artık kullanabilirsiniz.
* **Uyumlu** – Azure AD etki alanı Hizmetleri, Windows Server Active Directory kanıtlanmış Kurumsal düzeyde altyapı oluşturulur. Bu nedenle, uygulamalarınızı bir Windows Server Active Directory özellikleri ile uyumluluk büyük ölçüde güvenebilirsiniz. Windows Server AD kullanılabilen tüm özellikleri şu anda Azure AD Etki Alanı Hizmetleri'nde kullanılabilir. Ancak, kullanılabilir özellikleri şirket içi altyapınızda Bel karşılık gelen Windows Server AD özellikleri ile uyumlu değildir. LDAP, Kerberos, NTLM, Grup İlkesi ve etki alanı katılma özellikleri, test ve çeşitli Windows Server sürümleri iyileştirilmiştir olgun bir teklifi oluşturur.
* **Düşük maliyetli** – Azure AD etki alanı Hizmetleri ile geleneksel dizin algılayan uygulamaları desteklemek üzere kimlik altyapınızı yönetme ile ilişkilendirilmiş altyapı ve yönetim yükünü önleyebilirsiniz. Azure altyapı hizmetleri bu uygulamalardan taşıyabilir ve işletim masrafları'nı hakkında daha fazla tasarruf etmenizi yararlı.


## <a name="next-steps"></a>Sonraki adımlar
### <a name="learn-more-about-azure-ad-domain-services"></a>Azure AD etki alanı hizmetleri hakkında daha fazla bilgi edinin
* [Özellikler](active-directory-ds-features.md)
* [Dağıtım senaryoları](active-directory-ds-scenarios.md)
* [Azure AD etki alanı Hizmetleri, kullanım örnekleri, uygun öğrenin](active-directory-ds-comparison.md)
* [Azure AD etki alanı Hizmetleri ile Azure AD dizinini nasıl eşitleneceğini anlama](active-directory-ds-synchronization.md)

### <a name="get-started-with-azure-ad-domain-services"></a>Azure AD Etki Alanı Hizmetleri’ni kullanmaya başlama
* [Azure portalını kullanarak Azure AD Etki Alanı Hizmetleri'ni etkinleştirme](active-directory-ds-getting-started.md)
