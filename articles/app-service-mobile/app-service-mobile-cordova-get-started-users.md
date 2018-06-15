---
title: Apache Cordova Mobile Apps ile kimlik doğrulaması ekleyin | Microsoft Docs
description: Apache Cordova uygulamanızı kimlik sağlayıcıları, Google, Facebook, Twitter ve Microsoft dahil olmak üzere çeşitli kullanıcıların kimliklerini doğrulamak için Azure App Service'de Mobile Apps kullanmayı öğrenin.
services: app-service\mobile
documentationcenter: javascript
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 10dd6dc9-ddf5-423d-8205-00ad74929f0d
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: b5cce832ae7ae83552c2a5ded2f5f5bda0ac76bf
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
ms.locfileid: "27591955"
---
# <a name="add-authentication-to-your-apache-cordova-app"></a>Apache Cordova uygulamanıza kimlik doğrulaması ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Özet
Bu öğretici kapsamında, kimlik doğrulama desteklenen kimlik sağlayıcısı kullanarak Apache Cordova todolist hızlı başlangıç projeye ekleyin. Bu öğretici dayanır [Mobile Apps'i kullanmaya başlamak] önce tamamlamanız gereken öğretici.

## <a name="register"></a>Kimlik doğrulaması için uygulamanızı kaydetme ve uygulama hizmetini yapılandırma
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Benzer adımları gösteren bir video izleyin](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

## <a name="permissions"></a>Kimliği doğrulanmış kullanıcılar için izinleri kısıtla
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Şimdi, arka adsız erişim devre dışı olduğunu doğrulayabilirsiniz. Visual Studio'da:

* Öğretici tamamlandığında oluşturduğunuz projeyi açın [Mobile Apps'i kullanmaya başlamak].
* Uygulamanızı çalıştırın **Google Android öykünücüsü**.
* Uygulama başlatıldıktan sonra beklenmeyen bir bağlantı hatası göründüğünü doğrulayın.

Ardından, mobil uygulama arka ucundan kaynakları istemeden önce kullanıcıların kimliklerini doğrulamak için uygulamayı güncelleştirme.

## <a name="add-authentication"></a>Kimlik doğrulaması için uygulama ekleme
1. Projenizde açmak **Visual Studio**, ardından açık `www/index.html` dosyayı düzenlemek için.
2. Bulun `Content-Security-Policy` baş bölümünde meta etiketi.  İzin verilen kaynaklar listesine OAuth konak ekleyin.

   | Sağlayıcı | SDK sağlayıcı adı | OAuth ana bilgisayar |
   |:--- |:--- |:--- |
   | Azure Active Directory | aad | https://login.microsoftonline.com |
   | Facebook | Facebook | https://www.facebook.com |
   | Google | Google | https://Accounts.Google.com |
   | Microsoft | MicrosoftAccount | https://Login.live.com |
   | Twitter | Twitter | https://api.twitter.com |

    İçerik güvenlik (Azure Active Directory için uygulanan) ilkeye örneği aşağıdaki gibidir:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.microsoftonline.com https://yourapp.azurewebsites.net; style-src 'self'">

    Değiştir `https://login.microsoftonline.com` önceki tabloda OAuth host ile.  İçerik güvenlik ilkesi meta etiketi hakkında daha fazla bilgi için bkz: [içerik Güvenlik İlkesi belgeleri].

    Bazı kimlik doğrulama sağlayıcıları uygun mobil cihazlarda kullanıldığında içerik güvenlik ilkesi değişikliklerini gerektirmez.  Örneğin, bir Android cihazında Google kimlik doğrulaması kullanırken, içerik güvenlik ilkesi değişiklik gerekmez.

3. Açık `www/js/index.js` dosya düzenlemek için bulun `onDeviceReady()` yöntemi, ve istemci oluşturmanın altında kod aşağıdaki kodu ekleyin:

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Bu kod, tablo başvurusu oluşturur ve kullanıcı arabirimini yeniler var olan kodu değiştirir.

    Tanımlar: login() yöntemi kimlik doğrulama sağlayıcı ile başlatır. JavaScript Promise döndüren bir zaman uyumsuz işlev tanımlar: login() yöntemidir.  Böylece tanımlar: login() yöntemi tamamlanana kadar bu yürütülemiyor başlatma kalan içinde promise yanıt yerleştirilir.

4. Yeni eklediğiniz kodla, `SDK_Provider_Name` , oturum açma sağlayıcısı adı. Örneğin, Azure Active Directory kullanmak `client.login('aad')`.
5. Projenizi çalıştırma.  Proje başlatma tamamlandığında, uygulamanız için seçilen kimlik doğrulama sağlayıcısı OAuth oturum açma sayfasına gösterir.

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [kimlik doğrulama hakkında] Azure uygulama hizmeti ile.
* Öğretici ekleyerek devam [anında iletme bildirimleri] Apache Cordova uygulamanıza.

SDK'ları kullanmayı öğrenin.

* [Apache Cordova SDK]
* [ASP.NET Sunucusu SDK]
* [Node.js Sunucusu SDK]

<!-- URLs. -->
[Mobile Apps'i kullanmaya başlamak]: app-service-mobile-cordova-get-started.md
[içerik Güvenlik İlkesi belgeleri]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[anında iletme bildirimleri]: app-service-mobile-cordova-get-started-push.md
[kimlik doğrulama hakkında]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Sunucusu SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Sunucusu SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
