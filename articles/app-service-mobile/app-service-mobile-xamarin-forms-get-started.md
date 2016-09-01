<properties
    pageTitle="Xamarin.Forms ile Mobile Apps’i kullanmaya başlama"
    description="Xamarin.Forms geliştirme için Azure Mobile Apps kullanmaya başlamak için bu öğreticiyi izleyin."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="wesmc7777"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/11/2016"
    ms.author="glenga"/>

#Xamarin.Forms uygulaması oluşturma

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##Genel Bakış

Bu öğreticide, bir Xamarin.Forms mobil uygulamasına Azure Mobil Uygulaması arka ucunu kullanarak bulut tabanlı arka uç hizmetini nasıl ekleyeceğiniz gösterilir. Yeni bir Mobil Uygulama arka ucu ve uygulama verilerini Azure’da depolayan basit bir _Yapılacaklar listesi_ Xamarin.Forms uygulaması oluşturacaksınız.

Bu öğreticiyi tamamlamak Xamarin.Forms uygulamalarına ilişkin tüm Mobile Apps öğreticileri için ön koşuldur.

##Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Etkin bir Azure hesabı. Bir hesabınız yoksa, Azure deneme sürümünü kaydolabilir ve deneme süresi bittikten sonra dahi kullanmaya devam edebileceğiniz 10 ücretsiz Mobil Uygulama edinebilirsiniz. Ayrıntılar için bkz. [Azure Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/).

