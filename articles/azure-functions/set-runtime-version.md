---
title: Azure işlevleri çalışma zamanı sürümleri nasıl hedeflenir?
description: Azure işlevleri çalışma zamanında birden çok sürümünü destekler. Azure'da barındırılan bir işlev uygulamasında çalışma zamanı sürümünü belirtmeyi öğrenin.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 10/05/2018
ms.author: glenga
ms.openlocfilehash: 6d89746c0a2d4642e5025789d352803195c0a3b9
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886816"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Azure işlevleri çalışma zamanı sürümleri nasıl hedeflenir?

Bir işlev uygulaması, belirli bir Azure işlevleri çalışma zamanı sürümünde çalışır. İki ana sürüm vardır: [1.x ve 2.x'i](functions-versions.md). Varsayılan olarak tüm Azure işlevleri 2.x çalışma zamanında çalışır. Bu makalede bir işlev uygulamasının seçtiğiniz sürüm üzerinde çalıştırılacak şekilde yapılandırılması açıklanmaktadır. Belirli bir sürümü için bir yerel geliştirme ortamı yapılandırma hakkında daha fazla bilgi için bkz: [kod ve test, Azure işlevleri yerel olarak](functions-run-local.md).

> [!NOTE]
> Bir veya daha fazla işleve sahip bir işlev uygulaması için çalışma zamanı sürümünü değiştiremezsiniz. Çalışma zamanı sürümü değiştirmek için Azure portalını kullanmanız gerekir.

## <a name="automatic-and-manual-version-updates"></a>Otomatik ve el ile sürüm güncelleştirmeleri

`FUNCTIONS_EXTENSION_VERSION` işlev uygulaması uygulama ayarını kullanarak çalışma zamanında belirli bir sürümün hedeflemesini sağlayabilirsiniz. İşlev uygulaması, yeni bir sürüm çıksa dahi sizin belirttiğiniz ana sürüm üzerinde tutulur.

Yalnızca birincil sürüm (2.x için "~2" veya 1.x için "~1") belirtirseniz, yeni ikincil sürümler yayınlandığında işlev uygulaması çalışma zamanı otomatik olarak güncelleştirilir. Yeni ikincil sürümler, uygulamaları kıracak değişiklikler içermez. Eğer ikincil sürüm (örneğin, "2.0.12345") belirtirseniz, işlev uygulamasını siz değiştirene kadar belirttiğiniz sürümüne sabitlenir.

Yeni bir sürüm genel kullanıma açıldığında, Azure Portal'da göreceğiniz bir uyarı size uygulamanızın çalışma zamanını yükseltme şansı tanır. Yeni bir sürüme taşındıktan sonra her zaman `FUNCTIONS_EXTENSION_VERSION` ayarını kullanarak önceki bir sürüme geri dönebilirsiniz.

Çalışma zamanı sürümünde değişiklik yapmak uygulamanızın yeniden başlatılmasına neden olur.

`FUNCTIONS_EXTENSION_VERSION` ayarı ile otomatik güncelleştirmeleri etkinleştirmek için şu anda kullanabileceğiniz değerler 1.x çalışma zamanı için "~ 1" ve 2 2.x için "~".

## <a name="view-and-update-the-current-runtime-version"></a>Geçerli çalışma zamanı sürümünü görüntüleme ve güncelleştirme

Aşağıdaki adımları kullanarak bir işlev uygulamasının kullandığı mevcut çalışma zamanı sürümünü görüntüleyebilirsiniz.

1. [Azure portalında](https://portal.azure.com) işlev uygulamanızı bulun.

1. Altında **yapılandırılan özellikler**, seçin **işlev uygulaması ayarları**.

    ![İşlev uygulaması ayarlarını seçin](./media/set-runtime-version/add-update-app-setting.png)

1.**İşlev uygulaması ayarları** sekmesinde, **çalışma zamanı sürümü** bilgisini bulabilirsiniz. Burada mevcut çalışma zamanı sürümünü ve istenen sürümü görebilirsiniz. Aşağıdaki örnekte, seçili sürüm `~2`.

   ![İşlev uygulaması ayarlarını seçin](./media/set-runtime-version/function-app-view-version.png)

1. İşlev uygulamanızı sürüm 1.x çalışma zamanına sabitlemek için **çalışma zamanı sürümü** altında **~1** seçeneğini seçin. İşlev uygulamanızda mevcut işlevler olduğunda bu anahtar devre dışı bırakıldı.

1. Çalışma zamanı sürümünü değiştirdiyseniz **genel bakış** sekmesini dönün ve **yeniden başlat** düğmesine basarak uygulamayı yeniden başlatınız. İşlev uygulamanız yeniden başlatıldıktan sonra 1.x çalışma zamanını seçtiyseniz tüm yeni işlevler için sürüm 1.x şablonları kullanılır.

## <a name="view-and-update-the-runtime-version-using-azure-cli"></a>Azure CLI ile çalışma zamanı sürümünü görüntüleme ve güncelleştirme

Ayrıca `FUNCTIONS_EXTENSION_VERSION` ayarını Azure CLI üzerinden de görüntüleyebilir ve değiştirebilirsiniz.

>[!NOTE]
>İşlev uygulamanızla ilgili farklı ayarlar çalışma zamanı sürümü değişiklikleri tarafından etkilenebilir. Bu nedenle çalışma zamanı sürümü değişikliklerini Azure Portal'ından değiştirmeniz tavsiye edilir. Çalışma zamanı sürümünü portaldan değiştirdiğinizde Node.js sürümü ve çalışma zamanı yığını gibi diğer gerekli güncelleştirmeleri de portal otomatik olarak yapar.  

Azure CLI'yı kullanarak, geçerli çalışma zamanı sürümünü görüntülemek için [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) komutunu kullanabilirsiniz.

```azurecli-interactive
az functionapp config appsettings list --name <function_app> \
--resource-group <my_resource_group>
```

Bu kod içinde `<function_app>` işlev uygulamanızın adıyla değiştirin. Ayrıca `<my_resource_group>` da işlev uygulamanızın içinde bulunduğu kaynak grubunun adıyla değiştirilmeli. 

Aşağıdaki çıktıda `FUNCTIONS_EXTENSION_VERSION` altında mevcut çalışma zamanı sürümüne ait bilgiyi bulabilirsiniz. Aşağıdaki örnek çıktı kolay okunabilmesi için kısaltılmıştır.

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

`FUNCTIONS_EXTENSION_VERSION` işlev uygulaması ayarını [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) komutu ile güncelleştirebilirsiniz.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```

Bu kod içinde `<function_app>` işlev uygulamanızın adıyla değiştirin. Ayrıca `<my_resource_group>` da işlev uygulamanızın içinde bulunduğu kaynak grubunun adıyla değiştirilmeli. Ek olarak `<version>` kısmını da istediğiniz bir 1.x çalışma zamanı veya geçerli bir  2.x sürümü için `~2` ile değiştirebilirsiniz.

Bu komutu [Azure Cloud Shell](../cloud-shell/overview.md) üzerinde de çalıştırabilirsiniz. seçerek **deneyin** önceki kod örneğinde. Ayrıca [az login](/cli/azure/reference-index#az-login) oturum açarak [Azure CLI'yi yerel olarak](/cli/azure/install-azure-cli) çalıştırıp yukarıdaki komutları da çalıştırabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerel geliştirme ortamınızda 2.0 çalışma zamanını hedeflemek](functions-run-local.md)

> [!div class="nextstepaction"]
> [Çalışma zamanı sürümleri için yayın notlarına bakın](https://github.com/Azure/azure-webjobs-sdk-script/releases)
