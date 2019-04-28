---
title: Xamarin iOS uygulamaları için Azure Uygulama Hizmeti Mobile Apps’i Kullanmaya Başlama | Microsoft Belgeleri
description: Xamarin.iOS geliştirme için Mobile Apps kullanmaya başlamak için bu öğreticiyi izleyin.
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 10/01/2016
ms.author: crdun
ms.openlocfilehash: 03fb286bd24bb12f3a1e508627a2de156e185568
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097497"
---
# <a name="create-a-xamarinios-app"></a>Yeni bir Xamarin.iOS uygulaması oluşturma
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, bir Xamarin.iOS mobil uygulamasına Azure mobil uygulaması arka ucunu kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir.  Hem yeni bir mobil arka uç hem de Azure’da uygulama verilerini depolayan basit bir *Yapılacaklar listesi* oluşturursunuz.

Bu öğreticiyi tamamlamak, Azure App Service’de Mobile Apps özelliğini kullanmayla ilgili diğer tüm Xamarin.iOS öğreticileri için ön koşuldur.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* Etkin bir Azure hesabı. Hesabınız yoksa Azure deneme sürümü için kaydolun ve deneme süreniz bittikten sonra bile kullanmaya devam edebileceğiniz 10 ücretsiz mobil uygulama edinin. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Xamarin ile Visual Studio. Yönergeler için bkz. [Visual Studio ve Xamarin için Kurulum ve Yükleme](/visualstudio/cross-platform/setup-and-install).
* Xcode v7.0 veya daha sonraki sürümü ve Xamarin Studio Community yüklü bir Mac. Bkz. [Visual Studio ve Xamarin için kurulum ve yükleme](/visualstudio/cross-platform/setup-and-install) ve [Mac kullanıcıları için kurulum, yükleme ve doğrulamalar](/visualstudio/cross-platform/setup-install-and-verifications-for-mac-users) (MSDN).

## <a name="create-an-azure-mobile-app-backend"></a>Azure Mobil Uygulama arka ucu oluşturma
Mobil Uygulama arka ucu oluşturmak için bu adımları izleyin.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Sunucu projesi yapılandırma
Artık, mobil istemci uygulamalarınız tarafından kullanılabilecek bir Azure Mobil Uygulama arka ucu sağladınız. Ardından, basit bir "Yapılacaklar listesi" arka ucu için sunucu projesi indirin ve Azure’a yayımlayın.

Sunucu projesini Node.js veya .NET arka ucunu kullanacak şekilde yapılandırmak için bu adımları izleyin.

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a>Xamarin iOS uygulamasını indirme ve çalıştırma
1. Bir tarayıcı penceresine [Azure portal]’ı açın.
2. Mobil Uygulamanızın dikey penceresinde, **Kullanmaya Başlama** > **Xamarin.iOS**.’a tıklayın. 3. adımın altında, henüz seçili değilse **Yeni uygulama oluştur**’a tıklayın.  Sonra **İndir** düğmesine tıklayın.

      Mobil arka ucunuza bağlanan istemci uygulaması indirilir. Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.
3. İndirdiğiniz projeyi çıkarın ve Xamarin Studio veya Visual Studio'da açın.

    ![][9]

    ![][8]
4. F5 tuşuna basarak projeyi oluşturun ve uygulamayı iPhone öykünücüsünde başlatın.
5. Uygulamada, *Xamarin öğren* gibi anlamlı bir metin yazın ve ardından **+** düğmesine tıklayın.

    ![][10]

    İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil uygulama arka ucu tarafından döndürülür ve veriler listede görüntülenir.

> [!NOTE]
> Sorgulamak ve QSTodoService.cs C# dosyasına veri eklemek için, mobil uygulamanızın arka ucuna erişen kodu gözden geçirebilirsiniz.
>
>

## <a name="next-steps"></a>Sonraki adımlar
* [Uygulamanıza Çevrimdışı Eşitleme ekleme](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-xamarin-ios-get-started-users.md)
* [Xamarin.Android uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-xamarin-ios-get-started-push.md)
* [Azure Mobile Apps için yönetilen istemciyi kullanma](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Getting started with mobile app backends]:#getting-started
[Create a new mobile app backend]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure portal]: https://portal.azure.com/
