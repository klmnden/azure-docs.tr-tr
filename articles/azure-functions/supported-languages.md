---
title: Azure işlevleri'nde desteklenen diller
description: Hangi dillerde desteklenir (GA) ve Deneysel veya Önizleme aşamasında olan öğrenin.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 08/02/2018
ms.author: glenga
ms.openlocfilehash: b735f93b2d7ad093ef752fd5f26be729a1157b37
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44090691"
---
# <a name="supported-languages-in-azure-functions"></a>Azure işlevleri'nde desteklenen diller

Bu makalede, Azure işlevleri ile kullanabileceğiniz destek düzeyleri dilleri için sunulan açıklanmaktadır.

## <a name="levels-of-support"></a>Destek düzeyleri

Destek üç düzeyi vardır:

* **Genel kullanıma (GA)** - tam olarak desteklenen ve üretim kullanımı için onaylandı.
* **Önizleme** - henüz desteklenir, ancak genel kullanım durumu gelecekte kullanıma ulaşması bekleniyor.
* **Deneysel** - desteklenmez ve durdurulmuş gelecekte; nihai Önizleme veya GA durum garantisi.

## <a name="languages-in-runtime-1x-and-2x"></a>Çalışma zamanı dillerde 1.x ve 2.x'i

[İki Azure işlevleri çalışma zamanı sürümünü](functions-versions.md) kullanılabilir. 1.x çalışma zamanı büyüyecek olan Bu, üretim uygulamaları için onaylanmış yalnızca çalışma zamanı olur. Şu anda 2.x çalışma zamanı Önizleme aşamasında olduğundan, onu destekleyen diller Önizleme aşamasındadır. Aşağıdaki tabloda, her çalışma zamanı sürümünde desteklenen hangi diller gösterilmektedir.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

### <a name="experimental-languages"></a>Deneysel dil

Deneysel dil sürümünde 1.x düzgün ölçeklendirilemediği ve tüm bağlamaları desteklemez. Örneğin, İşlevler çalışma zamanı çalıştığı için Python yavaş *python.exe* her işlev Çağırma ile. Ve Python HTTP bağlantıları desteklese de, istek nesnesi erişemez.

Varsayılan olarak hangi işlev uygulamaları çalıştırın Vm'lerde yüklü olduğu için sürüm 5.1, PowerShell için Deneysel desteği sınırlıdır. PowerShell betikleri çalıştırmak istiyorsanız, göz önünde bulundurun [Azure Otomasyonu](https://azure.microsoft.com/services/automation/).

1.x çalışma zamanı modülü yalnızca içinde 1.x kullanılabilir dilleri birini kullanmak istiyorsanız, kalın. Ancak bunları resmi desteği olduğu Deneysel dillerden, bağlı olduğunuz her şey için kullanmayın. Tarafından Yardım isteğinde bulunabilirsiniz [GitHub sorunları oluşturmak](https://github.com/Azure/azure-webjobs-sdk-script/issues), ancak destek çalışmaları değil açılmalıdır Deneysel dili ile ilgili sorunlar için. 

Sürüm 2.x çalışma zamanı, Deneysel dilleri desteklemez. Yalnızca dil üretimde desteklenen yeni diller için destek eklenir. 

### <a name="language-extensibility"></a>Dil genişletilebilirliği

2.x çalışma zamanı sunmak üzere tasarlanmıştır [dil genişletilebilirlik](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility). Bu genişletilebilirlik alan ilk diller arasında 2.x önizlemede olan Java modelidir.

## <a name="next-steps"></a>Sonraki adımlar

Azure işlevleri'nde GA veya Önizleme dillerden birini kullanma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

> [!div class="nextstepaction"]
> [C#](functions-reference-csharp.md)

> [!div class="nextstepaction"]
> [F#](functions-reference-fsharp.md)

> [!div class="nextstepaction"]
> [JavaScript](functions-reference-node.md)

> [!div class="nextstepaction"]
> [Java](functions-reference-java.md)
