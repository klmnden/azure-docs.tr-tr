---
title: "Beş dakika içinde Azure’da ilk Java web uygulamanızı oluşturma | Microsoft Docs"
description: "Örnek bir uygulama dağıtarak App Service&quot;te web uygulamaları çalıştırmanın ne kadar kolay olduğunu öğrenin."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: ab9b49cd86f37499ebf8fd8162779be305019f36
ms.lasthandoff: 03/17/2017


---
# <a name="create-your-first-java-web-app-in-azure-in-five-minutes"></a>Beş dakika içinde Azure’da ilk Java web uygulamanızı oluşturma
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)]

Bu Hızlı Başlangıç, ilk Java web uygulamanızı birkaç dakika içinde [Azure Uygulama Hizmeti](../app-service/app-service-value-prop-what-is.md)’ne dağıtmanıza yardımcı olur.

Başlamadan önce Azure CLI’nin yüklü olduğundan emin olun. Daha fazla bilgi için bkz. [Azure CLI yükleme kılavuzu](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="log-in-to-azure"></a>Azure'da oturum açma
`az login` çalıştırarak ve ekrandaki yönergeleri izleyerek Azure’da oturum açın.
   
```azurecli
az login
```
   
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma   
Bir [kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. Web uygulaması ve SQL Veritabanı arka ucu gibi birlikte yönetmek istediğiniz tüm Azure kaynaklarını buraya yerleştirirsiniz.

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

`---location` için hangi olası değerleri kullanabileceğinizi görmek için `az appservice list-locations` Azure CLI komutunu kullanın.

## <a name="create-an-app-service-plan"></a>App Service planı oluşturma
"ÜCRETSİZ" bir [App Service planı](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) oluşturun. 

```azurecli
az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
```

## <a name="create-a-web-app"></a>Web uygulaması oluşturma
`<app_name>` içinde benzersiz bir ada sahip bir web uygulaması oluşturun.

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan my-free-appservice-plan
```

## <a name="deploy-sample-application"></a>Örnek uygulama dağıtma
GitHub’dan örnek bir Java uygulaması dağıtın.

```azurecli
az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
--repo-url "https://github.com/azure-appservice-samples/JavaCoffeeShopTemplate.git" --branch master --manual-integration 
```

## <a name="browse-to-web-app"></a>Web uygulamasına göz atma
Uygulamanızı Azure’da çalışırken görmek için şu komutu çalıştırın.

```azurecli
az appservice web browse --name <app_name> --resource-group myResourceGroup
```

Tebrikler, ilk Java web uygulamanız Azure Uygulama Hizmeti’nde çalışıyor.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Önceden oluşturulmuş [Web uygulamaları CLI betiklerini](app-service-cli-samples.md) keşfedin.

