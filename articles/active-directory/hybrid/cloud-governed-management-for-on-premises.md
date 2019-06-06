---
title: Azure AD bulut yönetimi için şirket içi iş yüklerini - Azure tarafından yönetilen
description: Bu konuda, şirket içi iş yükleri için yönetilen bulut yönetimi açıklanmaktadır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b7e54f6acfe1cbbe6e46fe92d132ebdaa91ff33
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66742352"
---
# <a name="how-azure-ad-delivers-cloud-governed-management-for-on-premises-workloads"></a>Azure AD teslim nasıl bulut yönetimi için şirket içi iş yüklerini kapsamındadır.

Azure Active Directory (Azure AD), kapsamlı bir kimlik milyonlarca kimlik, erişim yönetimi ve güvenliği tüm yönleriyle kapsayan bir kuruluş tarafından kullanılan bir hizmeti (Idaas) çözümü olarak. Azure AD, bir milyardan fazla kullanıcı kimliklerini içerir ve kullanıcıların oturum açın ve hem de güvenli bir şekilde erişmenize yardımcı olur:

* Microsoft Office 365, Azure portalı ve diğer hizmet olarak yazılım-a-(SaaS) uygulamaları binlerce gibi dış kaynaklar.
* Bir kuruluşun kurumsal ağ ve bu kuruluşun geliştirdiği tüm bulut uygulamaları ile birlikte bir intranet üzerinde uygulamalar gibi iç kaynaklara.

Kuruluşlar, bunlar 'saf bulut' veya 'karma' bunlar varsa dağıtım şirket içi iş yüklerini Azure AD kullanabilir. Azure ad karma bir dağıtımda, bir kuruluşun kendi BT varlıklarınızı buluta geçirme veya yeni bulut Hizmetleri ile birlikte var olan şirket içi altyapı tümleştirmek devam etmek için stratejisinin bir parçası olabilir.

Tarihsel olarak, bir uzantı, var olan şirket içi altyapınız olarak 'karma' kuruluşlar Azure AD'ye gördünüz. Bu dağıtımlarda, şirket içi kimlik İdaresi yönetim, Windows Server Active Directory veya diğer şirket içi dizin sistemleri olan denetim noktaları ve kullanıcılar ve gruplar bu sistemlerden Azure AD gibi bulut dizini için eşitlenir. Bulutta kimliklerle olduktan sonra Office 365, Azure ve diğer uygulamalar için kullanılabilir.

