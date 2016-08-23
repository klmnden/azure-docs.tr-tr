<properties 
    pageTitle="Visual Studio için Python Araçları 2.2 ile Azure’da Django ve MySQL" 
    description="MySQL veritabanı örneğinde veri depolayan ve bunu Azure App Service Web Apps’e dağıtan bir Django web uygulaması oluşturmak için Visual Studio için Python Araçlarını kullanmayı öğrenin." 
    services="app-service\web" 
    documentationCenter="python" 
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python"
    ms.topic="get-started-article" 
    ms.date="07/07/2016"
    ms.author="huvalo"/>

# Visual Studio için Python Araçları 2.2 ile Azure’da Django ve MySQL 

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Bu öğreticide, PTVS örnek şablonlarından birini kullanarak basit yoklamalar web uygulaması oluşturmak için [Visual Studio için Python Araçları]’nı (PTVS) kullanacaksınız. Azure üzerinde barındırılan bir MySQL hizmetini kullanmayı, MySQL kullanmak için web uygulaması yapılandırmayı ve web uygulamasını [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)’te yayımlamayı öğreneceksiniz.

> [AZURE.NOTE] Bu öğreticide yer alan bilgiler aşağıdaki videoda da kullanılabilir:
> 
> [PTVS 2.1: MySQL ile Django][video]

Azure Table Storage, MySQL ve SQL Database hizmetleriyle Bottle, Flask ve Django web altyapılarını kullanarak PTVS ile Azure Uygulama Hizmeti Web Apps geliştirmeyi kapsayan diğer makaleler için bkz. [Python Geliştirici Merkezi]. Bu makale App Service’e odaklanmakla birlikte, [Azure Cloud Services]’i geliştirirken adımlar benzerdir.

## Ön koşullar

 - Visual Studio 2015
 - [Python 2.7 32 bit] veya [Python 3.4 32 bit]
 - [Visual Studio için Python Araçları 2.2]
 - [Visual Studio Örnekleri VSIX için Python Tools 2.2]
 - [VS 2015 için Azure SDK Araçları]
 - Django 1.9 veya üzeri

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<!-- This note should not render as part of the the previous include. -->

