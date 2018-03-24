---
title: Docker kapsayıcısı kayıt defteri kapsayıcıları - Azure için Web uygulaması ile sürekli dağıtımı | Microsoft Docs
description: Docker kapsayıcısı kayıt defteri kapsayıcıları için Web uygulamasında sürekli dağıtım nasıl kurulur.
keywords: Azure uygulama hizmeti, linux, docker, acr, oss
services: app-service
documentationcenter: ''
author: ahmedelnably
manager: cfowler
editor: ''
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: aelnably;msangapu
ms.openlocfilehash: ac35dbd041de50ab8aae1a0fb4c00fe3917a7297
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
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

Web kancası URL'si, aşağıdaki bitiş noktasını gerekir: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Elde edebilirsiniz, `publishingusername` ve `publishingpwd` yayımlama profili Azure Portalı'nı kullanarak web uygulamasını yükleyerek.

![Web kancası 2 ekleme işleminin ekran görüntüsü](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="add-a-webhook"></a>Bir Web kancası ekleme

### <a name="azure-container-registry"></a>Azure Container Kayıt Defteri

1. Kayıt defteri portal sayfasında, seçin **Kancalarını**.
2. Yeni bir Web kancası oluşturmak için seçin **Ekle**. 
3. İçinde **Web kancası oluşturma** bölmesinde, Web kancası bir ad verin. Web kancası için URI, önceki bölümde edindiğiniz URL'sini sağlayın.

Kapsayıcı görüntüsünü içeren depo kapsamını tanımlamak emin olun.

![Web kancası ekran görüntüsü](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

Görüntü güncelleştirdiğinizde, web uygulaması ile yeni bir imaj otomatik olarak güncelleştirilir.

### <a name="docker-hub"></a>Docker Hub

Docker hub'a sayfanızda seçin **Kancalarını**ve ardından **bir Web KANCASI oluşturma**.

![Web kancası 1 ekleme işleminin ekran görüntüsü](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

Web kancası URL'si, daha önce aldığınız URL'sini sağlayın.

![Web kancası 2 ekleme işleminin ekran görüntüsü](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

Görüntü güncelleştirdiğinizde, web uygulaması ile yeni bir imaj otomatik olarak güncelleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure uygulama hizmeti Linux'ta giriş](./app-service-linux-intro.md)
* [Azure kapsayıcı kayıt defteri](https://azure.microsoft.com/services/container-registry/)
* [Linux’ta App Service’te .NET Core web uygulaması oluşturma](quickstart-dotnetcore.md)
* [Linux üzerinde App Service'te bir Söyleniş web uygulaması oluşturma](quickstart-ruby.md)
* [Web uygulaması için kapsayıcıları bir Docker/Git web uygulamasını dağıtma](quickstart-docker-go.md)
* [Linux’ta Azure App Service hakkında SSS](./app-service-linux-faq.md)
* [Web uygulaması için Azure CLI kullanarak kapsayıcıları yönetme](./app-service-linux-cli.md)
