---
title: "Xamarin.Forms ile Mobile Apps’i kullanmaya başlama"
description: "Xamarin.Forms geliştirme için Azure Mobile Apps kullanmaya başlamak için bu öğreticiyi izleyin."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 12c7eb78b5049b385ee34c7ac8e3574b064d7ecf
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="create-a-xamarinforms-app"></a>Xamarin.Forms uygulaması oluşturma
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, bir Xamarin.Forms mobil uygulamasına bulut tabanlı bir arka uç hizmetini Azure Uygulama Hizmeti’nin Mobile Apps özelliğini kullanarak nasıl ekleyeceğiniz gösterilir. Yeni bir Mobil Uygulama arka ucu ve uygulama verilerini Azure’da depolayan basit bir yapılacaklar listesi Xamarin.Forms uygulaması oluşturacaksınız.

Bu öğreticiyi tamamlamak Xamarin.Forms uygulamalarına ilişkin tüm Mobile Apps öğreticileri için ön koşuldur.

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Bir hesabınız yoksa, Azure deneme sürümünü kaydolabilir ve deneme süresi bittikten sonra dahi kullanmaya devam edebileceğiniz 10 ücretsiz mobil uygulama edinebilirsiniz. Daha fazla bilgi için bkz. [Azure Ücretsiz Denemesi](https://azure.microsoft.com/pricing/free-trial/).

* Xamarin ile Visual Studio. Bilgi için [Visual Studio ve Xamarin’i ayarlama ve yükleme](https://msdn.microsoft.com/library/mt613162.aspx) sayfasına bakın.

* Xcode v7.0 veya daha sonraki sürümü ve Xamarin Studio Community yüklü bir Mac. Bilgi için bkz. [Visual Studio ve Xamarin’i ayarlama ve yükleme](https://msdn.microsoft.com/library/mt613162.aspx) ve [Mac kullanıcıları için kurulum, yükleme ve doğrulamalar](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

## <a name="create-a-new-mobile-apps-back-end"></a>Yeni bir Mobile Apps arka ucu oluşturma

Yeni bir Mobile Apps arka ucu oluşturmak için aşağıdakileri yapın:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Şimdi mobil istemci uygulamalarınızın kullanabileceği bir Mobile Apps arka ucu ayarlamış oldunuz. Sırada, basit bir yapılacaklar listesi arka ucu için bir sunucu projesi indirme ve Azure’a yayımlama var.

## <a name="configure-the-server-project"></a>Sunucu projesi yapılandırma

Sunucu projesini Node.js veya .NET arka ucunu kullanacak şekilde yapılandırmak için aşağıdakileri yapın:

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinforms-solution"></a>Xamarin.Forms çözümünü indirme ve çalıştırma

Çözümü iki yolla indirebilirsiniz. Çözümü bir Mac’e indirebilir ve Xamarin Studio’da açabilir ya da çözümü bir Windows bilgisayara indirebilir ve ağ ile bağlı bir Mac kullanarak iOS uygulaması oluşturmak için açabilirsiniz. Daha fazla bilgi için [Visual Studio ve Xamarin’i ayarlama ve yükleme](https://msdn.microsoft.com/library/mt613162.aspx) sayfasına bakın.

Bir Mac veya Windows bilgisayarda aşağıdakileri yapın:

1. [Azure Portal] gidin.

2. Mobil Uygulamanızın ayarlar dikey penceresinde **Hızlı Başlangıç** (Dağıtım altında) > **Xamarin.Forms**'a tıklayın. 3. Adım altında, henüz seçili değilse, **Yeni uygulama oluştur**’u seçin.  Sonra **İndir** düğmesine tıklayın.

   Bu işlem, mobil uygulamanıza bağlı olan istemci uygulaması içeren bir projeyi indirir. Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

3. İndirdiğiniz projeyi çıkarın ve Xamarin Studio (Mac) veya Visual Studio'da (Windows) açın.

   ![Xamarin Studio'da ayıklanan proje][9]

   ![Visual Studio'da ayıklanan proje][8]

## <a name="optional-run-the-ios-project"></a>(İsteğe bağlı) iOS projesi çalıştırma
Bu bölümde iOS cihazlarda Xamarin iOS projesi çalıştırırsınız. iOS cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

#### <a name="in-xamarin-studio"></a>Xamarin Studio’da
1. iOS projesine sağ tıklayın ve ardından **Başlangıç Projesi Olarak Ayarla**’yı seçin.

2. **Çalıştır** menüsünde, **Hata Ayıklamayı Başlat**’a tıklayarak projeyi oluşturun ve iPhone öykünücüsünde uygulamayı başlatın.

#### <a name="in-visual-studio"></a>Visual Studio’da
1. iOS projesine sağ tıklayın ve ardından **Başlangıç Projesi Olarak Ayarla**’yı seçin.

2. **Yapı** menüsünde, **Yapılandırma Yöneticisi**’ni seçin.

3. **Yapılandırma Yöneticisi** iletişim kutusunda, iOS projesinin yanındaki **Yapı** ve **Dağıt** onay kutularını seçin.

4. Projeyi oluşturmak ve uygulamayı iPhone öykünücüsünde başlatmak için **F5** tuşuna basın.

   > [!NOTE]
   > Projeyi oluşturma konusunda sorun yaşarsanız, NuGet paket yöneticisini çalıştırın ve Xamarin destek paketlerinin en son sürümüne güncelleştirin. Hızlı Başlangıç projelerinin son sürümlerine güncelleştirilme işlemleri yavaş olabilir.    
   >
   >

5. Uygulamada, *Xamarin öğren* gibi anlamlı bir metin yazın ve ardından artı simgesini (**+**) seçin.

    ![][10]

    Bu işlem, Azure üzerinde barındırılan yeni Mobile Apps arka ucuna bir post isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler Mobile Apps arka ucu tarafından döndürülür ve veriler listede görüntülenir.

    > [!NOTE]
    > Çözümünüzün taşınabilir sınık kitaplık projesinin .TodoItemManager.cs C# dosyasında Mobile Apps arka ucuna erişen kodu bulacaksınız.
    >
    >

## <a name="optional-run-the-android-project"></a>(İsteğe bağlı) Android projesi çalıştırma
Bu bölümde Android için Xamarin droid projesini çalıştırırsınız. Android cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

#### <a name="in-xamarin-studio"></a>Xamarin Studio’da

1. Android projesine sağ tıklayın ve ardından **Başlangıç Projesi Olarak Ayarla**’yı seçin.

2. Projeyi oluşturmak ve uygulamayı Android öykünücüsünde başlatmak için **Çalıştır** menüsünde, **Hata Ayıklamayı Başlat**’a tıklayın.

#### <a name="in-visual-studio"></a>Visual Studio’da

1. Android (Droid) projesine sağ tıklayın ve ardından **Başlangıç Projesi Olarak Ayarla**’yı seçin.

2. **Yapı** menüsünde, **Yapılandırma Yöneticisi**’ni seçin.

3. **Yapılandırma Yöneticisi** iletişim kutusunda, Android projesinin yanındaki **Yapı** ve **Dağıt** onay kutularını seçin.

4. Projeyi oluşturmak ve Android öykünücüsünde uygulamayı başlatmak için **F5** tuşuna basın.

   > [!NOTE]
   > Projeyi oluşturma konusunda sorun yaşarsanız, NuGet paket yöneticisini çalıştırın ve Xamarin destek paketlerinin en son sürümüne güncelleştirin. Hızlı Başlangıç projelerinin son sürümlerine güncelleştirilme işlemleri yavaş olabilir.    
   >
   >

5. Uygulamada, *Xamarin öğren* gibi anlamlı bir metin yazın ve ardından artı simgesini (**+**) seçin.

    ![][11]
    
    Bu işlem, Azure üzerinde barındırılan yeni Mobile Apps arka ucuna bir post isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler Mobile Apps arka ucu tarafından döndürülür ve veriler listede görüntülenir.
    
    > [!NOTE]
    > Çözümünüzün taşınabilir sınık kitaplık projesinin .TodoItemManager.cs C# dosyasında Mobile Apps arka ucuna erişen kodu bulacaksınız.
    >
    >

## <a name="optional-run-the-windows-project"></a>(İsteğe bağlı) Windows projesi çalıştırma

Bu bölümde Windows cihazları için Xamarin WinApp projesini çalıştırırsınız. Windows cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

#### <a name="in-visual-studio"></a>Visual Studio’da

1. Windows projelerinden birine sağ tıklayın ve ardından **Başlangıç Projesi Olarak Ayarla**’yı seçin.

2. **Yapı** menüsünde, **Yapılandırma Yöneticisi**’ni seçin.

3. **Yapılandırma Yöneticisi** iletişim kutusunda, seçtiğiniz Windows projesinin yanındaki **Yapı** ve **Dağıt** onay kutularını seçin.

4. Projeyi oluşturmak ve uygulamayı Windows öykünücüsünde başlatmak için **F5** tuşuna basın.

   > [!NOTE]
   > Projeyi oluşturma konusunda sorun yaşarsanız, NuGet paket yöneticisini çalıştırın ve Xamarin destek paketlerinin en son sürümüne güncelleştirin. Hızlı Başlangıç projelerinin son sürümlerine güncelleştirilme işlemleri yavaş olabilir.    
   >
   >

5. Uygulamada, *Xamarin öğren* gibi anlamlı bir metin yazın ve ardından artı simgesini (**+**) seçin.

    Bu işlem, Azure üzerinde barındırılan yeni Mobile Apps arka ucuna bir post isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler Mobile Apps arka ucu tarafından döndürülür ve veriler listede görüntülenir.
    
    ![][12]
    
    > [!NOTE]
    > Çözümünüzün taşınabilir sınık kitaplık projesinin .TodoItemManager.cs C# dosyasında Mobile Apps arka ucuna erişen kodu bulacaksınız.
    >
    >

## <a name="next-steps"></a>Sonraki adımlar

* [Uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-xamarin-forms-get-started-users.md)  
  Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.

* [Uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-xamarin-forms-get-started-push.md)  
  Uygulamanıza anında iletme bildirimleri desteği eklemeyi ve anında iletme bildirimleri göndermek için Azure Notification Hubs’ı kullanmak üzere Mobile App arka ucunuzu yapılandırmayı öğrenin.

* [Uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Mobile Apps arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı eşitleme ile mobil uygulamanızın verilerini ağ bağlantısı olmasa bile görüntüleyebilir, ekleyebilir ve değiştirebilirsiniz.

* [Mobile Apps için yönetilen istemciyi kullanma](app-service-mobile-dotnet-how-to-use-client-library.md)  
  Xamarin uygulamanızda yönetilen istemci SDK’sıyla çalışmayı öğrenin.

<!-- Anchors. -->
[Get started with Mobile Apps back ends]:#getting-started
[Create a new Mobile Apps back end]:#create-new-service
[Next steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure Portal]: https://portal.azure.com/
