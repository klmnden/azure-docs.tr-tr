---
title: Azure Active Directory'de - FHIR için Azure API gizli bir istemci uygulaması kaydetme
description: Bu makalede, gizli bir istemci uygulaması Azure Active Directory'ye kaydetmeniz açıklanmaktadır.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 064cf4bb0b6e9454a4049cc6c32b51b09e5b2a53
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824256"
---
# <a name="register-a-confidential-client-application-in-azure-active-directory"></a>Azure Active Directory'de bir gizli istemci uygulamayı kaydetme

Bu makalede, gizli bir istemci uygulaması Azure Active Directory'ye kaydetmeniz öğreneceksiniz. Bir istemci uygulama kaydı bir kullanıcı ve isteği bir erişim sağlamak adına kimlik doğrulaması için kullanılan bir uygulamayı Azure Active Directory gösterimidir [kaynak uygulaması](register-resource-azure-ad-client-app.md). Gizli dizi tutun ve bu gizli erişim belirteci isteğinde bulunurken sunmak için güvenilir bir uygulama gizli istemci uygulamasıdır. Sunucu tarafı uygulamalar gizli uygulamalar örnekleridir.

Portalda yeni bir gizli uygulama kaydetmek için aşağıdaki adımları izleyin.

## <a name="app-registrations-in-azure-portal"></a>Azure portalında uygulama kayıtları

1. [Azure portalda](https://portal.azure.com) sol taraftaki gezinti panelinden **Azure Active Directory**’ye tıklayın.

2. İçinde **Azure Active Directory** dikey penceresinde **uygulama kayıtları (Önizleme)**:

    ![Azure Portalı'nı tıklatın. Yeni uygulama kaydı.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Tıklayın **yeni kayıt**.

## <a name="register-a-new-application"></a>Yeni uygulama kaydetme

1. Uygulama görünen adı verin.

2. Yanıt URL'si sağlayın. Bu ayrıntılar daha sonra değiştirilebilir ancak uygulamanızın yanıt URL'sini biliyorsanız, şimdi girin.

    ![Yeni gizli bir istemci uygulama kaydı.](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT.png)

## <a name="api-permissions"></a>API izinleri

Sonraki API izinleri ekleyin:

1. Açık **API izinleri** ve seçin, [FHIR API kaynak uygulama kaydı](register-resource-azure-ad-client-app.md):

    ![Gizli istemci. API izinleri](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-Permissions.png)

2. Tıklayın **izin Ekle**

3. API kaynağınızı seçin:

    ![Gizli istemci. API'lerim](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-MyApis.png)

4. Gizli uygulama için bir kullanıcı adına sorabilirsiniz kapsamlarınızı (izinler) seçin:

    ![Gizli istemci. Temsilcili İzinler](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-DelegatedPermissions.png)

## <a name="application-secret"></a>Uygulama gizli anahtarı

1. Bir uygulama gizli anahtarı (gizli) oluşturun:

    ![Gizli istemci. Uygulama gizli anahtarı](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-SECRET.png)

2. Gizli dizi süre ve açıklama sağlayın.

3. Oluşturduktan sonra portalda yalnızca bir kez görüntülenir. Bir not edin ve güvenle saklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, gizli bir istemci uygulaması Azure Active Directory'ye kaydetmeniz öğrendiniz. Ardından, azure'da bir FHIR API dağıtın.
 
>[!div class="nextstepaction"]
>[Açık kaynak FHIR sunucusu dağıtma](fhir-oss-powershell-quickstart.md)