---
title: Azure Active Directory kimlik doğrulama ve yetkilendirme hata kodları | Microsoft Docs
description: Azure AD güvenlik belirteci Hizmeti'nden (STS) döndürülen AADSTS hata kodları hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 02/13/2019
ms.author: ryanwi
ms.reviewer: hirsin, justhu
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3d6ed5c80d5c3241a9a328a2427ed8b920790635
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67482478"
---
# <a name="authentication-and-authorization-error-codes"></a>Kimlik doğrulaması ve yetkilendirme hata kodları

Azure Active Directory (Azure AD) güvenlik belirteci Hizmeti'nden (STS) döndürülen AADSTS hata kodları hakkında bilgi mi arıyorsunuz? AADSTS öğrenmek için bu belgeyi okuma hatası açıklamaları, düzeltmeler ve bazı geçici çözümleri önerilen.

> [!NOTE]
> Bu bilgiler geçicidir ve değiştirilebilir. Bir sorunuz veya aradığınız ne bulamıyor? Bir GitHub sorunu oluşturun veya bakın [geliştiriciler için destek ve Yardım seçeneklerini](active-directory-develop-help-support.md) diğer yollar hakkında bilgi edinmek için Yardım alabilir ve destek.
>
> Bu belge, geliştirici ve Yönetim Kılavuzu için sağlanır, ancak istemci tarafından hiçbir zaman kullanılmamalıdır. Hata kodları herhangi bir zamanda geliştirici kendi uygulama oluştururken yardımcı olması amaçlanmıştır daha ayrıntılı hata iletileri sağlamak için değiştirilebilir. Bir bağımlılık metin ya da hata kodunu numaralarında yararlanan uygulamalar zamana kırılır.  

## <a name="aadsts-error-codes"></a>AADSTS hata kodları

