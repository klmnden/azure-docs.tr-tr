---
title: Kimlik doğrulama ve yetkilendirme Azure uygulama Hizmeti'ndeki mobil uygulamalar | Microsoft Docs
description: Kavramsal başvurusu ve genel kimlik doğrulama / yetkilendirme özelliğini Azure App Service için mobil uygulamalar için özellikle
services: app-service
documentationcenter: ''
author: mattchenderson
manager: erikre
editor: ''
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 87bdfcc827155e5dd0a02ffb1640bf7e9cd4e479
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60859133"
---
# <a name="authentication-and-authorization-in-azure-app-service-for-mobile-apps"></a>Kimlik doğrulama ve yetkilendirme Azure App service'taki mobile apps

Bu makalede nasıl kimlik doğrulaması ve yetkilendirme çalıştığı bir App Service arka ucu ile yerel mobil uygulamalar geliştirirken açıklanır. App Service ile tümleşik kimlik doğrulaması ve yetkilendirme sağlar. böylece mobil uygulamalarınızı App Service'te herhangi bir kod değişikliği olmadan kullanıcılar oturum açabilir. Uygulamanızı korumak ve kullanıcı başına verilerle çalışmak için kolay bir yol sunar. 

Bu makale, mobil uygulama geliştirme üzerinde odaklanır. App Service kimlik doğrulama ve yetkilendirme mobil uygulamanız için hızla çalışmaya başlamak için aşağıdaki öğreticilerden birine bakın: [iOS uygulamanıza kimlik doğrulaması ekleme] [ iOS] (veya [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms], veya [Cordova]). 

