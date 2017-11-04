---
title: "Bir Windows Server Azure VM'de Django web uygulaması | Microsoft Docs"
description: "Bir Windows Server 2012 R2 Datacenter VM ile klasik dağıtım modelini kullanarak azure'da Django tabanlı bir Web sitesi barındırma öğrenin."
services: virtual-machines-windows
documentationcenter: python
author: huguesv
manager: wpickett
editor: 
tags: azure-service-management
ms.assetid: e36484d1-afbf-47f5-b755-5e65397dc1c3
ms.service: virtual-machines-windows
ms.workload: web
ms.tgt_pltfrm: vm-windows
ms.devlang: python
ms.topic: article
ms.date: 05/31/2017
ms.author: huvalo
ms.openlocfilehash: 283a296fb39863c2801be1093cc4f56904786abd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="django-hello-world-web-app-on-a-windows-server-vm"></a>Windows Server VM üzerinde Django Hello World web uygulaması

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Azure Resource Manager ve klasik dağıtım modeli](../../../resource-manager-deployment-model.md). Bu makalede Klasik dağıtım modeli açıklanır. En yeni dağıtımların Resource Manager modelini kullanmasını öneririz.

Bu öğretici, Azure Virtual Machines'de Windows Server'daki Django tabanlı bir Web sitesi barındırmak nasıl gösterir. Öğreticide, Azure ile konusunda deneyim varsayıyoruz. Öğreticiyi tamamladığınızda, Django tabanlı bir uygulamayı oluşturan ve bulutta çalışan sahip olabilir.

Şunları nasıl yapacağınızı öğrenin:

* Bir Azure sanal makinesi konağa Django ayarlayın. Bu öğretici, bunu yapmak açıklanmaktadır ancak **Windows Server**, Azure'da bir Linux VM barındırılan için aynı yapabilirsiniz.
* Yeni bir Django uygulaması Windows oluşturun.

Öğretici, temel bir Hello World web uygulamasının nasıl oluşturulacağını gösterir. Uygulamayı bir Azure sanal makinesi barındırılır.

Aşağıdaki ekran görüntüsünde tamamlanan uygulama gösterilir:

