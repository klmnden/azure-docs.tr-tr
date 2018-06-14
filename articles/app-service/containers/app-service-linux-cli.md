---
title: Web uygulaması için Azure CLI 2.0 kullanarak kapsayıcıları yönetme | Microsoft Docs
description: Web uygulaması için Azure CLI kullanarak kapsayıcıları yönetin.
keywords: Azure uygulama hizmeti, web uygulaması, CLI, linux, oss
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
ms.openlocfilehash: 54c979313a6ffa43008aa9870332b92d2b2f182a
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "24105398"
---
# <a name="manage-web-app-for-containers-using-azure-cli"></a>Web uygulaması için Azure CLI kullanarak kapsayıcıları yönetme

Bu makalede komutları kullanarak oluşturmak ve bir Web uygulaması için Azure CLI 2.0 kullanarak kapsayıcıları yönetmek kullanabilirsiniz.
CLI yeni sürümünü kullanarak iki yolla başlatabilirsiniz:

* [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) makinenizde.
* Kullanarak [Azure bulut Kabuğu (Önizleme)](../../cloud-shell/overview.md)

## <a name="create-a-linux-app-service-plan"></a>Linux uygulama hizmeti planı oluştur

Bir Linux App Service planı oluşturmak için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az appservice plan create -n appname -g rgname --is-linux -l "South Central US" --sku S1 --number-of-workers 1
```

## <a name="create-a-custom-docker-container-web-app"></a>Özel bir Docker kapsayıcısı Web uygulaması oluşturma

Bir web uygulaması ve özel bir Docker kapsayıcısı çalıştırmak için yapılandırma oluşturmak için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -i elnably/dockerimagetest
```

## <a name="activate-the-docker-container-logging"></a>Docker kapsayıcısı günlüğe kaydetmeyi etkinleştirmek

Docker kapsayıcısı günlüğe kaydetmeyi etkinleştirmek için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp log config -n sname -g rgname --web-server-logging filesystem
```

## <a name="change-the-custom-docker-container-for-an-existing-web-app-for-containers-app"></a>Özel Docker kapsayıcısı kapsayıcıları uygulama için var olan bir Web uygulaması için değiştirme

Yeni bir görüntüye geçerli Docker görüntüden daha önce oluşturulmuş bir uygulama değiştirmek için aşağıdaki komutu kullanabilirsiniz:

```azurecli-interactive
az webapp config container set -n sname -g rgname -c apurvajo/mariohtml5
```

## <a name="using-docker-images-from-a-private-registry"></a>Özel bir kayıt defterinden Docker görüntüleri kullanma

Özel bir kayıt defterinden görüntüleri kullanmak için uygulamanızı yapılandırabilirsiniz. Kayıt defteri, kullanıcı adı ve parola URL'sini sağlamanız gerekir. Bu aşağıdaki komutu kullanarak elde edilir:

```azurecli-interactive
az webapp config container set -n sname1 -g rgname -c <container name> -r <server url> -u <username> -p <password>
```

## <a name="enable-continuous-deployments-for-custom-docker-images"></a>Özel Docker görüntüleri için sürekli dağıtımı etkinleştirme

Aşağıdaki komutla CD işlevselliğini etkinleştirmek ve Web kancası URL'si alın. Bu url, DockerHub ya da Azure kapsayıcı kayıt depoları yapılandırmak için kullanılabilir.

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
```

## <a name="create-a-web-app-for-containers-app-using-one-of-our-built-in-runtime-frameworks"></a>Kapsayıcıları bizim yerleşik çalışma zamanı çerçeveleri birini kullanarak uygulama için bir Web uygulaması oluşturma

Kapsayıcıları uygulaması için bir PHP 5.6 Web uygulaması oluşturmak için aşağıdaki komutu kullanabilirsiniz.

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -r "php|5.6"
```

## <a name="change-framework-version-for-an-existing-web-app-for-containers-app"></a>Kapsayıcıları için var olan bir Web uygulaması için framework sürümünü Değiştir

Önceden oluşturulmuş bir uygulama Node.js 6.11 geçerli framework sürümüne değiştirmek için aşağıdaki komutu kullanabilirsiniz:

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
* [Azure CLI 2.0 yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli)
* [Azure bulut Kabuğu (Önizleme)](../../cloud-shell/overview.md)
* [Hazırlık Azure App Service ortamları ayarlama](../../app-service/web-sites-staged-publishing.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Web uygulaması ile sürekli dağıtımın kapsayıcıları için](app-service-linux-ci-cd.md)
