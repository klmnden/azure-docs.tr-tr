---
title: Linux'ta - Azure App Service'e Python uygulaması oluşturma | Microsoft Docs
description: Linux üzerinde Azure App Service'te ilk Python merhaba dünya uygulamanızı birkaç dakika içinde dağıtın.
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
ms.date: 03/28/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: a386924be2f82aadafeaccbeadc68e5e27404855
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59546340"
---
# <a name="create-a-python-app-in-azure-app-service-on-linux-preview"></a>Linux (Önizleme) üzerinde Azure App Service'te bir Python uygulaması oluşturma

[Linux’ta App Service](app-service-linux-intro.md) Linux işletim sistemini kullanan yüksek oranda ölçeklenebilir, otomatik olarak düzeltme eki uygulayan bir web barındırma hizmeti sağlar. Bu hızlı başlangıçta, kullanarak Linux üzerinde App Service'te yerleşik Python görüntüsünü (Önizleme) üzerinde bir Python uygulamasının nasıl dağıtılacağı gösterilmektedir [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

Mac, Windows veya Linux makinesi kullanarak bu makaledeki adımları izleyebilirsiniz.

![Azure'da çalışan örnek uygulama](media/quickstart-python/hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="download-the-sample"></a>Örneği indirme

Cloud Shell'de bir quickstart dizini oluşturun ve o dizine geçin.

```bash
mkdir quickstart

cd quickstart
```

Ardından, örnek uygulama deposunu quickstart dizininize kopyalamak için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

Çalıştırıldığında, aşağıdaki örneğe benzer bilgiler görüntüler:

```bash
Cloning into 'python-docs-hello-world'...
remote: Enumerating objects: 43, done.
remote: Total 43 (delta 0), reused 0 (delta 0), pack-reused 43
Unpacking objects: 100% (43/43), done.
Checking connectivity... done.
```

## <a name="create-a-web-app"></a>Web uygulaması oluşturma

Örnek kodu içeren dizine geçin ve `az webapp up` komutunu çalıştırın.

Aşağıdaki komutta <app_name> kısmını benzersiz uygulama adıyla değiştirin.

```bash
cd python-docs-hello-world

az webapp up -n <app_name>
```

Bu komutun çalıştırılması birkaç dakika sürebilir. Çalıştırıldığında, aşağıdaki örneğe benzer bilgiler görüntüler:

```json
The behavior of this command has been altered by the following extension: webapp
Creating Resource group 'appsvc_rg_Linux_CentralUS' ...
Resource group creation complete
Creating App service plan 'appsvc_asp_Linux_CentralUS' ...
App service plan creation complete
Creating app '<app_name>' ....
Webapp creation complete
Creating zip with contents of dir /home/username/quickstart/python-docs-hello-world ...
Preparing to deploy contents to app.
All done.
{
  "app_url": "https:/<app_name>.azurewebsites.net",
  "location": "Central US",
  "name": "<app_name>",
  "os": "Linux",
  "resourcegroup": "appsvc_rg_Linux_CentralUS ",
  "serverfarm": "appsvc_asp_Linux_CentralUS",
  "sku": "BASIC",
  "src_path": "/home/username/quickstart/python-docs-hello-world ",
  "version_detected": "-",
  "version_to_create": "python|3.7"
}
```

`az webapp up` komutu şu eylemleri gerçekleştirir:

- Varsayılan kaynak grubunu oluşturur.

- Bir varsayılan uygulama hizmeti planı oluşturur.

- Belirtilen adla bir uygulama oluşturur.

- [Zip dağıtma](https://docs.microsoft.com/azure/app-service/deploy-zip) dosyaları uygulamanın geçerli çalışma dizini.

## <a name="browse-to-the-app"></a>Uygulamaya göz atma

Web tarayıcınızı kullanarak, dağıtılan uygulamanın konumuna gidin.

```bash
http://<app_name>.azurewebsites.net
```

Python örnek kodu, bir yerleşik görüntü ile Linux üzerinde App Service'te çalışıyor.

![Azure'da çalışan örnek uygulama](media/quickstart-python/hello-world-in-browser.png)

**Tebrikler!** Linux üzerinde App Service'e ilk Python uygulamanızı dağıttınız.

## <a name="update-locally-and-redeploy-the-code"></a>Kodu yerel makinede güncelleştirme ve yeniden dağıtma

Cloud Shell'de yazın `code application.py` Cloud Shell düzenleyiciyi açın.

![Kod application.py](media/quickstart-python/code-applicationpy.png)

 `return` çağrısında metinde küçük bir değişiklik yapın:

```python
return "Hello Azure!"
```

Yaptığınız değişiklikleri kaydedin ve düzenleyiciden çıkın. Kaydetmek için `^S` ve çıkmak için `^Q` komutunu kullanın.

Şimdi uygulamayı yeniden dağıtacaksınız. Yedek `<app_name>` uygulamanızla.

```bash
az webapp up -n <app_name>
```

Dağıtım tamamlandıktan sonra **Uygulamaya göz atma** adımında açılan tarayıcı penceresine dönüp sayfayı yenileyin.

![Azure'da çalışan güncelleştirilmiş örnek uygulama](media/quickstart-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-app"></a>Yeni Azure uygulamanızı yönetme

Git <a href="https://portal.azure.com" target="_blank">Azure portalında</a> oluşturduğunuz uygulamayı yönetmek için.

Sol menüden **uygulama hizmetleri**ve ardından Azure uygulamanızın adına tıklayın.

![Azure uygulamasına portal gezintisi](./media/quickstart-python/app-service-list.png)

Uygulamanızın genel bakış sayfasını görürsünüz. Buradan göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz.

![Azure portalında App Service sayfası](media/quickstart-python/app-service-detail.png)

Soldaki menü, uygulamanızı yapılandırmak için farklı sayfalar sağlar. 

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Linux'ta App Service hizmetindeki yerleşik Python görüntüsü şu anda Önizleme aşamasındadır ve uygulamanızı başlatmak için kullanılan komutu özelleştirebilirsiniz. Ayrıca bunun yerine özel bir kapsayıcı kullanarak üretim aşamasında Python uygulamaları oluşturabilirsiniz.

> [!div class="nextstepaction"]
> [Öğretici: PostgreSQL ile Python uygulaması](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Python uygulamasını yapılandırma](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Öğretici: Özel kapsayıcı deposundan dağıtın](tutorial-custom-docker-image.md)
