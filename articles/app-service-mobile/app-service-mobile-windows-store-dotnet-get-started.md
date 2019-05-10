---
title: Azure Mobile Apps kullanan bir evrensel Windows Platformu (UWP) oluşturma | Microsoft Docs
description: C#, Visual Basic ya da JavaScript’te Evrensel Windows Platformu (UWP) uygulaması geliştirme için Azure mobil uygulama arka uçlarını kullanmaya başlamak üzere bu öğreticiyi izleyin.
services: app-service\mobile
documentationcenter: windows
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 47124296-2908-4d92-85e0-05c4aa6db916
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: crdun
ms.openlocfilehash: 959c1ff8b199320105f650a7eb62a04bedb03b3b
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65412778"
---
# <a name="create-a-windows-app-with-an-azure-backend"></a>Bir Azure arka uç ile bir Windows uygulaması oluşturma

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir evrensel Windows Platformu’na (UWP) bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir. Daha fazla bilgi için bkz. [Mobile Apps nedir?](app-service-mobile-value-prop.md). Aşağıdaki ekran görüntüleri tamamlanan şu uygulamalara aittir:

![Tamamlanmış masaüstü uygulaması](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)

Bu öğreticiyi tamamlamak UWP uygulamalarına ilişkin tüm Mobil Uygulama öğreticileri için ön koşuldur.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Bir hesabınız yoksa, Azure deneme sürümünü kaydolabilir ve deneme süresi bittikten sonra dahi kullanmaya devam edebileceğiniz 10 ücretsiz mobil uygulama edinebilirsiniz. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Windows 10.
* [Visual Studio Community].
* UWP uygulama geliştirme ile aşinalık. Ziyaret [UWP belgeleri](https://docs.microsoft.com/windows/uwp/) bilgi edinmek için nasıl [ayarlanmasını](https://docs.microsoft.com/windows/uwp/get-started/get-set-up) UWP uygulamaları oluşturmak için.

## <a name="create-a-new-azure-mobile-app-backend"></a>Yeni bir Azure Mobil Uygulama arka ucu oluşturma

Yeni Mobil Uygulama arka ucu oluşturmak için bu adımları izleyin.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Artık, mobil istemci uygulamalarınız tarafından kullanılabilecek bir Azure Mobil Uygulama arka ucu sağladınız. Sonra, basit bir "yapılacaklar listesi" arka ucu için bir sunucu projesi indirecek ve Azure’a yayımlayacaksınız.

## <a name="configure-the-server-project"></a>Sunucu projesi yapılandırma

[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-client-project"></a>İstemci projesi indirme ve çalıştırma

Mobil Uygulama arka ucunuzu yapılandırdıktan sonra, Azure’a bağlanmak için yeni bir istemci uygulaması oluşturabilir veya mevcut bir uygulamayı değiştirebilirsiniz. Bu bölümde, Mobil Uygulama arka ucunuza bağlanmak için özelleştirilmiş bir UWP örnek uygulaması projesi indirirsiniz.

1. Mobil Uygulama arka ucunuzun **Hızlı Başlangıç** dikey penceresine dönerek, **Yeni uygulama oluştur** > **İndir**’e tıklayın ve ardından sıkıştırılmış proje dosyalarını yerel bilgisayarınıza çıkarın.

    ![Windows hızlı başlangıç projesi indirme](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)

2. UWP projesini açın, uygulamayı dağıtmak ve çalıştırmak için F5 tuşuna basın.
3. Uygulamada **TodoItem Ekle** metin kutusuna *Öğreticiyi tamamla* gibi anlamlı bir metin yazın ve ardından**Kaydet**’e tıklayın.

    ![Windows hızlı başlangıç tamamlanmış masaüstü](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Bu, Azure üzerinde barındırılan yeni mobil uygulama arka ucuna bir POST isteği gönderir.

> [!TIP]
> .NET arka ucu kullanıyorsanız UWP uygulaması projesini sunucu projesiyle aynı çözüme ekleyebilirsiniz. Bu, aynı Visual Studio çözümündeki uygulama ve arka uç için hata ayıklama ve test etmeyi daha kolay hale getirir. Arka uç çözüme UWP uygulaması projesi eklemek için Visual Studio 2017 veya sonraki bir sürümü kullanılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [Uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.
* [Uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Uygulamanıza anında iletme bildirimleri desteği eklemeyi ve anında iletme bildirimleri göndermek için Azure Notification Hubs’ı kullanmak üzere Mobile App arka ucunuzu yapılandırmayı öğrenin.
* [Uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Mobil Uygulama arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı eşitleme son kullanıcıların, ağ bağlantısı yokken dahi, mobil uygulama ile etkileşim kurmalarına &mdash;veri görüntüleme, ekleme ya da değiştirme&mdash; olanak tanır.

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Mobile App SDK]: https://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio Community]: https://go.microsoft.com/fwLink/p/?LinkID=534203