* Xamarin ile Visual Studio. Yönergeler için bkz. [Visual Studio ve Xamarin için Kurulum ve Yükleme](https://msdn.microsoft.com/library/mt613162.aspx). 

* Xcode v7.0 veya daha sonraki sürümü ve Xamarin Studio Community yüklü bir Mac. Bkz. [Visual Studio ve Xamarin için kurulum ve yükleme](https://msdn.microsoft.com/library/mt613162.aspx) ve [Mac kullanıcıları için kurulum, yükleme ve doğrulamalar](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).
 
>[AZURE.NOTE] Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’de hemen kısa süreli bir başlangıç Mobil Uygulama oluşturabileceğiniz [App Service’i Deneyin](https://tryappservice.azure.com/?appServiceName=mobile) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.

## Yeni bir Azure Mobil Uygulama arka ucu oluşturma

Yeni Mobil Uygulama arka ucu oluşturmak için bu adımları izleyin.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]


Artık, mobil istemci uygulamalarınız tarafından kullanılabilecek bir Azure Mobil Uygulama arka ucu sağladınız. Sonra, basit bir "yapılacaklar listesi" arka ucu için bir sunucu projesi indirecek ve Azure’a yayımlayacaksınız.

## Sunucu projesi yapılandırma

Node.js veya .NET arka ucu kullanmak için sunucu projesi yapılandırmak üzere aşağıdaki adımları izleyin.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

##Xamarin.Forms çözümünü indirme ve çalıştırma

Burada birkaç seçeneğiniz vardır. Çözümü bir Mac’e indirebilir ve Xamarin Studio’da açabilir ya da çözümü bir Windows bilgisayara indirebilir ve ağ ile bağlı bir Mac kullanarak iOS uygulaması oluşturmak için açabilirsiniz. Xamarin kurulum senaryoları hakkında daha ayrıntılı yönergeler gerekiyorsa bkz. [Visual Studio ve Xamarin için kurulum ve yükleme](https://msdn.microsoft.com/library/mt613162.aspx).

Şimdi devam edelim:

 1. Mac veya Windows bilgisayarınızda, ir tarayıcı penceresinde [Azure Portal]’ı açın.
 2. Mobil uygulamanızın dikey penceresinde, **Kullanmaya Başlama** (Mobil altında) > **Xamarin.Forms**’a tıklayın. 3. Adım altında, henüz seçili değilse, **Yeni uygulama oluştur**’u seçin.  Sonra **İndir** düğmesine tıklayın.

    Bu, mobil uygulamanıza bağlı olan istemci uygulaması içeren bir projeyi indirir. Sıkıştırılmış proje dosyasını yerel bilgisayarınıza kaydedin ve kaydettiğiniz yeri not edin.

 3. İndirdiğiniz projeyi çıkarın ve Xamarin Studio veya Visual Studio'da açın.

    ![][9]

    ![][8]


##(İsteğe bağlı) iOS projesi çalıştırma

Bu bölüm iOS cihazları için Xamarin iOS projesi çalıştırmaya yöneliktir. iOS cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

####Xamarin Studio’da

1. iOS projesine sağ tıklayın ve ardından **Başlangıç Projesi Olarak Ayarla**’ya tıklayın.
2. **Çalıştır** menüsünde, **Hata Ayıklamayı Başlat**’a tıklayarak projeyi oluşturun ve uygulamayı iPhone öykünücüsünde başlatın.

####Visual Studio’da
1. iOS projesine sağ tıklayın ve ardından **Başlangıç Projesi Olarak Ayarla**’ya tıklayın.
2. **Yapı** menüsünde, **Yapılandırma Yöneticisi**’ne tıklayın.
3. **Yapılandırma Yöneticisi** iletişim kutusunda, iOS projesine ait **Yapı** ve **Dağıt** onay kutularını seçin.
4. **F5** tuşuna basarak projeyi oluşturun ve uygulamayı iPhone öykünücüsünde başlatın.

    >[AZURE.NOTE] Oluşturma sorunları varsa, NuGet paket yöneticisini çalıştırın ve Xamarin destek paketlerinin en son sürümüne güncelleştirin. Bazen Hızlı Başlangıç projeleri son sürüme güncelleştirilirken geri kalabilir.    

Uygulamada, _Xamarin öğren_ gibi anlamlı bir metin yazın ve ardından **+** düğmesine tıklayın.

![][10]

Bu, Azure üzerinde barındırılan yeni mobil uygulama arka ucuna bir POST isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil uygulama arka ucu tarafından döndürülür ve veriler listede görüntülenir.

>[AZURE.NOTE]
> Çözümünüzün taşınabilir sınık kitaplık projesinin .TodoItemManager.cs C# dosyasında mobil uygulamanızın arka ucuna erişen kodu bulacaksınız.

##(İsteğe bağlı) Android projesi çalıştırma

Bu bölüm Android cihazları için Xamarin Android projesi çalıştırmaya yöneliktir. Android cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.

####Xamarin Studio’da

1. Android projesine sağ tıklayın ve ardından **Başlangıç Projesi Olarak Ayarla**’ya tıklayın.
2. **Çalıştır** menüsünde, **Hata Ayıklamayı Başlat**’a tıklayarak projeyi oluşturun ve uygulamayı Android öykünücüsünde başlatın.

####Visual Studio’da
1. Android (Droid) projesine sağ tıklayın ve ardından **Başlangıç Projesi Olarak Ayarla**’ya tıklayın.
4. **Yapı** menüsünde, **Yapılandırma Yöneticisi**’ne tıklayın.
5. **Yapılandırma Yöneticisi** iletişim kutusunda, Android projesine ait **Yapı** ve **Dağıt** onay kutularını seçin.
6. **F5** tuşuna basarak projeyi oluşturun ve uygulamayı Android öykünücüsünde başlatın.

    >[AZURE.NOTE] Oluşturma sorunları varsa, NuGet paket yöneticisini çalıştırın ve Xamarin destek paketlerinin en son sürümüne güncelleştirin. Bazen Hızlı Başlangıç projeleri son sürüme güncelleştirilirken geri kalabilir.    


Uygulamada, _Xamarin öğren_ gibi anlamlı bir metin yazın ve ardından **+** düğmesine tıklayın.

![][11]

Bu, Azure üzerinde barındırılan yeni mobil uygulama arka ucuna bir POST isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil uygulama arka ucu tarafından döndürülür ve veriler listede görüntülenir.

> [AZURE.NOTE]
> Çözümünüzün taşınabilir sınık kitaplık projesinin .TodoItemManager.cs C# dosyasında mobil uygulamanızın arka ucuna erişen kodu bulacaksınız.


##(İsteğe bağlı) Windows projesi çalıştırma


Bu bölüm Windows cihazları için Xamarin WinApp projesi çalıştırmaya yöneliktir. Windows cihazlarıyla çalışmıyorsanız, bu bölümü atlayabilirsiniz.


####Visual Studio’da
1. Windows projelerinden birine sağ tıklayın ve ardından **Başlangıç Projesi Olarak Ayarla**’ya tıklayın.
4. **Yapı** menüsünde, **Yapılandırma Yöneticisi**’ne tıklayın.
5. **Yapılandırma Yöneticisi** iletişim kutusunda, seçtiğiniz Windows projesine ait **Yapı** ve **Dağıt** onay kutularını seçin.
6. **F5** tuşuna basarak projeyi oluşturun ve uygulamayı Windows öykünücüsünde başlatın.

    >[AZURE.NOTE] Oluşturma sorunları varsa, NuGet paket yöneticisini çalıştırın ve Xamarin destek paketlerinin en son sürümüne güncelleştirin. Bazen Hızlı Başlangıç projeleri son sürüme güncelleştirilirken geri kalabilir.    


Uygulamada, _Xamarin öğren_ gibi anlamlı bir metin yazın ve ardından **+** düğmesine tıklayın.

Bu, Azure üzerinde barındırılan yeni mobil uygulama arka ucuna bir POST isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil uygulama arka ucu tarafından döndürülür ve veriler listede görüntülenir.

![][12]

> [AZURE.NOTE]
> Çözümünüzün taşınabilir sınık kitaplık projesinin .TodoItemManager.cs C# dosyasında mobil uygulamanızın arka ucuna erişen kodu bulacaksınız.

##Sonraki adımlar

* [Uygulamanıza kimlik doğrulaması ekleme](app-service-mobile-xamarin-forms-get-started-users.md)  
Uygulamanızdaki kullanıcıların kimliklerini bir kimlik sağlayıcısı ile nasıl doğrulayacağınızı öğrenin.

* [Uygulamanıza anında iletme bildirimleri ekleme](app-service-mobile-xamarin-forms-get-started-push.md)  
Uygulamanıza anında iletme bildirimleri desteği eklemeyi ve anında iletme bildirimleri göndermek için Azure Notification Hubs’ı kullanmak üzere Mobile App arka ucunuzu yapılandırmayı öğrenin.

* [Uygulamanız için çevrimdışı eşitlemeyi etkinleştirme](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Mobil Uygulama arka ucu kullanarak uygulamanıza çevrimdışı destek eklemeyi öğrenin. Çevrimdışı eşitleme son kullanıcıların, ağ bağlantısı yokken dahi, mobil uygulama ile etkileşim kurmalarına &mdash;veri görüntüleme, ekleme ya da değiştirme&mdash; olanak tanır.

* [Azure Mobile Apps için yönetilen istemci kullanma](app-service-mobile-dotnet-how-to-use-client-library.md)  
Xamarin uygulamanızda yönetilen istemci SDK’sıyla çalışmayı öğrenin. 


<!-- Anchors. -->
[Mobil uygulama arka ucu kullanmaya başlama]:#getting-started
[Yeni bir mobil uygulama arka ucu oluşturma]:#create-new-service
[Sonraki Adımlar]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobil uygulama SDK’sı]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure Portal]: https://portal.azure.com/




<!--HONumber=Aug16_HO4-->


