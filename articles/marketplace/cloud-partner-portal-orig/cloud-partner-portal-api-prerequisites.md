---
title: API önkoşulları | Azure Market
description: Excel'den bulut iş ortağı portalı API'lerini kullanarak Önkoşullar.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: a973ab0a406168756af61900fd35947c8be6d03b
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935392"
---
<a name="api-prerequisites"></a>API önkoşulları
================

Bulut iş ortağı portalı API'lerini kullanmanız gereken iki gerekli programlı varlıkları vardır: bir hizmet sorumlusu ve bir Azure Active Directory (Azure AD) belirtecine erişmesini.


<a name="create-a-service-principal-in-your-azure-active-directory-tenant"></a>Azure Active Directory kiracınızda bir hizmet sorumlusu oluşturma
----------------------------------------------------------------

İlk olarak, Azure AD kiracınızda bir hizmet sorumlusu oluşturmanız gerekir. Bu kiracıyı kendi bulut iş ortağı Portalı'nda izin kümesi atanır. Kişisel kimlik bilgilerinizi kullanarak yerine bu Kiracı olarak API'lerini kullanarak kodunuzu çağırır.  Bir hizmet sorumlusu oluşturma tam açıklama için bkz. [Azure Active Directory kaynaklarına erişmek uygulama ve hizmet sorumlusu oluşturmak için portalı kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal).


<a name="add-the-service-principal-to-your-account"></a>Hesabınız için hizmet sorumlusu ekleme
-----------------------------------------

Hizmet sorumlusu kiracınızdaki oluşturduktan sonra bulut iş ortağı portalı hesabınıza bir kullanıcı olarak ekleyebilirsiniz. Yalnızca bir kullanıcı gibi hizmet sorumlusu sahibi veya katkıda bulunan portalı olabilir.

Hizmet sorumlusu eklemek için aşağıdaki adımları kullanın:

1. Bulut iş ortağı portalı üzerinde oturum. 
2. Tıklayarak **kullanıcılar** sol menü çubuğunda ve **Kullanıcı Ekle**.

   ![Kullanıcı Portalı'na ekleme](./media/cloud-partner-portal-api-prerequisites/add-user.jpg)

3. Gelen **türü** açılır menüsünde, select **hizmet sorumlusu** açıp aşağıdaki ayrıntıları ekleyin:

-   A **kolay ad** hizmet sorumlusunun, örneğin `spAccount`.
-   **Uygulama kimliği**. Bu tanımlayıcı bulmak için Git [Azure portalı](https://portal.azure.com), tıklayın **Azure Active Directory**, seçin **uygulama kayıtları**, uygulamanıza tıklayın.
-   **Kiracı kimliği**olarak da bilinen **dizin kimliği**, Azure AD kiracınız için. Bu tanımlayıcı Azure Active Directory sayfasında bulabilirsiniz [Azure portalında](https://portal.azure.com)altında **özellikleri**.
-   **Nesne kimliği** için hizmet sorumlusu nesnesi. Bu tanımlayıcı, Azure portalından alabilirsiniz. Git **Azure Active Directory**, seçin **uygulama kayıtları**, uygulamanızı ve uygulama adı altında tıklayın **uygulama yerel dizinde yönetilen**. Ardından, Git **özellikleri** sayfasında nesne kimliği bulmak için Uygulamanızı, ancak bunun yerine yönetilen uygulamayı nesne kimliği olduğundan ilk nesne kimliği kapmasını değil emin olun.
-   **Rol** RBAC için kullanılacak hesabı ile ilişkili.

     ![Portalda yönetilen bir uygulama ekleyin](./media/cloud-partner-portal-api-prerequisites/managedapp.png)

1. Tıklayın **Ekle** hizmet sorumlusu hesabınıza eklemek için.

   ![Bir hizmet sorumlusu ekleme](./media/cloud-partner-portal-api-prerequisites/add-service-principal.jpg)


<a name="get-an-azure-ad-access-token"></a>Azure AD erişim belirteci alma
----------------------------

Bulut iş ortağı portalı API'ler, kimlik doğrulaması sırasında aşağıdaki varlıkları ve protokolleri kullanın:

- Kaynaklara erişimi istemek için JSON Web Token (JWT) taşıyıcı belirteç
- [Openıd Connect](https://openid.net/connect/) (OIDC) protokolü, kimlik doğrulamak için
- [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) kimlik yetkilisi olarak

Program aracılığıyla bir JWT belirteci alınırken İlkesi iki yaklaşım vardır:

- .NET için Microsoft kimlik doğrulama kitaplığı kullanma ([MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet)).  .NET geliştiricileri için bu üst düzey bir yaklaşım önerilir. 
- Olun bir **HTTP POST** Azure AD oauth isteği **belirteci** uç nokta biçimi alır:

``` HTTP
POST https://login.microsoftonline.com/<tenant-id>/oauth2/token
    client_id: <application-id>
    client_secret:<application-secret>
    grant_type: client_credentials
    resource: https://cloudpartner.azure.com
```

Şimdi, bu belirteci yetkilendirme üst bilgisi API istekleri için bir parçası olarak geçirebilirsiniz.

``` HTTP
GET https://cloudpartner.azure.com/api/offerTypes?api-version=2016-08-01-preview 
    Accept: application/json
    Authorization: Bearer <access-token>
```
> [!NOTE]
> Açıkça belirtildiği şekilde tüm API'leri için bu başvuru, yetkilendirme üst bilgisi her zaman geçti, varsayılır.

Kimlik doğrulaması hatalarla karşılaşırsanız, istekte çalıştırırsanız, bkz. [kimlik doğrulama hataları sorunlarını giderme](./cloud-partner-portal-api-troubleshooting-authentication-errors.md).
