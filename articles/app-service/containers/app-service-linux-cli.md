---
title: Azure CLI - Azure App Service kullanarak kapsayıcılar için Web uygulamasını yönetme | Microsoft Docs
description: Azure CLI kullanarak kapsayıcılar için Web App yönetin.
keywords: Azure app service, web uygulaması, CLI, linux, oss
services: app-service
documentationCenter: ''
author: ahmedelnably
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: aelnably
ms.custom: seodec18
ms.openlocfilehash: 21f6963fbaada4524f27602454d38e7252a5e8b9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60850093"
---
# <a name="manage-web-app-for-containers-using-azure-cli"></a>Azure CLI kullanarak kapsayıcılar için Web uygulamasını yönetme

Bu makalede Azure CLI kullanarak kapsayıcılar için Web uygulaması oluşturma ve yönetmeden komutları kullanarak.
CLI'ın yeni sürümü iki şekilde kullanmaya başlayabilirsiniz:

* [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) makinenizde.
* Kullanarak [Azure Cloud Shell'i (Önizleme)](../../cloud-shell/overview.md)

## <a name="create-a-linux-app-service-plan"></a>Bir Linux App Service planı oluşturma

Bir Linux App Service planı oluşturmak için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az appservice plan create -n appname -g rgname --is-linux -l "South Central US" --sku S1 --number-of-workers 1
```

## <a name="create-a-custom-docker-container-web-app"></a>Özel bir Docker kapsayıcısı Web uygulaması oluşturma

Bir web uygulaması ve özel bir Docker kapsayıcısını çalıştırma yapılandırma oluşturmak için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -i elnably/dockerimagetest
```

## <a name="activate-the-docker-container-logging"></a>Docker kapsayıcı günlüğe kaydetme etkinleştir

Docker kapsayıcı günlüğe kaydetmeyi etkinleştirmek için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp log config -n sname -g rgname --web-server-logging filesystem
```

## <a name="change-the-custom-docker-container-for-an-existing-web-app-for-containers-app"></a>Özel Docker kapsayıcısı için mevcut bir Web uygulaması için kapsayıcı uygulamasında değiştirin.

Geçerli bir Docker görüntüsü yeni bir görüntüye önceden oluşturulmuş bir uygulama değiştirmek için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp config container set -n sname -g rgname -c apurvajo/mariohtml5
```

## <a name="using-docker-images-from-a-private-registry"></a>Özel bir kayıt defterinden Docker görüntülerini kullanma

Bir özel kayıt defterinizdeki görüntüleri kullanacak şekilde yapılandırabilirsiniz. Kayıt defteri, kullanıcı adı ve parola URL'sini sağlamanız gerekir. Bu, aşağıdaki komutu kullanarak gerçekleştirilebilir:

```azurecli-interactive
az webapp config container set -n sname1 -g rgname -c <container name> -r <server url> -u <username> -p <password>
```

## <a name="enable-continuous-deployments-for-custom-docker-images"></a>Özel Docker görüntüleri için sürekli dağıtımı etkinleştirme

Aşağıdaki komutla CD işlevselliğini etkinleştirmek ve Web kancası URL'sini alın. Bu url, DockerHub veya Azure Container Registry deposu yapılandırmak için kullanılabilir.

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
```

## <a name="create-a-web-app-for-containers-app-using-one-of-our-built-in-runtime-frameworks"></a>Bir Web uygulaması için kapsayıcı bizim yerleşik çalışma zamanı çerçeveleri kullanarak uygulama oluşturma

Kapsayıcı uygulaması için PHP 5.6 Web uygulaması oluşturmak için aşağıdaki komutu kullanabilirsiniz.

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -r "php|5.6"
```

## <a name="change-framework-version-for-an-existing-web-app-for-containers-app"></a>Mevcut bir Web uygulaması için kapsayıcı uygulamasında framework sürümünü Değiştir

Node.js 6.11 geçerli framework sürümünden daha önce oluşturulmuş bir uygulama değiştirmek için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp config set -n sname -g rgname --linux-fx-version "node|6.11"
```

## <a name="set-up-git-deployments-for-your-web-app"></a>Web uygulamanız için Git dağıtımları ayarlama

Uygulamanız için Git dağıtımları ayarlamak için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp deployment source config -n sname -g rgname --repo-url <gitrepo url> --branch <branch>
```

## <a name="next-steps"></a>Sonraki adımlar

* [Linux üzerinde Azure App Service nedir?](app-service-linux-intro.md)
* [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli)
* [Azure Cloud Shell'i (Önizleme)](../../cloud-shell/overview.md)
* [Azure App Service ortamlarında hazırlık ayarlama](../../app-service/deploy-staging-slots.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Kapsayıcılar için Web App ile sürekli dağıtım](app-service-linux-ci-cd.md)