![Kimlik yaşam döngüsü](media/cloud-governed-management-for-on-premises//image1.png)

Kuruluşlar, bulut uygulamalarını birlikte BT altyapısını daha fazla taşırken, birçok gelişmiş güvenlik ve Basitleştirilmiş yönetim özelliklerini bir hizmet olarak kimlik yönetimi için arıyoruz. Bulut teslimli Idaas özellikleri Azure AD'de yönetilen yönetim çözümleri ve sayesinde kuruluşlar hızlıca benimseyin ve kendi kimlik yönetimi daha geleneksel şirket içi ad'nizden taşıma özellikleri sağlayarak bulut geçişi hızlandırın sistemleri Azure AD'ye mevcut yanı sıra yeni uygulamaları desteklemeye devam edebilirsiniz.

Bu yazıda, karma Idaas için Microsoft'un stratejisini açıklar ve kuruluşlar için mevcut uygulamalarını Azure AD'ye nasıl kullanabileceğinizi açıklar.

## <a name="the-azure-ad-approach-to-cloud-governed-identity-management"></a>Yönetilen Kimlik Yönetimi bulut Azure AD yaklaşımı

Bulut, kuruluşların geçişi olarak sahip oldukları denetimleri tamamlandı - ortamlarında üzerinden daha fazla güvenlik ve etkinliklere, otomasyon ve proaktif ınsights tarafından desteklenen daha fazla görünürlük Güvenceleri ihtiyaç duydukları. "**Bulut Yönetimi tarafından yönetilen**" nasıl kuruluşların yönetmek ve kullanıcıların, uygulamaları, gruplar ve cihazları buluttan yönetmek açıklar.

Bu modern dünyasında, kuruluşların uygun ölçekte, işyerinde SaaS uygulamaları ve işbirliği ve dış kimlikler artan rolünü nedeniyle etkili bir şekilde yönetmek gerekir. Bulutun yeni risk yatay bir kuruluş daha duyarlı olmamız gerekir - Bulut ve şirket içi uygulamalar kullanan bir bulut kullanıcısı tehlikeye atar ve kötü amaçlı bir aktör etkileyebilecek anlamına gelir.

Özellikle, temsilci ve otomatik hale getirmek için gereken kuruluşlar karma görevlerini, geçmişte BT el ile vermedi. Görevleri otomatikleştirmek için API'ler ve yaşam döngüsünü farklı kimlikle ilgili kaynaklar (kullanıcılar, gruplar, uygulamalar, cihazlar), düzenleme işlemleri ihtiyaç duydukları dışında daha fazla kişiler için bu kaynakları günlük yönetimini devredebilirsiniz şekilde BT personeli çekirdek. Yerel kullanıcılar için kimlik doğrulaması gerekmeden şirket kimlik altyapısı ve Azure AD kullanıcı hesabı Yönetimi aracılığıyla bu gereksinimleri adresleri. Şirket içi altyapıyı oluşturma değil, kullanıcı, iş ortakları, kendi şirket içi dizininde kaynaklanan olmadı, ancak iş sonuçlarını elde etmek için kritik olan erişim yönetimi gibi yeni toplulukları olan kuruluşlar yararlı olabilir.

Ayrıca, yönetim idare---tamamlanmadı ve bu yeni dünyasında idare eklenti yerine kimlik sistemi, tümleşik bir parçasıdır. Kimlik Yönetimi, kuruluşların kimlik yönetimi ve yaşam döngüsü çalışanlar, iş ortakları ve satıcılar ve Hizmetleri ve uygulamaları erişim olanağı sunar.

Kimlik Yönetimi ekleme yönetilen yönetim buluta geçiş kuruluş etkinleştirmeyi kolaylaştırır, sağlar ölçeklendirmek için BT ile Konukları yeni giderilmesini ve daha ayrıntılı Öngörüler ve müşterilerin sahip olduğu daha Otomasyon sağlar. Şirket içi altyapı. Bu yeni dünyasında idare saydamlık, görünürlük ve uygun kontrolleri kuruluş içindeki kaynaklara erişim sağlamak bir kuruluş için yeteneği anlamına gelir. Azure AD ile güvenlik işlemleri ve denetim ekipleri kimlerin---ve kimin olmalıdır - görünürlük (hangi cihazlarında) kuruluş, bu erişim ile bu kullanıcıların ne yaptıklarını ve kuruluş sahip olup olmadığını belirler ve kullandığı uygun hangi kaynaklara erişimi kaldırma veya şirket veya yasal düzenleme ilkelerine uygun olarak erişimini denetler.

Yeni yönetim modeli, yönetin ve bu uygulamalara güvenli erişim için daha kolay mümkün olduğu gibi SaaS hem satır iş kolu (LOB) uygulamaları, bir kuruluşlarıyla fayda sağlar. Uygulamaları Azure AD ile tümleştirme, kuruluşların kullanabilecek ve her iki bulut erişimi yönetin ve şirket içi kimlikleri tutarlı bir şekilde oluşturulur. Uygulama yaşam döngüsü yönetimi daha otomatik hale gelir ve Azure AD, şirket içi kimlik yönetiminde kolayca ulaşılabilir durumda uygulama kullanımı hakkında zengin Öngörüler sağlar. Azure AD Office 365 grupları ve Self Servis özellik ekipleri kuruluşların kolayca erişim yönetimi ve işbirliği için gruplar oluşturun ve ekleyebilir veya bulutta işbirliği etkinleştirme ve erişim yönetimi gereksinimlerine Kullanıcıları Kaldır.

Bulut kullanılacak uygulamalar üzerinde yönetilen yönetim bağlıdır ve bu uygulamaların Azure AD'ye nasıl tümleştirilecek sağ Azure AD özelliklerini seçme. Aşağıdaki bölümlerde yaklaşımları AD tümleşik uygulamaları ve Federasyon protokolleri (örneğin, SAML, OAuth veya Openıd Connect) kullanan uygulamalar için ana hat.

## <a name="cloud-governed-management-for-ad-integrated-applications"></a>AD tümleşik uygulamaları için yönetilen bulut Yönetimi

Azure AD yönetimi bir kuruluşun şirket içi Active Directory ile tümleşik uygulamalara güvenli uzaktan erişim ve koşullu erişim aracılığıyla bu uygulamalara için iyileştirir. Ayrıca, Azure AD hesabı yaşam döngüsü yönetimi ve kimlik bilgileri yönetimi dahil olmak üzere kullanıcının mevcut AD hesapları için sağlar:

* **Güvenli uzaktan erişim ve şirket içi uygulamalar için koşullu erişim**

Çoğu kuruluş için buluttan şirket içi AD ile tümleşik web ve Uzak Masaüstü tabanlı uygulamalar için erişim yönetmenin ilk adımı dağıtmaktır [uygulama proxy'si](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) sağlamak için bu uygulamaları önünde güvenli hale getirin Uzaktan erişim.

Bir çoklu oturum açma sonra Azure ad, kullanıcılar hem buluttaki hem de şirket içi uygulamaları dış URL veya bir iç uygulama portal erişebilir. Örneğin, Uzak Masaüstü, SharePoint yanı sıra, Tableau, Qlik ve iş kolu (LOB) uygulamaları gibi uygulamalar için uzaktan erişim ve çoklu oturum açma uygulaması Ara sunucusu sağlar. Ayrıca, koşullu erişim ilkelerini görüntüleme içerebilir [kullanım koşullarını](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou) ve [kullanıcı sağlama kabul etmelerini](https://docs.microsoft.com/azure/active-directory/conditional-access/require-tou) uygulamaya erişebilmeleri için önce.

![Uygulama Ara sunucusu mimarisi](media/cloud-governed-management-for-on-premises/image2.png)

* **Active Directory hesapları için otomatik yaşam döngüsü yönetimi**

Kimlik Yönetimi yardımcı olan kuruluşlar arasında bir denge sağlamak *üretkenlik* ---ne kadar hızlı bir kişi olabilir erişim için kaynaklar, kuruluş katılmaları gibi zaman ihtiyaç duydukları? ---ve *güvenlik* ---nasıl erişimleri değiştirilmelidir o kişinin iş durumu değiştiğinde gibi zaman içinde? Kimlik yaşam döngüsü yönetimi kimlik yönetimi için altyapıdır ve uygun ölçekte etkin idare modernleştirme uygulamalar için kimlik yaşam döngüsü Yönetim Altyapısı gerektirir.

Çoğu kuruluş için çalışanların kimlik yaşam döngüsü bir insan sermayesi Yönetimi (HCM) sistemde kullanıcı gösterimini bağlıdır. Workday HCM sistemlerine kullanan kuruluşlar için Azure AD kullanıcı hesaplarının ad sağlayabilirsiniz [otomatik olarak sağlanan ve çalışanları için Workday'de sağlaması](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial). Bunu müşteri adayları için geliştirilmiş kullanıcı üretkenliğini birthright otomasyonunu yapmak ve hesapları risk yönetir kullanıcı rollerini değiştirir veya kuruluştan ayrılması uygulama sağlayarak erişim otomatik olarak güncelleştirilir. Workday tabanlı kullanıcı sağlama [dağıtım planı](https://aka.ms/WorkdayDeploymentPlan) beş adımlı bir işlemin içinde Active Directory kullanıcı sağlama çözüme kuruluşlar Workday en iyi yöntemler uygulaması aracılığıyla anlatan ayrıntılı bir kılavuzdur.

Azure AD Premium, SAP, dahil olmak üzere diğer şirket içi HCM sistemlerden, kayıtlarını içeri aktarmak Microsoft Identity Manager ayrıca içerir Oracle eBusiness ve Oracle PeopleSoft.

İşletmeler arası işbirliği giderek kuruluşunuz dışındaki kişilere erişim izni verme gerektirir. [Azure AD B2B](https://docs.microsoft.com/azure/active-directory/b2b/) kuruluşların güvenli bir şekilde uygulamalarını ve hizmetlerini Konuk kullanıcılar ve dış iş ortakları kendi Kurumsal veriler üzerinde denetim korurken paylaşmak işbirliği sağlar.

Azure AD için [hesapları AD Konuk kullanıcılar için otomatik olarak oluşturma](https://docs.microsoft.com/azure/active-directory/b2b/hybrid-cloud-to-on-premises) gerektiği şekilde erişmek şirket konuklarınız etkinleştirme AD tümleşik uygulamaları başka bir parolaya gerek kalmadan şirket. Kuruluşların ayarlayabilirsiniz [Konuk kullanıcı için çok faktörlü kimlik doğrulaması (MFA) İlkeleri](https://docs.microsoft.com/azure/active-directory/b2b/conditional-access)yapılır uygulama proxy kimlik doğrulaması sırasında MFA kontrol edecek şekilde s. Ayrıca, tüm [erişim gözden geçirmeleriyle](https://docs.microsoft.com/azure/active-directory/governance/manage-guest-access-with-access-reviews) kullanıcılar geçerli şirket içi kullanıcıların bulutta B2B bitti. Örneğin, bulut kullanıcı yaşam döngüsü yönetimi ilkeleri silinirse, şirket içi kullanıcı da silinir.

**Kimlik bilgisi yönetimi için Active Directory hesaplarını** Azure AD Self Servis parola sıfırlama izin verir, kullanıcılar parolalarını kimliğinin yeniden doğrulanması ve değiştirilen parolalarla parolalarını sıfırlamak için unutmuş [yazılan Şirket içi Active Directory'nin](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback). Parola sıfırlama işlemi, şirket içi Active Directory parola ilkelerini de kullanabilirsiniz: Bir kullanıcı parolasını sıfırlandıktan sonra bu dizine yapmadan önce şirket içi Active Directory ilkesini karşıladığından emin olmak için denetlenir. Self Servis parola sıfırlama [dağıtım planı](https://aka.ms/deploymentplans/sspr) web ve Windows tümleşik deneyimler aracılığıyla kullanıcıları için Self Servis parola sıfırlamayı kullanıma almak için en iyi uygulamalar özetlenmektedir.

![Azure AD SSPR mimarisi](media/cloud-governed-management-for-on-premises/image3.png)

Son olarak, kullanıcıların AD parolalarını değiştirmeye izin kuruluşlar için AD kuruluş Azure AD'de kullanarak aynı parola ilkesini kullanmak üzere yapılandırılabilir [Azure AD parola koruması özelliği](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises), şu anda Genel önizlemeye sunuldu.

Bir kuruluş azure'a uygulamayı barındıran işletim sistemi tarafından bir AD ile tümleşik uygulamanızı buluta taşıma hazır olduğunda [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) AD uyumlu etki alanı Hizmetleri (örneğin, etki alanına katılım, sağlar Grup İlkesi, LDAP ve Kerberos/NTLM kimlik doğrulaması). Azure AD etki alanı Hizmetleri, Kurumsal kimlik bilgilerini kullanarak oturum açmasına çözmelerine kuruluşun mevcut Azure AD kiracısına ile tümleştirilir. Ayrıca, varolan grupların ve kullanıcı hesaplarını kaynaklarına erişimi güvenli hale getirmek için bir daha yumuşak 'lift-and-shift' şirket içi kaynakların Azure altyapı hizmetleri için sağlama kullanılabilir.

![Azure AD Domain Services](media/cloud-governed-management-for-on-premises/image4.png)

## <a name="cloud-governed-management-for-on-premises-federation-based-applications"></a>Şirket içi Federasyon tabanlı uygulamalar için yönetilen bulut Yönetimi

Zaten bir şirket içi kimlik sağlayıcısı kullanan bir kuruluş için uygulamaların Azure AD'ye taşıma için daha güvenli erişim ve Federasyon yönetimi için daha kolay yönetim deneyimi sağlar. Azure AD, Azure AD koşullu erişim kullanarak Azure multi-Factor Authentication dahil ayrıntılı uygulama başına erişim denetimlerini yapılandırma sağlar. Azure AD uygulamaya özgü belirteç imzalama sertifikaları ve yapılandırılabilir sertifika sona erme tarihleri gibi daha fazla özellikleri destekler. Bu özellikler, Araçlar ve rehberlik, kuruluşların şirket içi kimlik sağlayıcıları devre dışı bırakma olanak sağlar. Microsoft'un kendi BT, bir örneğin 17,987 uygulamaları Azure AD'ye Microsoft'un iç Active Directory Federasyon Hizmetleri'nden (AD FS) taşındı.

![Azure AD evrimi](media/cloud-governed-management-for-on-premises/image5.png)

Geçişi başlatmak için kimlik sağlayıcısı olarak Azure AD'ye Federasyon uygulamalarına başvurduğu https://aka.ms/migrateapps bağlantılarını içerir:

* Teknik incelemeyi [bilgisayarınızı uygulamaları Azure Active Directory'ye geçirme](https://aka.ms/migrateapps/whitepaper), geçişin yararları sunar ve net bir şekilde açıklanan dört aşamaya geçiş planlama açıklar: bulma, Sınıflandırma, geçiş, ve devam eden yönetimi. İşlemleri hakkında düşünmek ve projenize kolayca kullanma parçalara ayırmanız nasıl destekli. Belge, süreç boyunca size yardımcı olacak önemli kaynakların bağlantılarını bağlıdır.

* Çözüm Kılavuzu [geçirme uygulama kimlik doğrulaması Active Directory Federasyon Hizmetleri için Azure Active Directory](https://aka.ms/migrateapps/adfssolutionguide) daha ayrıntılı olarak aynı planlama ve uygulama geçiş projesi yürütme dört aşama keşfediyor . Bu kılavuzda, belirli bir uygulamayı Azure AD'ye Active Directory Federasyon Hizmetleri (AD FS) taşıma amacı bu aşamaları uygulamak öğreneceksiniz.

* [Active Directory Federasyon Hizmetleri geçiş hazırlığı betik](https://aka.ms/migrateapps/adfstools) uygulamaları Azure AD'ye geçiş için hazır olup olmadığını belirlemek için mevcut şirket içi Active Directory Federasyon Hizmetleri (AD FS) sunucuları üzerinde çalıştırılabilir.

## <a name="ongoing-access-management-across-cloud-and-on-premises-applications"></a>Bulut ve şirket içi uygulamalarınız arasında sürekli erişim yönetimi

Kuruluşların ölçeklenebilir erişimi yönetmek için bir işlem gerekir. Kullanıcıların erişim haklarını biriktirmesi ve hangi başlangıçta kendileri için sağlanan ötesinde sonunda devam. Ayrıca, kuruluşlar verimli bir şekilde geliştirin ve erişim ilkesi ve düzenli olarak denetimleri zorunlu kılmak için ölçeğini genişletebilmesi gerekir.

Genellikle, BT temsilciler erişmek için iş karar mekanizmalarına onay kararları. Ayrıca, BT kullanıcıları içerebilir. Örneğin, Avrupa pazarlama uygulamada bir şirketin gizli müşteri verilerine erişen kullanıcılar şirketin ilkelerini bilmeniz gerekir. Konuk kullanıcıları bağımsız olarak, bunlar davet edildiniz bir kuruluşta veri işleme gereksinimleri olabilir.

Kuruluşlar otomatik hale getirmenizi teknolojileri aracılığıyla erişim yaşam döngüsü işlemi gibi [dinamik gruplar](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership), kullanıcı için sağlama ile bağlı [SaaS uygulamalarına](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list), veya [uygulamaları için etki alanları arası kimlik yönetimi sistemi kullanarak tümleşik (SCIM](https://docs.microsoft.com/azure/active-directory/manage-apps/use-scim-to-provision-users-and-groups)) standart. Kuruluşlar da denetleyebilir, [konuk kullanıcıların şirket içi uygulamalara erişiminin](https://docs.microsoft.com/azure/active-directory/b2b/hybrid-cloud-to-on-premises). Bu erişim hakları can sonra yinelenen kullanarak düzenli olarak gözden geçirilmesi [Azure AD erişim gözden geçirmeleri](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview).

## <a name="future-directions"></a>Gelecek yönergeleri

Karma ortamlarda, Microsoft'un stratejisini dağıtımları etkinleştirmektir burada **bulut kimliği için Denetim düzlemi olan**ve şirket içi dizinlere ve Active Directory ve diğer şirket içi gibi diğer kimlik sistemleri uygulamalardır, kullanıcılara erişim sağlamak için hedef. Bu strateji, hakları, kimlikleri ve bu uygulamaları ve bunlar üzerine kullanan iş yükleri erişim sağlamaya devam eder. Bu bitiş durumuna, kuruluşlar son kullanıcı üretkenliği tamamen buluttan mümkün olacaktır.

![Azure AD mimarisi](media/cloud-governed-management-for-on-premises/image6.png)

## <a name="next-steps"></a>Sonraki adımlar

Adresinde yer alan Azure AD dağıtım planları bu yolculukta başlama hakkında daha fazla bilgi için bkz. <https://aka.ms/deploymentplans> . Bunlar, Azure Active Directory (Azure AD) özellikleri dağıtmak nasıl uçtan uca yönergeler sağlar. Her plan planlama konuları, tasarım ve başarıyla yaygın Azure AD özelliklerini kullanıma almak için gereken, işlem yordamlarını iş değeri açıklar. Microsoft Azure AD ile buluttan yönetmek için yeni özellikler ekliyoruz, müşteri dağıtımları ve diğer geri öğrendiğimiz en iyi deneyimleri dağıtım planlar sürekli olarak güncelleştirir.
