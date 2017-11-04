---
title: "Azure işlevleri çalışma zamanı sürümlerini hedefleyen nasıl | Microsoft Docs"
description: "İşlevler çalışma zamanı birden fazla sürümünü destekler. Barındırılan bir Azure çalışma zamanı sürümü belirtin öğrenin işlev uygulaması."
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: glenga
ms.openlocfilehash: 26d4276a0a550d78a9c7657c464bd3320c956fb0
ms.sourcegitcommit: 54fd091c82a71fbc663b2220b27bc0b691a39b5b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2017
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Hedef Azure işlevleri çalışma zamanı sürümlerini nasıl

Azure işlevleri çalışma zamanı Azure içinde sunucusuz kodunuzun yürütülmesini uygular. Bu çalışma zamanı dışında Azure'da barındırılan çeşitli ortamlar bulabilirsiniz. [Azure işlevleri çekirdek Araçları](functions-run-local.md) geliştirme bilgisayarınızda bu çalışma zamanı uygular. [Azure işlevleri çalışma zamanı](functions-runtime-overview.md) , şirket içi ortamlarda işlevlerini kullanmanıza olanak sağlar. 

İşlevler çalışma zamanı birden çok ana sürümleri destekler. Ana sürüm güncelleştirme önemli değişiklikler ortaya çıkarabilir. Bu konuda, Azure üzerinde barındırılan belirli çalışma zamanı sürümü işlevi uygulamalarınıza nasıl hedef açıklanmaktadır. 

İşlevleri kullanarak çalışma zamanının belirli bir ana sürüm hedef olanak tanır `FUNCTIONS_EXTENSION_VERSION` işlev uygulaması uygulama ayarı. Bu iki ortak olarak uygulanır ve önizleme sürümleri. Yeni bir sürüme taşımak seçtiğiniz kadar işlevi uygulamanızı belirtilen ana çalışma zamanı sürümünde tutulur. İşlev uygulaması mevcut olduklarında çalışma zamanının yeni ikincil sürümleri için güncelleştirildi. Alt sürüm güncelleştirmeleri önemli değişiklikler tanıtmak değil.  

Yeni bir ana sürüm genel kullanıma açık olduğunda, işlev uygulaması portalında görüntülediğinizde bu sürüme yukarı taşımak için Fırsat verilir. Yeni bir sürüme taşıdıktan sonra her zaman kullanabilirsiniz `FUNCTIONS_EXTENSION_VERSION` önceki bir çalışma zamanı sürümüne geri taşımak için uygulama ayarı.

Çalışma zamanı sürümü her değişiklik işlevi uygulamanızı yeniden başlatmayı neden olur. Tüm çalışma zamanı sürümleri (birincil ve ikincil) için sürüm notları yayımlanır [GitHub deposunu](https://github.com/Azure/azure-webjobs-sdk-script/releases).   
## <a name="view-the-current-runtime-version"></a>Geçerli çalışma zamanı sürümü görüntüleyin

Şu anda işlevi uygulamanız tarafından kullanılan belirli çalışma zamanı sürümü görüntülemek için aşağıdaki yordamı kullanın. 

1. İçinde [Azure portal](https://portal.azure.com), işlevi uygulamanıza ve altında gidin **yapılandırılan özellikler**, seçin **işlev uygulaması ayarları**. 

    ![İşlev uygulama ayarlarını seçin](./media/functions-versions/add-update-app-setting.png)

2. İçinde **işlev uygulaması ayarları** sekmesinde, bulmak **çalışma zamanı sürümü**. Belirli bir çalışma zamanı sürümü ve istenen ana sürüm unutmayın. Aşağıdaki örnekte, ana sürüm ayarlamak `~1.0`.
 
   ![İşlev uygulama ayarlarını seçin](./media/functions-versions/function-app-view-version.png)

## <a name="target-the-functions-version-20-runtime"></a>Hedef işlevleri sürüm 2.0 çalışma zamanı

>[!IMPORTANT]   
> Azure işlevleri çalışma zamanı 2.0 Önizleme aşamasındadır ve Azure işlevlerinin şu anda tüm özellikler desteklenir. Daha fazla bilgi için bkz: [Azure işlevleri çalışma zamanı 2.0 bilinen sorunlar](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues)  

<!-- Add a table comparing the 1.x and 2.x runtime features-->

[!INCLUDE [functions-set-runtime-version](../../includes/functions-set-runtime-version.md)]

Azure portalında çalışma zamanı sürüm 2.0 Önizleme işlevi uygulamanızı taşıyabilirsiniz. İçinde **işlev uygulaması ayarları** sekmesinde, seçin **beta** altında **çalışma zamanı sürümü**.  

   ![İşlev uygulama ayarlarını seçin](./media/functions-versions/function-app-view-version.png)

Bu ayar ayarına eşdeğerdir `FUNCTIONS_EXTENSION_VERSION` uygulama ayarı için `beta`. Seçin **~ 1** geri geçerli genel olarak taşımak için düğmeyi desteklenen ana sürüm. Azure CLI, bu uygulama ayarını güncelleştirmek için de kullanabilirsiniz. 

## <a name="target-a-specific-runtime-version-from-the-portal"></a>Hedef Portalı'ndan belirli çalışma zamanı sürümü

Geçerli ana dışındaki ana sürüm hedeflemek gerektiğinde sürüm veya sürüm 2.0, ayarlamalısınız `FUNCTIONS_EXTENSION_VERSION` uygulama ayarı.

1. İçinde [Azure portal](https://portal.azure.com), işlevi uygulamanıza ve altında gidin **yapılandırılan özellikler**, seçin **uygulama ayarları**.

    ![İşlev uygulama ayarlarını seçin](./media/functions-versions/add-update-app-setting1a.png)

2. İçinde **uygulama ayarları** sekmesinde, bulmak `FUNCTIONS_EXTENSION_VERSION` ayarlama ve 1.x çalışma zamanı ana geçerli bir sürümüne değerini değiştirin veya `beta` sürüm 2.0 için. 

    ![İşlev uygulama ayarı](./media/functions-versions/add-update-app-setting2.png)

3. Tıklatın **kaydetmek** uygulama ayarı güncelleştirmesi kaydetmek için. 

## <a name="target-a-specific-version-using-azure-cli"></a>Hedef Azure CLI kullanarak belirli bir sürümü

 Ayrıca ayarlayabilirsiniz `FUNCTIONS_EXTENSION_VERSION` Azure clı'dan. Azure CLI kullanarak güncelleştirme ile işlev uygulaması uygulama ayarı [az functionapp config appsettings kümesi](/cli/azure/functionapp/config/appsettings#set) komutu.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```
Bu kodla `<function_app>` işlevi uygulamanızın adı. Ayrıca değiştirin `<my_resource_group>` işlevi uygulamanız için kaynak grubu adı. Değiştir `<version>` 1.x çalışma zamanı geçerli ana sürümü veya `beta` sürüm 2.0 için. 

Bu komutu çalıştırabilirsiniz [Azure bulut Kabuk](../cloud-shell/overview.md) seçerek **deneyin** önceki kod örneğinde. Aynı zamanda [yerel olarak Azure CLI](/cli/azure/install-azure-cli) yürüttükten sonra bu komutu yürütmek için [az oturum açma](/cli/azure#az_login) oturum açmak için.