![Bir tarayıcı penceresi Azure'da hello world sayfasını görüntüler][1]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-to-host-django"></a>Oluşturma ve bir Azure sanal makinesi konağa Django ayarlama

1. Bir Azure sanal makinesi ile Windows Server 2012 R2 Datacenter dağıtımı oluşturmak için bkz: [Azure portalında Windows çalıştıran bir sanal makine oluşturma](tutorial.md).
2. Bağlantı noktası 80 trafiğinden web sanal makinede 80 numaralı bağlantı noktasına yönlendirmek için Azure ayarlayın:
   
   1. Azure portalında panoya gidin ve yeni oluşturulan sanal makineyi seçin.
   2. **Uç noktaları**’na ve ardından **Ekle**’ye tıklayın.

     ![Bir uç nokta ekleme](./media/python-django-web-app/django-helloworld-add-endpoint-new-portal.png)

   3. Üzerinde **uç nokta ekleme** sayfası için **adı**, girin **HTTP**. Ortak ve özel TCP bağlantı noktaları kümesine **80**.

     ![Bir ad girin ve ortak ve özel bağlantı noktalarını ayarlayın](./media/python-django-web-app/django-helloworld-add-endpoint-set-ports-new-portal.png)

   4. **Tamam** düğmesine tıklayın.
     
3. Panosunda, VM'yi seçin. Yeni oluşturulan Azure sanal makineye uzaktan oturum açmak için Uzak Masaüstü Protokolü (RDP) kullanmak için tıklatın **Bağlan**.  

> [!IMPORTANT] 
> Aşağıdaki yönergeler, sanal makineye doğru şekilde oturum açtığına varsayalım. Ayrıca komutları sanal makine ve yerel bilgisayarınızda değil dağıttığınız varsayılmıştır.

## <a id="setup"></a>Python, Django ve Wfastcgı yükleyin
> [!NOTE]
> Internet Explorer kullanarak indirmek için Internet Explorer yapılandırmanız gerekebilir **Artırılmış Güvenlik Yapılandırması** ayarlar. Bunu yapmak için tıklatın **Başlat** > **Yönetimsel Araçlar** > **Sunucu Yöneticisi'ni** > **yerel sunucu**. Tıklatın **IE Artırılmış Güvenlik Yapılandırması**ve ardından **devre dışı**.

1. Python 2.7 veya Python 3.4 gelen en son sürümlerini yüklemek [python.org][python.org].
2. PIP kullanarak wfastcgı ve django paketleri yükleyin.
   
    Python 2.7 için aşağıdaki komutu kullanın:
   
        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django
   
    Python 3.4 için aşağıdaki komutu kullanın:
   
        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="install-iis-with-fastcgi"></a>IIS Fastcgı ile yükleme
* Internet Information Services (IIS) Fastcgı desteğiyle yükleyin. Bu yürütmek için birkaç dakika sürebilir.
   
        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="create-a-new-django-application"></a>Yeni bir Django uygulaması oluşturma
1. C:\inetpub\wwwroot içinde yeni bir Django projesi oluşturmak için aşağıdaki komutu girin:
   
   Python 2.7 için aşağıdaki komutu kullanın:
   
       C:\Python27\Scripts\django-admin.exe startproject helloworld
   
   Python 3.4 için aşağıdaki komutu kullanın:
   
       C:\Python34\Scripts\django-admin.exe startproject helloworld
   
   ![New-AzureService komutunun sonucu](./media/python-django-web-app/django-helloworld-cmd-new-azure-service.png)
2. `django-admin` Komutu Django tabanlı Web siteleri için basit bir yapı oluşturur:
   
   * `helloworld\manage.py`barındırma başlatmak ve durdurmak, Django tabanlı Web sitesi barındırma yardımcı olur.
   * `helloworld\helloworld\settings.py`Uygulamanız için Django ayarlarına sahiptir.
   * `helloworld\helloworld\urls.py`her URL ve kendi görünüm arasında eşleme koduna sahip.
3. C:\inetpub\wwwroot\helloworld\helloworld dizininde views.py adlı yeni bir dosya oluşturun. Bu dosya, "hello world" sayfasını işler görünüme sahiptir. Kod düzenleyicisinde, aşağıdaki komutları girin:
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. Urls.py dosyasının içeriğini aşağıdaki komutları ile değiştirin:
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-iis"></a>ISS kurmak
1. Genel applicationhost.config dosyasında işleyicileri bölümün kilidini açın.  Bu, Python işleyicisi kullanmak web.config dosyanızın sağlar. Bu komutu ekleyin:
   
        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers
2. Wfastcgı etkinleştirin. Bu yürütülebilir, Python yorumlayıcı ve wfastcgi.py betik başvuran genel applicationhost.config dosyasının bir uygulama ekler.
   
    Python 2.7:
   
        C:\python27\scripts\wfastcgi-enable
   
    Python 3.4:
   
        C:\python34\scripts\wfastcgi-enable
3. C:\inetpub\wwwroot\helloworld içinde bir web.config dosyası oluşturun. Değeri `scriptProcessor` özniteliği, önceki adımdaki çıktı eşleşmelidir. Wfastcgı ayarı hakkında daha fazla bilgi için bkz: [pypı wfastcgı][wfastcgi].
   
   Python 2.7:
   
        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>
   
   Python 3.4:
   
        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>
4. Django proje klasöre işaret IIS varsayılan Web sitesi konumunu güncelleştirin:
   
        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"
5. Web tarayıcınızda yükleyin.

![Bir tarayıcı penceresinde hello world sayfasını Azure üzerinde görüntüler.][1]

## <a name="shut-down-your-azure-virtual-machine"></a>Azure, sanal makineyi Kapat
Bu öğreticiyi tamamladığınızda, kapatıldı veya öğretici için oluşturduğunuz Azure VM Kaldır öneririz. Bu diğer öğreticileri için kaynakları serbest bırakır ve Azure kullanım ücretlerinin yansıtılmasını önleyebilirsiniz.

[1]: ./media/python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
