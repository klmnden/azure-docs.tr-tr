---
title: Azure Notification hubs'ı, APNS için belirteç tabanlı (HTTP/2) kimlik doğrulaması | Microsoft Docs
description: Bu konuda, yeni belirteç kimlik doğrulaması için APNS yararlanma işlemi açıklanmaktadır
services: notification-hubs
documentationcenter: .net
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 02/13/2019
ms.author: jowargo
ms.openlocfilehash: 890577c013a96fc06acf3b05881649ad8202a083
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60872416"
---
# <a name="token-based-http2-authentication-for-apns"></a>APNS için belirteç tabanlı (HTTP/2) kimlik doğrulaması

## <a name="overview"></a>Genel Bakış

Bu makalede, belirteç tabanlı kimlik doğrulaması ile yeni bir APNS HTTP/2 protokolüne kullanma işlemi açıklanmaktadır.

Yeni protokolünü kullanmanın başlıca yararları şunlardır:

* Belirteç oluşturma uğraşmanıza ücretsiz (sertifikaları karşılaştırıldığında) görece olduğu
* Daha fazla bitiş tarih – kimlik doğrulama belirteçlerinizi ve bunların iptal denetimi sizdedir
* Yükleri artık en fazla 4 KB olabilir
* Eşzamanlı geri bildirim
* Apple'nın en son protokolünü dileriz-sertifikaları kullanmaya devam kullanımdan kaldırma için işaretli ikili Protokolü

Bu yeni mekanizma kullanarak iki adımda birkaç dakika içinde yapılabilir:

1. Apple Geliştirici hesabı Portalı'ndan gerekli bilgileri edinin
2. Yeni bilgilerle bildirim hub'ınızı yapılandırma

Bildirim hub'ları, artık yeni kimlik doğrulama sistemi APNS ile birlikte kullanmak için tüm kümesi.

İçin APNS sertifikası kimlik bilgilerini kullanarak geçirdiyseniz dikkat edin:

* belirteç özelliklerini sertifikanızı sistemimizde üzerine yazma
* Ancak, uygulamanız bildirimlerin sorunsuz bir şekilde devam eder.

## <a name="obtaining-authentication-information-from-apple"></a>Apple'dan kimlik doğrulama bilgilerini alma

Belirteç tabanlı kimlik doğrulamasını etkinleştirmek için aşağıdaki özellikleri Apple Geliştirici hesabınızı gerekir:

### <a name="key-identifier"></a>Anahtar tanımlayıcısı

Anahtar tanımlayıcısı Apple Geliştirici hesabınızın "Anahtarlar" sayfasından edinilebilir.

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier--application-name"></a>Uygulama tanımlayıcısı & Uygulama adı

Uygulama adı uygulama kimlikleri sayfasında Geliştirici hesabı aracılığıyla kullanılabilir.

![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

Uygulama tanımlayıcısı üyelik Ayrıntılar sayfası Geliştirici hesabına aracılığıyla kullanılabilir.

![](./media/notification-hubs-push-notification-http2-token-authentification/app-id.png)

### <a name="authentication-token"></a>Kimlik doğrulama belirteci

Uygulamanız için bir belirteç oluşturduktan sonra kimlik doğrulama belirteci indirilebilir. Bu belirteci oluşturma hakkında daha fazla bilgi için başvurmak [Apple'nın Geliştirici belgeleri](https://help.apple.com/xcode/mac/current/#/devdfd3d04a1).

## <a name="configuring-your-notification-hub-to-use-token-based-authentication"></a>Belirteç tabanlı kimlik doğrulaması kullanmak için bildirim hub'ınızı yapılandırma

### <a name="configure-via-the-azure-portal"></a>Azure portalı üzerinden yapılandırma

Portalda belirteç tabanlı kimlik doğrulamasını etkinleştirmek için Azure Portal'da oturum açın ve bildirim Hub'ınıza gidin > Bildirim Hizmetleri > APNS paneli.

Yeni bir özelliği – *kimlik doğrulama modu*. Belirteç seçerek, tüm ilgili belirteç özellikleri ile hub'ınıza güncelleştirmenizi sağlar.

![](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

* Apple Geliştirici hesabınızdan alınan özelliklerini girin
* Uygulama modunu (üretim veya sanal) seçin
* Tıklayın **Kaydet** APNS kimlik bilgilerinizi güncelleştirmek için düğme

### <a name="configure-via-management-api-rest"></a>Yönetim API'si (REST) aracılığıyla yapılandırma

Kullanabileceğiniz bizim [yönetim API'leri](https://msdn.microsoft.com/library/azure/dn495827.aspx) belirteç tabanlı kimlik doğrulaması kullanmak için bildirim hub'ınızı güncelleştirilecek.
Yapılandırmakta uygulamanın (Apple Geliştirici hesabınızda belirtilen) bir korumalı alan veya üretim uygulamasının olup bağlı olarak, karşılık gelen uç noktalar birini kullanın:

* Korumalı alan uç noktası: [https://api.development.push.apple.com:443/3/device](https://api.development.push.apple.com:443/3/device)
* Üretim uç noktası: [https://api.push.apple.com:443/3/device](https://api.push.apple.com:443/3/device)

> [!IMPORTANT]
> Belirteç tabanlı kimlik doğrulaması, bir API sürümünü gerektirir: **2017-04 veya üzeri**.

Belirteç tabanlı kimlik doğrulaması ile bir hub'ı güncelleştirmek için PUT isteğinin bir örneği aşağıda verilmiştir:

    ```text
    PUT https://{namespace}.servicebus.windows.net/{Notification Hub}?api-version=2017-04
      "Properties": {
        "ApnsCredential": {
          "Properties": {
            "KeyId": "<Your Key Id>",
            "Token": "<Your Authentication Token>",
            "AppName": "<Your Application Name>",
            "AppId": "<Your Application Id>",
            "Endpoint":"<Sandbox/Production Endpoint>"
          }
        }
      }
    ```

### <a name="configure-via-the-net-sdk"></a>.NET SDK'sı yapılandırma

Belirteç tabanlı kimlik doğrulaması kullanmak için hub'ınızı yapılandırabileceğiniz bizim [en son istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8).

Doğru kullanımını gösteren kod örneği aşağıdadır:

```csharp
NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
string token = "YOUR TOKEN HERE";
string keyId = "YOUR KEY ID HERE";
string appName = "YOUR APP NAME HERE";
string appId = "YOUR APP ID HERE";
NotificationHubDescription desc = new NotificationHubDescription("PATH TO YOUR HUB");
desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
nm.UpdateNotificationHubAsync(desc);
```

## <a name="reverting-to-using-certificate-based-authentication"></a>Sertifika tabanlı kimlik doğrulaması kullanmaya geri alma

Önceki herhangi bir yöntemi kullanarak ve belirteç özelliklerini yerine sertifikayı geçirerek sertifika tabanlı kimlik doğrulaması kullanarak dilediğiniz zaman geri dönebilirsiniz. Bu eylem, daha önce depolanan kimlik bilgileri geçersiz kılar.
