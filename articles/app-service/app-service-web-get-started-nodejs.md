---
title: Azure'da Node.js web uygulaması oluşturma | Microsoft Docs
description: Azure App Service Web Uygulamalarında ilk Node.js Hello World uygulamanızı birkaç dakika içinde dağıtın.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 12/13/2017
ms.author: cephalin;cfowler
ms.custom: mvc, devcenter
ms.openlocfilehash: 936077f9445ec38fe8d05314fdf20d8412fb8b06
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31514294"
---
# <a name="create-a-nodejs-web-app-in-azure"></a>Azure App Service'te Node.js web uygulaması oluşturma

> [!NOTE]
> Bu makalede bir uygulamanın Windows üzerinde App Service'e dağıtımı yapılır. _Linux_ üzerinde App Service'e dağıtım yapmak için bkz. [Linux üzerinde Azure App Service'te Node.js web uygulaması oluşturma](./containers/quickstart-nodejs.md).
>

[Azure Web Apps](app-service-web-overview.md) yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar.  Bu hızlı başlangıç öğreticisinde, Azure Web Apps'te ilk Node.js uygulamasının nasıl dağıtılacağı gösterilmektedir. [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)'yi kullanarak web uygulamasını oluşturabilir ve örnek Node.js kodunu web uygulamasında dağıtmak için Git'i kullanabilirsiniz.

![Azure'da çalışan örnek uygulama](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

Mac, Windows veya Linux makinesi kullanarak buradaki adımları izleyebilirsiniz. Önkoşullar yüklendikten sonra adımların tamamlanması yaklaşık olarak beş dakika sürer.   

> [!VIDEO https://channel9.msdn.com/Shows/Azure-for-Node-Developers/Create-a-Nodejs-app-in-Azure-Quickstart/player]   

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için:

* <a href="https://nodejs.org/" target="_blank">Node.js ve NPM'yi yükleyin</a>

## <a name="download-the-sample"></a>Örneği indirme

[https://github.com/Azure-Samples/nodejs-docs-hello-world/archive/master.zip](https://github.com/Azure-Samples/nodejs-docs-hello-world/archive/master.zip) adresinden örnek Node.js projesini indirin ve ZIP arşivini ayıklayın.

Terminal penceresinde, örnek Node.js projesinin kök dizinine gidin (_index.js_ dosyasını içeren dizin).

## <a name="run-the-app-locally"></a>Uygulamayı yerel olarak çalıştırma

Yerleşik Node.js HTTP sunucusunu başlatmak için bir terminal penceresi açıp ve `npm start` betiğini kullanıp uygulamayı yerel olarak çalıştırın.

```bash
npm start
```

Bir web tarayıcısı açın ve `http://localhost:1337` konumundaki örnek uygulamaya gidin.

Sayfada gösterilen örnek uygulamada **Hello World** iletisini görebilirsiniz.

![Yerel olarak çalışan örnek uygulama](media/app-service-web-get-started-nodejs-poc/localhost-hello-world-in-browser.png)

Terminal pencerenizde **Ctrl+C** tuşlarına basarak web sunucusundan çıkın.

[!INCLUDE [Create ZIP file](../../includes/app-service-web-create-zip.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)] 

[!INCLUDE [Create app service plan](../../includes/app-service-web-create-app-service-plan.md)] 

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

Cloud Shell’de, [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) komutuyla `myAppServicePlan` App Service planında bir web uygulaması oluşturun. 

Aşağıdaki örnekte `<app_name>` kısmını genel olarak benzersiz bir uygulama adıyla değiştirin (geçerli karakterler `a-z`, `0-9` ve `-` şeklindedir). Çalışma zamanı `NODE|6.9` olarak ayarlanmıştır. Desteklenen tüm çalışma zamanları görmek için [`az webapp list-runtimes`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_list_runtimes) komutunu çalıştırın. 

```azurecli-interactive
# Bash
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "NODE|6.9"
# PowerShell
az --% webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "NODE|6.9"
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

Yeni web uygulamanız aşağıdaki gibi görünmelidir:

![Boş web uygulaması sayfası](media/app-service-web-get-started-php/app-service-web-service-created.png)

[!INCLUDE [Deploy ZIP file](../../includes/app-service-web-deploy-zip.md)]

## <a name="browse-to-the-app"></a>Uygulamaya göz atma

Web tarayıcınızı kullanarak, dağıtılan uygulamanın konumuna gidin.

```
http://<app_name>.azurewebsites.net
```

Node.js örnek kodu bir Azure App Service web uygulamasında çalışıyor.

![Azure'da çalışan örnek uygulama](media/app-service-web-get-started-nodejs-poc/hello-world-in-browser.png)

**Tebrikler!** App Service’e ilk Node.js uygulamanızı dağıttınız.

## <a name="update-and-redeploy-the-code"></a>Kodu güncelleştirme ve yeniden dağıtma

Bir metin düzenleyici kullanarak `index.js` dosyasını Node.js uygulaması içinde açın ve `response.end` çağrısındaki metinde küçük bir değişiklik yapın:

```nodejs
response.end("Hello Azure!");
```

Yerel terminal penceresinde, uygulamanızın kök dizinine gidin ve güncelleştirilmiş projeniz için yeni bir ZIP dosyası oluşturun.

```
# Bash
zip -r myUpdatedAppFiles.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath myUpdatedAppFiles.zip
``` 

[ZIP dosyasını karşıya yükleme](#upload-the-zip-file) altında verilen adımları kullanarak bu yeni ZIP dosyasını App Service'e dağıtın.

**Uygulamaya göz atma** adımında açılan tarayıcı penceresine dönüp sayfayı yenileyin.

![Azure'da çalışan güncelleştirilmiş örnek uygulama](media/app-service-web-get-started-nodejs-poc/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-web-app"></a>Yeni Azure web uygulamanızı yönetme

Oluşturduğunuz web uygulamasını yönetmek için <a href="https://portal.azure.com" target="_blank">Azure portalına</a> gidin.

Sol menüden **Uygulama Hizmetleri**'ne ve ardından Azure web uygulamanızın adına tıklayın.

![Portaldan Azure web uygulamasına gitme](./media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-list.png)

Web uygulamanızın Genel Bakış sayfasını görürsünüz. Buradan göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. 

![Azure portalında App Service sayfası](media/app-service-web-get-started-nodejs-poc/nodejs-docs-hello-world-app-service-detail.png)

Soldaki menü, uygulamanızı yapılandırmak için farklı sayfalar sağlar. 

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [MongoDB ile Node.js](app-service-web-tutorial-nodejs-mongodb-app.md)
