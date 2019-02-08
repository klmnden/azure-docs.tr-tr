---
title: Python uygulamaları - Azure App Service'ı yapılandırma
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
ms.date: 01/29/2019
ms.author: astay;cephalin;kraigb
ms.custom: seodec18
ms.openlocfilehash: 6965379aadefd110ce6e46e105bbde10626b63c1
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55892176"
---
# <a name="configure-your-python-app-for-azure-app-service"></a>Python uygulamanızı Azure App Service için yapılandırma
Bu makalede nasıl [Azure App Service](app-service-linux-intro.md) Python uygulamaları ve gerektiğinde App Service'in davranışını nasıl özelleştirebileceğiniz çalışır. Python uygulamaları tüm ile dağıtılması gerekir gerekli [pip](https://pypi.org/project/pip/) modüller. App Service dağıtım Altyapısı'nı (Kudu) otomatik olarak sanal ortam etkinleştirir ve çalışan `pip install -r requirements.txt` dağıttığınızda sizin için bir [Git deposu](../deploy-local-git.md), veya bir [Zip paketini](../deploy-zip.md) yapı işlemleri ile açık.

> [!NOTE]
> [App Service'in Windows flavor Python'u](https://docs.microsoft.com/visualstudio/python/managing-python-on-azure-app-service) kullanım dışıdır ve kullanım için önerilmez.
>

## <a name="show-python-version"></a>Python sürümü göster

Geçerli Python sürümü göstermek için aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource_group_name> --name <app_name> --query linuxFxVersion
```

Tüm desteklenen Python sürümleri göstermek için aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep PYTHON
```

Python'ın desteklenmeyen bir sürümünü, bunun yerine kendi kapsayıcı görüntünüzü oluşturarak çalıştırabilirsiniz. Daha fazla bilgi için [özel bir Docker görüntüsü kullanma](tutorial-custom-docker-image.md).

## <a name="set-python-version"></a>Python sürümünü ayarlama

Aşağıdaki komutu çalıştırın [Cloud Shell](https://shell.azure.com) -3.7 Python sürümünü ayarlamak için:

```azurecli-interactive
az webapp config set --resource-group <group_name> --name <app_name> --linux-fx-version "PYTHON|3.7"
```

## <a name="container-characteristics"></a>Kapsayıcı özellikleri

Dağıtılan GitHub deposunda tanımlanan bir Docker kapsayıcısı içinde çalıştırmak için Linux üzerinde App Service'e Python uygulamaları [Python 3.6](https://github.com/Azure-App-Service/python/tree/master/3.6.6) veya [Python 3.7](https://github.com/Azure-App-Service/python/tree/master/3.7.0).
Bu kapsayıcı aşağıdaki özelliklere sahiptir:

- Uygulamalar ek `--bind=0.0.0.0 --timeout 600` bağımsız değişkenleri kullanılarak [Gunicorn WSGI HTTP Server](https://gunicorn.org/) ile çalıştırılır.
- Temel görüntü varsayılan olarak Flask web çerçevesini içerir ancak kapsayıcı Django gibi WSGI ve Python 3.7 ile uyumlu diğer çerçeveleri de destekler.
- Django gibi ek paketleri yüklemek için `pip freeze > requirements.txt` kullanarak projenizin kök dizininde bir [*requirements.txt*](https://pip.pypa.io/en/stable/user_guide/#requirements-files) dosyası oluşturun. Ardından projenizi Git dağıtımı kullanarak App Service'te yayımlayın. Bunu yaptığınızda uygulamanızın bağımlılıklarının yüklenmesi için kapsayıcıda otomatik olarak `pip install -r requirements.txt` çalıştırılır.

## <a name="container-startup-process"></a>Kapsayıcı başlatma işlemi

Başlatma sırasında Linux'ta App Service kapsayıcısı şu adımları çalıştırır:

1. Kullanım bir [özel bir başlangıç komutu](#customize-startup-command), sağlanan.
2. Varlığını denetleyin bir [Django uygulaması](#django-app)ve algılanırsa Gunicorn için başlatın.
3. Varlığını denetleyin bir [Flask uygulaması](#flask-app)ve algılanırsa Gunicorn için başlatın.
4. Başka bir uygulama bulunamazsa yoksa kapsayıcıda bulunan varsayılan uygulamayı başlatır.

Aşağıdaki bölümlerde bu seçeneklerle ilgili ek ayrıntılar verilmiştir.

### <a name="django-app"></a>Django uygulaması

Django uygulamaları için App Service uygulama kodunuzda `wsgi.py` adlı bir dosya olup olmadığını denetler ve ardından şu komutu kullanarak Gunicorn'u çalıştırır:

```bash
# <module> is the path to the folder that contains wsgi.py
gunicorn --bind=0.0.0.0 --timeout 600 <module>.wsgi
```

Başlangıç komutu üzerinde ayrıntılı denetim istiyorsanız, bir özel başlatma komutunu kullanın ve Değiştir `<module>` içeren modül adıyla *wsgi.py*.

### <a name="flask-app"></a>Flask uygulaması

Flask için App Service için adlı bir dosya arar *application.py* veya *app.py* ve Gunicorn şu şekilde başlar:

```bash
# If application.py
gunicorn --bind=0.0.0.0 --timeout 600 application:app
# If app.py
gunicorn --bind=0.0.0.0 --timeout 600 app:app
```

Ana uygulama modülünüzde farklı bir dosya içeriyorsa, uygulama nesnesinin farklı bir ad kullanın veya istediğiniz ek bağımsız değişkenler için Gunicorn sağlamak için bir özel başlatma komutunu kullanın.

### <a name="default-behavior"></a>Varsayılan davranış

App Service özel komut dosyası, Django uygulaması veya Flask uygulaması bulamazsa _opt/defaultsite_ klasöründe bulunan varsayılan salt okunur uygulamayı çalıştırır. Varsayılan uygulama aşağıdaki gibi görünür:

![Varsayılan Linux'ta App Service web sayfası](media/how-to-configure-python/default-python-app.png)

## <a name="customize-startup-command"></a>Başlangıç komutu özelleştirme

Özel bir Gunicorn başlangıç komutu sağlayarak kapsayıcının başlangıç davranışını denetleyebilirsiniz. Örneğin ana modülünün adı *hello.py* ve o dosyadaki Flask uygulama nesnesinin adı da `myapp` olan bir Flask uygulamanız varsa kullanmanız gereken komut şöyle olacaktır:

```bash
gunicorn --bind=0.0.0.0 --timeout 600 hello:myapp
```

Ana modülünüz `website` gibi bir alt klasör ise bu klasörü `--chdir` bağımsız değişkeniyle belirtin:

```bash
gunicorn --bind=0.0.0.0 --timeout 600 --chdir website hello:myapp
```

Komuta Gunicorn için ek bağımsız değişkenler de ekleyebilirsiniz, örneğin: `--workers=4`. Daha fazla bilgi için bkz. [Gunicorn'u Çalıştırma](https://docs.gunicorn.org/en/stable/run.html) (docs.gunicorn.org).

Bir Gunicorn olmayan sunucu gibi kullanılacak [aiohttp](https://aiohttp.readthedocs.io/en/stable/web_quickstart.html), çalıştırabilirsiniz:

```bash
python3.7 -m aiohttp.web -H localhost -P 8080 package.module:init_func
```

Özel komut sağlamak için aşağıdaki adımları izleyin:

1. Azure portalda [Uygulama ayarları](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) sayfasına gidin.
1. **Çalışma zamanı** ayarlarında **Yığın** seçeneğini **Python 3.7** olarak ayarlayın ve komutu doğrudan **Başlangıç Dosyası** alanına girin.
Alternatif olarak, komut, projenizin kökünde bir metin dosyasındaki gibi bir adı kullanarak kaydedebileceğiniz *startup.txt* (veya istediğiniz herhangi bir ad). Ardından bu dosyayı App Service'e dağıtın ve **Başlangıç Dosyası** alanında bu dosyanın adını belirtin. Bu seçenek, komutu Azure portal yerine kaynak kodu deponuzdan yönetmenizi sağlar.
1. **Kaydet**’i seçin. App Service otomatik olarak yeniden başlatılır ve birkaç saniye sonra özel başlangıç komutu uygulanır.

> [!Note]
> App Service, özel komut dosyasının işlenmesi sırasında oluşan hataları yoksayar ve başlatma işlemine Django ve Flask uygulamalarını arayarak devam eder. Beklediğiniz davranışı görmüyorsanız başlangıç dosyanızın App Service'e dağıtıldığından ve dosyada hata bulunmadığından emin olun.

## <a name="access-environment-variables"></a>Ortam değişkenlerine erişme

App Service uygulama ayarları, uygulama kodunuz dışında ayarlayabilirsiniz (bkz [ortam değişkenlerini ayarlama](../web-sites-configure.md)). Standardını kullanarak bunlar erişebilir [os.environ](https://docs.python.org/3/library/os.html#os.environ) deseni. Örneğin, bir uygulama ayarı erişmeye adlı `WEBSITE_SITE_NAME`, aşağıdaki kodu kullanın:

```python
os.environ['WEBSITE_SITE_NAME']
```

## <a name="detect-https-session"></a>HTTPS oturumu algılayın

App Service'te [SSL sonlandırma](https://wikipedia.org/wiki/TLS_termination_proxy) tüm HTTPS isteklerini, şifrelenmemiş HTTP istekleri olarak uygulamanızı ulaşmak için Ağ Yük Dengeleyiciler, olur. Uygulama mantığı ihtiyaçlarınızı veya değil, kullanıcı isteklerini şifreli olup olmadığı denetlenecek incelemek, `X-Forwarded-Proto` başlığı.

```python
if 'X-Forwarded-Proto' in request.headers and request.headers['X-Forwarded-Proto'] == 'https':
# Do something when HTTPS is used
```

Popüler web çerçeveleri erişmenizi `X-Forwarded-*` bilgileri, standart uygulama deseni. İçinde [CodeIgniter](https://codeigniter.com/), [is_https()](https://github.com/bcit-ci/CodeIgniter/blob/master/system/core/Common.php#L338-L365) değerini denetler `X_FORWARDED_PROTO` varsayılan olarak.

## <a name="troubleshooting"></a>Sorun giderme

- **Kendi uygulama kodunuzu dağıttıktan sonra varsayılan uygulamayı görüyorsunuz.** Varsayılan uygulama ya da App Service, uygulama kodunuz dağıtılan henüz veya App Service, uygulama kodunuz bulunamadı ve bunun yerine varsayılan uygulamayı çalıştırdığınız için görünür.
- App Service'i yeniden başlatın, 15-20 saniye bekleyin ve uygulamayı yeniden denetleyin.
- Windows tabanlı örnek yerine Linux için App Service’i kullandığınızdan emin olun. Azure CLI’de `<resource_group_name>` ve `<app_service_name>` hizmetini uygun bir şekilde değiştiren `az webapp show --resource-group <resource_group_name> --name <app_service_name> --query kind` komutunu çalıştırın. Çıktı olarak `app,linux` görünmelidir, aksi takdirde App Service’i yeniden oluşturun ve Linux’u seçin.
- SSH veya Kudu kullanarak doğrudan App Service'e bağlanın ve dosyalarınızın *site/wwwroot* dizininde bulunduğunu doğrulayın. Dosyalarınız orada değilse dağıtım işlemlerinizi gözden geçirin ve uygulamayı yeniden dağıtın.
- Dosyalarınız oradaysa App Service başlangıç dosyanızı tanımlayamamış olabilir. Uygulamanızı App Service için bekliyor olarak yapılandırıldığını denetleyin [Django](#django-app) veya [Flask](#flask-app), veya bir özel başlatma komutunu kullanın.
- **Tarayıcıda "Hizmet Kullanılamıyor" iletisini görüyorsunuz.** Bu durum, tarayıcının App Service'ten yanıt beklerken zaman aşımına uğradığını gösterir. Bunun nedeni App Service'in Gunicorn sunucusunu başlatmış olması ancak uygulama kodunu belirten bağımsız değişkenlerin hatalı olmasıdır.
- Özellikle App Service Planınızda en düşük fiyatlandırma katmanlarını kullanıyorsanız tarayıcıyı yenileyin. Ücretsiz katmanları kullandığınızda uygulamanın başlaması daha uzun sürebilir ve tarayıcıyı yenilediğinizde yanıt verebilir.
- Uygulamanızın App Service'in [Django](#django-app) veya [Flask](#flask-app) için beklediği şekilde yapılandırılmış olduğundan emin olun veya [özel başlangıç komutu](#customize-startup-command) kullanın.
- SSH veya Kudu kullanarak App Service'e bağlanın ve *LogFiles* klasöründe bulunan tanılama günlüklerini inceleyin. Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [Azure App Service’te web uygulamaları için tanılama günlüğünü etkinleştirme](../troubleshoot-diagnostic-logs.md).
