---
title: 'Azure AD Connect: destekli topolojileri | Microsoft Docs'
description: "Bu konuda, Azure AD Connect için desteklenen ve desteklenmeyen topolojileri ayrıntıları"
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 1034c000-59f2-4fc8-8137-2416fa5e4bfe
ms.service: active-directory
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 50cf58c7d2d9be4644ada4feae02d0d5219a3fd6
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="topologies-for-azure-ad-connect"></a>Azure AD Connect için topolojiler
Bu makalede, çeşitli şirket içi ve Azure AD Connect eşitleme anahtar tümleştirme çözümü olarak kullanan Azure Active Directory (Azure AD) topolojileri açıklanır. Bu makalede, desteklenen ve desteklenmeyen yapılandırmalar içerir.

Makaleyi resimleri için gösterge şöyledir:

| Açıklama | Simgesi |
| --- | --- |
| Şirket içi Active Directory ormanı |![Şirket içi Active Directory ormanı](./media/active-directory-aadconnect-topologies/LegendAD1.png) |
| Şirket içi Active Directory ile filtrelenmiş alma |![Active Directory ile filtrelenmiş alma](./media/active-directory-aadconnect-topologies/LegendAD2.png) |
| Azure AD Connect eşitleme sunucusu |![Azure AD Connect eşitleme sunucusu](./media/active-directory-aadconnect-topologies/LegendSync1.png) |
| "Hazırlama modu" azure AD Connect eşitleme sunucusu |!["Hazırlama modu" azure AD Connect eşitleme sunucusu](./media/active-directory-aadconnect-topologies/LegendSync2.png) |
| Forefront Identity Manager (FIM) 2010 veya Microsoft Identity Manager (MIM) 2016 ile GALSync |![FIM 2010 veya MIM 2016 ile GALSync](./media/active-directory-aadconnect-topologies/LegendSync3.png) |
| Ayrıntılı azure AD Connect eşitleme sunucusu |![Ayrıntılı azure AD Connect eşitleme sunucusu](./media/active-directory-aadconnect-topologies/LegendSync4.png) |
| Azure AD |![Azure Active Directory](./media/active-directory-aadconnect-topologies/LegendAAD.png) |
| Desteklenmeyen senaryo |![Desteklenmeyen senaryo](./media/active-directory-aadconnect-topologies/LegendUnsupported.png) |

