---
title: Azure işlevleri desteklenen dilleri
description: Hangi dilleri desteklenir (GA) ve Deneysel veya önizlemede olan öğrenin.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/07/2017
ms.author: tdykstra
ms.openlocfilehash: 5786a206b258cfe7c48f52ead9b5a4cceb64cd5f
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
ms.locfileid: "24879443"
---
# <a name="supported-languages-in-azure-functions"></a>Azure işlevleri desteklenen dilleri

Bu makalede, Azure işlevleri ile kullanabileceğiniz diller için destek düzeyleri sunulan açıklanmaktadır.

## <a name="levels-of-support"></a>Destek düzeyleri

Destek üç düzeyi vardır:

* **Genel olarak kullanılabilir (GA)** - tam olarak desteklenir ve üretim kullanımı için onaylandı.
* **Önizleme** - henüz desteklenir, ancak gelecekte GA durumuna ulaşmasını beklenir.
* **Deneysel** - değil desteklenen ve terk gelecekte; hiçbir garanti son Önizleme veya GA durumu.

## <a name="languages-in-runtime-1x-and-2x"></a>Çalışma zamanı dillerde 1.x ve 2.x

[Azure işlevleri çalışma zamanının iki sürümü](functions-versions.md) kullanılabilir. İST 1.x çalışma zamanı olduğu Bu, üretim uygulamaları için onaylanmış yalnızca çalışma zamanı gösterir. Önizleme'de desteklenen dilleri; bu nedenle 2.x çalışma zamanı şu anda önizlemede, değil. Aşağıdaki tabloda, her çalışma zamanı sürümünde desteklenen hangi dilleri gösterir.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

### <a name="experimental-languages"></a>Deneysel dilleri

1.x Deneysel dillerde iyi ölçeklendirme yoktur ve tüm bağlamaları desteklemez. Örneğin, işlevleri çalışma zamanı çalıştığından Python yavaş *Python.exe'yi* her işlev çağrısını ile. Ve Python HTTP bağlamaları desteklerken, istek nesnesi erişemez.

İşlev uygulamalarının çalışan sanal makineleri üzerinde yüklü olduğu için PowerShell Deneysel desteği sürüm 4.0 sınırlıdır. PowerShell betikleri çalıştırmak istiyorsanız, göz önünde bulundurun [Azure Otomasyonu](https://azure.microsoft.com/services/automation/).

2.x çalışma zamanı Deneysel dilleri desteklemiyor. Yalnızca iyi ölçeklenir olduğunda bir dil için destek ve Tetikleyicileri Gelişmiş destekler 2.x içinde ekleyeceğiz.

Yalnızca 1.x içinde kullanılabilen dilleri birini kullanmak istiyorsanız, 1.x çalışma zamanı üzerinde kalır. Ancak bunları resmi desteği olduğundan Deneysel diller, bağlı herhangi bir şey için kullanmayın. Tarafından Yardım isteğinde bulunabilirsiniz [GitHub sorunları oluşturma](https://github.com/Azure/azure-webjobs-sdk-script/issues), ancak destek olaylarının değil açılmalıdır Deneysel dilleri ile ilgili sorunlara. 

### <a name="language-extensibility"></a>Dil genişletilebilirliği

2.x çalışma zamanı sunmak üzere tasarlanmıştır [dil genişletilebilirlik](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility). Bu genişletilebilirlik üzerinde temel ilk diller arasında 2.x önizlemede yer Java modelidir.

## <a name="next-steps"></a>Sonraki adımlar

GA veya Önizleme dillerden biriyle Azure işlevlerini kullanma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

> [!div class="nextstepaction"]
> [C#](functions-reference-csharp.md)

> [!div class="nextstepaction"]
> [F#](functions-reference-fsharp.md)

> [!div class="nextstepaction"]
> [JavaScript](functions-reference-node.md)

> [!div class="nextstepaction"]
> [Java](functions-reference-java.md)