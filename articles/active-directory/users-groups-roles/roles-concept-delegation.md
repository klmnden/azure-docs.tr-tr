---
title: Azure Active Directory'de yönetici rolleri temsilci | Microsoft Docs
description: Temsilci modeli, örnekleri ve Azure Active Directory'de güvenlik rolü
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 08/09/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: ad772a2e0c036c30aca2918496ac01f31157fc64
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "40208579"
---
# <a name="delegate-administration-in-azure-active-directory"></a>Azure Active Directory'de yönetim temsilcisi

Daha fazla karmaşıklık kuruluş büyümesi ile birlikte gelir ve bazı Azure Active Directory (AD) yönetici rolleri erişim yönetimiyle yükü azaltmak için bir ortak yanıt olur. Mümkün olan en küçük ayrıcalık uygulamalarını erişmek ve görevlerini gerçekleştirmek için kullanıcılara atayabilirsiniz. Her uygulama sahibi için genel Yönetici rolüne atamak istemeyebilirsiniz ancak artırabilen uygulama yönetimi Sorumluluklar varolan genel Yöneticiler üzerine zorla olabilir. Daha fazla merkezi olmayan bir yönetim doğru bir kuruluş taşıma için birçok neden vardır. Bu makalede, kuruluşunuzdaki temsilci planlamanıza yardımcı olabilir.

<!--What about reporting? Who has which role and how do I audit?-->


## <a name="centralized-versus-delegated-permissions"></a>Temsilci izinleri karşı Merkezi

Bir kuruluş büyüdükçe, belirli yönetici rollerine hangi kullanıcıların olduğunu takip etmek zor olabilir. Bir çalışan bunlar olmamalıdır yönetici hakları varsa bir kuruluşun güvenlik ihlallerini maruz olabilir. Genel olarak, yönetici sayısını ve ayrıntı düzeyi izinlerini, boyutu ve dağıtım karmaşıklığı bağlıdır.

* Küçük veya kavram kanıtı dağıtımlarda, bir veya birkaç yöneticiler her şeyi yapabilirsiniz; Temsilci yoktur. Bu durumda, her yöneticinin genel yönetici rolü oluşturun.
* Daha fazla makineleri, uygulamalar ve Masaüstü ile büyük dağıtımlarda, daha fazla temsilci gereklidir. Birçok yönetici daha belirli işlevsel sorumlulukları (roller) olabilir. Örneğin, bazı ayrıcalıklı kimlik yöneticileri olabilir ve diğer uygulama yöneticileri olabilir. Ayrıca, yöneticinin yalnızca belirli cihazlar gibi nesne gruplarını yönetin.
* Daha da büyük dağıtımları daha da ayrıntılı izinler yanı sıra büyük olasılıkla yöneticilerine sıra dışı veya karma rolleri gerektirebilir.

Azure AD portalında yapabilecekleriniz [herhangi rolünün tüm üyelerinin görüntülemek](directory-manage-roles-portal.md), yardımcı olabilecek hızlı dağıtım ve temsilci izinleri denetleyin.

Azure kaynaklarına erişimi ve Azure AD'de değil yönetimsel erişim için temsilci seçme içinde ilgileniyorsanız bkz [rol tabanlı erişim denetimi (RBAC) rolüne atamak](../../role-based-access-control/role-assignments-portal.md).

## <a name="delegation-planning"></a>Temsilci planlama

Hurdle benzersiz kuruluşunuzun gereksinimlerine en uygun bir temsilci modeli geliştirmek için olsa da gerçekte çok az değişiklikle uygulanabilir çok basit modelleri yoktur. Bir temsilci modeli geliştirmek bir yinelemeli tasarım işlemdir ve aşağıdaki adımları öneririz:

* Gereksinim duyduğunuz rolleri tanımlama
* Temsilci uygulama yönetimi
* Uygulamaları kaydetme hakkı
* Uygulama sahipliğini devretme
* Güvenlik planı geliştirme
* Acil Durum hesapları oluştur
* Güvenli, yönetici rolleri
* Ayrıcalıklı yükseltme geçici olun

## <a name="define-roles"></a>Rolleri tanımlama

Hangi Active Directory görevleri yöneticileri tarafından gerçekleştirilen ve bu görevlerin rollere nasıl eşleneceğini belirler. Örneğin, güvenlik grubu üyeliğinin değiştirilmesini genellikle veri yönetimi altında düştüğünde sırasında Active Directory site oluşturma bir hizmet yönetim görevi ' dir. Yapabilecekleriniz [ayrıntılı rol açıklamalarını görüntülemek](directory-manage-roles-portal.md) Azure portalında.

Her görev, sıklığı, önem derecesi ve zorluk değerlendirilmelidir. İzin temsilcisi olup, yönetmek için görev tanımı önemli yönleri şunlardır. Görevlerinin tamamlanması için düzenli olarak gerçekleştirilir, sınırlı risk ve önemsiz temsilci seçme için iyi adaylar olduğu. Öte yandan, nadiren gerçekleştirilir ancak kuruluş genelinde harika bir etkiye sahip ve yüksek insanları gerektiren görevleri çok dikkatli bir şekilde temsilci olarak görevlendirme önce düşünülmelidir. Bunun yerine, [geçici olarak bir hesap için gerekli olan rol yükseltmesine](../active-directory-privileged-identity-management-configure.md) veya görev yeniden atayın.

## <a name="delegate-app-administration"></a>Temsilci uygulama yönetimi

