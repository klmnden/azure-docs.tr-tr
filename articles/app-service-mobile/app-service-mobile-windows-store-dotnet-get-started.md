---
title: "Mobile Apps’te Evrensel Windows Platformu (UWP) oluşturma | Microsoft Belgeleri"
description: "C#, Visual Basic ya da JavaScript’te Evrensel Windows Platformu (UWP) uygulaması geliştirme için Azure mobil uygulama arka uçlarını kullanmaya başlamak üzere bu öğreticiyi izleyin."
services: app-service\mobile
documentationcenter: windows
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 47124296-2908-4d92-85e0-05c4aa6db916
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: b1a633a86bd1b5997d5cbf66b16ec351f1043901
ms.openlocfilehash: 1a031c4858bcbc75ee807ba520e1b22c89471498
ms.contentlocale: tr-tr
ms.lasthandoff: 02/16/2017

---
# <a name="create-a-windows-app"></a>Windows uygulaması oluşturma
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, bir evrensel Windows Platformu’na (UWP) bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir. Daha fazla bilgi için bkz. [Mobile Apps nedir?](app-service-mobile-value-prop.md). Aşağıdaki ekran görüntüleri tamamlanan şu uygulamalara aittir:

![Tamamlanmış masaüstü uygulaması](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed-desktop.png)   
Masaüstünde çalışma.

![Tamamlanmış. telefon uygulaması](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)  
Telefonda çalışma

Bu öğreticiyi tamamlamak UWP uygulamalarına ilişkin tüm Mobil Uygulama öğreticileri için ön koşuldur.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Bir hesabınız yoksa, Azure deneme sürümünü kaydolabilir ve deneme süresi bittikten sonra dahi kullanmaya devam edebileceğiniz 10 ücretsiz mobil uygulama edinebilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/).
* [Visual Studio Community 2015] ya da daha yeni sürümü.

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak istiyorsanız [App Service’i Deneyin](https://azure.microsoft.com/try/app-service/mobile/)’e gidin. Burada, App Services’de hemen bir kısa süreli başlangıç mobil uygulaması oluşturabilirsiniz; kredi kartı gerekmez ve hiçbir taahhüt yoktur.
>
>

## <a name="create-a-new-azure-mobile-app-backend"></a>Yeni bir Azure Mobil Uygulama arka ucu oluşturma
Yeni Mobil Uygulama arka ucu oluşturmak için bu adımları izleyin.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Artık, mobil istemci uygulamalarınız tarafından kullanılabilecek bir Azure Mobil Uygulama arka ucu sağladınız. Sonra, basit bir "yapılacaklar listesi" arka ucu için bir sunucu projesi indirecek ve Azure’a yayımlayacaksınız.

## <a name="configure-the-server-project"></a>Sunucu projesi yapılandırma
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-client-project"></a>İstemci projesi indirme ve çalıştırma
Mobil Uygulama arka ucunuzu yapılandırdıktan sonra, Azure’a bağlanmak için yeni bir istemci uygulaması oluşturabilir veya mevcut bir uygulamayı değiştirebilirsiniz. Bu bölümde, Mobil Uygulama arka ucunuza bağlanmak için özelleştirilmiş bir UWP uygulaması şablonu projesi indirirsiniz.

1. Mobil Uygulama arka ucunuzun **Hızlı Başlangıç** dikey penceresine dönerek, **Yeni uygulama oluştur** > **İndir**’e tıklayın ve ardından sıkıştırılmış proje dosyalarını yerel bilgisayarınıza çıkarın.

    ![Windows hızlı başlangıç projesi indirme](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-app-windows-quickstart.png)
2. (İsteğe bağlı) UWP uygulaması projesini sunucu projesi olarak aynı çözüme ekleyin. Bu, yapmayı seçmeniz halinde, aynı Visual Studio çözümündeki uygulama ve arka uç için hata ayıklama ve test etmeyi daha kolay hale getirir. Bir çözüme UWP uygulaması projesi eklemek için Visual Studio 2015 veya sonraki bir sürümünü kullanıyor olmanız gerekir.
3. Başlangıç projesi olarak UWP uygulamasıyla, uygulamayı dağıtmak ve çalıştırmak için F5 tuşuna basın.
4. Uygulamada **TodoItem Ekle** metin kutusuna *Öğreticiyi tamamla* gibi anlamlı bir metin yazın ve ardından**Kaydet**’e tıklayın.

    ![Windows hızlı başlangıç tamamlanmış masaüstü](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-startup.png)

    Bu, Azure üzerinde barındırılan yeni mobil uygulama arka ucuna bir POST isteği gönderir.
5. (İsteğe bağlı) Uygulamayı durdurun ve farklı cihaz veya mobil öykünücü üzerinde yeniden başlatın.

    ![Windows hızlı başlangıç tamamlanmış telefon](./media/app-service-mobile-windows-store-dotnet-get-started/mobile-quickstart-completed.png)

    Önceki adımda kaydedilen verilerin UWP uygulaması başladıktan sonra Azure’dan yüklenip yüklenmediğine dikkat edin.

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
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2015]: https://go.microsoft.com/fwLink/p/?LinkID=534203

