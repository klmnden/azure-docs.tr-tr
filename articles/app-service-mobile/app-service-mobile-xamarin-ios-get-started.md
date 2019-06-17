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
ms.date: 05/06/2019
ms.author: crdun
ms.openlocfilehash: 559050cbc575fce5bdb5b32ec266e1cc3d09b2d5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66242718"
---
# <a name="create-a-xamarinios-app"></a>Yeni bir Xamarin.iOS uygulaması oluşturma
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, bir Xamarin.iOS mobil uygulamasına Azure mobil uygulaması arka ucunu kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir.  Hem yeni bir mobil arka uç hem de Azure’da uygulama verilerini depolayan basit bir *Yapılacaklar listesi* oluşturursunuz.

Bu öğreticiyi tamamlamak, Azure App Service’de Mobile Apps özelliğini kullanmayla ilgili diğer tüm Xamarin.iOS öğreticileri için ön koşuldur.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

* Etkin bir Azure hesabı. Hesabınız yoksa Azure deneme sürümü için kaydolun ve deneme süreniz bittikten sonra bile kullanmaya devam edebileceğiniz 10 ücretsiz mobil uygulama edinin. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Mac için Visual Studio Bkz: [Kurulum ve Mac için Visual Studio için yükleme](https://docs.microsoft.com/visualstudio/mac/installation?view=vsmac-2019)
* Mac'te Xcode 9.0 veya sonraki bir sürümü.
  
## <a name="create-an-azure-mobile-app-backend"></a>Azure Mobil Uygulama arka ucu oluşturma
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="create-a-database-connection-and-configure-the-client-and-server-project"></a>Veritabanı bağlantısı oluşturma ve istemci ve sunucu projesi yapılandırma
[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="run-the-xamarinios-app"></a>Xamarin iOS uygulamasını çalıştırma
1. Xamarin.iOS projesi açın.

2. Git [Azure portalında](https://portal.azure.com/) ve oluşturduğunuz mobil uygulamaya gidin. Üzerinde `Overview` dikey penceresinde mobil uygulamanız için genel bir uç nokta URL'sini arayın. Örnek - sitename my app name "test123" için olacak https://test123.azurewebsites.net.

3. Dosyayı açmak `QSTodoService.cs` bu klasördeki - xamarin.iOS/ZUMOAPPNAME. Uygulama adı `ZUMOAPPNAME`.

4. İçinde `QSTodoService` sınıfı, yerine `ZUMOAPPURL` yukarıdaki genel uç noktası ile değişken.

    `const string applicationURL = @"ZUMOAPPURL";`

    olur
    
    `const string applicationURL = @"https://test123.azurewebsites.net";`
    
5. Dağıtmak ve bir iPhone öykünücüsünde uygulamayı çalıştırmak için F5 tuşuna basın.

6. Uygulamada, gibi anlamlı bir metin yazın *öğreticiye* ve ardından + düğmesini.

    ![][10]

    İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil uygulama arka ucu tarafından döndürülür ve veriler listede görüntülenir.

   > [!NOTE]
   > Sorgulamak ve ToDoActivity.cs C# dosyasında bulunan verileri eklemek için, mobil uygulamanızın arka ucuna erişen kodu gözden geçirebilirsiniz.
   
<!-- Images. -->
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png