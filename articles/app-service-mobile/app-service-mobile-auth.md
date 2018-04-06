---
title: Kimlik doğrulama ve yetkilendirme mobil uygulamaları için Azure App Service | Microsoft Docs
description: Kavramsal başvurusu ve kimlik doğrulamasına genel bakış / yetkilendirme özelliğini Azure App Service için özellikle mobil uygulamalar için
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
ms.openlocfilehash: 237310c607eb8488e53631b6e69d01703d1ebf99
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="authentication-and-authorization-in-azure-app-service-for-mobile-apps"></a>Kimlik doğrulama ve yetkilendirme mobil uygulamaları için Azure uygulama hizmeti

Bu makalede nasıl kimlik doğrulaması ve yetkilendirme çalıştığı bir uygulama hizmeti arka uç ile yerel mobil uygulamaları geliştirirken açıklanmaktadır. Mobil uygulamalarınız kullanıcılar App Service'te herhangi kodunu değiştirmeden oturum açabilmeniz için tümleşik kimlik doğrulaması ve yetkilendirme, uygulama hizmeti sağlar. Uygulamanızı korumak ve kullanıcı başına verilerle çalışmak için kolay bir yol sağlar. 

Bu makalede, mobil uygulama geliştirme hakkında odaklanır. Uygulama hizmeti kimlik doğrulama ve yetkilendirme mobil uygulamanız için hızlı bir şekilde başlamak için şu eğitimlerden birine bakın [iOS uygulamanıza kimlik doğrulaması ekleme] [ iOS] (veya [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms], veya [Cordova ]). 

Kimlik doğrulama ve yetkilendirme App Service içinde nasıl çalıştığı hakkında daha fazla bilgi için bkz: [kimlik doğrulama ve yetkilendirme Azure App Service'te](../app-service/app-service-authentication-overview.md).

## <a name="authentication-with-provider-sdk"></a>Kimlik doğrulama sağlayıcısı SDK ile

Her şeyi App Service'te yapılandırıldıktan sonra uygulama hizmeti ile imzalamak için mobil istemcilerin değiştirebilirsiniz. Burada iki yaklaşım vardır:

* Belirtilen kimlik sağlayıcısı kimliğini oluşturmak ve uygulama hizmeti erişim kazanmak için yayımlayan bir SDK'yı kullanın.
* Tek satırlık bir kod kullanabilir, böylece Mobile Apps istemci SDK'sı kullanıcılar oturum açabilir.

> [!TIP]
> Çoğu uygulama belirteci yenileme desteği kullanmak üzere ve sağlayıcıyı belirtir diğer avantajlarından yararlanabilmek için kullanıcıların oturum açtığınızda daha tutarlı bir deneyim almak için bir sağlayıcı SDK kullanmanız gerekir.
> 
> 

SDK sağlayıcı kullandığınızda, kullanıcılar uygulamayı çalıştıran işletim sistemine sahip daha sıkı bir şekilde tümleşen bir deneyim için oturum açabilir. Bu yöntem aynı zamanda, sağlayıcı belirteci ve grafik API'leri kullanabilir ve kullanıcı deneyimini özelleştirmek çok daha kolay hale getirir istemcide bazı kullanıcı bilgileri sunar. Kullanıcıların istemci açtığında kodu ve istemci kodu sağlayıcısı belirteç erişimi olduğundan bazen bloglar ve forumlar, onu için "istemci akışı" veya "istemci yönlendirilmiş akış" adlandırılır.

Sağlayıcı belirteci alındıktan sonra uygulama hizmeti için doğrulama gönderilmesi gerekiyor. Uygulama hizmeti belirteci doğruladıktan sonra uygulama hizmeti istemciye döndürülen yeni bir uygulama hizmeti belirteci oluşturur. Mobile Apps istemci SDK'sı, bu exchange yönetmek ve otomatik olarak uygulama arka ucunun yapılan tüm isteklere belirteci eklemek için yardımcı yöntemler vardır. Geliştiriciler ayrıca sağlayıcı belirteci başvuru kullanmaya devam edebilir.

Kimlik doğrulaması akışı hakkında daha fazla bilgi için bkz: [App Service kimlik doğrulama akışı](../app-service/app-service-authentication-overview.md#authentication-flow). 

## <a name="authentication-without-provider-sdk"></a>Sağlayıcı SDK olmadan kimlik doğrulaması

Sağlayıcısını kurma SDK istemiyorsanız, oturum açmak için Azure App Service Mobile Apps özelliğini izin verebilirsiniz. Mobile Apps istemci SDK seçtiğiniz sağlayıcı web görünümüne açın ve kullanıcı oturum açabilir. Bazen bloglar ve forumlar, bunu "sunucu akış" veya "sunucu yönlendirilmiş akış" sunucusu kullanıcılar oturum açtığında sürecini yönetir ve istemci SDK sağlayıcısı belirtecin hiçbir zaman alır çünkü çağrılır.

Bu akış başlatmak için kod her platform için kimlik doğrulama öğreticideki dahil edilir. Akış sonunda SDK istemcisi bir uygulama hizmeti belirtecine sahip ve belirteç uygulama arka ucu için tüm istekleri için otomatik olarak eklenir.

Kimlik doğrulaması akışı hakkında daha fazla bilgi için bkz: [App Service kimlik doğrulama akışı](../app-service/app-service-authentication-overview.md#authentication-flow). 
## <a name="more-resources"></a>Diğer kaynaklar

Nasıl kimlik doğrulaması kullanarak mobil istemcilerinize eklemek aşağıdaki öğreticileri Göster [sunucu yönlendirilmiş akış](../app-service/app-service-authentication-overview.md#authentication-flow):

* [İOS uygulamanıza kimlik doğrulaması ekleme][iOS]
* [Android uygulamanıza kimlik doğrulaması ekleme][Android]
* [Windows kimlik doğrulaması ekleme][Windows]
* [Xamarin.iOS uygulamanıza kimlik doğrulaması ekleme][Xamarin.iOS]
* [Xamarin.Android uygulamanıza kimlik doğrulaması ekleme][Xamarin.Android]
* [Xamarin.Forms uygulamanıza kimlik doğrulaması ekleme][Xamarin.Forms]
* [Cordova uygulamanıza kimlik doğrulaması ekleme][Cordova ]

Kullanmak istiyorsanız, aşağıdaki kaynakları kullanın [istemci yönlendirilmiş akış](../app-service/app-service-authentication-overview.md#authentication-flow) Azure Active Directory için:

* [İOS için Active Directory kimlik doğrulama kitaplığı kullanma][ADAL-iOS]
* [Android için Active Directory kimlik doğrulama kitaplığı kullanma][ADAL-Android]
* [Windows ve Xamarin için Active Directory Authentication Library kullanın][ADAL-dotnet]

Kullanmak istiyorsanız, aşağıdaki kaynakları kullanın [istemci yönlendirilmiş akış](../app-service/app-service-authentication-overview.md#authentication-flow) Facebook için:

* [İOS için Facebook SDK'yı kullanma](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Kullanmak istiyorsanız, aşağıdaki kaynakları kullanın [istemci yönlendirilmiş akış](../app-service/app-service-authentication-overview.md#authentication-flow) Twitter için:

* [İOS için twitter doku kullanma](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Kullanmak istiyorsanız, aşağıdaki kaynakları kullanın [istemci yönlendirilmiş akış](../app-service/app-service-authentication-overview.md#authentication-flow) Google için:

* [İOS için oturum açma Google SDK'yı kullanma](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova ]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: app-service-mobile-how-to-configure-google-authentication.md
[MSA]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
