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
ms.date: 03/08/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: e48e03e86a325b8f39809a49cdd19820dfa78bdc
ms.lasthandoff: 03/10/2017


---
# <a name="create-your-first-java-web-app-in-azure-in-five-minutes"></a>Beş dakika içinde Azure’da ilk Java web uygulamanızı oluşturma
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)]

Bu Hızlı Başlangıç, ilk Java web uygulamanızı birkaç dakika içinde [Azure Uygulama Hizmeti](../app-service/app-service-value-prop-what-is.md)’ne dağıtmanıza yardımcı olur.

Bu Hızlı Başlangıç’ı başlatmadan önce, [Azure CLI’nin makinenizde yüklü olduğundan](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) emin olun.

## <a name="create-a-java-web-app-in-azure"></a>Azure’da bir Java web uygulaması oluşturma
2. `az login` çalıştırarak ve ekrandaki yönergeleri izleyerek Azure’da oturum açın.
   
    ```azurecli
    az login
    ```
   
3. Bir [kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. Web uygulaması ve SQL Veritabanı arka ucu gibi birlikte yönetmek istediğiniz tüm Azure kaynaklarını buraya yerleştirirsiniz.

    ```azurecli
    az group create --location "West Europe" --name myResourceGroup
    ```

    `---location` için hangi olası değerleri kullanabileceğinizi görmek için `az appservice list-locations` Azure CLI komutunu kullanın.

3. "ÜCRETSİZ" bir [App Service planı](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) oluşturun. 

    ```azurecli
    az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
    ```

4. `<app_name>` içinde benzersiz bir ada sahip yeni bir web uygulaması oluşturun.

    ```azurecli
    az appservice web create --name <app_name> --resource-group myResourceGroup --plan my-free-appservice-plan
    ```

4. GitHub’dan örnek bir Java uygulaması dağıtın.

    ```azurecli
    az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
    --repo-url "https://github.com/azure-appservice-samples/JavaCoffeeShopTemplate.git" --branch master --manual-integration 
    ```


5. Uygulamanızı Azure’da çalışırken görmek için şu komutu çalıştırın.

    ```azurecli
    az appservice web browse --name <app_name> --resource-group myResourceGroup
    ```

Tebrikler, ilk Java web uygulamanız Azure Uygulama Hizmeti’nde çalışıyor.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Önceden oluşturulmuş [Web uygulamaları CLI betiklerini](app-service-cli-samples.md) keşfedin.

