---
title: Müşteri kapsayıcıları - Azure App Service'ı yapılandırma | Microsoft Docs
description: Node.js uygulamalarını Azure App Service'te çalışacak şekilde yapılandırma hakkında bilgi edinin
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnock
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/28/2019
ms.author: cephalin
ms.openlocfilehash: 1e5faa8d356b891d825586414c0a1a1b9fa47090
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60001890"
---
# <a name="configure-a-custom-linux-container-for-azure-app-service"></a>Azure App Service için özel bir Linux kapsayıcısını yapılandırın

Bu makalede, Azure App Service'te çalıştırılması için özel bir Linux kapsayıcısını yapılandırın işlemini göstermektedir.

Bu kılavuzu temel kavramları ve kapsayıcı uygulamalar App Service Linux için yönergeler sağlar. Azure App Service daha önce kullanmadıysanız izleyin [özel kapsayıcı hızlı](quickstart-docker-go.md) ve [öğretici](tutorial-custom-docker-image.md) ilk. Ayrıca bir [çoklu kapsayıcı uygulaması Hızlı Başlangıç](quickstart-multi-container.md) ve [öğretici](tutorial-multi-container-app.md).

## <a name="configure-port-number"></a>Bağlantı noktası numarasını yapılandırmadan

Özel görüntünüzü de web sunucusu, 80 dışında bir bağlantı noktası kullanabilirsiniz. Azure kullanarak kendi özel kullandığı bağlantı noktası hakkında bilgi `WEBSITES_PORT` uygulama ayarı. [Bu öğreticideki Python örneği](https://github.com/Azure-Samples/docker-django-webapp-linux) için GitHub sayfası, `WEBSITES_PORT` olarak _8000_ ayarlamanız gerektiğini gösterir. Çalıştırarak ayarlayabilirsiniz [ `az webapp config appsettings set` ](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) Cloud shell'de komutu. Örneğin:

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_PORT=8000
```

## <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma

Özel kapsayıcınızın dışarıdan sağlanması gereken ortam değişkenlerini kullanabilirsiniz. Bunları çalıştırarak geçirebilirsiniz [ `az webapp config appsettings set` ](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) Cloud shell'de komutu. Örneğin:

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WORDPRESS_DB_HOST="myownserver.mysql.database.azure.com"
```

Bu yöntem, hem tek kapsayıcı uygulamaları veya burada ortam değişkenleri belirtilir, çok kapsayıcılı uygulamalar için çalışır *docker-compose.yml* dosya.

## <a name="use-persistent-shared-storage"></a>Kalıcı paylaşılan depolama kullanmak

Kullanabileceğiniz */home* yeniden başlatmaları arasındaki dosyaları kalıcı hale getirmek ve örnekleri arasında paylaşmak için uygulamanızın dosya sistemindeki dizin. `/home` Uygulamanızda kalıcı depolamaya erişmek kapsayıcı uygulamanızı etkinleştirmek için sağlanır.

Ne zaman kalıcı depolama devre dışı bırakıldı ve Yazar `/home` uygulama yeniden başlatmaları arasında veya birden fazla örnek arasında dizin kalıcı değildir. Tek özel durum `/home/LogFiles` dizinini Docker ve kapsayıcı günlüklerini depolamak için kullanılır. Kalıcı depolama etkinleştirildiğinde, tüm yazma işlemlerini `/home` dizin kalıcı ve genişletilmiş uygulama tüm örnekleri tarafından erişilebilir.

Varsayılan olarak, kalıcı depolama, *devre dışı*. Etkinleştirmek veya devre dışı bırakmak için ayarlanmış `WEBSITES_ENABLE_APP_SERVICE_STORAGE` çalıştırarak uygulama ayarı [ `az webapp config appsettings set` ](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) Cloud shell'de komutu. Örneğin:

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=true
```

> [!NOTE]
> Ayrıca [kalıcı depolamanızı yapılandırın](how-to-serve-content-from-azure-storage.md).

## <a name="enable-ssh"></a>SSH'yi etkinleştirmek

SSH, kapsayıcı ile istemci arasında güvenli iletişime olanak tanır. SSH'yi desteklemesi özel bir kapsayıcı için sırayla Dockerfile eklemelisiniz.

> [!TIP]
> Tüm yerleşik Linux kapsayıcıları SSH yönergeleri, görüntü depolarında ekledik. Aşağıdaki yönergelerden aracılığıyla gidebilirsiniz [Node.js 10.14 depo](https://github.com/Azure-App-Service/node/blob/master/10.14) bunu orada nasıl etkinleştirildiğini öğrenin.

- Kullanım [ÇALIŞTIRMA](https://docs.docker.com/engine/reference/builder/#run) SSH sunucusu yüklemek ve kök hesabın parolasını ayarlamak için yönerge `"Docker!"`. Örneğin, temel alan bir görüntü için [Alpine Linux](https://hub.docker.com/_/alpine), aşağıdaki komutları gerekir:

    ```Dockerfile
    RUN apk add openssh \
         && echo "root:Docker!" | chpasswd 
    ```

    Bu yapılandırma kapsayıcıya dış bağlantılar izin vermez. SSH yalnızca `https://<app-name>.scm.azurewebsites.net` ve yayımlama kimlik bilgileriyle kimlik doğrulaması yapılır.

- Ekleme [bu sshd_config dosyasına](https://github.com/Azure-App-Service/node/blob/master/10.14/sshd_config) görüntü deposuna ve kullanım [kopyalama](https://docs.docker.com/engine/reference/builder/#copy) dosyasına kopyalamak için yönerge */etc/ssh/* dizin. Hakkında daha fazla bilgi için *sshd_config* dosyaları görmek [OpenBSD belgeleri](https://man.openbsd.org/sshd_config).

    ```Dockerfile
    COPY sshd_config /etc/ssh/
    ```

    > [!NOTE]
    > *Sshd_config* dosyası şu öğeleri içermelidir:
    > - `Ciphers`, bu listedeki en az bir öğeyi içermelidir: `aes128-cbc,3des-cbc,aes256-cbc`.
    > - `MACs`, bu listedeki en az bir öğeyi içermelidir: `hmac-sha1,hmac-sha1-96`.

- Kullanım [kullanıma](https://docs.docker.com/engine/reference/builder/#expose) kapsayıcıda 2222 numaralı bağlantı noktasına açmak için yönerge. Kök parolası biliniyor olsa da, 2222 numaralı bağlantı noktasına internet'ten erişilemez. Yalnızca özel bir sanal ağın köprü ağı içinde kapsayıcıları tarafından erişilebilir durumda.

    ```Dockerfile
    EXPOSE 80 2222
    ```

- Kapsayıcınız için başlatma betiği, SSH sunucuyu başlatın.

    ```bash
    /usr/sbin/sshd
    ```

    Bir örnek için bkz. nasıl varsayılan [Node.js 10.14 kapsayıcı](https://github.com/Azure-App-Service/node/blob/master/10.14/startup/init_container.sh) SSH sunucusu başlatır.

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="configure-multi-container-apps"></a>Çok kapsayıcılı uygulamaları yapılandırma

- [Docker Compose kalıcı depolama kullanma](#use-persistent-storage-in-docker-compose)
- [Önizleme sınırlamaları](#preview-limitations)
- [Docker Compose seçenekleri](#docker-compose-options)
- [Kubernetes yapılandırma seçenekleri](#kubernetes-configuration-options)

### <a name="use-persistent-storage-in-docker-compose"></a>Docker Compose kalıcı depolama kullanma

WordPress gibi çok kapsayıcılı uygulamalar düzgün çalışması için kalıcı depolamaya ihtiyacınız vardır. Bunu etkinleştirmek için Docker Compose yapılandırmanıza bir depolama konumuna işaret etmelidir *dışında* kapsayıcınızı. Kapsayıcınızın içindeki depolama konumları, uygulamanın yeniden ötesinde değişiklikleri kalıcı hale getirmek yok.

Kalıcı depolama etkinleştirmeniz `WEBSITES_ENABLE_APP_SERVICE_STORAGE` ayarlama, kullanarak uygulama [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) Cloud shell'de komutu.

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=TRUE
```

İçinde *docker-compose.yml* dosya, harita `volumes` seçeneğini `${WEBAPP_STORAGE_HOME}`. 

`WEBAPP_STORAGE_HOME`, App Service içinde bulunan ve uygulamanız için kalıcı depolamayla eşlenmiş olan bir ortam değişkenidir. Örneğin:

```yaml
wordpress:
  image: wordpress:latest
  volumes:
  - ${WEBAPP_STORAGE_HOME}/site/wwwroot:/var/www/html
  - ${WEBAPP_STORAGE_HOME}/phpmyadmin:/var/www/phpmyadmin
  - ${WEBAPP_STORAGE_HOME}/LogFiles:/var/log
```

### <a name="use-custom-storage-in-docker-compose"></a>Docker Compose özel depolama kullanma

Azure depolama (Azure dosyaları veya Azure Blob) ile çok kapsayıcılı uygulamaları özel kimliğini kullanarak bağlanabilir. Özel kimlik adı görüntülemek için çalıştırın [ `az webapp config storage-account list --name <app_name> --resource-group <resource_group>` ](/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-list).

İçinde *docker-compose.yml* dosya, harita `volumes` seçeneğini `custom-id`. Örneğin:

```yaml
wordpress:
  image: wordpress:latest
  volumes:
  - <custom-id>:<path_in_container>
```

### <a name="preview-limitations"></a>Önizleme sınırlamaları

Çok kapsayıcılı şu anda Önizleme aşamasındadır. Aşağıdaki App Service platformu özellikleri desteklenmez:

- Kimlik Doğrulama / Yetkilendirme
- Yönetilen kimlikler

### <a name="docker-compose-options"></a>Docker Compose seçenekleri

Aşağıdaki listelerde, desteklenen ve desteklenmeyen Docker Compose yapılandırma seçenekleri gösterilir:

#### <a name="supported-options"></a>Desteklenen seçenekler

- command
- entrypoint
- environment
- image
- ports
- restart
- services
- volumes

#### <a name="unsupported-options"></a>Desteklenmeyen seçenekler

- build (izin verilmez)
- depends_on (yoksayılır)
- networks (yoksayılır)
- secrets (yoksayılır)
- 80 ve 8080 (yoksayıldı) dışındaki bağlantı noktaları

> [!NOTE]
> Açıkça çağrılan diğer seçenekler, genel Önizleme aşamasında göz ardı edilir.

### <a name="kubernetes-configuration-options"></a>Kubernetes yapılandırma seçenekleri

Kubernetes için aşağıdaki yapılandırma seçeneklerini destekler:

- args
- command
- containers
- image
- ad
- ports
- spec

> [!NOTE]
> Açıkça çekilerek diğer seçenekler, genel Önizleme sürümünde desteklenmez.
>

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Özel kapsayıcı deposundan dağıtın](tutorial-custom-docker-image.md)

> [!div class="nextstepaction"]
> [Öğretici: WordPress çoklu kapsayıcı uygulaması](tutorial-multi-container-app.md)
