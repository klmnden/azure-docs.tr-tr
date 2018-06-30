---
title: Azure uygulama hizmeti Linux SSS | Microsoft Docs
description: Azure uygulama hizmeti Linux SSS.
keywords: Azure uygulama hizmeti, web uygulaması, SSS, linux, oss, kapsayıcıları, birden çok kapsayıcı, multicontainer için web uygulaması
services: app-service
documentationCenter: ''
author: yili
manager: apurvajo
editor: ''
ms.assetid: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: yili
ms.openlocfilehash: a35f3d428674c3398497cd43465e0bd501f5c3fc
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131501"
---
# <a name="azure-app-service-on-linux-faq"></a>Azure uygulama hizmeti Linux SSS

Uygulama hizmeti Linux'ta sürümle birlikte, özellik ekleme ve bizim platformu geliştirmeler yapıyoruz çalışıyoruz. Bu makale, müşterilerimizin bize son isteyen olduğunu soruların yanıtlarını sağlar.

Bir sorunuz varsa, bu makalede açıklama.

## <a name="built-in-images"></a>Yerleşik görüntüleri

**Platform sağlayan yerleşik Docker kapsayıcıları çatallaştırma istiyorsunuz. Bu dosyaları nereden bulabilirim?**

Tüm Docker dosyaları bulabilirsiniz [GitHub](https://github.com/azure-app-service). Tüm Docker kapsayıcıları bulabilirsiniz [Docker hub'a](https://hub.docker.com/u/appsvc/).

**Çalışma zamanı yığını yapılandırdığınızda, başlangıç dosyasını bölümü için beklenen değerler nelerdir?**

Node.js için PM2 yapılandırma dosyası veya komut dosyasını belirtin. .NET Core için derlenmiş DLL adınızı belirtmek `dotnet <myapp>.dll`. Ruby için uygulamanızı başlatmak istediğiniz Söyleniş betik belirtebilirsiniz.

## <a name="management"></a>Yönetim

**Yeniden başlatma düğme Azure portalında bastığınızda ne olur?**

Bu eylem, Docker yeniden başlatma ile aynıdır.

**Uygulama kapsayıcı sanal makine (VM) bağlamak için güvenli Kabuk (SSH) kullanabilir miyim?**

Evet, bunu kaynak denetimi (SCM) yönetim sitesi üzerinden yapabilirsiniz.

> [!NOTE]
> SSH, SFTP veya Visual Studio Code (Node.js apps canlı hata ayıklaması için) kullanarak doğrudan yerel geliştirme makinenizden de uygulama kapsayıcısına bağlanabilirsiniz. Daha fazla bilgi için bkz. [Linux üzerinde App Service’te uzaktan hata ayıklama ve SSH](https://aka.ms/linux-debug).
>

**Bir Linux uygulama hizmeti planı bir SDK veya Azure Resource Manager şablonu aracılığıyla nasıl oluşturabilir miyim?**

Ayarlamanız gerekir **ayrılmış** app Service'e alanını *doğru*.

## <a name="continuous-integration-and-deployment"></a>Sürekli tümleştirme ve dağıtım

**Docker hub'a görüntüde güncelleştirilmiş sonra web Uygulamam eski Docker kapsayıcısı görüntüyü devam eder. Sürekli tümleştirme ve özel kapsayıcılar dağıtımını destekliyor musunuz?**

Sürekli Tümleştirme/dağıtım Azure kapsayıcı kayıt defteri veya DockerHub, aşağıdaki tarafından belirlenen için Evet'i, [kapsayıcıları için Web uygulaması ile sürekli dağıtımın](./app-service-linux-ci-cd.md). Özel kayıt defterleri için kapsayıcı durdurma ve ardından web uygulamanızı başlatma yenileyebilirsiniz. Değiştirme veya, kapsayıcı yenilemeye zorlamak için sahte uygulama ayarı ekleyin.

**Hazırlama ortamlarını destekliyor musunuz?**

Evet.

**Kullanabilirim *web dağıtımı* web Uygulamam dağıtmak için?**

Evet, denilen bir uygulama ayarlamanız gerekir `WEBSITE_WEBDEPLOY_USE_SCM` için *false*.

**Linux web uygulaması kullanılırken uygulamamın Git dağıtımı başarısız oluyor. Sorunun geçici nasıl çalışabilir mi?**

Linux web uygulamanızı Git dağıtımı başarısız olursa, uygulama kodunuzun dağıtmak için aşağıdaki seçeneklerden birini seçin:

- Kesintisiz teslim (Önizleme) özelliğini kullanın: bir Team Services Git deposuna ya da Azure kesintisiz teslim kullanmak için GitHub deposuna uygulamanızın kaynak kodunu depolayabilirsiniz. Daha fazla bilgi için bkz: [Linux web uygulaması için sürekli teslim yapılandırma](https://blogs.msdn.microsoft.com/devops/2017/05/10/use-azure-portal-to-setup-continuous-delivery-for-web-app-on-linux/).

- Kullanmak [ZIP dağıtmak API](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file): Bu API kullanmak için [web uygulamanızı içine SSH](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-ssh-support#making-a-client-connection) ve kodunuzu dağıtmak istediğiniz klasörüne gidin. Aşağıdaki kodu çalıştırın:

   ```bash
   curl -X POST -u <user> --data-binary @<zipfile> https://{your-sitename}.scm.azurewebsites.net/api/zipdeploy
   ```

   Bir hata alırsanız `curl` komutu bulunamadı, kullanarak curl yüklediğinizden emin olun `apt-get install curl` önceki çalıştırmadan önce `curl` komutu.

## <a name="language-support"></a>Dil desteği

**Web yuvalarını Node.js Uygulamam, tüm özel ayarlarını veya yapılandırmaları ayarlamak için kullanmak istediğiniz?**

Evet, devre dışı `perMessageDeflate` , sunucu tarafı Node.js kodunuzda. Örneğin, Socket.IO kullanıyorsanız, aşağıdaki kodu kullanın:

```nodejs
var io = require('socket.io')(server,{
  perMessageDeflate :false
});
```

**Derlenmemiş .NET Core uygulamaları destekliyor musunuz?**

Evet.

**PHP uygulamaları için bir bağımlılık Yöneticisi olarak Oluşturucu destekliyor musunuz?**

Evet, Git dağıtımı sırasında Kudu (sayesinde dosyasının varlığı, yönetimin bir composer.lock) PHP uygulaması dağıtıyorsanız ve Kudu sonra Oluşturucu yükleme tetikleyecek algılaması gerekir.

## <a name="custom-containers"></a>Özel kapsayıcılar

**Kendi özel kapsayıcı kullanıyorum. Bir SMB paylaşımına bağlamak için platform istediğiniz `/home/` dizin.**

Bunu ayarlayarak yapabilirsiniz `WEBSITES_ENABLE_APP_SERVICE_STORAGE` uygulama ayarının *doğru*. Platform depolama bir değişiklik olduğunda bu kapsayıcı yeniden oluşturacağını aklınızda bulundurun.

>[!NOTE]
>Varsa `WEBSITES_ENABLE_APP_SERVICE_STORAGE` ayarı belirtilmemiş veya kümesine *false*, `/home/` dizin paylaşılmayan ölçek örneklerinde ve orada yazılan dosyaları kalıcı hale getirilemedi yeniden başlatmaları arasındaki.

**My özel kapsayıcı Başlat uzun süren ve başlatma tamamlanmadan önce platform kapsayıcıyı yeniden başlatır.**

Platform, kapsayıcı yeniden başlatılmadan önce bekleyeceği süreyi yapılandırabilirsiniz. Bunu yapmak için ayarlanmış `WEBSITES_CONTAINER_START_TIME_LIMIT` uygulama ayarının bulmak istediğiniz değer. Varsayılan değer 230 saniyedir ve en büyük değer 1800 saniyedir.

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

## <a name="multi-container-with-docker-compose-and-kubernetes"></a>Docker ile birden çok kapsayıcı oluşturun ve Kubernetes

**Azure kapsayıcı kayıt defteri (ACR), birden çok kapsayıcı ile kullanmak için nasıl yapılandırırım?**

ACR çok kapsayıcı ile kullanmak için **tüm kapsayıcı görüntüleri** aynı ACR kayıt defteri sunucu üzerinde barındırılması gerekir. Aynı kayıt defteri sunucuda olduktan sonra uygulama ayarları oluşturmak ve ardından ACR görüntü adı eklemek için Docker Compose veya Kubernetes yapılandırma dosyasını güncelleştirmek gerekir.

Aşağıdaki uygulama ayarları oluşturun:

- DOCKER_REGISTRY_SERVER_USERNAME
- DOCKER_REGISTRY_SERVER_URL (tam URL, örn: https://<server-name>.azurecr.io)
- DOCKER_REGISTRY_SERVER_PASSWORD (ACR Ayarları'nda yönetici erişimi etkinleştir)

Yapılandırma dosyasında aşağıdaki örneğe benzer ACR görüntünüzü başvuru:

```yaml
image: <server-name>.azurecr.io/<image-name>:<tag>
```

**Nasıl Internet erişilebilir kapsayıcıdır biliyor musunuz?**

- Yalnızca bir kapsayıcı erişimi açık olabilir.
- Yalnızca bağlantı noktası 80 ve 8080 erişilebilir (gösterilen bağlantı noktaları) değil

Hangi kapsayıcı öncelik sırasına göre - erişilebilen belirlemek için kurallar şunlardır:

- Uygulama ayarı `WEBSITES_WEB_CONTAINER_NAME` kapsayıcı adına ayarlayın
- Bağlantı noktası 80 veya 8080 tanımlamak için ilk kapsayıcı
- Yukarıdakilerin hiçbiri true ise, dosyasında tanımlanan ilk kapsayıcı erişilebilir (açık)

## <a name="pricing-and-sla"></a>Fiyatlandırma ve SLA

**Hizmeti genel olarak kullanılabilir olduğunda, fiyatlandırma nedir?**

Uygulamanızın çalıştırdığı saat sayısı için normal Azure uygulama hizmeti fiyatlandırması sizden ücret kesilir.

## <a name="other-questions"></a>Diğer sorular

**Uygulama ayarları adları desteklenen karakter nelerdir?**

Uygulama ayarları için yalnızca harf (A-Z, a-z), sayılar (0-9) ve alt çizgi karakteri (_) kullanabilirsiniz.

**Yeni özellikler burada isteyebilir miyim?**

Bir fikir gönderme [Web Apps geri bildirim Forumunda](https://aka.ms/webapps-uservoice). "[Linux]" ilişkin düşünceniz başlık ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Linux üzerinde Azure App Service nedir?](app-service-linux-intro.md)
- [Azure App Service’te hazırlık ortamları ayarlama](../../app-service/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Web uygulaması ile sürekli dağıtımın kapsayıcıları için](./app-service-linux-ci-cd.md)
