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
ms.devlang: multiple
ms.topic: reference
ms.date: 10/05/2018
ms.reviewer: mbullwin
ms.author: tilee
ms.openlocfilehash: 05958f35f80a53da27e020d367799519ef5a9bd7
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901592"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Application ınsights'ı Azure işlevleri için desteklenen özellikler

Şu anda desteklenen özellikleri listesi aşağıdadır [Application Insights ile Azure işlevleri tümleştirmesi](https://docs.microsoft.com/azure/azure-functions/functions-monitoring). Azure işlevleri Kılavuzu gözden [Başlarken](https://github.com/Azure/Azure-Functions/wiki/App-Insights).


| Azure İşlevleri                       | V1                | V2 (Ignite 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **Application ınsights'ı .NET SDK'sı**         | **2.5.0**             | **2.7.2**                 |
| | | | 
| **Otomatik olarak toplama**              |                   |                   |               
| &bull; İstekleri                           | Evet               | Evet               | 
| &bull; Özel durumlar                         | Evet               | Evet               | 
| &bull; Bağımlılıkları               |                   |                   |               
| &mdash; HTTP                              |                   | Evet               | 
| &mdash; serviceBus                        |                   | Evet               | 
| &mdash; eventHub                          |                   | Evet               | 
| &mdash; SQL                               |                   | Evet               | 
| | | | 
| **Desteklenen özellikler**                    |                   |                   |               
| &bull; QuickPulse/LiveMetrics                         | Evet               | Evet               | 
| &bull; Örnekleme                           | Evet               | Evet               | 
| &bull; Sinyal                         |       | Evet               | 
| | | | 
| **Bağıntı**                           |                   |                   |               
| &bull; serviceBus                         |                   | Evet               | 
| &bull; eventHub                           |                   | Evet               | 
| | | | 
| **Yapılandırılabilir**                  |                   |                   |           
| &bull;Tam olarak yapılandırılabilir.<br/>Bkz: [Azure işlevleri](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) yönergeler için.<br/>Bkz: [Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) tüm seçenekler için.               |                   | Evet                   | 
