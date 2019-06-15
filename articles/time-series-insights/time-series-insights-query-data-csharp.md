---
title: Bir Azure zaman serisi öngörüleri GA ortam kullanarak veri sorgulama C# kod | Microsoft Docs
description: Bu makalede C# (C-sharp) .NET dilinde yazılan özel bir uygulama kodlama yaparak bir Azure zaman serisi görüşleri ortamından veri sorgulama işlemini açıklamaktadır.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
reviewer: jasonwhowell, kfile, tsidocs
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/05/2019
ms.custom: seodec18
ms.openlocfilehash: 250dd691c3ef3146d6768123de52bf0628b10e42
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66728958"
---
# <a name="query-data-from-the-azure-time-series-insights-ga-environment-using-c"></a>Azure zaman serisi öngörüleri GA ortamı kullanarak verileri SorgulamaC#

Bu C# örnek, Azure zaman serisi öngörüleri GA ortamından veri sorgulama işlemini göstermektedir.

Örnek, Sorgu API’si kullanımının birkaç temel örneğini gösterir:

1. Hazırlık adımı olarak, Azure Active Directory API'si aracılığıyla erişim belirteci alın. Bu belirteci geçirmek `Authorization` her sorgu API'si isteği üstbilgisi. Etkileşimli olmayan uygulamalar ayarlamak için bkz: [kimlik doğrulama ve yetkilendirme](time-series-insights-authentication-and-authorization.md). Ayrıca, örnek başına tanımlı sabitler doğru ayarlandığından emin olun.
1. Kullanıcı erişimi olan ortamların listesi elde edilir. Ortamların biri ilgilenilen ortam seçilir ve daha fazla veri bu ortam için sorgulanır.
1. HTTPS isteğinin bir örneği olarak, ilgilenilen ortam için kullanılabilirlik verileri istenir.
1. Web yuvası isteğinin bir örneği olarak, ilgilenilen ortam için toplam olay verileri istenir. Veriler kullanılabilir oldukları tüm zaman aralığı için istenir.

> [!NOTE]
> Örnek kod kullanılabilir [ https://github.com/Azure-Samples/Azure-Time-Series-Insights ](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-ga-sample).

## <a name="project-dependencies"></a>Proje bağımlılıkları

NuGet paketleri Ekle `Microsoft.IdentityModel.Clients.ActiveDirectory` ve `Newtonsoft.Json`.

## <a name="c-example"></a>C# örneği

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-ga-sample/Program.cs)]

## <a name="next-steps"></a>Sonraki adımlar

- Sorgulama hakkında daha fazla bilgi edinmek için [sorgu API'si başvurusu](/rest/api/time-series-insights/ga-query-api).

- Nasıl olduğunu okuyun için [bir JavaScript tek sayfa uygulamasını bağlama](tutorial-create-tsi-sample-spa.md) Time Series Insights için.