---
title: Azure Active Directory'de kimlik altyapınızı güvenli hale getirmek için beş adım
description: Bu belge Yöneticiler, kuruluşlarının Azure AD özellikleri kullanarak bunları güvenli yardımcı olmak için uygulamalıdır önemli eylemlerin bir listesini özetler.
services: active-directory
author: martincoetzer
manager: manmeetb
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 06/18/2018
ms.author: martincoetzer
ms.openlocfilehash: 8e9101a1e23d361e66c5c30969069cbd4b971590
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65236784"
---
# <a name="five-steps-to-securing-your-identity-infrastructure"></a>Kimlik altyapınızın güvenliğini sağlamak için beş adım

Bu belgede güvenlik önemini kullanan okuyorsanız. Büyük olasılıkla zaten, kuruluşunuzun güvenliğini sağlama sorumluluğunu taşır. Başkalarının güvenliğinin önemi ikna gerekiyorsa, şirketlerde en son okuma [Microsoft Güvenlik zekası raporu](https://go.microsoft.com/fwlink/p/?linkid=2073747).

Bu belge kuruluşunuzu siber saldırılara karşı aşılanabilmesi için beş adım denetim listesini kullanarak Azure Active Directory özelliklerini kullanarak daha güvenli bir duruş almanıza yardımcı olur.

Bu denetim listesini açıklayarak hemen kuruluşunuzu korumak için kritik önerilen eylemler hızlı bir şekilde dağıtmanıza yardımcı olacak nasıl yapılır:

* Kimlik bilgilerinizi güçlendirin.
* Saldırı yüzey alanını azaltın.
* Tehdit yanıt otomatikleştirin.
* Denetleme ve izleme, bilinirliğinizi artırın.
* Kendi kendine yardım ile daha tahmin edilebilir ve tüm son kullanıcı güvenlik sağlar.

> [!NOTE]
> Bu belgedeki önerilerin çoğu, Azure Active Directory, kimlik sağlayıcısı olarak kullanmak üzere yapılandırılan uygulamalar için geçerlidir. Uygulamaları için çoklu oturum açmayı yapılandırma kimlik bilgisi ilkeleri, tehdit algılama, avantajlarını sağlar, bu uygulamaları denetleme, günlüğe kaydetme ve diğer özellikleri ekleyin. [Azure Active Directory aracılığıyla çoklu oturum açmayı](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-sso) tüm bu önerileri dayalı - temelidir.

Bu belgedeki önerileri ile hizalanır [kimlik güvenli puanı](https://docs.microsoft.com/azure/active-directory/fundamentals/identity-secure-score), bir Azure AD kiracınızın kimliğini güvenlik yapılandırması değerlendirmesinin otomatik. Kuruluşlar geçerli Microsoft güvenlik için en iyi uygulamaları takip etmeleri emin olmak için geçerli güvenlik yapılandırmalarını boşlukları bulmak için Azure AD portalında güvenli kimlik puanı sayfasını kullanabilirsiniz. Her önerinin güvenli puanı sayfasında uygulama puanınız artırmak ve ilerlemenizi izleyin yanı sıra diğer benzer boyutu kuruluşlar veya sektörünüze yönelik uygulamanızda karşılaştırmanıza yardımcı olanak sağlar.

![Kimlik Güvenliği Puanı](media/azure-ad/azure-ad-sec-steps0.png)

## <a name="before-you-begin-protect-privileged-accounts-with-mfa"></a>Başlamadan önce: MFA ile ayrıcalıklı hesapları korumak

Bu denetim listesini başlamadan önce bu denetim listesini makaleyi okuduğunuz karşın, aşılan yoksa emin olun. Önce ayrıcalıklı hesapları korumak gerekir.

Bu hesapların ilk korunması kritik, bu nedenle ayrıcalıklı hesapların denetim elde saldırganlar Bilim insanları için inanılmaz zarar yapabilirsiniz. Etkinleştirin ve gerekli [Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) (MFA) kullanarak kuruluş içindeki tüm yöneticiler için [temel koruma](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-baseline-protection). MFA uygulamadığınız, şimdi yapın! Bu önemlidir.

Tüm ayarlansın mı? Denetim listesi üzerinde başlayalım.

## <a name="step-1---strengthen-your-credentials"></a>1. adım - kimlik bilgilerinizi güçlendirin 

Çoğu Kurumsal güvenlik ihlallerini birkaç parola ilaç, ihlal yeniden yürütme ya da kimlik avı gibi yöntemlerin biriyle tehlikeye bir hesapla kaynaklanır. (45 dakika) Bu videoda bu saldırılar hakkında daha fazla bilgi edinin:
> [!VIDEO https://www.youtube.com/embed/uy0j1_t5Hd4]

### <a name="make-sure-your-organization-use-strong-authentication"></a>Kuruluş kullanım güçlü kimlik doğrulaması emin olun

Parolalar tahmin kötü amaçlı yazılım çalınırsa ya da yeniden kullanılabilir phished sıklığını verilen parola ile güçlü bir kimlik bilgisi çeşit geri – hakkında daha fazla bilgi edinmek için önemlidir [Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication).

### <a name="start-banning-commonly-attacked-passwords-and-turn-off-traditional-complexity-and-expiration-rules"></a>Başlangıç sık engellemeyi parolaları Saldırıya uğrayan ve geleneksel karmaşıklığı ve sona erme kuralları devre dışı bırakma.

Çoğu kuruluş (gerektiren özel karakterler, sayılar, büyük ve küçük harf) geleneksel karmaşıklığını ve parola süre sonu kuralları kullanır. [Microsoft research](https://aka.ms/passwordguidance) Bu ilkeler, kullanıcıların tahmin edilmesi kolaydır parolaları seçim neden göstermiştir.

Azure AD'nin [dinamik yasaklı parola](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad) özellik, kullanıcıların kolayca tahmin edilebilir parolaları ayarlama özelliğinden önlemek için geçerli bir saldırganın davranışı kullanır. Bu özellik, kullanıcıların bulutta oluşturulduğunda her zaman açık olan ancak dağıttıklarında ayrıca hibrit kuruluşlar için kullanıma sunuldu [Windows Server Active Directory için Azure AD parola koruması](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises). Azure AD parola koruması bu sık kullanılan parolaları seçme kullanıcıların engeller ve belirttiğiniz özel anahtar sözcükler içeren parola engelleyecek şekilde genişletilebilir. Örneğin, şirketinizin ürün adlarını veya bir yerel Spor takım içeren parolaları seçmesini kullanıcılarınızın engelleyebilirsiniz.

Microsoft öneriyor bağlı olarak aşağıdaki modern parola ilkesini benimseme [NIST kılavuzu](https://pages.nist.gov/800-63-3/sp800-63b.html):

1. Parolalar en az 8 karaktere sahip olması gerekir. Bunlar, kullanıcıların tahmin edilebilir bir parola seçin, parolaları dosyaları Kaydet veya bunları Not neden artık gerekmeyen daha iyi değildir.
2. Gibi bir kolayca tahmin edilebilecek parolaları kullanıcılara sürücü süre sonu kurallarını devre dışı **Spring2019!**
3. Karakter-birleştirme gereksinimleri devre dışı bırakın ve kullanıcıların parolalar tahmin edilebilir karakter değişimler seçmek neden olarak kullanıcıların yaygın olarak Saldırıya uğrayan parolaları seçmesini önleyerek.

Kullanabileceğiniz [parola süresinin dolmasını engellemek için PowerShell](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy) doğrudan Azure AD'de kimlik oluşturursanız, kullanıcılar için. Hibrit kuruluşlar kullanarak bu ilkeleri uygulamanız gerekir [etki alanı Grup İlkesi ayarları](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh994572(v%3dws.10)) veya [Windows PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/set-addefaultdomainpasswordpolicy).

### <a name="protect-against-leaked-credentials-and-add-resilience-against-outages"></a>Sızdırılan kimlik bilgilerine karşı koruma ve kesintilere karşı dayanıklılık ekleme

Doğrudan kimlik doğrulama veya Federasyon ile kuruluşunuzun karma kimlik çözümü kullanıyorsa, aşağıdaki iki nedenden dolayı parola karma eşitlemesini etkinleştirmeniz gerekir:

* [Kimlik bilgileri sızdırılan kullanıcılar](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events) Azure AD yönetim raporu "koyu Web'de." kullanıma sunması, kullanıcı adı ve parola çiftleri sizi uyarır Parolaların inanılmaz bir birim aracılığıyla kimlik avı, kötü amaçlı yazılım ve parola yeniden kullanımını üçüncü taraf sitelerinde daha sonra ihlal sızmış. Microsoft kimlik bilgilerinin sızdırılması bunların çoğu bulur ve, Bu raporda eşleşirlerse kimlik bilgileri, kuruluş – ancak yalnızca size bildiririz [parola karması eşitlemeyi etkinleştirme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization)!
* Şirket içi kesinti (örneğin, bir fidye yazılımı saldırısını) durumunda, kullanmaya geçmek mümkün olacaktır [parola karması eşitleme kullanarak bulut kimlik doğrulaması](https://docs.microsoft.com/azure/security/azure-ad-choose-authn). Bu yedekleme kimlik doğrulama yöntemi, Azure Active Directory'ye Office 365 dahil olmak üzere, kimlik doğrulaması için yapılandırılan uygulamalar erişmeye devam etmek izin verir. Bu durumda BT çalışanlarının şirket içi kesinti çözülene kadar veri paylaşımı için kişisel bir e-posta hesaplarına başvurmadan gerekmez.

Hakkında daha fazla bilgi [parola karma eşitlemesi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization) çalışır.

> [!NOTE]
> Parola Karması eşitlemeyi etkinleştirin ve Azure AD Etki Alanı Hizmetleri'ni kullanarak, Kerberos (AES 256) karmaları ve isteğe bağlı olarak NTLM (RC4, hiçbir güvenlik değeri) karmaları da şifrelenecek ve Azure AD'ye eşitlenen. 

### <a name="implement-ad-fs-extranet-smart-lockout"></a>AD FS extranet kilitleme akıllı uygulama

Kuruluşlar, doğrudan Azure AD'ye kimlik doğrulaması için uygulamaları yapılandırma yararlanabilir [Azure AD'ye akıllı kilitleme](https://docs.microsoft.com/azure/active-directory/active-directory-secure-passwords). Windows Server 2012 R2 ' AD FS kullanıyorsanız AD FS'yi uygulamaya [extranet kilitlenme koruması](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-soft-lockout-protection). Windows Server 2016'da AD FS kullanıyorsanız, uygulama [akıllı extranet kilitleme](https://support.microsoft.com/help/4096478/extranet-smart-lockout-feature-in-windows-server-2016). AD FS akıllı kilitleme karşı deneme yanılma korur Extranet saldırıları, kullanıcılar Active Directory'de kilitlenmelerini engelleyen sırasında hangi hedef AD FS.

### <a name="take-advantage-of-intrinsically-secure-easier-to-use-credentials"></a>Doğası gereği güvenli, kimlik bilgilerini kullanmayı daha kolay bir şekilde yararlanın

Kullanarak [Windows Hello](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification), parolalar, bilgisayarları ve mobil cihazlarda güçlü iki öğeli kimlik doğrulama ile değiştirebilirsiniz. Bu kimlik doğrulaması, kullanıcı kimlik bilgisi bir cihaza güvenli bir şekilde bağlanır ve bir biyometrik veya PIN kodu kullanan yeni bir tür oluşur.

## <a name="step-2---reduce-your-attack-surface"></a>2. adım - saldırı yüzeyinizi azaltmak

Parola güvenlik ihlali kapsamlılığıyla göz önünde bulundurulduğunda, kuruluşunuzdaki saldırı yüzeyini en aza önemlidir. Erişim girdisi sınırlama, eski ve daha az güvenli protokolleri kullanımı ortadan işaret ve daha önemli denetimin kaynaklarına yönetim erişimi uygulama saldırı yüzey alanını azaltmaya yardımcı olabilir.

### <a name="block-legacy-authentication"></a>Eski kimlik doğrulamasını engelleme

Azure AD kimlik doğrulaması ve şirket verilerine erişmek için kendi eski yöntemleri kullanarak uygulamaları kuruluşlar için başka bir risk yol açar. POP3, IMAP4 veya SMTP istemciler eski kimlik doğrulaması kullanan uygulamalar örnekleridir. Eski bir kimlik doğrulama uygulamaları, kullanıcı adına kimlik doğrulaması ve Azure AD güvenlik değerlendirmeleri Gelişmiş yapmak öğesinden engelleyebilirsiniz. Alternatif, modern kimlik doğrulaması çok faktörlü kimlik doğrulama ve koşullu erişim desteklediğinden, bir güvenlik riski düşürür. Aşağıdaki üç eylem önerilir:

1. Blok [AD FS kullanıyorsanız eski bir kimlik doğrulama](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/access-control-policies-w2k12).
2. Kurulum [SharePoint Online ve Exchange Online modern kimlik doğrulaması kullanacak şekilde](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-no-modern-authentication).
3. Kullanım [eski bir kimlik doğrulama engellemek için koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-conditions).

### <a name="block-invalid-authentication-entry-points"></a>Blok geçersiz kimlik doğrulama giriş noktaları

Varsay ihlal yapısı, koruyucuların kullanarak gerçekleştiğinde güvenliği aşılan kullanıcı kimlik bilgilerini etkisini azaltmak. Ortamınızdaki her uygulama için geçerli kullanım durumları göz önünde bulundurun: grupları, hangi ağları hangi cihazlar ve diğer öğeleri yetkiniz – sonra rest engelleyin. İle [Azure AD koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal), tanımladığınız belirli koşullara bağlı kendi uygulamaları ve kaynaklarına nasıl yetkili kullanıcılara erişimi denetleyebilirsiniz.

### <a name="block-end-user-consent"></a>Blok son kullanıcı onayı

OAuth 2.0 ve Microsoft Identity yararlanan uygulamalar vermek için izin varsayılan olarak, tüm kullanıcılar Azure AD'de [onay çerçevesine](https://docs.microsoft.com/azure/active-directory/develop/consent-framework) şirket verilerine erişim izni. Tümleştirip kullanıcıların, Microsoft 365 ve Azure ile tümleşen yararlı uygulamaları kolayca almaya izin vermez ancak bu bir güvenlik riski değilse kullanılan ve dikkatli bir şekilde izlenen. [Tüm gelecek kullanıcı onayı işlemleri devre dışı bırakma](https://docs.microsoft.com/azure/active-directory/manage-apps/methods-for-removing-user-access) yüzey alanınızı azaltmak ve bu riskin azaltılmasına yardımcı olabilir. Son kullanıcı onayı önceki devre dışı bırakılırsa izin veren hala kabul edilir, ancak bir yönetici tarafından tüm onaylar işlemlerinin gerçekleştirilmesi gerekir. Bu işlev devre dışı bırakmadan önce kullanıcıların yeni uygulamalar için yönetici onayı iste nasıl anladığınızdan emin olun önerilir; Bunun yapılması yardımcı kullanıcı uyuşmazlıkları azaltmak, destek birimi en aza indirmek ve Azure olmayan AD kimlik bilgilerini kullanan uygulamalar için kullanıcıların kaydolmayın emin olun.

### <a name="implement-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management uygulamak

"İhlal varsayımı" başka bir etkisi, gizliliği tehlikeye giren hesap ayrıcalıklı bir role ile çalışabilir olasılığını en aza indirmek için gerekli değildir. 

[Azure AD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) yardımcı olarak hesap ayrıcalığı küçültülmüş yardımcı olur:

* Tanımlamak ve yönetici rollerine atandığı kullanıcılar yönetin.
* Kullanılmayan anlamak veya aşırı ayrıcalıklı rollerini kaldırmanız gerekir.
* Multi-Factor authentication tarafından korunan emin ayrıcalıklı rolleri yapmak için kurallar oluşturun.
* Ayrıcalıklı bir görevi tamamlamak için ayrıcalıklı rolleri yalnızca yeteri kadar verilen emin olmak için kurallar oluşturun.

Azure AD PIM etkinleştirin ve ardından yönetim rolleri atanır ve bu rollerdeki gereksiz hesaplarını kaldırmak kullanıcıları görüntüleyin. Kalan ayrıcalıklı kullanıcılar için kalıcı için uygun taşıyabilirsiniz. Son olarak, emin olmak için uygun ilkeleri oluşturmak ayrıcalıklı roller erişmek ihtiyaç duydukları, bunlar güvenli bir şekilde bunu, denetim gerekli ile değiştirin.

Ayrıcalıklı hesap işleminizi dağıtma bir parçası olarak izleyin [açısından en iyisi, en az iki Acil Durum hesapları oluşturmak için](https://docs.microsoft.com/azure/active-directory/admin-roles-best-practices) varsa Azure ad erişim, kendiniz kilitlemesine emin olmak için.

## <a name="step-3---automate-threat-response"></a>3. adım: tehdit yanıtı otomatikleştirme

Azure Active Directory, otomatik olarak algılama ve yanıtlama arasındaki gecikme kaldırmak için saldırıları, müdahale çok özellikleri vardır. Maliyetleri düşürebilir ve kendilerini ortamınıza eklemek için zaman Suçları azalttığınızda riskleri kullanın. Gerçekleştirebileceğiniz somut adımlar aşağıda verilmiştir.

### <a name="implement-user-risk-security-policy-using-azure-ad-identity-protection"></a>Kullanıcı riski İlkesi Azure AD kimlik Koruması'nı kullanarak uygulama

Kullanıcı riski gösteren bir kullanıcının kimlik bilgilerini açığa çıkardığını ve hesaplanır olanağını temel alarak [kullanıcı risk olaylarını](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) kullanıcı kimliğiyle ilişkili olan. Kullanıcı riski İlkesi risk düzeyi belirli kullanıcı veya grup için değerlendirilen bir koşullu erişim ilkesi var. Azaldı dayalı, Orta, yüksek risk düzeyi, bir ilke multi-Factor authentication kullanarak güvenli parola değişikliği gerektiren ya da erişimi engellemek için yapılandırılabilir. Microsoft'un yüksek riskli kullanıcı güvenli parola değişikliği iste önerilir.

![Riskli oldukları belirlenen kullanıcılar](media/azure-ad/azure-ad-sec-steps1.png)

### <a name="implement-sign-in-risk-policy-using-azure-ad-identity-protection"></a>Azure AD kimlik Koruması'nı kullanarak uygulama oturum açma riski İlkesi

Oturum açma riski, hesap sahibi dışındaki bir şahsa kimliğini kullanarak oturum açmaya çalışıyor olasılığıdır. A [oturum açma riski İlkesi](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) risk düzeyi belirli kullanıcı veya grup için değerlendirilen bir koşullu erişim ilkesi. Risk düzeyi (Orta/Yüksek/düşük) göre bir ilke erişimi engellemeye veya çok faktörlü kimlik doğrulaması zorlamak için yapılandırılabilir. Çok faktörlü kimlik doğrulaması ortamdaki veya riskli oturum açma işlemleri üzerinde zorla emin olun.

![Anonim Ip'lerden gelen oturum açın](media/azure-ad/azure-ad-sec-steps2.png)

## <a name="step-4---increase-your-awareness"></a>4. adım -, bilinirliğinizi artırın

Denetim ve güvenlikle ilgili olaylar ve ilgili uyarıları günlük kaydını verimli koruma stratejisi için temel bileşenleridir. Kuşkulu etkinlikleri ve ağ ve iç saldırıları denenmedi veya başarılı dış sızma gösterebilecek desenleri algılamak Yardım elektronik kaydıyla, güvenlik günlüklerini ve raporları sağlar. Denetim kullanıcı etkinliğini, belge yönetmeliklere uygunluk, adli analiz ve daha fazlasını gerçekleştirmek için kullanabilirsiniz. Uyarılar, bildirimler güvenlik olayları sağlar.

### <a name="monitor-azure-ad"></a>Azure AD İzleyicisi

Microsoft Azure Hizmetleri ve Özellikler Denetim ve günlüğe kaydetme seçenekleri ihlallerini önlemeye yardımcı olmak için bu boşlukları adres ve güvenlik ilkeleri ve mekanizmaları boşlukları belirlemesine yardımcı olmak için yapılandırılabilir güvenlik sağlar. Kullanabileceğiniz [Azure günlük kaydı ve Denetim](https://docs.microsoft.com/azure/security/azure-log-audit) ve [denetim etkinlik raporları Azure Active Directory portalındaki](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs).

### <a name="monitor-azure-ad-connect-health-in-hybrid-environments"></a>Azure AD Connect Health Karma ortamlarda izleyin

[Azure AD Connect Health ile AD FS izleme](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs) olası sorunları ve AD FS altyapınıza yapılan saldırıları görünürlüğünü büyük öngörü sağlar. Azure AD Connect Health uyarıları ayrıntılarını, çözüm adımları ve bağlantıları ile ilgili belgeler için sunar; kimlik doğrulama trafiği için ilgili çeşitli ölçümleri yönelik kullanım analizi; performans izleme ve raporlar.

![Azure AD Connect Health](media/azure-ad/azure-ad-sec-steps4.png)

### <a name="monitor-azure-ad-identity-protection-events"></a>Azure AD Identity Protection olaylarını izleme

[Azure AD kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) büyük bildirimi, izleme ve Raporlama Aracı, kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılama için kullanabilirsiniz. Sızan kimlik bilgileri, mümkün olmayan seyahat gibi risk olaylarını algılar ve oturum açma işlemleri virüs bulaşmış cihazlar, anonim IP adresleri, IP adresleri şüpheli etkinlikleri ve konumları bilinmeyen ile ilişkili. Risk altındaki kullanıcılar, e-posta ve/veya Haftalık Özet e-posta almak bildirim uyarıları etkinleştirin.

Azure AD kimlik koruması, günlük izlemeniz iki önemli raporları sağlar:
1. Riskli oturum açma raporları araştırmanız gereken kullanıcı oturum açma etkinlikleri belirir, meşru sahibi oturum açma yapmamış.
2. Mümkün olmayan seyahat olaya neden farklı konumlardan kullanıcının oturum açtığı ya da riskli kullanıcı raporları, algılandı sızan kimlik bilgileri gibi tehlikeye girmiş olabilecek kullanıcı hesaplarının belirir. 

![Riskli oldukları belirlenen kullanıcılar](media/azure-ad/azure-ad-sec-steps3.png)

### <a name="audit-apps-and-consented-permissions"></a>Denetim uygulamalar ve onay verilmiş izinleri

Kullanıcılar, güvenliği aşılmış bir web sitesi ya da, profil bilgileri ve kullanıcı verilerine erişim, kullanıcıların e-posta gibi tabanına uygulamalarda gezinme içine sağladı. Kötü amaçlı bir aktör, posta kutusu içeriklerinin şifrelemek ve posta kutusu verilerinizi yeniden elde etmek için bir ransom talep için alınan onay verilmiş izinleri kullanabilirsiniz. [Yöneticiler gözden geçirmeniz ve Denetim](https://docs.microsoft.com/office365/securitycompliance/detect-and-remediate-illicit-consent-grants) kullanıcılar tarafından verilen izinler.

## <a name="step-5---enable-end-user-self-help"></a>Adım 5 - Enable son kullanıcı kendi kendine yardım

Mümkün olduğunca üretkenlik ile güvenlik Bakiye isteyebilirsiniz. Güvenlik temeli uzun vadede tutunun anlayış ile yolculuğunuza yaklaşan, aynı satırlar, dikkatli kalırken kullanıcıların sağlayarak kuruluşunuzdan uyuşmazlıkları kaldırabilirsiniz. 

### <a name="implement-self-service-password-reset"></a>Self Servis parola sıfırlama uygulayın

Azure'nın [Self Servis parola sıfırlama (SSPR)](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started) sıfırlama veya kendi parolalarını veya hesaplarını yönetici müdahalesi olmadan kilidini açmasına izin vermek, BT yöneticileri için basit bir yol sunar. Sistem, kullanıcıların sisteme erişimini izleyen ayrıntılı raporlama içerir, ayrıca kötüye kullanım veya uygunsuz kullanım konusunda uyaran bildirimler sağlar. 

### <a name="implement-self-service-group-management"></a>Self Servis Grup Yönetimi uygulama

Azure AD güvenlik grupları ve Office 365 grupları ile kaynaklara erişimi yönetme olanağı sağlar. Bu gruplar, BT yöneticileri yerine Grup sahipleri tarafından yönetilebilir. Bilinen [Self Servis Grup Yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), bu özellik, oluşturmak ve kendi isteklerini işlemek için Yöneticiler üzerinde bağlı olmadan grupları yönetmek için bir yönetici rolü atanmamış Grup sahipleri sağlar.

### <a name="implement-azure-ad-access-reviews"></a>Uygulama Azure AD erişim gözden geçirmeleri

İle [Azure AD erişim gözden geçirmeleri](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), grup üyeliklerini, Kurumsal uygulamalara erişimi yönetebilir ve kullanıcılar için erişim sağlamaz bir güvenlik standardı bulunduğundan emin olmak için ayrıcalıklı rol atamalarını dönemlerini genişletilmiş zaman zaman gerekmez.

## <a name="summary"></a>Özet

Güvenli bir kimlik altyapısı için birçok yönden vardır, ancak bu beş adım denetim listesini hızla güvenli ve güvenli kimlik altyapısını gerçekleştirmenize yardımcı olur:

* Kimlik bilgilerinizi güçlendirin.
* Saldırı yüzey alanını azaltın.
* Tehdit yanıt otomatikleştirin.
* Denetleme ve izleme, bilinirliğinizi artırın.
* Kendi kendine yardım ile daha tahmin edilebilir ve tüm son kullanıcı güvenlik sağlar.

Nasıl ciddi bir şekilde kimlik güvenliği alın ve daha güvenli bir duruş kuruluşunuz için kullanışlı bir yol haritası bu belgeyi Umarım veriyoruz.

## <a name="next-steps"></a>Sonraki adımlar
Planlama ve dağıtım önerileri için yardıma ihtiyacınız varsa başvurmak [Azure AD proje dağıtım planları](https://aka.ms/deploymentplans) Yardım.
