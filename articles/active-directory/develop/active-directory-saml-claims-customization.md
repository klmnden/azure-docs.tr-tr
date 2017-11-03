---
title: "Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen talepler özelleştiriliyor | Microsoft Docs"
description: "Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen talepler özelleştirmeyi öğrenin"
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: a1a33705648a982631315439f99425bab59ad1d1
ms.sourcegitcommit: 1131386137462a8a959abb0f8822d1b329a4e474
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2017
---
# <a name="customizing-claims-issued-in-the-saml-token-for-enterprise-applications-in-azure-active-directory"></a>Azure Active Directory'de kurumsal uygulamalar için SAML belirtecinde verilen talepler özelleştiriliyor
Bugün Azure Active Directory çoklu oturum açma üzerinde Azure AD uygulama galerisinde yanı sıra özel uygulamalar önceden tümleştirilmiş her iki uygulamayı da dahil olmak üzere çoğu kuruluş uygulamalarıyla destekler. SAML 2.0 protokolü kullanarak Azure AD üzerinden bir uygulama için kullanıcı kimliği doğruladığında, Azure AD bir belirteç (aracılığıyla bir HTTP POST) uygulamaya gönderir. Ve daha sonra uygulama doğrular ve bir kullanıcı adı ve parola istemek yerine kullanıcı oturum belirteci kullanır. Bu SAML belirteçleri "talep" olarak bilinen kullanıcı hakkında bilgi içerir.

