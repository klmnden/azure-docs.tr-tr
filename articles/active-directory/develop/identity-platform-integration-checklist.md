---
title: Microsoft kimlik platformu ile tümleştirme | Azure
description: En iyi yöntemler ve ortak oversights Microsoft identity platformuna (v2.0) ile tümleştirdiğinizde öğrenin.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: ryanwi
ms.reviewer: lenalepa, sureshja, jesakowi
ms.custom: aaddev
ms.openlocfilehash: e9070127780659142ab8f956a8016622ecfea144
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540142"
---
# <a name="microsoft-identity-platform-integration-checklist"></a>Microsoft kimlik platformu tümleştirme denetim listesi

Microsoft kimlik platformu tümleştirme denetim listesi, yüksek kaliteli ve güvenli bir tümleştirme kılavuzu için tasarlanmıştır. En iyi uygulamaları vurgular ve Microsoft kimlik platformu ile tümleştirdiğinizde ortak oversights, bu nedenle kalitesini ve güvenliğini kimlik platformu ile uygulamanızın tümleştirme bulunduğundan emin olmak için düzenli olarak listesini gözden geçirin. Denetim listesi, tüm uygulamanızın gözden geçirmek için tasarlanmamıştır. Platforma geliştirmeye denetim içeriğini değişikliğe tabi olduğu.

Yeni başlıyorsanız, kullanıma [belgeleri](index.yml) kimlik doğrulaması temel bilgileri, Microsoft kimlik platformu uygulama senaryolarında ve daha fazla bilgi edinmek için.

## <a name="testing-your-integration"></a>Tümleştirme testi

