---
title: "Hedef Azure işlevleri çalışma zamanı sürümlerini nasıl"
description: "Azure işlevleri çalışma zamanı birden fazla sürümünü destekler. Azure üzerinde barındırılan bir işlev uygulaması çalışma zamanı sürümü belirleme konusunda bilgi edinin."
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: glenga
ms.openlocfilehash: 6fc84642050f4b7acfa2e3c5b4518135d6a97171
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Hedef Azure işlevleri çalışma zamanı sürümlerini nasıl

Bir işlev uygulaması Azure işlevleri çalışma zamanı belirli bir sürümünü çalıştırır. İki ana sürümü vardır: [1.x ve 2.x](functions-versions.md). Bu makalede, bir işlev uygulaması seçtiğiniz sürümü üzerinde çalıştırmak için Azure yapılandırmak açıklanmaktadır. Belirli bir sürümü için bir yerel geliştirme ortamı yapılandırma hakkında daha fazla bilgi için bkz: [koduna ve test Azure işlevleri yerel olarak](functions-run-local.md).

>[!IMPORTANT]   
> Azure işlevleri çalışma zamanı 2.0 Önizleme aşamasındadır ve Azure işlevlerinin şu anda tüm özellikler desteklenir. Daha fazla bilgi için bkz: [Azure işlevleri çalışma zamanı sürümleri genel bakış](functions-versions.md).

## <a name="automatic-and-manual-version-updates"></a>Otomatik ve el ile sürüm güncelleştirmeleri

Çalışma zamanı belirli bir sürümünü kullanarak hedef işlevleri sağlar `FUNCTIONS_EXTENSION_VERSION` bir işlev uygulaması uygulama ayarı. Yeni bir sürüme taşımak seçtiğiniz kadar işlev uygulaması belirtilen ana sürümü tutulur.

Yalnızca birincil sürüm (~ için "1" 1.x) veya "beta" 2.x için belirtirseniz, mevcut olduklarında işlev uygulaması çalışma zamanının yeni ikincil sürümleri için otomatik olarak güncelleştirilir. Yeni ikincil sürümleri önemli değişiklikler tanıtmak değil. Alt sürüm (örneğin, "1.0.11360") belirtirseniz, işlev uygulaması açıkça değiştirene kadar bu sürümünde tutulur. 

Yeni bir sürüm genel kullanıma açık olduğunda, Portalı'nda isteme, bu sürüme yukarı olanağı sağlar. Yeni bir sürüme taşıdıktan sonra her zaman kullanabilirsiniz `FUNCTIONS_EXTENSION_VERSION` bir önceki sürüme geri taşımak için uygulama ayarı.

Çalışma zamanı sürümü değişiklik yeniden başlatmak bir işlev uygulaması neden olur.

İçinde ayarlanan değerlerle `FUNCTIONS_EXTENSION_VERSION` otomatik güncelleştirmeleri etkinleştirmek üzere bu ayarı uygulama şu anda olan ~ için "1" 1.x çalışma zamanı ve 2.x için "beta".

## <a name="view-the-current-runtime-version"></a>Geçerli çalışma zamanı sürümü görüntüleyin

Şu anda bir işlev uygulaması tarafından kullanılan çalışma zamanı sürümü görüntülemek için aşağıdaki yordamı kullanın. 

1. İçinde [Azure portal](https://portal.azure.com), işlev uygulaması için ve altında gidin **yapılandırılan özellikler**, seçin **işlev uygulaması ayarları**. 

    ![İşlev uygulama ayarlarını seçin](./media/functions-versions/add-update-app-setting.png)

2. İçinde **işlev uygulaması ayarları** sekmesinde, bulmak **çalışma zamanı sürümü**. Belirli bir çalışma zamanı sürümü ve istenen ana sürüm unutmayın. Aşağıdaki örnekte, İŞLEVLERİ\_UZANTISI\_sürüm uygulama ayar `~1`.
 
   ![İşlev uygulama ayarlarını seçin](./media/functions-versions/function-app-view-version.png)

## <a name="target-a-version-using-the-portal"></a>Hedef Portalı'nı kullanarak bir sürüm

Sürüm geçerli sürümle dışında ya da sürüm 2.0 hedef gerektiğinde ayarlamak `FUNCTIONS_EXTENSION_VERSION` uygulama ayarı.

1. İçinde [Azure portal](https://portal.azure.com), işlevi uygulamanıza ve altında gidin **yapılandırılan özellikler**, seçin **uygulama ayarları**.

    ![İşlev uygulama ayarlarını seçin](./media/functions-versions/add-update-app-setting1a.png)

2. İçinde **uygulama ayarları** sekmesinde, bulmak `FUNCTIONS_EXTENSION_VERSION` ayarlama ve değeri 1.x çalışma zamanı geçerli bir sürüm olarak değiştirin veya `beta` sürüm 2.0 için. Bir tilde ana sürümü bu sürümle (örneğin, "~ 1") en son sürümünü kullanmanız anlamına gelir. 

    ![İşlev uygulama ayarı](./media/functions-versions/add-update-app-setting2.png)

3. Tıklatın **kaydetmek** uygulama ayarı güncelleştirmesi kaydetmek için. 

## <a name="target-a-version-using-azure-cli"></a>Hedef Azure CLI kullanarak bir sürüm

 Ayrıca ayarlayabilirsiniz `FUNCTIONS_EXTENSION_VERSION` Azure clı'dan. Azure CLI kullanarak güncelleştirme ile işlev uygulaması uygulama ayarı [az functionapp config appsettings kümesi](/cli/azure/functionapp/config/appsettings#set) komutu.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```
Bu kodla `<function_app>` işlevi uygulamanızın adı. Ayrıca değiştirin `<my_resource_group>` işlevi uygulamanız için kaynak grubu adı. Değiştir `<version>` 1.x çalışma zamanı geçerli sürümü veya `beta` sürüm 2.0 için. 

Bu komutu çalıştırabilirsiniz [Azure bulut Kabuk](../cloud-shell/overview.md) seçerek **deneyin** önceki kod örneğinde. Aynı zamanda [yerel olarak Azure CLI](/cli/azure/install-azure-cli) yürüttükten sonra bu komutu yürütmek için [az oturum açma](/cli/azure/reference-index#az_login) oturum açmak için.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hedef yerel geliştirme ortamınızda 2.0 çalışma zamanı](functions-run-local.md)

> [!div class="nextstepaction"]
> [Çalışma zamanı sürümleri için sürüm notlarına bakın](https://github.com/Azure/azure-webjobs-sdk-script/releases)