İçinde kimlik-seslendir, "talep" bir kimlik sağlayıcısı bildiren bir kullanıcı bu kullanıcı için sorun belirteci içinde hakkındaki bilgilerdir. İçinde [SAML belirteci](http://en.wikipedia.org/wiki/SAML_2.0), bu veri genellikle SAML özniteliği deyiminde içeriyor. Kullanıcının benzersiz kimliği genellikle SAML ad tanımlayıcısı da bilinen konu gösterilir.

Varsayılan olarak, Azure Active Directory uygulamanıza bir NameIdentifier taleple Azure AD'de kullanıcının kullanıcı adını (AKA kullanıcı asıl adı) değerini içeren bir SAML belirteci verir. Bu değer, kullanıcıyı benzersiz şekilde tanımlayabilir. SAML belirteci, kullanıcının e-posta adresi, ilk ad ve Soyadı içeren ek talepleri de içerir.

Görüntülemek veya uygulama için SAML belirtecinde verilen talepler düzenlemek için Azure Portal'da uygulamayı açın. Ardından **Görünüm ve diğer tüm kullanıcı özniteliklerini düzenleme** onay kutusu **kullanıcı öznitelikleri** uygulama bölümü.

![Kullanıcı öznitelikleri bölümü][1]

Neden SAML belirtecinde verilen talepler düzenlemek için gerekebilecek iki olası nedeni vardır:
* Uygulama, bir talep URI'ler farklı kümesi gerektiren veya talep değerleri için yazılmış.
* Uygulama Azure Active Directory'de depolanan kullanıcı adı (AKA kullanıcı asıl adı) dışında bir şey olması NameIdentifier talep gerektiren bir şekilde dağıtıldı.

Varsayılan talep değerleri düzenleyebilirsiniz. SAML belirteci öznitelikleri tabloda talep satırı seçin. Bu açılır **öznitelik Düzenle** bölüm ve daha sonra talep adını, değeri ve taleple ilişkili ad alanı düzenleyebilirsiniz.

![Kullanıcı özniteliği Düzenle][2]

Talepleri (dışında NameIdentifier) tıklayarak açar bağlam menüsünü kullanarak da kaldırabilirsiniz **...**  simgesi.  Yeni talepleri kullanarak da ekleyebilirsiniz **Ekle özniteliği** düğmesi.

![Kullanıcı özniteliği Düzenle][3]

## <a name="editing-the-nameidentifier-claim"></a>NameIdentifier talep düzenleme
Uygulama süredir olduğu sorunu çözmek için farklı bir kullanıcı adı kullanılarak dağıtılan, tıklayın **kullanıcı tanımlayıcısı** içinde açılan **kullanıcı öznitelikleri** bölümü. Bu eylem bir iletişim kutusu birkaç farklı seçeneklerle sağlar:

![Kullanıcı özniteliği Düzenle][4]

Aşağı açılır menüsünde, seçin **user.mail** kullanıcının e-posta adresi dizininde NameIdentifier talep ayarlamak için. Ya da seçin **user.onpremisessamaccountname** kullanıcıya ayarlamak için kullanıcının içi AD'den Azure AD'ye eşitlenen SAM hesap adı.

Özel de kullanabilirsiniz **ExtractMailPrefix()** e-posta adresi, SAM hesap adı veya kullanıcı asıl adı etki alanı soneki kaldırmak için işlevi. Bu yalnızca ilk bölümü aracılığıyla geçirilen kullanıcı adının ayıklar (örneğin, "joe_smith" yerine joe_smith@contoso.com).

![Kullanıcı özniteliği Düzenle][5]

Şimdi de ekledik **join()** kullanıcı tanımlayıcısı değeri ile doğrulanan etki alanına katılmak için işlevi. join() işlevinde seçtiğinizde **kullanıcı tanımlayıcısı** önce e-posta adresi veya kullanıcı asıl adı olarak gibi kullanıcı tanımlayıcısı seçin ve ardından ikinci açılan doğrulanmış etki alanınızı seçin. Doğrulanmış etki alanı ile e-posta adresi seçin ve ardından Azure AD ilk değer joe_smith gelen kullanıcı adı ayıklar varsa joe_smith@contoso.com ve contoso.onmicrosoft.com ile ekler. Aşağıdaki örneğe bakın:

![Kullanıcı özniteliği Düzenle][6]

## <a name="adding-claims"></a>Talep ekleme
Bir talep eklerken (URI düzeni SAML spec göre izlemek için kesinlikle gerekmez) öznitelik adı belirtebilirsiniz. Değerini dizinde saklanan tüm kullanıcı özniteliklerini ayarlayın.

![Kullanıcı öznitelik Ekle][7]

Örneğin, bir talep (örneğin, satış) olarak kuruluşlarındaki kullanıcının ait olduğu bölüm göndermeniz gerekir. Uygulama tarafından beklendiği gibi talep adını girin ve ardından **user.department** değeri olarak.

> [!NOTE]
> Belirli bir kullanıcının seçili öznitelik için depolanan hiçbir değer varsa, daha sonra bu talep belirteçte çıkarılan değil.

> [!TIP]
> **User.onpremisesecurityidentifier** ve **user.onpremisesamaccountname** kullanıcı verilerini eşitleme Active Directory kullanarak şirket içi, yalnızca desteklenen [Azure AD Aracı bağlanmak](../active-directory-aadconnect.md).

## <a name="restricted-claims"></a>Kısıtlı talepleri

SAML bazı kısıtlı talep vardır. Bu talep eklerseniz, Azure AD bu talepler göndermez. Kısıtlanmış SAML talep kümesi şunlardır:

    | Talep türü (URI) |
    | ------------------- |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/Expiration |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/Expired |
    | http://schemas.microsoft.com/identity/Claims/accesstoken |
    | http://schemas.microsoft.com/identity/Claims/openid2_id |
    | http://schemas.microsoft.com/identity/Claims/identityprovider |
    | http://schemas.microsoft.com/identity/Claims/objectidentifier |
    | http://schemas.microsoft.com/identity/Claims/puid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/nameidentifier [MR1] |
    | http://schemas.microsoft.com/identity/Claims/tenantid |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/authenticationinstant |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/AuthenticationMethod |
    | http://schemas.microsoft.com/accesscontrolservice/2010/07/Claims/identityprovider |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/groups |
    | http://schemas.microsoft.com/Claims/Groups.Link |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/role |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/wids |
    | http://schemas.microsoft.com/2014/09/devicecontext/Claims/iscompliant |
    | http://schemas.microsoft.com/2014/02/devicecontext/Claims/isknown |
    | http://schemas.microsoft.com/2012/01/devicecontext/Claims/ismanaged |
    | http://schemas.microsoft.com/2014/03/psso |
    | http://schemas.microsoft.com/Claims/authnmethodsreferences |
    | http://schemas.xmlsoap.org/ws/2009/09/identity/Claims/Actor |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/samlissuername |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/confirmationkey |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsaccountname |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/primarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/primarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/authorizationdecision |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/Authentication |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/Sid |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/denyonlyprimarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/denyonlyprimarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/denyonlysid |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/denyonlywindowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsdeviceclaim |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsfqbnversion |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowssubauthority |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/windowsuserclaim |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/x500distinguishedname |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/UPN |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/groupsid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/SPN |
    | http://schemas.microsoft.com/ws/2008/06/identity/Claims/ispersistent |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/Claims/privatepersonalidentifier |
    | http://schemas.microsoft.com/identity/Claims/Scope |

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](../active-directory-apps-index.md)
* [Azure Active Directory uygulama galerisinde bulunmayan uygulamalar için çoklu oturum açmayı yapılandırma](../application-config-sso-how-to-configure-federated-sso-non-gallery.md)
* [Sorun giderme SAML tabanlı çoklu oturum açma](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/user-attribute-section.png
[2]: ./media/active-directory-saml-claims-customization/edit-claim-name-value.png
[3]: ./media/active-directory-saml-claims-customization/delete-claim.png
[4]: ./media/active-directory-saml-claims-customization/user-identifier.png
[5]: ./media/active-directory-saml-claims-customization/extractemailprefix-function.png
[6]: ./media/active-directory-saml-claims-customization/join-function.png
[7]: ./media/active-directory-saml-claims-customization/add-attribute.png
