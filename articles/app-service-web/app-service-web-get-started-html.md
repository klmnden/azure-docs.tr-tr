---
title: "Beş dakika içinde Azure’da statik HTML web uygulaması oluşturma | Microsoft Docs"
description: "Örnek bir uygulama dağıtarak App Service&quot;te web uygulamaları çalıştırmanın ne kadar kolay olduğunu öğrenin."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/08/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: 3a618e4dfed5e941fb0200f4f21a6a45e33d948e
ms.lasthandoff: 03/15/2017


---
# <a name="create-a-static-html-web-app-in-azure-in-five-minutes"></a>Beş dakika içinde Azure’da statik HTML web uygulaması oluşturma
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Bu Hızlı Başlangıç, basit bir HTML+CSS sitesini birkaç dakika içinde [Azure Uygulama Hizmeti](../app-service/app-service-value-prop-what-is.md)’ne dağıtmanıza yardımcı olur.

Bu Hızlı Başlangıç’ı başlatmadan önce, [Azure CLI’nin makinenizde yüklü olduğundan](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) emin olun.

## <a name="create-a-static-html-site-in-azure"></a>Azure’da statik HTML sitesi oluşturma
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

4. GitHub’dan örnek bir HTML sitesi dağıtın.

    ```azurecli
    az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
    --repo-url "https://github.com/Azure-Samples/app-service-web-html-get-started.git" --branch master --manual-integration 
    ```

5. Uygulamanızı Azure’da çalışırken görmek için şu komutu çalıştırın.

    ```azurecli
    az appservice web browse --name <app_name> --resource-group myResourceGroup
    ```

Tebrikler, ilk statik HTML siteniz Azure Uygulama Hizmeti’nde çalışıyor.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Önceden oluşturulmuş [Web uygulamaları CLI betiklerini](app-service-cli-samples.md) keşfedin.

