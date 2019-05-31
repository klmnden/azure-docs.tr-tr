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
ms.date: 05/06/2019
ms.author: crdun
ms.openlocfilehash: f3e8ca4f9736dffe4928fc8920b0890dff87367b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66236038"
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

* Etkin bir Azure hesabı. Hesabınız yoksa Azure deneme sürümü için kaydolun ve 10 ücretsiz Mobil Uygulama edinin. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Xamarin ile Visual Studio. Yönergeler için bkz. [Visual Studio ve Xamarin için Kurulum ve Yükleme](/visualstudio/cross-platform/setup-and-install).

## <a name="create-an-azure-mobile-app-backend"></a>Azure Mobil Uygulama arka ucu oluşturma
Mobil Uygulama arka ucu oluşturmak için bu adımları izleyin.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Artık, mobil istemci uygulamalarınız tarafından kullanılabilecek bir Azure Mobil Uygulama arka ucu sağladınız. Ardından, basit bir "Yapılacaklar listesi" arka ucu için sunucu projesi indirin ve Azure’a yayımlayın.

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Veritabanı bağlantısı oluşturma ve istemci ve sunucu projesi yapılandırma
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinandroid-app"></a>Xamarin.Android uygulaması çalıştırma
1. Xamarin.Android projesi açın.

2. Git [Azure portalında](https://portal.azure.com/) ve oluşturduğunuz mobil uygulamaya gidin. Üzerinde `Overview` dikey penceresinde mobil uygulamanız için genel bir uç nokta URL'sini arayın. Örnek - sitename my app name "test123" için olacak https://test123.azurewebsites.net.

3. Dosyayı açmak `ToDoActivity.cs` bu klasördeki - xamarin.android/ZUMOAPPNAME/ToDoActivity.cs. Uygulama adı `ZUMOAPPNAME`.

4. İçinde `ToDoActivity` sınıfı, yerine `ZUMOAPPURL` yukarıdaki genel uç noktası ile değişken.

    `const string applicationURL = @"ZUMOAPPURL";`

    olur
    
    `const string applicationURL = @"https://test123.azurewebsites.net";`
    
5. Dağıtabilir ve uygulamayı çalıştırmak için F5 tuşuna basın.

6. Uygulamada, *Öğreticiyi tamamla* gibi anlamlı bir metin yazın ve ardından **Ekle** düğmesine basın.

    ![][10]

    İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil uygulama arka ucu tarafından döndürülür ve veriler listede görüntülenir.

   > [!NOTE]
   > Sorgulamak ve ToDoActivity.cs C# dosyasında bulunan verileri eklemek için, mobil uygulamanızın arka ucuna erişen kodu gözden geçirebilirsiniz.
   
## <a name="troubleshooting"></a>Sorun giderme
Çözümü oluşturma konusunda sorun yaşarsanız, NuGet paket yöneticisini çalıştırıp `Xamarin.Android` destek paketlerini güncelleştirin. Hızlı Başlangıç projeleri her zaman son sürümleri içermeyebilir.

Projenizde başvurulan tüm destek paketlerinin aynı sürüme sahip olması gerektiğini unutmayın. [Azure Mobile Apps NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/), Android platformu için `Xamarin.Android.Support.CustomTabs` bağımlılığına sahiptir, yani projeniz daha yeni destek paketleri kullanıyorsa, çakışmaları önlemek için doğrudan gerekli sürüme sahip bu paketi yüklemeniz gerekir.

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png
<!-- URLs. -->
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203
