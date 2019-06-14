---
title: 'Azure AD Connect: Desteklenen topolojiler | Microsoft Docs'
description: Bu konu Azure AD Connect için topolojiler desteklenen ve desteklenmeyen ayrıntıları
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 1034c000-59f2-4fc8-8137-2416fa5e4bfe
ms.service: active-directory
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.topic: conceptual
ms.date: 11/27/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b1c0d33a7d920f76bcbea6d8d6babc7390003bc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60383891"
---
# <a name="topologies-for-azure-ad-connect"></a>Azure AD Connect için topolojiler
Bu makalede, çeşitli şirket içi ve Azure AD Connect eşitleme anahtar tümleştirme çözümü olarak kullanan Azure Active Directory (Azure AD) topolojileri açıklanır. Bu makale, desteklenen ve desteklenmeyen yapılandırmalar içerir.


Gösterge makalede resimler için şu şekildedir:

| Açıklama | Sembol |
| --- | --- |
| Şirket içi Active Directory ormanı |![Şirket içi Active Directory ormanı](./media/plan-connect-topologies/LegendAD1.png) |
| Şirket içi Active Directory ile filtrelenmiş içeri aktarma |![Active Directory ile filtrelenmiş içeri aktarma](./media/plan-connect-topologies/LegendAD2.png) |
| Azure AD Connect eşitleme sunucusu |![Azure AD Connect eşitleme sunucusu](./media/plan-connect-topologies/LegendSync1.png) |
| "Hazırlama modunda" azure AD Connect eşitleme sunucusu |!["Hazırlama modunda" azure AD Connect eşitleme sunucusu](./media/plan-connect-topologies/LegendSync2.png) |
| Forefront Identity Manager (FIM) 2010 veya Microsoft Identity Manager (MIM) 2016 ile GALSync |![FIM 2010 veya MIM 2016 ile GALSync](./media/plan-connect-topologies/LegendSync3.png) |
| Ayrıntılı azure AD Connect eşitleme sunucusu |![Ayrıntılı azure AD Connect eşitleme sunucusu](./media/plan-connect-topologies/LegendSync4.png) |
| Azure AD |![Azure Active Directory](./media/plan-connect-topologies/LegendAAD.png) |
| Desteklenmeyen senaryo |![Desteklenmeyen senaryo](./media/plan-connect-topologies/LegendUnsupported.png) |


> [!IMPORTANT]
> Microsoft, değiştirme veya yapılandırmaları veya resmi olarak belgelenen Eylemler dışında Azure AD Connect eşitleme işletim desteklemiyor. Bu yapılandırmalar veya Eylemler, bir Azure AD Connect eşitleme tutarsız veya desteklenmeyen durumda neden olabilir. Sonuç olarak Microsoft, bu tür dağıtımlar için teknik destek sağlayamaz.


## <a name="single-forest-single-azure-ad-tenant"></a>Tek bir orman, tek bir Azure AD kiracısı
![Tek bir ormana ve tek bir kiracı için topoloji](./media/plan-connect-topologies/SingleForestSingleDirectory.png)

Tek bir orman, bir veya birden çok etki alanları ve tek bir Azure AD Kiracı şirket içinde en sık kullanılan topolojidir. Azure AD kimlik doğrulaması için parola karması eşitleme kullanılır. Azure AD Connect'i hızlı yükleme yalnızca bu topoloji destekler.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Tek orman, birden çok eşitleme sunucusu için bir Azure AD kiracısı
![Desteklenmeyen, filtrelenmiş topolojisi tek orman](./media/plan-connect-topologies/SingleForestFilteredUnsupported.png)