| Hata | Açıklama |
|---|---|
| AADSTS16000 | SelectUserAccount - birden çok geçerli SSO oturumu arasından seçmek kullanıcıya izin veren UI sonuçlanır Azure AD tarafından oluşturulan bir kesme budur. Bu hata oldukça yaygındır ve uygulamaya, döndürülen `prompt=none` belirtilir. |
| AADSTS16001 | Kullanıcı oturumu seçme mantığı reddetti bir kutucuğa tıklarsa, UserAccountSelectionInvalid - bu hatayı görürsünüz. Bu hata, tetiklendiğinde, çekme kutucukları/oturumlarının güncelleştirilmiş bir listeden veya başka bir hesabı seçerek, kurtarılır kullanıcının sağlar. Bu hata nedeniyle kod hatasını veya yarış durumu oluşabilir. |
| AADSTS16002 | AppSessionSelectionInvalid - uygulama belirtilen SID gereksinim karşılanmadı.  |
| AADSTS16003 | SsoUserAccountNotFoundInResourceTenant - kullanıcının kiracıya açıkça eklenmemiş gösterir. |
| AADSTS17003 | CredentialKeyProvisioningFailed - Azure AD kullanıcı anahtarı sağlayamazsınız. |
| AADSTS20001 | WsFedSignInResponseError - Federasyon kimlik sağlayıcınız ile ilgili bir sorun yoktur. Bu sorunu çözmek için IDP’ye başvurun. |
| AADSTS20012 | WsFedMessageInvalid - Federasyon kimlik sağlayıcınız ile ilgili bir sorun yoktur. Bu sorunu çözmek için IDP’ye başvurun. |
| AADSTS20033 | FedMetadataInvalidTenantName - Federasyon kimlik sağlayıcınız ile ilgili bir sorun yoktur. Bu sorunu çözmek için IDP’ye başvurun. |
| AADSTS40008 | OAuth2IdPUnretryableServerError - Federasyon kimlik sağlayıcınız ile ilgili bir sorun yoktur. Bu sorunu çözmek için IDP’ye başvurun. |
| AADSTS40009 | OAuth2IdPRefreshTokenRedemptionUserError - Federasyon kimlik sağlayıcınız ile ilgili bir sorun yoktur. Bu sorunu çözmek için IDP’ye başvurun. |
| AADSTS40010 | OAuth2IdPRetryableServerError - Federasyon kimlik sağlayıcınız ile ilgili bir sorun yoktur. Bu sorunu çözmek için IDP’ye başvurun. |
| AADSTS40015 | OAuth2IdPAuthCodeRedemptionUserError - Federasyon kimlik sağlayıcınız ile ilgili bir sorun yoktur. Bu sorunu çözmek için IDP’ye başvurun. |
| AADSTS50000 | TokenIssuanceError - oturum açma hizmeti ile ilgili bir sorun yoktur. Bu sorunu çözmek için bir [destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md). |
| AADSTS50001 | InvalidResource - kaynak devre dışı bırakıldı veya artık yok. Erişmeye çalıştığınız kaynak için tam Kaynak URL belirttiğinizden emin olmak için uygulamanızın kodunu kontrol edin.  |
| AADSTS50002 | NotAllowedTenant - bir kiracı üzerinde sınırlı proxy erişim nedeniyle oturum açma başarısız oldu. Kendi Kiracı ilkesi varsa, bu sorunu gidermek için kısıtlı Kiracı ayarlarını değiştirebilirsiniz. |
| AADSTS50003 | Eksik bir imzalama anahtarı veya sertifika nedeniyle oturum açma MissingSigningKey - başarısız oldu. Uygulamada yapılandırılmış herhangi bir imzalama anahtarı olduğundan bu olabilir. [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured) sayfasında belirtilen çözümlere bakın. Sorunlar görmeye devam ediyorsanız, uygulama sahibi veya bir uygulama Yöneticisi ile iletişime geçin |
| AADSTS50005 | DevicePolicyError - kullanıcı bir cihaza koşullu erişim ilkesi aracılığıyla şu anda desteklenmeyen bir platformdan oturum açmayı denedi. |
| AADSTS50006 | InvalidSignature - imza doğrulaması geçersiz bir imza nedeniyle başarısız oldu. |
| AADSTS50007 | PartnerEncryptionCertificateMissing - bu uygulama için iş ortağı şifreleme sertifikası bulunamadı. [Bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md) bu sabit almak için Microsoft ile. |
| AADSTS50008 | InvalidSamlToken - eksik veya belirteçte SAML onayı. Federasyon sağlayıcınıza başvurun. |
| AADSTS50010 | AudienceUriValidationFailed - hedef kitle URİ'si hiçbir belirteç Hedef Kitleleri yapılandırıldıktan uygulaması için doğrulama başarısız oldu. |
| AADSTS50011 | InvalidReplyTo - yanıt adresi eksik, yanlış, veya uygulama için yapılandırılan yanıt adresleriyle eşleşmiyor.  Bir çözüm, Azure Active Directory uygulamayı bu yanıt adresi eksik ekleyin veya birisi sizin için bunu Active Directory'de uygulamanızı yönetmek için gerekli izinlere sahip olun.|
| AADSTS50012 | AuthenticationFailed - kimlik doğrulaması aşağıdaki nedenlerden birinden dolayı başarısız oldu:<ul><li>İmzalama sertifikasının konu adı yetkili değil</li><li>Yetkili bir konu adı için eşleşen bir güvenilir yetkili ilke bulunamadı</li><li>Sertifika zinciri, geçerli değil.</li><li>İmzalama sertifikası geçerli değil</li><li>İlke kiracıda yapılandırılmadı</li><li>İmzalama sertifikasının parmak izi yetkili değil</li><li>İstemci onayı geçersiz bir imza içeriyor.</li></ul> |
| AADSTS50013 | InvalidAssertion - onaylama geçersiz çeşitli nedenlerle - range-- hatalı biçimlendirilmiş - yenileme belirteci süresi dolmuş sürümü, geçerli bir saat içinde API belirteci veren eşleşmiyor onaylama birincil yenileme belirteci değil. |
| AADSTS50014 | GuestUserInPendingState - kullanıcının kullanım bir bekleyen durumda. Konuk kullanıcı hesabı henüz tam olarak oluşturulmaz. |
| AADSTS50015 | ViralUserLegalAgeConsentRequiredState - yasal yaş grubu onay gerektirir. |
| AADSTS50017 | CertificateValidationFailed - sertifika doğrulaması başarısız oldu, aşağıdaki nedenlerle nedenler:<ul><li>Güvenilir sertifika listesinde verme sertifikası bulunamıyor</li><li>Beklenen CrlSegment bulunamıyor</li><li>Güvenilir sertifika listesinde verme sertifikası bulunamıyor</li><li>Delta CRL dağıtım noktası, karşılık gelen bir CRL dağıtım noktası olmadan yapılandırılmış</li><li>Bir zaman aşımı sorun nedeniyle geçerli CRL segmentleri alınamıyor</li><li>CRL indirilemiyor</li></ul>Kiracı yöneticisine başvurun. |
| AADSTS50020 | UserUnauthorized - kullanıcılar, bu uç noktasını çağırmak yetkiniz yok. |
| AADSTS50027 | InvalidJwtToken - aşağıdaki nedenlerden dolayı geçersiz JWT belirteci:<ul><li>Bir kerelik anahtar talebi içermiyor, alt talep</li><li>konu tanımlayıcısı uyuşmazlığı</li><li>idToken taleplerinde yinelenen talep</li><li>beklenmeyen veren</li><li>beklenmeyen hedef kitle</li><li>geçerli zaman aralığı içinde değil </li><li>belirteç biçimi doğru değil</li><li>Verenin dış kimlik belirteci imza doğrulaması başarısız oldu.</li></ul> |
| AADSTS50029 | Geçersiz URI - etki alanı adı geçersiz karakterler içeriyor. Kiracı yöneticisine başvurun. |
| AADSTS50032 | WeakRsaKey - hatalı kullanıcı zayıf bir RSA anahtarı kullanma girişimi gösterir. |
| AADSTS50033 | RetryableError - veritabanı işlemleriyle ilgili değil, geçici bir hata gösterir. |
| AADSTS50034 | Bu uygulamada oturum açmanız UserAccountNotFound - hesabın dizine eklenmesi gerekir. |
| AADSTS50042 | UnableToGeneratePairwiseIdentifierWithMissingSalt - İlkesi ikili tanımlayıcısını oluşturmak için gereken güvenlik değeri eksik. Kiracı yöneticisine başvurun. |
| AADSTS50043 | UnableToGeneratePairwiseIdentifierWithMultipleSalts |
| AADSTS50048 | SubjectMismatchesIssuer - konu uyuşmazlıkları veren istemci onayı talep edin. Kiracı yöneticisine başvurun. |
| AADSTS50049 | NoSuchInstanceForDiscovery - bilinmeyen veya geçersiz örneği. |
| AADSTS50050 | MalformedDiscoveryRequest - isteği hatalı biçimlendirilmiş. |
| AADSTS50053 | Kullanıcı çok fazla kez yanlış kullanıcı kimliği veya parola ile oturum açmaya çünkü IdsLocked - hesap kilitlendi. |
| AADSTS50055 | InvalidPasswordExpiredPassword - parola süresi doldu. |
| AADSTS50056 | Geçersiz veya null parola-parola deposunda bu kullanıcı için mevcut değil. |
| AADSTS50057 | UserDisabled - kullanıcı hesabı devre dışı bırakıldı. Hesap bir yönetici tarafından devre dışı bırakıldı. |
| AADSTS50058 | UserInformationNotProvided - bir kullanıcı oturum açmamış olduğunu anlamına gelir. Bu kullanıcı kimliği doğrulanmamış ve henüz oturum açmadı olduğunda, beklenen ortak bir hatadır.</br>Bu hata, burada kullanıcının daha önce oturum açıp açmamasına SSO bağlamında önerilir, bu, SSO oturum değil bulunamadı veya geçersiz anlamına gelir.</br>Bu hata uygulamaya komut istemi, döndürülebilir = belirtilmedi. |
| AADSTS50059 | MissingTenantRealmAndNoUserInformationProvided - Kiracı tanımlama bilgileri ya da istek bulunamadı veya sağlanan kimlik bilgilerini tarafından kapsanan. Sorunu gidermek için Kiracı Yöneticisi kullanıcı başvurabilirsiniz. |
| AADSTS50061 | SignoutInvalidRequest - oturum kapatma isteği geçersiz. |
| AADSTS50064 | CredentialAuthenticationError - kullanıcı adı veya parola kimlik bilgileri doğrulaması başarısız oldu. |
| AADSTS50068 | SignoutInitiatorNotParticipant - oturum kapatma başarısız oldu. Oturum kapatma başlatılan uygulama geçerli oturumda bir katılımcı değil. |
| AADSTS50070 | SignoutUnknownSessionIdentifier - oturum kapatma başarısız oldu. Oturum kapatma isteği mevcut oturumlara eşleşmedi bir ad tanımlayıcısı belirtildi. |
| AADSTS50071 | SignoutMessageExpired - oturum kapatma isteğin süresi doldu. |
| AADSTS50072 | UserStrongAuthEnrollmentRequiredInterrupt - kullanıcı ikinci faktörlü kimlik doğrulaması (etkileşimli) kaydolması gerekir. |
| AADSTS50074 | Güçlü kimlik doğrulaması UserStrongAuthClientAuthNRequiredInterrupt - gereklidir ve kullanıcı MFA testini geçemedi. |
| AADSTS50076 | UserStrongAuthClientAuthNRequired - yönetici tarafından bir yapılandırma değişikliği nedeniyle yapılan veya yeni bir konuma taşınmış olduğundan, kullanıcının çok faktörlü kimlik doğrulaması kaynağa erişmek için kullanmanız gerekir. Kaynak için yeni bir authorize isteğinin ile yeniden deneyin. |
| AADSTS50079 | Yönetici tarafından bir yapılandırma değişikliği nedeniyle UserStrongAuthEnrollmentRequired - yapılan veya kullanıcının yeni bir konuma taşınmış olduğundan, kullanıcının çok faktörlü kimlik doğrulaması kullanmak için gereklidir. |
| AADSTS50085 | Yenileme belirteci için sosyal IDP oturum açma bilgileri gerekiyor. Kullanıcıdan kullanıcı adı-parola ile yeniden oturum açmasını isteyin |
| AADSTS50086 | SasNonRetryableError |
| AADSTS50087 | SasRetryableError - hizmet geçici olarak kullanılamıyor. Yeniden deneyin. |
| AADSTS50089 | Akış belirtecinin süresi doldu - Kimlik Doğrulaması Başarısız Oldu. Kullanıcı oturum açma kullanıcı adı'yla yeniden deneyin.-parola. |
| AADSTS50097 | DeviceAuthenticationRequired - cihaz kimlik doğrulaması gereklidir. |
| AADSTS50099 | PKeyAuthInvalidJwtUnauthorized - JWT imzası geçersiz. |
| AADSTS50105 | EntitlementGrantsNotFound - oturum açmış olan kullanıcının oturum açmış olan uygulama için bir role atanmadı. Uygulamaya kullanıcı atama. Daha fazla bilgi için:[https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role). |
| AADSTS50107 | InvalidRealmUri - istenen Federasyon bölgesi nesnesi yok. Kiracı yöneticisine başvurun. |
| AADSTS50120 | ThresholdJwtInvalidJwtFormat - JWT üstbilgi sorun. Kiracı yöneticisine başvurun. |
| AADSTS50124 | ClaimsTransformationInvalidInputParameter - talep dönüştürme Geçersiz girdi parametresi içerir. İlke güncelleştirmek için kiracı yöneticisine başvurun. |
| AADSTS50125 | PasswordResetRegistrationRequiredInterrupt - oturum açma parolasını sıfırlama veya parola kaydı girişi nedeniyle yarıda kesildi. |
| AADSTS50126 | InvalidUserNameOrPassword - geçersiz kullanıcı adı veya parola nedeniyle kimlik bilgileri doğrulanırken hata oluştu. |
| AADSTS50127 | BrokerAppNotInstalled - kullanıcının, bu içeriğe erişim kazanmak için bir aracı uygulamayı yüklemeleri gerekir. |
| AADSTS50128 | Geçersiz etki alanı adı - Kiracı tanımlama bilgisi yok ya da istek bulunamadı veya sağlanan kimlik bilgilerini tarafından kapsanan. |
| AADSTS50129 | DeviceIsNotWorkplaceJoined - cihaz kaydetmek için çalışma alanına katılma gereklidir. |
| AADSTS50131 | ConditionalAccessFailed - hatalı Windows cihaz durumu, şüpheli etkinlik, erişim ilkesi veya güvenlik ilkesi kararları nedeniyle engellenen istek gibi çeşitli koşullu erişim hataları gösterir. |
| AADSTS50132 | SsoArtifactInvalidOrExpired - oturum parola sona erme veya son parola değişikliği nedeniyle geçerli değil. |
| AADSTS50133 | SsoArtifactRevoked - oturum parola sona erme veya son parola değişikliği nedeniyle geçerli değil. |
| AADSTS50134 | DeviceFlowAuthorizeWrongDatacenter - yanlış bir veri merkezi. OAuth 2.0 cihaz akıştaki bir uygulama tarafından başlatılan bir isteği yetkilendirmek için yetki verme taraf özgün istek bulunduğu aynı veri merkezinde olması gerekir. |
| AADSTS50135 | PasswordChangeCompromisedPassword - parola değişikliği hesabı riski nedeniyle gereklidir. |
| AADSTS50136 | RedirectMsaSessionToApp - tek MSA oturum algılandı. |
| AADSTS50139 | SessionMissingMsaOAuth2RefreshToken - oturum eksik bir dış yenileme belirteci dolayı geçersiz. |
| AADSTS50140 | Kullanıcı oturum açma KmsiInterrupt - "Oturumumu açık bırak" kesinti nedeniyle bu hata oluştu. Daha fazla bilgi almak için Bağıntı Kimliği, İstek Kimliği ve Hata kodu ile [bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md). |
| AADSTS50143 | Oturum uyuşmazlığı - kullanıcı Kiracı etki alanı ipucu farklı kaynak nedeniyle eşleşmediği için oturum geçersiz.  [Bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md) daha ayrıntılı bilgi edinmek için bağıntı kimliği, istek kimliği ve hata koduna sahip. |
| AADSTS50144 | InvalidPasswordExpiredOnPremPassword - kullanıcı Active Directory parolasının süresi doldu. Kullanıcı için yeni bir parola veya kullanıcı parolalarını sıfırlamak için Self Servis sıfırlama aracını kullanın. |
| AADSTS50146 | MissingCustomSigningKey - bu uygulamanın uygulamaya özel bir imzalama anahtarı ile yapılandırılması gereklidir. Bu şekilde yapılandırılmamış veya anahtarın süresi dolmuş ya da anahtar henüz geçerli değil. |
| AADSTS50147 | MissingCodeChallenge - kod sınama parametresinin boyutu geçerli değil. |
| AADSTS50155 | DeviceAuthenticationFailed - cihaz kimlik doğrulaması bu kullanıcı için başarısız oldu. |
| AADSTS50158 | ExternalSecurityChallenge - dış güvenlik sınaması karşılanmadı. |
| AADSTS50161 | InvalidExternalSecurityChallengeConfiguration - dış sağlayıcı tarafından gönderilen talepleri değil yeterli veya dış sağlayıcı için eksik talep istendi. |
| AADSTS50166 | ExternalClaimsProviderThrottled - talep sağlayıcısı için istek gönderilemedi. |
| AADSTS50168 | İstemci ChromeBrowserSsoInterruptRequired - Windows 10 hesapları uzantısı aracılığıyla bir SSO belirteci alma özelliğine sahip, ancak istekte belirteç bulunamadı veya sağlanan belirtecin süresi sona erdi. |
| AADSTS50169 | InvalidRequestBadRealm - bölge yapılandırılmış bir bölgesi geçerli hizmet ad alanı değil. |
| AADSTS50170 | MissingExternalClaimsProviderMapping - dış denetimler eşlemesi eksik. |
| AADSTS50177 | ExternalChallengeNotSupportedForPassthroughUsers - dış sınama kullanıcılar için geçiş desteklenmiyor. |
| AADSTS50178 | SessionControlNotSupportedForPassthroughUsers - oturum denetimi kullanıcılar için geçiş desteklenmiyor. |
| AADSTS50180 | WindowsIntegratedAuthMissing - tümleşik Windows kimlik doğrulaması gereklidir. Sorunsuz SSO için kiracıyı etkinleştirin. |
| AADSTS50187 | DeviceInformationNotProvided - hizmet cihaz kimlik doğrulaması gerçekleştiremedi. |
| AADSTS51000 | RequiredFeatureNotEnabled - özelliği devre dışı bırakıldı. |
| AADSTS51001 | DomainHintMustbePresent - etki alanı ipucu şirket içi güvenlik tanımlayıcısı veya şirket içi UPN ile mevcut olması gerekir. |
| AADSTS51004 | UserAccountNotInDirectory - kullanıcı hesabının dizinde mevcut değil. |
| AADSTS51005 | TemporaryRedirect - istenen bilgileri konum üst bilgisinde Belirtilen URI yer olduğunu gösteren 307, HTTP durumu eşdeğerdir. Yanıtla ilişkili konum üst bilgisi, bu durum iletisini aldığınızda izleyin. Özgün istek yöntemi POST olduğunda, yeniden yönlendirilen istek de POST yöntemini kullanır. |
| AADSTS51006 | ForceReauthDueToInsufficientAuth - tümleşik Windows kimlik doğrulaması gereklidir. Tümleşik Windows kimlik doğrulaması talep eksik bir oturum belirteci kullanarak oturum açmış kullanıcı. Yeniden oturum açmak için Kullanıcı isteği. |
| AADSTS52004 | DelegationDoesNotExistForLinkedIn - kullanıcı onayı LinkedIn kaynaklara erişim sağlamadı. |
| AADSTS53000 | DeviceNotCompliant - koşullu erişim ilkesi uyumlu cihaz gerektirir ve cihaz uyumlu değil. Intune gibi onaylanmış bir MDM sağlayıcısıyla kullanıcı cihazını kaydetmesi gerekir. |
| AADSTS53001 | DeviceNotDomainJoined - koşullu erişim ilkesi, bir etki alanına katılmış cihaz gerektirir ve etki alanına katılmış cihaz değil. Cihaz kullanıcı kullanmak bir etki alanına katıldı. |
| AADSTS53002 | -ApplicationUsedIsNotAnApprovedApp kullanılan uygulaması koşullu erişim için onaylanmış bir uygulama değil. Kullanıcı bir uygulama onaylı uygulamalar listesinden erişmek için kullanılacak kullanması gerekir. |
| AADSTS53003 | Koşullu erişim ilkeleri tarafından BlockedByConditionalAccess - erişim engellendi. Erişim ilkesi, belirteç yayınında izin vermez. |
| AADSTS53004 | ProofUpBlockedDueToRisk - kullanıcının, bu içeriğe erişmeden önce çok faktörlü kimlik doğrulaması kayıt işlemlerini tamamlamanız gerekir. Kullanıcının çok faktörlü kimlik doğrulamasına kaydolması gerekir. |
| AADSTS54000 | MinorUserBlockedLegalAgeGroupRule |
| AADSTS65001 | DelegationDoesNotExist - kullanıcı veya yönetici bu kullanıcı ve kaynak için uygulama kimliği x ile bir etkileşimli yetkilendirme isteği gönderin kullanmak için izin yok. |
| AADSTS65004 | UserDeclinedConsent - kullanıcı, uygulamaya erişmek için onay verme reddetti. Kullanıcıdan oturum açmayı yeniden denemesini ve uygulamaya izin vermesini isteyin|
| AADSTS65005 | MisconfiguredApplication - uygulamayı gerekli kaynak erişim listesi kaynak tarafından bulunabilen uygulamaları içermiyor veya istemci uygulaması kendi gerekli kaynak erişim listesi veya Graph hizmeti hatalı döndürülen belirtilmedi kaynağa erişim isteğinde bulundu istek veya kaynak bulunamadı. Uygulama SAML destekliyorsa, yanlış tanımlayıcı (varlık) uygulama yapılandırmış olmanız. Aşağıdaki bağlantıyı kullanarak SAML için listelenen çözümü deneyin: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#no-resource-in-requiredresourceaccess-list](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav) |
| AADSTS67003 | ActorNotValidServiceIdentity |
| AADSTS70000 | InvalidGrant - kimlik doğrulaması başarısız oldu. Yenileme belirteci geçerli değil. Hatanın nedeni şunlar olabilir:<ul><li>Belirteç bağlama üstbilgisi boş</li><li>Belirteç bağlama karma eşleşmiyor</li></ul> |
| AADSTS70001 | UnauthorizedClient - uygulamayı devre dışı bırakıldı. |
| AADSTS70002 | InvalidClient - kimlik bilgileri doğrulanırken hata oluştu. Belirtilen client_secret, bu istemci için beklenen değerle eşleşmiyor. Client_secret değerini düzeltin ve yeniden deneyin. Daha fazla bilgi için bkz. [bir erişim belirteci istemek için yetkilendirme kodunu kullanın](v1-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token). |
| AADSTS70003 | UnsupportedGrantType - uygulama bir desteklenmeyen izin verme türü döndürdü. |
| AADSTS70004 | InvalidRedirectUri - uygulama, geçersiz bir yeniden yönlendirme URI'si döndürdü. İstemci tarafından belirtilen yeniden yönlendirme adresi, yapılandırılmış bir adresle veya OIDC onay listesindeki herhangi bir adresle eşleşmiyor. |
| AADSTS70005 | UnsupportedResponseType - uygulama, aşağıdaki nedenlerden dolayı bir desteklenmeyen yanıt türü döndürüldü:<ul><li>yanıt türü 'belirteci' uygulaması için etkin değil</li><li>'id_token' yanıt türü 'OpenID' kapsamı gerektiriyor -kodlanmış wctx içinde desteklenmeyen bir OAuth parametre değeri içeriyor</li></ul> |
| AADSTS70007 | UnsupportedResponseMode - uygulama, desteklenmeyen bir değer döndürdü `response_mode` bir belirteç isterken.  |
| AADSTS70008 | ExpiredOrRevokedGrant - eylemsizlik nedeniyle yenileme belirtecinin süresi doldu. Belirteç XXX verilmiş ve belirli bir süreliğine etkin değil. |
| AADSTS70011 | Uygulama tarafından istenen kapsam InvalidScope - geçersiz. |
| AADSTS70012 | MsaServerError - MSA (tüketici) kullanıcı kimlik doğrulaması sırasında sunucu hatası oluştu. Yeniden deneyin. Bu başarısız olmaya devam ederse [bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md) |
| AADSTS70016 | AuthorizationPending - OAuth 2.0 cihaz akışı hatası. Yetkilendirme beklemede. Cihaz yoklama isteği yeniden deneyecek. |
| AADSTS70018 | BadVerificationCode - kullanıcı cihazı kod akışı yanlış kullanıcı kodunu yazarak nedeniyle geçersiz doğrulama kodu. Yetkilendirme onaylanmadı. |
| AADSTS70019 | Doğrulama kodu CodeExpired - süresi. Kullanıcı oturum açma yeniden sahip. |
| AADSTS75001 | BindingSerializationError - SAML ileti bağlama sırasında bir hata oluştu. |
| AADSTS75003 | UnsupportedBindingError - uygulama, desteklenmeyen bağlama ile ilgili bir hata döndürdü (SAML protokolü yanıt gönderilemez bağlamaları dışındaki HTTP POST aracılığıyla). |
| AADSTS75005 | Saml2MessageInvalid - Azure AD SSO için uygulama tarafından gönderilen SAML isteğini desteklemiyor. |
| AADSTS75008 | SAML isteği beklenmeyen bir hedef olduğu bu yana RequestDeniedError - uygulamadan istek reddedildi. |
| AADSTS75011 | NoMatchedAuthnContextInOutputClaims - kimlik doğrulama yöntemi olarak hizmetiyle kimlik doğrulaması kullanıcının eşleşmiyor kimlik doğrulama yöntemini istedi. |
| AADSTS75016 | Geçersiz NameIdPolicy Saml2AuthenticationRequestInvalidNameIDPolicy - olduğu saml2 tabanlı kimlik doğrulama isteği var. |
| AADSTS80001 | OnPremiseStoreIsNotAvailable - kimlik doğrulama Aracısı Active Directory'ye bağlanamıyor. Aracı sunucularının parolaları doğrulanması gereken kullanıcılar aynı AD ormanında üyeleri ve Active Directory'ye bağlanamıyor olduklarından emin olun. |
| AADSTS80002 | OnPremisePasswordValidatorRequestTimedout - parola doğrulama isteği zaman aşımına uğradı. Active Directory kullanılabilir ve yanıt verdiğinden aracılardan isteklerine olduğundan emin olun. |
| AADSTS80005 | OnPremisePasswordValidatorUnpredictableWebException - kimlik doğrulama Aracısı gelen yanıt işlenirken bilinmeyen bir hata oluştu. İsteği yeniden deneyin. Bu başarısız olmaya devam ederse [bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md) hata hakkında ayrıntılı bilgi edinmek için. |
| AADSTS80007 | OnPremisePasswordValidatorErrorOccurredOnPrem - kimlik doğrulama Aracısı kullanıcının parolasını doğrulayamıyor. Daha fazla bilgi için aracı günlüklerini denetleyin ve Active Directory beklendiği gibi çalıştığını doğrulayın. |
| AADSTS80010 | OnPremisePasswordValidationEncryptionException - kimlik doğrulama Aracısı parolanın şifresini çözemedi. |
| AADSTS80012 | Kullanıcıların OnPremisePasswordValidationAccountLogonInvalidHours - çalıştı dışında izin verilen maksimum oturum açmak (bu AD içinde belirtilen) saat. |
| AADSTS80013 | OnPremisePasswordValidationTimeSkew - kimlik doğrulama girişimi nedeniyle AD ve kimlik doğrulama Aracısı çalıştıran makine arasında eğriltme zaman tamamlanamadı. Düzeltme saati eşitleme sorunları. |
| AADSTS81004 | DesktopSsoIdentityInTicketIsNotAuthenticated - Kerberos kimlik doğrulaması girişimi başarısız oldu. |
| AADSTS81005 | DesktopSsoAuthenticationPackageNotSupported - kimlik doğrulama paketi desteklenmiyor. |
| AADSTS81006 | DesktopSsoNoAuthorizationHeader - yetkilendirme üst bilgi bulundu. |
| AADSTS81007 | DesktopSsoTenantIsNotOptIn - Kiracı için sorunsuz SSO etkin değil. |
| AADSTS81009 | DesktopSsoAuthorizationHeaderValueWithBadFormat - kullanıcının Kerberos anahtarı doğrulayamadı. |
| AADSTS81010 | Kullanıcının Kerberos anahtarının süresi doldu veya geçersiz DesktopSsoAuthTokenInvalid - sorunsuz çoklu oturum açma başarısız oldu. |
| AADSTS81011 | DesktopSsoLookupUserBySidFailed - kullanıcının Kerberos anahtarı içindeki bilgileri temel alan kullanıcı nesnesi bulunamadı. |
| AADSTS81012 | DesktopSsoMismatchBetweenTokenUpnAndChosenUpn - Azure AD'de oturum açmaya çalışan kullanıcı cihazda oturum açmış olan kullanıcıdan farklıdır. |
| AADSTS90002 | InvalidTenantName - Kiracı adı, veri deposunda bulunamadı. Doğru Kiracı kimliğiyle sahip olduğunuzdan emin olun |
| AADSTS90004 | InvalidRequestFormat - istek düzgün şekilde biçimlendirilmemiş. |
| AADSTS90005 | InvalidRequestWithMultipleRequirements - isteği tamamlayamıyor. İstek tanımlayıcısı ve oturum açma ipucuyla birlikte kullanılamaz çünkü geçerli değil.  |
| AADSTS90006 | ExternalServerRetryableError - hizmet geçici olarak kullanılamıyor.|
| AADSTS90007 | InvalidSessionId - hatalı istek. Geçilen oturum kimliği ayrıştırılamıyor. |
| AADSTS90008 | Uygulamayı kullanmak için TokenForItselfRequiresGraphPermission - kullanıcı veya yönetici tarafından onaylanan edilmemiş. En azından uygulama oturum açma belirterek Azure AD erişim gerektirir ve kullanıcı profili izni okuyun. |
| AADSTS90009 | TokenForItselfMissingIdenticalAppIdentifier - uygulama bir belirteç kendisi için istiyor. Belirtilen kaynak GUID tabanlı uygulama kimliği. yalnızca kullanıyorsa, bu senaryo desteklenir |
| AADSTS90010 | NotSupported - algoritması oluşturulamıyor. |
| AADSTS90012 | RequestTimeout - istenen zaman aşımına uğradı. |
| AADSTS90013 | InvalidUserInput - kullanıcı girişi geçerli değil. |
| AADSTS90014 | Kimlik bilgilerinde beklenen bir alan olmadığında MissingRequiredField - çeşitli durumlarda bu hata kodu görünebilir. |
| AADSTS90015 | QueryStringTooLong - sorgu dizesi çok uzun. |
| AADSTS90016 | MissingRequiredClaim - erişim belirteci geçerli değil. Gerekli talebi eksik. |
| AADSTS90019 | MissingTenantRealm - Azure AD Kiracı tanımlayıcısı istek belirleyemedi. |
| AADSTS90022 | AuthenticatedInvalidPrincipalNameFormat - asıl adı biçimi geçerli değil veya beklenen karşılamıyor `name[/host][@realm]` biçimi. Asıl adı gereklidir, konak ve bölge isteğe bağlıdır ve ayarlanmış olabilir null. |
| AADSTS90023 | Kimlik doğrulama hizmet talebi InvalidRequest - geçerli değil. |
| AADSTS90024 | RequestBudgetExceededError - geçici bir hata oluştu. Yeniden deneyin. |
| AADSTS90033 | MsodsServiceUnavailable - Microsoft çevrimiçi dizin hizmeti (MSODS) kullanılamıyor. |
| AADSTS90036 | MsodsServiceUnretryableFailure - MSODS'un tarafından barındırılan bir WCF hizmeti beklenmeyen, denenemez bir hata oluştu. Hata hakkında daha fazla bilgi almak için [bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md). |
| AADSTS90038 | Belirtilen kiracıyı 'Y', NationalCloudTenantRedirection - Ulusal bulut 'X' aittir. Geçerli bulut örneği 'Z' X'ile federasyona değil. Bulut yeniden yönlendirme hata döndürülür. |
| AADSTS90043 | NationalCloudAuthCodeRedirection - özelliği devre dışı bırakıldı. |
| AADSTS90051 | InvalidNationalCloudId - geçersiz bulut tanımlayıcı Ulusal bulut tanımlayıcısını içerir. |
| AADSTS90055 | TenantThrottlingError - gelen çok fazla istek var. Engellenen kiracılar için bu özel durum oluşturulur. |
| AADSTS90056 | BadResourceRequest - bir erişim belirteci, uygulama kodunu kullanmak için bir POST isteği gönderin `/token` uç noktası. Ayrıca, bundan önce bir yetkilendirme kodu sağlayın ve POST isteğinde göndermeden `/token` uç noktası. OAuth 2.0 yetkilendirme kod akışı genel bir bakış için bu makaleye bakın: [ https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code ](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code). Kullanıcıya doğrudan `/authorize` uç noktası bir authorization_code döndürür. Bir istek yayınlayarak `/token` uç noktası, kullanıcı erişim belirteci alır. Azure portalında oturum açın ve denetleme **uygulama kayıtları > uç noktalar** iki uç nokta yapılandırıldığını onaylamak için. |
| AADSTS90072 | PassThroughUserMfaError - bunlar oturum açmış Kiracı ile kullanıcı oturum açtığı dış hesap mevcut değil; Bu nedenle kullanıcı Kiracı MFA gereksinimlerini gerçekleştiremiyor. Hesap kiracıda bir dış kullanıcı olarak önce eklenmesi gerekir. Oturumu kapatın ve oturum açın, farklı bir Azure AD kullanıcı hesabı. |
| AADSTS90081 | Hizmet bir WS-Federation iletiyi işlemeye çalışırken OrgIdWsFederationMessageInvalid - bir hata oluştu. İletinin geçerli değil. |
| AADSTS90082 | OrgIdWsFederationNotSupported - istek için seçilen kimlik doğrulama İlkesi şu anda desteklenmemektedir. |
| AADSTS90084 | OrgIdWsFederationGuestNotAllowed - bu site için konuk hesaplarını izin verilmez. |
| AADSTS90085 | OrgIdWsFederationSltRedemptionFailed - şirket nesne henüz sağlanmamış taşınmadığından çünkü bir belirteç vermek üzere hizmet silemiyor. |
| AADSTS90086 | OrgIdWsTrustDaTokenExpired - kullanıcı DA belirtecin süresi doldu. |
| AADSTS90087 | OrgIdWsFederationMessageCreationFromUriFailed - URI'SİNDEN WS-Federasyon iletisi oluşturulurken bir hata oluştu. |
| AADSTS90090 | GraphRetryableError - hizmet geçici olarak kullanılamıyor. |
| AADSTS90091 | GraphServiceUnreachable |
| AADSTS90092 | GraphNonRetryableError |
| AADSTS90093 | GraphUserUnauthorized - grafik bir istek için Yasak hata kodu döndürdü. |
| AADSTS90094 | AdminConsentRequired - yönetici izni gereklidir. |
| AADSTS90100 | InvalidRequestParameter - parametresi boş veya geçersiz. |
| AADSTS901002 | AADSTS901002: 'Resource' istek parametresi desteklenmiyor. |
| AADSTS90101 | InvalidEmailAddress - sağlanan veriler geçerli bir eposta adresi değil. E-posta adresi biçiminde olmalıdır `someone@example.com`. |
| AADSTS90102 | InvalidUriParameter - değeri geçerli bir mutlak URI olmalıdır. |
| AADSTS90107 | InvalidXml - istek geçerli değil. Verilerinizi geçersiz karakterler olmadığından emin olun.|
| AADSTS90114 | InvalidExpiryDate - kesilecek süresi dolmuş bir belirteci toplu belirteç süre sonu zaman damgası neden olur. |
| AADSTS90117 | InvalidRequestInput |
| AADSTS90119 | InvalidUserCode - kullanıcı kodunun, null veya boş.|
| AADSTS90120 | InvalidDeviceFlowRequest - isteği zaten yetkili veya reddedildi. |
| AADSTS90121 | InvalidEmptyRequest - geçersiz boş istek'i tıklatın.|
| AADSTS90123 | Kimlik veya talep verme sağlayıcısı isteği reddedildi çünkü IdentityProviderAccessDenied - belirteci alamaz. |
| AADSTS90124 | V1ResourceV2GlobalEndpointNotSupported - kaynak desteklenmiyor üzerinden `/common` veya `/consumers` uç noktaları. Kullanım `/organizations` veya kiracıya özgü uç nokta yerine. |
| AADSTS90125 | DebugModeEnrollTenantNotFound - sistemde kullanıcı değil. Kullanıcı adının doğru girildiğinden emin olun. |
| AADSTS90126 | Bu uç noktada DebugModeEnrollTenantNotInferred - kullanıcı türü desteklenmiyor. Sistem, kullanıcının Kiracı Kullanıcı adından çıkarsanamıyor. |
| AADSTS90130 | NonConvergedAppV2GlobalEndpointNotSupported - uygulama desteklenmiyor üzerinden `/common` veya `/consumers` uç noktaları. Kullanım `/organizations` veya kiracıya özgü uç nokta yerine. |
| AADSTS120000 | PasswordChangeIncorrectCurrentPassword |
| AADSTS120002 | PasswordChangeInvalidNewPasswordWeak |
| AADSTS120003 | PasswordChangeInvalidNewPasswordContainsMemberName |
| AADSTS120004 | PasswordChangeOnPremComplexity |
| AADSTS120005 | PasswordChangeOnPremSuccessCloudFail |
| AADSTS120008 | PasswordChangeAsyncJobStateTerminated - denenemez bir hata oluştu.|
| AADSTS120011 | PasswordChangeAsyncUpnInferenceFailed |
| AADSTS120012 | PasswordChangeNeedsToHappenOnPrem |
| AADSTS120013 | PasswordChangeOnPremisesConnectivityFailure |
| AADSTS120014 | PasswordChangeOnPremUserAccountLockedOutOrDisabled |
| AADSTS120015 | PasswordChangeADAdminActionRequired |
| AADSTS120016 | PasswordChangeUserNotFoundBySspr |
| AADSTS120018 | PasswordChangePasswordDoesnotComplyFuzzyPolicy |
| AADSTS120020 | PasswordChangeFailure |
| AADSTS120021 | PartnerServiceSsprInternalServiceError |
| AADSTS130004 | NgcKeyNotFound - kullanıcı asıl yapılandırılmış NGC kimlik anahtarı yok. |
| AADSTS130005 | NgcInvalidSignature - NGC anahtar İmza doğrulandı başarısız oldu.|
| AADSTS130006 | Cihazda NgcTransportKeyNotFound - NGC aktarım anahtarı yapılandırılmamış. |
| AADSTS130007 | NgcDeviceIsDisabled - cihaz devre dışı bırakıldı. |
| AADSTS130008 | NgcDeviceIsNotFound - NGC anahtar tarafından başvurulan cihaz bulunamadı. |
| AADSTS135010 | KeyNotFound |
| AADSTS140000 | InvalidRequestNonce - istek nonce sağlanmadı. |
| AADSTS140001 | InvalidSessionKey - oturum anahtarı geçerli değil.|
| AADSTS165900 | InvalidApiRequest - geçersiz istek'i tıklatın. |
| AADSTS220450 | UnsupportedAndroidWebViewVersion - Chrome WebView sürüm desteklenmiyor. |
| AADSTS220501 | İnvalidCrlDownload |
| AADSTS221000 | Yalnızca cihaz belirteçlerini kabul etmesini DeviceOnlyTokensNotSupportedByResource - kaynak yapılandırılmadı. |
| AADSTS240001 | Cihazları Azure AD'ye kaydetme BulkAADJTokenUnauthorized - kullanıcı yetkili değil. |
| AADSTS240002 | RequiredClaimIsMissing - id_token kullanılamaz olarak `urn:ietf:params:oauth:grant-type:jwt-bearer` verin.|
| AADSTS530032 | BlockedByConditionalAccessOnSecurityPolicy - Kiracı Yöneticisi bu istek engelleyen bir güvenlik ilkesi yapılandırdı. İsteğiniz ilke gereksinimlerini karşılayıp karşılamadığını belirlemek için Kiracı düzeyinde tanımlanan güvenlik ilkelerini denetleyin. |
| AADSTS700016 | UnauthorizedClient_DoesNotMatchRequest - directory/kiracıda uygulama bulunamadı. Uygulama değil Kiracı Yöneticisi tarafından yüklenmemiş veya kiracıdaki herhangi bir kullanıcı tarafından onay varsa bu durum oluşabilir. Uygulama tanımlayıcısı değeri yanlış veya kimlik doğrulaması isteğinizi yanlış kiracıya göndermiş. |
| AADSTS700020 | InteractionRequired - erişim izni etkileşimini gerektirir. |
| AADSTS700022 | Birden fazla kaynak içerdiğinden InvalidMultipleResourcesScope - giriş parametresi kapsamı için sağlanan değer geçerli değil. |
| AADSTS700023 | InvalidResourcelessScope - belirtilen değer giriş parametresi kapsamı geçerli değil, isteği bir erişim belirteci. |
| AADSTS1000000 | UserNotBoundError - bağlama API henüz oldu edilmemiş bir dış IDP ile de kimlik doğrulaması Azure AD kullanıcı gerektirir. |
| AADSTS1000002 | BindCompleteInterruptError - bağlama başarıyla tamamlandı, ancak kullanıcı bilgilendirilmesi gerekir. |

## <a name="next-steps"></a>Sonraki adımlar

* Bir sorunuz veya aradığınız ne bulamıyor? Bir GitHub sorunu oluşturun veya bakın [geliştiriciler için destek ve Yardım seçeneklerini](active-directory-develop-help-support.md) diğer yollar hakkında bilgi edinmek için Yardım alabilir ve destek.
