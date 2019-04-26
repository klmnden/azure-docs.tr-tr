---
title: Azure Active Directory'ye kayıtlı uygulamanızda uygulama rolleri ekleme ve bunları belirteci alma
description: Azure Active Directory'de kayıtlı bir uygulamada uygulama rollerini ekleme hakkında bilgi edinin, kullanıcılar ve gruplar bu rollere atamak ve bunları alma `roles` belirtecinde talep.
services: active-directory
documentationcenter: ''
author: kkrishna
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: kkrishna
ms.reviewer: ''
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 427e293c28f634df9f66a7210d79e0df0d4d063c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60410357"
---
# <a name="how-to-add-app-roles-in-your-application-and-receive-them-in-the-token"></a>Nasıl yapılır: Uygulama rolleri uygulamanıza ekleyin ve bunları belirteci alma

Rol tabanlı erişim denetimi (RBAC), uygulamalar yetkilendirmeyi zorlamak için popüler bir mekanizmadır. RBAC kullanarak, yönetici rolleri ve bireysel kullanıcılar veya gruplar için izinleri verir. Yönetici rolleri farklı kullanıcılara ve gruplara kimlerin hangi içerik ve İşlevler erişebilir ardından atayabilirsiniz.

Uygulama rolleri ve rol talepleri ile RBAC kullanarak, geliştiriciler uygulamalarını yapmalarına çok az çabayla yetkilendirme güvenli bir şekilde uygulayabilir.

Başka bir Azure AD grupları ve grup talepleri gösterildiği yaklaşımdır [WebApp GroupClaims DotNet](https://github.com/Azure-Samples/WebApp-GroupClaims-DotNet). Azure AD grupları ve uygulama rolleri olmadığı Hayır olarak birbirini dışlar; Bunlar dağıtımınızla daha ayrıntılı erişim denetimi sağlamak için kullanılabilir.

## <a name="declare-roles-for-an-application"></a>Bir uygulama için rolleri bildirme

Bu uygulama rolleri tanımlanan [Azure portalında](https://portal.azure.com) uygulamanın kayıt katıştırır.  Azure AD kullanıcı uygulamayı açtığında, yayan bir `roles` kullanıcı, tek tek kullanıcı ve grup üyeliklerini verilmiş her rol için talep.  Kullanıcılar ve gruplar için rol ataması, portalın kullanıcı Arabirimi aracılığıyla yapılan veya program aracılığıyla kullanarak olabilir [Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/azuread-identity-access-management-concept-overview).

### <a name="declare-app-roles-using-azure-portal"></a>Azure portalını kullanarak uygulama rolleri bildirme

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst çubuğunda, hesabınızı seçin ve ardından **dizini Değiştir**.
1. Bir kez **dizin + abonelik** bölmesi açılır; uygulamanızı kaydetmek istediğiniz Active Directory kiracısı seçin **Sık Kullanılanlar** veya **tüm dizinleri**listesi.
1. Seçin **tüm hizmetleri** , sol taraftaki gezinti ve **Azure Active Directory**.
1. İçinde **Azure Active Directory** bölmesinde **uygulama kayıtları** tüm uygulamaların bir listesini görüntülemek için.

     Burada görünmesi istediğiniz uygulamayı göremiyorsanız en üstündeki çeşitli filtreleri kullanın **uygulama kayıtları** liste veya uygulama bulmak için listede aşağı kısıtlamak için liste.

1. Uygulama rolleri tanımlamak istediğiniz uygulamayı seçin.
1. Uygulamanızın dikey penceresinde, seçin **bildirim**.
1. Uygulama bildirimi bularak Düzenle `appRoles` ayarlama ve tüm uygulama rolleri ekleme.

     > [!NOTE]
     > Bu bildirimdeki her rol tanımı farklı bir sahip geçerli **GUID** "Id" özelliği için. `"value"` Her rolün özellik dizelerin tam olarak eşleşmelidir, uygulamadaki kodu kullanılır.
     
1. Bildirim kaydedin.

### <a name="examples"></a>Örnekler

Aşağıdaki örnekte gösterildiği `appRoles` için atayabileceğiniz `users`.

> [!NOTE]
>  `id` Benzersiz bir GUID olması gerekir.

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "User"
      ],
      "displayName": "Writer",
      "id": "d1c2ade8-98f8-45fd-aa4a-6d06b947c66f",
      "isEnabled": true,
      "description": "Writers Have the ability to create tasks.",
      "value": "Writer"
    }
  ],
