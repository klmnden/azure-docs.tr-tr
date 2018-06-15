---
title: Belirteç tabanlı (HTTP/2) kimlik doğrulaması için Azure Notification Hubs APNS | Microsoft Docs
description: Bu konuda, yeni belirteç kimlik doğrulama APNS için yararlanacağınızı açıklanmaktadır
services: notification-hubs
documentationcenter: .net
author: dimazaid
manager: kpiteira
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: ca86130e9c184576fc44119190d6224a363c6561
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33777901"
---
# <a name="token-based-http2-authentication-for-apns"></a>APNS için belirteç tabanlı (HTTP/2) kimlik doğrulaması
## <a name="overview"></a>Genel Bakış
Bu makalede yeni APNS HTTP/2 protokolü belirteç tabanlı kimlik doğrulaması ile kullanmak nasıl ayrıntılarını verir.

Yeni protokolünü kullanmanın avantajları şunlardır:
-   Belirteci oluşturma mücadele boş (sertifikaları karşılaştırıldığında) görece olduğu
-   Daha fazla hiçbir bitiş tarihlerini – kimlik doğrulama belirteçleri ve bunların iptal denetiminde olan
-   Yükü artık en fazla 4 KB olabilir
- Eşzamanlı geri bildirim
-   Apple'nın en son protokolünü dileriz – sertifikaları kullanmaya devam kullanımdan kaldırma için işaretli ikili Protokolü

Bu yeni mekanizmayı kullanarak iki adımda birkaç dakika içinde yapılabilir:
1.  Apple Geliştirici hesap portalından gerekli bilgileri edinmenize
2.  Yeni bilgilerle bildirim hub'ınızı yapılandırma

Bildirim hub'ları olan şimdi APNS ile yeni kimlik doğrulama sistemi kullanmak için tüm kümesi. 

APNS için sertifika kimlik bilgilerini kullanarak geçirdiyseniz dikkat edin:
- belirteç özellikleri sertifikanızı sistemimizde üzerine yazma
- ancak sorunsuz bildirimleri almak uygulamanızı devam eder.

## <a name="obtaining-authentication-information-from-apple"></a>Apple'dan kimlik doğrulama bilgilerini alma
Belirteç tabanlı kimlik doğrulamasını etkinleştirmek için aşağıdaki özellikleri Apple Developer hesabınızdan gerekir:
### <a name="key-identifier"></a>Anahtar Tanımlayıcısı
Apple Geliştirici hesabınızda "Anahtarlar" sayfasından anahtarı tanımlayıcısı alınabilir

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier--application-name"></a>Uygulama tanımlayıcısı & Uygulama adı
Uygulama adı, geliştirici hesabını uygulama kimlikleri sayfasında aracılığıyla kullanılabilir. 
![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

Uygulama tanımlayıcısı, geliştirici hesabını üyelik Ayrıntıları sayfasında aracılığıyla kullanılabilir.
![](./media/notification-hubs-push-notification-http2-token-authentification/app-id.png)


### <a name="authentication-token"></a>Kimlik doğrulama belirteci
Uygulamanız için bir belirteç oluşturmak sonra kimlik doğrulama belirteci indirilebilir. Bu belirteci üretmek nasıl hakkında daha fazla bilgi için başvurmak [Apple Geliştirici belgeleri](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65).

## <a name="configuring-your-notification-hub-to-use-token-based-authentication"></a>Belirteç tabanlı kimlik doğrulaması kullanacak şekilde bildirim hub'ınızı yapılandırma
### <a name="configure-via-the-azure-portal"></a>Azure portalı üzerinden yapılandırma
Portalda belirteç tabanlı kimlik doğrulamasını etkinleştirmek için Azure portalında oturum açın ve bildirim Hub'ına gidin > Bildirim Hizmetleri > APNS paneli. 

Yeni bir özellik – yoktur *kimlik doğrulama modu*. Belirteç seçerek, tüm ilgili belirteci özellikleri ile hub'ınızı güncelleştirmenizi sağlar.

![](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

- Apple Geliştirici hesabınızdan alınan özelliklerini girin 
- uygulama modu (üretim veya korumalı alan) seçin 
- APNS kimlik bilgilerinizi güncelleştirmek için Kaydet'i tıklatın. 

### <a name="configure-via-management-api-rest"></a>Yönetim API'si (REST) yapılandırın

Kullanabileceğiniz bizim [Yönetimi API'leri](https://msdn.microsoft.com/library/azure/dn495827.aspx) belirteç tabanlı kimlik doğrulamasını kullanmak için bildirim hub'ı güncelleştirmek için.
Yapılandırmakta uygulamanın (Apple Geliştirici hesabınızda belirtilen) bir korumalı alan veya üretim uygulaması olup bağlı olarak, karşılık gelen uç noktaları birini kullanın:

- Korumalı alan uç noktası: [https://api.development.push.apple.com:443/3/device](https://api.development.push.apple.com:443/3/device)
- Üretim uç noktası: [https://api.push.apple.com:443/3/device](https://api.push.apple.com:443/3/device)

> [!IMPORTANT]
> Belirteç tabanlı kimlik doğrulaması gerektiren bir API sürümü: **2017-04 ya da daha sonra**.
> 
> 

Belirteç tabanlı kimlik doğrulaması ile bir hub'ı güncelleştirmek için bir PUT İsteği bir örneği burada verilmiştir:


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
        

### <a name="configure-via-the-net-sdk"></a>.NET SDK'sı yapılandırın
Hub'ınızı belirteç tabanlı kimlik doğrulamasını kullanmak için yapılandırabilirsiniz bizim [son istemci SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8). 

Doğru kullanımını gösteren kod örneği şöyledir:


        NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
        string token = "YOUR TOKEN HERE";
        string keyId = "YOUR KEY ID HERE";
        string appName = "YOUR APP NAME HERE";
        string appId = "YOUR APP ID HERE";
        NotificationHubDescription desc = new NotificationHubDescription("PATH TO YOUR HUB");
        desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
        desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
        nm.UpdateNotificationHubAsync(desc);

## <a name="reverting-to-using-certificate-based-authentication"></a>Sertifika tabanlı kimlik doğrulaması kullanmaya geri alma
Herhangi bir zamanda herhangi bir önceki yöntemini kullanarak ve belirteç özellikleri yerine sertifikanın geçirme sertifika tabanlı kimlik doğrulaması kullanmaya geri dönebilirsiniz. Bu eylem, önceden depolanan kimlik bilgileri üzerine yazar.
