---
title: Azure Active Directory Uygulama bildirimini anlama | Microsoft Docs
description: "Azure AD kiracısı bir uygulamanın kimlik yapılandırmada temsil eder ve OAuth yetkilendirme, onayı deneyimi ve daha fazlasını kolaylaştırmak için kullanılan Azure Active Directory Uygulama bildirimini ayrıntılı kapsamını."
services: active-directory
documentationcenter: 
author: sureshja
manager: mbaldwin
editor: 
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: sureshja
ms.custom: aaddev
ms.reviewer: elisol
ms.openlocfilehash: d5e18f41d6eb69ccb7eafaa4de2646c4c38df5e2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="understanding-the-azure-active-directory-application-manifest"></a>Azure Active Directory Uygulama bildirimini anlama
Azure Active Directory (AD ile) tümleştirme uygulamaları uygulaması için sürekli kimlik yapılandırma sağlayan bir Azure AD kiracısı ile kayıtlı olması gerekir. Bu yapılandırma, dış ve kimlik doğrulama/yetkilendirme Azure AD ile aracısı için bir uygulama izin senaryoları etkinleştirme zamanında alınmadığında. Azure AD uygulama modeli hakkında daha fazla bilgi için bkz: [ekleme, güncelleştirme ve uygulama kaldırma] [ ADD-UPD-RMV-APP] makalesi.

## <a name="updating-an-applications-identity-configuration"></a>Bir uygulamanın kimlik yapılandırması güncelleştiriliyor
Özellikleri ve derece zorluk, aşağıdakiler de dahil olmak üzere farklılık bir uygulamanın kimlik yapılandırma özelliklerini güncelleştirmek için kullanılabilir gerçekte birden çok seçenek vardır:

