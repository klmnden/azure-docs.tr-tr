---
title: Azure kimlik ve erişim en iyi güvenlik yöntemleri | Microsoft Docs
description: Bu makalede bir dizi kimlik yönetimi için en iyi yöntemler ve erişim denetimi Azure özellikleri kullanılarak.
services: security
documentationcenter: na
author: barclayn
manager: mbaldwin
editor: TomSh
ms.assetid: 07d8e8a8-47e8-447c-9c06-3a88d2713bc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/26/2018
ms.author: barclayn
ms.openlocfilehash: e3fe033de05ed42d221795159461048790e1cec8
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39493311"
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Azure kimlik yönetimi ve erişim en iyi güvenlik denetimi

Birçok yeni bir sınır katman bu role geleneksel ağ merkezli perspektifinden ele güvenlik, kimlik göz önünde bulundurun. Bu birincil pivot evrimi güvenlik dikkat ve yatırımlarınızı ağ duvarlar hale giderek porous ve bu çevre savunması kez olarak etkin olamaz olgu gelen için önce açılımı olan [KCG ](http://aka.ms/byodcg) cihazlar ve bulut uygulamaları.

Bu makalede, Azure kimlik yönetimi ve erişim denetimi en iyi güvenlik uygulamaları koleksiyonu ele alır. Bu en iyi uygulamaları ile deneyimimizi türetilmiştir [Azure AD'ye](../active-directory/fundamentals/active-directory-whatis.md) ve müşteri deneyimleri bulunun.

En iyi her uygulama için biz açıklar:

* En iyi nedir
* Bu en iyi etkinleştirmek istediğiniz neden
* En iyi etkinleştirme başarısız olursa ne sonuç olabilir
* En iyi olası alternatifler
* Nasıl en iyi etkinleştirmek bilgi edinebilirsiniz

Bu makale, bu Azure kimlik yönetimi ve erişim denetimi güvenliği zaman oldukları gibi en iyi yöntemler makalesi fikir birliğine varılmış fikrim ve Azure platformu özellikleri ve özellik kümeleri dayalı olarak yazılmıştır. Fikirlerini ve teknolojileri zamanla değişir ve bu makalede, bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

Bu makalede ele alınan azure kimlik yönetimi ve erişim denetimi güvenlik en iyi uygulamalar şunlardır:

* Kimlik yönetimini merkezileştirin
* Çoklu oturum açma (SSO) etkinleştir
* Parola yönetimini dağıtma
* Kullanıcılar için multi-Factor authentication (MFA) zorunlu
* Rol tabanlı erişim denetimi (RBAC) kullanın
* Kaynak Yöneticisi'ni kullanarak kaynakların oluşturulabileceği denetim konumları
* SaaS uygulamaları için kimlik özelliklerinden yararlanmak için Geliştirici Kılavuzu
* Şüpheli etkinlikler için etkin bir şekilde izleyin

## <a name="centralize-your-identity-management"></a>Kimlik yönetimini merkezileştirin

Kimliğinizi güvenliğini sağlamaya yönelik bir en önemli adımlardan biri, BT'nin emin olmak için bu hesabın oluşturulduğu yere ilişkin tek bir konumdan hesapları yönetin. Kendi birincil hesap şirket içi dizin ve kuruluşların BT kuruluşlarının büyük çoğunluğu varken, karma bulut dağıtımları yükselişe ve şirket içi tümleştirme ve sorunsuz bir sağlayan bulut dizinlerinizde nasıl anlamak önemlidir için son kullanıcı deneyimi.

Bunu gerçekleştirmek için [karma kimlik](../active-directory/active-directory-hybrid-identity-design-considerations-overview.md) senaryo iki seçenek öneririz:

* Şirket içi dizininizi Azure AD Connect kullanılarak bulut dizininizle eşitleme
* Çoklu oturum açma ile etkinleştirme [parola karması eşitleme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization), [geçişli kimlik doğrulaması](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq) veya şirket içi kimlik bilgilerinizi kullanarak bulut dizini ile federasyona [Active Directory Federasyon Hizmetleri](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/deploying-federation-servers) (AD FS)

Şirket içi kimliklerini bulut kimliklerini ile tümleştirmek için başarısız olan kuruluşlar, hatalar ve güvenlik ihlallerini olasılığını artıran hesaplarını yönetme, yönetim iş yükünün artmasına karşılaşırsınız.

Azure AD eşitleme hakkında daha fazla bilgi için bkz [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../active-directory/active-directory-aadconnect.md).

## <a name="enable-single-sign-on-sso"></a>Çoklu oturum açma (SSO) etkinleştir

Yönetmek için birden çok dizini varsa, bu değil yalnızca yönetimsel bir sorun haline BT, birden fazla parolayı hatırlamak zorunda son kullanıcılar için de. Kullanarak [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) yeteneğini oturum açın ve, bundan bağımsız olarak burada bu kaynağı şirket içinde veya bulutta ihtiyaç duydukları kaynaklara erişmek için aynı kimlik bilgileri kümesini kullanın, kullanıcılarınızın sağlayın.

SSO kullanıcıların erişim sağlamak için kullanın, [SaaS uygulamalarına](../active-directory/manage-apps/what-is-single-sign-on.md) Azure AD'de kuruluş hesabını temel. Bu yalnızca Microsoft SaaS uygulamaları, ancak diğer uygulamalar için de geçerli olduğu gibi [Google Apps](../active-directory/saas-apps/google-apps-tutorial.md) ve [Salesforce](../active-directory/saas-apps/salesforce-tutorial.md). Uygulama olarak Azure AD'yi kullanacak şekilde yapılandırılabilir bir [SAML tabanlı kimlik](../active-directory/fundamentals-identity.md) sağlayıcısı. Güvenlik denetimi gibi Azure AD uygulamasına Azure AD kullanarak erişim verilmiş sürece oturum için bir belirteç yayınlamayacaktır. Doğrudan veya bir grup üyesi oldukları erişim.

> [!NOTE]
> SSO karar, şirket içi dizininizi bulut dizininizle tümleştirmek etkiler. SSO istiyorsanız, dizin eşitleme yalnızca sağlayacağından, Federasyon, kullanmanız gerekecektir [aynı oturum açma deneyimi](../active-directory/active-directory-aadconnect.md).
>
>

SSO, kullanıcılar ve uygulamalar için zorlama kuruluşlar, doğrudan parolaları yeniden kullanma veya zayıf parolalarını kullanarak kullanıcı olasılığını artırır kullanıcıların sahip olduğu birden çok parola senaryoları için daha kullanıma sunulur.

Makaleyi okuyarak Azure AD SSO hakkında daha fazla bilgi edinebilirsiniz [AD FS yönetimi ve Azure AD Connect ile özelleştirme](../active-directory/active-directory-aadconnect-federation-management.md).

## <a name="deploy-password-management"></a>Parola yönetimini dağıtma

Burada birden çok kiracıya sahip olduğunuz veya kullanıcılara etkinleştirmek istediğiniz senaryolarda [kendi parolalarını sıfırlama](../active-directory/user-help/active-directory-passwords-update-your-own-password.md), kötüye kullanımı önlemek için uygun güvenlik ilkelerini kullanmak önemlidir. Azure'da, Self Servis parola sıfırlama özelliği yararlanın ve güvenlik seçenekleri, iş gereksinimlerinizi karşılayacak şekilde özelleştirin.

Bu kullanıcılardan geri bildirim almak ve aşağıdaki adımları gerçekleştirmek çalışırken kendi deneyimleri öğrenmek önemlidir. Bu deneyimler bağlı olarak, daha büyük bir grup için dağıtım sırasında oluşabilecek olası sorunları gidermek için bir plan özenli. Ayrıca, kullanmanız önerilir [parola sıfırlama kayıt Etkinlik Raporu](../active-directory/active-directory-passwords-get-insights.md) kaydettirmekte olduğunuz kullanıcıları izlemek için.

Parola değişikliği destek aramalarını engellemek istiyorsanız ancak kendi parolalarını sıfırlamalarına olanak tanıma kuruluşlara, hizmet Masası parola sorunları nedeniyle daha yüksek bir çağrı birime daha açıktır. Birden çok kiracıya sahip kuruluşlarda, bu tür bir özelliği ve parola Güvenlik İlkesi'nde oluşturulan güvenlik sınırları içinde sıfırlama gerçekleştirmelerini sağlama zorunludur.

Parola sıfırlama makaleyi okuyarak hakkında daha fazla bilgi [parola yönetimini dağıtma ve kullanıcılara eğitim verme kullanılacağını](../active-directory/authentication/howto-sspr-deployment.md).

## <a name="enforce-multi-factor-authentication-mfa-for-users"></a>Kullanıcılar için multi-Factor authentication (MFA) zorunlu

Gibi sektör standartları ile uyumlu olması gereken kuruluşlar için [PCI DSS sürümü 3.2](http://blog.pcisecuritystandards.org/preparing-for-pci-dss-32), multi-Factor authentication, şart için kullanıcıların kimlik doğrulaması özelliğine sahip. Sektör standartları ile uyumlu olmasının ötesinde, kullanıcıların kimliğini doğrulamak için MFA zorlama da gibi kimlik bilgisi hırsızlığı türü, saldırının etkisini azaltmak için kuruluşlara yardımcı olabilir [Pass--Hash (PtH)](http://aka.ms/PtHPaper).

Kullanıcılarınız için Azure mfa'yı sağlayarak, kullanıcı oturum açmaları ve işlemleri için ikinci bir güvenlik katmanı ekliyoruz. Bu durumda, bir işlem bir dosya sunucusunda veya SharePoint Online bulunan bir belgeye erişme. Azure mfa'yı da yardımcı olan BT güvenliği aşılmış bir kimlik bilgisi kuruluş verilerine erişimi olduğunu olasılığını azaltmak için.

Örneğin: kullanıcılarınız için Azure MFA zorlamak ve telefon görüşmesi veya SMS mesajı doğrulaması kullanacak şekilde yapılandırın. Saldırgan, kullanıcının kimlik bilgilerinin güvenliğinin aşılması durumunda kullanıcının telefonuna erişimi olmadığından herhangi bir kaynağa erişebilir değil. Ek kimlik koruma katmanları eklemeyin kuruluşlar, veri güvenliğinin aşılmasına yol açabilir kimlik bilgisi hırsızlığı saldırısına için daha açıktır.

Bir alternatif için tüm denetim şirket içi kimlik doğrulaması tutmak isteyen kuruluşların kullanmaktır [Azure multi-Factor Authentication sunucusu](../active-directory/authentication/howto-mfaserver-deploy.md), şirket içi MFA olarak da bilinir. Bu yöntemi kullanarak, multi-Factor authentication sunucusu şirket içi MFA tutarken uygulayabilmektir devam edersiniz.

Azure MFA hakkında daha fazla bilgi için bkz [bulutta Azure multi Factor Authentication kullanmaya başlama](../active-directory/authentication/howto-mfa-getstarted.md).

## <a name="use-role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC) kullanın

Erişimi kısıtlama temel alarak [bilmeniz gereken](https://en.wikipedia.org/wiki/Need_to_know) ve [en az ayrıcalık](https://en.wikipedia.org/wiki/Principle_of_least_privilege) güvenlik ilkeleri, veri erişimi için güvenlik ilkelerini zorlamak istediğinizde kuruluşlar için zorunlu. Azure rol tabanlı erişim denetimi (RBAC), kullanıcılara, gruplara ve uygulamalara belirli bir kapsamda izinleri atamak için kullanılabilir. Rol atamasının kapsamı, bir abonelik, kaynak grubu veya tek bir kaynak olabilir.

Yararlanabileceğiniz [RBAC yerleşik](../role-based-access-control/built-in-roles.md) ayrıcalıkları kullanıcılara atamak için azure'da rolleri. Kullanmayı *depolama hesabı Katılımcısı* için depolama hesapları yönetmek için gereken bulut operatörleri ve *Klasik depolama hesabı Katılımcısı* Klasik depolama hesaplarını yönetmek için rol. VM'ler ve depolama hesabını yönetmek için gereken bulut operatörleri için ekleyerek bunları göz önünde bulundurun *sanal makine Katılımcısı* rol.

Veri erişim denetimi, RBAC gibi özelliklerinden yararlanarak zorlama kuruluşların kullanıcıları için gerekenden daha fazla ayrıcalık vermiş. Bu yol açabilir veri güvenliğinin aşılmasına tarafından kullanıcılar erişime izin ilk başta olmamalıdır (örneğin, yüksek iş etkisi) veri belirli bir türde.

Azure RBAC hakkında daha fazla makaleyi okuyarak bilgi [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md).

## <a name="control-locations-where-resources-are-created-using-resource-manager"></a>Kaynak Yöneticisi'ni kullanarak kaynakların oluşturulabileceği denetim konumları

Bulut operatörleri, kuruluşunuzun kaynakları yönetmek için gerekli olan kuralları kesilmesini önlenirken görevleri gerçekleştirmek etkinleştirmek çok önemlidir. Kaynakların oluşturulabileceği konumlarını denetlemek isterseniz kuruluşların Bu konumlar sabit kodlamanız gerekir.

Bunu başarmak için kuruluşlar eylemler veya reddedilen kaynakları tanımlamak tanımlara sahip güvenlik ilkeleri oluşturabilirsiniz. Bu ilke tanımları aboneliğe, kaynak grubuna ya da ayrı bir kaynak gibi istenilen kapsamda atarsınız.

> [!NOTE]
> Bu RBAC ile aynı değildir, aslında bu kaynakları oluşturmak için ayrıcalığına sahip kullanıcıların kimliklerini doğrulamak için RBAC yararlanır.
>
>

Gücünden yararlanarak [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) de burada kuruluş yalnızca uygun maliyet merkezi ilişkilendirildiğinde işlemlerine izin ver istiyor; Aksi takdirde, bunlar isteği reddet senaryoları için özel ilkeler oluşturmak için.

Kaynakları nasıl oluşturulduğunu denetlediğiniz olmayan kuruluşlar, kullanıcılara ihtiyaç duydukları çok daha fazla kaynak oluşturarak hizmeti kötüye daha açıktır. Kaynak oluşturma işlemini sağlamlaştırma, çok kiracılı senaryo güvenliğini sağlamak için önemli bir adımdır.

Makaleyi okuyarak Azure Resource Manager ile ilkeleri oluşturma hakkında daha fazla bilgi [Azure İlkesi nedir?](../azure-policy/azure-policy-introduction.md)

## <a name="guide-developers-to-leverage-identity-capabilities-for-saas-apps"></a>SaaS uygulamaları için kimlik özelliklerinden yararlanmak için Geliştirici Kılavuzu

Kullanıcı Kimliği kullanıcıların erişirken pek çok senaryoda yararlanılarak [SaaS uygulamaları](https://azure.microsoft.com/marketplace/active-directory/all/) dizini Bulut veya şirket içi ile tümleştirilebilir. İlk ve, geliştiriciler gibi bu uygulamaları geliştirmek için güvenli bir yöntem kullanmanızı öneririz [Microsoft Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx). Azure AD ile bir hizmet olarak kimlik sağlayarak geliştiriciler gibi endüstri standardı protokoller için desteği için kimlik doğrulamasını kolaylaştırır [OAuth 2.0](http://oauth.net/2/) ve [Openıd Connect](http://openid.net/connect/)de gibi açık kaynak farklı platformlar için kitaplıkları.

Azure AD kimlik outsources herhangi bir uygulamayı kaydetme emin olmak için bu zorunlu bir yordamdır. Bunun altında yatan neden, Azure AD oturum açma (SSO) işleme ya da değişimi belirteçler, uygulama ile iletişimi koordine etmek gerekli olmasıdır. Azure AD tarafından verilen belirtecin süresi dolduğunda, kullanıcının oturum sona erer. Bu süre, uygulamanızın kullanması gerekiyorsa veya bu süreyi kısaltabilirsiniz her zaman değerlendirin. Yaşam süresini azaltarak kullanıcıların oturumu kapatmak için temel bir işlem yapılmadan geçen süre üzerinde zorlar bir güvenlik önlemi olarak işlev görebilir.

Kimlik denetimi uygulamalara erişmek için zorlama ve geliştiricileri güvenli bir şekilde uygulama kendi kimlik yönetimi sistemi ile tümleştirme konusunda rehberlik olmayan kuruluşlar gibi kimlik bilgisi hırsızlığı türü, saldırının daha elverişli [zayıf Açık Web uygulaması güvenlik Project (OWASP) ilk 10 açıklanan kimlik doğrulaması ve oturum yönetimi](https://www.owasp.org/index.php/OWASP_Top_Ten_Cheat_Sheet).

SaaS uygulamaları için kimlik doğrulama senaryoları hakkında daha fazla okuyarak bilgi [Azure AD için kimlik doğrulama senaryoları](../active-directory/develop/authentication-scenarios.md).

## <a name="actively-monitor-for-suspicious-activities"></a>Şüpheli etkinlikler için etkin bir şekilde izleyin

Şunlara göre [Verizon 2016 veri İhlale yol açmak üzere raporu](http://www.verizonenterprise.com/verizon-insights-lab/dbir/2016/), kimlik bilgilerinin tehlikeye atılması olduğundan hala hacmindeki ve En Karlı işletmelerin biri için siber Suçları olma. Bu nedenle, bir etkin kimliği izleme sistemi, hızlı bir şekilde kuşkulu davranış etkinliği algılayabilir ve daha fazla araştırma için bir uyarının tetiklenmesini yerde olması önemlidir. Azure AD, kuruluşların kimliklerini izlemesine yardımcı olacak iki önemli özellikleri vardır: Azure AD Premium [anomali raporları](../active-directory/active-directory-view-access-usage-reports.md) ve Azure AD [kimlik koruması](../active-directory/active-directory-identityprotection.md) yeteneği.

Oturum açmak için deneme tanımlamak için anormallik raporları kullandığınızdan emin olun [izlenen olmadan](../active-directory/active-directory-reporting-sign-ins-from-unknown-sources.md), [yanılma](../active-directory/active-directory-reporting-sign-ins-after-multiple-failures.md) saldırılarına karşı belirli bir hesabı, birden çok konumlardan oturum saldırılara [oturum açın virüs bulaşmış cihazlardan ve şüpheli IP adresleri. Bu raporlar olduğunu aklınızda bulundurun. Diğer bir deyişle, bu raporlar günlük olarak veya isteğe bağlı olarak (genellikle, bir olay yanıtı senaryosu) çalıştırmak, BT yöneticileri için yerinde süreçleri ve yordamları olmalıdır.

Buna karşılık, Azure AD kimlik koruması etkin bir izleme sistemi, ve geçerli riskler, kendi Pano için işaretler. Yanı sıra de e-posta ile günlük özet bildirim alırsınız. Risk düzeyi iş gereksinimlerinize uygun şekilde ayarlamanızı öneririz. Risk olayı yönelik risk düzeyi (yüksek, Orta veya düşük) risk olayının önem göstergesidir. Risk düzeyi kuruluşuna riskini azaltmak için almalıdır eylemleri öncelik kimlik koruması kullanıcılara yardımcı olur.

Etkin kimlik sistemlerinin izlemeyin kuruluşların, kullanıcı kimlik bilgilerini ele yaşama risk altındadır. Bu kimlik bilgilerini kullanarak şüpheli etkinlikleri sürüp bilginiz dışında yerleştirin, kuruluşların bu tür bir tehdidi azaltmak mümkün olmayacaktır.
Okuyarak Azure kimlik koruması hakkında daha fazla bilgi edinebilirsiniz [Azure Active Directory kimlik koruması](../active-directory/active-directory-identityprotection.md).
