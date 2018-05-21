---
title: Azure kimlik ve erişim en iyi güvenlik uygulamaları | Microsoft Docs
description: Bu makalede kimlik yönetimi için en iyi yöntemler kümesi sağlar ve erişim denetimi Azure özellikleri kullanılarak.
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
ms.openlocfilehash: 6632ab962f3df0cfee8d28d7dad40bad8baf3f50
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>En iyi güvenlik uygulamaları Azure kimlik yönetimi ve erişim denetimi

Birçok yeni bir sınır katman geleneksel ağ merkezli açısından bu rolü ele güvenlik için kimlik göz önünde bulundurun. Bu birincil Özet evrimi güvenlik dikkat ve Yatırımlar ağ çevreyi hale giderek porous ve bu çevre savunması kez bunlar olabildiğince etkili olamaz olgu gelen için önce Patlaması olan [KCG](http://aka.ms/byodcg) cihazlar ve bulut uygulamaları.

Bu makalede, Azure kimlik yönetimi ve erişim denetimi en iyi güvenlik yöntemleri koleksiyonunu tartışın. Bu en iyi uygulamaları ile deneyimi bizim türetilmiş [Azure AD](../active-directory/active-directory-whatis.md) ve müşteri deneyimleri bulunun.

En iyi her uygulama için biz açıklamaktadır:

* En iyi uygulama nedir
* Bu en iyi uygulama etkinleştirmek istediğiniz neden
* En iyi uygulama olarak etkinleştirmek başarısız olursa ne sonucu olabilir
* En iyi uygulama için olası alternatifler
* Nasıl en iyi uygulama olarak etkinleştirmek bilgi edinebilirsiniz

Bu makale, bu Azure kimlik yönetimi ve aynı anda oldukları gibi en iyi yöntemler makalesi anlaşma fikir ve Azure platformu özellikleri ve özellik kümeleri dayanan erişim denetimi güvenliği yazılmıştır. Zaman içinde görüşlerini ve teknolojileri değiştirebilirsiniz ve bu makalede bu değişiklikleri yansıtacak şekilde düzenli olarak güncelleştirilir.

Bu makalede ele alınan azure kimlik yönetimi ve erişim denetimi güvenlik en iyi uygulamalar şunlardır:

* Kimlik yönetimini merkezi hale getirin
* Çoklu oturum açma (SSO) etkinleştir
* Parola yönetimi dağıtma
* Kullanıcılar için çok faktörlü kimlik doğrulaması (MFA) zorunlu
* Rol tabanlı erişim denetimi (RBAC) kullanın
* Kaynakları Kaynak Yöneticisi'ni kullanarak oluşturulduğu denetim konumları
* SaaS uygulamaları için kimlik özelliklerden yararlanacak şekilde Geliştirici Kılavuzu
* Etkin olarak şüpheli etkinlikleri izleme

## <a name="centralize-your-identity-management"></a>Kimlik yönetimini merkezi hale getirin

Kimliğinizi güvenliğini sağlamaya yönelik bir önemli adımlardan biri, BT can emin olmak için bu hesabın oluşturulduğu ile ilgili bir konumdan tek hesaplarını yönetin. BT kuruluşları kuruluşların çoğu olmakla birlikte, birincil hesap şirket içi dizin, karma bulut dağıtımları yükselişe ve şirket içi tümleştirme ve bulut dizinlerinizin ve sorunsuz bir sağlamak nasıl anlamak önemlidir Son kullanıcı deneyimi.

Bunu gerçekleştirmek için [karma kimlik](../active-directory/active-directory-hybrid-identity-design-considerations-overview.md) senaryo iki seçenek öneririz:

* Şirket içi dizininizi Azure AD Connect'i kullanarak, bulut dizini ile eşitleme
* İle çoklu oturum açmayı etkinleştir [parola karması eşitlemesi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization), [doğrudan kimlik doğrulama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-pass-through-authentication-faq) veya şirket içi kimlik bilgilerinizi kullanarak bulut dizini ile birleştirmek [Active Directory Federasyon Hizmetleri](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/deploying-federation-servers) (AD FS)

Şirket içi kimliklerini kendi bulut kimliği ile tümleştirmek için başarısız olan kuruluşlar, hatalar ve güvenlik ihlallerini olasılığını hesapları yönetme yönetim iş yükünün artmasına karşılaşırsınız.

Azure AD eşitleme hakkında daha fazla bilgi için bkz: [şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../active-directory/active-directory-aadconnect.md).

## <a name="enable-single-sign-on-sso"></a>Çoklu oturum açma (SSO) etkinleştir

Yönetmek için birden fazla dizine sahip olduğunuzda, bu değil yalnızca yönetimsel bir sorun haline BT, birden fazla parolayı hatırlamak zorunda son kullanıcılar için de. Kullanarak [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) özelliğini oturum açın ve, ne olursa olsun bu kaynak şirket içinde olduğu veya bulutta ihtiyaç duydukları kaynaklara erişmek için aynı kimlik bilgileri kümesini kullanın, kullanıcılarınızın sağlayın.

Kullanıcıların erişmek SSO kullan kendi [SaaS uygulamaları](../active-directory/manage-apps/what-is-single-sign-on.md) kuruluş hesaplarıyla içinde Azure AD tabanlı. Bu yalnızca Microsoft SaaS uygulamaları, ancak diğer uygulamalar için de geçerli olduğu gibi [Google Apps](../active-directory/active-directory-saas-google-apps-tutorial.md) ve [Salesforce](../active-directory/active-directory-saas-salesforce-tutorial.md). Uygulamanızı Azure AD olarak kullanmak için yapılandırılabilir bir [SAML tabanlı kimlik](../active-directory/fundamentals-identity.md) sağlayıcısı. Güvenlik denetimi olarak Azure AD, Azure AD kullanarak erişim verildi sürece uygulamasına oturum vermeden bir belirteç yayınlamayacaktır. Doğrudan veya bir grup üyesi oldukları erişim.

> [!NOTE]
> SSO kullanmaya karar nasıl şirket içi dizininizi bulut diziniyle tümleştirmek etkiler. SSO istiyorsanız, dizin eşitleme yalnızca sağlayacağından, Federasyon, kullanmanıza gerek [aynı oturum açma deneyimi](../active-directory/active-directory-aadconnect.md).
>
>

SSO, kullanıcılar ve uygulamalar için zorunlu olmayan kuruluşlar, doğrudan parolaları yeniden kullanma veya Zayıf parolalar kullanarak kullanıcıların arttırır kullanıcıların birden çok parola sahip olduğu senaryolar için daha maruz kalır.

Makaleyi okuyarak Azure AD SSO hakkında daha fazla bilgiyi [AD FS yönetimi ve Azure AD Connect ile özelleştirme](../active-directory/active-directory-aadconnect-federation-management.md).

## <a name="deploy-password-management"></a>Parola yönetimi dağıtma

Birden çok kiracıya sahip veya kullanıcılara etkinleştirmek istediğiniz olduğu senaryolarda [kendi parolasını sıfırlama](../active-directory/active-directory-passwords-update-your-own-password.md), kötüye engellemek için uygun güvenlik ilkelerini kullanma önemlidir. Azure'da, Self Servis parola sıfırlama yeteneğinden yararlanabilir ve iş gereksinimlerinizi karşılamak için güvenlik seçeneklerini özelleştirebilirsiniz.

Bu kullanıcılardan görüş edinmek ve bu adımları gerçekleştirmek çalışırken deneyimlerini bilgi almak önemlidir. Bu deneyimler bağlı olarak, daha büyük bir grup için dağıtım sırasında oluşabilecek olası sorunları gidermek için bir plan özenli. Ayrıca, kullanmanızı tavsiye edilir [parola sıfırlama kayıt Etkinlik Raporu](../active-directory/active-directory-passwords-get-insights.md) kaydediyorsunuz kullanıcıları izlemek için.

Parola değişikliği destek aramaları önlemek istiyor ancak kullanıcıların kendi parolalarını sıfırlamasına olanak kuruluşlar, hizmet Masası parola sorunları nedeniyle daha yüksek bir çağrı birime daha açıktır. Birden çok kiracıya sahip kuruluşlarda, bu tür bir yetenek ve parola güvenlik ilkesinde oluşturulan güvenlik sınırları içinde sıfırlama gerçekleştirmelerini sağlama zorunludur.

Parola makalesini okuyarak sıfırlama hakkında daha fazla bilgiyi [dağıtma parola yönetimi ve kullanıcılara eğitim kullanmak için](../active-directory/authentication/howto-sspr-deployment.md).

## <a name="enforce-multi-factor-authentication-mfa-for-users"></a>Kullanıcılar için çok faktörlü kimlik doğrulaması (MFA) zorunlu

Gibi endüstri standartları ile uyumlu olması gereken kuruluşlar [PCI DSS sürüm 3.2](http://blog.pcisecuritystandards.org/preparing-for-pci-dss-32), çok faktörlü kimlik doğrulaması şart olduğu için kullanıcıların kimlik doğrulaması özelliğine sahip. Endüstri standartları ile uyumlu olmasını ötesinde kullanıcıların kimliklerini doğrulamak için MFA zorlamayı da saldırı, kimlik bilgisi hırsızlığı türü gibi azaltmak için kuruluşlara yardımcı olabilir [Pass--Hash (PtH)](http://aka.ms/PtHPaper).

Kullanıcılarınız için Azure MFA etkinleştirerek, kullanıcı oturum açmaları ve işlemleri için ikinci bir güvenlik katmanı ekliyorsunuz. Bu durumda, bir işlem bir dosya sunucusunda veya, SharePoint Online'da bulunan bir belge erişiyor. Ayrıca, Azure MFA yardımcı BT güvenliği aşılmış bir kimlik bilgisi kuruluşunuzun veri erişimi olduğunu olasılığını azaltmak için.

Örneğin: kullanıcılarınız için Azure MFA zorlamak ve bir telefon araması veya kısa mesaj doğrulama kullanacak şekilde yapılandırın. Kullanıcının kimlik bilgilerinin güvenliği aşıldığında saldırgan, kullanıcının telefonu erişimi olmadığı beri herhangi bir kaynağa erişim mümkün değil. Ek kimlik koruma katmanları eklemeyin kuruluşlar veri güvenliğinin aşılmasına neden kimlik bilgisi hırsızlığı saldırısına daha açıktır.

Tüm kimlik doğrulama denetimi içi tutmak istediğiniz kuruluşlar için bir alternatif kullanmaktır [Azure çok faktörlü kimlik doğrulama sunucusu](../active-directory/authentication/howto-mfaserver-deploy.md), MFA şirket içi olarak da bilinir. Bu yöntemi kullanarak, çok faktörlü kimlik doğrulaması, MFA sunucusu şirket içi korurken zorlayabilir devam edersiniz.

Azure MFA hakkında daha fazla bilgi için bkz: [bulutta Azure multi-Factor Authentication kullanmaya Başlarken](../active-directory/authentication/howto-mfa-getstarted.md).

## <a name="use-role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC) kullanın

Erişimi kısıtlamak temel alarak [bilmeniz](https://en.wikipedia.org/wiki/Need_to_know) ve [en az ayrıcalık](https://en.wikipedia.org/wiki/Principle_of_least_privilege) güvenlik ilkeleri kesinlik temelli veri erişimi için güvenlik ilkelerini zorlamak istiyorsanız kuruluşlar için. Azure rol tabanlı erişim denetimi (RBAC), kullanıcılar, gruplar ve uygulamalar belirli bir kapsamda izinleri atamak için kullanılabilir. Rol atamasının kapsamı, bir abonelik, bir kaynak grubu veya tek bir kaynak olabilir.

Yararlanabileceğiniz [RBAC yerleşik](../role-based-access-control/built-in-roles.md) ayrıcalıkları kullanıcılara atamak için Azure rollerinde. Kullanmayı *depolama hesabı katkıda bulunan* depolama hesaplarını yönetmek için gereken bulut operatörleri için ve *Klasik depolama hesabı katkıda bulunan* Klasik depolama hesaplarını yönetmek için rol. Sanal makineleri ve depolama hesabı yönetmesi gereken bulut operatörleri için onlara eklemeyi düşünün *sanal makine Katılımcısı* rol.

Veri erişim denetimi RBAC gibi özellikler yararlanarak zorlamaz kuruluşların kullanıcılarına gerekenden daha fazla ayrıcalık vermiş. Bu yol açabilir veri tarafından güvenliğinin aşılmasına izin kullanıcıları belirli türde bir ilk başta olmamalıdır (örneğin, yüksek iş etkisi) veri erişim.

Makaleyi okuyarak Azure RBAC hakkında daha fazla bilgiyi [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md).

## <a name="control-locations-where-resources-are-created-using-resource-manager"></a>Kaynakları Kaynak Yöneticisi'ni kullanarak oluşturulduğu denetim konumları

Bulut operatörleri, kuruluşunuzun kaynakları yönetmek için gerekli olan kuralları kesilmesini önlerken görevleri gerçekleştirmek etkinleştirme önemlidir. Kaynakları oluşturulduğu konumları denetlemek isteyen kuruluşlar, sabit bu konumları kod.

Bunun için kuruluşlar eylemler veya reddedilir kaynakları tanımlamak tanımları olan güvenlik ilkeleri oluşturabilir. Bu ilke tanımları abonelik, kaynak grubu veya tek başına bir kaynak gibi istenen kapsamındaki atayın.

> [!NOTE]
> Bu RBAC ile aynı değildir, bu kaynakları oluşturmak için ayrıcalığına sahip kullanıcıların kimliklerini doğrulamak için RBAC gerçekte yararlanır.
>
>

Dengeleme [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) da burada kuruluş yalnızca uygun maliyet merkezi ilişkilendirildiğinde işlemleri izin vermek istiyor; Aksi halde, bunlar isteği reddedecek senaryoları için özel ilkeler oluşturmak için.

Kaynakları nasıl oluşturulduğunu denetlediğiniz olmayan kuruluşlar, hizmet ihtiyaç duydukları olandan daha fazla kaynak oluşturarak kötüye kullanıcıları daha açıktır. Kaynak oluşturma işlemi sağlamlaştırma, çok kiracılı senaryo güvenliğini sağlamak için önemli bir adımdır.

Makaleyi okuyarak Azure Resource Manager ile ilkeleri oluşturma hakkında daha fazla bilgi edinebilirsiniz [Azure ilke nedir?](../azure-policy/azure-policy-introduction.md)

## <a name="guide-developers-to-leverage-identity-capabilities-for-saas-apps"></a>SaaS uygulamaları için kimlik özelliklerden yararlanacak şekilde Geliştirici Kılavuzu

Kullanıcı kimliği kullanıcılar eriştiğinizde, birçok senaryoda işlevden [SaaS uygulamaları](https://azure.microsoft.com/marketplace/active-directory/all/) ile şirket içi tümleştirilebilir veya Bulut dizini. Öncelikle, geliştiriciler bu uygulamaları gibi geliştirmek için güvenli bir yöntem kullanmanızı öneririz [Microsoft Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx). Azure AD ile bir hizmet olarak kimlik sağlayarak geliştiriciler için endüstri standardı protokolleri gibi desteği için kimlik doğrulama basitleştirir [OAuth 2.0](http://oauth.net/2/) ve [Openıd Connect](http://openid.net/connect/), de olarak açık kaynak farklı platformlar için kitaplıkları.

Bu zorunlu bir yordamdır, Azure ad kimlik doğrulama outsources herhangi bir uygulama kaydetmek emin olun. Azure AD oturum açma (SSO) işleme veya değişimi belirteçler, uygulama ile iletişimi koordine etmek gerektiğinden bu arkasında nedenidir. Azure AD tarafından verilen belirtecin süresi sona erdiğinde kullanıcının oturum sona erer. Her zaman bu süre, uygulamanızın kullanması gerekiyorsa veya bu süresini azaltabilir, değerlendirin. Yaşam süresini azaltarak kullanıcıların oturumu tabanlı bir süre işlem yapılmadığında hakkında zorlar bir güvenlik önlemi olarak çalışabilir.

Uygulamalara erişmek için kimlik denetimi uygulamaz ve bunların geliştiriciler güvenli bir şekilde uygulama kendi kimlik yönetimi sistemi ile tümleştirme hakkında kılavuzluk değil kuruluşlar hırsızlığı tür saldırılar, kimlik bilgisi gibi daha açıktır [açık Web uygulaması güvenlik proje (OWASP) ilk 10 açıklanan zayıf kimlik doğrulama ve oturum yönetimi](https://www.owasp.org/index.php/OWASP_Top_Ten_Cheat_Sheet).

SaaS uygulamaları için kimlik doğrulama senaryoları hakkında daha fazla okuyarak bilgi [Azure AD için kimlik doğrulama senaryoları](../active-directory/active-directory-authentication-scenarios.md).

## <a name="actively-monitor-for-suspicious-activities"></a>Etkin olarak şüpheli etkinlikleri izleme

Göre [Verizon 2016 veri ihlali rapor](http://www.verizonenterprise.com/verizon-insights-lab/dbir/2016/), kimlik bilgilerinin tehlikeye atılması olduğundan hala yükseklik ve En Karlı işletmeler birini siber suçlular olma. Bu nedenle, bir etkin kimlik izleme sistemi, hızlı bir şekilde kuşkulu davranış etkinliği algılayabilir ve daha fazla araştırma için bir uyarı tetiklemesi yerde olması önemlidir. Azure AD'de kuruluşların kimliklerini izlemenize yardımcı olacak iki önemli özellikleri vardır: Azure AD Premium [anomali raporları](../active-directory/active-directory-view-access-usage-reports.md) ve Azure AD [kimlik koruması](../active-directory/active-directory-identityprotection.md) yeteneği.

Oturum açmak için denemeleri tanımlamak için kural dışı durum raporları kullandığınızdan emin olun [izlenen olmadan](../active-directory/active-directory-reporting-sign-ins-from-unknown-sources.md), [yanılma](../active-directory/active-directory-reporting-sign-ins-after-multiple-failures.md) saldırılarına karşı belirli bir hesabı, birden çok konumdan oturum saldırılara [oturum açın Etkilenen cihazlar ve kuşkulu IP adresleri. Bu raporlar olduğunu aklınızda bulundurun. Diğer bir deyişle, günlük olarak veya isteğe bağlı olarak (genellikle bir olay yanıtlama senaryosunda) bu raporları çalıştırmak BT yöneticilerine yönelik işlemler ve yordamlar olmalıdır.

Buna karşılık, Azure AD kimlik koruması etkin bir izleme sistemi ve kendi Panoda geçerli riskleri işaretler. Yanı sıra da e-posta yoluyla günlük özet bildirim alırsınız. Risk düzeyi, iş gereksinimlerinize göre ayarlamanız önerilir. Risk olayı için risk düzeyi (yüksek, Orta veya düşük) risk olay önem derecesi göstergesidir. Risk düzeyi için kuruluşları riskini azaltmak için gerçekleştirmeniz gereken eylemleri öncelik kimlik koruması kullanıcıların yardımcı olur.

Etkin kimlik sistemlerini izlemeyecek kuruluşların, kullanıcı kimlik bilgilerini tehlikeye sahip olmanın risk altındadır. Bu kimlik bilgileri kullanılarak kuşkulu etkinlikleri kaplayan bilginiz dışında yerleştirin, kuruluşların bu tür tehdidi azaltmak erişemezsiniz.
Okuyarak Azure kimlik koruması hakkında daha fazla bilgiyi [Azure Active Directory kimlik koruması](../active-directory/active-directory-identityprotection.md).
