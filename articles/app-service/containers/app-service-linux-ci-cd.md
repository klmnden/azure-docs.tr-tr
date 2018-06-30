---
title: Docker kapsayıcısı kayıt defteri kapsayıcıları - Azure için Web uygulaması ile sürekli dağıtımı | Microsoft Docs
description: Docker kapsayıcısı kayıt defteri kapsayıcıları için Web uygulamasında sürekli dağıtım nasıl kurulur.
keywords: Azure uygulama hizmeti, linux, docker, acr, oss
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
ms.openlocfilehash: 0f2d4626308eed376b71f1b3df2f9e43f1b2a4f7
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130982"
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Kapsayıcıları için Web uygulaması ile sürekli dağıtımı

Bu öğreticide, bir özel kapsayıcı görüntüsü için sürekli dağıtım yapılandırma yönetilen [Azure kapsayıcı kayıt defteri](https://azure.microsoft.com/services/container-registry/) depoları veya [Docker hub'a](https://hub.docker.com).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="enable-the-continuous-deployment-feature"></a>Sürekli dağıtım özelliğini etkinleştir

Sürekli dağıtım özelliğini kullanarak etkinleştirin [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) ve aşağıdaki komutu yürütme:

```azurecli-interactive
az webapp deployment container config --name name --resource-group myResourceGroup --enable-cd true
```

İçinde [Azure portal](https://portal.azure.com/)seçin **uygulama hizmeti** sayfanın sol tarafında seçeneği.

Docker hub'a sürekli dağıtımı yapılandırmak istediğiniz uygulama adını seçin.

Üzerinde **Docker kapsayıcısı** sayfasında, **üzerinde**ve ardından **kaydetmek** sürekli dağıtımını etkinleştirmek için.

![Uygulama ayarı ekran görüntüsü](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="prepare-the-webhook-url"></a>Web kancası URL'si hazırlama

Web kancası URL'si kullanarak elde [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) ve aşağıdaki komutu yürütme:

```azurecli-interactive
az webapp deployment container show-cd-url --name sname1 --resource-group rgname
```

Web kancası URL'si not edin. Sonraki bölümde gerekir.
`https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Elde edebilirsiniz, `publishingusername` ve `publishingpwd` yayımlama profili Azure Portalı'nı kullanarak web uygulamasını yükleyerek.

![Web kancası 2 ekleme işleminin ekran görüntüsü](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="add-a-webhook"></a>Bir Web kancası ekleme

Bir Web kancası eklemek için bu kılavuzlara'ndaki adımları izleyin:

- [Azure kapsayıcı kayıt defteri](../../container-registry/container-registry-webhook.md) Web kancası URL'si kullanma
- [Docker hub'ına yönelik Web kancaları](https://docs.docker.com/docker-hub/webhooks/)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure uygulama hizmeti Linux'ta giriş](./app-service-linux-intro.md)
* [Azure kapsayıcı kayıt defteri](https://azure.microsoft.com/services/container-registry/)
* [Linux’ta App Service’te .NET Core web uygulaması oluşturma](quickstart-dotnetcore.md)
* [Linux üzerinde App Service'te bir Söyleniş web uygulaması oluşturma](quickstart-ruby.md)
* [Web uygulaması için kapsayıcıları bir Docker/Git web uygulamasını dağıtma](quickstart-docker-go.md)
* [Linux’ta Azure App Service hakkında SSS](./app-service-linux-faq.md)
* [Web uygulaması için Azure CLI kullanarak kapsayıcıları yönetme](./app-service-linux-cli.md)