* **[Azure portal'ın] [ AZURE-PORTAL] Web kullanıcı arabirimi** , bir uygulamanın en yaygın özelliklerini güncelleştirmenizi sağlar. Bu, uygulamanızın özelliklerini güncelleştirme, hızlı ve en az bir hata potansiyeli yoludur, ancak sonraki iki yöntemleri gibi tüm özellikler için tam erişim sağlamaz.
* Azure Klasik Portalı'nda gösterilmez özellikleri güncelleştirmek için gereken burada daha Gelişmiş senaryolar için değiştirebileceğiniz **uygulama bildirimi**. Bu makalede odak ve sonraki bölümde başlangıç daha ayrıntılı ele alınmıştır.
* Ayrıca mümkün **kullanan bir uygulamayı yazma [grafik API'si] [ GRAPH-API]**  en çaba gerektirir uygulamanızı güncelleştirmek için. Yönetim yazılımı yazma veya uygulama özellikleri otomatik bir şekilde, düzenli olarak güncelleştirmek ihtiyacınız varsa bu rağmen çekici bir seçenek olabilir.

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>Bir uygulamanın kimlik yapılandırmasını güncelleştirmek için uygulama bildirimi kullanma
Aracılığıyla [Azure portal][AZURE-PORTAL], satır içi bildirim Düzenleyicisi'ni kullanarak uygulama bildirimi güncelleştirerek uygulamanızın kimlik yapılandırmasını yönetebilirsiniz. Ayrıca, indirin ve uygulama bildirimi bir JSON dosyası olarak karşıya yükleme. Gerçek bir dosya dizininde depolanır. Uygulama bildirimi yalnızca bir HTTP GET işlemi Azure AD Graph API uygulaması varlık ve yükleme uygulama varlığı üzerinde bir HTTP PATCH işlemi.

Sonuç olarak, biçimini ve uygulama bildirimi özelliklerini anlamak için grafik API'si başvurusu gerekecektir [uygulama varlığı] [ APPLICATION-ENTITY] belgeleri. Uygulama bildirimi olsa karşıya yükleme gerçekleştirilebilir güncelleştirmeleri örnekleri şunlardır:

* **İzin kapsamları (oauth2Permissions) bildirme** web API tarafından kullanıma sunulan. "Gösterme Web API'leri için diğer uygulamaları" konusuna bakın [Azure Active Directory Tümleştirme uygulamalarla] [ INTEGRATING-APPLICATIONS-AAD] oauth2Permissions kullanılarak kullanıcı kimliğine bürünme özelliğini uygulama hakkında bilgi için Temsilci izni kapsamı. Daha önce belirtildiği gibi uygulama varlık özellikleri grafik API'sini belgelenen [varlık ve karmaşık tür] [ APPLICATION-ENTITY] başvurusu makalesinde, bir koleksiyonu olan oauth2Permissions özelliği de dahil olmak üzere, tür [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].
* **Uygulama rolleri (appRoles) uygulamanız tarafından kullanıma sunulan bildirme**. Uygulama varlığın appRoles özellik türü bir koleksiyonudur [uygulama rolü][APPLICATION-ENTITY-APP-ROLE]. Bkz: [rol tabanlı erişim denetimi kullanarak Azure AD bulut uygulamalarında] [ RBAC-CLOUD-APPS-AZUREAD] makale uygulama örneği.
* **Bilinen istemci uygulamaları (knownClientApplications) bildirme**, mantıksal olarak kaynak/web API'si için belirtilen istemci uygulamaları izin tie sağlar.
* **Grup üyelikleri talebi vermek için Azure AD isteği** oturum açmış olan kullanıcının (groupMembershipClaims).  Bu ayrıca kullanıcı dizin rol üyeliklerini ilgili talepleri vermek üzere yapılandırılabilir. Bkz: [AD grupları kullanarak bulut uygulamalarında yetkilendirme] [ AAD-GROUPS-FOR-AUTHORIZATION] makale uygulama örneği.
* **Uygulamanız OAuth 2.0 örtük grant desteklemek izin** akışlar (oauth2AllowImplicitFlow). Grant akış bu tür katıştırılmış JavaScript web sayfaları veya tek sayfa uygulamaları (SPA) ile kullanılır. Örtük yetkilendirme verme hakkında daha fazla bilgi için bkz: [örtük OAuth2 anlama izin akışı Azure Active Directory'de][IMPLICIT-GRANT].
* **X509 kullanımını etkinleştirmek gizli anahtarı olarak sertifikaları** (keyCredentials). Bkz: [yapı hizmeti ve arka plan programı uygulamaları Office 365'te] [ O365-SERVICE-DAEMON-APPS] ve [Geliştirici Kılavuzu'na auth Azure Kaynak Yöneticisi API'si ile] [ DEV-GUIDE-TO-AUTH-WITH-ARM] makaleleri uygulama örnekleri için.
* **Yeni bir uygulama kimliği URI'sini ekleyin** uygulamanızın (identifierURIs[]). Uygulama Kimliği URI, uygulamanın kendi Azure AD kiracısı içinde (veya özel etki alanını doğruladıysanız nitelikli hale geldiğinde çok kiracılı senaryoları için birden çok Azure AD kiracılarıyla) benzersiz şekilde tanımlamak için kullanılır. Bunlar, bir kaynak uygulaması veya bir kaynak uygulaması için bir erişim belirteci alma izni isterken kullanılır. Bu öğe güncelleştirdiğinizde, aynı güncelleştirme uygulamanın ev kiracısında yaşadığı karşılık gelen hizmet sorumlusunun servicePrincipalNames [] koleksiyonuna yapılır.

Uygulama bildirimini Ayrıca uygulama kaydınızı durumunu izlemek için iyi bir yol sağlar. JSON biçiminde kullanılabilir olduğundan, dosya gösterimi, uygulamanızın kaynak kodunu yanı sıra, kaynak denetimine denetlenebilir.

## <a name="step-by-step-example"></a>Adım adım örnek tarafından
Şimdi, uygulamanızın kimlik yapılandırması uygulama bildirimi aracılığıyla güncelleştirmek için gereken adımlarda size yol sağlar. Biz önceki örneklerinden birini bir kaynak uygulamasının yeni bir izin kapsamı bildirmeyi gösteren vurgular:

1. [Azure portalında][AZURE-PORTAL] oturum açın.
2. Kimlik doğruladınız sonra sayfanın sağ üst köşesinden seçerek Azure AD kiracınıza seçin.
3. Seçin **Azure Active Directory** 'ı tıklatın ve sol gezinti bölmesini uzantı **uygulama kayıtlar**.
4. Tıklayın ve listede güncelleştirmek istediğiniz uygulamayı bulun.
5. Uygulama sayfasından tıklatın **bildirim** satır içi bildirim Düzenleyicisi'ni açın. 
6. Bu düzenleyiciyi kullanarak bildirim doğrudan düzenleyebilirsiniz. Bildirim için bir şemayı izlediğinden Not [uygulama varlığı] [ APPLICATION-ENTITY] yukarıda belirtildiği gibi: Örneğin, istediğimizi yeni bir izin uygulama/sunmaya varsayılarak "Employees.Read.All" adlı bizim Kaynak uygulama (API), yalnızca yeni/saniye öğesi oauth2Permissions koleksiyonuna IE eklediğiniz:
   
        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],
   
    Giriş benzersiz olması gerekir ve bu nedenle yeni genel olarak benzersiz bir kimlik (GUID) için oluşturmalıdır `"id"` özelliği. Bu durumda, biz belirtilmediğinden `"type": "User"`, bu izin, kaynak/API uygulama kaydedilir Azure AD tarafından kimliği doğrulanmış herhangi bir hesabı tarafından Kiracı izin verdiği. Bu istemci verir uygulama hesabın adına erişim izni. Açıklama ve görünen ad dizeleri onay sırasında ve Azure portalında görüntülenmesi için kullanılır.
6. Bildirim güncelleştirme bittiğinde, tıklatın **kaydetmek** bildirimi kaydetmek için.  
   
Bildirim kaydedilir, kayıtlı verebilirsiniz eklediğimiz yukarıda yeni izni istemci uygulama erişimi. Bu süre istemci uygulamanın bildirimi düzenlemek yerine Azure portal'ın Web kullanıcı arabirimini kullanarak şunları yapabilirsiniz:  

1. İlk Git **ayarları** yeni API'sine erişim eklemek istediğiniz istemci uygulaması dikey tıklayın **gerekli izinler** ve **bir API seçin**.
2. Daha sonra Kiracı (API) uygulamalarında kayıtlı kaynak listesiyle birlikte sunulur. Seçmek için kaynak uygulama'yı tıklatın veya Ara kutusuna uygulamanın adını yazın. Uygulama buldunuz tıklatın **seçin**.  
3. Bu, olanak sürecek **Select izinleri** sayfasında uygulama izinleri ve kaynak uygulama için kullanılabilir izinlere temsilci listesini gösterir. İstemcinin istenen izinleri listesine eklemek için yeni bir izin seçin. Bu yeni izni "requiredResourceAccess" koleksiyon özelliği de istemci uygulamanın kimlik yapılandırmada depolanır.


Bu kadar. Şimdi kendi yeni kimlik Yapılandırması'nı kullanarak uygulamalarınızı çalışır.

## <a name="next-steps"></a>Sonraki adımlar
* Bir uygulamanın uygulama ve hizmet sorumlusu nesneleri arasındaki ilişki hakkında daha fazla bilgi için bkz: [uygulama ve hizmet asıl nesneleri Azure AD'de][AAD-APP-OBJECTS].
* Bkz: [Azure AD Geliştirici sözlüğü] [ AAD-DEVELOPER-GLOSSARY] bazı Azure Active Directory (AD) Geliştirici kavramları tanımları için.

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için lütfen aşağıdaki Açıklamalar bölümüne kullanın.

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