Uygulamanız ile etkili bir şekilde tümleşiktir emin olmak için aşağıdaki denetim listesini kullanın [Microsoft kimlik platformu](https://docs.microsoft.com/legal/mdsa).

### <a name="basics"></a>Temel

|   |   |
|---|---|
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Okumanız ve anlamanız [Microsoft Platformu ilkeleri](https://docs.microsoft.com/legal/mdsa). Uygulamanızın kullanıcıları ve platform korumak için tasarlanmışlardır gibi ana hatlarıyla belirtilen koşulları uyduğundan emin olun. |

### <a name="ownership"></a>Sahipliği

|   |   |
|---|---|
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Kaydolun ve uygulamaları yönetmek için kullandığınız hesapla ilişkili bilgileri güncel olduğundan emin olun. |

### <a name="branding"></a>Markalama

|   |   |
|---|---|
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Bağlı kalmayı [marka yönergeleri uygulamalar için](howto-add-branding-in-azure-ad-apps.md). |
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanız için anlamlı bir ad ve logo sağlar. Bu bilgiler, uygulamanızın onay istemi görünür. Böylece kullanıcılar bilinçli kararlar verebilirsiniz temsilcisi şirket/ürünün adını ve logosunu olduğundan emin olun. Ticari markaları ihlal değil emin olun. |

### <a name="privacy"></a>Gizlilik

|   |   |
|---|---|
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanızın kullanım koşulları, hizmet ve gizlilik bildirimi bağlantıları verilmektedir. |

### <a name="security"></a>Güvenlik

|   |   |
|---|---|
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Tüm, yeniden yönlendirme URI'leri sahipliğini korur ve DNS kayıtları için güncel kalmalarını sağlayın. Joker karakter (*) içinde bir URI'leri kullanmayan. Web apps için tüm bir URI'leri, güvenli ve şifrelenmiş (örneğin, https şeması kullanma) olduğundan emin olun. Genel istemciler için platforma özel yeniden yönlendirme URI'leri varsa (genellikle iOS ve Android için) kullanın. Aksi takdirde, kullanım yeniden yönlendirme URI'leri uygulamanıza geri çağrılırken çakışmalarını önlemek için rastgele yüksek miktarda. Uygulamanızı bir yalıtılmış web Aracısı'ndan kullanılıyorsa kullanabilir https://login.microsoftonline.com/nativeclient. Gözden geçirin ve tüm kullanılmayan veya gereksiz yeniden yönlendirme URI'leri düzenli olarak kesim. |
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanız bir dizinde kayıtlı değilse, en aza indirmek ve el ile uygulama kayıt sahipleri listesinde izleyin. |
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Desteğini etkinleştirme [OAuth2 örtük verme akışı](v2-oauth2-implicit-grant-flow.md) açıkça gerekmedikçe. Geçerli senaryosu hakkında bilgi edinin [burada](v1-oauth2-implicit-grant-flow.md#suitable-scenarios-for-the-oauth2-implicit-grant). |
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Kullanmayın [kaynak sahibi parola kimlik bilgileri akışı (ROPC)](v2-oauth-ropc.md), hangi kullanıcıların parolalarını doğrudan işler. Bu akış, yüksek derecede güven ve kullanıcı Etkilenme gerektirir ve daha güvenli, diğer akışlar kullanıldığında kullanılmalıdır. |
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Koruma ve uygulama kimlik bilgilerinizi yönetin. Kullanım [sertifika kimlik bilgileri](active-directory-certificate-credentials.md), parola kimlik bilgilerini değil (istemci gizli anahtarlar). Parola kimlik bilgisi kullanmanız gerekirse, el ile ayarlamanız gerekmez. Kullanmayın, kod veya yapılandırma kimlik bilgilerini depolamak ve hiçbir zaman insanlar tarafından işlenmesine izin. Mümkünse, kullanın [kimliklerini Azure kaynakları için yönetilen](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) veya [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) depolamak ve düzenli olarak kimlik bilgilerinizi döndürmek için. |
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanızın en az ayrıcalık izinleri isteyen emin olun. Yalnızca uygulamanızın kesinlikle gereken izinleri ve yalnızca sor gereksinim duyarsınız. Farklı anlamak [tür izinler](v1-permissions-and-consent.md#types-of-permissions). Yalnızca gerekli uygulama izinlerini kullanın. temsilci izinleri mümkün olan yerlerde kullanın. Microsoft Graph izinleri tam bir listesi için bkz. Bu [izinler başvurusu](https://docs.microsoft.com/graph/permissions-reference). |
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Microsoft kimlik platformu kullanarak API güvenliğini sağlama, bunu kullanıma izinler dikkatlice düşünün. Çözümünüz için doğru ayrıntı nedir ve hangi izinler yönetici onayı gerektiren göz önünde bulundurun. Yetkilendirme karar vermeden önce gelen belirteçlerdeki beklenen izinleri denetleyin. |

### <a name="implementation"></a>Uygulama

|   |   |
|---|---|
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Modern kimlik doğrulaması çözümleri kullanın (OAuth 2.0 [Openıd Connect](v2-protocols-oidc.md)) güvenli bir şekilde kullanıcılarının oturumunu açmak. |
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Yok – kendiniz protokoller uygulamak kullanın [Microsoft tarafından desteklenen kimlik doğrulama kitaplıkları](reference-v2-libraries.md) (MSAL, sunucusu ara yazılımı). İle entegre ettik kimlik doğrulama Kitaplığı'nın en son sürümü kullandığınızdan emin olun. |
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulamanız için gerekli veriler aracılığıyla kullanılabilir ise [Microsoft Graph](https://developer.microsoft.com/graph), tek tek API'si yerine Microsoft Graph uç noktası'nı kullanarak bu verileri izinlerini istek. |

### <a name="end-user-experience"></a>Son kullanıcı deneyimi

|   |   |
|---|---|
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | [Onay deneyimini anlama](application-consent-experience.md) ve son kullanıcılara ve yöneticilere uygulamanızı güven belirlemek için yeterli bilgiye sahip onay istemi uygulamanızın parçalarını yapılandırın. |
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Bir kullanıcı sessiz deneyerek uygulamanızı kullanırken oturum açma kimlik bilgileri girmeniz gerekiyor sayısını en aza etkileşimli akışlar önce kimlik doğrulaması (sessiz belirteç edinme işlemi). |
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | "Komut istemi onay her oturum için =" kullanmayın. Yalnızca istemi kullanmak için ek izinler (örneğin, uygulamanızın gerekli izinleri değiştirdiyseniz) onay isteyin gerektiğini belirlediyseniz onay =. |
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygunsa, kullanıcı verilerini uygulamanızla zenginleştirin. Kullanım [Microsoft Graph API](https://developer.microsoft.com/graph) bunu yapmanın kolay bir yoludur. [Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer) başlamanıza yardımcı olabilecek bir araç. |
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Yöneticiler için kendi Kiracı kolayca onay verebilir böylece uygulamanız için gerekli izinler kümesini kaydedin. Kullanım [artımlı onay](azure-ad-endpoint-comparison.md#incremental-and-dynamic-consent) ilgilendiriyor veya ilk Başlat menüsünde istendiğinde kullanıcı işlemlerini birbirine karıştırmaktadır izinleri isteyen kullanıcıların nedenini anlamasına yardımcı olmak için çalışma zamanında uygulamanızı. |
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Uygulama bir [temiz çoklu oturum kapatma deneyimi](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-6-SignOut). Gizlilik ve güvenlik gereksinimi ve bir iyi kullanıcı deneyimi sağlar. |

### <a name="testing"></a>Test ediliyor

|   |   |
|---|---|
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | Test [koşullu erişim ilkeleri](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-6-SignOut) kullanıcılarınızın uygulamanızı kullanma yeteneğini etkileyebilir. |
| ![Onay kutusu](./media/active-directory-integration-checklist/checkbox-two.svg) | (Örneğin, iş veya Okul hesapları, kişisel Microsoft hesapları, çocuk hesapları ve bağımsız hesapları için) desteklemeyi planladığınız tüm olası hesaplarıyla uygulamanızı test edin. |

## <a name="additional-resources"></a>Ek kaynaklar

V2.0 hakkında ayrıntılı bilgileri keşfedin:

* [Microsoft kimlik Platformu (v2.0 genel bakış)](v2-overview.md)
* [Microsoft kimlik platformu protokolleri başvurusu](active-directory-v2-protocols.md)
* [Erişim belirteçleri başvurusu](access-tokens.md)
* [Kimlik belirteçleri başvurusu](id-tokens.md)
* [Kimlik doğrulaması Kitaplığı Başvurusu](reference-v2-libraries.md)
* [İzinler ve onay Microsoft kimlik platformu](v2-permissions-and-consent.md)
* [Microsoft Graph API'si](https://developer.microsoft.com/graph)