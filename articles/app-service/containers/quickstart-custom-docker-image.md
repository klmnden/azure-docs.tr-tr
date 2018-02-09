---
title: "Azure Kapsayıcılar için Web App’te özel Docker Hub görüntüsü çalıştırma | Microsoft Docs"
description: "Azure Kapsayıcılar için Web App’e yönelik özel Docker görüntüsü kullanma"
keywords: "azure app service, web uygulaması, linux, docker, kapsayıcı"
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/02/2017
ms.author: cephalin;wesmc
ms.custom: mvc
ms.openlocfilehash: 7f6ed6d52bea1faec9dbed96a8d7aef020aea5d9
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="run-a-custom-docker-hub-image-in-azure-web-app-for-containers"></a>Azure Kapsayıcılar için Web App’te özel Docker Hub görüntüsü çalıştırma

App Service, Linux’ta PHP 7.0 ve Node.js 4.5 gibi belirli sürümleri destekleyen önceden tanımlı uygulama yığınları sağlar. Ayrıca web uygulamanızı Azure’da zaten tanımlı olmayan bir uygulama yığınında çalıştırmak için özel bir Docker görüntüsü de kullanabilirsiniz. Bu hızlı başlangıçta bir web uygulaması oluşturma ve [resmi Nginx Docker görüntüsünü](https://hub.docker.com/r/_/nginx/) bu uygulamaya dağıtma gösterilmektedir. Web uygulamasını [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) kullanarak oluşturursunuz.

![Azure'da çalışan örnek uygulama](media/quickstart-custom-docker-image/hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app-for-container"></a>Kapsayıcılar için Web Uygulaması oluşturma

[`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) komutuyla `myAppServicePlan` App Service planında bir [web uygulaması](../app-service-web-overview.md) oluşturun. `<app name>` değerini benzersiz bir uygulama adıyla değiştirmeyi unutmayın.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name nginx
```

Önceki komutta, `--deployment-container-image-name` genel Docker Hub görüntüsüne işaret eder [https://hub.docker.com/r/_/nginx/](https://hub.docker.com/r/_/nginx/).

Web uygulaması oluşturulduğunda Azure CLI aşağıda yer alan çıktıdaki gibi bilgiler gösterir:

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

Web tarayıcınızı kullanarak aşağıdaki URL’ye gidin.

```bash
http://<app_name>.azurewebsites.net
```

![Azure'da çalışan örnek uygulama](media/quickstart-custom-docker-image/hello-world-in-browser.png)

**Tebrikler!** Kapsayıcılar için Web App’e bir özel Docker görüntüsü dağıttınız.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel Docker görüntüsü kullanma](tutorial-custom-docker-image.md)
