---
title: Azure App Service Web Apps ile Python'ı yapılandırma
description: Bu öğreticide, yazma ve basit bir Web sunucusu Ağ Geçidi Arabirimi (WSGI) uyumlu Python uygulamasını Azure App Service Web Apps üzerinde yapılandırma seçenekleri açıklanmaktadır.
services: app-service
documentationcenter: python
tags: python
author: cephalin
manager: erikre
editor: ''
ms.assetid: fd00dc91-9935-4331-b955-4bd71e66d518
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/26/2016
ms.author: huvalo
ms.openlocfilehash: 796de682df28c28bc66f2409e486dfdc221aedd1
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2018
ms.locfileid: "42062113"
---
# <a name="configuring-python-with-azure-app-service-web-apps"></a>Azure App Service Web Apps ile Python'ı yapılandırma
Bu öğreticide, yazma ve yapılandırma temel Web sunucusu Ağ Geçidi Arabirimi (WSGI) uyumlu bir Python uygulamasını yönelik seçeneklerle [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Bu, Git dağıtımı, sanal ortamı ve paket yüklemesi Requirements.txt dosyasını kullanma gibi ek özellikleri açıklanmaktadır.

## <a name="bottle-django-or-flask"></a>Bottle, Django veya Flask?
Azure marketi, Bottle, Django ve Flask çerçeveler için şablonlar içerir. İlk web uygulamanız Azure App Service'te geliştiriyorsanız hızlı bir şekilde Azure portalından bir tane oluşturabilirsiniz:

* [Linux üzerinde Bottle ile Web uygulaması](https://portal.azure.com/#create/PTVS.BottleLinux)
* [Linux üzerinde Django ile Web uygulaması](https://portal.azure.com/#create/PTVS.DjangoLinux)
* [Linux üzerinde Flask ile Web uygulaması](https://portal.azure.com/#create/PTVS.FlaskLinux)

Veya [Azure Marketi'nde kendiniz keşfedin](https://portal.azure.com/#create/hub).

## <a name="web-app-creation-on-azure-portal"></a>Azure Portal'da Web uygulaması oluşturma
Bu öğreticide, bir var olan bir Azure aboneliği ve Azure portalına erişim varsayılır.

Mevcut bir web uygulaması yoksa birinden oluşturabilirsiniz [Azure portalında](https://portal.azure.com). Sol üst köşede tıklayın **kaynak Oluştur** > **Web + mobil** > **Web uygulaması**.

## <a name="git-publishing"></a>Git yayımlamayı
Yeni oluşturulan web uygulamanız için, [Azure Uygulama Hizmeti’nde Yerel Git Dağıtımı](app-service-deploy-local-git.md) başlığındaki yönergeleri izleyerek Git yayımlamayı yapılandırın. Bu öğreticide Git oluşturmak, yönetmek ve Azure App Service'e Python web uygulamanızı yayımlamak için kullanılır.

Git yayımlamayı ayarladıktan sonra bir Git deposu oluşturulur ve web uygulamanızla ilişkili. Deponun URL'sini görüntülenir ve buluta yerel geliştirme ortamından veri göndermek için kullanılabilir. Git üzerinden uygulamalar yayımlamak için bir Git istemcisi de yüklü olduğundan emin olun ve Azure App Service'te web uygulaması içeriğinizi göndermeye verilen yönergeleri kullanın.

## <a name="application-overview"></a>Uygulamaya Genel Bakış
Sonraki bölümlerde aşağıdaki dosyalar oluşturulur. Git deposunun kök dizininde yerleştirilmelidir.

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a>WSGI işleyicisi
WSGI tarafından açıklanan bir Python standart olan [CESARETLENDİRİCİ 3333](http://www.python.org/dev/peps/pep-3333/) Python ve web sunucusu arasında bir arabirim tanımlama. Bu, çeşitli web uygulamaları ve çerçeveleri kullanarak Python yazmak için standartlaştırılmış bir arabirim sağlar. Popüler Python web çerçeveleri WSGI kullanmaya hemen başlayın. Azure App Service Web Apps sağlar; bu tür bir çerçeveler için destek özel işleyici WSGI belirtimi yönergelerine uyduğundan sürece ek olarak, İleri düzey kullanıcılar bile kendi yazabilirsiniz.

İşte bir örnek bir `app.py` , özel bir işleyici tanımlar:

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

Bu uygulama ile yerel olarak çalıştırabileceğiniz `python app.py`, ardından gözatın `http://localhost:5555` web tarayıcınızda.

## <a name="virtual-environment"></a>Sanal ortam
Önceki örnek uygulamayı herhangi bir dış paketi gerektirmeyen rağmen uygulamanızın bazı gerektirdiği olasıdır.

Dış Paket bağımlılıklarını yönetmenize yardımcı olmak için sanal ortamların oluşturulması Azure Git dağıtımını destekler.

Azure depo kök dizininde bir requirements.txt algıladığında, adlı bir sanal ortam otomatik olarak oluşturduğu `env`. Bu, yalnızca ilk dağıtımı oluşur veya çalışma zamanı sırasında herhangi bir dağıtıma seçili Python sonra değişti.

Büyük olasılıkla geliştirme için yerel bir sanal ortam oluşturmak istediğiniz, ancak Git deponuza dahil değildir.

## <a name="package-management"></a>Paket Yönetimi
Requirements.txt içinde listelenen paketler pip kullanarak sanal ortamda otomatik olarak yüklenir. Bu her dağıtımda gerçekleşir, ancak bir paket zaten yüklü ise pip yüklemeyi atlar.

Örnek `requirements.txt`:

    azure==0.8.4


## <a name="python-version"></a>Python Sürümü
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

Örnek `runtime.txt`:

    python-2.7


## <a name="webconfig"></a>Web.config
Sunucu istekleri nasıl işleyeceğini belirtmek için bir web.config dosyası oluşturmanız gerekir.

Seçili Python çalışma zamanını x.y eşleştiği, deponuzda Web.x.y.config'i dosya varsa Azure uygun dosyayı otomatik olarak web.config kopyalar.

Sonraki bölümde açıklanan bir sanal ortam proxy betiği aşağıdaki web.config örnekler dayanır.  Örnekte kullanılan WSGI işleyici çalışmak `app.py` yukarıda.

Örnek `web.config` Python 2.7 için:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Örnek `web.config` Python 3.4 için:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Statik dosyalar web sunucusu tarafından geliştirilmiş performans için Python kodu aracılığıyla geçmeden doğrudan işlenir.

Yukarıdaki örneklerde, statik dosyalar diskte konumunu URL konumu eşleşmelidir. Bir istek için buna `http://pythonapp.azurewebsites.net/static/site.css` dosyanın disk üzerinde kullanılacak `\static\site.css`.

`WSGI_ALT_VIRTUALENV_HANDLER` WSGI işleyici belirttiğiniz olur. Önceki örneklerde, sahip `app.wsgi_app` işleyici adlı bir işlev olduğundan `wsgi_app` içinde `app.py` kök klasöründe.

`PYTHONPATH` özelleştirilebilir, ancak sanal ortamda requirements.txt içinde belirterek tüm bağımlılıklarınızı yüklerseniz, bunu değiştirmeniz gerekmez.

## <a name="virtual-environment-proxy"></a>Sanal ortam Proxy
Aşağıdaki betiği WSGI işleyici almak, sanal ortamı ve günlük hataları etkinleştirmek için kullanılır. Genel ve değişiklik olmadan kullanılan olacak şekilde tasarlanmıştır.

İçeriğini `ptvs_virtualenv_proxy.py`:

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject to terms and conditions of the Apache License, Version 2.0. A 
     # copy of the license can be found in the License.html file at the root of this distribution. If 
     # you cannot locate the Apache License, Version 2.0, please send an email to 
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing to be bound 
     # by the terms of the Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors to a log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n')

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')

        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)

        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()

        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))

        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []

        site.main()

        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a>Git dağıtımını özelleştirme
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]

## <a name="troubleshooting---package-installation"></a>Sorun giderme - Paket Yükleme
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a>Sorun giderme - Sanal Ortam
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="troubleshooting---startup-errors"></a>Sorun giderme - başlatma hataları
[!INCLUDE [web-sites-python-troubleshooting-wsgi-error-log](../../includes/web-sites-python-troubleshooting-wsgi-error-log.md)]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz. [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/).

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](https://azure.microsoft.com/try/app-service/) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
> 
> 