App Service kimlik doğrulama ve yetkilendirmeyi nasıl çalıştığı hakkında daha fazla bilgi için bkz: [kimlik doğrulama ve yetkilendirme Azure App Service'te](../app-service/overview-authentication-authorization.md).

## <a name="authentication-with-provider-sdk"></a>Sağlayıcı SDK ile kimlik doğrulaması

Her şeyi App Service'te yapılandırıldıktan sonra App Service ile oturum açmanız mobil istemciler değiştirebilirsiniz. Burada iki yaklaşım vardır:

* Belirtilen kimlik sağlayıcısı kimliğini oluşturmak ve ardından App Service erişmek için yayımlayan bir SDK'yı kullanın.
* Mobile Apps istemci SDK'sı kullanıcılar oturum açabilir, tek satırlık bir kod kullanın.

> [!TIP]
> Çoğu uygulama belirteci yenileme desteği ve sağlayıcıyı belirtir diğer avantajlarından yararlanabilmek için kullanıcılar oturum açtığında, daha tutarlı bir deneyim almak için bir sağlayıcı SDK'sını kullanmanız gerekir.
> 
> 

SDK sağlayıcı kullandığınızda, kullanıcılar uygulamayı çalıştıran işletim sistemine sahip daha sıkı bir şekilde tümleşen bir deneyim için oturum açabilir. Bu yöntem, sağlayıcı belirteci ve graf API'lerini kullanmak ve kullanıcı deneyimini özelleştirmek çok daha kolay hale getirir istemcide bazı kullanıcı bilgilerini sağlar. İstemci oturum açtığında kullanıcı kodunu ve istemci kodu sağlayıcısı belirtecine erişimi olduğundan bazen bloglar ve Forum, onu için "istemci akışı" veya "istemci yönlendirilmiş flow" olarak adlandırılır.

Bir sağlayıcı belirteç alındıktan sonra doğrulama için App Service'e gönderilmesi gerekir. App Service, App Service belirteci doğruladıktan sonra istemciye döndürülen yeni bir App Service belirteci oluşturur. Mobile Apps istemci SDK'sı, bu exchange yönetmek ve otomatik olarak uygulama arka ucunun yapılan tüm isteklere belirteci eklemek için yardımcı yöntemler vardır. Geliştiriciler ayrıca sağlayıcısı belirtece başvuru tutabilirsiniz.

Kimlik doğrulaması akışı hakkında daha fazla bilgi için bkz. [App Service kimlik doğrulama akışı](../app-service/overview-authentication-authorization.md#authentication-flow). 

## <a name="authentication-without-provider-sdk"></a>Kimlik doğrulama sağlayıcısı SDK olmadan

Bir sağlayıcı SDK ' istemiyorsanız, oturum açmak için Azure App Service Mobile Apps özelliğini izin verebilirsiniz. Mobile Apps istemci SDK'sı sağlayıcısına seçtiğiniz bir web görünümü açılır ve kullanıcısı ile oturum açın. Bazen bloglar ve Forum, "sunucu akışı" veya "sunucu yönlendirilmiş akış" kullanıcılar oturum açtığında işlem sunucusu yönetir ve istemci SDK'sı sağlayıcısı belirtecin hiçbir zaman alır çünkü adlandırılır.

Bu akışı başlatmak için kod her platform için kimlik doğrulama öğreticisini dahildir. Akışın sonunda bir App Service belirteci istemci SDK'sı vardır ve belirteç uygulama arka ucu için tüm istekleri için otomatik olarak eklenir.

Kimlik doğrulaması akışı hakkında daha fazla bilgi için bkz. [App Service kimlik doğrulama akışı](../app-service/overview-authentication-authorization.md#authentication-flow). 
## <a name="more-resources"></a>Diğer kaynaklar

Aşağıdaki öğreticilerde kimlik doğrulaması kullanarak mobil istemcilerinize ekleme Göster [sunucu yönlendirilmiş akış](../app-service/overview-authentication-authorization.md#authentication-flow):

* [İOS uygulamanıza kimlik doğrulaması ekleme][iOS]
* [Android uygulamanıza kimlik doğrulaması ekleme][Android]
* [Windows kimlik doğrulaması ekleme][Windows]
* [Xamarin.iOS uygulamanıza kimlik doğrulaması ekleme][Xamarin.iOS]
* [Xamarin.Android uygulamanıza kimlik doğrulaması ekleme][Xamarin.Android]
* [Xamarin.Forms uygulamanıza kimlik doğrulaması ekleme][Xamarin.Forms]
* [Cordova uygulamanız için kimlik doğrulaması ekleme][Cordova]

Kullanmak istiyorsanız, aşağıdaki kaynakları kullanın [istemci yönlendirilmiş akış](../app-service/overview-authentication-authorization.md#authentication-flow) Azure Active Directory için:

* [İOS için Active Directory Authentication Library kullanın][ADAL-iOS]
* [Android için Active Directory kimlik doğrulama kitaplığını kullanma][ADAL-Android]
* [Windows ve Xamarin için Active Directory kimlik doğrulama kitaplığını kullanma][ADAL-dotnet]

Kullanmak istiyorsanız, aşağıdaki kaynakları kullanın [istemci yönlendirilmiş akış](../app-service/overview-authentication-authorization.md#authentication-flow) Facebook için:

* [İOS için Facebook SDK'sını kullanma](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Kullanmak istiyorsanız, aşağıdaki kaynakları kullanın [istemci yönlendirilmiş akış](../app-service/overview-authentication-authorization.md#authentication-flow) Twitter için:

* [İOS için twitter Fabric kullanma](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Kullanmak istiyorsanız, aşağıdaki kaynakları kullanın [istemci yönlendirilmiş akış](../app-service/overview-authentication-authorization.md#authentication-flow) Google için:

* [İOS için oturum açma Google SDK'sını kullanma](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service/configure-authentication-provider-aad.md
[Facebook]: ../app-service/configure-authentication-provider-facebook.md
[Google]: configure-authentication-provider-google.md
[MSA]: ../app-service/configure-authentication-provider-microsoft.md
[Twitter]: ../app-service/configure-authentication-provider-twitter.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