> [AZURE.NOTE] Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](http://go.microsoft.com/fwlink/?LinkId=523751) sayfasına gidin. Kredi kartı ve taahhüt gerekli değildir.

## Proje oluşturma

Bu bölümde, örnek şablonu kullanarak bir Visual Studio projesi oluşturacaksınız. Bir sanal ortam oluşturacak ve gerekli paketleri yükleyeceksiniz. Sqlite kullanarak yerel bir veritabanı oluşturacaksınız. Sonra uygulamayı yerel olarak çalışacaksınız.

1. Visual Studio'da, **Dosya**, **Yeni Proje**’yi seçin.

1. [Visual Studio Örnekleri VSIX için Python Tools 2.2] proje şablonlarını **Python**, **Örnekler** altında bulabilirsiniz. **Yoklamalar Django Web Projesi**’ni seçin ve projeyi oluşturmak için Tamam’a tıklayın.

    ![Yeni Proje İletişim Kutusu](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)

1. Dış paketleri yüklemeniz istenir. **Sanal bir ortama yükle**’yi seçin.

    ![Dış Paketler İletişim Kutusu](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)

1. Temel yorumlayıcı olarak **Python 2.7** veya **Python 3.4**’ü seçin.

    ![Sanal Ortama Ekle İletişim Kutusu](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)

1. **Çözüm Gezgini**’nde, proje düğümüne sağ tıklayın ve **Python**’u ve ardından **Django Geçişi**’ni seçin.  Ardından **Django Yetkili Kullanıcısı Oluşturma**’yı seçin.

1. Bu, Django yönetim konsolunu açar ve proje klasöründe bir sqlite veritabanı oluşturur. Bir kullanıcı oluşturmak için istemleri takip edin.

1. `F5` tuşuna basarak uygulamanın çalıştığını doğrulayın.

1. Üst kısımdaki gezinti çubuğunda **Oturum Aç**’a tıklayın.

    ![Django Gezinti Çubuğu](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)

1. Veritabanını eşitlediğinizde, oluşturduğunuz kullanıcı için kimlik bilgilerini girin.

    ![Oturum Açma Formu](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)

1. **Örnek Yoklamalar Oluştur**’a tıklayın.

    ![Örnek Yoklamalar Oluştur](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)

1. Bir yoklamaya tıklayın ve oy verin.

    ![Örnek Yoklamalarda Oy Verme](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## MySQL Veritabanı Oluşturma

Veritabanı için, Azure’da ClearDB MySQL barındırılan veritabanı oluşturacaksınız.

Alternatif olarak, Azure'da çalışan kendi sanal makinenizi oluşturabilir ve ardından MySQL’i kendiniz yönetebilirsiniz.

Aşağıdaki adımları izleyerek, boş bir plan ile bir veritabanı oluşturabilirsiniz.

1. [Azure Portal]’da oturum açın.

1. Gezinti bölmesinin üst kısmında, **YENİ**’ye ve ardından **Veri + Depolama** ve de **MySQL Veritabanı**'na tıklayın. 

1. Yeni bir kaynak grubu oluşturarak yeni MySQL veritabanını yapılandırın ve bunun için uygun bir konum seçin.

1. MySQL veritabanı oluşturulduktan sonra, veritabanı dikey penceresinde **Özellikler**’e tıklayın.

1. **CONNECTION STRING** değerini panoya eklemek için kopyala düğmesini kullanın.

## Projeyi Yapılandırma

Bu bölümde, az önce oluşturduğunuz MySQL veritabanını kullanmak için web uygulamamızı yapılandıracaksınız. Ayrıca MySQL veritabanlarını Django ile birlikte kullanmak için gerekli ek Python paketlerini de yükleyeceksiniz. Sonra, web uygulamasını yerel olarak çalıştıracaksınız.

1. Visual Studio'da, *ProjectName* klasöründe **settings.py** dosyasını açın. Geçici olarak bağlantı dizesini düzenleyiciye yapıştırın. Bağlantı dizesi bu biçimdedir:

        Database=<NAME>;Data Source=<HOST>;User Id=<USER>;Password=<PASSWORD>

    Varsayılan veritabanı olan **ENGINE**’i MySQL kullanacak şekilde değiştirin ve **NAME**, **USER**, **PASSWORD** ve **HOST** değerlerini **CONNECTIONSTRING**’den alarak ayarlayın.

        DATABASES = {
            'default': {
                'ENGINE': 'django.db.backends.mysql',
                'NAME': '<Database>',
                'USER': '<User Id>',
                'PASSWORD': '<Password>',
                'HOST': '<Data Source>',
                'PORT': '',
            }
        }


1. Çözüm Gezgini'nde, **Python Ortamları** altında, sanal ortama sağ tıklayın ve **Python Paketini Yükle**’yi seçin.

1. **pip** kullanarak `mysqlclient` paketini yükleyin.

    ![Paketi Yükle İletişim Kutusu](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)

1. **Çözüm Gezgini**’nde, proje düğümüne sağ tıklayın ve **Python**’u ve ardından **Django Geçişi**’ni seçin.  Ardından **Django Yetkili Kullanıcısı Oluşturma**’yı seçin.

    Bu, önceki bölümde oluşturduğunuz MySQL veritabanı için tablolar oluşturur. Bu makalenin ilk bölümünde oluşturulan sqlite veritabanındaki kullanıcıyla eşleşmesi gerekmeyen bir kullanıcı oluşturmak için istemleri takip edin.

1. Uygulamayı `F5` ile çalıştırın. **Örnek Yoklamalar Oluştur** seçeneğiyle oluşturulan yoklamalar ve oy vermeyle gönderilen veriler MySQL veritabanında seri hale getirilir.

## Web uygulamasını Azure App Service’te yayımlama

Azure .NET SDK’sı web uygulamanızı Azure App Service’te dağıtmanız için kolay bir yol sağlar.

1. **Çözüm Gezgini**’nde, proje düğümüne sağ tıklayın ve **Yayımla**’yı seçin.

    ![Web Yayımlama İletişim Kutusu](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)

1. **Microsoft Azure Uygulama Hizmeti**’ne tıklayın.

1. Yeni bir web uygulaması oluşturmak için **Yeni**’ye tıklayın.

1. Aşağıdaki alanları doldurun ve **Oluştur**’a tıklayın:
    - **Web Uygulaması adı**
    - **App Service planı**
    - **Kaynak grubu**
    - **Bölge**
    - **Veritabanı sunucusu**nu **Veritabanı yok** olarak bırakın.

1. Diğer tüm varsayılanları kabul edin ve **Yayımla**’ya tıklayın.

1. Web tarayıcınız yayımlanan web uygulamasına otomatik olarak açılır. Azure’da barındırılan **MySQL** kullanarak, web uygulamasının beklendiği gibi çalıştığını görmelisiniz.

    ![Web Tarayıcısı](./media/web-sites-python-ptvs-django-mysql/PollsDjangoAzureBrowser.png)

    Tebrikler! MySQL tabanlı web uygulamanızı başarıyla Azure’da yayımladınız.

## Sonraki adımlar

Visual Studio, Django ve MySQL için Python Araçları hakkında daha fazla bilgi için bu bağlantıları izleyin.

- [Visual Studio için Python Araçları Belgeleri]
  - [Web Projeleri]
  - [Bulut Hizmeti Projeleri]
  - [Microsoft Azure’da Uzaktan Hata Ayıklama]
- [Django Belgeleri]
- [MySQL]

Daha fazla bilgi için bkz. [Python Geliştirici Merkezi](/develop/python/).

<!--Link references-->

[Python Geliştirici Merkezi]: /develop/python/
[Azure Cloud Services]: ../cloud-services-python-ptvs.md

<!--External Link references-->

[Azure Portal]: https://portal.azure.com
[Visual Studio için Python Araçları]: http://aka.ms/ptvs
[Visual Studio için Python Araçları 2.2]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio Örnekleri VSIX için Python Tools 2.2]: http://go.microsoft.com/fwlink/?LinkID=624025
[VS 2015 için Azure SDK Araçları]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32 bit]: http://go.microsoft.com/fwlink/?LinkId=517190 
[Python 3.4 32 bit]: http://go.microsoft.com/fwlink/?LinkId=517191
[Visual Studio Belgeleri için Python Araçları]: http://aka.ms/ptvsdocs
[Microsoft Azure’da Uzaktan Hata Ayıklama]: http://go.microsoft.com/fwlink/?LinkId=624026
[Web Projeleri]: http://go.microsoft.com/fwlink/?LinkId=624027
[Bulut Hizmeti Projeleri]: http://go.microsoft.com/fwlink/?LinkId=624028
[Django Belgeleri]: https://www.djangoproject.com/
[MySQL]: http://www.mysql.com/
[video]: http://youtu.be/oKCApIrS0Lo



<!--HONumber=Aug16_HO1-->


