---
title: Azure Active Directory portalında bulunan oturum açma etkinlik raporundaki hata kodları | Microsoft Docs
description: Oturum açma etkinlik raporu hata kodları başvurusu.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/31/2018
ms.author: rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: cb636db4e6e2097f494fcbc7a6584f0172514b95
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34698518"
---
# <a name="sign-in-activity-report-error-codes-in-the-azure-active-directory-portal"></a>Azure Active Directory portalında bulunan oturum açma etkinlik raporundaki hata kodları

Kullanıcı oturum açma işlemlerinin raporuyla sağlanan bilgiler sayesinde aşağıdakiler gibi soruların yanıtlarını bulabilirsiniz:

- Kimler Azure Active Directory kullanarak oturum açtı?
- Hangi uygulamalarda oturum açıldı?
- Hangi oturum açma girişimleri başarısız oldu ve neden?

Bu makalede hata kodları ve ilgili açıklamalar listelenmektedir. 

## <a name="how-can-i-display-failed-sign-ins"></a>Başarısız oturum açma girişimlerini nasıl görüntüleyebilirim? 

Tüm oturum açma etkinliği verilerine ilk giriş noktanız, **Azure Active**’in **Etkinlik** bölümündeki **[Oturum açma işlemleri](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns)** kısmıdır.


![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins-errors/61.png "oturum açma etkinliği")

Oturum açma raporunuzda tüm başarısız oturum açma işlemlerini görüntülemek için, **Oturum açma durumu** olarak **Başarısız**'ı seçebilirsiniz.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins-errors/06.png "oturum açma etkinliği")

Görüntülenen listedeki bir öğeye tıklarsanız **Etkinlik Ayrıntıları: Oturum açma işlemleri** dikey penceresi açılır. Bu görünüm size Azure Active Directory’nin oturum açma işlemleriyle ilgili olarak izlediği tüm ayrıntıları gösterir; bunlara **oturum açma hata kodu** ve **hatanın nedeni** de dahildir.

![Oturum açma etkinliği](./media/active-directory-reporting-activity-sign-ins-errors/05.png "oturum açma etkinliği")


