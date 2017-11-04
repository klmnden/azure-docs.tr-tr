---
title: Azure uygulama hizmeti Linux SSS | Microsoft Docs
description: Azure uygulama hizmeti Linux SSS.
keywords: "Azure uygulama hizmeti, web uygulaması, SSS, linux, oss"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: cfowler
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: 38e771b8d7211e8f4f408a43b1ab2e293370ab9c
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="azure-app-service-on-linux-faq"></a>Azure uygulama hizmeti Linux SSS

Uygulama hizmeti Linux'ta sürümle birlikte, özellik ekleme ve bizim platformu geliştirmeler yapıyoruz çalışıyoruz. Bu makale, müşterilerimizin bize son isteyen olduğunu soruların yanıtlarını sağlar.

Varsa, makaleyi yorum sorularınız ve biz bunu mümkün olan en kısa sürede yanıt.

## <a name="built-in-images"></a>Yerleşik görüntüleri

**Platform sağlayan yerleşik Docker kapsayıcıları çatallaştırma istiyorsunuz. Bu dosyaları nereden bulabilirim?**

Tüm Docker dosyaları bulabilirsiniz [GitHub](https://github.com/azure-app-service). Tüm Docker kapsayıcıları bulabilirsiniz [Docker hub'a](https://hub.docker.com/u/appsvc/).

**Çalışma zamanı yığını yapılandırdığınızda, başlangıç dosyasını bölümü için beklenen değerler nelerdir?**

Node.js için PM2 yapılandırma dosyası veya komut dosyasını belirtin. .NET Core için derlenmiş DLL adınızı belirtin. Ruby için uygulamanızı başlatmak istediğiniz Söyleniş betik belirtebilirsiniz.

## <a name="management"></a>Yönetim

**Yeniden başlatma düğme Azure portalında bastığınızda ne olur?**

Bu eylem, Docker yeniden başlatma ile aynıdır.

**Uygulama kapsayıcı sanal makine (VM) bağlamak için güvenli Kabuk (SSH) kullanabilir miyim?**

Evet, bunu kaynak denetimi (SCM) yönetim sitesi üzerinden yapabilirsiniz.

**Bir Linux uygulama hizmeti planı bir SDK veya Azure Resource Manager şablonu aracılığıyla nasıl oluşturabilir miyim?**

Ayarlamak gereken **ayrılmış** app Service'e alanını *doğru*.

## <a name="continuous-integration-and-deployment"></a>Sürekli tümleştirme ve dağıtım

**Docker hub'a görüntüde güncelleştirilmiş sonra web Uygulamam eski Docker kapsayıcısı görüntüyü devam eder. Sürekli tümleştirme ve özel kapsayıcılar dağıtımını destekliyor musunuz?**

Azure kapsayıcı kayıt defteri veya DockerHub denetimi tarafından aşağıdaki makaleyi görüntülerini için sürekli tümleştirme/dağıtımı ayarlamak üzere [kapsayıcıları için Web uygulaması ile sürekli dağıtımın](./app-service-linux-ci-cd.md). Özel kayıt defterleri için kapsayıcı durdurma ve ardından web uygulamanızı başlatma yenileyebilirsiniz. Değiştirme veya, kapsayıcı yenilemeye zorlamak için sahte uygulama ayarı ekleyin.

**Hazırlama ortamlarını destekliyor musunuz?**

Evet.

**Kullanabilirim *web dağıtımı* web Uygulamam dağıtmak için?**

Evet, denilen bir uygulama ayarlamanız gerekir `WEBSITE_WEBDEPLOY_USE_SCM` için *false*.

## <a name="language-support"></a>Dil desteği

**Derlenmemiş .NET Core uygulamaları destekliyor musunuz?**

Evet.

**PHP uygulamaları için bir bağımlılık Yöneticisi olarak Oluşturucu destekliyor musunuz?**

Evet. Git dağıtımı sırasında Kudu (sayesinde dosyasının varlığı, yönetimin bir composer.lock) PHP uygulaması dağıtıyorsanız, Kudu sonra Oluşturucu yükleme sizin için tetikleyecek algılar ve.

## <a name="custom-containers"></a>Özel kapsayıcılar

**Kendi özel kapsayıcı kullanıyorum. Bir SMB paylaşımına bağlamak için platform istediğiniz `/home/` dizin.**

Bunu ayarlayarak yapabilirsiniz `WEBSITES_ENABLE_APP_SERVICE_STORAGE` uygulama ayarının *doğru* veya uygulama kaldırarak tamamen ayarlama. Platform depolama bir değişiklik olduğunda Bunun yapılması kapsayıcı yeniden neden olacak göz önünde bulundurun. 

>[!NOTE]
>Varsa `WEBSITES_ENABLE_APP_SERVICE_STORAGE` ayar *false*, `/home/` dizin paylaşılmayan ölçek örneklerinde ve orada yazılan dosyaları kalıcı hale getirilemedi yeniden başlatmaları arasındaki.

**My özel kapsayıcı Başlat uzun süren ve başlatma tamamlanmadan önce platform kapsayıcıyı yeniden başlatır.**

Platform, kapsayıcı yeniden başlatılmadan önce bekleyeceği süreyi yapılandırabilirsiniz. Bunu yapmak için ayarlanmış `WEBSITES_CONTAINER_START_TIME_LIMIT` uygulama ayarının bulmak istediğiniz değer. Varsayılan değer 230 saniyedir ve en büyük değer 600 saniyedir.

**Özel kayıt defteri sunucu URL'SİNİN biçimi nedir?**

Tam kayıt defteri URL'sini sağlamanız dahil olmak üzere `http://` veya `https://`.

**Özel kayıt defteri seçeneğinde görüntü adı biçimi nedir?**

Özel kayıt defteri URL (örneğin, myacr.azurecr.io/dotnet:latest) dahil olmak üzere tam görüntüyü adını ekleyin. Görüntü özel bir bağlantı noktası adları [Portalı aracılığıyla girilemez](https://feedback.azure.com/forums/169385-web-apps/suggestions/31304650). Ayarlamak için `docker-custom-image-name`, kullanın [ `az` komut satırı aracı](https://docs.microsoft.com/cli/azure/webapp/config/container?view=azure-cli-latest#az_webapp_config_container_set).

**My özel kapsayıcı görüntüde birden fazla bağlantı noktası kullanıma?**

Şu anda birden fazla bağlantı noktası gösterme desteklemiyoruz.

**Kendi depolama getirebilir miyim?**

Şu anda kendi depolama getiren desteklemiyoruz.

**SCM sitesinden my özel kapsayıcının dosya sistemi veya çalışan işlemleri neden öğesine göz atamıyor?**

SCM site ayrı bir kapsayıcıda çalışır. Uygulama kapsayıcısı dosya sistemi veya çalışan işlemlerinin denetlenemiyor.

**Bağlantı noktası 80 dışında bir bağlantı noktası için My özel kapsayıcı dinler. Uygulamam istekleri bu bağlantı noktasına yönlendirmek için nasıl yapılandırabilir miyim?**

Otomatik olarak bağlantı noktası algılama sunuyoruz. Denilen bir uygulama da belirtebilirsiniz *WEBSITES_PORT* ve beklenen bağlantı noktası numarası değerini verin. Daha önce kullanılan platform *bağlantı noktası* uygulama ayarı. Biz bu uygulama ayarı Kaldır ve kullanmak için planlama *WEBSITES_PORT* özel olarak.

**My özel kapsayıcısında HTTPS uygulamak gerekiyor mu?**

Hayır, platform paylaşılan ön uçlar HTTPS sonlandırmanın işler.

## <a name="pricing-and-sla"></a>Fiyatlandırma ve SLA

**Hizmeti genel olarak kullanılabilir olduğunda, fiyatlandırma nedir?**

Uygulamanızın çalıştırdığı saat sayısı için normal Azure uygulama hizmeti fiyatlandırması sizden ücret kesilir.

## <a name="other-questions"></a>Diğer sorular

**Uygulama ayarları adları desteklenen karakter nelerdir?**

Uygulama ayarları için yalnızca harf (A-Z, a-z), sayılar (0-9) ve alt çizgi karakteri (_) kullanabilirsiniz.

**Yeni özellikler burada isteyebilir miyim?**

Bir fikir gönderme [Web Apps geri bildirim Forumunda](https://aka.ms/webapps-uservoice). "[Linux]" ilişkin düşünceniz başlık ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Linux üzerinde Azure App Service nedir?](app-service-linux-intro.md)
* [Hazırlık Azure App Service ortamları ayarlama](../../app-service/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Web uygulaması ile sürekli dağıtımın kapsayıcıları için](./app-service-linux-ci-cd.md)
