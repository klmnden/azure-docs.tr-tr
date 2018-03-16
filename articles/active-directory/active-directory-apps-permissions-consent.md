---
title: "Azure Active Directory’de uygulamalar, izinler ve onay.| Microsoft Docs"
description: "Azure AD Connect, şirket içi dizinlerinizi Azure Active Directory ile tümleştirir. Bu sayede Azure AD ile tümleşik Office 365, Azure ve SaaS uygulamaları için ortak bir kimlik oluşturabilirsiniz."
keywords: "Azure AD’ye giriş, uygulamalar, Azure AD Connect nedir, active directory yükleme"
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 25897cc4-7687-49b6-b0d5-71f51302b6b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/03/2018
ms.author: billmath
ms.reviewer: jesakowi
ms.custom: oldportal;it-pro;
ms.openlocfilehash: d3e14c18d5e4cd77f4c68d8a5d9d0b915e896695
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2018
---
# <a name="apps-permissions-and-consent-in-azure-active-directory"></a>Azure Active Directory’de uygulamalar, izinler ve onay
Azure Active Directory’de dizininize uygulamalar ekleyebilirsiniz.  Uygulamalar, uygulama türüne bağlı olarak değişir.  Portalda uygulamaları görüntülemek için bir dizin seçin ve uygulamaları belirleyin.