"availableToOtherTenants": false,
```

Hedef uygulama rolleri tanımlayabilirsiniz `users`, `applications`, veya her ikisini de. Kullanılabilir olduğunda `applications`, uygulama izinlerini uygulama rolleri görülür **gerekli izinler** dikey penceresi. Doğru hedefleyen bir uygulama rolü aşağıdaki örnekte bir `Application`.

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "Application"
      ],
      "displayName": "Consumer Apps",
      "id": "47fbb575-859a-4941-89c9-0f7a6c30beac",
      "isEnabled": true,
      "description": "Consumer apps have access to the consumer data.",
      "value": "Consumer"
    }
  ],
"availableToOtherTenants": false,
```

### <a name="assign-users-and-groups-to-roles"></a>Kullanıcılar ve gruplar rollerine atama

Uygulama rolleri uygulamanızda ekledikten sonra kullanıcılar ve gruplar bu rollerine atayabilirsiniz.

1. İçinde **Azure Active Directory** bölmesinde **kurumsal uygulamalar** gelen **Azure Active Directory** sol taraftaki gezinti menüsünde.
1. Seçin **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

     Burada görünmesi istediğiniz uygulamayı göremiyorsanız en üstündeki çeşitli filtreleri kullanın **tüm uygulamaları** liste veya uygulama bulmak için listede aşağı kısıtlamak için liste.

1. Kullanıcıları veya güvenlik grubunu rollere atamak istediğiniz uygulamayı seçin.
1. Seçin **kullanıcılar ve gruplar** bölmesinde uygulamanın sol taraftaki gezinti menüsünde.
1. Üst kısmındaki **kullanıcılar ve gruplar** listesinden **Kullanıcı Ekle** açmak için düğmeyi **atama Ekle** bölmesi.
1. Seçin **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

     Kullanıcılar ve güvenlik grupları listesi, aramak ve belirli bir kullanıcı veya grup bulmak için bir textbox ile birlikte gösterilir. Bu ekran, tek bir seferde birden çok kullanıcı ve grupları seçmenize olanak sağlar.

1. İşiniz bittiğinde kullanıcıları ve grupları seçme, basın **seçin** düğmesine bir sonraki bölümüne geçmeye alt.
1. Seçin **rolü Seç** seçiciden **ataması ekleme** bölmesi. Daha önce uygulama bildiriminde belirtilen tüm rollerin gösterilir.
1. Bir rolü seçip ENTER tuşuna **seçin** düğmesi.
1. Tuşuna **atama** son kullanıcılar ve gruplar için uygulama atamalarını için alt düğmesine.
1. Kullanıcıları ve grupları eklediğiniz güncelleştirilmiş gösterdiklerini doğrulayın **kullanıcılar ve gruplar** listesi.

## <a name="more-information"></a>Daha fazla bilgi

- [Azure AD uygulama rollerini kullanarak bir web uygulaması yetkilendirmeyi &amp; rol talepleri (örnek)](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapp-roleclaims/)
- [Güvenlik grupları ve uygulama rolleri uygulamalarınızda (Video) kullanma](https://www.youtube.com/watch?v=V8VUPixLSiM)
- [Azure Active Directory, Grup talepleri ve uygulama rolleri ile artık](https://cloudblogs.microsoft.com/enterprisemobility/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles)
- [Azure Active Directory Uygulama bildirimi](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest)
- [AAD erişim belirteçleri](access-tokens.md)
- [AAD `id_tokens`](id-tokens.md)
