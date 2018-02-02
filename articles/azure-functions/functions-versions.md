---
title: "Azure işlevleri çalışma zamanı sürümleri genel bakış"
description: "Azure işlevleri çalışma zamanı birden fazla sürümünü destekler. Bunları ve sizin için uygun olan bir seçme arasındaki farklar hakkında bilgi edinin."
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
ms.openlocfilehash: 9f916aaa8032ff519709d73a1c1f51195f811686
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="azure-functions-runtime-versions-overview"></a>Azure işlevleri çalışma zamanı sürümleri genel bakış

 Azure işlevleri çalışma zamanı iki ana sürümü vardır: 1.x ve 2.x. Bu makalede önemli hangi sürümün kullanılacağını seçme açıklanmaktadır.

> [!IMPORTANT] 
> Çalışma zamanı 1.x üretim kullanımı için onaylanan yalnızca sürüm değil.

| Çalışma Zamanı | Durum |
|---------|---------|
|1.x|Genellikle kullanılabilir (GA)|
|2.x|Önizleme|

Bir işlev uygulaması veya geliştirme ortamınızı belirli bir sürüm için nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz: [Azure işlevleri çalışma zamanı sürümlerini hedefleyen nasıl](set-runtime-version.md) ve [koduna ve test yerel olarak Azure işlevleri](functions-run-local.md).

## <a name="cross-platform-development"></a>Platformlar arası geliştirme

Çalışma zamanı 1.x destekler geliştirme ve yalnızca portalında veya Windows barındırma. .NET Core, macOS ve Linux dahil .NET Core tarafından desteklenen tüm platformlarda çalışabilir anlamına çalışma zamanı 2.x çalışır. Bu, platformlar arası geliştirme ve 1.x ile mümkün olmayan barındırma senaryolarını sağlar.

## <a name="languages"></a>Diller

Çalışma zamanı 2.x yeni bir dil genişletilebilirlik modeli kullanır. Başlangıçta, JavaScript ve Java yeni Bu modelin avantajı, sürüyor. Azure işlevleri 1.x Deneysel dilleri 2.x içinde desteklenmez şekilde yeni model kullanılacak güncelleştirilmedi. Aşağıdaki tabloda, hangi programlama dillerini her çalışma zamanı sürümünde desteklendiğini gösterir.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Daha fazla bilgi için bkz: [desteklenen diller](supported-languages.md).

## <a name="bindings"></a>Bağlamalar 

Çalışma zamanı 2.x kullanan yeni bir [bağlama genişletilebilirlik modelini](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) aşağıdaki avantajları sunar:

* Üçüncü taraf bağlama uzantıları için destek.
* Çalışma zamanı ve bağlamaları ayırma. Bu bağlama uzantıları sürümü tutulan ve bağımsız olarak yayımlanmış olmasını sağlar. Örneğin, temel alınan bir SDK daha yeni bir sürümü güvenen uzantı sürümüne yükseltmek için tercih edebilirsiniz.
* Bağlamaları kullanımda olduğu bilinen ve çalışma zamanı tarafından yüklenen bir açık yürütme ortamı.

Tüm yerleşik Azure işlevleri bağlamaları bu modeli benimseyen ve artık Zamanlayıcı tetikleyicisi ve HTTP tetikleyicisini dışında varsayılan olarak dahil edilir. Bu uzantılar, Visual Studio gibi araçlar aracılığıyla veya portal üzerinden işlevleri oluşturduğunuzda otomatik olarak yüklenir.

Aşağıdaki tabloda, hangi bağlamaları her çalışma zamanı sürümünde desteklendiğini gösterir.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

## <a name="known-issues-in-2x"></a>2.x bilinen sorunlar

Bağlamaları desteği ve diğer işlevsel boşlukları 2.x hakkında daha fazla bilgi için bkz: [çalışma zamanı bilinen sorunlar 2.0](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Hedef yerel geliştirme ortamınızda 2.0 çalışma zamanı](functions-run-local.md)

> [!div class="nextstepaction"]
> [Çalışma zamanı sürümleri için sürüm notlarına bakın](https://github.com/Azure/azure-webjobs-sdk-script/releases)