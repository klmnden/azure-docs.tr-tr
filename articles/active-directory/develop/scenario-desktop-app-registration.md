---
title: Masaüstü uygulaması çağrıları API'ler (uygulama kaydı) - web Microsoft kimlik platformu
description: Bir masaüstü uygulaması oluşturmayı öğrenin çağrıları veritabanını web API'leri (uygulama kaydı)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5da934709274d90668d94dfea3a9c223e191d032
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076068"
---
# <a name="desktop-app-that-calls-web-apis---app-registration"></a>Web API'leri - uygulama kaydı çağrıları masaüstü uygulaması

Bu makale, bir masaüstü uygulaması için uygulama kaydı specificities içerir.

## <a name="supported-accounts-types"></a>Desteklenen hesapları türleri

Bir masaüstü uygulamasında desteklenen hesap türlerini istediğiniz ışık yedekleme deneyimi bağlıdır, ve bu nedenle kullanmak istiyorsanız akışlarda.

### <a name="audience-for-interactive-token-acquisition"></a>Etkileşimli belirteç edinme işlemi için hedef kitle

Masaüstü uygulamanızı etkileşimli kimlik doğrulaması kullanıyorsa, tüm kullanıcıların oturum açabilirsiniz [hesap türü](quickstart-register-app.md#register-a-new-application-using-the-azure-portal)

### <a name="audience-for-desktop-app-silent-flows"></a>Masaüstü uygulaması sessiz akışlar için hedef kitle

- Tümleşik Windows kimlik doğrulaması veya kullanıcı adı/parola kullanmak istiyorsanız, kullanıcıların kendi kiracınızı (iş KOLU geliştiricisi) veya Azure Active directory kuruluşlardaki (ISV senaryosu) oturum açmak uygulamanız gerekir. Bu kimlik doğrulama akışları kişisel Microsoft hesapları için desteklenmez
- Cihaz kod akışını kullanmak istiyorsanız, kullanıcıların kendi kişisel Microsoft hesapları ile henüz oturum açılamıyor
- Kullanıcılar bir B2C yetkilisi hem de ilke sosyal kimliklerle oturum açarsanız, yalnızca etkileşimli ve kullanıcı adı parola kimlik doğrulaması kullanabilirsiniz.

## <a name="redirect-uris"></a>Yeniden Yönlendirme URI'leri

Yeniden yönlendirme URI'leri bir masaüstü uygulamasında kullanmak için kullanmak istediğiniz akışı üzerinde yeniden bağlı olacaktır.

- Etkileşimli kimlik doğrulaması kullanıyorsanız, kullanmak isteyeceksiniz `https://login.microsoftonline.com/common/oauth2/nativeclient`. Karşılık gelen URL'yi tıklayarak bu yapılandırma başarıya ulaşırsınız **kimlik doğrulaması** uygulamanız için bölüm
  
  > [!IMPORTANT]
  > Bugün Windows üzerinde çalışan masaüstü uygulamalarda varsayılan olarak başka bir yeniden yönlendirme URI'si MSAL.NET kullanır (`urn:ietf:wg:oauth:2.0:oob`). Gelecekte bu varsayılanı değiştirmek istiyoruz ve bu nedenle kullanmanızı öneririz `https://login.microsoftonline.com/common/oauth2/nativeclient`

- Tümleşik Windows kimlik doğrulaması, kullanıcı adı/parola veya cihaz kod akışı, uygulamanızı yalnızca kullanıyorsanız, uygulamanız için bir yeniden yönlendirme URI'si kaydetme gerekmez. Aslında bu akışlar Microsoft kimlik platformu v2.0 uç noktasına bir gidiş dönüş yapmak ve uygulamanızın belirli tüm URI üzerinde geri çağrılması olmaz. Bir ayrım yapmak için bunları sahip gizli bir istemci uygulama akışını yeniden yönlendirme URI'leri (arka plan programı uygulamalarında kullanılan ya da istemci kimlik bilgileri akışı), uygulamanızın genel istemci uygulaması olduğunu express gerekir. Bu yapılandırma sayfasına gidip sağlanır **kimlik doğrulaması** bölümü ve uygulamanız için **Gelişmiş ayarlar** ayrıntılarının öğesini **Evet**, sorusuna **Uygulama genel bir istemci kabul** (içinde **varsayılan istemci türü** paragraf)

  ![Genel istemci izin ver](media/scenarios/default-client-type.png)

## <a name="api-permissions"></a>API izinleri

Masaüstü uygulamaları, oturum açmış kullanıcı adına API'lerini çağırma. Temsilci izinleri istemek gerekir. Bunlar uygulama izinleri isteyemez (hangi yalnızca işlenir [arka plan programı uygulamaları](scenario-daemon-overview.md))

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Masaüstü uygulaması - uygulama yapılandırma](scenario-desktop-app-configuration.md)
