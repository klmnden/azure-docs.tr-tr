---
title: "Hedef Azure işlevleri çalışma zamanı sürümlerini nasıl"
description: "Azure işlevleri çalışma zamanı birden fazla sürümünü destekler. Barındırılan bir Azure çalışma zamanı sürümü belirtin öğrenin işlev uygulaması."
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2017
ms.author: glenga
ms.openlocfilehash: 063232e40b30d03b0ee8b087a602fed0fee3be0a
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Hedef Azure işlevleri çalışma zamanı sürümlerini nasıl

Bir işlev uygulaması Azure işlevleri çalışma zamanı belirli bir sürümünü çalıştırır. İki ana sürümü vardır: 1.x ve 2.x. Bu makalede, kullanılacak ana sürüm ve sürümü çalıştırdığınız Azure'da bir işlev uygulaması yapılandırma seçim seçim açıklanmaktadır. Belirli bir sürümü için bir yerel geliştirme ortamı yapılandırma hakkında daha fazla bilgi için bkz: [koduna ve test Azure işlevleri yerel olarak](functions-run-local.md).

## <a name="differences-between-runtime-1x-and-2x"></a>Çalışma zamanı arasındaki farklar 1.x ve 2.x

> [!IMPORTANT] 
> Çalışma zamanı 1.x üretim kullanımı için onaylanan yalnızca sürüm değil.

| Çalışma zamanı | Durum |
|---------|---------|
|1.x|Genellikle kullanılabilir (GA)|
|2.x|Önizleme|

Aşağıdaki bölümlerde diller, bağlamaları ve platformlar arası geliştirme desteği farkları açıklamaktadır.

### <a name="languages"></a>Diller

Aşağıdaki tabloda, hangi programlama dillerini her çalışma zamanı sürümünde desteklendiğini gösterir.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Daha fazla bilgi için bkz: [desteklenen diller](supported-languages.md).

### <a name="bindings"></a>Bağlamalar 

Çalışma zamanı 1.x destekler 2.x içinde kullanılabilir olmadığından Deneysel bağlar. Bağlamaları desteği ve diğer işlevsel boşlukları 2.x hakkında daha fazla bilgi için bkz: [çalışma zamanı bilinen sorunlar 2.0](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues).

Çalışma zamanı 2.x sağlar, oluşturduğunuz özel [uzantıları bağlama](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). Bu genişletilebilirlik modelini kullanan yerleşik bağlamaları, yalnızca 2.x içinde bulunur; Bunlardan ilki arasında [Microsoft Graph bağlamaları](functions-bindings-microsoft-graph.md).

### <a name="cross-platform-development"></a>Platformlar arası geliştirme

Çalışma zamanı 1.x destekler işlevi geliştirme yalnızca portalında veya Windows; 2.x ile geliştirin ve Linux veya macOS Azure işlevleri çalıştırın.

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

## <a name="target-the-version-20-runtime"></a>Hedef sürüm 2.0 çalışma zamanı

>[!IMPORTANT]   
> Azure işlevleri çalışma zamanı 2.0 Önizleme aşamasındadır ve Azure işlevlerinin şu anda tüm özellikler desteklenir. Daha fazla bilgi için bkz: [çalışma zamanı arasındaki farklar 1.x ve 2.x](#differences-between-runtime-1x-and-2x) bu makalenin önceki.

Çalışma zamanı sürüm 2.0 Önizleme Azure portalında bir işlev uygulaması taşıyabilirsiniz. İçinde **işlev uygulaması ayarları** sekmesinde, seçin **beta** altında **çalışma zamanı sürümü**.  

![İşlev uygulama ayarlarını seçin](./media/functions-versions/function-app-view-version.png)

Bu ayar ayarına eşdeğerdir `FUNCTIONS_EXTENSION_VERSION` uygulama ayarı için `beta`. Seçin **~ 1** geri geçerli genel olarak taşımak için düğmeyi desteklenen ana sürüm. Azure CLI, bu uygulama ayarını güncelleştirmek için de kullanabilirsiniz. 

## <a name="target-a-version-using-the-portal"></a>Hedef Portalı'nı kullanarak bir sürüm

Sürüm geçerli sürümle dışında ya da sürüm 2.0 hedef gerektiğinde ayarlamalısınız `FUNCTIONS_EXTENSION_VERSION` uygulama ayarı.

1. İçinde [Azure portal](https://portal.azure.com), işlevi uygulamanıza ve altında gidin **yapılandırılan özellikler**, seçin **uygulama ayarları**.

    ![İşlev uygulama ayarlarını seçin](./media/functions-versions/add-update-app-setting1a.png)

2. İçinde **uygulama ayarları** sekmesinde, bulmak `FUNCTIONS_EXTENSION_VERSION` ayarlama ve değeri 1.x çalışma zamanı geçerli bir sürüm olarak değiştirin veya `beta` sürüm 2.0 için. 

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

Bu komutu çalıştırabilirsiniz [Azure bulut Kabuk](../cloud-shell/overview.md) seçerek **deneyin** önceki kod örneğinde. Aynı zamanda [yerel olarak Azure CLI](/cli/azure/install-azure-cli) yürüttükten sonra bu komutu yürütmek için [az oturum açma](/cli/azure#az_login) oturum açmak için.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hedef yerel geliştirme ortamınızda 2.0 çalışma zamanı](functions-run-local.md)

> [!div class="nextstepaction"]
> [Çalışma zamanı sürümleri için sürüm notlarına bakın](https://github.com/Azure/azure-webjobs-sdk-script/releases)