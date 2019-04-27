---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 9f7c82a2bd35f06da096fa0dd9d5d1fa4d08011e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60838937"
---
Cloud Shell’de, `myAppServicePlan` App Service planında bir [web uygulaması](../articles/app-service/overview.md) oluşturun. Bunu, [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) komutunu kullanarak yapabilirsiniz. Aşağıdaki örnekte, değiştirin  *\<-adı >* bir genel benzersiz uygulama adıyla (geçerli karakterler `a-z`, `0-9`, ve `-`).

```azurecli-interactive
az webapp create --name <app-name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-local-git
```

Web uygulaması oluşturulduğunda Azure CLI aşağıda yer alan örnekteki gibi bilgiler gösterir:

```json
Local git is configured with url of 'https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app-name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Git dağıtımı etkin boş bir web uygulaması oluşturdunuz.

> [!NOTE]
> Git uzak URL’si `deploymentLocalGitUrl` özelliği içinde `https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git` biçiminde gösterilir. Bu URL’ye daha sonra ihtiyacınız olacağı için URL’yi kaydedin.
>

Yeni oluşturulan web uygulamasına göz atın.

```
http://<app-name>.azurewebsites.net
```

Yeni web uygulamanız aşağıdaki gibi görünmelidir:
