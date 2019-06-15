---
title: SSS - Azure Linux'ta App Service | Microsoft Docs
description: SSS Linux'ta Azure App Service.
keywords: Azure app service, web uygulaması, SSS, linux, oss, kapsayıcılar, çok kapsayıcılı multicontainer için web app
services: app-service
documentationCenter: ''
author: yili
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: yili
ms.custom: seodec18
ms.openlocfilehash: dbf63ff47b11c2e75966b4a4b91fb1b00b40d216
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65594270"
---
# <a name="azure-app-service-on-linux-faq"></a>Azure App Service Linux SSS hakkında

Linux'ta App Service'nın yayınlanmasıyla birlikte, özellik ekleme ve platformumuz için geliştirmeler yapmayı üzerinde çalışıyoruz. Bu makalede, müşterilerimizin bize son istediği, soruların yanıtlarını sağlar.

Herhangi bir sorunuz varsa, bu makalede yorum.

## <a name="built-in-images"></a>Yerleşik görüntüleri

**Platformu sağlayan yerleşik Docker kapsayıcılarını çatal istiyorsunuz. Bu dosyaların nerede bulabilirim?**

Tüm Docker dosyaları bulabilirsiniz [GitHub](https://github.com/azure-app-service). Tüm Docker kapsayıcılarını bulabilirsiniz [Docker Hub](https://hub.docker.com/u/appsvc/).

<a id="#startup-file"></a>

**Çalışma zamanı yığını yapılandırabilirim, başlangıç dosyasını bölümü için beklenen değerler nelerdir?**

| Yığın           | Beklenen değer                                                                         |
|-----------------|----------------------------------------------------------------------------------------|
| Java SE         | JAR uygulamanızı başlatmak için komutu (örneğin, `java -jar my-app.jar --server.port=80`) |
| Tomcat, Wildfly | Tüm gerekli yapılandırmaları gerçekleştirmek için bir komut dosyasının konumunu (örneğin, `/home/site/deployments/tools/startup_script.sh`)          |
| Node.js         | PM2 yapılandırma dosyasının veya komut dosyanızı                                |
| .Net Core       | olarak derlenen DLL'nin adıdır `dotnet <myapp>.dll`                                 |
| Ruby            | uygulamanızı başlatmak istediğiniz Ruby betiğini                     |

Bu komutları veya betikleri yerleşik Docker kapsayıcısında başlatıldı, ancak önce uygulamanızın kod başlatıldığında sonra yürütülür.

## <a name="management"></a>Yönetim

**Yeniden Başlat düğmesi Azure portalında bastığınızda ne olur?**

Bu eylem, Docker yeniden başlatma ile aynıdır.

**Uygulama kapsayıcı sanal makineye (VM) bağlamak için güvenli Kabuk (SSH) kullanabilir miyim?**

Evet, kaynak denetim Yönetimi (SCM) sitesi üzerinden bunu yapabilirsiniz.

> [!NOTE]
> SSH, SFTP veya Visual Studio Code (Node.js apps canlı hata ayıklaması için) kullanarak doğrudan yerel geliştirme makinenizden de uygulama kapsayıcısına bağlanabilirsiniz. Daha fazla bilgi için bkz. [Linux üzerinde App Service’te uzaktan hata ayıklama ve SSH](https://aka.ms/linux-debug).
>

**Bir Linux App Service planında bir SDK veya Azure Resource Manager şablonu aracılığıyla nasıl oluşturabilirim?**

Ayarlamalısınız **ayrılmış** alanını adlı app service ile *true*.

## <a name="continuous-integration-and-deployment"></a>Sürekli tümleştirme ve dağıtım

**Ben görüntü Docker Hub üzerinde güncelleştirdikten sonra web uygulamamı hala eski bir Docker kapsayıcı görüntüsü kullanır. Sürekli tümleştirme ve dağıtım özel kapsayıcılar destekliyorsunuz?**

Evet, sürekli tümleştirme/dağıtım için Azure Container Registry veya DockerHub, aşağıdaki ayarlanacak [kapsayıcılar için Web App ile sürekli dağıtım](./app-service-linux-ci-cd.md). Özel kayıt defterleri için kapsayıcıyı durdurmak ve ardından web uygulamanızı başlatma yenileyebilirsiniz. Değiştirme veya kapsayıcınızı yenilemeye zorlamak için bir işlevsiz uygulama ayarı ekleyin.

**Hazırlama ortamları destekliyorsunuz?**

Evet.

**Kullanabileceğim *WebDeploy/MSDeploy* web uygulamamı dağıtmak için?**

Evet, çağrılan ayarlama uygulama ayarlamanız gerekir `WEBSITE_WEBDEPLOY_USE_SCM` için *false*.

**Git dağıtımı uygulamamın Linux web uygulaması kullanırken başarısız olur. Sorunun geçici çözümü nasıl çalışabilir mi?**

Linux web uygulamanızı Git dağıtımı başarısız olursa uygulama kodunuzu dağıtmak için aşağıdaki seçeneklerden birini seçin:

- Sürekli teslim (Önizleme) özelliğini kullanın: Uygulamanızın kaynak kodu bir Azure DevOps Git deposu veya Azure sürekli teslim için GitHub deposunu depolayabilirsiniz. Daha fazla bilgi için [Linux web uygulaması için sürekli teslimi yapılandırmak nasıl](https://blogs.msdn.microsoft.com/devops/2017/05/10/use-azure-portal-to-setup-continuous-delivery-for-web-app-on-linux/).

- Kullanım [ZIP dağıtma API](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file): Bu API kullanmak üzere [SSH web uygulamanıza](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-ssh-support) ve kodunuzu dağıtmak istediğiniz klasöre gidin. Aşağıdaki kodu çalıştırın:

   ```bash
   curl -X POST -u <user> --data-binary @<zipfile> https://{your-sitename}.scm.azurewebsites.net/api/zipdeploy
   ```

   Bir hata alırsanız `curl` komutu bulunamazsa, kullanarak curl yüklediğinizden emin olun `apt-get install curl` önceki çalıştırmadan önce `curl` komutu.

## <a name="language-support"></a>Dil desteği

**Web yuvaları Node.js Uygulamam, tüm özel ayarlarını veya yapılandırmaları ayarlamak için kullanmak istiyorsunuz?**

Evet, devre dışı `perMessageDeflate` sunucu tarafı Node.js kod. Örneğin, socket.io kullanıyorsanız, aşağıdaki kodu kullanın:

```nodejs
const io = require('socket.io')(server,{
  perMessageDeflate :false
});
```

**.NET Core uygulamaları derlenmemiş destekliyorsunuz?**

Evet.

**PHP uygulamaları için bir bağımlılık Yöneticisi olarak Oluşturucusu destekliyorsunuz?**

Evet, Git dağıtımı sırasında bir PHP uygulaması (sayesinde composer.lock dosyasının varlığını) dağıtıyorsanız ve Kudu ardından Oluşturucusu yükleme tetikleyecek Kudu algılanmalıdır.

## <a name="custom-containers"></a>Özel kapsayıcılar

**Kendi özel kapsayıcınızı kullanıyorum. Platform için bir SMB paylaşımını bağlaması istiyorum `/home/` dizin.**

Ayarlayarak bunu yapabilirsiniz `WEBSITES_ENABLE_APP_SERVICE_STORAGE` uygulama ayarının *true*. Platform depolama bir değişiklik olduğunda bu kapsayıcı yeniden neden olacağını aklınızda bulundurun.

>[!NOTE]
>Varsa `WEBSITES_ENABLE_APP_SERVICE_STORAGE` ayarı belirtilmemiş veya kümesine *false*, `/home/` dizin ölçek örneklerinde değil paylaşılır ve orada yazılır dosyaları kalıcı yeniden başlatmaları arasındaki.

**My özel kapsayıcı başlatmak uzun sürüyor ve platform, başlatma tamamlanmadan önce kapsayıcıyı yeniden başlatır.**

Platform kapsayıcınızı başlatılmadan önce bekleyeceği süreyi yapılandırabilirsiniz. Bunu yapmak için ayarlanmış `WEBSITES_CONTAINER_START_TIME_LIMIT` uygulama ayarı için değer. Varsayılan değer 230 saniyedir ve 1800 saniye en yüksek değerdir.

**Özel kayıt defteri sunucusu URL'si biçimi nedir?**

Tam kayıt defteri URL'si sağlamak da dahil olmak üzere `http://` veya `https://`.

**Özel kayıt defteri seçeneği görüntü adı için biçimi nedir?**

Özel kayıt defteri URL'si (örneğin, myacr.azurecr.io/dotnet:latest) dahil olmak üzere tam görüntü adını ekleyin. Görüntü özel bir bağlantı noktası adları [portal üzerinden girilemez](https://feedback.azure.com/forums/169385-web-apps/suggestions/31304650). Ayarlanacak `docker-custom-image-name`, kullanın [ `az` komut satırı aracı](https://docs.microsoft.com/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set).

**Birden fazla bağlantı noktası özel kapsayıcı Görüntümü kullanıma sunabilirsiniz?**

Birden fazla bağlantı noktası gösterme desteklemiyoruz.

**Ben, kendi depolama getirebilir miyim?**

Evet, [kendi depolamanı Getir](https://docs.microsoft.com/azure/app-service/containers/how-to-serve-content-from-azure-storage) Önizleme aşamasındadır.

**My özel kapsayıcının dosya sistemi veya çalışan işlemlerden SCM sitesine neden öğesine göz atamıyor?**

SCM sitesine ayrı bir kapsayıcıda çalışır. Dosya sistemi veya çalışan işlemlerinin uygulama kapsayıcısı da denetleyemiyor.

**My özel kapsayıcı 80 numaralı bağlantı noktasını farklı bir bağlantı noktası dinler. İstekleri bu bağlantı noktasına yönlendirmek için uygulamamı nasıl yapılandırabilirim?**

Otomatik olarak bağlantı noktası algılama sahibiz. Çağrılan ayarı bir uygulama belirtebilirsiniz *WEBSITES_PORT* ve beklenen bağlantı noktası değerini verin. Daha önce kullanılan platform *bağlantı noktası* uygulama ayarı. Biz bu uygulama ayarının kullanımdan ve kullanmak için planlama *WEBSITES_PORT* özel kullanımda.

**HTTPS özel my kapsayıcısında uygulama gerekiyor mu?**

Hayır, platform paylaşılan ön uçlar, HTTPS sonlandırma işler.

## <a name="multi-container-with-docker-compose"></a>Çok kapsayıcılı Docker ile oluşturma

**Azure Container Registry (ACR), çok kapsayıcılı ile kullanmak için nasıl yapılandırabilirim?**

ACR çok kapsayıcılı ile kullanmak için **tüm kapsayıcı görüntülerini** aynı ACR kayıt defteri sunucu üzerinde barındırılması gerekir. Aynı kayıt defteri sunucuda oldukları sonra uygulama ayarları oluşturma ve ardından ACR görüntü adını içerecek şekilde Docker Compose yapılandırma dosyasını güncelleştirmek gerekir.

Aşağıdaki uygulama ayarları oluşturma:

- DOCKER_REGISTRY_SERVER_USERNAME
- DOCKER_REGISTRY_SERVER_URL (tam URL'si, ör: `https://<server-name>.azurecr.io`)
- DOCKER_REGISTRY_SERVER_PASSWORD (ACR Ayarları'nda yönetici erişimi etkinleştir)

Yapılandırma dosyasının içinde aşağıdaki örnekte olduğu gibi ACR görüntü başvurusu:

```yaml
image: <server-name>.azurecr.io/<image-name>:<tag>
```

**Nasıl İnternet'ten erişilemediği kapsayıcıdır biliyor musunuz?**

- Yalnızca bir kapsayıcı için erişimi açık olabilir.
- Yalnızca bağlantı noktası 80 ve 8080 olan erişilebilir (kullanıma sunulan bağlantı noktası)

Hangi kapsayıcı öncelik sırasına göre erişilebilir - belirlemek için kurallar şunlardır:

- Uygulama ayarı `WEBSITES_WEB_CONTAINER_NAME` kapsayıcının adına ayarlayın
- İlk kapsayıcı 80 veya 8080 bağlantı noktası tanımlamak için
- Yukarıdakilerin hiçbiri true ise, dosyasında tanımlanan ilk kapsayıcı erişilebilir olması (açık)

## <a name="pricing-and-sla"></a>Fiyatlandırma ve SLA

**Hizmet genel kullanıma açık olduğuna göre, fiyatlandırma nedir?**

Uygulamanız çalışırken saat sayısı için normal Azure App Service fiyatlandırma ücretlendirilir.

## <a name="other-questions"></a>Diğer sorular

**Uygulama ayarları adlarının desteklenen karakterler nelerdir?**

İçin uygulama ayarları, yalnızca harfler (A-Z, a-z), sayılar (0-9) ve alt çizgi karakteri (_) kullanabilirsiniz.

**Burada yeni özellikler talep edebilir?**

Fikrinizi, gönderdiğiniz [Web Apps geri bildirim Forumu](https://aka.ms/webapps-uservoice). "[Linux]" fikrinizi başlığı ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Linux üzerinde Azure App Service nedir?](app-service-linux-intro.md)
- [Azure App Service ortamlarında hazırlık ayarlama](../../app-service/deploy-staging-slots.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Kapsayıcılar için Web App ile sürekli dağıtım](./app-service-linux-ci-cd.md)
