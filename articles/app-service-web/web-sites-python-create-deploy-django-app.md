<properties
    pageTitle="Azure’da Django ile web uygulamaları oluşturma"
    description="Azure App Service Web Apps’te bir Python web uygulaması çalıştırmayı gösteren bir öğretici."
    services="app-service\web"
    documentationCenter="python"
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article" 
    ms.date="02/19/2016"
    ms.author="huvalo"/>


# Azure’da Django ile web uygulamaları oluşturma

Bu öğretici, çalışan, [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)’te Python çalıştırmaya nasıl başlayacağınızı açıklar. Web Apps sınırlı ücretsiz barındırma ve hızlı dağıtım sağlar ve Python’u kullanmanıza olanak tanır! Uygulamanız büyüdükçe, ücretli barındırmaya geçebilir ve aynı zamanda tüm diğer Azure hizmetleriyle tümleştirebilirsiniz.

Django web altyapısını kullanarak bir uygulama oluşturacaksınız (bu öğreticinin diğer sürümleri için bkz. [Flask](web-sites-python-create-deploy-flask-app.md) ve [Bottle](web-sites-python-create-deploy-bottle-app.md)). Azure Marketi'nde bir web uygulaması oluşturacak, Git dağıtımı ayarlayacak ve depoyu yerel olarak kopyalayacaksınız. Sonra, uygulamayı yerel olarak çalıştıracak, değişiklikler yapacak, yürütecek ve bunları Azure'a ileteceksiniz. Öğretici, Windows veya Mac/Linux’ta bunun nasıl yapıldığını gösterir.

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](http://go.microsoft.com/fwlink/?LinkId=523751) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.


## Ön koşullar

- Windows, Mac veya Linux
- Python 2.7 ya da 3.4
- setuptools, pip, virtualenv (yalnızca Python 2.7)
- Git
- [Visual Studio için Python Araçları][] (PTVS) - Not: Bu özellik isteğe bağlıdır

**Not**: TFS yayımlama şu anda Python projeleri için desteklenmiyor.

### Windows

Python 2.7 ya da 3.4 yüklü (32 bit) değilse, Web Platformu Yükleyicisi'ni kullanarak [Python 2.7 için Azure SDK] veya [Python 3.4 için Azure SDK]’yı yüklemenizi öneririz. Bu, Python’un 32 bit sürümünü, setuptools, PIP, virtualenv vb.yükler (32 bit Python, Azure ana makinelerde yüklü olandır). Alternatif olarak, Python’u [python.org] adresinden edinebilirsiniz.

Git için, [Windows için Git] veya [Windows için GitHub]’ı öneririz. Visual Studio kullanıyorsanız, tümleşik Git desteğini kullanabilirsiniz.

Ayrıca [Visual Studio için Python Araçları 2.2]’yi yüklemenizi öneririz  Bu isteğe bağlıdır, ancak ücretsiz Visual Studio Community 2013 veya Web için Visual Studio Express 2013 içeren [Visual Studio] varsa, bu size mükemmel bir Python IDE verir.

### Mac/Linux

Python ve Git sizde zaten yüklü olmalıdır, ancak Python 2.7 veya 3.4 olduğundan emin olun.


## Portalda Web Uygulaması Oluşturma

Uygulamanızı oluşturmanın ilk adımı, [Azure Portal](https://portal.azure.com) aracılığıyla web uygulaması oluşturmaktır.

1. Azure Portal’da oturum açın ve sol alt köşede **NEW** düğmesine tıklayın.
3. Arama kutusuna, "python" yazın.
4. Arama sonuçlarında, **Django**’yu seçin ve ardından **Oluştur**’a tıklayın.
5. Yeni bir App Service planı ve bunun için yeni bir kaynak grubu oluşturma şeklinde, yeni Django uygulamasını yapılandırın. Sonra, **Oluştur**’a tıklayın.
6. Yeni oluşturulan web uygulamanız için, [Azure Uygulama Hizmeti’nde Yerel Git Dağıtımı](app-service-deploy-local-git.md) başlığındaki yönergeleri izleyerek Git yayımlamayı yapılandırın.

## Uygulamaya Genel Bakış

### Git deposu içeriği

Burada, sonraki bölümde kopyalayacağımız, ilk Git deposunda bulacağınız dosyalara bir genel bakış yer alır.

    \app\__init__.py
    \app\forms.py
    \app\models.py
    \app\tests.py
    \app\views.py
    \app\static\content\
    \app\static\fonts\
    \app\static\scripts\
    \app\templates\about.html
    \app\templates\contact.html
    \app\templates\index.html
    \app\templates\layout.html
    \app\templates\login.html
    \app\templates\loginpartial.html
    \DjangoWebProject\__init__.py
    \DjangoWebProject\settings.py
    \DjangoWebProject\urls.py
    \DjangoWebProject\wsgi.py

Uygulama için ana kaynaklar Ana düzenle birlikte 3 sayfadan (dizin, hakkında, iletişim) oluşur. Statik içerik ve betikler bootstrap, jquery, modernize ve yanıtı içerir.

    \manage.py

Yerel yönetim ve geliştirme sunucusu desteği. Uygulamayı yerel olarak çalıştırmak, veritabanını eşitlemek vb. için bunu kullanın.

    \db.sqlite3

Varsayılan veritabanı. Uygulamanın çalıştırılması için gerekli tabloları içerir, ancak herhangi bir kullanıcı içermez (kullanıcı oluşturmak için veritabanını eşitleyin) içermiyor.

    \DjangoWebProject.pyproj
    \DjangoWebProject.sln

[Visual Studio için Python Araçları] ile birlikte kullanmak için proje dosyaları.

    \ptvs_virtualenv_proxy.py

Sanal ortamlar için IIS proxy ve PTVS uzaktan hata ayıklama desteği

    \requirements.txt

Bu uygulamaya dış paketler gerekir. Dağıtım betiği pip bu dosyada listelenen paketleri pip yükler.

    \web.2.7.config
    \web.3.4.config

IIS yapılandırma dosyaları. Dağıtım betiği, uygun web.x.y.config’i kullanır ve bunu web.config olarak kopyalar.

### İsteğe bağlı dosyalar - Dağıtımı özelleştirme

[AZURE.INCLUDE [web-sites-python-django-customizing-deployment](../../includes/web-sites-python-django-customizing-deployment.md)]

### İsteğe bağlı dosyalar - Python çalışma zamanı

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### Sunucu üzerindeki ek dosyalar

Bazı dosyalar sunucuda yer alır ancak git deposuna eklenmez. Bunlar dağıtım betiği tarafından oluşturulur.

    \web.config

IIS yapılandırma dosyası. Her dağıtımda web.x.y.config tarafından oluşturulur.

    \env\

Python sanal ortamı. Web uygulamasında uyumlu sanal ortam zaten yoksa, dağıtım ırasında oluşturulur. Requirements.txt içinde listelenen paketler pip yüklüdür, ancak paketler zaten yüklü ise pip yüklemeyi atlar.

Sonraki 3 bölümde farklı 3 ortamda web uygulaması geliştirmeye devam etme açıklanmaktadır:

- Windows, Visual Studio için Python Araçları ile
- Windows, komut satırı ile
- Mac/Linux, komut satırı ile


## Web uygulaması geliştirme - Windows - Visual Studio için Python Araçları

### Depoyu kopyalama

İlk olarak, Azure Portal'da sağlanan URL'yi kullanarak depoyu kopyalayın. Daha fazla bilgi için bkz. [Azure Uygulama Hizmeti’nde Yerel Git Dağıtımı](app-service-deploy-local-git.md).

Depo kök dizininde bulunan çözüm dosyasını (.sln) açın.

![](./media/web-sites-python-create-deploy-django-app/ptvs-solution-django.png)

### Sanal ortamı oluşturun.

Şimdi, yerel geliştirme için sanal bir ortam oluşturacağız. Sağ **Python Ortamları**’na sağ tıklayın **Sanal Ortam Ekle...** seçeneğini seçin.

- Ortam adının `env` olduğundan emin olun.

- Temel yorumlayıcıyı seçin. Web uygulamanız için seçilen Python ile aynı sürümü kullandığınızdan emin olun (runtime.txt içinde veya Azure Portal’da uygulamanızın **Uygulama Ayarları** dikey penceresinde).

- Paketleri indirme ve yükleme seçeneğinin işaretli olduğundan emin olun.

![](./media/web-sites-python-create-deploy-django-app/ptvs-add-virtual-env-27.png)

**Oluştur**’a tıklayın. Bu, sanal ortamı oluşturur ve requirements.txt içinde listelenen bağımlılıkları yükler.

### Süper kullanıcı oluşturma

Uygulamayla birlikte gelen veritabanında tanımlı bir süper kullanıcı yoktur. Uygulamadaki oturum açma işlevini ya da Django yönetim arabirimini (etkinleştirmeye karar verirseniz) kullanmak için, bir süper kullanıcı oluşturmanız gerekir.

Proje klasörünüzdeki komut satırından bunu çalıştırın:

    env\scripts\python manage.py createsuperuser

Kullanıcı adı, parola vb. ayarlamak için yönergeleri izleyin.

### Geliştirme sunucusu kullanarak çalıştırma

Hata ayıklamayı başlatmak için F5 tuşuna basın, böylece web tarayıcınız yerel olarak çalışan sayfaya otomatik olarak açılır.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

Kaynaklarda kesme noktalarını ayarlayabilir, gözcü pencerelerini kullanabilirsiniz vb. Çeşitli özellikler hakkında daha fazla bilgi için bkz. [Visual Studio Belgeleri için Python Araçları].

### Değişiklik yapma

Şimdi uygulama kaynakları ve/veya şablonlarında değişiklikler yapmayı deneyebilirsiniz.

Değişikliklerinizi test ettikten sonra bunları Git deposuna kaydedin:

![](./media/web-sites-python-create-deploy-django-app/ptvs-commit-django.png)

### Daha fazla paket yükleme

Uygulamanızın Python ve Django ötesinde bağımlılıkları olabilir.

Pip kullanarak ek paketleri yükleyebilirsiniz. Bir paketi yüklemek için, sanal ortamda sağ tıklayıp **Python Paketini Yükle**’yi seçin.

Örneğin, size Azure Storage, Service Bus ve diğer Azure hizmetleri için erişim imkanı sağlayan, Python için Azure SDK'yı yüklemek için şunu girin: `azure`

![](./media/web-sites-python-create-deploy-django-app/ptvs-install-package-dialog.png)

Sanal ortamda sağ tıklayıp **requirements.txt oluştur**’u seçerek requirements.txt dosyasını güncelleştirin.

Ardından, requirements.txt dosyasındaki değişiklikleri Git deposuna uygulayın.

### Azure’a dağıtma

Bir dağıtımı tetiklemek için tıklatın **Eşitle** veya **İlet**’e tıklayın. Eşitleme, iletme ve çekme işlemini yapar.

![](./media/web-sites-python-create-deploy-django-app/ptvs-git-push.png)

Sanal ortam, yükleme paketleri vb. oluşturacağında, ilk dağıtımın gerçekleşmesi biraz zaman alır.

Visual Studio dağıtımın ilerleme durumunu göstermez. Çıktıyı gözden geçirmek isterseniz, üzerinde bakın [Sorun Giderme - Dağıtım](#troubleshooting-deployment)’daki bölüme bakın.

Yaptığınız değişiklikleri görmek için Azure URL'sine gidin.


## Web uygulaması geliştirme - Windows - komut satırı

### Depoyu kopyalama

İlk olarak, Azure Portal'da sağlanan URL'yi kullanarak depoyu kopyalayın ve uzak olarak Azure deposunu ekleyin. Daha fazla bilgi için bkz. [Azure Uygulama Hizmeti’nde Yerel Git Dağıtımı](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### Sanal ortamı oluşturun.

Geliştirme amacına yönelik yeni bir sanal ortam oluşturacağız (bunu depoya eklemeyin). Uygulama üzerinde çalışan her geliştiricinin yerel olarak kendininkini oluşturacağı şekilde, Python’daki sanal ortamlar yeniden yerleştirilebilir değildir.

Web uygulamanız için seçilen Python ile aynı sürümü kullandığınızdan emin olun (runtime.txt içinde veya Azure Portal’da uygulamanızın Uygulama Ayarları dikey penceresinde).

Python 2.7:

    c:\python27\python.exe -m virtualenv env

Python 3.4:

    c:\python34\python.exe -m venv env

Uygulamanız için gereken herhangi bir dış paketi yükleyin. Sanal ortamınıza paketleri yüklemek için depo kökündeki requirements.txt dosyasını kullanabilirsiniz:

    env\scripts\pip install -r requirements.txt

### Süper kullanıcı oluşturma

Uygulamayla birlikte gelen veritabanında tanımlı bir süper kullanıcı yoktur. Uygulamadaki oturum açma işlevini ya da Django yönetim arabirimini (etkinleştirmeye karar verirseniz) kullanmak için, bir süper kullanıcı oluşturmanız gerekir.

Proje klasörünüzdeki komut satırından bunu çalıştırın:

    env\scripts\python manage.py createsuperuser

Kullanıcı adı, parola vb. ayarlamak için yönergeleri izleyin.

### Geliştirme sunucusu kullanarak çalıştırma

Aşağıdaki komutla bir geliştirme sunucusu altında uygulamayı başlatabilirsiniz:

    env\scripts\python manage.py runserver

Konsol URL’yi ve sunucunun dinlediği bağlantı noktasını görüntüler:

![](./media/web-sites-python-create-deploy-django-app/windows-run-local-django.png)

Sonra, bu URL için web tarayıcınızı açın.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

### Değişiklik yapma

Şimdi uygulama kaynakları ve/veya şablonlarında değişiklikler yapmayı deneyebilirsiniz.

Değişikliklerinizi test ettikten sonra bunları Git deposuna kaydedin:

    git add <modified-file>
    git commit -m "<commit-comment>"

### Daha fazla paket yükleme

Uygulamanızın Python ve Django ötesinde bağımlılıkları olabilir.

Pip kullanarak ek paketleri yükleyebilirsiniz. Örneğin, size Azure Storage, Service Bus ve diğer Azure hizmetleri için erişim imkanı sağlayan, Python için Azure SDK'yı yüklemek için aşağıdakileri yazın:

    env\scripts\pip install azure

Requirements.txt dosyasını güncelleştirdiğinizden emin olun:

    env\scripts\pip freeze > requirements.txt

Değişiklikleri uygulayın:

    git add requirements.txt
    git commit -m "Added azure package"

### Azure’a dağıtma

Bir dağıtımı tetiklemek için, değişiklikleri Azure’a gönderin:

    git push azure master

Sanal ortam oluşturma, paketleri yükleme, web.config oluşturma dahil dağıtım betiği çıktısını görürsünüz.

Yaptığınız değişiklikleri görmek için Azure URL'sine gidin.


## Web uygulaması geliştirme - Mac/Linux - komut satırı

### Depoyu kopyalama

İlk olarak, Azure Portal'da sağlanan URL'yi kullanarak depoyu kopyalayın ve uzak olarak Azure deposunu ekleyin. Daha fazla bilgi için bkz. [Azure Uygulama Hizmeti’nde Yerel Git Dağıtımı](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### Sanal ortamı oluşturun.

Geliştirme amacına yönelik yeni bir sanal ortam oluşturacağız (bunu depoya eklemeyin). Uygulama üzerinde çalışan her geliştiricinin yerel olarak kendininkini oluşturacağı şekilde, Python’daki sanal ortamlar yeniden yerleştirilebilir değildir.

Web uygulamanız için seçilen Python ile aynı sürümü kullandığınızdan emin olun (runtime.txt içinde veya Azure Portal’da uygulamanızın Uygulama Ayarları dikey penceresinde).

Python 2.7:

    python -m virtualenv env

Python 3.4:

    python -m venv env

or

    pyvenv env

Uygulamanız için gereken herhangi bir dış paketi yükleyin. Sanal ortamınıza paketleri yüklemek için depo kökündeki requirements.txt dosyasını kullanabilirsiniz:

    env/bin/pip install -r requirements.txt

### Süper kullanıcı oluşturma

Uygulamayla birlikte gelen veritabanında tanımlı bir süper kullanıcı yoktur. Uygulamadaki oturum açma işlevini ya da Django yönetim arabirimini (etkinleştirmeye karar verirseniz) kullanmak için, bir süper kullanıcı oluşturmanız gerekir.

Proje klasörünüzdeki komut satırından bunu çalıştırın:

    env/bin/python manage.py createsuperuser

Kullanıcı adı, parola vb. ayarlamak için yönergeleri izleyin.

### Geliştirme sunucusu kullanarak çalıştırma

Aşağıdaki komutla bir geliştirme sunucusu altında uygulamayı başlatabilirsiniz:

    env/bin/python manage.py runserver

Konsol URL’yi ve sunucunun dinlediği bağlantı noktasını görüntüler:

![](./media/web-sites-python-create-deploy-django-app/mac-run-local-django.png)

Sonra, bu URL için web tarayıcınızı açın.

![](./media/web-sites-python-create-deploy-django-app/mac-browser-django.png)

### Değişiklik yapma

Şimdi uygulama kaynakları ve/veya şablonlarında değişiklikler yapmayı deneyebilirsiniz.

Değişikliklerinizi test ettikten sonra bunları Git deposuna kaydedin:

    git add <modified-file>
    git commit -m "<commit-comment>"

### Daha fazla paket yükleme

Uygulamanızın Python ve Django ötesinde bağımlılıkları olabilir.

Pip kullanarak ek paketleri yükleyebilirsiniz. Örneğin, size Azure Storage, Service Bus ve diğer Azure hizmetleri için erişim imkanı sağlayan, Python için Azure SDK'yı yüklemek için aşağıdakileri yazın:

    env/bin/pip install azure

Requirements.txt dosyasını güncelleştirdiğinizden emin olun:

    env/bin/pip freeze > requirements.txt

Değişiklikleri uygulayın:

    git add requirements.txt
    git commit -m "Added azure package"

### Azure’a dağıtma

Bir dağıtımı tetiklemek için, değişiklikleri Azure’a gönderin:

    git push azure master

Sanal ortam oluşturma, paketleri yükleme, web.config oluşturma dahil dağıtım betiği çıktısını görürsünüz.

Yaptığınız değişiklikleri görmek için Azure URL'sine gidin.


## Sorun giderme - Paket Yükleme

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## Sorun giderme - Sanal Ortam

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]


## Sorun giderme - Statik Dosyalar

Django statik dosyaları toplama kavramına sahiptir. Bu, tüm statik dosyaları özgün konumlarından alır ve tek bir klasöre kopyalar. Bu uygulama için, bunlar `/static` klasörüne kopyalanır.

Statik dosyalar farklı Django “uygulamalarından” gelebileceğinden bu yapılır. Örneğin, Django yönetim arabirimlerindeki statik dosyalar sanal ortamdaki bir Django kitaplığı alt klasöründe yer alır. Bu uygulama tarafından tanımlanan statik dosyalar `/app/static` içinde bulunur. Daha fazla Django “uygulamaları” kullandıkça, birden fazla yerde bulunan statik dosyalarınız olur.

Uygulamayı hata ayıklama modunda çalıştırdığınızda, uygulama statik dosyaları özgün konumlarından işleme alır.

Uygulama yayın modunda çalışırken, uygulama statik dosyaları işleme **almaz**. Dosyaları işleme almak web sunucusunun sorumluluğundadır. Bu uygulama için, IIS statik dosyaları `/static` konumundan işleme alır.

Statik dosyaların toplanması işlemi önceden toplanan dosyalar temizlenerek, dağıtım betiğinin bir parçası olarak otomatik yapılır Bu, her dağıtımda meydana gelen toplamanın dağıtımı yavaşlattığı anlamına gelir ancak, potansiyel bir güvenlik sorunu önlenerek eski dosyaların kullanılmamasını sağlar.

Django uygulamanız için statik dosya toplamayı atlamak istiyorsanız:

    \.skipDjango

Toplamayı yerel makinenizde el ile yapmanız gerekir:

    env\scripts\python manage.py collectstatic

Sonra, `.gitignore` içindeki `\static` klasörünü kaldırmanız ve Git deposuna eklemeniz gerekir.


## Sorun giderme - Ayarlar

Uygulama için çeşitli ayarlar`DjangoWebProject/settings.py` içinde değiştirilebilir.

Geliştiriciye kolaylık sağlamak için hata ayıklama modu etkindir. Bunun olumlu bir yan etkisi, yerle olarak çalıştırırken, statik dosyaları toplamak zorunda kalmadan, resimleri ve diğer statik içeriği görebilecek olmanızdır.

Hata ayıklama modunu devre dışı bırakmak için:

    DEBUG = False

Hata ayıklama devre dışı bırakıldığında, `ALLOWED_HOSTS` değerinin Azure ana bilgisayar adını içerecek şekilde güncelleştirilmesi gerekir. Örneğin:

    ALLOWED_HOSTS = (
        'pythonapp.azurewebsites.net',
    )

veya herhangi birini etkinleştirmek için:

    ALLOWED_HOSTS = (
        '*',
    )

Uygulamada, hata ayıklama ile yayımlama modu arasında geçiş yapma ve ana bilgisayar adını almakla uğraşmak için daha karmaşık bir şey yapmak isteyebilirsiniz.

Azure Portal aracılığıyla, **CONFIGURE** sayfasında, **uygulaması ayarları** bölümünde ortam değişkenlerini ayarlayabilirsiniz.  Bu, kaynaklarda (bağlantı dizeleri, parolalar vb.) görünmesini istemeyebileceğini ya da Azure ile yerel makineniz arasında farklı ayarlamak isteyeceğiniz değerler için faydalı olabilir. `settings.py` içinde, `os.getenv` kullanarak ortam değişkenlerini sorgulayabilirsiniz.


## Bir Veritabanını Kullanma

Uygulama ile birlikte gelen veritabanı bir sqlite veritabanıdır. Neredeyse hiçbir kurulum gerektirmediğinden, geliştirme için kullanmak üzere kullanışlı ve faydalıdır. Veritabanı proje klasöründeki db.sqlite3 dosyasında depolanır.

Azure Django uygulamasından kullanımı kolay olan veritabanı hizmetleri sağlar. Django uygulamasından [SQL Database] ve [MySQL] kullanma öğreticileri, veritabanı hizmeti oluşturmak, `DjangoWebProject/settings.py` içinde veritabanı ayarlarını değiştirmek için gerekli adımları ve yükleme için gerekli kitaplıkları gösterir.

Elbette, kendi veritabanı sunucularınızı yönetmek isterseniz, bunu Windows veya Linux Azure üzerinde çalışan sanal makineleri kullanarak yapabilirsiniz.


## Django Yönetim Arabirimi

Modellerinizi oluşturmaya başladıktan sonra, veritabanını bazı verilerle doldurmak istersiniz. Etkileşimli olarak içerik ekleme ve düzenleme işlemi yapmanın kolay bir yolu Django yönetim arabirimini kullanmaktır.

Yönetim arabirimi kodu, uygulama kaynaklarında açıklanmıştır ve kolayca etkinleştirebileceğiniz şekilde açıkça işaretlenmiştir.

Etkinleştirildikten sonra, veritabanını eşitleyin, uygulamayı çalıştırın ve `/admin` konumuna gidin.


## Sonraki Adımlar

Visual Studio için Django ve Python Araçları hakkında daha fazla bilgi için bu bağlantıları izleyin.

- [Django Belgeleri]
- [Visual Studio Belgeleri için Python Araçları]

SQL Database’i ve MySQL’i kullanma hakkında bilgi için:

- [Visual Studio için Python Araçları ile Azure’da Django ve MySQL]
- [Visual Studio için Python Araçları ile Azure’da Django ve SQL Database]

Daha fazla bilgi için bkz. [Python Geliştirici Merkezi](/develop/python/).


## Yapılan değişiklikler
* Web Sitelerinden App Service’e kadar değiştirme kılavuzu için bkz. [Azure App Service ve Mevcut Azure Hizmetlerine Etkileri](http://go.microsoft.com/fwlink/?LinkId=529714)


<!--Link references-->
[Visual Studio için Python Araçları ile Azure’da Django ve MySQL]: web-sites-python-ptvs-django-mysql.md
[Visual Studio için Python Araçları ile Azure’da Django ve SQL Database]: web-sites-python-ptvs-django-sql.md
[SQL Database]: web-sites-python-ptvs-django-sql.md
[MySQL]: web-sites-python-ptvs-django-mysql.md

<!--External Link references-->
[Python 2.7 için Azure SDK]: http://go.microsoft.com/fwlink/?linkid=254281
[Python 3.4 için Azure SDK]: http://go.microsoft.com/fwlink/?linkid=516990
[python.org]: http://www.python.org/
[Windows için Git]: http://msysgit.github.io/
[Windows için GitHub]: https://windows.github.com/
[Visual Studio için Python Araçları]: http://aka.ms/ptvs
[Visual Studio için Python Araçları 2.2]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Visual Studio Belgeleri için Python Araçları]: http://aka.ms/ptvsdocs
[Django Belgeleri]: https://www.djangoproject.com/



<!--HONumber=Aug16_HO1-->


