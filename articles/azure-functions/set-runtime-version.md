---
title: Azure İşlevleri çalışma zamanı sürümleri nasıl hedeflenir?
description: Azure İşlevleri, birden fazla çalışma zamanı sürümünü destekler. Azure'da barındırılan bir işlev uygulamasında çalışma zamanı sürümünü nasıl belirteceğinizi öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: glenga
ms.openlocfilehash: 6e8142e391dd02e78be42e1f16ae2626b74c41c3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60325532"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Azure İşlevleri çalışma zamanı sürümleri nasıl hedeflenir?

Bir işlev uygulaması, Azure İşlevleri çalışma zamanının belirli bir sürümünde çalışır. İki ana sürümü vardır: [1.x ve 2.x'i](functions-versions.md). İşlev uygulamaları, varsayılan olarak çalışma zamanının 2.x sürümünde oluşturulur. Bu makalede, Azure'daki bir işlev uygulamasının seçtiğiniz sürüm üzerinde çalıştırılacak şekilde nasıl yapılandırılacağı açıklanmaktadır. Yerel bir geliştirme ortamını belirli bir sürüm için yapılandırma hakkında bilgi için bkz.[Code and test Azure Functions locally (Azure İşlevleri'ni yerel olarak kodlama ve test etme)](functions-run-local.md).

> [!NOTE]
> Bir veya daha fazla işleve sahip bir işlev uygulaması için çalışma zamanı sürümünü değiştiremezsiniz. Çalışma zamanı sürümünü değiştirmek için Azure portal'ı kullanmanız gerekir.

## <a name="automatic-and-manual-version-updates"></a>Otomatik ve el ile sürüm güncelleştirmeleri

İşlevler, bir işlev uygulamasında `FUNCTIONS_EXTENSION_VERSION` uygulama ayarını kullanarak çalışma zamanının belirli bir sürümünü hedeflemenize olanak sağlar. İşlev uygulaması siz açıkça yeni bir sürüme geçene kadar belirtilen ana sürüm üzerinde tutulur.

Yalnızca ana sürümü (2.x için "~2" veya 1.x için "~1") belirtirseniz işlev uygulaması, çalışma zamanının yeni ikincil sürümleri kullanıma sunulduğunda otomatik olarak bu sürümlere güncelleştirilir. Yeni ikincil sürümler hataya neden olan değişiklikler içermez. İkincil bir sürüm (örneğin, "2.0.12345") belirtirseniz işlev uygulaması, siz açıkça değiştirene kadar ilgili sürüme sabitlenir.

Yeni bir sürüm genel kullanıma sunulduğunda portalda görüntülenecek bir istem size yazılımınızı söz konusu sürüme yükseltme olanağı sunar. Yeni bir sürüme yükseltme yaptıktan sonra dilediğiniz zaman `FUNCTIONS_EXTENSION_VERSION` ayarını kullanarak önceki bir sürüme geri dönebilirsiniz.

Çalışma zamanı sürümündeki değişiklikler işlev uygulamanızın yeniden başlatılmasına neden olur.

Otomatik güncelleştirmeleri etkinleştirmek için `FUNCTIONS_EXTENSION_VERSION` uygulama ayarında şu anda 1.x çalışma zamanı için "~1" ve 2.x için "~2" değerini belirleyebilirsiniz.

## <a name="view-and-update-the-current-runtime-version"></a>Geçerli çalışma zamanı sürümünü görüntüleme ve güncelleştirme

Tarafından kullanılan çalışma zamanı sürümünü değiştirebilirsiniz işlev uygulaması. İşlev uygulamanızda herhangi bir işlev oluşturmadan önce bozucu değişiklikler olasılığı nedeniyle, yalnızca çalışma zamanı sürümünü değiştirmeniz gerekir. Çalışma zamanı sürümü tarafından belirlenir ancak `FUNCTIONS_EXTENSION_VERSION` ayarı, bu değişikliği Azure portalında ve ayarı değiştirerek değil siz doğrudan. Portal, değişikliklerinizi doğrular ve ilgili diğer değişiklikleri gerektiği gibi yapar olmasıdır.

### <a name="from-the-azure-portal"></a>Azure portalından

[!INCLUDE [Set the runtime version in the portal](../../includes/functions-view-update-version-portal.md)]

### <a name="view-and-update-the-runtime-version-using-azure-cli"></a>Azure CLI

`FUNCTIONS_EXTENSION_VERSION` ayarını Azure CLI'sinden de görüntüleyebilir ve belirleyebilirsiniz.

>[!NOTE]
>Çalışma zamanı sürümü diğer ayarları etkileyebileceğinden sürümü portalda değiştirmeniz gerekir. Çalışma zamanı sürümlerini değiştirdiğinizde portal, Node.js sürümü ve çalışma zamanı yığını gibi diğer gerekli güncelleştirmeleri de otomatik olarak yapar.  

Azure CLI'sini kullanarak [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings) komutuyla geçerli çalışma zamanı sürümünü görüntüleyebilirsiniz.

```azurecli-interactive
az functionapp config appsettings list --name <function_app> \
--resource-group <my_resource_group>
```

Bu kodun `<function_app>` kısmını işlev uygulamanızın adıyla, `<my_resource_group>` kısmını ise işlev uygulamanıza ilişkin kaynak grubunun adıyla değiştirin. 

Kolayca anlaşılması için kesilmiş olan aşağıdaki çıktıda `FUNCTIONS_EXTENSION_VERSION` ayarını görebilirsiniz.

```output
[
  {
    "name": "FUNCTIONS_EXTENSION_VERSION",
    "slotSetting": false,
    "value": "~2"
  },
  {
    "name": "FUNCTIONS_WORKER_RUNTIME",
    "slotSetting": false,
    "value": "dotnet"
  },
  
  ...
  
  {
    "name": "WEBSITE_NODE_DEFAULT_VERSION",
    "slotSetting": false,
    "value": "8.11.1"
  }
]
```

`FUNCTIONS_EXTENSION_VERSION` işlev uygulaması ayarını [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings) komutu ile güncelleştirebilirsiniz.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```

Değiştirin `<function_app>` işlev uygulamanızın adıyla. `<my_resource_group>` kısmını ise işlev uygulamanıza ilişkin kaynak grubunun adıyla değiştirin. Ayrıca `<version>`kısmını, 1.x çalışma zamanının geçerli bir sürümüyle veya 2.x sürümü için `~2` ile değiştirin.

Önceki kod örneğinde **Deneyin** seçeneğini belirleyerek bu komutu [Azure Cloud Shell](../cloud-shell/overview.md) üzerinde de çalıştırabilirsiniz. Oturum açmak için [az login](/cli/azure/reference-index#az-login) komutunu yürüttükten sonra bu komutu yürütmek için Azure [Azure CLI'yi yerel olarak](/cli/azure/install-azure-cli) da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerel geliştirme ortamınızda 2.0 çalışma zamanını hedefleyin](functions-run-local.md)

> [!div class="nextstepaction"]
> [Çalışma zamanı sürümleri için yayın notlarına bakın](https://github.com/Azure/azure-webjobs-sdk-script/releases)
