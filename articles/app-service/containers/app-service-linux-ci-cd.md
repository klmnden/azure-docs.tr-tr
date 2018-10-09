---
title: Bir Docker kapsayıcısı kayıt defteri kapsayıcılar - Azure için Web App ile sürekli dağıtım | Microsoft Docs
description: Kapsayıcılar için Web App'te bir Docker kapsayıcısı kayıt defteri sürekli dağıtımı ayarlama yapma.
keywords: Azure app service, linux, docker, acr, oss
services: app-service
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2018
ms.author: msangapu
ms.openlocfilehash: 20ca63b7126a6800538129115ff339308c11d8c5
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867042"
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Kapsayıcılar için Web App ile sürekli dağıtım

Bu öğreticide, bir özel kapsayıcı görüntüsü için sürekli dağıtım yapılandırma öğesinden yönetilen [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) depoları veya [Docker Hub](https://hub.docker.com).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="enable-the-continuous-deployment-feature"></a>Sürekli dağıtım özelliğini etkinleştir

Kullanarak sürekli dağıtım özelliği etkinleştirmek [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) ve aşağıdaki komut yürütülüyor:

```azurecli-interactive
az webapp deployment container config --name name --resource-group myResourceGroup --enable-cd true
```

İçinde [Azure portalında](https://portal.azure.com/)seçin **App Service** sayfanın sol tarafındaki seçeneği.

Docker Hub sürekli dağıtımı yapılandırmak istediğiniz uygulamanın adını seçin.

Üzerinde **kapsayıcı ayarları** sayfasında **üzerinde**ve ardından **Kaydet** sürekli dağıtımını etkinleştirmek için.

![Uygulama ayarının ekran görüntüsü](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="prepare-the-webhook-url"></a>Web kancası URL'sini hazırlama

Web kancası URL'sini kullanarak elde [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) ve aşağıdaki komut yürütülüyor:

```azurecli-interactive
az webapp deployment container show-cd-url --name sname1 --resource-group rgname
```

Web kancası URL'sini not edin. Sonraki bölümde gerekir.
`https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Alabilirsiniz, `publishingusername` ve `publishingpwd` yayımlama profili Azure portalını kullanarak web uygulamasını yükleyerek.

![Web kancasını 2 ekleme işleminin ekran görüntüsü](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="add-a-webhook"></a>Bir Web kancası Ekle

Bir Web kancası eklemek için bu kılavuzlarındaki adımları izleyin:

- [Azure Container Registry](../../container-registry/container-registry-webhook.md) Web kancası URL'si kullanılarak
- [Docker Hub için Web kancaları](https://docs.docker.com/docker-hub/webhooks/)

## <a name="next-steps"></a>Sonraki adımlar

* [Linux üzerinde Azure App Service'e Giriş](./app-service-linux-intro.md)
* [Azure kapsayıcı kayıt defteri](https://azure.microsoft.com/services/container-registry/)
* [Linux’ta App Service’te .NET Core web uygulaması oluşturma](quickstart-dotnetcore.md)
* [Linux üzerinde App Service'te bir Ruby web uygulaması oluşturma](quickstart-ruby.md)
* [Kapsayıcılar için Web App'te bir Docker/Go web uygulaması dağıtma](quickstart-docker-go.md)
* [Linux’ta Azure App Service hakkında SSS](./app-service-linux-faq.md)
* [Azure CLI kullanarak kapsayıcılar için Web uygulamasını yönetme](./app-service-linux-cli.md)