## <a name="single-forest-single-azure-ad-tenant"></a>Tek bir orman, tek bir Azure AD kiracısı
![Tek bir ormana ve tek bir kiracı için topoloji](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

Tek bir orman, bir veya birden çok etki alanı ve tek bir Azure AD Kiracı şirket içi en sık kullanılan topolojidir. Azure AD kimlik doğrulaması için parola eşitleme kullanılır. Azure AD Connect hızlı yükleme yalnızca bu topoloji destekler.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Tek orman, birden çok eşitleme sunucusu için bir Azure AD kiracısı
![Desteklenmeyen, filtrelenmiş topolojisi tek orman](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

Birden çok Azure AD Connect eşitleme sunucusu için aynı Azure AD kiracısı bağlı olması desteklenmiyor, dışında bir [server hazırlama](#staging-server). Bu sunucular ile birbirini dışlayan bir nesneler kümesini eşitlemek için yapılandırılmış olsa bile desteklenmeyen sahip. Tek bir sunucudan ormandaki tüm etki alanları erişemezse ya da yük çeşitli sunucular arasında dağıtmak istiyorsanız bu topoloji kabul.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Birden çok orman, tek bir Azure AD kiracısı
![Birden çok orman ve tek bir kiracı için topoloji](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

Çoğu kuruluş ortamları ile birden çok şirket içi Active Directory ormanı sahiptir. Birden fazla şirket içi Active Directory ormanına sahip olmak için çeşitli nedenleri vardır. Tasarımlar hesap-kaynak ormanına ve birleşme veya alım sonucu ile tipik örnekleridir.

Birden çok orman, tüm ormanlardaki olduğunda tek bir tarafından erişilebilir olmalıdır Azure AD Connect eşitleme sunucusu. Sunucu bir etki alanına sahip değilsiniz. Gerekirse tüm ormanlarda ulaşmak, sunucuyu bir çevre ağında (DMZ, sivil bölge ve denetimli alt ağ olarak da bilinir) yerleştirebilirsiniz.

Azure AD Connect Yükleme Sihirbazı'nı birden fazla ormanda temsil kullanıcılar birleştirmek için çeşitli seçenekler sağlar. Bir kullanıcı, Azure AD içinde yalnızca bir kez temsil edilir hedeftir. Yükleme Sihirbazı'nda özel yükleme yolundaki yapılandırabilirsiniz bazı ortak topolojileri vardır. Üzerinde **kullanıcılarınızı benzersiz olarak tanımlama** sayfasında, topolojinizi temsil eden ilgili seçeneği seçin. Birleştirme yalnızca kullanıcılar için yapılandırılır. Varsayılan yapılandırmada yinelenen grupları birleştirilmiş değil.

Ortak topolojileri hakkında bölümlerde açıklanan [ayrı topolojileri](#multiple-forests-separate-topologies), [tam mesh](#multiple-forests-full-mesh-with-optional-galsync), ve [hesap-kaynak topoloji](#multiple-forests-account-resource-forest).

Azure AD Connect eşitleme Varsayılan yapılandırmada varsayılır:

* Her kullanıcının yalnızca bir etkin hesaba sahip ve bu hesabın bulunduğu orman kullanıcının kimliğini doğrulamak için kullanılır. Bu, hem parola eşitleme hem de Federasyon için varsayılır. UserPrincipalName ve sourceAnchor/İmmutableıd bu ormandan gelir.
* Her kullanıcının yalnızca bir posta kutusu vardır.
* Bir kullanıcının posta kutusunu barındıran ormanda Exchange Genel adres listesi (GAL) içinde görünür öznitelikler için en iyi veri kalitesini vardır. Kullanıcı için hiçbir posta kutusu varsa, bu öznitelik değerlerini katkıda bulunmak için herhangi bir orman kullanılabilir.
* Bağlı bir posta kutusu varsa, yoktur da bir hesap oturum açma için kullanılan farklı bir ormanda.

Ortamınızı bu varsayımları eşleşmiyorsa, aşağıdaki durumlar ortaya çıkar:

* Birden fazla etkin hesabı ya da birden fazla posta kutusu varsa, eşitleme altyapısı birini seçer ve diğer yok sayar.
* Başka bir etkin hesabı bağlı posta kutusu Azure AD'ye aktarılan değil. Kullanıcı hesabının herhangi bir grubu üye olarak gösterilmiyor. DirSync bağlı bir posta kutusu her zaman normal bir posta kutusu olarak temsil edilir. Bu kasıtlı olarak daha iyi birden çok orman senaryoları desteklemek için farklı bir davranış farklıdır.

Daha ayrıntılı bilgi bulabilirsiniz [varsayılan yapılandırmayı anlama](active-directory-aadconnectsync-understanding-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Birden çok orman, birden çok eşitleme sunucusu için bir Azure AD kiracısı
![Birden fazla ormanına ve birden çok eşitleme sunucusu için desteklenmeyen topolojisi](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

Birden fazla Azure AD Connect eşitleme sunucusu tek bir bağlı olan Azure AD kiracısı desteklenmiyor. Özel durum kullanımıdır bir [server hazırlama](#staging-server).

### <a name="multiple-forests-separate-topologies"></a>Birden çok orman, ayrı topolojileri
![Kullanıcıların yalnızca bir kez temsil eden tüm dizinlerde seçeneği](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![Birden çok orman ve ayrı topolojileri gösterimi](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

Bu ortamda, tüm şirket içi ormanları ayrı varlıklar olarak kabul edilir. Hiçbir kullanıcı, başka bir ormanda mevcuttur. Her ormanda kendi Exchange kuruluşu vardır ve ormanlar arasında hiçbir GALSync yoktur. Bu topoloji, burada her iş birimi bağımsız olarak çalışır bir birleşme/edinme sonra veya bir kuruluşta durum olabilir. Bu ormanlarda Azure AD aynı kuruluşta bulunan ve birleşik GAL ile görünür. Yukarıdaki resimde, her ormanda her nesnenin meta veri deposunda bir kez temsil ve hedef Azure AD kiracısı'nda bir araya getirilir.

### <a name="multiple-forests-match-users"></a>Birden çok orman: eşleşen kullanıcıları
Bu senaryolar için dağıtım yaygındır ve güvenlik grupları kullanıcılar, kişiler ve yabancı güvenlik sorumlusu (FSP) bir karışımını içerebilir. FSP Active Directory Etki Alanı Hizmetleri'nde (AD DS) bir güvenlik grubundaki diğer ormanlardaki üyeleri göstermek için kullanılır. Tüm FSP Azure AD'de gerçek nesnesine çözümlenir.

### <a name="multiple-forests-full-mesh-with-optional-galsync"></a>Birden çok orman: tam mesh isteğe bağlı GALSync ile
![Kullanıcı kimlikleri birden fazla dizinde bulunur zaman eşleştirme için posta özniteliğinin kullanma seçeneği](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![Birden çok orman için tam ağ topolojisi](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

Tam ağ topolojisi kullanıcılar ve kaynaklar içinde herhangi bir orman bulunmasını sağlar. Genellikle, ormanlar arasında iki yönlü güven vardır.

Birden fazla ormanda Exchange varsa olabilir (isteğe bağlı) bir şirket içi GALSync çözümü. Her kullanıcı ardından diğer tüm ormanlarda kişi olarak temsil edilir. GALSync sık FIM 2010 veya MIM 2016 aracılığıyla uygulanır. Azure AD Connect, şirket içi GALSync için kullanılamaz.

Bu senaryoda, posta özniteliğinin kimlik nesneleri birleştirilir. Bir posta kutusunu bir ormanda olan bir kullanıcı, diğer ormanlardaki kişilerle birleştirilir.

### <a name="multiple-forests-account-resource-forest"></a>Birden çok orman: hesap-kaynak ormanı
![Kimlikleri birden fazla dizinde bulunur zaman eşleştirme için objectSID ve msExchMasterAccountSID öznitelikleri kullanma seçeneği](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![Birden çok orman için hesap-kaynak ormanı topolojisi](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

Bir veya daha fazla sahip bir hesap-kaynak ormanı topolojisinde *hesap* etkin kullanıcı hesapları ile ormanlar. Bir veya daha fazla de sahip *kaynak* devre dışı bırakılan hesapları ormanlar.

Bu senaryoda, tüm hesap ormanları bir (veya daha fazla) kaynak orman güvenleri. Kaynak orman genellikle Exchange ve Lync ile genişletilmiş bir Active Directory şeması vardır. Tüm Exchange ve Lync Hizmetleri, diğer paylaşılan hizmetler yanı sıra, bu ormanda yer alır. Kullanıcılar devre dışı bırakılmış kullanıcı hesabı, bu ormandaki sahip ve posta kutusu hesabı ormana bağlanır.

## <a name="office-365-and-topology-considerations"></a>Office 365 ve topolojisi hakkında önemli noktalar
Bazı Office 365 iş yükleri üzerinde desteklenen topolojiler bazı kısıtlamalar vardır:

| İş yükü | Kısıtlamalar |
--------- | ---------
| Exchange Online | Exchange Online tarafından desteklenen karma topolojiler hakkında daha fazla bilgi için bkz: [birden çok Active Directory ormanına karma dağıtımlarında](https://technet.microsoft.com/library/jj873754.aspx). |
| Skype Kurumsal | Birden çok şirket içi ormanları kullanırken, yalnızca hesap-kaynak orman topolojisini desteklenir. Daha fazla bilgi için bkz: [Business Server 2015 için Skype ortam gereksinimleri](https://technet.microsoft.com/library/dn933910.aspx). |


## <a name="staging-server"></a>Hazırlama sunucusu
![Hazırlama sunucu topoloji](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

İkinci bir sunucu yüklemeyi azure AD Connect destekler *hazırlama modu*. Bu modundaki bir sunucu, tüm bağlı dizinlerden verileri okur ancak hiçbir şey bağlı dizinlere yazmaz. Normal eşitleme döngüsü kullanır ve bu nedenle kimlik verilerini güncelleştirilmiş bir kopyasını sahiptir.

Burada birincil sunucu başarısız bir felaket içinde üzerinden hazırlama sunucusuna yük devredebilirsiniz. Bu Azure AD Connect Sihirbazı'nda yapın. Birincil sunucu ile hiçbir altyapı paylaşıldığından bu ikinci sunucuyu farklı bir veri merkezinde yer alabilir. İkinci sunucuyu birincil sunucuya yapılan herhangi bir yapılandırma değişikliği el ile kopyalamanız gerekir.

Yeni bir özel yapılandırma ve verilerinizi olan etkisini test etmek için bir hazırlama sunucu kullanabilirsiniz. Önizleme değişiklikleri ve yapılandırmasını ayarlayın. Yeni yapılandırmayla memnun kaldığınızda, hazırlama sunucuyu active server yapmak ve eski active server hazırlama modu olarak ayarlanmış.

Bu yöntem, active eşitleme sunucusunu değiştirmek için de kullanabilirsiniz. Yeni bir sunucu hazırlayın ve hazırlama modu olarak ayarlanmış. İyi durumda, hazırlama modu (etkin hale), devre dışı olduğundan emin olun ve şu anda etkin sunucuyu kapatın.

Birden çok yedekleme farklı veri merkezlerinde sahip istediğinizde birden fazla hazırlama sunucu olması mümkündür.

## <a name="multiple-azure-ad-tenants"></a>Birden çok Azure AD kiracılarıyla
Bir kuruluş için Azure AD'de tek bir kiracı olması önerilir.
Birden çok Azure AD kiracılarıyla kullanmayı planlıyorsanız önce makalesine bakın [Azure AD'de idari Birim Yönetimi](../active-directory-administrative-units-management.md). Tek bir kiracı burada kullanabileceğiniz ortak senaryolar kapsar.

![Birden fazla ormanına ve birden çok Kiracı için topoloji](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

Bir Azure AD Connect eşitleme sunucusu ve Azure AD kiracısı arasında 1:1 ilişkisi yoktur. Her bir Azure AD Kiracı için bir Azure AD Connect eşitleme sunucusu yüklemesi gerekir. Azure AD Kiracı örnekleri tasarım gereği yalıtılır. Diğer bir deyişle, bir kiracı kullanıcılar diğer Kiracı kullanıcılar göremezsiniz. Bu ayrım istiyorsanız, desteklenen bir yapılandırmadır. Aksi durumda, tek kullanmanız gereken Azure AD Kiracı modeli.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Her nesne yalnızca bir kez Azure AD kiracısı
![Tek bir orman için filtrelenmiş topolojisi](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

Bu topolojide, her bir Azure AD Kiracı için bir Azure AD Connect eşitleme sunucusu bağlandı. Böylece her dışlayan bir küme üzerinde çalışan nesnelerin sahip Azure AD Connect eşitleme sunucuları filtreleme için yapılandırılmalıdır. Örneğin, her sunucunun belirli bir etki alanı veya kuruluş birimi kapsamını olabilir.

Bir DNS etki alanı yalnızca tek bir kaydedilebilir Azure AD kiracısı. Kullanıcıların şirket içi Active Directory örneğinde UPN'ler ayrıca ayrı ad alanları kullanmanız gerekir. Örneğin, önceki resimde, üç ayrı UPN soneki şirket içi Active Directory örneğinde kayıtlı: contoso.com, fabrikam.com ve wingtiptoys.com. Her şirket içi Active Directory etki alanındaki kullanıcıların farklı bir ad kullanın.

Azure AD Kiracı örnekleri arasında hiçbir GALSync yoktur. Adres Defteri Exchange Online ve Skype iş gösterir yalnızca kullanıcılar aynı Kiracı için.

Bu topoloji aşağıdaki sahip başka kısıtlamalar desteklenen senaryolar:

* Azure AD kiracılarıyla yalnızca biri şirket içi Active Directory örneğiyle bir Exchange karma etkinleştirebilirsiniz.
* Windows 10 cihazları tek bir Azure AD Kiracı ile ilişkilendirilebilir.
* Çoklu oturum açma (SSO) seçeneği parola eşitleme ve geçişli kimlik doğrulaması için yalnızca bir Azure AD Kiracı ile kullanılabilir.

Birbirini dışlayan bir nesneler kümesini gereksinimini geri yazma için de geçerlidir. Tek şirket içi yapılandırma varsayar çünkü geri yazma özelliklerinden bazıları bu topolojisi ile desteklenmez. Bu özellikler şunlardır:

* Grup geri yazma varsayılan yapılandırmaya sahip.
* Cihaz geri yazma.

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Her nesne birden çok kez Azure AD kiracısı
![Tek bir ormanına ve birden çok Kiracı için desteklenmeyen topolojisi](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Tek bir ormanına ve birden çok bağlayıcı için desteklenmeyen topolojisi](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

Bu görevler desteklenmez:

* Birden çok Azure AD kiracılarıyla aynı kullanıcıya eşitleyin.
* Bir yapılandırma bir Azure AD Kiracı kullanıcılar başka bir Azure AD kiracısı ilgili kişi olarak görünmesini sağlayacak şekilde değişikliği yapın.
* Birden çok Azure AD kiracılarıyla bağlanmak için Azure AD Connect eşitleme değiştirin.

### <a name="galsync-by-using-writeback"></a>Geri yazma kullanarak GALSync
![Birden çok orman ve üzerinde Azure AD odaklanan GALSync ile birden çok dizin için desteklenmeyen topolojisi](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![Birden çok orman ve GALSync odaklanan ile birden çok dizin için desteklenmeyen topoloji üzerinde Active Directory şirket içi](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure AD kiracılarıyla tasarım gereği yalıtılır. Bu görevler desteklenmez:

* Başka bir Azure AD kiracısı verileri okumak için Azure AD Connect eşitleme yapılandırmasını değiştirin.
* Kullanıcılar başka bir şirket içi Active Directory örneğine kişiler olarak Azure AD Connect eşitleme kullanarak dışarı aktarın.

### <a name="galsync-with-on-premises-sync-server"></a>Şirket içi eşitleme sunucusu ile GALSync
![Birden fazla ormanına ve birden çok dizin için topoloji GALSync](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

FIM 2010 veya MIM 2016 şirket içi (aracılığıyla GALSync) kullanıcıların iki Exchange kuruluş arasında eşitlemek için kullanabilirsiniz. Bir kuruluştaki kullanıcılar diğer kuruluştaki yabancı kullanıcıları/kişiler olarak görünür. Bu farklı şirket içi Active Directory örnekleri sonra kendi Azure AD kiracılar ile eşitlenebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu senaryolar için Azure AD Connect'i yüklemek öğrenmek için bkz: [Azure AD Connect özel yüklemesi](active-directory-aadconnect-get-started-custom.md).

Daha fazla bilgi edinmek [Azure AD Connect eşitleme](active-directory-aadconnectsync-whatis.md) yapılandırma.

Daha fazla bilgi edinmek [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md).
