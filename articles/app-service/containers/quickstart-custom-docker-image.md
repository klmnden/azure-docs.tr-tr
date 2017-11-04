---
title: "Web uygulaması kapsayıcıları için özel bir Docker hub'a görüntü çalıştırmak | Microsoft Docs"
description: "Kapsayıcıları için Web uygulaması için özel bir Docker görüntü kullanma"
keywords: "Azure uygulama hizmeti, web uygulaması, linux, docker, kapsayıcı"
services: app-service
documentationcenter: 
author: naziml
manager: cfowler
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/05/2017
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: c85f79cc14cdcecd2a05fc0ff91c4864b9fba277
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="run-a-custom-docker-hub-image-in-web-app-for-containers"></a>Web uygulaması kapsayıcıları için özel bir Docker hub'a görüntü çalıştırın

Uygulama hizmeti önceden tanımlanmış uygulama yığınları PHP 7.0 ve Node.js 4.5 gibi belirli sürümleri için destek ile Linux'ta sağlar. Özel bir Docker görüntü, Azure içinde tanımlı değil bir uygulama yığınını web uygulamanızı dağıtmak için de kullanabilirsiniz. Bu hızlı başlangıç web uygulaması oluşturma ve temel bir Python dağıtma gösterilmektedir Docker görüntüye. Web uygulamasını kullanarak oluşturduğunuz [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app-for-container"></a>Bir Web uygulaması için kapsayıcı oluşturma

[az webapp create](/cli/azure/webapp#create) komutuyla `myAppServicePlan` App Service planında bir [web uygulaması](../app-service-web-overview.md) oluşturun. Değiştirmeyi unutmayın `<app name>` benzersiz bir uygulama adına sahip.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name elnably/dockerimagetest
```

Önceki komutta `--deployment-container-image-name` ortak Docker hub'a görüntüye işaret eden [https://hub.docker.com/r/elnably/dockerimagetest/](https://hub.docker.com/r/elnably/dockerimagetest/). Kendi içerik inceleyebilirsiniz [https://github.com/ahmedelnably/dockerimagetest](https://github.com/ahmedelnably/dockerimagetest).

Web uygulaması oluşturduğunuzda Azure CLI çıktı aşağıdaki örneğe benzer şekilde gösterir:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app name>.scm.azurewebsites.net/<app name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

## <a name="browse-to-the-app"></a>Uygulamaya göz atma

Web tarayıcınızı kullanarak aşağıdaki URL'ye gidin.

```bash
http://<app_name>.azurewebsites.net
```

![Azure'da çalışan örnek uygulama](media/quickstart-custom-docker-image/hello-world-in-browser.png)

**Tebrikler!** Kapsayıcıları için Web uygulaması için özel bir Docker görüntü dağıttıktan sonra.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure'da Docker Python ve PostgreSQL bir web uygulaması oluşturma](tutorial-docker-python-postgresql-app.md)
