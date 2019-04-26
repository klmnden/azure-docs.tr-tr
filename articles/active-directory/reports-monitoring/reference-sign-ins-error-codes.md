---
title: Azure Active Directory portalında bulunan oturum açma etkinlik raporundaki hata kodları | Microsoft Docs
description: Oturum açma etkinlik raporu hata kodları başvurusu.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/18/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: a8fefc2eb2e642b5c7ac93b8a1cfc25a54c3b646
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60437177"
---
# <a name="sign-in-activity-report-error-codes"></a>Oturum açma etkinlik raporundaki hata kodları 

Tarafından sağlanan bilgilerle [kullanıcı oturum açma işlemleri raporu](concept-sign-ins.md), aşağıdakiler gibi soruların yanıtlarını bulabilirsiniz:

- Kimin uygulamama açan?
- Hangi uygulamalar için doğru açıldı?
- Hangi oturum açma neden başarısız oldu?

Bir oturum açma başarısız olduğunda, hataya karşılık gelen bir hata kodunu görürsünüz. Bu makalede, uygunsa hata kodlarını ve açıklamalarını, önerilen eylem birlikte listeler. 

## <a name="how-can-i-display-failed-sign-ins"></a>Başarısız oturum açma girişimlerini nasıl görüntüleyebilirim? 

