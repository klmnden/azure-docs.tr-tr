---
title: "Azure Kapsayıcılar için Web App’te bir Go Uygulaması Dağıtma | Microsoft Docs"
description: "Azure Kapsayıcılar için Web App’e yönelik Go uygulaması çalıştıran bir Docker görüntüsü dağıtma."
keywords: "azure app service, web uygulaması, go, docker, kapsayıcı"
services: app-service
author: sptramer
manager: cfowler
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.devlang: go
ms.topic: quickstart
ms.date: 01/17/2018
ms.author: sttramer
ms.custom: mvc
ms.openlocfilehash: dbe4d7bc6f5f1d83a079993c9616206fd82a1b9d
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="deploy-a-go-app-in-azure-web-app-for-containers"></a>Azure Kapsayıcılar için Web App’te bir Go Uygulaması Dağıtma

App Service, Linux’ta PHP 7.0 ve Node.js 4.5 gibi belirli sürümleri destekleyen önceden tanımlı uygulama yığınları sağlar. Ayrıca web uygulamanızı Azure’da zaten tanımlı olmayan bir uygulama yığınında çalıştırmak için özel bir Docker görüntüsü de kullanabilirsiniz. Bu hızlı başlangıç, Go uygulaması çalıştıran mevcut bir Docker kapsayıcısını alıp Azure App Service’te çalıştırmayı gösterir. Web uygulamasını [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) kullanarak oluşturursunuz.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app-for-the-container"></a>Kapsayıcı için Web Uygulaması oluşturma

[az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) komutuyla `myAppServicePlan` App Service planında bir [web uygulaması](../app-service-web-overview.md) oluşturun. `<app name>` değerini genel olarak benzersiz bir uygulama adıyla değiştirmeyi unutmayın.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --deployment-container-image-name microsoft/appservice-go-quickstart
```

Önceki komutta, `--deployment-container-image-name` genel Docker Hub görüntüsüne işaret eder [microsoft/appservice-go-quickstart](https://hub.docker.com/r/microsoft/appservice-go-quickstart).

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

## <a name="get-data-from-the-apps-endpoint"></a>Uygulamanın uç noktasından veri alma

`curl` komutunu kullanarak, kapsayıcının uygulaması tarafından sağlanan REST API uç noktası ile iletişim kurun.

```bash
curl -X GET http://<app_name>.azurewebsites.net:8080/hello
```

```output
Hello world!
```

**Tebrikler!** Kapsayıcılar için Web App’e yönelik Go uygulaması çalıştıran özel bir Docker görüntüsü dağıttınız.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel Docker görüntüsü kullanma](tutorial-custom-docker-image.md)
