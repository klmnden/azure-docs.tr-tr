---
title: Bir hizmet uygulaması Azure Active Directory'de - FHIR için Azure API kaydetme
description: Bu makalede, Azure Active Directory'de bir hizmet uygulaması kaydedilecek açıklanmaktadır.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 19f730c1d8a0fc0f809e8e16016795e725d80b61
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824294"
---
# <a name="register-a-service-client-application-in-azure-active-directory"></a>Bir hizmeti istemci uygulaması Azure Active Directory'de Kaydettir

Bu makalede, bir hizmeti istemci uygulaması Azure Active Directory'ye kaydetmeniz öğreneceksiniz. İstemci uygulama kayıtları kimliğini doğrulamak ve belirteçleri elde etmek için kullanılan uygulamaların Azure Active Directory temsillerini ' dir. Bir hizmeti istemcisi, bir kullanıcının etkileşimli kimlik doğrulaması olmadan bir erişim belirteci almak için bir uygulama tarafından kullanılacak yöneliktir. Bu, belirli uygulama izinlere sahip olması ve erişim belirteci alınırken bir uygulama gizli dizisini (parola) kullanın.

Yeni bir hizmet istemcisi oluşturmak için aşağıdaki adımları izleyin.

## <a name="app-registrations-in-azure-portal"></a>Azure portalında uygulama kayıtları

1. [Azure portalda](https://portal.azure.com) sol taraftaki gezinti panelinden **Azure Active Directory**’ye tıklayın.

2. İçinde **Azure Active Directory** dikey penceresinde **uygulama kayıtları (Önizleme)**:

    ![Azure Portalı'nı tıklatın. Yeni uygulama kaydı.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Tıklayın **yeni kayıt**.

## <a name="service-client-application-details"></a>Hizmet istemci uygulama ayrıntıları

* Hizmet istemcinin görünen adı gerekir ve bir yanıt URL'si de sağlayabilirsiniz ancak bu genellikle kullanılmayacak.

    ![Azure Portalı'nı tıklatın. Yeni hizmet istemci uygulama kaydı.](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-NAME.png)

## <a name="api-permissions"></a>API izinleri

Uygulama rolleri hizmeti istemcisi vermeniz gerekir. 

1. Açık **API izinleri** ve seçin, [FHIR API kaynak uygulama kaydı](register-resource-azure-ad-client-app.md):

    ![Azure Portalı'nı tıklatın. Hizmet İstemcisi API izinleri](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-API-PERMISSIONS.png)

2. Uygulama rolleri seçin, kaynak uygulama üzerinde tanımlanan gördüğünüzden:

    ![Azure Portalı'nı tıklatın. Hizmet istemci uygulaması izinleri](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-APPLICATION-PERMISSIONS.png)

3. Uygulamaya izin verin. Gerekli izinler yoksa, Azure Active Directory yöneticinizle birlikte denetleyin:

    ![Azure Portalı'nı tıklatın. Hizmet istemci yönetici onayı](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-ADMIN-CONSENT.png)

## <a name="application-secret"></a>Uygulama gizli anahtarı

Hizmeti istemcisi, belirteç alınırken kullanılan olacak bir gizli dizinin (parola) gerekir.

1. Tıklayın **sertifikaları &amp; gizli dizileri**

2. Tıklayın **yeni istemci gizli anahtarı**

    ![Azure Portalı'nı tıklatın. Hizmet istemci parolası](media/how-to-aad/portal-aad-register-new-app-registration-SERVICE-CLIENT-SECRET.png)

3. Gizli dizi süre sağlayın.

4. Oluşturulduktan sonra bunu yalnızca bir kez portalda görüntülenir. Bir not edin ve depolamak bir güvenli bir şekilde.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir hizmeti istemci uygulaması Azure Active Directory'ye kaydetmeniz öğrendiniz. Ardından, azure'da bir FHIR API dağıtın.
 
>[!div class="nextstepaction"]
>[Açık kaynak FHIR sunucusu dağıtma](fhir-oss-powershell-quickstart.md)