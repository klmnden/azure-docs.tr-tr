---
title: Genel istemci uygulaması, Azure Active Directory'de - FHIR için Azure API kaydetme
description: Bu makalede, bir ortak istemci uygulamasının Azure Active Directory'ye kaydetmeniz açıklanmaktadır.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 69504bc9ba0420b47a26519a0b112ff102171254
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824282"
---
# <a name="register-a-public-client-application-in-azure-active-directory"></a>Genel istemci uygulaması Azure Active Directory'de Kaydettir

Bu makalede, bir genel uygulama Azure Active Directory'ye kaydetmeniz öğreneceksiniz. İstemci uygulama kayıtlarını Azure Active Directory kimlik doğrulaması yapmak ve bir kullanıcı adına API izinleri isteyin uygulamalar temsillerini ' dir. Genel istemciler, mobil uygulamalar ve gizli anahtarları, gizli engelleyemezsiniz tek sayfalı javascript uygulamaları gibi uygulamalardır. Yordamı benzer [gizli bir istemci kaydetme](register-confidential-azure-ad-client-app.md), ancak genel istemcilere, uygulama gizli tutmak için güvenilir olamayacağından eklemesini gerek yoktur.

## <a name="app-registrations-in-azure-portal"></a>Azure portalında uygulama kayıtları

1. [Azure portalda](https://portal.azure.com) sol taraftaki gezinti panelinden **Azure Active Directory**’ye tıklayın.

2. İçinde **Azure Active Directory** dikey penceresinde tıklayın **uygulama kayıtları (Önizleme)**:

    ![Azure Portalı'nı tıklatın. Yeni uygulama kaydı.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Tıklayın **yeni kayıt**.

## <a name="application-registration-overview"></a>Uygulama kayıt genel bakış

1. Uygulama görünen adı ile verin.

2. Yanıt URL'si sağlayın. Burada kimlik doğrulama kodlarını istemci uygulamasına döndürülecek yanıt URL'dir. Daha fazla yanıt URL'leri eklemek ve varolanları daha sonra düzenleyebilirsiniz.

    ![Azure Portalı'nı tıklatın. Yeni genel uygulama kaydı.](media/how-to-aad/portal-aad-register-new-app-registration-PUB-CLIENT-NAME.png)

## <a name="api-permissions"></a>API izinleri

Benzer şekilde [gizli bir istemci uygulaması](register-confidential-azure-ad-client-app.md), bu uygulamanın hangi API izinleri kullanıcılar adına talep erişebiliyor olmalısınız seçmek ihtiyacınız olacak:

1. Açık **API izinleri** ve seçin, [FHIR API kaynak uygulama kaydı](register-resource-azure-ad-client-app.md):

    ![Azure Portalı'nı tıklatın. Yeni Genel API izinleri.](media/how-to-aad/portal-aad-register-new-app-registration-PUB-CLIENT-API-PERMISSIONS.png)

2. Uygulama isteği yapabilmeyi istediğiniz kapsamları seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir ortak istemci uygulamasının Azure Active Directory'ye kaydetmeniz öğrendiniz. Ardından, azure'da FHIR API dağıtın.
 
>[!div class="nextstepaction"]
>[Açık kaynak FHIR sunucusu dağıtma](fhir-oss-powershell-quickstart.md)