Gidin [oturum açma işlemleri raporu](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns) içinde [Azure portalında](https://portal.azure.com).

![Oturum açma etkinliği](./media/reference-sign-ins-error-codes/61.png "oturum açma etkinliği")

Tüm başarısız oturum açma işlemleri görüntülemek için Raporu filtrelemek **hatası** gelen **oturum açma durumu** açılan kutusu.

![Oturum açma etkinliği](./media/reference-sign-ins-error-codes/06.png "oturum açma etkinliği")

Filtrelenmiş listeden bir öğe seçildiğinde açılır **Etkinlik ayrıntıları: Oturum açma işlemleri** dikey penceresi. Bu görünüm, başarısız oturum açma olayı hakkında ek bilgi sağlar dahil olmak üzere **oturum açma hata kodu** ve **hata nedeni**.

![Oturum açma etkinliği](./media/reference-sign-ins-error-codes/05.png "oturum açma etkinliği")

Oturum açma verilerini kullanarak programlama yoluyla da erişebilir [raporlama API'sini](concept-reporting-api.md).

## <a name="error-codes"></a>Hata kodları


|Hata|Açıklama|
|---|---|
|16000|Bir iç uygulama ayrıntısı ve bir hata durumu budur. Bu başvuru güvenle yok sayabilirsiniz.|
|20001|Federasyon Kimlik Sağlayıcısı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun.|
|20012|Federasyon Kimlik Sağlayıcısı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun.|
|20033|Federasyon Kimlik Sağlayıcısı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun.|
|40008|Federasyon Kimlik Sağlayıcısı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun.|
|40009|Federasyon Kimlik Sağlayıcısı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun.|
|40014|Federasyon Kimlik Sağlayıcısı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun.|
|50000|Oturum açma hizmetimizle ilgili bir sorun var. Bu sorunu çözmek için bir [destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md).|
|50001|Hizmet asıl adı bu kiracıda bulunamadı. Uygulama kiracının Yöneticisi tarafından yüklenmedi veya kaynak sorumlusu dizinde bulunamamış veya geçersiz yoksa bu durum oluşabilir.|
|50002|Kiracıda kısıtlı proxy erişimi nedeniyle oturum açma başarısız oldu. Kendi Kiracı ilkesi varsa, bu sorunu gidermek için kısıtlı Kiracı ayarlarını değiştirebilirsiniz.|
|50003|Eksik anahtar veya sertifika nedeniyle oturum açma başarısız oldu. Bunun nedeni, uygulamada yapılandırılmış bir imzalama anahtarının olmaması olabilir. [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured) sayfasında belirtilen çözümlere bakın. Sorun devam ederse, uygulama sahibi veya uygulama yöneticisine başvurun.|
|50005|Kullanıcı bir cihaza koşullu erişim ilkesi aracılığıyla şu anda desteklenmeyen bir platformdan oturum açmaya çalıştı.|
|50006| Geçersiz imza nedeniyle imza doğrulaması başarısız oldu. [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery) sayfasında belirtilen çözüme bakın. Sorun devam ederse uygulamanın sahibiyle veya uygulama Yöneticisi ile iletişime geçin.|
|50007|Bu uygulama için iş ortağı şifreleme sertifikası bulunamadı. [Bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md) bu sabit almak için Microsoft ile.|
|50008|SAML onayı eksik veya belirteçte yanlış yapılandırılmış. Federasyon sağlayıcınıza başvurun.|
|50010|Belirteç hedef kitlesi yapılandırılmadığından uygulama için doğrulaması başarısız oldu. Çözüm için uygulama sahibine başvurun.|
|50011|Yanıt adresi eksik, yanlış yapılandırılmış veya uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor. Konusunda listelenen çözümü deneyin [ https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application ](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application). Sorun devam ederse uygulamanın sahibiyle veya uygulama Yöneticisi ile iletişime geçin.|
|50012| Bu kimlik doğrulamasının başarısız gösteren bir genel hata iletisidir. Bu, eksik veya geçersiz kimlik bilgileri veya istek talepleri gibi nedenlerle ortaya çıkabilir. Doğru kimlik bilgilerini ve talepleri ile istek gönderilir emin olun. |
|50013|Çeşitli nedenlerle onayı geçersiz. Örneğin, belirteci veren kendi geçerli zaman aralığı içinde API sürümü eşleşmiyor, belirtecin süresi dolmuş veya hatalı biçimlendirilmiş veya yenileme belirteci onaylama birincil yenileme belirteci değil.|
|50017|Sertifika doğrulaması aşağıdaki nedenlerden dolayı başarısız oldu:<ul><li>Güvenilir sertifika listesinde verme sertifikası bulunamıyor</li><li>Beklenen CrlSegment bulunamıyor</li><li>Güvenilir sertifika listesinde verme sertifikası bulunamıyor</li><li>Delta CRL dağıtım noktası, karşılık gelen bir CRL dağıtım noktası olmadan yapılandırılmış</li><li>Zaman aşımı sorunu nedeniyle geçerli CRL segmentleri alınamıyor</li><li>CRL indirilemiyor</li></ul>Kiracı yöneticisine başvurun.|
|50020|Kullanıcı aşağıdaki nedenlerden biri için yetkilendirilmemiş.<ul><li>Kullanıcıya v1 uç noktası ile bir MSA hesapla oturum açmak çalışıyor</li><li>Kullanıcı, kiracıda mevcut değil.</li></ul> Uygulama sahibine başvurun.|
|50027|Aşağıdaki nedenlerden dolayı geçersiz JWT belirteci:<ul><li>Bir kerelik anahtar talebi içermiyor, alt talep</li><li>konu tanımlayıcısı uyuşmazlığı</li><li>idToken taleplerinde yinelenen talep</li><li>beklenmeyen veren</li><li>beklenmeyen hedef kitle</li><li>geçerli zaman aralığı içinde değil </li><li>belirteç biçimi doğru değil</li><li>Verenin dış kimlik belirteci imza doğrulaması başarısız oldu.</li></ul>Uygulama sahibine başvurun|
|50029|Geçersiz URI - etki alanı adı geçersiz karakterler içeriyor. Kiracı yöneticisine başvurun.|
|50034|Kullanıcı dizinde yok. Kiracı yöneticinizle iletişime geçin.|
|50042|İlkesi ikili tanımlayıcısını oluşturmak için gereken güvenlik değeri eksik. Kiracı yöneticisine başvurun.|
|50048|Konu, istemci onayındaki Veren talebi ile uyuşmuyor. Kiracı yöneticisine başvurun.|
|50050|İstek yanlış biçimlendirilmiş. Uygulama sahibine başvurun.|
|50053|Kullanıcı çok fazla kez yanlış kullanıcı kimliği veya parola ile oturum açmaya çalıştığı için hesap kilitlendi.|
|50055|Geçersiz parola, süresi dolmuş parola girildi.|
|50056|Parola - geçersiz veya null deposunda bu kullanıcı için parola yok.|
|50057|Kullanıcı hesabı devre dışı bırakıldı. Hesap bir yönetici tarafından devre dışı bırakıldı.|
|50058|Uygulama sessiz bir oturum açma gerçekleştirdi ve kullanıcı sessiz bir şekilde oturum açamadı. Uygulamanın, oturum açma için bir seçenek kullanıcılara dair bir etkileşimli akışı başlatmak gerekir. Uygulama sahibine başvurun.|
|50059|Kullanıcı dizinde yok. Kiracı yöneticinizle iletişime geçin.|
|50061|Oturumu kapatma isteği geçersiz. Uygulama sahibine başvurun.|
|50072|Kullanıcı (etkileşimli) iki öğeli kimlik doğrulamasına kaydolması gerekir.|
|50074|Kullanıcı MFA testini geçemedi.|
|50076|Kullanıcı MFA testini (etkileşimli olmayan) geçemedi.|
|50079|Kullanıcı (etkileşimli olmayan oturum açma bilgileri) iki faktörlü kimlik doğrulamasına kaydolması gerekir.|
|50085|Yenileme belirteci için sosyal IDP oturum açma bilgileri gerekiyor. Sahip. kullanıcı oturum açma kullanıcı adı ve parola ile yeniden deneyin.|
|50089|Akış belirtecin süresi doldu - kimlik doğrulaması başarısız oldu. Kullanıcı oturum açma kullanıcı adı ve parola ile yeniden deneyin|
|50097|Cihaz kimlik doğrulaması gerekli. Cihaz kimliği veya DeviceAltSecId talepler için oluşabilir **null**, ya da cihaz tanımlayıcısına karşılık gelen cihaz varsa.|
|50099|JWT imzası geçersiz. Uygulama sahibine başvurun.|
|50105|Oturum açmış kullanıcı, oturum açmış uygulama için bir role atanmadı. Uygulamaya kullanıcı atama. Daha fazla bilgi için: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role)|
|50107|İstenen federasyon bölgesi nesnesi yok. Kiracı yöneticisine başvurun.|
|50120|JWT üst bilgisi ile ilgili sorun var. Kiracı yöneticisine başvurun.|
|50124|Talep Dönüşümü, geçersiz giriş parametresi içeriyor. İlke güncelleştirmek için Kiracı yöneticisine başvurun.|
|50125|Oturum açma parolasını sıfırlama veya parola kaydı girişi nedeniyle yarıda kesildi.|
|50126|Geçersiz kullanıcı adı veya parola ya da geçersiz şirket içi kullanıcı adı veya parola.|
|50127|Kullanıcının bu içeriğe erişim kazanmak için bir aracı uygulaması yüklemeniz gerekir.|
|50128|Geçersiz etki alanı adı - Kiracı tanımlama bilgisi yok ya da istek bulunamadı veya sağlanan kimlik bilgilerini tarafından kapsanan.|
|50129|Cihaz çalışma alanına katılmış - değil **çalışma alanına katılma** cihazı kaydetmek için gereklidir.|
|50130|Talep değeri, bilinen bir kimlik doğrulama yöntemi yorumlanamıyor.|
|50131|Çeşitli koşullu erişim hatalarında kullanılır. Örneğin Hatalı Windows cihazı durumu, şüpheli etkinlik nedeniyle istek engellendi, erişim ilkesi ve güvenlik ilkesi kararları.|
|50132|Kimlik bilgileri aşağıdaki nedenlerden dolayı iptal edildi:<ul><li>SSO Yapıtı geçersiz veya süresi dolmuş</li><li>Oturum, uygulama için yeterince yeni değil</li><li>Sessiz oturum açma isteği gönderildi ancak kullanıcının Azure AD oturumu geçersiz veya süresi dolmuş.</li></ul>|
|50133|Süresi dolduğu veya yakın zamanda parola değiştirildiği için oturum geçersiz.|
|50135|Parola değiştirme, hesap riski nedeniyle gereklidir.|
|50136|MSA oturumu tek MSA oturum algılandı - uygulamaya yönlendirir. |
|50140|Bu hata, kullanıcı oturum açtığında "Oturumumu açık bırak" kesintisi nedeniyle oluştu. Daha fazla bilgi almak için Bağıntı Kimliği, İstek Kimliği ve Hata kodu ile [bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md). |
|50143|Oturum uyuşmazlığı - kullanıcı Kiracı etki alanı ipucu farklı kaynak nedeniyle eşleşmediği için oturum geçersiz.  [Bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md) daha ayrıntılı bilgi edinmek için bağıntı kimliği, istek kimliği ve hata koduna sahip.|
|50144|Kullanıcının Active Directory parolasının süresi doldu. Kullanıcı için yeni bir parola veya son kullanıcı Self Servis sıfırlama aracını kullanarak sahip.|
|50146|Bu uygulamanın, uygulamaya özel bir imzalama anahtarı ile yapılandırılması gerekiyor. Bu şekilde yapılandırılmamış veya anahtarın süresi dolmuş ya da anahtar henüz geçerli değil. Uygulama sahibine başvurun.|
|50148|code_verifier, PKCE yetkilendirme isteğinde belirtilen code_challenge ile eşleşmiyor. Uygulama geliştiricisine başvurun. |
|50155|Bu kullanıcı için cihaz kimlik doğrulaması başarısız oldu.|
|50158|Dış güvenlik sınaması karşılanmadı.|
|50161|Harici sağlayıcı tarafından gönderilen talepleri dış sağlayıcı için istenen yeterli ya da eksik talep değil.|
|50166|Talep sağlayıcı isteği gönderilemedi.|
|50169|Bölge geçerli hizmet ad alanının yapılandırılmış bir bölgesi değil.|
|50172|Dış talep sağlayıcısı onaylanmadı. Kiracı yöneticinizle iletişime geçin|
|50173|Yeni kimlik doğrulama belirteci gereklidir. Yeni kimlik bilgilerini kullanarak yeniden kullanıcı oturum açma vardır.|
|50177|Dış sınaması kullanıcılar için geçiş desteklenmiyor.|
|50178|Oturum denetimi kullanıcılar için geçiş desteklenmiyor.|
|50180|Windows Tümleşik kimlik doğrulaması gerekli. Sorunsuz SSO için kiracıyı etkinleştirin.|
|51001|Etki alanı ipucu şirket içi güvenlik tanımlayıcısı - şirket içi UPN ile mevcut değil.|
|51004|Kullanıcı hesabı dizinde yok.|
|51006|Windows Tümleşik kimlik doğrulaması gerekli. Talep eksik Oturum belirteci kullanarak oturum açmış kullanıcı. Kullanıcıdan yeniden oturum açmasını isteyin.|
|52004|Kullanıcı, LinkedIn kaynaklarına erişim izni sağlamadı. |
|53000|Koşullu Erişim ilkesi uyumlu bir cihaz gerektiriyor ve cihaz uyumlu değil. Cihazlarını Intune gibi onaylanmış bir MDM sağlayıcısıyla kullanıcı sahip.|
|53001|Koşullu Erişim ilkesi, etki alanına katılmış bir cihaz gerektiriyor ve cihaz etki alanına katılmamış. Cihaz kullanıcı kullanmak bir etki alanına katıldı.|
|53002|Kullanılan uygulama, koşullu erişim için onaylanmış bir uygulama değil. Kullanıcının erişim elde etmesi için kullanılacak onaylı uygulamalar listesinden bir uygulama seçmesi gerekir.|
|53003|Koşullu erişim ilkeleri nedeniyle erişim engellendi.|
|53004|Kullanıcının bu içeriğe erişmeden önce çok faktörlü kimlik doğrulaması işlemini tamamlaması gerekiyor. Kullanıcının çok faktörlü kimlik doğrulamasına kaydolması gerekir.|
|65001|X uygulamasının Y uygulamasına erişim izni yok veya erişim izni iptal edildi. Veya Kullanıcı ya da yönetici X kimliğiyle uygulamanın kullanılmasını onaylamadı. Bu kullanıcı veya kaynak için etkileşimli yetkilendirme isteği gönderin. Veya kullanıcı veya yönetici uygulamayı uygulama adına hareket Kiracı yöneticinize bir yetkilendirme isteği kimliği Mac'inizi gönderin ile kullanmak için: Y kaynak için: Z.|
|65004|Kullanıcı, uygulamaya erişmeyi reddetti. Kullanıcıdan oturum açmayı yeniden denemesini ve uygulamaya izin vermesini isteyin|
|65005|Uygulamaya gereken kaynak erişim listesi, kaynak tarafından bulunabilen uygulamaları içermiyor veya İstemci uygulaması kendi gerekli kaynak erişim listesinde belirtilmemiş bir kaynağa erişim isteğinde bulundu veya Graph hizmeti hatalı istek döndürdü veya kaynak bulunamadı. Uygulama SAML desteği içeriyorsa, yanlış Tanımlayıcı (Varlık) ile uygulamayı yapılandırmış olabilirsiniz. Aşağıdaki bağlantıyı kullanarak SAML için listelenen çözümü deneyin: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav#no-resource-in-requiredresourceaccess-list](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav)|
|70000|Aşağıdaki nedenlerden dolayı geçersiz izin:<ul><li>İstenen SAML 2.0 onay deyimi geçersiz Konu Onay Yöntemine sahip</li><li>Uygulama OnBehalfOf akışı V2’de desteklenmiyor</li><li>Birincil yenileme belirteci, oturum anahtarı ile imzalanmamış</li><li>Geçersiz dış yenileme belirteci</li><li>Farklı bir kiracı için erişim izni elde edildi.</li></ul>|
|70001|X adlı uygulama Y adlı kiracıda bulunamadı. Tanımlayıcısı X olan uygulama, kiracının yöneticisi tarafından yüklenmediyse veya kiracıdaki herhangi bir kullanıcı tarafından onaylanmadıysa bu durum ortaya çıkabilir. Uygulama tanımlayıcısı değeri yanlış veya kimlik doğrulaması isteğinizi yanlış kiracıya göndermiş.|
|70002|Uygulama geçersiz istemci kimlik bilgileri döndürdü. Uygulama sahibine başvurun.|
|70003|Uygulama desteklenmeyen bir izin verme türü döndürdü. Uygulama sahibine başvurun.|
|70004|Uygulama geçersiz bir yeniden yönlendirme URI'si döndürdü. İstemci tarafından belirtilen yeniden yönlendirme adresi, yapılandırılmış bir adresle veya OIDC onay listesindeki herhangi bir adresle eşleşmiyor. Uygulama sahibine başvurun.|
|70005|Uygulama, aşağıdaki nedenlerden dolayı desteklenmeyen bir yanıt türü döndürdü:<ul><li>uygulama için 'token' yanıt türü etkin değil</li><li>'id_token' yanıt türü 'OpenID' kapsamı gerektiriyor -kodlanmış wctx içinde desteklenmeyen bir OAuth parametre değeri içeriyor</li></ul>Uygulama sahibine başvurun.|
|70007|Uygulama bir belirteç isterken desteklenmeyen bir 'response_mode' değeri döndürdü. Uygulama sahibine başvurun.|
|70008|Sağlanan yetki kodu veya yenileme belirtecinin süresi doldu veya iptal edildi. Oturum açma kullanıcı yeniden deneme vardır.|
|70011|Uygulama tarafından istenen kapsam geçersiz. Uygulama sahibine başvurun.|
|70012|MSA (tüketici) kullanıcısının kimliği doğrulanırken bir sunucu hatası oluştu. Oturum açma, yeniden deneyin ve sorun devam ederse [bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md) |
|70018|Kullanıcının cihaz kod akışı için yanlış kullanıcı kodu girmesi nedeniyle geçersiz doğrulama kodu. Yetkilendirme onaylanmadı.|
|70019|Doğrulama kodunun süresi doldu. Kullanıcı oturum açma yeniden sahip.|
|70037|Hatalı sınama yanıtı sağlandı. Uzaktan kimlik doğrulama oturumu reddedildi.|
|75001|SAML iletisi bağlama sırasında bir hata oluştu.|
|75003|Uygulama, desteklenmeyen Bağlama ile ilgili bir hata döndürdü (SAML protokolü yanıtı HTTP POST dışındaki bağlamalar üzerinden gönderilemiyor). Uygulama sahibine başvurun.|
|75005|Azure AD, uygulama tarafından Çoklu oturum açma için gönderilen SAML İsteğini desteklemiyor. Uygulama sahibine başvurun.|
|75008|SAML isteği beklenmeyen bir hedefe sahip olduğundan, uygulamadan alınan istek reddedildi. Uygulama sahibine başvurun.|
|75011|Kullanıcının hizmette kimlik doğrulaması yaparken kullandığı kimlik doğrulama yöntemi, istenen kimlik doğrulama yöntemi ile eşleşmiyor. Uygulama sahibine başvurun.|
|75016|SAML2 Kimlik Doğrulama İsteği geçersiz NameIdPolicy değerine sahip. Uygulama sahibine başvurun.|
|80001|Kimlik Doğrulama Aracısı Active Directory'ye bağlanamadı. Kimlik doğrulama aracısının, kullanıcının oturum açma isteğine hizmet edebilen bir CD ile görüş hattı olan, etki alanına katılmış bir makinede yüklü olduğundan emin olun.|
|80002|İç hata. Parola doğrulama isteği zaman aşımına uğradı. İç Karma Kimlik Hizmeti’ne kimlik doğrulama isteği gönderemedik. Hata hakkında daha fazla bilgi almak için [bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md).|
|80003|Kimlik Doğrulama Aracısı tarafından geçersiz yanıt alındı. Şirket içi Active Directory ile kimlik doğrulaması denenirken bilinmeyen bir hata oluştu. Hata hakkında daha fazla bilgi almak için [bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md).|
|80005|Kimlik Doğrulama Aracısı: Kimlik Doğrulama Aracısı gelen yanıt işlenirken bilinmeyen bir hata oluştu. Hata hakkında daha fazla bilgi almak için [bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md).|
|80007|Kimlik Doğrulama Aracısı kullanıcının parolasını doğrulayamıyor.|
|80010|Kimlik Doğrulama Aracısı parolanın şifresini çözemedi. |
|80011|Kimlik Doğrulama Aracısı şifreleme anahtarını alamıyor.|
|80012|Kullanıcıların dışında izin verilen maksimum oturum açmaya çalıştı saat (bu AD içinde belirtilen).|
|80013|Kimlik doğrulama aracısını ve AD’yi çalıştıran makineler arasındaki zaman dengesizliği nedeniyle kimlik doğrulaması girişimi tamamlanamadı. Zaman eşitleme sorunlarını düzeltin|
|80014|Kimlik doğrulama aracısı zaman aşımına uğradı. [Bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md) hata kodu, bağıntı kimliği ve bu hata hakkında ayrıntılı bilgi edinmek için Datetime.|
|81001|Kullanıcının Kerberos anahtarı fazla büyük. Bu durum, kullanıcı çok fazla grupta ise ve sonuç olarak Kerberos bileti çok fazla grup üyeliği içeriyorsa gerçekleşebilir. Kullanıcının grup üyeliklerini azaltın ve yeniden deneyin.|
|81005|Kimlik doğrulama paketi desteklenmiyor.|
|81007|Kiracı için sorunsuz SSO etkin değil.|
|81012|Bu bir hata durumu değil. Bu, Azure AD'de oturum açmaya çalışırken, kullanıcı, cihazda oturum açmış olan kullanıcıdan farklıdır gösterir. Bu kod, günlüklerde güvenle yok sayabilirsiniz.|
|90010|İstek, çeşitli nedenlerle desteklenmiyor. Örneğin, istek (yalnızca POST yöntemini desteklenir) bir desteklenmeyen istek yöntemi kullanılarak yapılan veya istenen belirteç imzalama algoritması desteklenmiyor. Uygulama geliştiricisine başvurun.|
|90014| Bir protokol iletisi için gerekli bir alan eksik, uygulama sahibine başvurun. Uygulama sahibi bu durumda, oturum açma isteği için gerekli tüm parametreleri olduğundan emin olun. |
|90051| Geçersiz bir temsilci belirteci. Geçersiz Ulusal bulut kimliği ({Cloudıd}) belirtildi.|
|90072| Hesabın kiracıda bir dış kullanıcı olarak önce eklenmesi gerekir. Oturum kapatma ve yeniden farklı bir Azure AD ile oturum açma hesabı.|
|90094| Grant yönetici izinleri gerektirir. Bu uygulama için rıza sağlamanın Kiracı yöneticinize başvurun.|
|500133| Onaylama işlemi, geçerli zaman aralığı içinde değil. Kullanıcı onayı için kullanmadan önce erişim belirtecinin süresi dolmadı emin olun veya yeni bir belirteç isteyin.|
|530021|Application onaylı koşullu erişim uygulama gereksinimlerini karşılamıyor.|

## <a name="next-steps"></a>Sonraki adımlar

* [Oturum açma işlemleri raporları genel bakış](concept-sign-ins.md)
* [Azure AD raporlar programlı erişim](concept-reporting-api.md)
