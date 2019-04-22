---
title: Xamarin.Android uygulamaları için Azure Mobile Apps kullanmaya başlama
description: Xamarin Android geliştirme için Azure Mobile Apps’i kullanmaya başlamak için bu öğreticiden yararlanın
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 81649dd3-544f-40ff-b9b7-60c66d683e60
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: crdun
ms.openlocfilehash: 29efa963a254913e3d4744ade1d161c5c8ce42e4
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58893359"
---
# <a name="create-a-xamarinandroid-app"></a>Xamarin.Android Uygulaması oluşturma
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, bir Xamarin.Android uygulamasına bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilmiştir. Daha fazla bilgi için bkz. [Mobile Apps nedir?](app-service-mobile-value-prop.md).

Aşağıda tamamlanmış uygulamadan bir ekran görüntüsünü görebilirsiniz:

![][0]

Bu öğreticiyi tamamlamak Xamarin Android uygulamalarına ilişkin tüm Mobile Apps öğreticileri için ön koşuldur.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* Etkin bir Azure hesabı. Hesabınız yoksa Azure deneme sürümü için kaydolun ve 10 ücretsiz Mobil Uygulama edinin. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Xamarin ile Visual Studio. Yönergeler için bkz. [Visual Studio ve Xamarin için Kurulum ve Yükleme](/visualstudio/cross-platform/setup-and-install).

## <a name="create-an-azure-mobile-app-backend"></a>Azure Mobil Uygulama arka ucu oluşturma
Mobil Uygulama arka ucu oluşturmak için bu adımları izleyin.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Artık, mobil istemci uygulamalarınız tarafından kullanılabilecek bir Azure Mobil Uygulama arka ucu sağladınız. Ardından, basit bir "Yapılacaklar listesi" arka ucu için sunucu projesi indirin ve Azure’a yayımlayın.

## <a name="configure-the-server-project"></a>Sunucu projesi yapılandırma
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>Xamarin Android uygulamasını indirme ve çalıştırma
1. Altında **Xamarin.Android projenizi indirme ve çalıştırma** altında **İndir**’e tıklayın.

      Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.
2. Projeyi oluşturmak ve uygulamayı başlatmak için **F5** tuşuna basın.
3. Uygulamada, *Öğreticiyi tamamla* gibi anlamlı bir metin yazın ve ardından **Ekle** düğmesine basın.

    ![][10]

    İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil uygulama arka ucu tarafından döndürülür ve veriler listede görüntülenir.

   > [!NOTE]
   > Sorgulamak ve ToDoActivity.cs C# dosyasında bulunan verileri eklemek için, mobil uygulamanızın arka ucuna erişen kodu gözden geçirebilirsiniz.
   >
   >

## <a name="troubleshooting"></a>Sorun giderme
Çözümü oluşturma konusunda sorun yaşarsanız, NuGet paket yöneticisini çalıştırıp `Xamarin.Android` destek paketlerini güncelleştirin. Hızlı Başlangıç projeleri her zaman son sürümleri içermeyebilir.

Projenizde başvurulan tüm destek paketlerinin aynı sürüme sahip olması gerektiğini unutmayın. [Azure Mobile Apps NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/), Android platformu için `Xamarin.Android.Support.CustomTabs` bağımlılığına sahiptir, yani projeniz daha yeni destek paketleri kullanıyorsa, çakışmaları önlemek için doğrudan gerekli sürüme sahip bu paketi yüklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* [Uygulamanıza Çevrimdışı Eşitleme ekleme](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-xamarin-android-get-started-users.md)
* [Xamarin.Android uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-xamarin-android-get-started-push.md)
* [Azure Mobile Apps için yönetilen istemciyi kullanma](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
