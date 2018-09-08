---
title: Azure işlevleri çalışma zamanı sürümlerini hedeflemek nasıl
description: Azure işlevleri çalışma zamanı birden çok sürümünü destekler. Azure'da barındırılan bir işlev uygulaması çalışma zamanı sürümünü belirtin öğrenin.
services: functions
documentationcenter: ''
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 01/24/2018
ms.author: glenga
ms.openlocfilehash: ced4b6846d291bfbb718c3346ea588ca9e961d07
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44093711"
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Azure işlevleri çalışma zamanı sürümlerini hedeflemek nasıl

Bir işlev uygulaması, belirli bir Azure işlevleri çalışma zamanı sürümünde çalışır. İki ana sürümü vardır: [1.x ve 2.x'i](functions-versions.md). Bu makalede seçtiğiniz sürüm üzerinde çalıştırmak için azure'da bir işlev uygulaması yapılandırma açıklanmaktadır. Belirli bir sürümü için bir yerel geliştirme ortamı yapılandırma hakkında daha fazla bilgi için bkz: [kod ve test, Azure işlevleri yerel olarak](functions-run-local.md).

>[!IMPORTANT]   
> Azure işlevleri çalışma zamanı 2.0 Önizleme aşamasındadır ve Azure işlevleri'nin şu anda tüm özellikler desteklenir. Daha fazla bilgi için [Azure işlevleri çalışma zamanı sürümleri genel bakış](functions-versions.md).

## <a name="automatic-and-manual-version-updates"></a>Otomatik ve el ile sürüm güncelleştirmeleri

İşlevleri kullanarak çalışma zamanının belirli bir sürümü hedeflemesini sağlar `FUNCTIONS_EXTENSION_VERSION` bir işlev uygulaması uygulama ayarı. İşlev uygulaması, yeni bir sürümüne taşımak seçtiğiniz kadar belirtilen ana sürümü üzerinde tutulur.

Yalnızca birincil sürüm ("~ 1" 1.x için) veya "beta" 2.x için belirtirseniz, mevcut olduklarında işlev uygulaması için yeni ikincil sürümleri çalışma zamanı otomatik olarak güncelleştirilir. Yeni ikincil sürümler, bozucu değişiklikleri İstemediğimiz. Bir ikincil sürüm (örneğin, "1.0.11360") belirtirseniz, işlev uygulamasını açıkça değiştirene kadar bu sürümünde tutulur. 

Yeni bir sürümü genel kullanıma açık olduğunda, Portalı'nda isteme bu sürümüne nahoru almanıza şans tanır. Yeni bir sürüme taşıdıktan sonra her zaman kullanabilirsiniz `FUNCTIONS_EXTENSION_VERSION` önceki bir sürüme geri taşımak için uygulama ayarı.

Bir değişiklik çalışma zamanı sürümüne yeniden başlatmak bir işlev uygulaması neden olur.

İçinde ayarlanan değerlerle `FUNCTIONS_EXTENSION_VERSION` otomatik güncelleştirmeleri etkinleştirmek üzere bu ayarı uygulama şu anda "~ 1" 1.x çalışma zamanı ve 2.x için "beta".

## <a name="view-the-current-runtime-version"></a>Geçerli çalışma zamanı sürümünü görüntüleme

Şu anda bir işlev uygulaması tarafından kullanılan çalışma zamanı sürümünü görüntülemek için aşağıdaki yordamı kullanın. 

1. İçinde [Azure portalında](https://portal.azure.com), işlev uygulaması ve altında gidin **yapılandırılan özellikler**, seçin **işlev uygulaması ayarları**. 

    ![İşlev uygulaması ayarlarını seçin](./media/functions-versions/add-update-app-setting.png)

2. İçinde **işlev uygulaması ayarları** sekmesinde, bulmak **çalışma zamanı sürümü**. Belirli bir çalışma zamanı sürümünü ve istenen sürümle unutmayın. Aşağıdaki örnekte, İŞLEVLERİN\_UZANTISI\_sürüm uygulama ayarı `~1`.
 
   ![İşlev uygulaması ayarlarını seçin](./media/functions-versions/function-app-view-version.png)

## <a name="target-a-version-using-the-portal"></a>Portalı kullanarak bir sürümü hedefleme

Geçerli ana sürüme dışında sürüm veya sürüm 2.0 hedeflemek gerektiğinde ayarlamak `FUNCTIONS_EXTENSION_VERSION` uygulama ayarı.

1. İçinde [Azure portalında](https://portal.azure.com), işlev uygulamanızı ve altında gidin **yapılandırılan özellikler**, seçin **uygulama ayarları**.

    ![İşlev uygulaması ayarlarını seçin](./media/functions-versions/add-update-app-setting1a.png)

2. İçinde **uygulama ayarları** sekmesinde, bulmak `FUNCTIONS_EXTENSION_VERSION` ayarlama ve çalışma zamanının 1.x geçerli bir sürüm değeri değiştirin veya `beta` sürümü 2.0. Bir tilde ana sürümle (örneğin, "~ 1") bu ana sürüm en son sürümünü kullanmanız anlamına gelir. 

    ![İşlev uygulaması ayarı](./media/functions-versions/add-update-app-setting2.png)

3. Tıklayın **Kaydet** uygulama ayarı güncelleştirmesi kaydetmek için. 

## <a name="target-a-version-using-azure-cli"></a>Azure CLI kullanarak bir sürümü hedefleme

 Ayrıca `FUNCTIONS_EXTENSION_VERSION` Azure clı'dan. Azure CLI'yı kullanarak, işlev uygulaması uygulama ayarı güncelleştirme [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) komutu.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```
Bu kod içinde `<function_app>` işlev uygulamanızın adıyla. Ayrıca değiştirin `<my_resource_group>` işlev uygulamanız için kaynak grubunun adıyla. Değiştirin `<version>` 1.x çalışma zamanı, geçerli bir sürüm ile veya `beta` sürümü 2.0. 

Bu komutu çalıştırabilirsiniz [Azure Cloud Shell](../cloud-shell/overview.md) seçerek **deneyin** önceki kod örneğinde. Ayrıca [Azure CLI'yi yerel olarak](/cli/azure/install-azure-cli) yürütüldükten sonra bu komutu yürütmek için [az login](/cli/azure/reference-index#az-login) oturum açmak için.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerel geliştirme ortamınızda 2.0 çalışma zamanını hedef](functions-run-local.md)

> [!div class="nextstepaction"]
> [Çalışma zamanı sürümleri için yayın notlarına bakın](https://github.com/Azure/azure-webjobs-sdk-script/releases)
