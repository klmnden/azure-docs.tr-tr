---
title: Azure AD'de kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme | Microsoft Docs
description: Azure AD'de kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme öğrenin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2018
ms.author: celested
ms.reviewer: luleon, jeedes
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9396fbc470f25e3cf6fad883ab525af1f96e96a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2019
ms.locfileid: "56188758"
---
# <a name="how-to-customize-claims-issued-in-the-saml-token-for-enterprise-applications"></a>Nasıl yapılır: Kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme

Bugün Azure Active Directory (Azure AD) çoklu oturum açma ile Azure AD uygulama galerisinde yanı sıra özel uygulamalar önceden tümleştirilmiş iki uygulama da dahil olmak üzere çoğu kuruluş uygulamaları destekler. Bir kullanıcı, uygulamanın SAML 2.0 protokolünü kullanarak Azure AD üzerinden kimliğini doğrular, Azure AD belirteç (bir HTTP POST) aracılığıyla uygulamaya gönderir. Ve daha sonra uygulama doğrular ve belirteci yerine bir kullanıcı adı ve parola bilgilerini isteyen kullanıcının oturumunu açmak için kullanır. Bu SAML belirteçlerini, "talepler" olarak bilinen kullanıcı hakkında bilgiler içerir.

A *talep* bildiren bir kimlik sağlayıcısı, sorunu bu kullanıcı için belirteç içinde bir kullanıcı hakkında bilgi. İçinde [SAML belirteci](https://en.wikipedia.org/wiki/SAML_2.0), bu veriler genellikle SAML özniteliği deyimde yer alır. Kullanıcının benzersiz kimliği genellikle SAML ad tanımlayıcısı da bilinen konu temsil edilir.

Varsayılan olarak, Azure AD uygulamanızı Azure AD'de kullanıcının kullanıcı adını (AKA kullanıcı asıl adı) değerini içeren bir NameIdentifier talebini içeren bir SAML belirteci verir. Bu değer, kullanıcıyı benzersiz şekilde tanımlayabilir. SAML belirtecindeki Ayrıca, kullanıcının e-posta adresi, ad ve soyadını içeren ek talepleri de içerir.

Görüntülemek veya uygulamaya SAML belirtecinde verilen talepleri düzenlemek için Azure portalında uygulama açın. Ardından **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** onay kutusu **kullanıcı öznitelikleri** uygulama bölümü.

![Kullanıcı öznitelikleri bölümü][1]

SAML belirtecinde verilen talepleri düzenlemeniz gerekebilir neden iki olası nedeni vardır:
* Uygulamayı farklı bir URI'leri talep kümesi gerektirir veya talep değerleri hedefine yazıldı.
* Uygulama, Azure AD'de depolanan kullanıcı adı (AKA kullanıcı asıl adı) dışında bir şey olacak şekilde NameIdentifier talebini gerektirdiği şekilde dağıtıldı.

Varsayılan talep değerleri düzenleyebilirsiniz. SAML belirteci öznitelikleri tablodaki talep satırı seçin. Bu açılır **özniteliğini Düzenle** bölümüne ve ardından talep ada, değere ve taleple ilişkili ad alanı düzenleyebilirsiniz.

![Kullanıcı özniteliği Düzenle][2]

(Dışında NameIdentifier) talep üzerine tıklayarak açılır bağlam menüsünü kullanarak da kaldırabilirsiniz **...**  simgesi. Yeni Talep kullanarak da ekleyebilirsiniz **eklemek agentconfigutil** düğmesi.

![Kullanıcı özniteliği Düzenle][3]

## <a name="editing-the-nameidentifier-claim"></a>NameIdentifier talebini düzenleme

Uygulamayı nerede oluştu sorunu çözmek için farklı bir kullanıcı adı kullanarak dağıtılmış, seçin **kullanıcı tanımlayıcısı** açılan menü **kullanıcı öznitelikleri** bölümü. Bu eylem, birkaç farklı seçenek içeren bir iletişim kutusu sağlar:

![Kullanıcı özniteliği Düzenle][4]

### <a name="attributes"></a>Öznitelikler

İstenen kaynağı seçin `NameIdentifier` (veya Nameıd) talep. Aşağıdaki seçenekler arasından seçim.

| Ad | Açıklama |
|------|-------------|
| Email | Kullanıcının e-posta adresi |
| userprincipalName | Kullanıcının kullanıcı asıl adı (UPN) |
| onpremisessamaccount | Şirket içi ad'nizden Azure AD'ye eşitlenen SAM hesabı adı |
| Nesne Kimliği | Azure AD'de kullanıcının nesne kimliği |
| EmployeeID | EmployeeID kullanıcının |
| Dizin genişletmeleri | Dizin genişletmeleri [şirket içi Azure AD Connect Sync kullanarak Active Directory eşitlendi](../hybrid/how-to-connect-sync-feature-directory-extensions.md) |
| 1-15 uzantı öznitelikleri | Şirket içinde Azure AD'ye şemayı genişletmek için kullanılan uzantı öznitelikleri |

### <a name="transformations"></a>Dönüşümler

Özel talep dönüştürme işlevleri de kullanabilirsiniz.

| İşlev | Açıklama |
|----------|-------------|
| **ExtractMailPrefix()** | Etki alanı soneki, e-posta adresi, SAM hesap adı veya kullanıcı asıl adı kaldırır. Bu aracılığıyla geçirilen kullanıcı adı, yalnızca ilk bölümünü ayıklar (örneğin, "joe_smith" yerine joe_smith@contoso.com). |
| **join()** | Bir öznitelik ile doğrulanmış bir etki alanına katılır. Seçili kullanıcı tanıtıcı değeri bir etki alanı varsa, seçili doğrulanmış etki alanına eklemek için kullanıcı adı ayıklayacaksınız. Örneğin, e-posta seçerseniz (joe_smith@contoso.com) kullanıcı tanıtıcı değeri ve doğrulanmış etki alanı olarak select contoso.onmicrosoft.com olarak bu sonuçlanır joe_smith@contoso.onmicrosoft.com. |
| **ToLower()** | Seçilen özniteliğin karakterleri, küçük harf karakterlere dönüştürür. |
| **ToUpper()** | Seçilen özniteliğin karakterleri büyük harf karakterlere dönüştürür. |

## <a name="adding-claims"></a>Talep ekleme

Bir talep eklerken (kesinlikle bir URI düzeni SAML belirtimi uyarınca izleyin gerekmez) bir öznitelik adı belirtebilirsiniz. Değerini dizinde depolanan tüm kullanıcı özniteliklerini ayarlayın veya bir contant değeri, kuruluşunuzdaki tüm kullanıcılar için statik bir giriş kullanın.

![Kullanıcı özniteliği Ekle][7]

Örneğin, bir kullanıcının ait olduğu bölüm kuruluşlarındaki (örneğin, Sales) talep olarak göndermek gerekir. Talep adı uygulama tarafından beklenen şekilde girin ve ardından **user.department** değeri.

> [!NOTE]
> Belirli bir kullanıcının varsa depolanan bir seçili öznitelik için değer, ardından bu talebi belirteç çıkarılan değil.

> [!TIP]
> **User.onpremisesecurityidentifier** ve **user.onpremisesamaccountname** kullanıcı verilerini eşitleme Active Directory kullanılarak şirket içinde olduğunda yalnızca desteklenen [Azure AD Aracı bağlanma](../hybrid/whatis-hybrid-identity.md).

## <a name="restricted-claims"></a>Kısıtlı talepleri

SAML bazı kısıtlı talep vardır. Ardından Azure AD, bu talepler eklerseniz, bu talepler göndermez. Kısıtlı SAML talep kümesi şunlardır:

    | Talep türü (URI) |
    | ------------------- |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/expiration |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/expired |
    | http://schemas.microsoft.com/identity/claims/accesstoken |
    | http://schemas.microsoft.com/identity/claims/openid2_id |
    | http://schemas.microsoft.com/identity/claims/identityprovider |
    | http://schemas.microsoft.com/identity/claims/objectidentifier |
    | http://schemas.microsoft.com/identity/claims/puid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier[MR1] |
    | http://schemas.microsoft.com/identity/claims/tenantid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod |
    | http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/groups |
    | http://schemas.microsoft.com/claims/groups.link |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/role |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/wids |
    | http://schemas.microsoft.com/2014/09/devicecontext/claims/iscompliant |
    | http://schemas.microsoft.com/2014/02/devicecontext/claims/isknown |
    | http://schemas.microsoft.com/2012/01/devicecontext/claims/ismanaged |
    | http://schemas.microsoft.com/2014/03/psso |
    | http://schemas.microsoft.com/claims/authnmethodsreferences |
    | http://schemas.xmlsoap.org/ws/2009/09/identity/claims/actor |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/samlissuername |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/confirmationkey |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authorizationdecision |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authentication |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/sid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/denyonlysid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlywindowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdeviceclaim |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsfqbnversion |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowssubauthority |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsuserclaim |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/x500distinguishedname |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/ispersistent |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier |
    | http://schemas.microsoft.com/identity/claims/scope |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD'de uygulama yönetimi](../manage-apps/what-is-application-management.md)
* [Azure AD uygulama galerisinde bulunmayan uygulamalar çoklu oturum açmayı yapılandırın](../manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
* [SAML tabanlı çoklu oturum açma sorunlarını giderme](howto-v1-debug-saml-sso-issues.md)

<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/user-attribute-section.png
[2]: ./media/active-directory-saml-claims-customization/edit-claim-name-value.png
[3]: ./media/active-directory-saml-claims-customization/delete-claim.png
[4]: ./media/active-directory-saml-claims-customization/user-identifier.png
[5]: ./media/active-directory-saml-claims-customization/extractemailprefix-function.png
[6]: ./media/active-directory-saml-claims-customization/join-function.png
[7]: ./media/active-directory-saml-claims-customization/add-attribute.png