> [!IMPORTANT]
> Microsoft, Azure AD’yi bu makalede bahsedilen Azure Portalı yerine Azure portalındaki [Azure AD yönetim merkezini](https://aad.portal.azure.com) kullanarak yönetmenizi öneriyor.

## <a name="types-of-apps"></a>Uygulama türleri

1. **Tek kiracılı uygulamalar** </br>
    - **Tek kiracılı uygulamalar** - Genellikle iş kolu (LOB) uygulamaları olarak adlandırılır. Bu seçenek, kuruluşunuzdaki bir kişi kendi uygulamasını geliştirip, kuruluştaki kullanıcıların uygulamada oturum açabilmesini istediğinde geçerlidir.
    ![](media/active-directory-apps-permissions-consent/apps2.png)
    - **Uygulama Ara Sunucusu uygulamaları** - Şirket içi bir uygulamayı Azure AD Uygulama Ara Sunucusu ile kullanıma sunduğunuzda, kiracınıza tek kiracılı bir uygulama kaydedilir (Uygulama Ara Sunucusu hizmetine ek olarak). Bu uygulama tüm bulut etkileşimleri (örneğin, kimlik doğrulaması) için şirket içi uygulamanızı temsil eder. (Uygulama Ara Sunucusu, Azure AD Temel veya daha yüksek sürüm gerektirir.)


2. **Çok kiracılı uygulamalar**
    - **Başkalarının onay verebileceği çok kiracılı uygulamalar** - “kuruluşunuzun geliştirdiği tek kiracılı uygulamalar” ile benzerdir. Temel farkı (uygulamadaki mantığa ek olarak), diğer kiracılardan kullanıcıların da uygulamayı onaylayabilmesi ve uygulamada oturum açabilmesidir.</br>
    ![](media/active-directory-apps-permissions-consent/apps4.png)
    - **Başkalarının geliştirdiği ve Contoso’nun onay verebildiği çok kiracılı uygulamalar**. (Veya kısaca “onaylanan uygulamalar”.) “Kuruluşunuzun geliştirdiği çok kiracılı uygulamalar” madalyonunun diğer yüzüdür. Başka bir kuruluş çok kiracılı bir uygulama geliştirdiğinde, kuruluşunuzun kullanıcıları uygulamayı onaylayıp uygulamada oturum açabilir.
    - **Microsoft birinci taraf uygulamaları** - Microsoft hizmetlerini temsil eden uygulamalar. Onay, hizmete kaydolup olmadığınıza bağlıdır. Genellikle uygulama erişimiyle ilgili ilkeler oluştururken kullanılan belirli birinci taraf uygulamalar için bazı özel UX ve mantıklar mevcuttur.</br>
    ![](media/active-directory-apps-permissions-consent/apps3.png)
    - **Önceden tümleştirilmiş uygulamalar** - Popüler SaaS uygulamalarına çoklu oturum açma sağlamak üzere dizininize ekleyebileceğiniz Azure AD Uygulama Galerisinde bulunan uygulamalar.
    - **Azure AD çoklu oturum açma**: SAML 2.0 veya OpenID Connect gibi desteklenen bir oturum açma protokolü aracılığıyla Azure AD ile tümleştirilebilen “Gerçek” SSO. Sihirbaz bu kurulumda size kılavuzluk eder.
    - **Parola ile çoklu oturum açma**: Azure AD, kullanıcının uygulamaya ilişkin kimlik bilgilerini güvenli bir şekilde depolar ve kimlik bilgileri Azure AD Uygulama Erişimi tarayıcı uzantısı ile oturum açma formuna “eklenir”. Bu işlem aynı zamanda “parola kasası oluşturma” olarak bilinir.

## <a name="permissions"></a>İzinler

Uygulama kaydedildiğinde, uygulamayı kaydeden kullanıcı (yani Geliştirici), uygulamanın erişmesi gereken izinleri ve kaynakları tanımlar. (Kaynaklar diğer uygulamalar gibi tanımlanır.) Örneğin, bir posta okuyucu uygulaması geliştiren bir kullanıcı, uygulamasının “Office 365 Exchange Online” kaynağında “Oturum açmış kullanıcı olarak posta kutularına erişim” iznini gerektirdiğini belirtir:
    
![](media/active-directory-apps-permissions-consent/apps6.png)

Bir uygulamanın (istemci) başka bir uygulamadan (kaynak) belirli bir izni istemesi için, kaynak uygulamanın geliştiricisi mevcut olan izni tanımlar. Bu örnekte, “Office 365 Exchange Online” kaynak uygulamasının sahibi olan Microsoft, “Oturum açmış kullanıcı olarak posta kutularına erişim” adlı bir izin tanımlamıştır.

İzinleri tanımlarken, uygulama geliştiricisi izne onay verilip verilemeyeceğini veya yönetici onayı gerektirip gerektirmediğini tanımlamalıdır. Bunun yapılması, geliştiricilerin yalnızca düşük hassasiyete sahip izinler isteyen uygulamaları kullanıcıların onaylamasına izin vermesini sağlar, ancak daha hassas izinlere yöneticilerin onay vermesini gerektirir. Örneğin, “Azure Active Directory” kaynak uygulaması tanımlandığı için kullanıcılar sınırlı salt okunur izinler gerektiren uygulamalara onay verebilir.  Ancak, tam okuma izinleri ve tüm yazma izinleri için yönetici onayı gereklidir.

Yerel istemcilerin kimliği doğrulanmadığı için, yerel istemci uygulaması olarak tanımlanmış bir uygulama yalnızca temsilci atanmış izinler isteyebilir. Bu durum, bir belirteç edinilirken her zaman gerçek bir kullanıcının rol alması gerektiği anlamına gelir. Web uygulamaları ve web API’leri (gizli istemciler) için erişim belirteci alınırken mutlaka Azure AD ile kimlik doğrulaması yapılmalıdır. Bu durum ayrıca, yalnızca uygulama izinleri isteme olasılığına sahip oldukları anlamına gelir. Örneğin, bir arka uç hizmetinin başka bir arka uç hizmetinde kimlik doğrulaması yapması gerekirse bu durum geçerlidir. Yalnızca uygulama izinleri isteyen uygulamalar her zaman yönetici onayı gerektirir.

Özetlemek gerekirse:



- Bir uygulama (istemci) diğer uygulamalar (kaynaklar) için gerekli olan izinleri belirtir.
- Bir uygulama (kaynak) diğer uygulamalara (istemciler) hangi izinlerin sunulduğunu belirtir.
- Bir izin yalnızca uygulama izni ya da temsilci atanmış izin olabilir.
- Temsilci atanmış izin, “kullanıcı onayına izin verir” veya “yönetici onayı gerektirir” olarak işaretlenebilir.
- Uygulama bir istemci olarak (bir kaynak için izinlere ihtiyaç duyduğunu bildirerek), kaynak olarak (hangi izinleri kullanıma sunduğunu bildirerek) veya her ikisi gibi davranabilir.

## <a name="controls"></a>Denetimler

Bu davranışların tümü için kullanılabilen farklı yönetici denetimlerinin listesi aşağıda verilmiştir.

Azure portalında, **yönetim**, **kullanıcı ayarları** altında.

![](media/active-directory-apps-permissions-consent/apps11.png)



- Kullanıcıların uygulamalara onay verip veremeyeceğini denetleyebilirsiniz:

Azure portalında, **kullanıcılar uygulamaların verilerine erişmesine izin verebilir**’i seçin.
![](media/active-directory-apps-permissions-consent/apps12.png)



- Kullanıcıların kendi tek kiracılı iş kolu (LOB) uygulamalarını kaydedip kaydedemeyeceğini denetleyebilirsiniz:

Azure portalında **kullanıcılar uygulamaları kaydedebilir** öğesini seçin.
![](media/active-directory-apps-permissions-consent/apps13.png)

>[!NOTE]
>Kullanıcıların tek kiracılı LOB uygulamaları kaydetmesine izin verseniz bile kaydedilebilecek uygulamalar sınırlıdır.  
>Örneğin, dizin yöneticisi olmayan geliştiriciler.
>
>- Kullanıcılar tek kiracılı bir uygulamayı çok kiracılı uygulama yapamaz.
>- Kullanıcılar, tek kiracılı LOB uygulamalarını kaydederken diğer uygulamalara yalnızca uygulama izinleri isteyemez.
>- Kullanıcılar tek kiracılı LOB uygulamaları kaydederken, yönetici onayı gerektiren izinler için diğer uygulamalara temsilci atanmış izinler isteyemez.
>- Kullanıcılar sahibi olmadıkları uygulamalarda değişiklik yapamaz.


## <a name="example"></a>Örnek

Kiracınızdaki kullanıcılara oturum açma bildirimi gönderdiğiniz “Office 365 için FabrikamMail” uygulamasını örnek olarak alalım. “FabrikamMail”, “Fabrikam, Inc.” tarafından yayımlanan Android’e yönelik bir posta okuma uygulamasıdır. Bu uygulama “başkalarının geliştirdiği ve Contoso’nun onay verebildiği çok kiracılı uygulamalar” sınıfındadır.

Kullanıcıların onaylamasına izin verilirse, kullanıcılar ilk kez oturum açtıklarında bir onay istemi alır: ![](media/active-directory-apps-permissions-consent/apps14.png)

“Posta kutularınıza erişim” , kullanıcının “Office 365 Exchange Online” (yani, Exchange) tarafından sunulan “Oturum açmış kullanıcı olarak posta kutularına erişim” izni için karşılaştığı onay dizesidir.

Office 365’e kaydolduğunuzda eklenen Exchange için ServicePrincipal nesnesine (kaynak) bakarak izinleri görebilirsiniz. ServicePrincipal nesnesini, kiracınızdaki uygulamanın farklı seçenekleri ve yapılandırmaları kaydetmek için kullanılan bir “örneği” olarak düşünebilirsiniz.  PowerShell’de `Get-AzureADServicePrincipal` öğesini kullanarak bu seçeneği görebilirsiniz.

    PS C:\> Get-AzureADServicePrincipal -ObjectId 383f7b97-6754-4d3d-9474-3908ebcba1c6 | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : Office 365 Exchange Online
    AppId                     : 00000002-0000-0ff1-ce00-000000000000
    AppOwnerTenantId          : 
    AppRoleAssignmentRequired : False
    AppRoles                  : {...}
    DisplayName               : Microsoft.Exchange
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {...
                                , class OAuth2Permission {
                                  AdminConsentDescription : Allows the app to have the same access to mailboxes as the signed-in user via Exchange Web Services.
                                  AdminConsentDisplayName : Access mailboxes as the signed-in user via Exchange Web Services
                                  Id                      : 3b5f3d61-589b-4a3c-a359-5dd4b5ee5bd5
                                  IsEnabled               : True
                                  Type                    : User
                                  UserConsentDescription  : Allows the app full access to your mailboxes on your behalf.
                                  UserConsentDisplayName  : Access your mailboxes
                                  Value                   : full_access_as_user
                                },
                                ...}
    PasswordCredentials       : {}
    PublisherName             : 
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {00000002-0000-0ff1-ce00-000000000000/outlook.office365.com, 00000002-0000-0ff1-ce00-000000000000/mail.office365.com, 00000002-0000-0ff1-ce00-000000000000/outlook.com, 
                                00000002-0000-0ff1-ce00-000000000000/*.outlook.com...}
    Tags                      : {}

Kullanıcı "Kabul et" seçeneğine tıkladığında onay başlatılır. İlk olarak, kiracıda “Office 365 için FabrikamMail” uygulamasına yönelik bir ServicePrincipal nesnesi oluşturulur. ServicePrincipal şuna benzer:

    PS C:\> Get-AzureADServicePrincipal -SearchString "FabrikamMail for Office 365" | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : a8b16333-851d-42e8-acd2-eac155849b37
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : FabrikamMail for Office 365
    AppId                     : aba7c072-2267-4031-8960-e7a2db6e0590
    AppOwnerTenantId          : 4a4076e0-a70f-41c6-b819-6f9c4a86df89
    AppRoleAssignmentRequired : False
    AppRoles                  : {}
    DisplayName               : FabrikamMail for Office 365
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {}
    PasswordCredentials       : {}
    PublisherName             : Fabrikam, Inc.
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {aba7c072-2267-4031-8960-e7a2db6e0590}
    Tags                      : {WindowsAzureActiveDirectoryIntegratedApp}

Bir uygulamaya onay verildiğinde aşağıdakiler arasında bir Oauth2PermissionGrant bağlantısı oluşturulur:
  
- kullanıcı nesnesi
- istemci uygulamaları ServicePrincipalName (SPN)
- kaynak uygulamaları ServicePrincipalName (SPN)
- kaynak uygulamadaki izinler.  

FabrikamMail örneğinde şu şekilde görünür:

    PS C:\> Get-AzureADUserOAuth2PermissionGrant -ObjectId ddiggle@aadpremiumlab.onmicrosoft.com | fl *
    
    ClientId    : a8b16333-851d-42e8-acd2-eac155849b37
    ConsentType : Principal
    ExpiryTime  : 05/15/2017 07:02:39 AM
    ObjectId    : M2OxqB2F6EKs0urBVYSbN5d7PzhUZz1NlH25COvLocbJjoxkUFfRQauryBKwBWet
    PrincipalId : 648c8ec9-5750-41d1-abab-c812b00567ad
    ResourceId  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    Scope       : full_access_as_user
    StartTime   : 01/01/0001 12:00:00 AM

(**ClientId**, FabrikamMail’in hizmet sorumlusu nesne kimliği (yeni oluşturulan); **PrincipalId**, kullanıcı nesne kimliği (onay veren kullanıcıya ait); **ResourceId**, Exchange’in hizmet sorumlusu nesne kimliği; Scope ise Exchange içinde onay verilen izindir).

Kullanıcıların onaylamasına izin verilmiyorsa kullanıcılar izin gerektiğini belirten bir ekran görür.

