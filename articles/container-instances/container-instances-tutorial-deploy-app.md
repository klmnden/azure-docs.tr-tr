---
title: "Azure kapsayıcı örnekleri Öğreticisi - app dağıtma"
description: "Azure kapsayıcı örnekleri Öğreticisi - app dağıtma"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/26/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 3b651526f5ee3197e7d04accb6a87e2f10bf0791
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="deploy-a-container-to-azure-container-instances"></a>Azure kapsayıcı örnekleri bir kapsayıcı dağıtma

Son üç bölümlük öğreticinin budur. Önceki bölümlerde [bir kapsayıcı görüntüsü oluşturuldu](container-instances-tutorial-prepare-app.md) ve [bir Azure kapsayıcı kayıt defterine gönderilen](container-instances-tutorial-prepare-acr.md). Bu bölümde, Azure kapsayıcı örnekleri kapsayıcı dağıtarak öğretici tamamlar. Tamamlanan adımları içerir:

> [!div class="checklist"]
> * Azure CLI kullanarak Azure kapsayıcı kayıt defterinden kapsayıcı dağıtma
> * Uygulamayı tarayıcıda görüntüleme
> * Kapsayıcı günlüklerini görüntüleme

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğretici, Azure CLI Sürüm 2.0.20 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

Bu öğreticiyi tamamlamak için Docker geliştirme ortamı gerekir. Docker, [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) veya [Linux](https://docs.docker.com/engine/installation/#supported-platforms)’ta Docker’ı kolayca yapılandırmanızı sağlayan paketler sağlar.

Azure bulut Kabuk her adımı tamamlamak için gereken Docker bileşenleri Bu öğretici içermez. Bu nedenle, Azure CLI ve Docker geliştirme ortamı yerel bir yüklemesini öneririz.

## <a name="deploy-the-container-using-the-azure-cli"></a>Azure CLI kullanarak kapsayıcı dağıtma

Azure CLI Azure kapsayıcı örnekleri için bir kapsayıcı tek bir komut içinde dağıtımını sağlar. Kapsayıcı görüntü özel Azure kapsayıcı kayıt defterinde barındırılan beri erişmek için gerekli kimlik bilgilerini eklemeniz gerekir. Gerekirse, aşağıda gösterildiği gibi bunları sorgulayabilirsiniz.

Kapsayıcı kayıt defteri oturum açma sunucusu (kayıt defteri adınızı güncelleştirmesiyle):

```azurecli
az acr show --name <acrName> --query loginServer
```

Kapsayıcı kayıt defteri parola:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

Kapsayıcı görüntünüzü 1 CPU çekirdek kaynak isteği ve 1 GB bellek ile kapsayıcı kayıt defterinden dağıtmak için aşağıdaki komutu çalıştırın:

```azurecli
az container create --name aci-tutorial-app --image <acrLoginServer>/aci-tutorial-app:v1 --cpu 1 --memory 1 --registry-password <acrPassword> --ip-address public -g myResourceGroup
```

Birkaç saniye içinde Azure Kaynak Yöneticisi'nden bir ilk yanıt almanız gerekir. Dağıtım durumunu görüntülemek için kullanın:

```azurecli
az container show --name aci-tutorial-app --resource-group myResourceGroup --query state
```

Biz durumu değişiklikleri kadar bu komut çalışmaya devam *bekleyen* için *çalıştıran*. Ardından biz geçebilirsiniz.

## <a name="view-the-application-and-container-logs"></a>Uygulama ve kapsayıcı günlükleri görüntüleme

Dağıtım başarılı olduktan sonra aşağıdaki komutu çıktıda gösterilen IP adresi için tarayıcınızı açın:

```bash
az container show --name aci-tutorial-app --resource-group myResourceGroup --query ipAddress.ip
```

```json
"13.88.176.27"
```

![Hello world uygulamayı tarayıcıda][aci-app-browser]

Ayrıca, kapsayıcı günlük çıktısı görüntüleyebilirsiniz:

```azurecli
az container logs --name aci-tutorial-app -g myResourceGroup
```

Çıktı:

```bash
listening on port 80
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.0.4 - - [21/Jul/2017:06:00:02 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://13.88.176.27/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure kapsayıcı örnekleri, kapsayıcıları dağıtma işlemi tamamlandı. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Azure CLI kullanarak Azure kapsayıcı kayıt defterinden kapsayıcı dağıtma
> * Uygulamayı tarayıcıda görüntüleme
> * Kapsayıcı günlüklerini görüntüleme

<!-- LINKS -->
[prepare-app]: ./container-instances-tutorial-prepare-app.md

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
