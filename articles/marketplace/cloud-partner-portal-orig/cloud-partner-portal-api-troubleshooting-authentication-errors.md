---
title: Genel kimlik doğrulama hataları sorunlarını giderme | Azure Market
description: Bulut iş ortağı portalı API'lerini kullanarak genel kimlik doğrulaması hatalarla ilgili Yardım sağlar.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: ddf3c9ce26a1538d91f1e6d6bcc04fd0d18e7936
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935812"
---
# <a name="troubleshooting-common-authentication-errors"></a>Genel kimlik doğrulama hataları sorunlarını giderme

Bu makalede, bulut iş ortağı portalı API'lerini kullanırken yaygın kimlik doğrulama hataları ile ilgili Yardım sağlar.

## <a name="unauthorized-error"></a>Yetkisiz hatası

Tutarlı bir şekilde alırsanız `401 unauthorized` hataları, sahip olduğunuz geçerli bir erişim belirteciyle doğrulayın.  Zaten yapmadıysanız, temel bir Azure Active Directory (Azure AD) uygulama ve hizmet sorumlusu açıklandığı gibi oluşturmak [Azure Active Directorykaynaklaraerişebilenuygulamasıvehizmetsorumlusuoluşturmakiçinportalıkullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal). Ardından, uygulama veya basit bir HTTP POST isteği erişiminizi doğrulamak için kullanın.  Kiracı kimliği, uygulama kimliği, nesne kimliği ve gizli anahtar aşağıdaki görüntüde gösterildiği gibi erişim belirteci almak için dahil edilir:

![401 hatası sorunlarını giderme](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-401-error.jpg)


## <a name="forbidden-error"></a>Yasak hatası

Alırsanız bir `403 forbidden` hata, doğru hizmet sorumlusu yayımcı hesabınızla bulut iş ortağı Portalı'nda eklediğinizden emin olun.
Bağlantısındaki [önkoşulları](./cloud-partner-portal-api-prerequisites.md) portalına hizmet sorumlunuzu eklemek için sayfa.

Ardından doğru hizmet sorumlusu eklenmiş olan tüm bilgileri doğrulayın. Nesne kimliği için Kapat dikkat edin, portalda girdi. Azure Active Directory uygulama kayıt sayfasında iki nesne kimlikleri vardır ve yerel nesne kimliği kullanmalıdır Doğru değeri giderek bulabilirsiniz **uygulama kayıtları** uygulamanız ve uygulama adı altında tıklayarak sayfası **uygulama yerel dizinde yönetilen**. Bu uygulama burada bulabilirsiniz doğru nesne kimliği için yerel özellikleri için gereken **özellikleri** sayfasında, aşağıdaki resimde gösterildiği gibi. Ayrıca, hizmet sorumlusu ekleme ve API çağrınızı doğru yayımcı Kimliğini kullandığınızdan emin olun.

![403 hatası sorunlarını giderme](./media/cloud-partner-portal-api-troubleshooting-authentication-errors/troubleshooting-403-error.jpg)
