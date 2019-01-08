---
title: Azure Application Insights - Azure işlevleri desteklenen özellikler | Microsoft Docs
description: Azure işlevleri için Application ınsights'ı desteklenen özellikler
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: ''
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: reference
ms.date: 10/05/2018
ms.reviewer: mbullwin
ms.author: tilee
ms.openlocfilehash: 9ad0579ff9c25753b1e4816b80948b4d8d1232f7
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54083379"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Application ınsights'ı Azure işlevleri için desteklenen özellikler

Azure işlevleri tekliflerini [yerleşik tümleştirme](https://docs.microsoft.com/azure/azure-functions/functions-monitoring) Application Insights ile olan ILogger arabirimi aracılığıyla kullanılabilir. Şu anda desteklenen özelliklerin listesi aşağıda verilmiştir. Azure işlevleri Kılavuzu gözden [Başlarken](https://github.com/Azure/Azure-Functions/wiki/App-Insights).

## <a name="supported-features"></a>Desteklenen özellikler

| Azure İşlevleri                       | V1                | V2 (Ignite 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **Application ınsights'ı .NET SDK'sı**   | **2.5.0**       | **2.7.2**         |
| | | | 
| **Otomatik olarak toplama**        |                 |                   |               
| &bull; İstekleri                     | Evet             | Evet               | 
| &bull; Özel durumlar                   | Evet             | Evet               | 
| &bull; Bağımlılıkları                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |                 | Evet               | 
| &nbsp;&nbsp;&nbsp;&mdash; serviceBus|                 | Evet               | 
| &nbsp;&nbsp;&nbsp;&mdash; eventHub  |                 | Evet               | 
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |                 | Evet               | 
| | | | 
| **Desteklenen özellikler**                |                   |                   |               
| &bull; QuickPulse/LiveMetrics       | Evet             | Evet               | 
| &bull; Örnekleme                     | Evet             | Evet               | 
| &bull; Sinyal                   |                 | Evet               | 
| | | | 
| **Bağıntı**                       |                   |                   |               
| &bull; serviceBus                     |                   | Evet               | 
| &bull; eventHub                       |                   | Evet               | 
| | | | 
| **Yapılandırılabilir**                      |                   |                   |           
| &bull;Tam olarak yapılandırılabilir.<br/>Bkz: [Azure işlevleri](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) yönergeler için.<br/>Bkz: [Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) tüm seçenekler için.               |                   | Evet                   | 


## <a name="sampling"></a>Örnekleme

Azure işlevleri, varsayılan olarak, yapılandırmada örnekleme sağlar. Daha fazla bilgi için [örnekleme yapılandırma](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling).
