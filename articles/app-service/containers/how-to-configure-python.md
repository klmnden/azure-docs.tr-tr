---
title: Linux'ta Azure App Service için Python uygulamalarını yapılandırma
description: Bu öğreticide Linux'ta Azure App Service için Python uygulamalarını yazma ve yapılandırma seçenekleri anlatılmaktadır.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/09/2018
ms.author: astay;cephalin;kraigb
ms.custom: mvc
ms.openlocfilehash: 71cbf0bb31a72e3b257f25c159d9d9eea31dbfbb
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901627"
---
# <a name="configure-your-python-app-for-the-azure-app-service-on-linux"></a>Python uygulamanızı Linux'ta Azure App Service için yapılandırma

Bu makalede [Linux'ta Azure App Service](app-service-linux-intro.md) hizmetinin Python uygulamalarını nasıl çalıştırdığı ve gerektiğinde App Service davranışlarını özelleştirme adımları anlatılmaktadır.

## <a name="container-characteristics"></a>Kapsayıcı özellikleri

Linux'ta App Service hizmetine dağıtılan Python uygulamaları GitHub deposunda tanımlı bir Docker kapsayıcısında çalışır: [Azure-App-Service/python kapsayıcısı](https://github.com/Azure-App-Service/python/tree/master/3.7.0).

Bu kapsayıcı aşağıdaki özelliklere sahiptir:

- Temel kapsayıcı görüntüsü `python-3.7.0-slim-stretch` şeklindedir ve bu da uygulamaların Python 3.7 ile çalıştırıldığını gösterir. Farklı bir Python sürümüne ihtiyacınız varsa bunu kullanmak yerine kendi kapsayıcı görüntünüzü derleyip dağıtmanız gerekir. Daha fazla bilgi için bkz. [Kapsayıcılar için Web App’e yönelik özel Docker görüntüsü kullanma](tutorial-custom-docker-image.md).

- Uygulamalar ek `--bind=0.0.0.0 --timeout 600` bağımsız değişkenleri kullanılarak [Gunicorn WSGI HTTP Server](http://gunicorn.org/) ile çalıştırılır.

- Temel görüntü varsayılan olarak Flask web çerçevesini içerir ancak kapsayıcı Django gibi WSGI ve Python 3.7 ile uyumlu diğer çerçeveleri de destekler.

- Django gibi ek paketleri yüklemek için `pip freeze > requirements.txt` kullanarak projenizin kök dizininde bir [*requirements.txt*](https://pip.pypa.io/en/stable/user_guide/#requirements-files) dosyası oluşturun. Ardından projenizi Git dağıtımı kullanarak App Service'te yayımlayın. Bunu yaptığınızda uygulamanızın bağımlılıklarının yüklenmesi için kapsayıcıda otomatik olarak `pip install -r requirements.txt` çalıştırılır.

## <a name="container-startup-process-and-customizations"></a>Kapsayıcı başlatma süreci ve özelleştirmeler

Başlatma sırasında Linux'ta App Service kapsayıcısı şu adımları çalıştırır:

1. Özel bir başlangıç komutu olup olmadığını denetler ve varsa uygular.
1. Django uygulamasının *wsgi.py* dosyasının olup olmadığını denetler ve varsa bu dosyayı kullanarak Gunicorn'u başlatır.
1. *application.py* adlı bir dosya olup olmadığını denetler ve varsa Flask uygulaması olduğunu kabul ederek Gunicorn'u `application:app` kullanarak başlatır.
1. Başka bir uygulama bulunamazsa yoksa kapsayıcıda bulunan varsayılan uygulamayı başlatır.

Aşağıdaki bölümlerde bu seçeneklerle ilgili ek ayrıntılar verilmiştir.

### <a name="django-app"></a>Django uygulaması

Django uygulamaları için App Service uygulama kodunuzda `wsgi.py` adlı bir dosya olup olmadığını denetler ve ardından şu komutu kullanarak Gunicorn'u çalıştırır:

```bash
# <module> is the path to the folder containing wsgi.py
gunicorn --bind=0.0.0.0 --timeout 600 <module>.wsgi
```

Başlangıç komutu üzerinde daha fazla denetime sahip olmak istiyorsanız [özel başlangıç komutu](#custom-startup-command) kullanın ve `<module>` yerine *wsgi.py* dosyasını içeren modülün adını yazın.

### <a name="flask-app"></a>Flask uygulaması

Flask için App Service *application.py* adlı bir dosya olup olmadığını denetler ve şu şekilde Gunicorn'u başlatır:

```bash
gunicorn --bind=0.0.0.0 --timeout 600 application:app
```

Ana uygulama modülünüz farklı bir dosyada bulunuyorsa uygulama nesnesi için farklı bir ad kullanın veya Gunicorn'a ek bağımsız değişkenler sağlamak istiyorsanız [özel başlangıç komutu](#custom-startup-command) kullanın. Bu bölümde giriş kodunun *hello.py* dosyasında bulunduğu ve `myapp` adlı bir Flask uygulama nesnesini kullanan bir Flask örneği verilmiştir.

### <a name="custom-startup-command"></a>Özel başlangıç komutu

Özel bir Gunicorn başlangıç komutu sağlayarak kapsayıcının başlangıç davranışını denetleyebilirsiniz. Örneğin ana modülünün adı *hello.py*, Flask uygulama nesnesinin adı da `myapp` olan bir Flask uygulamanız varsa kullanmanız gereken komut şöyle olacaktır:

```bash
gunicorn --bind=0.0.0.0 --timeout 600 hello:myapp
```

Komuta Gunicorn için ek bağımsız değişkenler de ekleyebilirsiniz, örneğin: `--workers=4`. Daha fazla bilgi için bkz. [Gunicorn'u Çalıştırma](http://docs.gunicorn.org/en/stable/run.html) (docs.gunicorn.org).

Özel komut sağlamak için aşağıdaki adımları izleyin:

1. Azure portalda [Uygulama ayarları](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) sayfasına gidin.

1. **Çalışma zamanı** ayarlarında **Yığın** seçeneğini **Python 3.7** olarak ayarlayın ve komutu doğrudan **Başlangıç Dosyası** alanına girin.

    Alternatif olarak komutu projenizin kök dizinindeki bir metin dosyasına kaydedebilir, *startup.txt* (veya istediğiniz bir ad) gibi bir ad verebilirsiniz. Ardından bu dosyayı App Service'e dağıtın ve **Başlangıç Dosyası** alanında bu dosyanın adını belirtin. Bu seçenek, komutu Azure portal yerine kaynak kodu deponuzdan yönetmenizi sağlar.

1. **Kaydet**’i seçin. App Service otomatik olarak yeniden başlatılır ve birkaç saniye sonra özel başlangıç komutu uygulanır.

> [!Note]
> App Service, özel komut dosyasının işlenmesi sırasında oluşan hataları yoksayar ve başlatma işlemine Django ve Flask uygulamalarını arayarak devam eder. Beklediğiniz davranışı görmüyorsanız başlangıç dosyanızın App Service'e dağıtıldığından ve dosyada hata bulunmadığından emin olun.

### <a name="default-behavior"></a>Varsayılan davranış

App Service özel komut dosyası, Django uygulaması veya Flask uygulaması bulamazsa _opt/defaultsite_ klasöründe bulunan varsayılan salt okunur uygulamayı çalıştırır. Varsayılan uygulama aşağıdaki gibi görünür:

![Varsayılan Linux'ta App Service web sayfası](media/how-to-configure-python/default-python-app.png)

## <a name="troubleshooting"></a>Sorun giderme

- **Kendi uygulama kodunuzu dağıttıktan sonra varsayılan uygulamayı görüyorsunuz.**  Varsayılan uygulamanın görünmesinin nedeni, uygulama kodunuzu App Service'e dağıtmamış olmanız veya App Service'in uygulama kodunuzu bulamayıp varsayılan uygulamayı çalıştırmış olması olabilir.
  - App Service'i yeniden başlatın, 15-20 saniye bekleyin ve uygulamayı yeniden denetleyin.
  - SSH veya Kudu kullanarak doğrudan App Service'e bağlanın ve dosyalarınızın *site/wwwroot* dizininde bulunduğunu doğrulayın. Dosyalarınız orada değilse dağıtım işlemlerinizi gözden geçirin ve uygulamayı yeniden dağıtın.
  - Dosyalarınız oradaysa App Service başlangıç dosyanızı tanımlayamamış olabilir. Uygulamanızın App Service'in [Django](#django-app) veya [Flask](#flask-app) için beklediği şekilde yapılandırılmış olduğundan emin olun veya [özel başlangıç komutu](#custom-startup-command) kullanın.

- **Tarayıcıda "Hizmet Kullanılamıyor" iletisini görüyorsunuz.** Bu durum, tarayıcının App Service'ten yanıt beklerken zaman aşımına uğradığını gösterir. Bunun nedeni App Service'in Gunicorn sunucusunu başlatmış olması ancak uygulama kodunu belirten bağımsız değişkenlerin hatalı olmasıdır.
  - Özellikle App Service Planınızda en düşük fiyatlandırma katmanlarını kullanıyorsanız tarayıcıyı yenileyin. Ücretsiz katmanları kullandığınızda uygulamanın başlaması daha uzun sürebilir ve tarayıcıyı yenilediğinizde yanıt verebilir.
  - Uygulamanızın App Service'in [Django](#django-app) veya [Flask](#flask-app) için beklediği şekilde yapılandırılmış olduğundan emin olun veya [özel başlangıç komutu](#custom-startup-command) kullanın.
  - SSH veya Kudu kullanarak App Service'e bağlanın ve *LogFiles* klasöründe bulunan tanılama günlüklerini inceleyin. Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [Azure App Service’te web uygulamaları için tanılama günlüğünü etkinleştirme](../web-sites-enable-diagnostic-log.md).
