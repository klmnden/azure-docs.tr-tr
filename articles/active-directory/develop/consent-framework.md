---
title: Azure Active Directory onay çerçevesine
description: Azure Active Directory ve nasıl, çok kiracılı web ve yerel istemci uygulamaları geliştirmek kolaylaştırır onay çerçevesine hakkında bilgi edinin.
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
ms.date: 11/30/2018
ms.author: ryanwi
ms.reviewer: zachowd, lenalepa, jesakowi
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 93adedc5c1343df1eee05b653b60cfd7e810044c
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540421"
---
# <a name="azure-active-directory-consent-framework"></a>Azure Active Directory onay çerçevesine

Azure Active Directory (Azure AD) onay çerçevesine, çok kiracılı web ve yerel istemci uygulamaları geliştirmeyi kolaylaştırır. Bu uygulamaları burada uygulamanın kayıtlı olandan farklı bir Azure AD kiracısı oturum açma ana kadar kullanıcı hesapları tarafından sağlar. Web API'leri gibi Microsoft Graph API'ye (Azure AD erişim, Intune ve Office 365 Hizmetleri) ve web API'leri yanı sıra diğer Microsoft Hizmetleri API'leri erişmek de gerekebilir.

Bir kullanıcı veya yönetici içerebilir directory verilerine erişme, dizinde kayıtlı ister bir uygulamaya onay vermiş framework dayanır. Örneğin, web istemci uygulaması, Office 365'ten Takvim kullanıcı hakkındaki bilgileri okumak gerekiyorsa kullanıcı istemci uygulama ilk onayı gerekli. İzin verilen sonra istemci uygulaması kullanıcı adına Microsoft Graph API'sini çağırmak ve Takvim bilgileri gerektiğinde mümkün olacaktır. [Microsoft Graph API](https://developer.microsoft.com/graph) (takvimler ve Exchange, siteler ve SharePoint, OneDrive, OneNote Not defterlerinden, Planner görevleri ve çalışma kitaplarından belgelerden listelerinden iletileri gibi Office 365'te verilere erişim sağlar. , Kullanıcılar ve grupları Azure ad ve diğer veri nesneleri daha fazla Microsoft bulut hizmetlerinden yanı sıra excel).

Genel ya da gizli istemciler kullanarak Yetkilendirme kodu verme ve istemci kimlik bilgileri verin gibi onay çerçevesine OAuth 2.0 ve kendi çeşitli akışlar üzerinde oluşturulmuştur. OAuth 2.0 kullanarak Azure AD, telefon, tablet, sunucu veya bir web uygulaması--gibi farklı türlerde istemci uygulamaları oluşturmak ve gerekli kaynaklara erişmesini mümkün kılar.

Onay çerçevesine OAuth2.0 yetkilendirme vermeleri ile kullanma hakkında daha fazla bilgi için bkz. [OAuth 2.0 ve Azure AD kullanarak web uygulamalarına erişim yetkisi verme](v1-protocols-oauth-code.md) ve [Azure AD için kimlik doğrulama senaryoları](authentication-scenarios.md). Microsoft Graph Office 365 için yetkili erişim sağlama hakkında daha fazla bilgi için bkz. [Microsoft Graph ile uygulama kimlik doğrulamasını](https://developer.microsoft.com/graph/docs/authorization/auth_overview).

## <a name="consent-experience---an-example"></a>Onay deneyiminde - örneği

Aşağıdaki adımlar nasıl onayı deneyimi çalıştığı uygulama geliştiricisi ve kullanıcı için gösterir.

1. Belirli bir kaynak/API'sine erişim izni istemek için gereken bir web istemci uygulaması olduğunu kabul edelim. Sonraki bölümde bu yapılandırmayı geçekleştirmeden öğreneceksiniz ancak temelde Azure portalı, yapılandırma sırasında izin isteklerini bildirmek için kullanılır. Diğer yapılandırma ayarları gibi uygulamanın Azure AD kaydı bir parçası haline gelir:

    ![Diğer uygulamalara izinler](./media/quickstart-v1-integrate-apps-with-azure-ad/requiredpermissions.png)

1. Uygulama izinleri güncelleştirildi, uygulamayı çalıştıran ve bir kullanıcı ilk kez kullanmak hakkında sağlamaktır göz önünde bulundurun. İlk olarak, Azure AD'den ait bir yetkilendirme kodunu almak uygulamanın ihtiyacı `/authorize` uç noktası. Yetkilendirme kodu, ardından belirteci yenileyin ve yeni bir erişim almak için kullanılabilir.

1. Kullanıcı kimliği doğrulanmış, Azure AD'nin değilse `/authorize` uç nokta, kullanıcının oturum açmasını ister.

    ![Azure ad kullanıcı veya yönetici oturumu açma](./media/quickstart-v1-integrate-apps-with-azure-ad/usersignin.png)

1. Kullanıcı oturum açtıktan sonra Azure AD kullanıcı bir onay sayfası gerekip gerekmediğini belirler. Bu belirleme, kullanıcı (veya onun kuruluşunun yönetici) zaten uygulama onay verilip üzerinde temel alır. Onay zaten verilmedi, Azure AD kullanıcıdan onayı ister ve çalışması için gerekli izinleri görüntüler. Onay iletişim kutusunda görüntülenen izin kümesiyle eşleşen seçili olanları **temsilci izinleri** Azure portalında.

    ![Kullanıcı onayı deneyimi](./media/quickstart-v1-integrate-apps-with-azure-ad/consent.png)

1. Kullanıcıya izin verir. sonra bir yetkilendirme kodu erişim belirteci alma ve yenileme belirteci için kullanılan, uygulamaya döndürülür. Bu akışı hakkında daha fazla bilgi için bkz. [Web API'sini uygulama türü](web-api.md).

1. Bir yönetici olarak, aynı zamanda tüm kullanıcılar adına uygulamanın temsilci izinleri için kiracınızda onay verebilir. Yönetici onayı onay iletişim kutusunu kiracıdaki her kullanıcı için görüntülenmesini engeller ve yapılabilir [Azure portalında](https://portal.azure.com) Yönetici rolüne sahip kullanıcılar tarafından. Hangi yönetici rolleri için temsilci izinleri izin alma bilgi edinmek için [Azure AD'de Yönetici rolü izinleri](../users-groups-roles/directory-assign-admin-roles.md).

    **Bir uygulamaya onay için temsilci izinleri**

   1. Git **ayarları** uygulamanız için sayfa
   1. Seçin **gerekli izinler**.
   1. Tıklayarak **izinleri verin** düğmesi.

      ![Açık yönetici onayı için izin ver](./media/quickstart-v1-integrate-apps-with-azure-ad/grantpermissions.png)

   > [!IMPORTANT]
   > Açık verme onay kullanarak **izinleri verin** düğmesidir ADAL.js kullanan tek sayfalı uygulamalar için (SPA) şu anda gerekli. Erişim belirteci istendiğinde, aksi takdirde uygulama başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

* Bkz: [uygulamanın çok kiracılı dönüştürme](howto-convert-app-to-be-multi-tenant.md)
* Daha fazla ayrıntı için bilgi [onayı sırasında yetkilendirme kodu verme akışı OAuth 2.0 protokolü katmanında nasıl desteklenir.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code#request-an-authorization-code)
