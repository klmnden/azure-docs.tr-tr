---
title: Kapsayıcılar - Azure App Service için Web App ile sürekli dağıtım | Microsoft Docs
description: Kapsayıcılar için Web uygulaması sürekli dağıtımı ayarlamak nasıl.
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
ms.date: 11/08/2018
ms.author: yili
ms.custom: seodec18
ms.openlocfilehash: 4acadc4c08ef50e7d52303689c38c43f81187669
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60852541"
---
# <a name="continuous-deployment-with-web-app-for-containers"></a>Kapsayıcılar için Web App ile sürekli dağıtım

Bu öğreticide, bir özel kapsayıcı görüntüsü için sürekli dağıtım yapılandırma öğesinden yönetilen [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) depoları veya [Docker Hub](https://hub.docker.com).

## <a name="enable-continuous-deployment-with-acr"></a>ACR ile sürekli dağıtımı etkinleştirme

![Ekran görüntüsü, ACR Web kancası](./media/app-service-webapp-service-linux-ci-cd/ci-cd-acr-02.png)

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **App Service** sayfanın sol tarafındaki seçeneği.
3. Sürekli dağıtımı yapılandırmak istediğiniz uygulamanın adını seçin.
4. Üzerinde **kapsayıcı ayarları** sayfasında **tek kapsayıcı**
5. Seçin **Azure kapsayıcı kayıt defteri**
6. Seçin **sürekli Dağıtım > üzerinde**
7. Seçin **Kaydet** sürekli dağıtımını etkinleştirmek için.

## <a name="use-the-acr-webhook"></a>ACR Web kancası kullanma

Sürekli dağıtım etkinleştirildikten sonra yeni oluşturulan Web kancası, Azure Container Registry Web kancalarını sayfasında görüntüleyebilirsiniz.

![Ekran görüntüsü, ACR Web kancası](./media/app-service-webapp-service-linux-ci-cd/ci-cd-acr-03.png)

Kapsayıcı kayıt defterinizde, geçerli Web kancaları görüntülemek için Web kancaları'a tıklayın.

## <a name="enable-continuous-deployment-with-docker-hub-optional"></a>Docker Hub (isteğe bağlı) ile sürekli dağıtımı etkinleştirme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **App Service** sayfanın sol tarafındaki seçeneği.
3. Sürekli dağıtımı yapılandırmak istediğiniz uygulamanın adını seçin.
4. Üzerinde **kapsayıcı ayarları** sayfasında **tek kapsayıcı**
5. Seçin **Docker hub'ı**
6. Seçin **sürekli Dağıtım > üzerinde**
7. Seçin **Kaydet** sürekli dağıtımını etkinleştirmek için.

![Uygulama ayarının ekran görüntüsü](./media/app-service-webapp-service-linux-ci-cd/ci-cd-docker-02.png)

Web kancası URL'sini kopyalayın. Docker Hub için bir Web kancası eklemek, takip <a href="https://docs.docker.com/docker-hub/webhooks/" target="_blank">Docker Hub için Web kancaları</a>.

## <a name="next-steps"></a>Sonraki adımlar

* [Linux üzerinde Azure App Service'e Giriş](./app-service-linux-intro.md)
* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
* [Linux’ta App Service’te .NET Core web uygulaması oluşturma](quickstart-dotnetcore.md)
* [Linux üzerinde App Service'te bir Ruby web uygulaması oluşturma](quickstart-ruby.md)
* [Kapsayıcılar için Web App'te bir Docker/Go web uygulaması dağıtma](quickstart-docker-go.md)
* [Linux’ta Azure App Service hakkında SSS](./app-service-linux-faq.md)
* [Azure CLI ile Kapsayıcılar için Web App Yönetimi](./app-service-linux-cli.md)
