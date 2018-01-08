---
title: "Azure'da PHP web uygulaması oluşturma | Microsoft Docs"
description: "Azure App Service Web Uygulamalarında ilk PHP Hello World uygulamanızı birkaç dakika içinde dağıtın."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 6feac128-c728-4491-8b79-962da9a40788
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 12/13/2017
ms.author: cephalin;cfowler
ms.custom: mvc
ms.openlocfilehash: 9a41c08868de853ba82874a63b80316ec834858a
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="create-a-php-web-app-in-azure"></a>Azure’da PHP web uygulaması oluşturma

> [!NOTE]
> Bu makalede bir uygulamanın Windows üzerinde App Service'e dağıtımı yapılır. _Linux_ üzerinde App Service'e dağıtım yapmak için bkz. [Linux üzerinde App Service'te PHP web uygulaması oluşturma](./containers/quickstart-php.md).
>

[Azure Web Apps](app-service-web-overview.md) yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar.  Bu hızlı başlangıç öğreticisinde, Azure Web Apps'te bir PHP uygulamasının nasıl dağıtılacağı gösterilmektedir. Cloud Shell’de [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) kullanarak web uygulamasını oluşturabilir ve örnek PHP kodunu web uygulamasına dağıtmak için [ZIP dosyası](app-service-deploy-zip.md) kullanabilirsiniz.

![Azure'da çalışan örnek uygulama]](media/app-service-web-get-started-php/hello-world-in-browser.png)

Mac, Windows veya Linux makinesi kullanarak buradaki adımları izleyebilirsiniz. Önkoşullar yüklendikten sonra adımların tamamlanması yaklaşık olarak beş dakika sürer.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için:

* <a href="https://php.net" target="_blank">PHP'yi yükleyin</a>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample-locally"></a>Örnekleri yerel makineye indirme

[Https://github.com/Azure-Samples/php-docs-hello-world/archive/master.zip](https://github.com/Azure-Samples/php-docs-hello-world/archive/master.zip) adresinden örnek PHP projesini indirin ve ZIP arşivini ayıklayın.

Terminal penceresinde, örnek PHP projesinin kök dizinine gidin (_index.php_ dosyasını içeren dizin).

## <a name="run-the-app-locally"></a>Uygulamayı yerel olarak çalıştırma

Yerleşik PHP web sunucusunu başlatmak için bir terminal penceresi açıp ve `php` komutunu kullanıp uygulamayı yerel olarak çalıştırın.

```bash
php -S localhost:8080
```

Bir web tarayıcısı açın ve `http://localhost:8080` konumundaki örnek uygulamaya gidin.

Sayfada gösterilen örnek uygulamada **Merhaba Dünya!** iletisini görürsünüz.

![Yerel olarak çalışan örnek uygulama](media/app-service-web-get-started-php/localhost-hello-world-in-browser.png)

Terminal pencerenizde **Ctrl+C** tuşlarına basarak web sunucusundan çıkın.

[!INCLUDE [Create ZIP file](../../includes/app-service-web-create-zip.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Upload zip file](../../includes/app-service-web-upload-zip.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)]

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

Cloud Shell’de, [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) komutuyla `myAppServicePlan` App Service planında bir web uygulaması oluşturun. 

Aşağıdaki örnekte `<app_name>` kısmını genel olarak benzersiz bir uygulama adıyla değiştirin (geçerli karakterler `a-z`, `0-9` ve `-` şeklindedir). Çalışma zamanı `PHP|7.0` olarak ayarlanmıştır. Desteklenen tüm çalışma zamanlarını görmek için, [az webapp list-runtimes](/cli/azure/webapp?view=azure-cli-latest#az_webapp_list_runtimes) komutunu çalıştırın. 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "PHP|7.0"
```

Web uygulaması oluşturulduğunda Azure CLI aşağıda yer alan çıktıdaki gibi bilgiler gösterir:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Yeni oluşturduğunuz web uygulamasına göz atın. _&lt;app name>_ değerini benzersiz bir uygulama adıyla değiştirin.

```bash
http://<app name>.azurewebsites.net
```

![Boş web uygulaması sayfası](media/app-service-web-get-started-php/app-service-web-service-created.png)

[!INCLUDE [Deploy uploaded ZIP file](../../includes/app-service-web-deploy-zip.md)]

## <a name="browse-to-the-app"></a>Uygulamaya göz atma

Web tarayıcınızı kullanarak, dağıtılan uygulamanın konumuna gidin.

```bash
http://<app_name>.azurewebsites.net
```

PHP örnek kodu bir Azure App Service web uygulamasında çalışıyor.

![Azure'da çalışan örnek uygulama](media/app-service-web-get-started-php/hello-world-in-browser.png)

**Tebrikler!** App Service’e ilk PHP uygulamanızı dağıttınız.

## <a name="update-locally-and-redeploy-the-code"></a>Kodu yerel makinede güncelleştirme ve yeniden dağıtma

Bir yerel metin düzenleyicisi kullanarak `index.php` dosyasını PHP uygulaması içinde açın ve `echo` öğesinin yanındaki dizenin içinde bulunan metinde küçük bir değişiklik yapın:

```php
echo "Hello Azure!";
```

Yerel terminal penceresinde, uygulamanızın kök dizinine gidin ve güncelleştirilmiş projeniz için yeni bir ZIP dosyası oluşturun.

```
# Bash
zip -r myUpdatedAppFiles.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath myUpdatedAppFiles.zip
``` 

[ZIP dosyasını karşıya yükleme](#upload-the-zip-file) altında verilen adımları kullanarak bu yeni ZIP dosyasını Cloud Shell'e yükleyin.

Ardından, Cloud Shell'de karşıya yüklenen ZIP dosyanızı yeniden dağıtın.

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResouceGroup --name <app_name> --src clouddrive/myUpdatedAppFiles.zip
```

**Uygulamaya göz atma** adımında açılan tarayıcı penceresine dönüp sayfayı yenileyin.

![Azure'da çalışan güncelleştirilmiş örnek uygulama](media/app-service-web-get-started-php/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Yeni Azure web uygulamanızı yönetme

Oluşturduğunuz web uygulamasını yönetmek için <a href="https://portal.azure.com" target="_blank">Azure portalına</a> gidin.

Sol menüden **Uygulama Hizmetleri**'ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/app-service-web-get-started-php/php-docs-hello-world-app-service-list.png)

Web uygulamanızın Genel Bakış sayfasını görürsünüz. Buradan göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz.

![Azure portalında App Service sayfası](media/app-service-web-get-started-php/php-docs-hello-world-app-service-detail.png)

Soldaki menü, uygulamanızı yapılandırmak için farklı sayfalar sağlar. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [MySQL ile PHP](app-service-web-tutorial-php-mysql.md)