Oturum açma verilerine erişmek için Azure portalını kullanmaya alternatif olarak [raporlama API'sini](active-directory-reporting-api-getting-started-azure-portal.md) de kullanabilirsiniz.


Aşağıdaki bölümde, tüm olası hataları ve ilgili açıklamalarını kapsayan bir genel bakış sağlanır. 

## <a name="error-codes"></a>Hata kodları


|Hata|Açıklama|
|---|---|
|20001|Federasyon Kimlik Sağlayıcısı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun.|
|20012|Federasyon Kimlik Sağlayıcısı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun.|
|20033|Federasyon Kimlik Sağlayıcısı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun.|
|40008|Federasyon Kimlik Sağlayıcısı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun.|
|40009|Federasyon Kimlik Sağlayıcısı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun.|
|40014|Federasyon Kimlik Sağlayıcısı ile ilgili bir sorun var. Bu sorunu çözmek için IDP’ye başvurun.|
|50000|Oturum açma hizmetimizle ilgili bir sorun var. Bu sorunu çözmek için bir [destek bileti açın](active-directory-troubleshooting-support-howto.md).|
|50001|Hizmet asıl adı bu kiracıda bulunamadı. Uygulama, kiracının yöneticisi tarafından yüklenmediyse bu durum oluşabilir. Ayrıca, kaynak sorumlusu dizinde bulunamamış veya geçersiz de olabilir.|
|50002|Kiracıda kısıtlı proxy erişimi nedeniyle oturum açma başarısız oldu. Kiracı ilkesi size aitse, bu sorunu çözmek için kısıtlı kiracı ayarlarınızı değiştirebilirsiniz|
|50003|Eksik anahtar veya sertifika nedeniyle oturum açma başarısız oldu. Bunun nedeni, uygulamada yapılandırılmış bir imzalama anahtarının olmaması olabilir. [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured) sayfasında belirtilen çözümlere bakın. Hala sorun görüyorsanız, uygulama sahibine veya uygulama yöneticisine başvurun|
|50005|Kullanıcı şu anda koşullu erişim ilkesiyle desteklenmeyen bir platformdan cihazda oturum açmayı denedi|
|50006| Geçersiz imza nedeniyle imza doğrulaması başarısız oldu. [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery) sayfasında belirtilen çözüme bakın. Hala sorun görüyorsanız uygulama sahibine veya uygulama yöneticisine başvurun|
|50007|Bu uygulama için iş ortağı şifreleme sertifikası bulunamadı. Bu sorunu çözmek için Microsoft’ta [bir destek bileti açın](active-directory-troubleshooting-support-howto.md)|
|50008|SAML onayı eksik veya belirteçte yanlış yapılandırılmış. Federasyon sağlayıcınıza başvurun.|
|50010|Belirteç hedef kitlesi yapılandırılmadığından uygulama için doğrulaması başarısız oldu. Uygulama sahibine başvurun|
|50011|Yanıt adresi eksik, yanlış yapılandırılmış veya uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor. [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application) sayfasında listelenen çözümü deneyin. Hala sorun görüyorsanız uygulama sahibine veya uygulama yöneticisine başvurun|
|50013|Onay çeşitli nedenlerden dolayı geçersiz: Belirteci veren, geçerli zaman aralığı içinde api sürümü ile eşleşmiyor -süresi dolmuş -yanlış biçimlendirilmiş - Onaydaki yenileme belirteci birincil yenileme belirteci değil.|
|50017|Sertifika doğrulaması aşağıdaki nedenlerden dolayı başarısız oldu:<ul><li>Güvenilir sertifika listesinde verme sertifikası bulunamıyor</li><li>Beklenen CrlSegment bulunamıyor</li><li>Güvenilir sertifika listesinde verme sertifikası bulunamıyor</li><li>Delta CRL dağıtım noktası, karşılık gelen bir CRL dağıtım noktası olmadan yapılandırılmış</li><li>Zaman aşımı sorunu nedeniyle geçerli CRL segmentleri alınamıyor</li><li>CRL indirilemiyor</li></ul>Kiracı yöneticisine başvurun.|
|50020|Kullanıcı yetkisiz - Sürüm sorunu nedeniyle belirteçler verilemiyor - Veren adı belirtilmemiş - Veren adı ile ilgili sorunlar var (null -max uzunluk). Uygulama sahibine başvurun|
|50027|Aşağıdaki nedenlerden dolayı geçersiz JWT belirteci:<ul><li>Bir kerelik anahtar talebi içermiyor, alt talep</li><li>konu tanımlayıcısı uyuşmazlığı</li><li>idToken taleplerinde yinelenen talep</li><li>beklenmeyen veren</li><li>beklenmeyen hedef kitle</li><li>geçerli zaman aralığı içinde değil </li><li>belirteç biçimi doğru değil</li><li>Verenin dış kimlik belirteci imza doğrulaması başarısız oldu.</li></ul>Uygulama sahibine başvurun|
|50029|Geçersiz URI - etki alanı adı geçersiz karakterler içeriyor. Kiracı yöneticisine başvurun.|
|50034|Kullanıcı dizinde yok. Kiracı yöneticinize başvurun.|
|50042|Sorumluda ikili tanımlayıcı oluşturmak için güvenlik dizisi eklemek gerekiyor. Kiracı yöneticisine başvurun.|
|50048|Konu, istemci onayındaki Veren talebi ile uyuşmuyor. Kiracı yöneticisine başvurun.|
|50050|İstek yanlış biçimlendirilmiş. Uygulama sahibine başvurun.|
|50053|Kullanıcı, yanlış kullanıcı kimliği veya parola ile çok fazla kez oturum açmaya çalıştığı için hesap kilitlendi.|
|50055|Geçersiz parola, süresi dolmuş parola girildi.|
|50056|Geçersiz veya null parola -Bu kullanıcının deposunda parola mevcut değil|
|50057|Kullanıcı hesabı devre dışı bırakıldı. Hesap bir yönetici tarafından devre dışı bırakıldı.|
|50058|Uygulama sessiz bir oturum açma gerçekleştirdi ve kullanıcı sessiz bir şekilde oturum açamadı. Uygulamanın kullanıcılara oturum açma seçeneği veren etkileşimli bir akış başlatması gerekiyor. Uygulama sahibine başvurun.|
|50059|Kullanıcı dizinde yok. Kiracı yöneticinize başvurun|
|50061|Oturumu kapatma isteği geçersiz. Uygulama sahibine başvurun|
|50072|Kullanıcının ikinci faktör kimlik doğrulamasına kaydolması gerekir (etkileşimli)|
|50074|Kullanıcı MFA testini geçemedi.|
|50076|Kullanıcı MFA testini geçemedi (etkileşimsiz)|
|50079|Kullanıcının ikinci faktör kimlik doğrulamasına kaydolması gerekiyor (etkileşimli olmayan oturum açma)|
|50085|Yenileme belirteci için sosyal IDP oturum açma bilgileri gerekiyor. Kullanıcıdan kullanıcı adı-parola ile yeniden oturum açmasını isteyin|
|50089|Akış belirtecinin süresi doldu - Kimlik Doğrulaması Başarısız Oldu. Kullanıcıdan kullanıcı adı-parola ile yeniden oturum açmasını isteyin|
|50097|Cihaz Kimlik Doğrulaması Gerekli - DeviceId -DeviceAltSecId talepleri null VEYA cihaz tanımlayıcısına karşılık gelen bir cihaz yok|
|50099|JWT imzası geçersiz. Uygulama sahibine başvurun.|
|50105|Oturum açmış kullanıcı, oturum açmış uygulama için bir role atanmadı. Kullanıcıyı uygulamaya atayın. Daha fazla bilgi için: [https://docs.microsoft.com/en-us/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role](https://docs.microsoft.com/en-us/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role)|
|50107|İstenen federasyon bölgesi nesnesi yok. Kiracı yöneticisine başvurun.|
|50120|JWT üst bilgisi ile ilgili sorun var. Kiracı yöneticisine başvurun.|
|50124|Talep Dönüşümü, geçersiz giriş parametresi içeriyor. İlke güncelleştirmek için kiracı yöneticisine başvurun.|
|50125|Oturum açma bir parola sıfırlama veya parola kayıt girişi nedeniyle yarıda kesildi|
|50126|Geçersiz kullanıcı adı veya parola ya da Geçersiz şirket içi kullanıcı adı veya parola.|
|50127|Kullanıcının bu içeriğe erişmek için bir aracı uygulaması yüklemesi gerekiyor.|
|50128|Geçersiz etki alanı adı - İstekte kiracıyı tanımlayan bilgiler bulunamadı veya sağlanan kimlik bilgilerinin kapsamında değil|
|50129|Cihaz Çalışma alanına katılmamış - Cihazın kaydedilmesi için çalışma alanına katılması gerekiyor.|
|50130|Talep değeri, bilinen kimlik doğrulama yöntemi olarak yorumlanamıyor|
|50131|Çeşitli koşullu erişim hatalarında kullanılır. Örneğin Hatalı Windows cihazı durumu, şüpheli etkinlik nedeniyle istek engellendi, erişim ilkesi ve güvenlik ilkesi kararları.|
|50132|Kimlik bilgileri aşağıdaki nedenlerden dolayı iptal edildi:<ul><li>SSO Yapıtı geçersiz veya süresi dolmuş</li><li>Oturum, uygulama için yeterince yeni değil</li><li>Sessiz oturum açma isteği gönderildi ancak kullanıcının Azure AD oturumu geçersiz veya süresi dolmuş.</li></ul>|
|50133|Süresi dolduğu veya yakın zamanda parola değiştirildiği için oturum geçersiz.|
|50135|Hesap riski nedeniyle parola değişikliği gerekiyor|
|50136|Msa oturumunu uygulamaya yeniden yönlendirin - Tek MSA oturumu algılandı |
|50140|Bu hata, kullanıcı oturum açtığında "Oturumumu açık bırak" kesintisi nedeniyle oluştu. Daha fazla bilgi almak için Bağıntı Kimliği, İstek Kimliği ve Hata kodu ile [bir destek bileti açın](active-directory-troubleshooting-support-howto.md). |
|50143|Oturum uyuşmazlığı - Kullanıcı kiracısı farklı kaynak nedeniyle etki alanı ipucu ile eşleşmediğinden oturum geçersiz. Daha fazla bilgi almak için Bağıntı Kimliği, İstek Kimliği ve Hata kodu ile [bir destek bileti açın](active-directory-troubleshooting-support-howto.md).|
|50144|Kullanıcının Active Directory parolasının süresi doldu. Kullanıcı için yeni bir parola oluşturun veya son kullanıcıdan self servis sıfırlama aracını kullanmasını isteyin|
|50146|Bu uygulamanın, uygulamaya özel bir imzalama anahtarı ile yapılandırılması gerekiyor. Bu şekilde yapılandırılmamış veya anahtarın süresi dolmuş ya da anahtar henüz geçerli değil. Uygulama sahibine başvurun|
|50148|code_verifier, PKCE yetkilendirme isteğinde belirtilen code_challenge ile eşleşmiyor. Uygulama geliştiricisine başvurun. |
|50155|Bu kullanıcı için cihaz kimlik doğrulaması başarısız oldu|
|50158|Dış güvenlik sınaması gerçekleştirilmedi|
|50161|Dış sağlayıcı tarafından gönderilen talepler yeterli değil veya dış sağlayıcıya eksik talep gönderildi|
|50166|Talep sağlayıcısına istek gönderilemedi|
|50169|Bölge geçerli hizmet ad alanının yapılandırılmış bir bölgesi değil.|
|50172|Dış talep sağlayıcısı onaylanmadı. Kiracı yöneticisine başvurun|
|50173|Yeni kimlik doğrulama belirteci gereklidir. Kullanıcıdan yeni kimlik bilgilerini kullanarak yeniden oturum açmasını isteyin|
|50177|Geçiş kullanıcıları için dış sınama desteklenmiyor|
|50178|Geçiş kullanıcıları için Oturum Denetimi desteklenmiyor|
|50180|Windows Tümleşik kimlik doğrulaması gerekli. Sorunsuz SSO için kiracıyı etkinleştirin.|
|51001|Şirket İçi Güvenlik Tanımlayıcısı - Şirket İçi UPN ile Etki Alanı İpucu mevcut değil|
|51004|Kullanıcı hesabı dizinde yok.|
|51006|Windows Tümleşik kimlik doğrulaması gerekli. Kullanıcı, wia talebi eksik olan oturum belirtecini kullanarak oturum açtı. Kullanıcıdan yeniden oturum açmasını isteyin.|
|52004|Kullanıcı, LinkedIn kaynaklarına erişim izni sağlamadı. |
|53000|Koşullu Erişim ilkesi uyumlu bir cihaz gerektiriyor ve cihaz uyumlu değil. Kullanıcıdan cihazı Intune gibi onaylı bir MDM sağlayıcısına kaydetmesini isteyin|
|53001|Koşullu Erişim ilkesi, etki alanına katılmış bir cihaz gerektiriyor ve cihaz etki alanına katılmamış. Kullanıcıdan etki alanına katılmış bir cihaz kullanmasını isteyin|
|53002|Kullanılan uygulama, koşullu erişim için onaylanmış bir uygulama değil. Kullanıcının erişim elde etmesi için kullanılacak onaylı uygulamalar listesinden bir uygulama seçmesi gerekir.|
|53003|Koşullu erişim ilkeleri nedeniyle erişim engellendi.|
|53004|Kullanıcının bu içeriğe erişmeden önce çok faktörlü kimlik doğrulaması işlemini tamamlaması gerekiyor. Kullanıcının çok faktörlü kimlik doğrulamasına kaydolması gerekir.|
|65001|X uygulamasının Y uygulamasına erişim izni yok veya erişim izni iptal edildi. Veya Kullanıcı ya da yönetici X kimliğiyle uygulamanın kullanılmasını onaylamadı. Bu kullanıcı veya kaynak için etkileşimli yetkilendirme isteği gönderin. Veya Kullanıcı ya da yönetici X kimliğiyle uygulamanın kullanılmasını onaylamadı. Kaynak: Z için Uygulama: Y adına işlem yapmak üzere kiracı yöneticinize bir yetkilendirme isteği gönderin.|
|65004|Kullanıcı, uygulamaya erişmeyi reddetti. Kullanıcıdan oturum açmayı yeniden denemesini ve uygulamaya izin vermesini isteyin|
|65005|Uygulamaya gereken kaynak erişim listesi, kaynak tarafından bulunabilen uygulamaları içermiyor veya İstemci uygulaması kendi gerekli kaynak erişim listesinde belirtilmemiş bir kaynağa erişim isteğinde bulundu veya Graph hizmeti hatalı istek döndürdü veya kaynak bulunamadı. Uygulama SAML desteği içeriyorsa, yanlış Tanımlayıcı (Varlık) ile uygulamayı yapılandırmış olabilirsiniz. Aşağıdaki bağlantıyı kullanarak SAML için listelenen çözümü deneyin: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav#no-resource-in-requiredresourceaccess-list](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav#no-resource-in-requiredresourceaccess-list)|
|70000|Aşağıdaki nedenlerden dolayı geçersiz izin:<ul><li>İstenen SAML 2.0 onay deyimi geçersiz Konu Onay Yöntemine sahip</li><li>Uygulama OnBehalfOf akışı V2’de desteklenmiyor</li><li>Birincil yenileme belirteci, oturum anahtarı ile imzalanmamış</li><li>Geçersiz dış yenileme belirteci</li><li>Farklı bir kiracı için erişim izni elde edildi.</li></ul>|
|70001|X adlı uygulama Y adlı kiracıda bulunamadı. Tanımlayıcısı X olan uygulama, kiracının yöneticisi tarafından yüklenmediyse veya kiracıdaki herhangi bir kullanıcı tarafından onaylanmadıysa bu durum ortaya çıkabilir. Uygulamanın Tanımlayıcı değerini yanlış yapılandırılmış veya uygulama isteğinizi yanlış kiracıya göndermiş olabilirsiniz|
|70002|Uygulama geçersiz istemci kimlik bilgileri döndürdü. Uygulama sahibine başvurun|
|70003|Uygulama desteklenmeyen bir izin verme türü döndürdü. Uygulama sahibine başvurun|
|70004|Uygulama geçersiz bir yeniden yönlendirme URI'si döndürdü. İstemci tarafından belirtilen yeniden yönlendirme adresi, yapılandırılmış bir adresle veya OIDC onay listesindeki herhangi bir adresle eşleşmiyor. Uygulama sahibine başvurun|
|70005|Uygulama, aşağıdaki nedenlerden dolayı desteklenmeyen bir yanıt türü döndürdü:<ul><li>uygulama için 'token' yanıt türü etkin değil</li><li>'id_token' yanıt türü 'OpenID' kapsamı gerektiriyor -kodlanmış wctx içinde desteklenmeyen bir OAuth parametre değeri içeriyor</li></ul>Uygulama sahibine başvurun|
|70007|Uygulama bir belirteç isterken desteklenmeyen bir 'response_mode' değeri döndürdü. Uygulama sahibine başvurun|
|70008|Sağlanan yetkilendirme kodu veya yenileme belirtecinin süresi doldu -iptal edildi. Kullanıcıdan yeniden oturum açmasını isteyin|
|70011|Uygulama tarafından istenen kapsam geçersiz. Uygulama sahibine başvurun|
|70012|MSA (tüketici) kullanıcısının kimliği doğrulanırken bir sunucu hatası oluştu. Lütfen yeniden deneyin. Başarısız olmaya devam ederse [bir destek bileti açın](active-directory-troubleshooting-support-howto.md) |
|70018|Kullanıcının cihaz kod akışı için yanlış kullanıcı kodu girmesi nedeniyle geçersiz doğrulama kodu. Yetkilendirme onaylanmadı|
|70019|Doğrulama kodunun süresi doldu. Kullanıcıdan oturum açmayı yeniden denemesini isteyin|
|70037|Hatalı sınama yanıtı sağlandı. Uzaktan kimlik doğrulama oturumu reddedildi.|
|75001|SAML iletisi bağlama sırasında bir hata oluştu.|
|75003|Uygulama, desteklenmeyen Bağlama ile ilgili bir hata döndürdü (SAML protokolü yanıtı HTTP POST dışındaki bağlamalar üzerinden gönderilemiyor). Uygulama sahibine başvurun|
|75005|Azure AD, uygulama tarafından Çoklu oturum açma için gönderilen SAML İsteğini desteklemiyor. Uygulama sahibine başvurun|
|75008|SAML isteği beklenmeyen bir hedefe sahip olduğundan, uygulamadan alınan istek reddedildi. Uygulama sahibine başvurun|
|75011|Kullanıcının hizmette kimlik doğrulaması yaparken kullandığı kimlik doğrulama yöntemi, istenen kimlik doğrulama yöntemi ile eşleşmiyor. Uygulama sahibine başvurun|
|75016|SAML2 Kimlik Doğrulama İsteği geçersiz NameIdPolicy değerine sahip. Uygulama sahibine başvurun|
|80001|Kimlik Doğrulama Aracısı Active Directory'ye bağlanamadı. Kimlik doğrulama aracısının, kullanıcının oturum açma isteğine hizmet edebilen bir CD ile görüş hattı olan, etki alanına katılmış bir makinede yüklü olduğundan emin olun.|
|80002|İç hata. Parola doğrulama isteği zaman aşımına uğradı. İç Karma Kimlik Hizmeti’ne kimlik doğrulama isteği gönderemedik. Hata hakkında daha fazla bilgi almak için [bir destek bileti açın](active-directory-troubleshooting-support-howto.md)|
|80003|Kimlik Doğrulama Aracısı tarafından geçersiz yanıt alındı. Şirket içi Active Directory ile kimlik doğrulaması denenirken bilinmeyen bir hata oluştu. Hata hakkında daha fazla bilgi almak için [bir destek bileti açın](active-directory-troubleshooting-support-howto.md).|
|80005|Kimlik Doğrulama Aracısı: Kimlik Doğrulama Aracısının yanıtı işlenirken bilinmeyen bir hata oluştu. Hata hakkında daha fazla bilgi almak için [bir destek bileti açın](active-directory-troubleshooting-support-howto.md).|
|80007|Kimlik Doğrulama Aracısı kullanıcının parolasını doğrulayamıyor.|
|80010|Kimlik Doğrulama Aracısı parolanın şifresini çözemedi. |
|80011|Kimlik Doğrulama Aracısı şifreleme anahtarını alamıyor.|
|80012|Kullanıcılar izin verilen saatlerin (AD’de belirtilmiştir) dışında oturum açmayı denedi|
|80013|Kimlik doğrulama aracısını ve AD’yi çalıştıran makineler arasındaki zaman dengesizliği nedeniyle kimlik doğrulaması girişimi tamamlanamadı. Zaman eşitleme sorunlarını düzeltin|
|80014|Kimlik doğrulama aracısı zaman aşımına uğradı. Bu hata hakkında daha fazla bilgi almak için hata kodu, bağıntı kimliği ve Tarih saat ile [bir destek bileti açın](active-directory-troubleshooting-support-howto.md)|
|81001|Kullanıcının Kerberos anahtarı fazla büyük. Bu durum, kullanıcı çok fazla grupta ise ve sonuç olarak Kerberos bileti çok fazla grup üyeliği içeriyorsa gerçekleşebilir. Kullanıcının grup üyeliklerini azaltın ve yeniden deneyin.|
|81005|Kimlik Doğrulaması Paketi Desteklenmiyor|
|81007|Kiracı Sorunsuz SSO için etkinleştirilmedi|


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Azure Active Directory portalındaki oturum açma etkinlik raporları](active-directory-reporting-activity-sign-ins.md).
