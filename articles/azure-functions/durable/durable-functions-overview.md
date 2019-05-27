---
title: Dayanıklı işlevler genel bakış - Azure
description: Azure işlevleri için dayanıklı işlevler uzantısını giriş.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: overview
ms.date: 12/22/2018
ms.author: azfuncdf, glenga
ms.openlocfilehash: 2228f3fe05e1021d0f87ce0b0d33a8287f048a8c
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65872804"
---
# <a name="what-are-durable-functions"></a>Dayanıklı işlevler nelerdir?

*Dayanıklı işlevler* 'ın bir uzantısı olan [Azure işlevleri](../functions-overview.md) durum bilgisi olan işlevleri, sunucusuz bir ortamda yazmanızı sağlayan. Uzantı sizin için durumu, denetim noktalarını ve yeniden başlatmaları yönetir.

## <a name="benefits"></a>Avantajlar

Uzantıyı kullanarak durum bilgisi olan iş akışları tanımlamanızı sağlar bir [ *orchestrator işlevi*](durable-functions-types-features-overview.md#orchestrator-functions), aşağıdaki faydaları sağlayabilir:

* Kod içinde iş akışlarınızı tanımlayabilirsiniz. Herhangi bir JSON şemalarının veya tasarımcıları gereklidir.
* Diğer işlevler hem de zaman uyumlu ve zaman uyumsuz olarak çağrılabilir. Çağrılan işlevler çıkışını yerel değişkenlere kaydedilebilir.
* İlerleme durumu otomatik olarak belirttiğinizde olduğunda işlev bekler. Yerel durumu hiçbir zaman işlemi geri dönüştüren veya VM yeniden kaybolur.

## <a name="application-patterns"></a>Uygulama desenleri

Dayanıklı işlevler için birincil kullanım durumu, sunucusuz uygulamalar karmaşık, durum bilgisi olan düzenleme gereksinimleri basitleştirme olduğu. Dayanıklı işlevlerden yararlanabilirsiniz bazı tipik uygulama desenleri şunlardır:

* [Zincirleme](durable-functions-concepts.md#chaining)
* [Fan-dışarı/fan-arada](durable-functions-concepts.md#fan-in-out)
* [Zaman uyumsuz HTTP API'leri](durable-functions-concepts.md#async-http)
* [İzleme](durable-functions-concepts.md#monitoring)
* [İnsan etkileşimi](durable-functions-concepts.md#human)

## <a name="language-support"></a>Desteklenen diller

Dayanıklı işlevler şu anda aşağıdaki dilleri desteklemektedir:

* **C#**: her ikisi de [sınıf kitaplıklarını önceden derlenmiş](../functions-dotnet-class-library.md) ve [ C# betik](../functions-reference-csharp.md).
* **F#**: sınıf kitaplıklarını önceden derlenmiş ve F# betiği. F#betik yalnızca sürüm için desteklenen 1.x Azure işlevleri çalışma zamanı.
* **JavaScript**: yalnızca sürüm için desteklenen Azure işlevleri çalışma zamanı 2.x. Dayanıklı işlevler uzantısını 1.7.0 sürümünü veya sonraki bir sürümünü gerektirir. 

Dayanıklı işlevler sahip tüm destekleyici bir hedef [Azure işlevleri diller](../supported-languages.md). Bkz: [dayanıklı işlevler listesi sorunları](https://github.com/Azure/azure-functions-durable-extension/issues) ek dilleri desteklemek için iş en son durumu.

Azure işlevleri gibi dayanıklı işlevler kullanarak geliştirmenize yardımcı olacak şablonlar vardır [Visual Studio 2019](durable-functions-create-first-csharp.md), [Visual Studio Code](quickstart-js-vscode.md)ve [Azure portalında](durable-functions-create-portal.md).

## <a name="billing"></a>Faturalama

Dayanıklı işlevler faturalandırılır Azure işlevleri ile aynı. Daha fazla bilgi için [Azure işlevleri fiyatlandırması](https://azure.microsoft.com/pricing/details/functions/).

## <a name="jump-right-in"></a>Hemen başlayın

Bu dile özgü hızlı başlangıç öğreticileri birini tamamlayarak 10 dakika içinde dayanıklı işlevler ile başlayabilirsiniz:

* [C#Visual Studio 2019 kullanma](durable-functions-create-first-csharp.md)
* [Visual Studio Code kullanarak JavaScript](quickstart-js-vscode.md)

Her iki Hızlı başlangıçlarda yerel olarak oluşturabilir ve bir "hello world" dayanıklı işlevi test etme. Ardından işlev kodunu Azure’da yayımlayacaksınız. Oluşturduğunuz işlev düzenler ve diğer işlevlere yapılan çağrıları zincir.

## <a name="learn-more"></a>Daha fazla bilgi edinin

Aşağıdaki video, dayanıklı işlevler avantajlarını vurgular:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Durable-Functions-in-Azure-Functions/player] 

Dayanıklı işlevler için Gelişmiş bir uzantısı olduğundan [Azure işlevleri](../functions-overview.md), tüm uygulamalar için uygun değildir. Dayanıklı işlevler hakkında daha fazla bilgi için bkz: [dayanıklı işlevler desenleri ve teknik kavramlar](durable-functions-concepts.md). Diğer Azure düzenleme teknolojileridir ile bir karşılaştırması için bkz. [karşılaştırma Azure işlevleri ve Azure Logic Apps](../functions-compare-logic-apps-ms-flow-webjobs.md#compare-azure-functions-and-azure-logic-apps).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı işlevler desenleri ve teknik kavramlar](durable-functions-concepts.md)
