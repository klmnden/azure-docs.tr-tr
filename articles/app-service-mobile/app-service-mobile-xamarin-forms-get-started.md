---
title: Xamarin.Forms ile Mobile Apps’i kullanmaya başlama
description: Xamarin.Forms geliştirme için Azure Mobile Apps kullanmaya başlamak için bu öğreticiyi izleyin.
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: crdun
ms.openlocfilehash: b99513cad34bba1b050a24795ecb21d0357d19c1
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65416089"
---
# <a name="create-a-xamarinforms-app-with-azure"></a>Azure ile Xamarin.Forms uygulaması oluşturma

[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

Bu öğreticide, bir Xamarin.Forms mobil uygulamasına bulut tabanlı bir arka uç hizmetini Azure Uygulama Hizmeti’nin Mobile Apps özelliğini kullanarak nasıl ekleyeceğiniz gösterilir. Yeni bir Mobil Uygulama arka ucu ve uygulama verilerini Azure’da depolayan basit bir yapılacaklar listesi Xamarin.Forms uygulaması oluşturacaksınız.

Bu öğreticiyi tamamlamak Xamarin.Forms uygulamalarına ilişkin tüm Mobile Apps öğreticileri için ön koşuldur.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Bir hesabınız yoksa, Azure deneme sürümünü kaydolabilir ve deneme süresi bittikten sonra dahi kullanmaya devam edebileceğiniz 10 ücretsiz mobil uygulama edinebilirsiniz. Daha fazla bilgi için bkz. [Azure Ücretsiz Denemesi](https://azure.microsoft.com/pricing/free-trial/).

* Mac için Visual Studio veya Visual Studio 2017 veya sonraki sürümlerde, Xamarin için Visual Studio Araçları Yönergeler için bkz. [Xamarin yükleme sayfası][Install Xamarin].

* (isteğe bağlı) iOS uygulaması oluşturmak için, Xcode 9.0 veya üzerine sahip bir Mac gereklidir. Mac için Visual Studio (Mac ağda kullanılabilir olduğu sürece) daha sonra kullanılabilir veya Visual Studio 2017 veya iOS uygulamaları geliştirmek için kullanılabilir.

## <a name="create-a-new-mobile-apps-back-end"></a>Yeni bir Mobile Apps arka ucu oluşturma

Yeni bir Mobile Apps arka ucu oluşturmak için aşağıdakileri yapın:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Şimdi mobil uygulamalarınızın kullanabileceği bir Mobile Apps arka ucu ayarlamış oldunuz. Sırada, basit bir yapılacaklar listesi arka ucu için bir sunucu projesi indirme ve Azure’a yayımlama var.

## <a name="configure-the-server-project"></a>Sunucu projesi yapılandırma

Sunucu projesini Node.js veya .NET arka ucunu kullanacak şekilde yapılandırmak için aşağıdakileri yapın:

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinforms-solution"></a>Xamarin.Forms çözümünü indirme ve çalıştırma

Çözümü açmak için Xamarin için Visual Studio Araçları gereklidir, bkz. [Xamarin yükleme yönergeleri][Install Xamarin]. Araçlar zaten yüklendiyse, çözümü indirip açmak için aşağıdaki adımları izleyin:

### <a name="visual-studio"></a>Visual Studio

1. [Azure Portal] gidin.

2. Mobil Uygulamanızın ayarlar dikey penceresinde **Hızlı Başlangıç** (Dağıtım altında) > **Xamarin.Forms**'a tıklayın. 3. Adım altında, henüz seçili değilse, **Yeni uygulama oluştur**’u seçin.  Sonra **İndir** düğmesine tıklayın.

   Bu işlem, mobil uygulamanıza bağlı olan istemci uygulaması içeren bir projeyi indirir. Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

3. İndirdiğiniz projeyi çıkarın ve ardından Visual Studio'da açın.

   ![Visual Studio'da ayıklanan proje][8]

4. Android veya Windows projelerini ve ağa bağlı bir Mac bilgisayar varsa iOS projesini çalıştırmak için aşağıdaki yönergeleri izleyin.

### <a name="visual-studio-for-mac"></a>Mac için Visual Studio

1. [Azure Portal] gidin.

2. Mobil Uygulamanızın ayarlar dikey penceresinde **Hızlı Başlangıç** (Dağıtım altında) > **Xamarin.Forms**'a tıklayın. 3. Adım altında, henüz seçili değilse, **Yeni uygulama oluştur**’u seçin.  Sonra **İndir** düğmesine tıklayın.

   Bu işlem, mobil uygulamanıza bağlı olan istemci uygulaması içeren bir projeyi indirir. Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

3. İndirdiğiniz projeyi ayıklayın ve Mac için Visual Studio'da açın.

   ![Mac için Visual Studio'da ayıklanan proje][9]

4. Android veya iOS projelerini çalıştırmak için aşağıdaki yönergeleri izleyin.



## <a name="optional-run-the-android-project"></a>(İsteğe bağlı) Android projesi çalıştırma

Bu bölümde Xamarin.Android projesini çalıştırırsınız. Android cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

### <a name="visual-studio"></a>Visual Studio

1. Android (Droid) projesine sağ tıklayın ve ardından **Başlangıç Projesi Olarak Ayarla**’yı seçin.

2. **Yapı** menüsünde, **Yapılandırma Yöneticisi**’ni seçin.

3. **Yapılandırma Yöneticisi** iletişim kutusunda, Android projesinin yanındaki **Yapı** ve **Dağıt** onay kutularını seçin ve paylaşılan kod projesinde **Yapı** kutusunun işaretli olduğundan emin olun.

4. Projeyi oluşturmak ve uygulamayı Android öykünücüsünde başlatmak için **F5** tuşuna basın veya **Başlat** düğmesine tıklayın.

### <a name="visual-studio-for-mac"></a>Mac için Visual Studio

1. Android projesine sağ tıklayın ve ardından **Başlangıç Projesi Olarak Ayarla**’yı seçin.

2. Projeyi oluşturmak ve uygulamayı Android öykünücüsünde başlatmak için **Çalıştır** menüsünü ve ardından **Hata Ayıklamayı Başlat**’ı seçin.



Uygulamada, *Xamarin öğren* gibi anlamlı bir metin yazın ve ardından artı simgesini (**+**) seçin.

![Android yapılacaklar uygulaması][11]

Bu işlem, Azure üzerinde barındırılan yeni Mobile Apps arka ucuna bir post isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler Mobile Apps arka ucu tarafından döndürülür ve veriler listede görüntülenir.

> [!NOTE]
> Mobile Apps arka ucuna erişen kod, çözümün paylaşılan kod projesinin **TodoItemManager.cs** C# dosyasındadır.
>

## <a name="optional-run-the-ios-project"></a>(İsteğe bağlı) iOS projesi çalıştırma

Bu bölümde iOS cihazlarda Xamarin iOS projesi çalıştırırsınız. iOS cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

### <a name="visual-studio"></a>Visual Studio

1. iOS projesine sağ tıklayın ve ardından **Başlangıç Projesi Olarak Ayarla**’yı seçin.

2. **Yapı** menüsünde, **Yapılandırma Yöneticisi**’ni seçin.

3. **Yapılandırma Yöneticisi** iletişim kutusunda, iOS projesinin yanındaki **Yapı** ve **Dağıt** onay kutularını seçin ve paylaşılan kod projesinde **Yapı** kutusunun işaretli olduğundan emin olun.

4. Projeyi oluşturmak ve uygulamayı iPhone öykünücüsünde başlatmak için **F5** tuşuna basın.

### <a name="visual-studio-for-mac"></a>Mac için Visual Studio

1. iOS projesine sağ tıklayın ve ardından **Başlangıç Projesi Olarak Ayarla**’yı seçin.

2. **Çalıştır** menüsünde, **Hata Ayıklamayı Başlat**’a tıklayarak projeyi oluşturun ve iPhone öykünücüsünde uygulamayı başlatın.



Uygulamada, *Xamarin öğren* gibi anlamlı bir metin yazın ve ardından artı simgesini (**+**) seçin.

![iOS yapılacaklar uygulaması][10]

Bu işlem, Azure üzerinde barındırılan yeni Mobile Apps arka ucuna bir post isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler Mobile Apps arka ucu tarafından döndürülür ve veriler listede görüntülenir.

> [!NOTE]
> Çözümün paylaşılan kod projesinin **TodoItemManager.cs** C# dosyasında Mobile Apps arka ucuna erişen kodu bulacaksınız.
>

## <a name="optional-run-the-windows-project"></a>(İsteğe bağlı) Windows projesi çalıştırma

Bu bölümde, Windows cihazlar için Xamarin.Forms Evrensel Windows Platformu (UWP) projelerini çalıştırırsınız. Windows cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

### <a name="visual-studio"></a>Visual Studio

1. Herhangi bir UWP projesine sağ tıklayın ve ardından **Başlangıç Projesi Olarak Ayarla**’yı seçin.

2. **Yapı** menüsünde, **Yapılandırma Yöneticisi**’ni seçin.

3. **Yapılandırma Yöneticisi** iletişim kutusunda, seçtiğiniz Windows projesinin yanındaki **Yapı** ve **Dağıt** onay kutularını seçin ve paylaşılan kod projesinde **Yapı** kutusunun işaretli olduğundan emin olun.

4. Projeyi oluşturmak ve uygulamayı Windows öykünücüsünde başlatmak için **F5** tuşuna basın veya **Başlat** düğmesine (**Yerel Makine** yazmalıdır) tıklayın.

> [!NOTE]
> Windows projesi macOS üzerinde çalıştırılamaz.



Uygulamada, *Xamarin öğren* gibi anlamlı bir metin yazın ve ardından artı simgesini (**+**) seçin.

Bu işlem, Azure üzerinde barındırılan yeni Mobile Apps arka ucuna bir post isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler Mobile Apps arka ucu tarafından döndürülür ve veriler listede görüntülenir.

![UWP yapılacaklar uygulaması][12]

> [!NOTE]
> Çözümünüzün taşınabilir sınık kitaplık projesinin **TodoItemManager.cs** C# dosyasında Mobile Apps arka ucuna erişen kodu bulacaksınız.
>

## <a name="troubleshooting"></a>Sorun giderme

Çözümü oluşturma konusunda sorun yaşarsanız, NuGet paket yöneticisini çalıştırıp `Xamarin.Forms`’in en son sürümüne güncelleştirin ve Android projesinde `Xamarin.Android` destek paketlerini güncelleştirin. Hızlı Başlangıç projeleri her zaman son sürümleri içermeyebilir.

Android projenizde başvurulan tüm destek paketlerinin aynı sürüme sahip olması gerektiğini unutmayın. [Azure Mobile Apps NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/), Android platformu için `Xamarin.Android.Support.CustomTabs` bağımlılığına sahiptir, yani projeniz daha yeni destek paketleri kullanıyorsa, çakışmaları önlemek için doğrudan gerekli sürüme sahip bu paketi yüklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* [Uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-xamarin-forms-get-started-users.md) Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.

* [Uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-xamarin-forms-get-started-push.md) Uygulamanıza anında iletme bildirimleri desteği eklemeyi ve anında iletme bildirimleri göndermek için Azure Notification Hubs’ı kullanmak üzere Mobile App arka ucunuzu yapılandırmayı öğrenin.

* [Uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-xamarin-forms-get-started-offline-data.md) Mobile Apps arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı eşitleme ile mobil uygulamanızın verilerini ağ bağlantısı olmasa bile görüntüleyebilir, ekleyebilir ve değiştirebilirsiniz.

* [Mobile Apps için yönetilen istemciyi kullanma](app-service-mobile-dotnet-how-to-use-client-library.md) Xamarin uygulamanızda yönetilen istemci SDK’sıyla çalışmayı öğrenin.

* [Diğer Azure hizmetlerini Xamarin.Forms ile kullanma](https://docs.microsoft.com/xamarin/xamarin-forms/data-cloud/) Arama, depolama ve bilişsel hizmetler gibi ek Azure hizmetlerini Xamarin.Forms uygulamalarına ekleme.

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
[Install Xamarin]: https://docs.microsoft.com/xamarin/cross-platform/get-started/installation/
[Mobile app SDK]: https://go.microsoft.com/fwlink/?LinkId=257545
[Azure portal]: https://portal.azure.com/
