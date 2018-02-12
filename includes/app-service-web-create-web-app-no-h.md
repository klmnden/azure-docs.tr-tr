---
title: "include dosyası"
description: "include dosyası"
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: d5dfe6ab6d53d450f7c75d376d630558ee03f698
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
`myAppServicePlan` App Service planında bir [web uygulaması](../articles/app-service/app-service-web-overview.md) oluşturun.

Cloud Shell'de, [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) komutunu kullanabilirsiniz. Aşağıdaki örnekte *\<uygulama_adı>* kısmını genel olarak benzersiz bir uygulama adıyla değiştirin (geçerli karakterler `a-z`, `0-9` ve `-` şeklindedir). 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-local-git
```

Web uygulaması oluşturulduğunda Azure CLI aşağıda yer alan örnekteki gibi bilgiler gösterir:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Git dağıtımı etkin boş bir web uygulaması oluşturdunuz.

> [!NOTE]
> Git uzak URL’si `deploymentLocalGitUrl` özelliği içinde `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git` biçiminde gösterilir. Bu URL’ye daha sonra ihtiyacınız olacağı için URL’yi kaydedin.
>

Yeni oluşturulan web uygulamasına göz atın.

```
http://<app_name>.azurewebsites.net
```

Yeni web uygulamanız aşağıdaki gibi görünmelidir:
