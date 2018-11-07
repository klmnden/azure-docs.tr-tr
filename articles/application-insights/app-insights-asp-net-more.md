---
title: Azure Application Insights'tan daha fazla alın | Microsoft Docs
description: Application Insights ile Başlarken sonra keşfedebilirsiniz özelliklerin özeti aşağıda verilmiştir.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 7ec10a2d-c669-448d-8d45-b486ee32c8db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 02/03/2017
ms.author: mbullwin
ms.openlocfilehash: a1244076efe1c920a23f29de9d25ab6845747fe4
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51245860"
---
# <a name="more-telemetry-from-application-insights"></a>Application ınsights'tan daha fazla telemetri
Sonra [ASP.NET kodunuza Application Insights eklenmiş](app-insights-asp-net.md), daha fazla telemetri almak için yapabileceğiniz birkaç şey vardır. 

| Eylem | Ne alacaksınız|
|---|---|
|(IIS sunucuları) [Durum İzleyicisi yükleyin](https://go.microsoft.com/fwlink/?LinkId=506648) her sunucu makinesinde.<br/>(Azure web uygulamaları) Web uygulaması için Azure denetim masasında Application Insights dikey penceresini açın.| [**Performans sayaçları**](app-insights-performance-counters.md)<br/>[**Özel durumlar** ](app-insights-asp-net-exceptions.md) - ayrıntılı Yığın izlemeleri<br/>[**Bağımlılıkları**](app-insights-asp-net-dependencies.md)|
|[JavaScript kod parçacığını web sayfalarınıza ekleme](app-insights-javascript.md)|[Sayfa performansı](app-insights-usage-overview.md), tarayıcı özel durumları, AJAX performans. Özel istemci tarafı telemetri.|
|[Kullanılabilirlik web testleri oluşturun](app-insights-monitor-web-app-availability.md)|Sitenizi kullanılamaz hale gelirse uyarılar alın|
|[Buildınfo.config olun](https://msdn.microsoft.com/library/dn449058.aspx) MSBuild tarafından oluşturulur|[Ek açıklamalar ölçüm grafikleri oluşturma](https://blogs.msdn.microsoft.com/visualstudioalm/2013/11/14/implementing-deployment-markers-in-application-insights/)
|[Yazma özel olaylar ve ölçümler](app-insights-api-custom-events-metrics.md)|İş olayları ve ölçümleri sayısı, ayrıntılı kullanım ve daha fazlasını izleyin.|
|[Sitenizin Canlı profil](https://aka.ms/AIProfilerPreview)|Canlı web uygulamanızdan ayrıntılı işlevi zamanlamaları|






