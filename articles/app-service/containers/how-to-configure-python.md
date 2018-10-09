---
title: Azure App Service'te yerleşik Python görüntüsü yapılandırma
description: Bu öğreticide Azure App Service'te yerleşik Python görüntüsüyle bir Python uygulaması yazma ve yapılandırma seçenekleri anlatılmaktadır.
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
ms.date: 09/25/2018
ms.author: astay;cephalin
ms.custom: mvc
ms.openlocfilehash: 9316805993b81e4d2511e833e0cc8f240807a1f9
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228569"
---
# <a name="configure-built-in-python-image-in-azure-app-service-preview"></a>Azure App Service'te yerleşik Python görüntüsü yapılandırma (Önizleme)

Bu makale, Python uygulamalarınızı çalıştırmak için [Linux üzerinde App Service'te](app-service-linux-intro.md) yerleşik Python görüntüsü yapılandırmayı göstermektedir.

## <a name="python-version"></a>Python sürümü

Linux üzerindeki App Service'teki Python çalışma zamanı `python-3.7.0` sürümünü kullanır.

## <a name="supported-frameworks"></a>Desteklenen çerçeveler

`python-3.7` çalışma zamanı ile uyumlu olan Web Sunucu Ağ Geçidi Arabirimi (WSGI) ile uyumlu tüm web çerçevesi sürümleri desteklenir.

## <a name="package-management"></a>Paket yönetimi

Git yayımlama sırasında, Kudu altyapısı depo kökünde [requirements.txt](https://pip.pypa.io/en/stable/user_guide/#requirements-files) dosyasını arar ve `pip` kullanarak paketleri Azure'a otomatik olarak yükler.

Yayımlama öncesinde bu dosyayı oluşturmak için depo köküne gidin ve Python ortamınızda şu komutu çalıştırın:

```bash
pip freeze > requirements.txt
```

## <a name="configure-your-python-app"></a>Python uygulamanızı yapılandırma

App Service'teki yerleşik Python görüntüsü, Python uygulamanızı çalıştırmak için [Gunicorn](http://gunicorn.org/) sunucusunu kullanır. Gunicorn UNIX için bir Python WSGI HTTP sunucusudur. App Service, Django ve Flask projeleri için Gunicorn'u otomatik olarak yapılandırır.

### <a name="django-app"></a>Django uygulaması

Bir `wsgi.py` modülü içeren bir Django projesini yayımlarsanız, Azure, şu komutu kullanarak otomatik olarak Gunicorn'u çağırır:

```bash
gunicorn <path_to_wsgi>
```

### <a name="flask-app"></a>Flask uygulaması

Bir Flask uygulaması yayımlıyorsanız ve giriş noktası bir `application.py` ya da `app.py` modülündeyse, Azure, sırasıyla şu komutları kullanarak otomatik olarak Gunicorn'u çağırır:

```bash
gunicorn application:app
```

Veya 

```bash
gunicorn app:app
```

### <a name="customize-start-up"></a>Başlatmayı özelleştirme

Uygulamanıza özel bir giriş noktası tanımlamak için önce özel bir Gunicorn komutuyla bir _.txt_ dosyası oluşturun ve dosyayı projenizin köküne yerleştirin. Örneğin sunucuyu _helloworld.py_ modülü ve `app` değişkeni ile başlatmak için içeriği aşağıdaki gibi olan bir _startup.txt_ dosyası oluşturun:

```bash
gunicorn helloworld:app
```

[Uygulama ayarları](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) sayfasında **Çalışma Zamanı Yığını** olarak **Python|3.7** sürümünü seçin ve önceki adımdaki **Başlatma Dosyanızın** adını girin. Örneğin _startup.txt_.
