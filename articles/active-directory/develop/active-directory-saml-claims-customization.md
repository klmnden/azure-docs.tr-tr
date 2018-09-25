---
title: Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme | Microsoft Docs
description: Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2018
ms.author: celested
ms.reviewer: jeedes
ms.custom: aaddev
ms.openlocfilehash: 80842f7e99ee0c58f1615892f3c3c4adf03119b6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956981"
---
# <a name="how-to-customize-claims-issued-in-the-saml-token-for-enterprise-applications-in-azure-ad"></a>Nasıl yapılır: Azure AD'de kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme

Bugün Azure Active Directory çoklu oturum açma ile Azure AD uygulama galerisinde yanı sıra özel uygulamalar önceden tümleştirilmiş iki uygulama da dahil olmak üzere çoğu kuruluş uygulamaları destekler. Bir kullanıcı, uygulamanın SAML 2.0 protokolünü kullanarak Azure AD üzerinden kimliğini doğrular, Azure AD belirteç (bir HTTP POST) aracılığıyla uygulamaya gönderir. Ve daha sonra uygulama doğrular ve belirteci yerine bir kullanıcı adı ve parola bilgilerini isteyen kullanıcının oturumunu açmak için kullanır. Bu SAML belirteçlerini, "talepler" olarak bilinen kullanıcı hakkında bilgiler içerir.

İçinde kimlik-konuşurken, "talep" bildiren bir kimlik sağlayıcısı, o kullanıcı için sorun belirteci içindeki kullanıcı hakkındaki bilgileri. İçinde [SAML belirteci](http://en.wikipedia.org/wiki/SAML_2.0), bu veriler genellikle SAML özniteliği deyimde yer alır. Kullanıcının benzersiz kimliği genellikle SAML ad tanımlayıcısı da bilinen konu temsil edilir.

Varsayılan olarak, Azure Active Directory, Azure AD'de kullanıcının kullanıcı adını (AKA kullanıcı asıl adı) değerini içeren bir NameIdentifier talebini içeren uygulamanıza SAML belirteci verir. Bu değer, kullanıcıyı benzersiz şekilde tanımlayabilir. SAML belirtecindeki Ayrıca, kullanıcının e-posta adresi, ad ve soyadını içeren ek talepleri de içerir.

Görüntülemek veya uygulamaya SAML belirtecinde verilen talepleri düzenlemek için Azure portalında uygulama açın. Ardından **görünümü ve diğer tüm kullanıcı özniteliklerini düzenleyin** onay kutusu **kullanıcı öznitelikleri** uygulama bölümü.

![Kullanıcı öznitelikleri bölümü][1]

SAML belirtecinde verilen talepleri düzenlemeniz gerekebilir neden iki olası nedeni vardır:
* Uygulamayı farklı bir URI'leri talep kümesi gerektirir veya talep değerleri hedefine yazıldı.
* Uygulama, Azure Active Directory'de depolanan kullanıcı adı (AKA kullanıcı asıl adı) dışında bir şey olacak şekilde NameIdentifier talebini gerektirdiği şekilde dağıtıldı.

Varsayılan talep değerleri düzenleyebilirsiniz. SAML belirteci öznitelikleri tablodaki talep satırı seçin. Bu açılır **özniteliğini Düzenle** bölümüne ve ardından talep ada, değere ve taleple ilişkili ad alanı düzenleyebilirsiniz.

![Kullanıcı özniteliği Düzenle][2]

(Dışında NameIdentifier) talep üzerine tıklayarak açılır bağlam menüsünü kullanarak da kaldırabilirsiniz **...**  simgesi. Yeni Talep kullanarak da ekleyebilirsiniz **eklemek agentconfigutil** düğmesi.

![Kullanıcı özniteliği Düzenle][3]

## <a name="editing-the-nameidentifier-claim"></a>NameIdentifier talebini düzenleme
Uygulamayı nerede oluştu sorunu çözmek için tıklayarak farklı bir kullanıcı adı kullanarak dağıtılmış, **kullanıcı tanımlayıcısı** açılan menü **kullanıcı öznitelikleri** bölümü. Bu eylem, birkaç farklı seçenek içeren bir iletişim kutusu sağlar:

![Kullanıcı özniteliği Düzenle][4]

Açılan menü, seçin **user.mail** NameIdentifier talebini dizinde kullanıcının e-posta adresi olarak ayarlanacak. Ya da seçin **user.onpremisessamaccountname** kullanıcıya ayarlamak için kullanıcının şirket içi ad'nizden Azure AD'ye eşitlenen SAM hesabı adı.

Özel kullanabilirsiniz **ExtractMailPrefix()** e-posta adresi, SAM hesap adı veya kullanıcı asıl adı etki alanı soneki kaldırılacağı işlevi. Bu aracılığıyla geçirilen kullanıcı adı, yalnızca ilk bölümünü ayıklar (örneğin, "joe_smith" yerine joe_smith@contoso.com).

![Kullanıcı özniteliği Düzenle][5]

Şimdi de ekledik **join()** kullanıcı tanıtıcı değeri ile doğrulanmış etki alanına katılmak için işlevi. join() işlevinde seçtiğinizde **kullanıcı tanımlayıcısı** ilk e-posta adresi veya kullanıcı asıl adı olarak gibi kullanıcı tanımlayıcısı seçin ve ardından ikinci açılan menü doğrulanmış etki alanınızı seçin. E-posta adresi doğrulanan etki alanı seçin ve ardından Azure AD alanından ilk değer joe_smith kullanıcı adını ayıklar, joe_smith@contoso.com ve contoso.onmicrosoft.com ile ekler. Aşağıdaki örneğe bakın:

![Kullanıcı özniteliği Düzenle][6]

## <a name="adding-claims"></a>Talep ekleme
Bir talep eklerken (kesinlikle bir URI düzeni SAML belirtimi uyarınca izleyin gerekmez) bir öznitelik adı belirtebilirsiniz. Değerini dizinde depolanan tüm kullanıcı özniteliklerini ayarlayın.

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
* [Azure Active Directory'de uygulama yönetimi](../manage-apps/what-is-application-management.md)
* [Azure Active Directory uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açmayı yapılandırma](../manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
* [SAML tabanlı çoklu oturum açma sorunlarını giderme](howto-v1-debug-saml-sso-issues.md)

<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/user-attribute-section.png
[2]: ./media/active-directory-saml-claims-customization/edit-claim-name-value.png
[3]: ./media/active-directory-saml-claims-customization/delete-claim.png
[4]: ./media/active-directory-saml-claims-customization/user-identifier.png
[5]: ./media/active-directory-saml-claims-customization/extractemailprefix-function.png
[6]: ./media/active-directory-saml-claims-customization/join-function.png
[7]: ./media/active-directory-saml-claims-customization/add-attribute.png
