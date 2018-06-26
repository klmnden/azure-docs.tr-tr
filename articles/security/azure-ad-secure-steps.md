---
title: Azure Active Directory'de kimlik altyapınızı güvenli hale getirmek için beş adımı
description: Bu belge Yöneticiler güvenli kuruluşlarının Azure AD özelliklerini kullanarak yardımcı olmak için uygulamalıdır önemli eylemlerin bir listesini özetler
services: active-directory
author: martincoetzer
manager: manmeetb
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 06/18/2018
ms.author: martincoetzer
ms.openlocfilehash: b15fff6e868bfac973f9d2a7277f0fac1e29d845
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938520"
---
# <a name="five-steps-to-securing-your-identity-infrastructure"></a>Kimlik altyapınızı güvenliğini sağlamak için beş adımı

Bu belge okuduğunuz, güvenlik önemini kullanan. Büyük olasılıkla zaten kuruluşunuzun güvenliğini sağlamaya yönelik sorumluluğu taşır. Başkalarının güvenlik önemini ikna ihtiyacınız varsa, bunları en son okumak için Gönder [Microsoft Güvenlik Intelligence rapor](https://www.microsoft.com/security/intelligence-report).

Bu belge, kuruluşunuzun siber saldırılarına karşı inoculate için beş adım denetim listesini kullanarak Azure Active Directory özelliklerini kullanarak daha güvenli bir duruş size yardımcı olacak.

Bu denetim açıklayarak hemen kuruluşunuzu korumak için kritik önerilen eylemler hızlı dağıtmanıza yardımcı olur nasıl yapılır:

* Kimlik bilgilerinizi güçlendirme.
* Saldırı yüzey alanınızı azaltın.
* Tehdit yanıt otomatikleştirin.
* Denetim ve izleme, tanıma artırın.
* Kendi kendine yardım ile daha tahmin edilebilir ve eksiksiz son kullanıcı güvenlik sağlar.

> [!NOTE]
> Bu belgedeki önerilerin çoğu yalnızca uygulamaları, Azure Active Directory, kimlik sağlayıcısı olarak kullanmak üzere yapılandırılmış için geçerlidir. Kimlik bilgisi ilkeleri, tehdit algılama, avantajları sağlar uygulamaları için çoklu oturum açmayı yapılandırma bu uygulamaları denetim, oturum açma ve diğer özellikleri ekleyin. [Çoklu oturum açma Azure Active Directory üzerinden](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-sso) tüm bu önerileri dayalı - temelidir.

## <a name="before-you-begin-protect-privileged-accounts-with-mfa"></a>Başlamadan önce: MFA ile ayrıcalıklı hesaplara koruma

Bu denetim başlamadan önce bu denetim okuduğunuz sırada, güvenliği aşılan yok emin olun. İlk ayrıcalıklı hesaplarınızdaki korumak için gerekir.

Bu hesapları ilk korumak için kritik olacak şekilde, ayrıcalıklı hesapların denetim elde saldırganlar inanılmaz hasar yapabilirsiniz. Gerekli ve [Azure çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) (MFA) kullanarak kuruluşunuzda içindeki tüm yöneticiler için [temel koruma](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-baseline-protection). MFA uygulanırsa yüklemediyseniz şimdi yapın! Bu önemlidir.

Tüm kümesi? Denetim listesinde başlayalım.

## <a name="step-1---strengthen-your-credentials"></a>1. adım - kimlik bilgilerinizi güçlendirme 

Çoğu Kurumsal güvenlik ihlallerini parola sprey, ihlal yeniden yürütme ya da kimlik avı gibi yöntemleri sayıda biriyle tehlikeye bir hesapla kaynaklanır. Bu videoda bu saldırıların hakkında daha fazla bilgi edinin:
> [!VIDEO https://channel9.msdn.com/events/Ignite/Microsoft-Ignite-Orlando-2017/BRK3016/player]

Kullanıcıların kimliğini sisteminizdeki Zayıf parolalar kullanıyorsanız ve bunları güçlendirme çok faktörlü kimlik doğrulamasıyla değil, sağlasa da, mı yoksa, – yalnızca "ne sıklıkta." aşılan olduğunda değil

### <a name="make-sure-your-organization-use-strong-authentication"></a>Kuruluş güçlü kimlik doğrulamasını kullan emin olun

Tahmin parolaları kötü amaçlı yazılım çalınırsa ya da yeniden, phished sıklığını verilen parola güçlü kimlik bilgisi çeşit ile geri – daha fazla bilgi edinmek için önemlidir [Azure çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication).

### <a name="turn-off-traditional-complexity-expiration-rules-and-start-banning-commonly-attacked-passwords-instead"></a>Geleneksel karmaşıklık, sona erme kuralları devre dışı bırakma ve yaygın olarak Saldırıya uğrayan parolalar yerine banning Başlat

Birçok kuruluş, geleneksel karmaşıklığı (örneğin, özel karakterler) ve parola süre sonu kuralları kullanın. Microsoft'un araştırma, bu ilkeler tahmin edilmesi daha kolay parolalar seçmelerini neden zararlı göstermiştir.

Microsoft'un önerileri, tutarlı [NIST kılavuzu](https://pages.nist.gov/800-63-3/sp800-63b.html), aşağıdaki üç uygulamak için:

1. Parolalar en az 8 karakter olması gerekir. Artık kullanıcıların tahmin edilebilir parolalar seçmesini, parolaları dosyalarının kaydedileceği veya not almanız neden mutlaka daha iyi değil.
2. Kullanıcıların kolayca tahmin edilen parolalar gibi sürücü sona erme kuralları devre dışı **Summer2018!**.
3. Karakter birleşim gereksinimleri devre dışı bırakın ve kullanıcıların parolaları tahmin edilebilir karakter değişimler seçmek neden olarak kullanıcıların yaygın olarak Saldırıya uğrayan parolalar seçmesini önleyerek.

Kullanabileceğiniz [parola süresinin dolmasını engellemek için PowerShell](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-policy) doğrudan Azure AD'de kimlikleri oluşturursanız, kullanıcılar. Şirket içi kullanan kurumlar (karma bir dağıtım da bilinir) Azure AD eşitleme kimlikleri için Azure AD Connect ile AD şirket içi uygulamanız gerekir [akıllı parola ilkeleri](https://aka.ms/passwordguidance) kullanarak [etki alanı Grup İlkesi ayarları](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh994572(v%3dws.10)) veya [Windows PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/set-addefaultdomainpasswordpolicy).

Azure Active Directory'nin [dinamik Engellenenler parola](https://docs.microsoft.com/azure/active-directory/active-directory-secure-passwords) özelliği, kullanıcıların kolayca tahmin edilebilir parolaları ayarından önlemek için geçerli saldırganın davranışı kullanır. Bu özellik her zaman açıktır ve karma bir dağıtım kuruluşlarla avantajını bu özelliğini etkinleştirerek [parola geri yazma](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-writeback) veya dağıtabilmeniz için [Windows Server Active için Azure AD parola koruması Dizin](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises). Genel sık kullanılan bir parola seçme kullanıcıların Azure AD parola koruması engeller ve özel parolalar yapılandırabilirsiniz.

### <a name="protect-against-leaked-credentials-and-add-resilience-against-outages"></a>Sızan kimlik bilgilerine karşı koruma ve esnekliği kesintilere karşı ekleyin

Kuruluşunuzun karma kimlik çözümü kullanıyorsa, aşağıdaki iki nedenden dolayı parola karma eşitlemesini etkinleştirmeniz gerekir:

* [Sızan kimlik bilgilerine sahip kullanıcılar](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events) Azure AD yönetim raporu "koyu Web'de." sunulan kullanıcı adı ve parola çiftleri sizi uyarır Parolaların olağanüstü bir birim, daha sonra ihlal üçüncü taraf sitelerinde kimlik avı, kötü amaçlı yazılım ve parolanızı yeniden aracılığıyla sızmış. Microsoft bunların çoğu sızmasını kimlik bilgileri bulur ve, Bu raporda, eşleşirlerse kimlik bilgileri, kuruluş – ancak yalnızca size söyleyecektir [parola karma eşitlemesini etkinleştir](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization)!
* Şirket içi kesinti (örneğin, gelen bir yazılımı saldırısı) olması durumunda, geçmek kullanabileceksiniz [bulut kimlik doğrulaması parola karması eşitlemesi kullanarak](https://docs.microsoft.com/azure/security/azure-ad-choose-authn). Bu yedekleme kimlik doğrulama yöntemi, Azure Active Directory'yle Office 365 de dahil olmak üzere, kimlik doğrulaması için yapılandırılan uygulamalara erişen devam etmenize izin verir.

Hakkında daha fazla bilgi [parola karması eşitlemesi](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization) çalışır.

### <a name="implement-ad-fs-extranet-lockout"></a>Uygulama AD FS extranet kilitleme

Doğrudan Azure AD kimlik doğrulaması için uygulamaları yapılandırma kuruluşlar yararlı [Azure AD akıllı kilitleme](https://docs.microsoft.com/azure/active-directory/active-directory-secure-passwords). AD FS kullanıyorsanız AD FS'yi uygulamaya [extranet kilitleme](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-extranet-lockout-protection). Extranet kilitleme, Active Directory'de kullanıcıları olmasını önleyen sırasında hangi hedef AD FS kilitli deneme yanılma saldırılarına karşı korur.

### <a name="take-advantage-of-intrinsically-secure-easier-to-use-credentials"></a>Doğası gereği güvenli, kimlik bilgilerini kullanmayı daha kolay yararlanın

Kullanarak [Windows Hello](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification), bilgisayarları ve mobil cihazlarda güçlü iki öğeli kimlik doğrulama ile parola değiştirebilirsiniz. Bu kimlik doğrulaması yeni bir cihaza bağlanır ve bir biyometrik veya PIN kodu kullanan kullanıcı kimlik bilgisi türünü oluşur.

## <a name="step-2---reduce-your-attack-surface"></a>2. adım - saldırı yüzeyini küçültmek

Parola güvenlik aşılması pervasiveness verildiğinde, kuruluşunuzdaki saldırı yüzeyini en aza önemlidir. Erişim girdisi sınırlama, daha eski, daha az güvenli protokollerinin kullanımını ortadan işaret ve kaynaklarına yönetim erişimi daha önemli denetimini kullanan saldırı alanını azaltmaya yardımcı olabilir.

### <a name="block-legacy-authentication"></a>Blok eski kimlik doğrulaması

Azure AD ile kimlik doğrulaması ve şirket verilerine erişmek için kendi eski yöntemleri kullanarak uygulamaları kuruluşlar için başka bir risk teşkil. POP3, IMAP4 veya SMTP istemcileri eski kimlik doğrulaması kullanan uygulamalar örnekleridir. Eski kimlik doğrulama uygulamalar kullanıcı adına kimlik doğrulaması ve Azure AD güvenlik değerlendirmeleri Gelişmiş yapmasının engelleyebilirsiniz. Alternatif, modern kimlik doğrulaması, çok faktörlü kimlik doğrulama ve koşullu erişim desteklediğinden güvenlik riskini azaltır. Aşağıdaki üç eylem öneririz:

1. Blok [AD FS kullanıyorsanız eski kimlik doğrulama](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/access-control-policies-w2k12).
2. Kurulum [SharePoint Online ve Exchange Online modern kimlik doğrulaması kullanacak şekilde](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-no-modern-authentication).
3. Kullanım [eski kimlik doğrulaması engellemek için koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-conditions#legacy-authentication).

### <a name="block-invalid-authentication-entry-points"></a>Blok geçersiz kimlik doğrulama giriş noktaları

Varsay ihlali mentality kullanarak, bunlar gerçekleştiğinde güvenliği aşılmış kullanıcı kimlik bilgilerini etkisini azaltmak. Ortamınızdaki her uygulama için geçerli kullanım örneklerini göz önünde bulundurun: grupları, hangi ağların hangi cihazların ve diğer öğeleri yetkilendirileceğini – sonra kalan engelleyin. Dikkatli kullanımını kısıtlamak için [yüksek ayrıcalıklı veya hizmet hesaplarını](https://docs.microsoft.com/azure/active-directory/admin-roles-best-practices). İle [Azure AD koşullu erişimi](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal), tanımladığınız kendi uygulamalarına ve kaynaklarına göre belirli koşullara göre nasıl yetkili kullanıcılara erişimi denetleyebilirsiniz.

Belirli hizmet hesapları (otomatik bir şekilde görevleri yapmak için kullanılan hesaplar) dikkat edin. Koşullu erişim kullanarak, bu tür hesaplara IP, yalnızca hizmet karşı çalıştırabilirsiniz ve gün zaman uygun olduğu emin olabilirsiniz.

### <a name="implement-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management uygulaması

"İhlali varsayın" başka bir etkisi gizliliği tehlikeye giren hesap ayrıcalıklı bir rolü ile çalışabilir olasılığını en aza indirmek için gerekiyor. 

[Azure AD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) yardımcı olarak hesap ayrıcalığı küçültülmüş yardımcı olur:

* Tanımlamak ve yönetici rollerine atanan kullanıcılar yönetin.
* Kullanılmayan anlamak veya aşırı ayrıcalıklı rollerini kaldırmanız gerekir.
* Çok faktörlü kimlik doğrulamasını korumalı emin ayrıcalıklı rolleri yapmak için kurallar oluşturun.
* Ayrıcalıklı rolleri yalnızca yetecek kadar uzun süre ayrıcalıklı görevi gerçekleştirmek için verilen emin olmak için kurallar oluşturun.

Azure AD PIM etkinleştirmek sonra yönetici rollerini atanır ve bu rolleri gereksiz hesaplarını kaldırmak kullanıcıları görüntüleyin. Kalan ayrıcalıklı kullanıcılar için kalıcı için uygun taşıyabilirsiniz. Son olarak, emin olmak için uygun politikalarını ayrıcalıklı rollerin erişmek ihtiyaç duydukları, bunlar güvenli bir şekilde bunu yapabilirsiniz.

Ayrıcalıklı bir hesap işleminizi dağıtma bir parçası olarak izleyin [açısından en iyisi, en az iki Acil Durum hesapları oluşturmak için](https://docs.microsoft.com/azure/active-directory/admin-roles-best-practices) varsa Azure AD erişimi, kendiniz kilitlemesine emin olmak için.

## <a name="step-3---automate-threat-response"></a>3. adım - threat yanıt otomatikleştirme

Azure Active Directory otomatik olarak algılama ve yanıt arasındaki gecikme süresi kaldırmak için saldırıları, müdahale birçok özellikleri vardır. Maliyetleri azaltabilir ve zaman suçlular azaltmak, riskler, kendilerini ortamınıza eklemek için kullanın. Yapabileceğiniz somut adımlar şunlardır.

### <a name="implement-user-risk-security-policy-using-azure-ad-identity-protection"></a>Azure AD Identity Protection kullanarak kullanıcı risk güvenlik ilkesi uygulama

Kullanıcı risk gösteren bir kullanıcının kimliğini tehlikeye girdiğini ve hesaplanır olasılığını temel alarak [kullanıcı risk olaylarına](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) bir kullanıcı kimliğiyle ilişkili olan. Bir kullanıcı risk belirli bir kullanıcı veya grup için risk düzeyi değerlendiren bir koşullu erişim ilkesi ilkesidir. Azaldı bağlı olarak, Orta, yüksek risk düzeyi, bir ilke erişimi engellemeye veya çok faktörlü kimlik doğrulama kullanarak güvenli parola değişikliği gerektiren şekilde yapılandırılabilir. Microsoft'un yüksek riskli kullanıcılar için güvenli parola değişikliği gerektirecek şekilde önerilir.

![Riskli oldukları belirlenen kullanıcılar](media/azure-ad/azure-ad-sec-steps1.png)

### <a name="implement-sign-in-risk-policy-using-azure-ad-identity-protection"></a>Azure AD Identity Protection kullanarak uygulama oturum açma riski İlkesi

Oturum açma birisi hesap sahibi dışındaki kimliğini kullanarak oturum açmaya çalışıyor olasılığını riskidir. A [oturum açma risk ilkesine](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) risk düzeyini belirli bir kullanıcı veya grup için değerlendirilen bir koşullu erişim ilkesi. Risk düzeyi (yüksek/orta/düşük) bağlı olarak, bir ilke erişimi engellemeye veya çok faktörlü kimlik doğrulaması zorlamak için yapılandırılabilir. Orta veya üzeri risk oturum açma işlemleri çok faktörlü kimlik doğrulaması zorla emin olun.

![Anonim Ip'lerden oturum açın](media/azure-ad/azure-ad-sec-steps2.png)

## <a name="step-4---increase-your-awareness"></a>4. adım - artış, tanıma

Denetim ve güvenlikle ilgili olaylar ve ilgili uyarıları günlük kaydını verimli koruma stratejisi, temel bileşenleridir. Güvenlik günlüklerini ve raporları kuşkulu etkinlikleri ve ağ ve iç saldırıları denenen veya başarılı dış sızma gösterebilir desenlerini algılayabilir Yardım elektronik bir kayıtla sağlar. Denetim kullanıcı etkinliği, belge Mevzuat uyumluluğu izlemek, adli analiz gerçekleştirmek için kullanabilirsiniz. Uyarıları, bildirimleri güvenlik olaylarının sağlar.

### <a name="monitor-azure-ad"></a>Azure AD İzleyicisi

Microsoft Azure Hizmetleri ve Özellikler Denetim ve güvenlik ilkeleri ve mekanizmaları kapsamın açıklarını tanımlamanıza ve ihlallerinden önlemeye yardımcı olmak için bu boşluklar adres yardımcı olmak için seçenekleri günlük yapılandırılabilir güvenlik sağlar. Kullanabileceğiniz [Azure günlüğe kaydetme ve Denetim](https://docs.microsoft.com/azure/security/azure-log-audit) ve [denetim etkinlik raporları Azure Active Directory portalında](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs).

### <a name="monitor-azure-ad-connect-health-in-hybrid-environments"></a>Azure AD Connect Health Karma ortamlarda izleme

[Azure AD Connect Health ile ADFS izleme](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs) olası sorunları ve ADFS altyapınızı saldırılar görünürlüğünü büyük bir anlayış ile sağlar. Azure AD Connect Health uyarıları ayrıntılarını, çözüm adımları ve bağlantıları ile ilgili belgeler için sunar; kimlik doğrulama trafiğini ilgili çeşitli ölçümler için kullanım analizi; performans izleme ve raporlar.

![Azure AD Connect Health](media/azure-ad/azure-ad-sec-steps4.png)

### <a name="monitor-azure-ad-identity-protection-events"></a>Azure AD Identity Protection olaylarını izleme

[Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) izleme ve Raporlama Aracı kuruluşunuzdaki kimlikleri etkileyen olası güvenlik açıklarını algılamak için kullanabileceğiniz bir bildirim. Sızan kimlik bilgileri, mümkün olmayan seyahat gibi risk olaylarını algılar ve oturum açmalardan aygıtlar, anonim IP adresleri, şüpheli etkinlik ve bilinmeyen konumları ile ilişkili IP adreslerini virüs bulaşmış. Risk altındaki kullanıcılar e-posta ve/veya Haftalık Özet e-posta almak bildirim uyarıları etkinleştirin.

![Riskli oldukları belirlenen kullanıcılar](media/azure-ad/azure-ad-sec-steps3.png)

## <a name="step-5---enable-end-user-self-help"></a>Adım 5 - Enable son kullanıcı kendi kendine yardım

Mümkün olduğunca, güvenlik verimliliğini Bakiye istersiniz. Yolculuğunuzun bir temel güvenlik için uzun vadede ayarladığınızı bakış ile yaklaşan aynı satır, kullanıcılarınızın temkinli kalan sırasında güçlendirilmesi tarafından kuruluşunuzdan uyuşmazlık kaldırabilirsiniz. 

### <a name="implement-self-service-password-reset"></a>Self Servis parola sıfırlama uygulama

Azure'nın [Self Servis parola sıfırlama (SSPR)](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-getting-started) basit bir yol sıfırlama veya bunların parolaları veya yönetici müdahalesi olmadan hesapları kilidini açmasına olanak vermek BT yöneticileri için sunmaktadır. Sistem, kullanıcıların sisteme erişimini izleyen ayrıntılı raporlama içerir, ayrıca kötüye kullanım veya uygunsuz kullanım konusunda uyaran bildirimler sağlar. 

### <a name="implement-self-service-group-management"></a>Self Servis Grup Yönetimi uygulamak

Azure AD güvenlik grupları ve Office 365 grupları kullanarak kaynaklarına erişimi yönetme olanağı sağlar. Bu gruplar, BT yöneticileri yerine Grup sahipleri tarafından yönetilebilir. Bilinen [Self Servis Grup Yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), bu özellik grupları oluşturmak ve kendi isteklerini işlemek için Yöneticiler bağlı kalmadan yönetmek için bir yönetici rolü atanmamış grup sahiplerine sağlar.

### <a name="implement-azure-ad-access-reviews"></a>Uygulama Azure AD erişim gözden geçirme

İle [Azure AD erişim incelemeleri](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview), grup üyelikleri, Kurumsal uygulamalara erişimi yönetebilir ve kullanıcılar için erişimi sağlamaz güvenlik standart korumak emin olmak için ayrıcalıklı rol atamaları dönemlerini genişletilmiş zaman zaman gerekmez.

## <a name="summary"></a>Özet

Güvenli bir kimlik altyapısı için birçok nokta, ancak bu beş adımı denetim daha güvenli ve güvenli kimlik altyapısı hızlı bir şekilde gerçekleştirmenize yardımcı olur:

* Kimlik bilgilerinizi güçlendirme.
* Saldırı yüzey alanınızı azaltın.
* Tehdit yanıt otomatikleştirin.
* Denetim ve izleme, tanıma artırın.
* Kendi kendine yardım ile daha tahmin edilebilir ve eksiksiz son kullanıcı güvenlik sağlar.

Nasıl ciddi kimlik güvenlik alabilir ve bu belge, kuruluşunuz için daha güvenli bir duruş için kullanışlı bir yol haritası değil umuyoruz veriyoruz.

## <a name="next-steps"></a>Sonraki adımlar
Planlama ve öneriler dağıtmak için yardıma gereksinim duyarsanız başvurmak [Azure AD proje dağıtım planları](http://aka.ms/deploymentplans) Yardım için.