Birden çok Azure AD Connect eşitleme sunucusu için aynı Azure AD kiracısına bağlı olması desteklenmiyor, dışında bir [sunucusu hazırlama](#staging-server). Bu sunucular ile birbirini dışlayan bir nesneler kümesini eşitlemek için yapılandırılmış olsa bile bu desteklenmiyor. Tek bir sunucudan ormandaki tüm etki alanları ulaşamazsa veya çeşitli sunucular arasında yük dağıtmak istiyorsanız bu topoloji kabul.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Birden çok orman, tek bir Azure AD kiracısı
![Birden çok ormanı ve tek bir kiracı için topoloji](./media/plan-connect-topologies/MultiForestSingleDirectory.png)

Birçok kuruluşun ortamlar ile birden çok şirket içi Active Directory ormanları vardır. Birden fazla şirket içi Active Directory ormanına sahip olmak için çeşitli nedenleri vardır. Hesap-kaynak ormanına ve birleşme veya sonucu ile tasarımları tipik örnekleridir.

Birden çok orman, tüm ormanlardaki olduğunda tek bir tarafından erişilebilir olmalıdır Azure AD Connect eşitleme sunucusu. Sunucu bir etki alanına sahip değilsiniz. Gerekirse tüm ormanlarda ulaşmak, sunucu bir çevre ağında (DMZ, sivil bölge ve denetimli alt ağ olarak da bilinir) yerleştirebilirsiniz.

Azure AD Connect Yükleme Sihirbazı'nı birden fazla ormanda temsil kullanıcılar birleştirmek için çeşitli seçenekler sunar. Bir kullanıcı, Azure AD'de yalnızca bir kez temsil edilir hedeftir. Özel bir yükleme yolu Yükleme Sihirbazı'nda yapılandırabileceğiniz bazı yaygın topoloji vardır. Üzerinde **kullanıcılarınız eşsiz şekilde tanımlanıyor** sayfasında, topolojinizi temsil eden ilgili seçeneği seçin. Birleştirme yalnızca kullanıcılar için yapılandırılır. Varsayılan yapılandırmada yinelenen grupları birleştirilmiş değil.

Ortak topolojiler, ayrı topolojiler hakkında bölümlerde açıklanmıştır [tam mesh](#multiple-forests-full-mesh-with-optional-galsync), ve [hesap-kaynak topolojisi](#multiple-forests-account-resource-forest).

Varsayılan yapılandırma, Azure AD Connect eşitleme varsayılır:

* Her kullanıcının yalnızca bir etkin hesabı vardır ve bu hesabın bulunduğu orman, kullanıcı kimlik doğrulaması için kullanılır. Bu, parola karma eşitlemesi, geçişli kimlik doğrulaması ve Federasyon varsayılır. UserPrincipalName ve sourceAnchor/Immutableıd bu ormandan gelir.
* Her kullanıcının yalnızca bir posta kutusu yok.
* Bir kullanıcının posta kutusunu barındıran ormanda Exchange Genel adres listesi (GAL) içinde görünür olan öznitelikler için en iyi veri kalitesi vardır. Kullanıcı için posta kutusu varsa, herhangi bir orman, bu öznitelik değerleri katkıda bulunmak için kullanılabilir.
* Bağlı bir posta kutusu varsa, ayrıca bir hesap var. oturum açmak için kullanılan farklı bir ormanda.

Bu varsayımların ortamınız eşleşmiyorsa, aşağıdaki durumlar ortaya çıkar:

* Birden fazla etkin hesap ya da birden fazla posta kutusu varsa, eşitleme altyapısı birini seçer ve diğer yok sayar.
* Bağlı bir posta kutusu etkin herhangi bir hesapla Azure AD'ye aktarılan değil. Kullanıcı hesabının herhangi bir grubu üye olarak gösterilmiyor. DirSync bağlı bir posta kutusu, her zaman normal bir posta kutusu temsil edilir. Bu kasıtlı olarak birden çok ormanlı senaryolar daha iyi desteklemek için farklı bir davranış değişikliktir.

Daha fazla bilgi bulabilirsiniz [varsayılan yapılandırmayı anlama](concept-azure-ad-connect-sync-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Birden çok orman, birden çok eşitleme sunucusu için bir Azure AD kiracısı
![Desteklenmeyen bir topoloji birden fazla ormanına ve birden çok eşitleme sunucusu](./media/plan-connect-topologies/MultiForestMultiSyncUnsupported.png)

Birden fazla Azure AD Connect eşitleme sunucusu tek bir bağlı olan Azure AD kiracısı desteklenmiyor. Özel durum kullanımı olan bir [sunucusu hazırlama](#staging-server).

Bir, bu topoloji farklıdır **birden çok eşitleme sunucusu** bağlı tek bir Azure AD Kiracı desteklenmiyor.

### <a name="multiple-forests-single-sync-server-users-are-represented-in-only-one-directory"></a>Birden çok orman, tek eşitleme sunucusu, kullanıcıların yalnızca bir dizine temsil edilir
![Kullanıcılar yalnızca bir kez temsil eden tüm dizinler seçeneği](./media/plan-connect-topologies/MultiForestUsersOnce.png)

![Birden çok orman ve ayrı topolojiler gösterimi](./media/plan-connect-topologies/MultiForestSeparateTopologies.png)

Bu ortamda, tüm şirket içi ormanları ayrı varlıklar olarak kabul edilir. Hiçbir kullanıcı başka bir ormanda bulunur. Her bir orman kendi Exchange kuruluşu olan ve ormanlar arasında hiçbir GALSync yoktur. Bu topoloji, burada her departman bağımsız olarak çalışır duruma birleşme/edinme sonra veya bir kuruluşta olabilir. Bu orman, Azure AD aynı kuruluşta bulunan ve birleşik bir GAL ile görünür. Önceki resimde, her ormanda her nesne meta veri deposunda bir kez temsil edilir ve hedef Azure AD kiracısında toplanır.

### <a name="multiple-forests-match-users"></a>Birden çok orman: eşleşen kullanıcılar
Tüm bu senaryolar, dağıtım için ortaktır ve güvenlik grupları, kullanıcıları, kişileri ve yabancı güvenlik sorumlusu (FSP) bir karışımını içerebilir. FSP Active Directory Etki Alanı Hizmetleri'nde (AD DS) bir güvenlik grubundaki diğer ormanlardaki üyelerini temsil etmek üzere kullanılır. Tüm FSP Azure AD'de gerçek nesneye çözümlenir.

### <a name="multiple-forests-full-mesh-with-optional-galsync"></a>Birden çok orman: tam mesh isteğe bağlı GALSync ile
![Posta özniteliği, kullanıcı kimlikleri birden fazla dizinde mevcut olduğunda eşleştirmek için kullanma seçeneği](./media/plan-connect-topologies/MultiForestUsersMail.png)

![Birden çok orman için tam ağ topolojisi](./media/plan-connect-topologies/MultiForestFullMesh.png)

Tam ağ topolojisi herhangi bir ormanda yer alması, kullanıcılar ve kaynaklar sağlar. Genellikle, ormanlar arasında iki yönlü güven vardır.

Birden fazla ormanda Exchange varsa olabilir (isteğe bağlı olarak) bir şirket içi GALSync çözümü. Her kullanıcının diğer tüm ormanlarda kişi olarak temsil edilir. GALSync, FIM 2010 veya MIM 2016 ile yaygın olarak uygulanır. Azure AD Connect, şirket içi GALSync için kullanılamaz.

Bu senaryoda, kimlik nesneleri posta özniteliği ile birleştirilir. Bir ormanda bir posta kutusu olan bir kullanıcı, diğer ormanlardaki kişiler ile birleştirilir.

### <a name="multiple-forests-account-resource-forest"></a>Birden çok orman: hesap-kaynak ormanı
![ObjectSID ve msExchMasterAccountSID öznitelikleri kimlikleri birden fazla dizinde mevcut olduğunda eşleştirmek için kullanma seçeneği](./media/plan-connect-topologies/MultiForestUsersObjectSID.png)

![Birden çok orman için hesap-kaynak ormanı topolojisi](./media/plan-connect-topologies/MultiForestAccountResource.png)

Bir veya daha fazla sahip bir hesap-kaynak orman topolojisini *hesabı* etkin kullanıcı hesapları ile ormanlar. Ayrıca bir veya daha fazla sahip *kaynak* devre dışı bırakılmış hesapları ormanlar.

Bu senaryoda, bir (veya daha fazla) kaynak ormanı tüm hesap ormanları güvenir. Kaynak ormanı genellikle Exchange ve Lync ile genişletilmiş bir Active Directory şeması vardır. Tüm Exchange ve Lync hizmetlerinin yanı sıra diğer paylaşılan hizmetler, bu ormanda yer alır. Kullanıcılar devre dışı bırakılmış kullanıcı hesabı, bu ormandaki sahip ve posta hesabı ormana bağlanır.

## <a name="office-365-and-topology-considerations"></a>Office 365 ve topolojide dikkat edilmesi gerekenler
Bazı Office 365 iş yükleri üzerinde desteklenen topolojiler bazı kısıtlamalar vardır:

| İş yükü | Kısıtlamalar |
| --------- | --------- |
| Exchange Online | Exchange Online tarafından desteklenen karma topolojiler hakkında daha fazla bilgi için bkz: [birden çok Active Directory ormanı ile karma dağıtımlar](https://technet.microsoft.com/library/jj873754.aspx). |
| Skype Kurumsal | Birden çok şirket içi ormanı kullanırken, yalnızca hesap-kaynak orman topolojisini desteklenir. Daha fazla bilgi için [Skype için ortam gereksinimleri sunucu 2015](https://technet.microsoft.com/library/dn933910.aspx). |

Daha büyük bir kuruluş olan sonra kullanmayı düşünmelisiniz [Office 365 PreferredDataLocation](how-to-connect-sync-feature-preferreddatalocation.md) özelliği. Hangi veri merkezi bölgesinde bulunan kullanıcının kaynakları tanımlamanıza olanak sağlar.

## <a name="staging-server"></a>Hazırlama sunucusu
![Hazırlama sunucusu topolojisinde](./media/plan-connect-topologies/MultiForestStaging.png)

Azure AD Connect desteklediği ikinci bir sunucuya yükleme *hazırlama modunda*. Bu modundaki bir sunucuyu bağlı tüm dizinleri verileri okur, ancak dizinlere bağlı hiçbir şey yazmaz. Bu, normal bir eşitleme döngüsü kullanır ve kimlik verilerini güncelleştirilmiş bir kopyasını dolayısıyla vardır.

Burada birincil sunucu başarısız bir olağanüstü durumda, hazırlama sunucuya devredebilir. Azure AD Connect Sihirbazı'nda yaparsınız. Herhangi bir altyapı olan birincil sunucuyu paylaşıldığından bu ikinci sunucuyu farklı bir veri merkezinde yer alabilir. İkinci sunucuyu birincil sunucuya yapılan herhangi bir yapılandırma değişikliği elle kopyalamanız gerekir.

Yeni özel yapılandırma ve verileriniz üzerinde olan etkisini test etmek için bir hazırlık sunucusu kullanabilirsiniz. Değişiklikleri Önizleme ve yapılandırmasını ayarlayın. Yeni yapılandırmayla memnun olduğunuzda, etkin sunucunun hazırlık sunucusu olun ve eski etkin sunucu hazırlama moduna ayarlayın.

Ayrıca, etkin eşitleme sunucusunu değiştirmek için bu yöntemi kullanabilirsiniz. Yeni sunucuyu hazırlamak ve hazırlama moduna ayarlayın. İyi durumda, hazırlama modunda (etkin yapmadan), devre dışı olduğundan emin olun ve şu anda etkin sunucuyu kapatın.

Farklı veri merkezlerinde birden çok yedeklemelere sahip olmasını istediğiniz zaman birden fazla hazırlık sunucusu olması mümkündür.

## <a name="multiple-azure-ad-tenants"></a>Birden çok Azure AD kiracıları
Bir kuruluş için Azure AD'de tek bir kiracının sahip öneririz.
Birden çok Azure AD kiracılarıyla kullanmayı planlıyorsanız önce bkz [Azure AD'de idari Birim Yönetimi](../users-groups-roles/directory-administrative-units.md). Bu, tek bir kiracının kullanabileceğiniz yaygın senaryoları da kapsar.

![Birden çok ormanına ve birden fazla Kiracı için topoloji](./media/plan-connect-topologies/MultiForestMultiDirectory.png)

Bir Azure AD Connect eşitleme sunucusu ve Azure AD kiracısı arasında 1:1 ilişki yoktur. Her Azure AD kiracınız için bir Azure AD Connect eşitleme sunucusu yüklemesi gerekir. Azure AD Kiracı örneğinde, tasarımı gereği yalıtılır. Diğer bir deyişle, bir kiracıdaki kullanıcılar diğer kiracıdaki kullanıcılar göremez. Bu ayrım isterseniz, desteklenen bir yapılandırmadır. Aksi takdirde, tek kullanmalısınız kiracılı model Azure AD.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Her bir nesnenin yalnızca bir kez Azure AD kiracısı
![Tek orman filtrelenmiş topolojisi](./media/plan-connect-topologies/SingleForestFiltered.png)

Bu topolojide, bir Azure AD Connect eşitleme sunucusu her Azure AD kiracısına bağlı. Her birbirini dışlayan nesne üzerinde çalışılacak sahip olacak şekilde filtre uygulamak için Azure AD Connect eşitleme sunucusu yapılandırılmalıdır. Örneğin, belirli bir etki alanı ya da kuruluş birimi her sunucuya kapsamını olabilir.

Bir DNS etki alanı yalnızca tek bir kayıtlı Azure AD kiracısı. Şirket içi Active Directory örneğinde kullanıcıların UPN de ayrı ad alanları kullanmanız gerekir. Örneğin, önceki resimde, üç ayrı UPN soneki şirket içi Active Directory örneğinde kaydedilir: contoso.com ve fabrikam.com wingtiptoys.com. Her şirket içi Active Directory etki alanındaki kullanıcıların farklı bir ad kullanın.

>[!NOTE]
>Genel adres listesi eşitleme (GalSync) otomatik olarak bu topolojide yapılmaz ve bir ek özel MIM uygulama her kiracıya bir tam genel adres listesi'nı (GAL) Exchange Online'da sahip olduğundan emin olun ve Skype Kurumsal çevrimiçi gerektirir.


Bu topoloji, aşağıdaki sahip başka kısıtlamalar desteklenen senaryolar:

* Azure AD kiracılarıyla yalnızca biri, bir Exchange karma şirket içi Active Directory örneği ile etkinleştirebilirsiniz.
* Windows 10 cihazları tek bir Azure AD kiracınız ile ilişkili olabilir.
* Çoklu oturum açma (SSO) seçeneği parola karma eşitlemesi ve geçişli kimlik doğrulaması için yalnızca bir Azure AD kiracınız ile kullanılabilir.

Birbirini dışlayan nesne gereksinimini geri yazma için de geçerlidir. Bunlar tek şirket içi yapılandırma olduğunu varsaydığından geri yazma özelliklerinden bazıları bu topoloji ile desteklenmez. Bu özellikler şunlardır:

* Varsayılan yapılandırma ile grup geri yazma.
* Cihaz geri yazma.

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Her bir nesnenin birden çok kez Azure AD kiracısı
![Tek bir ormana ve birden fazla Kiracı için desteklenmeyen topolojisi](./media/plan-connect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Tek bir ormana ve birden çok bağlayıcı için desteklenmeyen topolojisi](./media/plan-connect-topologies/SingleForestMultiConnectorsUnsupported.png)

Bu görevler desteklenmez:

* Birden çok Azure AD kiracılarıyla aynı kullanıcıya eşitleyin.
* Bir yapılandırma kullanıcıların bir Azure AD kiracısında başka bir Azure AD kiracısında kişiler olarak görünecek biçimde değişikliği yapın.
* Birden çok Azure AD kiracıları için bağlanmak için Azure AD Connect eşitleme değiştirin.

### <a name="galsync-by-using-writeback"></a>Geri yazma özelliğini kullanarak GALSync
![Desteklenmeyen bir topoloji birden çok ormanı ve Azure AD üzerinde odaklanarak GALSync ile birden çok dizini](./media/plan-connect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![Desteklenmeyen bir topoloji birden çok ormanı ve odaklanarak GALSync ile birden çok dizin, üzerinde Active Directory şirket](./media/plan-connect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure AD kiracılarıyla tasarım gereği yalıtılır. Bu görevler desteklenmez:

* Başka bir Azure AD kiracısından verileri okumak için Azure AD Connect eşitleme yapılandırmasını değiştirin.
* Azure AD Connect eşitleme kullanarak başka bir şirket içi Active Directory örneğine kişiler olarak kullanıcıların verin.

### <a name="galsync-with-on-premises-sync-server"></a>Şirket içi eşitleme sunucusu ile GALSync
![Birden çok ormanına ve birden çok dizini topolojisinde GALSync](./media/plan-connect-topologies/MultiForestMultiDirectoryGALSync.png)

FIM 2010 veya MIM 2016'yı şirket içi (GALSync) aracılığıyla kullanıcıların iki Exchange kuruluş arasında eşitlemek için kullanabilirsiniz. Dış kullanıcılar/contacts diğer kuruluştaki bir kuruluştaki kullanıcılara görünür. Bu farklı şirket içi Active Directory örnekleri ardından kendi Azure AD kiracıları ile eşitlenebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu senaryolar için Azure AD Connect'i yükleme konusunda bilgi almak için bkz: [Azure AD Connect özel yüklemesi](how-to-connect-install-custom.md).

Daha fazla bilgi edinin [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırma.

Daha fazla bilgi edinin [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md).