Kuruluşunuzdaki uygulamaları çoğalan temsilci modelinizin zorlayabilir. Genel yönetici yükünü uygulama erişim yönetimi için yerleştirir, Zaman geçtikçe, model, yükü artırır olasıdır. Kişiler gibi Kurumsal uygulamaları için yapılandırma öğeleri için genel yönetici rolü verdiyseniz, bunların artık daha az ayrıcalıklı rollerde boşaltabilirsiniz. Bunun yapılması, güvenlik duruşunuzu artırmaya yardımcı olur ve talihsiz hataları olasılığını azaltır. Çoğu ayrıcalıklı uygulama Yönetici rolü şunlardır:

* **Uygulama Yöneticisi** , dizinde kayıtları, çoklu oturum açma ayarları, kullanıcı ve Grup atamalarını ve lisanslama, uygulama proxy'si ayarları dahil olmak üzere tüm uygulamaları yönetme olanağı veren rolü ve onay. Koşullu erişim yönetme olanağı tanımaz.
* **Bulut uygulaması Yöneticisi** (hiçbir şirket içi izni olduğundan), erişim için uygulama proxy'si ayarları tanımaz dışında tüm becerileri, uygulama Yöneticisi, veren rol. ### temsilci uygulama sahibi izinlerini uygulama başına

## <a name="delegate-app-registration"></a>Temsilci uygulama kaydı

Varsayılan olarak, tüm kullanıcılar, uygulama kayıtları oluşturabilir. Seçerek uygulama kayıtları oluşturma yeteneği vermek istiyorsanız, ayarlamak zorunda kalırsınız **kullanıcılar uygulamaları kaydedebilir** için kullanıcı ayarlarını ve ardından Ata uygulama geliştiricisi rol yok. Bu rol, uygulama kayıtları oluşturma yeteneği verir. yalnızca **kullanıcılar uygulamaları kaydedebilir** devre dışıdır. Uygulama geliştiricileri de onay kendileri için zaman **kullanıcılar uygulamaları kendileri adına şirket verilerine erişme izni verebilir** Hayır olarak ayarlayın Bir uygulama geliştiricisi, yeni bir uygulama kaydı oluşturduğunda, bunlar ilk sahibi olarak otomatik olarak eklenir.

## <a name="delegate-app-ownership"></a>Uygulama sahipliğini devretme

Uygulama bile dağıtımına erişim temsilci seçme için ayrı Kurumsal uygulamalara sahipliği atayabilirsiniz. Bu uygulama kayıt sahiplerini atamak için mevcut destek tamamlar. Sahiplik, kurumsal uygulamalar dikey penceresinde her Kurumsal uygulama temelinde atanır. Sahipler oldukları Kurumsal uygulamaları yönetebilir avantajdır. Örneğin, Salesforce uygulama için bir sahip atayabilir ve bu sahibi, Salesforce ve başka bir uygulama için yapılandırma ve erişimi yönetebilir. Kurumsal bir uygulamanın birçok sahipleri olabilir ve birçok kurumsal uygulamalar için sahip bir kullanıcı olabilir. İki uygulama sahibinin rol vardır:

* **Kurumsal uygulama sahibi** rol yönetme olanağı veren ' çoklu oturum açma ayarları, kullanıcı ve Grup atamaları dahil ve ek sahipleri ekleme kullanıcı sahip kurumsal uygulamalar. Uygulama proxy'si ayarları veya koşullu erişim yönetme olanağı tanımaz.
* **Uygulama kayıt sahibi** rol için ek sahipleri ekleme ve uygulama bildirimi dahil olmak üzere kullanıcı sahip app uygulama kayıtları yönetme olanağı verir.

## <a name="develop-a-security-plan"></a>Güvenlik planı geliştirme

Azure AD, planlama ve Azure AD yönetim rolleri üzerinde bir güvenlik planı yürütme için kapsamlı bir kılavuz sağlar [ayrıcalıklı erişimin güvenliğini sağlama ve karma bulut dağıtımları için](directory-admin-roles-secure.md). 

## <a name="establish-emergency-accounts"></a>Acil Durum hesapları oluştur

Sorun ortaya çıktığında, kimlik yönetimi deposuna erişimi sürdürmek istiyorsanız, Acil Durum erişim hesapları'na göre hazırlama [Acil Durum erişimi yönetici hesapları oluşturma](directory-emergency-access.md).

## <a name="secure-your-administrator-roles"></a>Güvenli, yönetici rolleri

Ayrıcalıklı hesapların denetim elde saldırganlar Bilim insanları için inanılmaz zarar yapın, böylece bu hesapların ilk olarak kullanarak korumak [temel erişim ilkesi](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/22/baseline-security-policy-for-azure-ad-admin-accounts-in-public-preview/) kullanılabilir varsayılan olarak tüm Azure AD kiracıları için (genel Önizleme aşamasında). Çok faktörlü kimlik doğrulama ilkeleri uygular üzerinde ayrıcalıklı Azure AD hesaplar. Aşağıdaki Azure AD rolleri Azure AD temel ilke tarafından ele alınmaktadır:

* Genel yönetici
* SharePoint yöneticisi
* Exchange yöneticisi
* Koşullu erişim yöneticisi
* Güvenlik yöneticisi

## <a name="elevate-privilege-temporarily"></a>Ayrıcalık geçici olarak Yükselt

Çoğu günlük etkinlikler için tüm kullanıcıların genel yönetici hakları gerekir. Ve tüm kullanıcılar kalıcı olarak genel Yönetici rolüne atanması gerekir. Genel yönetici olarak görev yapacak kullanıcıları gerektiğinde, Azure AD'de rol atama etkinleştirmelisiniz [Privileged Identity Management](../active-directory-privileged-identity-management-configure.md) kendi hesabını veya başka bir yönetici hesabı.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD rol açıklamalarını başvuru için bkz: [Azure AD'de yönetici rolleri atama](directory-assign-admin-roles.md)